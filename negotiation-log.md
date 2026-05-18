# Biên bản đàm phán hợp đồng API

- Cặp đàm phán: Camera Stream → AI Vision
- Product: Smart Campus
- Provider: AI Vision
- Consumer: Camera Stream
- Phiên: v1.0
- Ngày: [Ngày hiện tại]

---

## Issue #1

- Raised by: Consumer
- Endpoint: POST /vision/detect
- Concern: Consumer không rõ ảnh gửi dưới dạng URL hay multipart.
- Proposal: Provider chỉ hỗ trợ ảnh dưới dạng URL để đơn giản hóa xử lý.
- Resolution: Accepted
- Rationale: URL dễ dàng quản lý và giảm tải cho Provider.
- Impact: Consumer cần đảm bảo ảnh được upload trước và cung cấp URL hợp lệ.

---

## Issue #2

- Raised by: Provider
- Endpoint: POST /vision/detect
- Concern: Kích thước frame ảnh không được giới hạn rõ ràng.
- Proposal: Giới hạn kích thước frame tối đa là 5MB.
- Resolution: Accepted
- Rationale: Giới hạn kích thước giúp tránh quá tải hệ thống.
- Impact: Consumer cần kiểm tra kích thước ảnh trước khi gửi.

---

## Issue #3

- Raised by: Consumer
- Endpoint: POST /vision/detect
- Concern: Consumer cần biết kết quả detect ngay hay chỉ nhận detectionId để polling.
- Proposal: Provider trả detectionId để Consumer polling kết quả qua GET /vision/detections/{detectionId}.
- Resolution: Accepted
- Rationale: Polling giúp Provider xử lý bất đồng bộ và giảm thời gian chờ của Consumer.
- Impact: Consumer cần triển khai cơ chế polling.

---

## Issue #4

- Raised by: Provider
- Endpoint: GET /vision/detections/{detectionId}
- Concern: Consumer có thể gửi detectionId không hợp lệ.
- Proposal: Provider trả lỗi 404 với thông báo chi tiết nếu detectionId không tồn tại.
- Resolution: Accepted
- Rationale: Trả lỗi chi tiết giúp Consumer dễ dàng debug và xử lý.
- Impact: Consumer cần kiểm tra detectionId trước khi gửi.

---

## Issue #5

- Raised by: Consumer
- Endpoint: POST /vision/detect
- Concern: Consumer cần biết các trường metadata nào là bắt buộc.
- Proposal: Provider chỉ yêu cầu `cameraId` và `timestamp` trong metadata.
- Resolution: Accepted
- Rationale: Giảm thiểu yêu cầu metadata giúp đơn giản hóa tích hợp.
- Impact: Consumer cần đảm bảo gửi đủ `cameraId` và `timestamp`.

---

## Issue #6

- Raised by: Provider
- Endpoint: POST /vision/detect
- Concern: Consumer có thể gửi payload sai schema.
- Proposal: Provider validate payload và trả lỗi 400 với thông báo chi tiết.
- Resolution: Accepted
- Rationale: Validation giúp đảm bảo dữ liệu đầu vào đúng định dạng.
- Impact: Consumer cần kiểm tra payload trước khi gửi.

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