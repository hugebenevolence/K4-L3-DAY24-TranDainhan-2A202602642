# Verification — 12/10/2026

Môi trường: Windows native, Python 3.12.14 trong `.venv`, Gitleaks 8.30.1. Alias `python` của máy trỏ vào WindowsApps không có runtime; các lệnh `python -m ...` dưới đây được chạy bằng `.\.venv\Scripts\python.exe -m ...`. Đặt `PYTHONIOENCODING=utf-8` để in tiếng Việt.

| Kiểm tra | Kết quả |
|---|---|
| `python -m pytest tests/test_pii.py -v -s --basetemp=.pytest_tmp` | PASS; gold=118, pred=118, tp=118; precision=1.000, recall=1.000 (>95%). |
| `python -m pytest tests/test_policy.py -v --basetemp=.pytest_tmp` | PASS: restricted + egress deny; search internal hợp lệ allow. |
| `python -m pytest tests/test_obo.py -v --basetemp=.pytest_tmp` | PASS: support deny, dpo allow, delegation/TTL. |
| `python -m pytest tests/test_split.py -v --basetemp=.pytest_tmp` | PASS: không đọc `KH-000777`; vẫn đọc khách liên quan hợp lệ. |
| `python -m pytest tests/test_injection.py -v --basetemp=.pytest_tmp` | PASS: 4 lure × 2 principal, không SKIP. |
| `python -m pytest tests/test_ledger.py -v --basetemp=.pytest_tmp` | PASS: hash chain, phát hiện sửa dòng, reason bắt buộc. |
| `python -m pytest tests/ -q --basetemp=.pytest_tmp` | **33 passed**. |
| `python -m agent.check_lure corpus/ticket-90*.md` | **4/4 lure sẵn sàng**. |
| `python -m agent.loop --mock --as dpo 'Tổng hợp các ticket còn mở tuần này'` | Chạy sau test với sink reset; dpo vẫn đọc hồ sơ gắn ticket. |
| `ledger.verify(Path('reports/ledger.jsonl'))` | **True**; có `http_post` deny, reason không rỗng, `on_behalf_of=dpo`. |
| `pre-commit run --all-files` | PASS với hook `gitleaks-system` pinned v8.30.1. |
| `pip install --require-hashes -r requirements.lock` | PASS. |
| `gitleaks git --config .gitleaks.toml --redact --verbose .` | Exit 1 dự kiến: đúng 1 token giả trong commit `1bdd849`; xem `secrets-incident.md`. |
| `gitleaks dir --config .gitleaks.toml --redact .` | Exit 0, không finding trong cây làm việc hiện tại. |

Evidence: `reports/attack-before.log` có 4 dòng, mỗi dòng là replay riêng của một lure trước containment và chứa record **synthetic** `KH-000999`; `reports/attack-after.log` có 0 byte sau containment. `reports/ledger.jsonl` chỉ ghi hash tham số, không ghi nguyên hồ sơ. Không ghi nhận lỗi còn lại trong test bắt buộc.

Giới hạn regex: chỉ nhận đúng các mẫu số/email của fixture (CCCD 12 số, số điện thoại bắt đầu 0, STK 8–16 số có nhãn, email thông thường). Có thể bỏ sót số có dấu cách/gạch nối, biến thể nhãn khác, hoặc tên người; cũng có thể báo nhầm chuỗi 12 số không phải CCCD. Precision/recall 1.000 chỉ đo trên testset synthetic 120 dòng, không suy ra hiệu quả production.
