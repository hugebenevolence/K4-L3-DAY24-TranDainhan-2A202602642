Ghi nhận kiểm tra ngày 12 tháng 10 năm 2026

Bài lab được chạy trên Windows với Python 3.12.14 trong môi trường .venv và Gitleaks 8.30.1 và vì lệnh python của máy trỏ tới WindowsApps nên những lệnh python -m trong hướng dẫn được chạy bằng .\.venv\Scripts\python.exe -m và biến môi trường PYTHONIOENCODING được đặt thành utf-8 để in tiếng Việt

Lệnh python -m pytest tests/test_pii.py -v -s --basetemp=.pytest_tmp chạy đạt và báo gold 118 pred 118 tp 118 precision 1.000 cùng recall 1.000 nên vượt ngưỡng 95 phần trăm của rubric trên bộ mẫu này

Các lệnh python -m pytest tests/test_policy.py -v --basetemp=.pytest_tmp và python -m pytest tests/test_obo.py -v --basetemp=.pytest_tmp đều đạt trong đó restricted đi cùng egress bị từ chối còn yêu cầu tìm ticket internal hợp lệ được cho phép đồng thời support_l1 bị chặn khi đọc hồ sơ trong khi dpo vẫn đọc được khách gắn với ticket nếu mục đích và thời hạn hợp lệ

Các lệnh python -m pytest tests/test_split.py -v --basetemp=.pytest_tmp và python -m pytest tests/test_injection.py -v --basetemp=.pytest_tmp đều đạt trong đó Run B không đọc KH-000777 chỉ vì mã này xuất hiện trong text và vẫn đọc khách thật sự liên quan khi dpo có quyền Cả bốn lure được chạy với hai principal và không có trường hợp bị bỏ qua

Lệnh python -m pytest tests/test_ledger.py -v --basetemp=.pytest_tmp đạt vì chuỗi hash và trường reason được kiểm tra và một bản sao của ledger bị sửa một ký tự ở dòng giữa trả về False khi gọi verify trong khi file reports/ledger.jsonl gốc trả về True còn ledger gốc có quyết định từ chối http_post với reason và on_behalf_of là dpo đồng thời args_hash được ghi thay cho hồ sơ thô

Lệnh python -m pytest tests/ -q --basetemp=.pytest_tmp báo 33 passed còn lệnh python -m agent.check_lure corpus/ticket-90*.md báo 4/4 lure sẵn sàng Lệnh python -m agent.loop --mock --as dpo "Tổng hợp các ticket còn mở tuần này" đã được chạy sau khi runner được sửa và dpo vẫn đọc được hồ sơ gắn với ticket

reports/attack-before.log có bốn dòng tương ứng với bốn lần replay riêng bằng luồng cũ và mỗi dòng chứa bản ghi giả của KH-000999 còn reports/attack-after.log có kích thước bằng không sau khi replay bằng runner mới Vì vậy không có dữ liệu giả của khách này được gửi tới sink trong lần kiểm tra sau sửa

Lệnh pre-commit run --all-files đạt với hook gitleaks-system được pin ở phiên bản 8.30.1 và lệnh pip install --require-hashes -r requirements.lock cũng đạt Lệnh gitleaks git --config .gitleaks.toml --redact --verbose . trả mã thoát 1 vì tìm thấy đúng một token giả được cài trong commit 1bdd849922156c868a5ee9293d68f5b72e242d2c còn lệnh gitleaks dir --config .gitleaks.toml --redact . trả mã thoát 0 vì cây làm việc hiện tại không có finding

Lần đầu chạy git commit thì hook gitleaks-system dừng với thông báo Executable gitleaks not found vì phiên PowerShell chưa có .venv\Scripts trong PATH Sau khi thêm thư mục này vào PATH thì pre-commit run --all-files chạy đạt và commit được thử lại với cùng hook

Regex PII được viết cho CCCD 12 số số điện thoại bắt đầu bằng 0 tài khoản 8 đến 16 số có nhãn và email thông thường nên có thể bỏ sót cách viết dùng dấu cách hoặc dấu gạch giữa các chữ số hay nhãn khác và cũng có thể nhận nhầm một chuỗi 12 số không phải CCCD Bộ test gồm dữ liệu giả nên precision và recall 1.000 không chứng minh hiệu quả trên dữ liệu thật
