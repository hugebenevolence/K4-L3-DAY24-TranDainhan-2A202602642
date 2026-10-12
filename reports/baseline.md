Báo cáo trạng thái ban đầu

Trước khi thêm các lớp kiểm tra thì hàm _naive_loop trong agent/loop.py lấy nội dung ticket và gọi công cụ trực tiếp nên agent chưa gắn quyền cụ thể cho người yêu cầu trong từng lần chạy

Khi ticket chứa chỉ thị chèn vào thì luồng cũ gọi read_customer để lấy hồ sơ rồi gọi http_post để gửi dữ liệu và ở thời điểm đó agent/tools.py chỉ giới hạn đích gửi vào sink trên máy cục bộ chứ chưa quyết định theo mục đích hay quyền của principal

Bằng chứng là reports/attack-before.log có bốn dòng tương ứng với bốn lure được chạy riêng và mỗi dòng đều ghi hồ sơ giả của KH-000999 được gửi tới sink trước khi runner được sửa
