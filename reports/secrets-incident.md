# Token giả trong lịch sử Git

- Scanner: Gitleaks v8.30.1, rule `lab24-synthetic-token` trong `.gitleaks.toml`.
- `gitleaks git --config .gitleaks.toml --redact --verbose .` trả exit 1 như dự kiến: 1 finding tại `lab-fake-secret.txt:1`, commit `1bdd849922156c868a5ee9293d68f5b72e242d2c`. Giá trị này là token giả vô hiệu của bài lab; không sao chép token vào báo cáo.
- `gitleaks dir --config .gitleaks.toml --redact .` trên thư mục hiện tại trả exit 0, không có finding. File tài liệu ví dụ do bản Gitleaks tải về đặt trong `.venv` đã được bỏ khỏi môi trường cục bộ trước khi quét; `.venv` không được commit.
- Nếu là credential thật: thu hồi/rotate ngay, kiểm log dùng token, giới hạn tác động và thông báo bên liên quan theo quy trình ứng phó. Xoá file khỏi HEAD không vô hiệu hoá token nằm trong lịch sử Git; pin dependency hoặc hook cũng không thu hồi được credential đã lộ.
- Hook `.pre-commit-config.yaml` pin `v8.30.1` và dùng `gitleaks-system`; `requirements.lock` có hash cho từng package. Hai biện pháp này kiểm nguồn cung ứng, không thay thế xử lý sự cố.
