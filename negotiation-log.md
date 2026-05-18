# Biên bản đàm phán hợp đồng API

- Cặp đàm phán: Camera Stream → AI Vision
- Product: Smart Campus
- Provider: AI Vision
- Consumer: Camera Stream
- Phiên: v1.0
- Ngày: [Ngày hiện tại]

---

## Issue #1

- **Bối cảnh**: Consumer cần gửi ảnh để AI Vision detect, nhưng không rõ ảnh nên gửi dưới dạng URL hay multipart.
- **Vấn đề**: Consumer không rõ ảnh gửi dưới dạng URL hay multipart.
- **Đề xuất**: Provider chỉ hỗ trợ ảnh dưới dạng URL để đơn giản hóa xử lý.
- **Quyết định**: Accepted
- **Rationale**: URL dễ dàng quản lý và giảm tải cho Provider.
- **Tác động đến service**: Consumer cần đảm bảo ảnh được upload trước và cung cấp URL hợp lệ.

---

## Issue #2

- **Bối cảnh**: Kích thước frame ảnh lớn có thể gây quá tải cho hệ thống của Provider.
- **Vấn đề**: Kích thước frame ảnh không được giới hạn rõ ràng.
- **Đề xuất**: Giới hạn kích thước frame tối đa là 5MB.
- **Quyết định**: Accepted
- **Rationale**: Giới hạn kích thước giúp tránh quá tải hệ thống.
- **Tác động đến service**: Consumer cần kiểm tra kích thước ảnh trước khi gửi.

---

## Issue #3

- **Bối cảnh**: Consumer cần biết cách nhận kết quả detect từ Provider.
- **Vấn đề**: Consumer cần biết kết quả detect ngay hay chỉ nhận detectionId để polling.
- **Đề xuất**: Provider trả detectionId để Consumer polling kết quả qua GET /vision/detections/{detectionId}.
- **Quyết định**: Accepted
- **Rationale**: Polling giúp Provider xử lý bất đồng bộ và giảm thời gian chờ của Consumer.
- **Tác động đến service**: Consumer cần triển khai cơ chế polling.

---

## Issue #4

- **Bối cảnh**: Consumer có thể gửi detectionId không hợp lệ khi kiểm tra kết quả detect.
- **Vấn đề**: Consumer có thể gửi detectionId không hợp lệ.
- **Đề xuất**: Provider trả lỗi 404 với thông báo chi tiết nếu detectionId không tồn tại.
- **Quyết định**: Accepted
- **Rationale**: Trả lỗi chi tiết giúp Consumer dễ dàng debug và xử lý.
- **Tác động đến service**: Consumer cần kiểm tra detectionId trước khi gửi.

---

## Issue #5

- **Bối cảnh**: Metadata trong request của Consumer có thể không đầy đủ, gây lỗi xử lý.
- **Vấn đề**: Consumer cần biết các trường metadata nào là bắt buộc.
- **Đề xuất**: Provider chỉ yêu cầu `cameraId` và `timestamp` trong metadata.
- **Quyết định**: Accepted
- **Rationale**: Giảm thiểu yêu cầu metadata giúp đơn giản hóa tích hợp.
- **Tác động đến service**: Consumer cần đảm bảo gửi đủ `cameraId` và `timestamp`.

---

## Issue #6

- **Bối cảnh**: Payload từ Consumer có thể không đúng schema, gây lỗi xử lý.
- **Vấn đề**: Consumer có thể gửi payload sai schema.
- **Đề xuất**: Provider validate payload và trả lỗi 400 với thông báo chi tiết.
- **Quyết định**: Accepted
- **Rationale**: Validation giúp đảm bảo dữ liệu đầu vào đúng định dạng.
- **Tác động đến service**: Consumer cần kiểm tra payload trước khi gửi.

---

# Chốt hợp đồng v1.0

Provider sign-off:  
Consumer sign-off:  
Witness (GV/TA):    
Date:               

---

## Ghi chú warning nếu Spectral còn cảnh báo

| Warning | Lý do chấp nhận tạm thời | Kế hoạch sửa |
|---|---|---|
| Không có | N/A | N/A |