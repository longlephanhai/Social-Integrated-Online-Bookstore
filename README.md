# Social-Integrated Online Bookstore

Hệ thống nhà sách trực tuyến full-stack theo kiến trúc MERN (MongoDB, Express.js, React, Node.js) tích hợp mạng xã hội, nhắn tin thời gian thực, thanh toán trực tuyến qua Stripe, bản đồ địa chỉ giao hàng và trang quản trị thống kê.

---

## Mục lục

- [Tính năng chính](#tính-năng-chính)
- [Công nghệ sử dụng](#công-nghệ-sử-dụng)
- [Cấu trúc dự án](#cấu-trúc-dự-án)
- [Yêu cầu hệ thống](#yêu-cầu-hệ-thống)
- [Cấu hình biến môi trường](#cấu-hình-biến-môi-trường)
- [Cài đặt & Chạy dự án](#cài-đặt--chạy-dự-án)
- [Cơ sở dữ liệu (Models)](#cơ-sở-dữ-liệu-models)
- [Tổng quan API Endpoints](#tổng-quan-api-endpoints)
- [Sự kiện Real-time Socket.IO](#sự-kiện-real-time-socketio)

---

## Tính năng chính

### Nhà sách trực tuyến & Thương mại điện tử
- **Danh mục & Khám phá sách**: Duyệt sách theo 8 danh mục (`Khoa học`, `Lịch sử`, `Tiếng anh`, `Toán học`, `Truyện tranh`, `Văn học`, `Địa lý`, `Vật lý`), tìm kiếm theo từ khóa và lọc sản phẩm.
- **Chi tiết sách & Đánh giá**: Xem thông tin chi tiết, nhiều hình ảnh sản phẩm, giá niêm yết/giá bán và gửi/xem đánh giá từ 1–5 sao kèm bình luận.
- **Giỏ hàng & Yêu thích**: Thêm, cập nhật số lượng, xóa sản phẩm trong giỏ hàng và lưu danh sách sách yêu thích với bộ đếm cập nhật trực tiếp trên thanh điều hướng.
- **Đặt hàng & Thanh toán**:
  - Định vị địa chỉ giao hàng và hiển thị bản đồ tương tác bằng **OpenStreetMap Nominatim** và **Leaflet**.
  - Thanh toán trực tuyến qua **Stripe Checkout** (đơn vị tiền tệ VND) và xác thực kết quả thanh toán (`/verify`).
  - Gửi email xác nhận đơn hàng qua **Nodemailer**, xem lịch sử đơn hàng (`/myorder`) và chi tiết đơn hàng (`/orderdetail/:id`).
- **Chatbot hỗ trợ**: Chatbot tư vấn chọn sách theo kịch bản ngay trên trang chủ (`react-simple-chatbot`).

### Xác thực & Tài khoản người dùng
- **Đăng ký / Đăng nhập bằng Email**: Đăng ký tài khoản có gửi mã xác thực qua email (`uuid` kèm thời gian hết hạn), gửi lại mã kích hoạt, đăng nhập và quản lý phiên bằng JWT lưu trong cookie.
- **Đăng nhập Google OAuth 2.0**: Hỗ trợ đăng nhập nhanh bằng tài khoản Google (`@react-oauth/google` & `google-auth-library`).
- **Khôi phục mật khẩu**: Quên mật khẩu và đặt lại mật khẩu qua đường dẫn token gửi về email.

### Mạng xã hội & Nhắn tin thời gian thực
- **Bảng tin cộng đồng (`/social`)**: Đăng bài viết kèm văn bản, hình ảnh hoặc video; thích và bình luận bài viết (có chức năng duyệt bài từ quản trị viên).
- **Trang cá nhân**: Xem và chỉnh sửa thông tin cá nhân (`/profile/:id`, `/your-profile/:id`) gồm trạng thái, tuổi, nơi ở, công việc.
- **Hệ thống kết bạn**: Tìm kiếm người dùng (`/not-friend`), gửi/hủy/chấp nhận/từ chối lời mời kết bạn (`/request`, `/accept`) và xem danh sách bạn bè (`/friend`) cập nhật thời gian thực qua Socket.IO.
- **Nhắn tin Real-time**:
  - Phòng chat 1-1 giữa bạn bè (`/room-chat/:id`) hỗ trợ gửi ảnh và hiển thị trạng thái đang nhập (`typing`).
  - Kênh chat hỗ trợ trực tiếp giữa khách hàng và Admin (`/chat`) có tích hợp chọn emoji (`emoji-picker-element`).

### Trang quản trị (`/admin-panel`)
- **Bảng điều khiển thống kê**: Biểu đồ và thẻ số liệu trực quan sử dụng `recharts` và Ant Design.
- **Các phân hệ quản lý**: Lịch (`/admin-panel/calendar`), quản lý người dùng & phân quyền (`ADMIN` / `USER`), thêm/sửa sản phẩm sách, quản lý & cập nhật trạng thái đơn hàng, kiểm duyệt bài viết và hộp thư hỗ trợ khách hàng.

---

## Công nghệ sử dụng

| Thành phần | Công nghệ & Thư viện |
| --- | --- |
| **Frontend** | React 18, React Router v6, Redux Toolkit, Ant Design v5, Tailwind CSS v3, Sass, Axios |
| **Giao diện & Biểu đồ** | Recharts, Framer Motion, Swiper, AOS, React Icons, React Toastify, React Simple Chatbot, Emoji Picker Element |
| **Bản đồ & Lưu trữ ảnh** | Leaflet, React Leaflet, Leaflet GeoSearch, Cloudinary (upload ảnh trực tiếp từ client) |
| **Backend** | Node.js, Express 4, MongoDB, Mongoose 8, Socket.IO 4 |
| **Xác thực, Thanh toán & Email** | JSON Web Token (`jsonwebtoken`), `bcryptjs`, `cookie-parser`, Google Auth Library, Stripe, Nodemailer |

---

## Cấu trúc dự án

```text
Social-Integrated-Online-Bookstore/
├── backend/
│   ├── config/
│   │   └── db.js                  # Cấu hình kết nối MongoDB
│   ├── controller/
│   │   ├── product/               # Xử lý sản phẩm: thêm, sửa, tìm kiếm, lọc, danh mục
│   │   └── user/                  # Xử lý người dùng, giỏ hàng, đơn hàng, đánh giá, mạng xã hội & chat
│   ├── helpers/
│   │   ├── permission.js          # Kiểm tra quyền Admin
│   │   └── verifyGoogleToken.js   # Xác thực Google OAuth token
│   ├── middleware/
│   │   └── authToken.js           # Middleware xác thực JWT từ cookie
│   ├── models/                    # Các Mongoose schema (user, product, order, post, chat, ...)
│   ├── routes/
│   │   └── index.js               # Định nghĩa các REST API routes dưới prefix /api
│   ├── socketIO/
│   │   └── index.js               # Xử lý các sự kiện Socket.IO cho chat & kết bạn
│   ├── index.js                   # Điểm khởi chạy server Express & Socket.IO
│   └── package.json
├── frontend/
│   ├── public/
│   ├── src/
│   │   ├── assest/                # Hình ảnh banner, logo thương hiệu và tài nguyên tĩnh
│   │   ├── common/                # Khai báo danh sách API (index.js) & vai trò (role.js)
│   │   ├── components/            # Các component dùng chung, biểu đồ, bản đồ, bài viết, sản phẩm
│   │   ├── context/               # React Context chia sẻ thông tin user, giỏ hàng, yêu thích
│   │   ├── helpers/               # Định dạng tiền tệ, upload Cloudinary, danh mục sách, cookie
│   │   ├── pages/                 # Các trang giao diện (Home, Social, Cart, Payment, AdminPanel, Chat, ...)
│   │   ├── routes/
│   │   │   └── index.js           # Cấu hình định tuyến React Router
│   │   ├── store/                 # Cấu hình Redux Toolkit store và userSlice
│   │   ├── App.js
│   │   └── index.js
│   ├── tailwind.config.js
│   └── package.json
└── README.md
```

---

## Yêu cầu hệ thống

- **Node.js** (khuyến nghị v18+) và **npm**
- **MongoDB** (cài đặt local hoặc sử dụng MongoDB Atlas)
- Tài khoản **Stripe** (để lấy `STRIPE_SECRET_KEY`)
- Tài khoản **Cloudinary** (để upload hình ảnh qua unsigned upload preset)
- **Google Cloud** OAuth 2.0 Client ID (cho tính năng đăng nhập Google)
- Tài khoản **Gmail** đã bật App Password (để gửi email xác thực, đặt lại mật khẩu và thông báo đơn hàng qua Nodemailer)

---

## Cấu hình biến môi trường

Tạo file `.env` trong cả hai thư mục `backend/` và `frontend/` (các file `.env` đã được cấu hình bỏ qua trong `.gitignore`).

### `backend/.env`

```env
PORT=8080
MONGODB_URI=mongodb://localhost:27017/bookstore
FRONTEND_URL=http://localhost:3000
TOKEN_SECRET_KEY=your_jwt_secret_key
STRIPE_SECRET_KEY=sk_test_your_stripe_secret_key
MAIL=your_gmail_address@gmail.com
PASS_EMAIL=your_gmail_app_password
GOOGLE_CLIENT_ID=your_google_oauth_client_id
NODE_ENV=development
```

> **Lưu ý:** Trong `backend/index.js`, biến `PORT` hiện viết là `8080 || process.env.PORT` nên server mặc định chạy ở cổng `8080`.

### `frontend/.env`

```env
REACT_APP_BACKEND_URL=http://localhost:8080
REACT_APP_CLOUD_NAME_CLOUDINARY=your_cloudinary_cloud_name
REACT_APP_GOOGLE_CLIENT_ID=your_google_oauth_client_id
```

> **Lưu ý:**
> - Hàm upload ảnh trong `frontend/src/helpers/uploadImage/uploadImage.js` sử dụng `upload_preset` tên là `product_cosmetics` (cần tạo unsigned preset này trên Cloudinary hoặc đổi sang tên preset của bạn).
> - File `frontend/src/index.js` hiện cũng truyền trực tiếp `clientId` vào `<GoogleOAuthProvider clientId="...">`; hãy cập nhật giá trị này nếu bạn sử dụng Google OAuth Client ID riêng.

---

## Cài đặt & Chạy dự án

1. **Clone mã nguồn**
   ```bash
   git clone https://github.com/longlephanhai/Social-Integrated-Online-Bookstore.git
   cd Social-Integrated-Online-Bookstore
   ```

2. **Cài đặt và chạy Backend**
   ```bash
   cd backend
   npm install
   npm run dev
   ```
   Backend API và Socket.IO server sẽ chạy tại `http://localhost:8080`.
   Sử dụng `npm start` để chạy ở chế độ production (`node index.js`).

3. **Cài đặt và chạy Frontend** (mở terminal mới)
   ```bash
   cd frontend
   npm install
   npm start
   ```
   Ứng dụng React sẽ khởi chạy tại `http://localhost:3000`.

### Các script có sẵn ở Frontend

- `npm start` — Chạy server phát triển (development)
- `npm run build` — Build mã nguồn production vào thư mục `frontend/build`
- `npm test` — Chạy kiểm thử với `react-scripts test`

---

## Cơ sở dữ liệu (Models)

Tất cả Mongoose models nằm trong thư mục `backend/models/`:

| Model | File | Mô tả |
| --- | --- | --- |
| `user` | `userModal.js` | Thông tin tài khoản, phân quyền (`USER`/`ADMIN`), Google ID, trạng thái xác thực, danh sách bạn bè & lời mời |
| `product` | `productModel.js` | Thông tin sách: tên sách, thương hiệu/tác giả, danh mục, hình ảnh, mô tả, giá gốc & giá bán |
| `addToCart` | `cartProduct.js` | Sản phẩm và số lượng trong giỏ hàng của người dùng |
| `favorite` | `likeProduct.js` | Danh sách sách yêu thích của người dùng |
| `order` | `order.js` | Thông tin đơn hàng, danh sách sản phẩm, tổng tiền, địa chỉ giao hàng, trạng thái đơn & thanh toán |
| `review` | `review.js` | Đánh giá sao (1–5) và nhận xét của người dùng cho từng cuốn sách |
| `post` | `post.js` | Bài viết mạng xã hội (nội dung, ảnh, video), lượt thích và trạng thái duyệt (`accept`) |
| `comment` | `comment.js` | Bình luận trên các bài viết |
| `infor` | `InforModel.js` | Thông tin hồ sơ mở rộng (`status`, `age`, `live`, `work`) |
| `chat` | `chat.js` | Tin nhắn hỗ trợ trực tiếp giữa người dùng và Admin |
| `roomChat` | `roomChat.js` | Phòng chat riêng tư được tạo khi hai người dùng chấp nhận kết bạn |
| `chatwithfriend` | `chatWithFriend.js` | Nội dung tin nhắn và hình ảnh trong phòng chat bạn bè |

---

## Tổng quan API Endpoints

Tất cả API được khai báo trong `backend/routes/index.js` dưới tiền tố `/api`:

- **Xác thực & Người dùng**: `/signup`, `/signin`, `/google-login`, `/userLogout`, `/user-details`, `/confirm/:token`, `/check-code`, `/retry-active`, `/forgot-password`, `/reset-password/:token`, `/all-user`, `/update-user`, `/limit-user`
- **Sản phẩm (Sách)**: `/upload-product`, `/get-product`, `/update-product`, `/get-categoryProduct`, `/category-product`, `/product-details`, `/search`, `/filter-product`
- **Giỏ hàng & Yêu thích**: `/addtocart`, `/countAddToCartProduct`, `/view-cart-product`, `/update-cart-product`, `/delete-cart`, `/favorite`, `/listfavorite`, `/count-favorite`, `/deletefavorite`
- **Đơn hàng & Thanh toán**: `/place`, `/verify`, `/userorders`, `/all-order`, `/status`, `/orderitem`, `/order-detail`, `/send-email`, `/location`
- **Đánh giá sản phẩm**: `/productId`, `/review`
- **Mạng xã hội & Kết bạn**: `/post`, `/get-post`, `/getpostbyid`, `/post-list`, `/change-accept/:id`, `/post-modal`, `/like-post`, `/comment-post`, `/get-comment`, `/find`, `/your-profile/:id`, `/your-post/:id`, `/not-friend`, `/request`, `/request-list`, `/accept`, `/friend-list`
- **Nhắn tin**: `/getmessagebyId`, `/list-message`, `/get-admin`, `/chat-with-friend/:roomChatId`, `/get-one-friend`

---

## Sự kiện Real-time Socket.IO

Được cấu hình trong `backend/socketIO/index.js`:

- **Chat hỗ trợ khách hàng**: `client_send_message` → `server_return_message`, `CLIENT_SEND_TYPYNG` → `SERVER_RETURN_TYPING`
- **Kết bạn**: `CLIENT_ADD_FRIEND`, `CLIENT_CANCEL_FRIEND`, `CLIENT_REFUSE_FRIEND`, `CLIENT_ACCEPT_FRIEND` → `SERVER_RETURN_LENGTH_ACCEPT_FRIEND`, `SERVER_RETURN_INFO_ACCEPT_FRIEND`, `SERVER_RETURN_USER_ID_CANCEL_FRIEND`
- **Chat trong phòng bạn bè**: `CLIENT_SEND_MESSAGE_WITH_FRIEND` → `SERVER_RETURN_MESSAGE_FRIEND`, `CLIENT_SEND_TYPYNG_FRIEND` → `SERVER_RETURN_TYPING_FRIEND`
