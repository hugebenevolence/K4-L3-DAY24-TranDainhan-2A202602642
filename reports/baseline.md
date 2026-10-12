# Baseline (trước containment)

1. Chưa có identity và quyền theo từng run: `agent/loop.py:27-54` gọi tool trực tiếp; `principal_id` chưa được kiểm trong `_naive_loop()`.
2. Không có cổng cho phép `http_post`: `agent/loop.py:44` gọi `tools.http_post()` khi `llm.find_injection()` thấy chỉ thị. `agent/tools.py` chỉ giới hạn đích vào sink localhost, không xét principal hay phân loại dữ liệu.
3. `reports/attack-before.log:1` ghi một POST tới `/reconcile` chứa record giả `KH-000999` với CCCD, điện thoại, STK. Bằng chứng được tạo bằng baseline `_naive_loop()` trước khi hoàn thiện runner.
