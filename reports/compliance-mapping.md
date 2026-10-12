# Compliance mapping (đối chiếu kỹ thuật, không phải kết luận tuân thủ)

Đã đối chiếu tên và phạm vi văn bản trên cổng văn bản Chính phủ; **điều khoản cụ thể chưa xác minh**. Các hàng dưới đây là liên hệ giữa chủ đề văn bản và control của bài lab, không khẳng định đã đáp ứng nghĩa vụ pháp lý.

| Nguồn | Control lab | Evidence file:dòng | Giới hạn |
|---|---|---|---|
| [Luật 91/2025/QH15 — Bảo vệ dữ liệu cá nhân](https://vanban.chinhphu.vn/?docid=214590&pageid=27160) | PII gate, giới hạn đọc hồ sơ và chặn egress | `agent/pii.py:42`, `agent/policy.py:60`, `reports/attack-after.log` (0 byte) | Điều khoản cụ thể chưa xác minh; dữ liệu chỉ là giả. |
| [NĐ 356/2025/NĐ-CP — chi tiết Luật Bảo vệ dữ liệu cá nhân](https://vanban.chinhphu.vn/?docid=216387&pageid=27160&typegroupid=4) | Quyết định có reason và log truy vết | `agent/ledger.py:49`, `agent/ledger.py:64`, `reports/ledger.jsonl:1` | Chưa xác minh yêu cầu hồ sơ, lưu giữ hay điều khoản cụ thể. |
| [Luật 134/2025/QH15 — Trí tuệ nhân tạo](https://vanban.chinhphu.vn/?docid=216334&pageid=27160) | Tách run và kiểm quyền trước tool | `agent/runner.py:79`, `agent/runner.py:100`, `tests/test_split.py` | Điều khoản cụ thể chưa xác minh; mock không đánh giá hệ thống AI thật. |
| [OWASP ASI01, ASI03, ASI04](https://genai.owasp.org/2025/12/09/owasp-top-10-for-agentic-applications-the-benchmark-for-agentic-security-in-the-age-of-autonomous-ai/) | Chặn goal hijack bằng split/PEP; OBO giới hạn privilege; pin hook/dependency và quét lịch sử | `agent/runner.py:136`, `agent/policy.py:60`, `.pre-commit-config.yaml:1`, `requirements.lock:1`, `reports/secrets-incident.md:1` | Mapping rủi ro, không phải chứng nhận bảo mật. |
