# Phân tích yêu cầu — vai Consumer

- Cặp đàm phán:
- Product: A / B
- Consumer service:
- Provider service:
- Người viết:
- Ngày:

---

## 1. Resource Consumer cần nhận/gửi

| Resource       | Consumer dùng để làm gì?                  | Field bắt buộc với Consumer | Field có thể tùy chọn |
|----------------|------------------------------------------|-----------------------------|------------------------|
| Frame/Image    | Gửi ảnh để AI Vision detect             | imageUrl                   | metadata              |
| Detection Info | Nhận kết quả detect từ AI Vision         | detectionId, objects       | confidence, riskLevel |
| Model Info     | Lấy thông tin model AI Vision đang dùng | modelName, version         | description           |

---

## 2. API Consumer cần gọi

| Method | Path                     | Lúc nào gọi?                          | Kỳ vọng response                     |
|--------|--------------------------|---------------------------------------|---------------------------------------|
| POST   | `/vision/detect`         | Khi phát hiện motion                 | detectionId, objects, confidence, riskLevel |
| GET    | `/vision/detections/{id}`| Khi cần kiểm tra kết quả detect       | detectionId, objects, confidence, riskLevel |
| GET    | `/vision/models/info`    | Khi cần lấy thông tin model AI Vision | modelName, version, description      |
| GET    | `/health`                | Định kỳ kiểm tra trạng thái Provider | status: UP/DOWN                      |

---

## 3. Error case Consumer cần xử lý

| Status | Consumer hiểu là gì?         | Consumer sẽ xử lý thế nào?           |
|--------|------------------------------|--------------------------------------|
| 400    | Request sai schema           | Sửa payload/log lỗi                 |
| 401    | Thiếu token                  | Refresh/cấu hình token              |
| 403    | Không đủ quyền               | Báo lỗi quyền truy cập              |
| 404    | Không tìm thấy resource      | Hiển thị trạng thái không tồn tại   |
| 409    | Xung đột nghiệp vụ           | Retry hoặc yêu cầu thao tác lại     |
| 422    | Vi phạm rule nghiệp vụ       | Hiển thị lý do cụ thể               |
| 500    | Lỗi hệ thống của Provider    | Retry hoặc báo lỗi hệ thống         |

---

## 4. Giả định bổ sung

- Giả định 1: Provider luôn trả về response trong vòng 5 giây.
- Giả định 2: Provider hỗ trợ tối đa 100 request/giây.
- Giả định 3: Provider không yêu cầu idempotency cho request POST.

---

## 5. Câu hỏi cho Provider

1. Ảnh gửi dạng multipart hay URL?
2. Giới hạn kích thước frame là bao nhiêu?
3. AI Vision trả kết quả đồng bộ hay trả detectionId để polling?

---

## 6. Rủi ro tích hợp

| Rủi ro                     | Tác động                     | Đề xuất xử lý                          |
|----------------------------|------------------------------|----------------------------------------|
| Provider đổi kiểu dữ liệu  | Consumer parse lỗi           | Chốt type/format/pattern              |
| Provider thiếu mã lỗi      | Consumer khó xử lý lỗi       | Chuẩn hóa Problem Details             |
| Timeout khi xử lý request  | Consumer không nhận kết quả  | Đưa ra SLA và tối ưu thời gian xử lý   |
| Payload lớn                | Timeout hoặc lỗi kết nối     | Thống nhất content-type và size limit |
| Retry không kiểm soát      | Xử lý lặp, trùng kết quả     | Thêm idempotency key vào request      |
