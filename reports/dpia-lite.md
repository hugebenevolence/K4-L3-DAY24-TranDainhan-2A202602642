Đánh giá tác động dữ liệu ở mức đơn giản

Nội dung ticket trong corpus được xem là dữ liệu nội bộ có thể chứa chỉ thị không đáng tin cậy còn hồ sơ trong data/customers.json được xem là dữ liệu hạn chế vì có tên cùng số CCCD số điện thoại số tài khoản và email dù tất cả đều là dữ liệu giả phục vụ bài lab

search_docs được dùng để tổng hợp ticket còn read_customer chỉ phục vụ trả lời hỗ trợ khi người yêu cầu có quyền phù hợp nên support_l1 chỉ tìm được ticket trong khi dpo có thể đọc hồ sơ của khách gắn với ticket cho mục đích support-reply

Trong Run A agent đọc ticket rồi lấy số ticket từ tên file và che những giá trị PII phát hiện được trước khi tóm tắt sau đó Run B chỉ nhận tập số ticket để tra related_tickets trong nguồn tin cậy và kiểm policy trước khi mở kho hồ sơ cũng như trước từng lần đọc khách

Nếu ticket yêu cầu gửi dữ liệu thì runner chỉ đưa URL vào lượt kiểm http_post với phân loại restricted và egress_enabled=True còn đích sink bị giới hạn ở 127.0.0.1:10024 nên hồ sơ không được đưa vào lời gọi gửi

Trước khi sửa thì reports/attack-before.log có bốn dòng gửi dữ liệu giả của KH-000999 còn sau khi sửa reports/attack-after.log rỗng và reports/ledger.jsonl có quyết định từ chối http_post cùng lý do trong khi các test OBO split và injection vẫn giữ được chức năng hợp lệ cho dpo

Kết quả này chỉ phản ánh dữ liệu và công cụ giả lập vì tìm kiếm theo từ khóa còn có thể trả về nhiều ticket khiến dpo đọc nhiều hồ sơ hơn mức cần thiết đồng thời regex PII có thể bỏ sót cách viết khác và ledger một file không phát hiện được việc cắt dòng cuối nếu không có mốc đối chiếu bên ngoài
