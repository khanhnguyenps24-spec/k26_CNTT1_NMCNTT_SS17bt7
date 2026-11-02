## Phần 1: Phân tích vấn đề
### 1. 4 điểm sai
1.  **DNS:** Sai vì trình duyệt phải phân giải `carepulse.vn` thành địa chỉ IP thông qua DNS trước khi kết nối máy chủ.
    
2.  **TCP/TLS:** Sai vì HTTPS vẫn cần thiết lập kết nối TCP và TLS trước khi truyền dữ liệu HTTP.
    
3.  **HTTP 404:** Sai vì **404 = không tìm thấy tài nguyên/URL**, không đồng nghĩa Database bị sập. Lỗi xử lý phía Server nên trả **500 Internal Server Error**.
    
4.  **Sửa DOM:** Sai vì sửa nội dung trong **DevTools → Elements** chỉ thay đổi DOM tạm thời trên máy khách, không sửa Database trên Server.
    

### 2.lỗi có thể dẫn đến 500

Khi Backend không truy vấn được Database hoặc gặp lỗi xử lý, Server không thể hoàn thành Request nên thường trả **500 Internal Server Error**.

Nếu thông báo sai thành **404**, bệnh nhân có thể hiểu rằng **hồ sơ xét nghiệm đã bị xóa hoặc không tồn tại**, gây hoang mang, mất niềm tin và có thể ảnh hưởng nghiêm trọng đến quyết định chăm sóc sức khỏe.

----------

## Phần 2: Sửa lỗi & hoàn thiện

### 3. Biên bản chuẩn xác

**Bước 1 – DNS:** Trình duyệt phân giải `carepulse.vn` thành địa chỉ IP của máy chủ.

**Bước 2 – TCP/TLS:** Trình duyệt thiết lập kết nối TCP và thực hiện TLS Handshake để tạo kết nối HTTPS an toàn.

**Bước 3 – HTTP Request/Response:** Trình duyệt gửi Request lấy kết quả xét nghiệm. Server xử lý và truy vấn Database. Nếu Backend/Database gặp lỗi, Server có thể trả **500**; nếu tài nguyên không tồn tại thì trả **404**.

**Bước 4 – Rendering:** Trình duyệt nhận Response, tạo **DOM từ HTML**, **CSSOM từ CSS**, sau đó thực hiện Layout và Paint để hiển thị giao diện.

### 4. Vì sao sửa Elements không sửa được Database?

DevTools **Elements** chỉ sửa **DOM đang nằm trong RAM của trình duyệt (Client-side)**.

```
Database Server
      |
 HTTP Response
      |
Trình duyệt → DOM trong RAM
      |
Sửa bằng F12 Elements
      |
Chỉ thay đổi tạm thời trên máy khách
```

Khi **F5/Reload**, trang được tải lại từ Server và thay đổi trên DOM sẽ mất. Muốn thay đổi dữ liệu bệnh án thật sự phải thông qua **API/Backend có cơ chế xác thực và ghi dữ liệu vào Database**.
