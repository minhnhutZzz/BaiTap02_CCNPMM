# 🛡️ API Đăng nhập (Login Authentication)

**Thông tin sinh viên:**
- **Họ và tên:** Đào Minh Nhựt
- **MSSV:** 23110282
- **Chuyên ngành:** Công nghệ phần mềm (Software Engineering)
- **Môn học:** Các công nghệ phần mềm mới

---

## 📖 Giới thiệu dự án
Dự án này triển khai một API Đăng nhập (Login) an toàn và tối ưu, tuân thủ các tiêu chuẩn bảo mật Backend hiện đại. Chức năng được thiết kế với kiến trúc rõ ràng và tích hợp nhiều lớp bảo vệ để đảm bảo tính an toàn tối đa cho hệ thống cũng như dữ liệu của người dùng.

## ✨ Các tính năng nổi bật (Key Features)

### 1. 🔑 Xác thực bằng JWT (JSON Web Token)
- Sử dụng tiêu chuẩn **JWT** để cấp phát Access Token sau khi người dùng xác thực thành công.
- Token được ký (sign) bằng secret key an toàn, bao gồm các thông tin cần thiết (payload) và thời gian sống (expiration time) giới hạn.
- Cơ chế xác thực **Stateless**, giúp tối ưu hiệu suất server và dễ dàng mở rộng (scale) hệ thống.

### 2. 🛡️ Data Validation (Kiểm tra dữ liệu đầu vào)
- Xác thực chặt chẽ mọi dữ liệu đầu vào từ client trước khi tiến hành xử lý logic (ví dụ: email phải đúng định dạng, mật khẩu không được bỏ trống và phải đạt độ dài/độ phức tạp tối thiểu).
- Ngăn chặn triệt để các rủi ro bảo mật như SQL Injection/NoSQL Injection và trả về các thông báo lỗi (Error Messages) rõ ràng, thân thiện cho Frontend xử lý.

### 3. 🚦 Rate Limiting (Chống Spam/Brute-force)
- Tích hợp cơ chế giới hạn số lượng request từ một địa chỉ IP trong một khoảng thời gian nhất định đối với endpoint Đăng nhập.
- Bảo vệ hệ thống khỏi các cuộc tấn công **Brute-force** (dò mật khẩu liên tục) và **DDoS** quy mô nhỏ, giữ cho server luôn hoạt động ổn định.

### 4. 🔐 Authorization (Phân quyền truy cập)
- Sau khi xác thực danh tính (Authentication), hệ thống tiếp tục kiểm tra quyền (Authorization) của người dùng đối với các tài nguyên cụ thể.
- Đảm bảo người dùng chỉ có thể thực hiện các thao tác và truy cập vào các API Endpoint tương ứng với Role của mình (VD: User, Admin, Manager).

---

## 🚀 Kết quả kiểm thử API (API Testing)

Dưới đây là các kịch bản kiểm thử API đã được thực hiện bằng Postman để chứng minh tính đúng đắn và sự chặt chẽ trong khâu xử lý lỗi của hệ thống.

### 1. POST Login API - Thành công
**Mô tả:** Khi client cung cấp thông tin đăng nhập (Email/Username và Password) hợp lệ. Hệ thống xử lý thành công, trả về HTTP Status `200 OK` kèm theo thông tin User và chuỗi `access_token` JWT.

> *Chèn ảnh minh họa cho trường hợp đăng nhập thành công vào bên dưới:*
<img width="1918" height="933" alt="image" src="https://github.com/user-attachments/assets/dec34a84-98e0-45eb-83a5-aa1c3ca59b47" />


### 2. POST Login API - Sai mật khẩu
**Mô tả:** Khi client nhập đúng Email/Username nhưng sai mật khẩu. Hệ thống từ chối truy cập, đảm bảo không rò rỉ thông tin thừa, trả về HTTP Status `401 Unauthorized` hoặc `400 Bad Request` kèm thông báo lỗi "Thông tin đăng nhập không chính xác".

> *Chèn ảnh minh họa cho trường hợp sai mật khẩu vào bên dưới:*
<img width="1915" height="873" alt="image" src="https://github.com/user-attachments/assets/7122212b-6334-46ec-b7fa-8e42e1025f3e" />


### 3. POST Login API - Thiếu Validation
**Mô tả:** Khi request body từ client gửi lên bị thiếu các trường bắt buộc (như không có trường mật khẩu) hoặc sai định dạng (email không có `@`). Lớp Middleware Validation bắt lỗi ngay lập tức và trả về HTTP Status `400 Bad Request` cùng danh sách chi tiết các trường bị lỗi.

> *Chèn ảnh minh họa cho trường hợp thiếu Validation vào bên dưới:*
<img width="1916" height="846" alt="image" src="https://github.com/user-attachments/assets/bec09434-6a6a-4777-92af-5b9cd8b62b6f" />


### 4. POST Login API - Bị chặn RateLimit
**Mô tả:** Khi một IP cố tình gửi quá nhiều request đăng nhập liên tục trong thời gian ngắn vượt mức cho phép. Hệ thống kích hoạt Rate Limit, lập tức chặn các request tiếp theo và trả về HTTP Status `429 Too Many Requests` kèm thông báo yêu cầu thử lại sau.

> *Chèn ảnh minh họa cho trường hợp bị chặn bởi Rate Limiting vào bên dưới:*
<img width="1918" height="758" alt="image" src="https://github.com/user-attachments/assets/7dfa6532-3a0b-4af7-a950-877829ab1b05" />


---

## 🛠️ Công nghệ & Thư viện 
*(Bạn có thể tùy chỉnh lại phần này theo đúng stack bạn đã làm)*
- **Nền tảng:** Node.js, Express.js
- **Bảo mật:** `jsonwebtoken`, `bcrypt` / `bcryptjs` (Hash mật khẩu)
- **Validation:** `Joi` / `express-validator` / `zod`
- **Rate Limit:** `express-rate-limit`
