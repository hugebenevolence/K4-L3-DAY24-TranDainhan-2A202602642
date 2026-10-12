Báo cáo về token giả trong lịch sử Git

Gitleaks phiên bản 8.30.1 được dùng cùng rule lab24-synthetic-token trong .gitleaks.toml để kiểm tra lịch sử Git và các file hiện tại

Khi quét lịch sử thì công cụ trả mã thoát 1 và tìm đúng một token giả trong lab-fake-secret.txt tại commit 1bdd849922156c868a5ee9293d68f5b72e242d2c vì vậy finding này là tình huống được cài sẵn cho bài lab chứ không phải bằng chứng có credential thật đang hoạt động

Khi quét cây làm việc hiện tại thì công cụ trả mã thoát 0 và không có finding nên giá trị token giả cũng không được sao chép vào báo cáo này

Nếu một credential thật bị lộ thì cần thu hồi hoặc rotate ngay rồi kiểm tra log sử dụng để xác định phạm vi ảnh hưởng và xử lý theo quy trình ứng phó vì việc xóa file khỏi phiên bản mới không làm credential trong lịch sử Git mất hiệu lực

Hook gitleaks-system đã được pin trong .pre-commit-config.yaml và các gói trong requirements.lock đều có hash để giảm rủi ro thay đổi dependency nhưng hai biện pháp này không thay thế bước thu hồi credential đã lộ
