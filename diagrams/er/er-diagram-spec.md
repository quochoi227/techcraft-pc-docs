# Đặc tả chi tiết các bảng trong Cơ sở Dữ liệu (Database Specification)

Tài liệu này đặc tả chi tiết cấu trúc các bảng dữ liệu trong hệ thống dựa trên sơ đồ thực thể quan hệ [er-diagram.mmd](file:///e:/Documents/HK1-26-27/NL2/computer-components-shop-docs/diagrams/er/er-diagram.mmd) và tài liệu kiến trúc [AGENTS.md](file:///e:/Documents/HK1-26-27/NL2/computer-components-shop-docs/AGENTS.md).

---

## Danh sách các bảng
1. [users](#1-bảng-users-người-dùng)
2. [products](#2-bảng-products-sản-phẩm--linh-kiện)
3. [product_images](#3-bảng-product_images-hình-ảnh-sản-phẩm)
4. [orders](#4-bảng-orders-đơn-hàng)
5. [order_items](#5-bảng-order_items-chi-tiết-đơn-hàng)
6. [cart_items](#6-bảng-cart_items-chi-tiết-giỏ-hàng)
7. [reviews](#7-bảng-reviews-đánh-giá--nhận-xét)
8. [pc_cases](#8-bảng-pc_cases-cấu-hình-pc-đúc-sẵn)
9. [pc_case_items](#9-bảng-pc_case_items-thành-phần-linh-kiện-pc-case)
10. [rag_documents](#10-bảng-rag_documents-tài-liệu-tri-thức-rag)
11. [rag_chunks](#11-bảng-rag_chunks-đoạn-văn-bản-và-vector-embedding)
12. [refresh_tokens](#12-bảng-refresh_tokens-token-làm-mới-phiên-đăng-nhập)
13. [email_verification_tokens](#13-bảng-email_verification_tokens-token-xác-thực-email)
14. [password_reset_tokens](#14-bảng-password_reset_tokens-token-đặt-lại-mật-khẩu)
15. [vouchers](#15-bảng-vouchers-mã-giảm-giá)
16. [voucher_usages](#16-bảng-voucher_usages-lịch-sử-sử-dụng-mã-giảm-giá)

---

## 1. Bảng `users` (Người dùng)

Bảng lưu trữ thông tin tài khoản người dùng và quản trị viên của hệ thống.

| Tên thuộc tính | Kiểu dữ liệu | Khóa chính | Khóa ngoại | Không rỗng | Bảng tham chiếu | Diễn giải |
|---|---|:---:|:---:|:---:|---|---|
| id | UUID | X | | X | | Khóa chính, mã định danh duy nhất của người dùng |
| email | VARCHAR(255) | | | X | | Email tài khoản dùng đăng nhập (Unique, duy nhất toàn hệ thống) |
| password_hash | VARCHAR(255) | | | X | | Mật khẩu tài khoản đã được mã hóa (băm an toàn bằng BCrypt) |
| full_name | VARCHAR(255) | | | | | Họ và tên người dùng |
| phone | VARCHAR(20) | | | | | Số điện thoại liên lạc của người dùng |
| address | TEXT | | | | | Địa chỉ nhận hàng mặc định |
| role | ENUM | | | X | | Vai trò người dùng (`USER`, `ADMIN`), mặc định là `USER` |
| created_at | TIMESTAMP | | | X | | Thời điểm tạo tài khoản người dùng |
| updated_at | TIMESTAMP | | | X | | Thời điểm cập nhật thông tin tài khoản gần nhất |

---

## 2. Bảng `products` (Sản phẩm / Linh kiện)

Bảng lưu trữ danh mục linh kiện phần cứng máy tính bán lẻ.

| Tên thuộc tính | Kiểu dữ liệu | Khóa chính | Khóa ngoại | Không rỗng | Bảng tham chiếu | Diễn giải |
|---|---|:---:|:---:|:---:|---|---|
| id | UUID | X | | X | | Khóa chính, mã định danh duy nhất của sản phẩm/linh kiện |
| name | VARCHAR(255) | | | X | | Tên sản phẩm linh kiện |
| price | DECIMAL(15, 2) | | | X | | Đơn giá bán của linh kiện (VNĐ, > 0) |
| stock_quantity | INT | | | X | | Số lượng tồn kho hiện tại (mặc định 0, >= 0) |
| category | ENUM | | | X | | Danh mục linh kiện (`CPU`, `MAINBOARD`, `RAM`, `GPU`, `STORAGE`, `PSU`, `CASE`, `CPU_COOLER`) |
| description | TEXT | | | | | Mô tả chi tiết giới thiệu sản phẩm |
| detail | JSONB | | | X | | Đặc tính thông số kỹ thuật chi tiết theo danh mục (socket, TDP, kích thước, ...) |
| is_active | BOOLEAN | | | X | | Trạng thái hiển thị bán (mặc định TRUE, FALSE: ẩn/ngừng kinh doanh/soft-deleted) |
| created_at | TIMESTAMP | | | X | | Thời điểm thêm sản phẩm vào hệ thống |
| updated_at | TIMESTAMP | | | X | | Thời điểm cập nhật thông tin sản phẩm gần nhất |

---

## 3. Bảng `product_images` (Hình ảnh sản phẩm)

Bảng lưu trữ danh sách hình ảnh của từng sản phẩm linh kiện tải lên Cloudinary/S3.

| Tên thuộc tính | Kiểu dữ liệu | Khóa chính | Khóa ngoại | Không rỗng | Bảng tham chiếu | Diễn giải |
|---|---|:---:|:---:|:---:|---|---|
| id | UUID | X | | X | | Khóa chính, mã định danh hình ảnh sản phẩm |
| product_id | UUID | | X | X | products(id) | Mã sản phẩm sở hữu hình ảnh (Xóa sản phẩm sẽ xóa ảnh liên quan - ON DELETE CASCADE) |
| image_url | VARCHAR(500) | | | X | | Đường dẫn URL của hình ảnh lưu trữ (Cloudinary / S3) |
| is_primary | BOOLEAN | | | X | | Đánh dấu ảnh đại diện chính của sản phẩm (mặc định FALSE; mỗi sản phẩm chỉ có 1 ảnh đại diện) |
| display_order | INT | | | X | | Thứ tự sắp xếp hiển thị ảnh trong gallery (mặc định 0) |
| created_at | TIMESTAMP | | | X | | Thời điểm tải hình ảnh lên |

- **Ràng buộc:** Ràng buộc duy nhất có điều kiện `UNIQUE(product_id, is_primary) WHERE (is_primary = TRUE)` đảm bảo mỗi sản phẩm chỉ có duy nhất một ảnh đại diện.

---

## 4. Bảng `orders` (Đơn hàng)

Bảng lưu trữ thông tin đơn đặt hàng của khách hàng.

| Tên thuộc tính | Kiểu dữ liệu | Khóa chính | Khóa ngoại | Không rỗng | Bảng tham chiếu | Diễn giải |
|---|---|:---:|:---:|:---:|---|---|
| id | UUID | X | | X | | Khóa chính, mã định danh duy nhất của đơn hàng |
| user_id | UUID | | X | X | users(id) | Mã người dùng thực hiện đặt đơn hàng |
| status | ENUM | | | X | | Trạng thái đơn hàng (`PENDING`, `CONFIRMED`, `SHIPPING`, `DELIVERED`, `CANCELLED`) |
| payment_method | ENUM | | | X | | Phương thức thanh toán (hỗ trợ `COD` - Thanh toán khi nhận hàng) |
| shipping_address | TEXT | | | X | | Địa chỉ nhận hàng của đơn hàng |
| total_amount | DECIMAL(15, 2) | | | X | | Tổng số tiền thanh toán của đơn hàng (VNĐ) |
| note | TEXT | | | | | Ghi chú hoặc lời nhắn của khách hàng cho đơn hàng |
| created_at | TIMESTAMP | | | X | | Thời điểm tạo đơn hàng |
| updated_at | TIMESTAMP | | | X | | Thời điểm cập nhật đơn hàng gần nhất |

---

## 5. Bảng `order_items` (Chi tiết đơn hàng)

Bảng lưu trữ các mặt hàng nằm trong một đơn hàng. Một mục đơn hàng có thể là một linh kiện lẻ hoặc một bộ cấu hình PC Case hoàn chỉnh.

| Tên thuộc tính | Kiểu dữ liệu | Khóa chính | Khóa ngoại | Không rỗng | Bảng tham chiếu | Diễn giải |
|---|---|:---:|:---:|:---:|---|---|
| id | UUID | X | | X | | Khóa chính, mã định danh chi tiết mục đơn hàng |
| order_id | UUID | | X | X | orders(id) | Mã đơn hàng chứa mục này |
| product_id | UUID | | X | | products(id) | Mã linh kiện lẻ (nullable) |
| pc_case_id | UUID | | X | | pc_cases(id) | Mã bộ PC Case hoàn chỉnh (nullable) |
| quantity | INT | | | X | | Số lượng đặt mua (nguyên dương, > 0) |
| unit_price | DECIMAL(15, 2) | | | X | | Đơn giá sản phẩm/PC Case tại thời điểm chốt đặt hàng (VNĐ) |

- **Ràng buộc:** `CHECK ((product_id IS NOT NULL AND pc_case_id IS NULL) OR (product_id IS NULL AND pc_case_id IS NOT NULL))` đảm bảo mỗi dòng chỉ được tham chiếu đến đúng một trong hai đối tượng: linh kiện lẻ HOẶC bộ PC Case.

---

## 6. Bảng `cart_items` (Chi tiết giỏ hàng)

Bảng lưu trữ các mục sản phẩm hoặc PC Case người dùng thêm vào giỏ hàng.

| Tên thuộc tính | Kiểu dữ liệu | Khóa chính | Khóa ngoại | Không rỗng | Bảng tham chiếu | Diễn giải |
|---|---|:---:|:---:|:---:|---|---|
| id | UUID | X | | X | | Khóa chính, mã định danh mục trong giỏ hàng |
| user_id | UUID | | X | X | users(id) | Mã người dùng sở hữu giỏ hàng |
| product_id | UUID | | X | | products(id) | Mã linh kiện lẻ thêm vào giỏ (nullable) |
| pc_case_id | UUID | | X | | pc_cases(id) | Mã bộ PC Case thêm vào giỏ (nullable) |
| quantity | INT | | | X | | Số lượng trong giỏ hàng (mặc định 1, > 0) |

- **Ràng buộc:**
  - `CHECK ((product_id IS NOT NULL AND pc_case_id IS NULL) OR (product_id IS NULL AND pc_case_id IS NOT NULL))`
  - `UNIQUE(user_id, product_id)`: Một người dùng chỉ có 1 dòng cho mỗi linh kiện lẻ trong giỏ hàng.
  - `UNIQUE(user_id, pc_case_id)`: Một người dùng chỉ có 1 dòng cho mỗi PC Case trong giỏ hàng.

---

## 7. Bảng `reviews` (Đánh giá & Nhận xét)

Bảng lưu trữ đánh giá sao và nhận xét của khách hàng đối với linh kiện lẻ hoặc bộ PC Case đã mua.

| Tên thuộc tính | Kiểu dữ liệu | Khóa chính | Khóa ngoại | Không rỗng | Bảng tham chiếu | Diễn giải |
|---|---|:---:|:---:|:---:|---|---|
| id | UUID | X | | X | | Khóa chính, mã định danh đánh giá |
| user_id | UUID | | X | X | users(id) | Mã người dùng viết đánh giá |
| product_id | UUID | | X | | products(id) | Mã linh kiện được đánh giá (nullable) |
| pc_case_id | UUID | | X | | pc_cases(id) | Mã bộ PC Case được đánh giá (nullable) |
| rating | SMALLINT | | | X | | Số sao đánh giá từ 1 đến 5 sao |
| comment | TEXT | | | | | Nội dung bình luận, nhận xét chi tiết |
| created_at | TIMESTAMP | | | X | | Thời điểm gửi đánh giá |
| updated_at | TIMESTAMP | | | X | | Thời điểm sửa đổi đánh giá gần nhất |

- **Ràng buộc:**
  - `CHECK (rating BETWEEN 1 AND 5)`
  - `CHECK ((product_id IS NOT NULL AND pc_case_id IS NULL) OR (product_id IS NULL AND pc_case_id IS NOT NULL))`
  - `UNIQUE(user_id, product_id)`: Mỗi khách hàng chỉ được đánh giá 1 lần cho một linh kiện đã mua.
  - `UNIQUE(user_id, pc_case_id)`: Mỗi khách hàng chỉ được đánh giá 1 lần cho một PC Case đã mua.

---

## 8. Bảng `pc_cases` (Cấu hình PC đúc sẵn)

Bảng lưu trữ các bộ cấu hình PC hoàn chỉnh do Admin thiết lập sẵn (được kiểm tra tính tương thích trước khi lưu).

| Tên thuộc tính | Kiểu dữ liệu | Khóa chính | Khóa ngoại | Không rỗng | Bảng tham chiếu | Diễn giải |
|---|---|:---:|:---:|:---:|---|---|
| id | UUID | X | | X | | Khóa chính, mã định danh cấu hình PC Case |
| name | VARCHAR(255) | | | X | | Tên cấu hình máy tính (ví dụ: "Gaming Entry 2025", "Workstation Pro") |
| use_case | VARCHAR(100) | | | | | Mục đích sử dụng chính (`gaming`, `study`, `office`, `rendering`) |
| total_price | DECIMAL(15, 2) | | | X | | Tổng giá trị của bộ PC Case (tính tự động từ tổng giá các linh kiện cấu thành) |
| description | TEXT | | | | | Mô tả chi tiết về cấu hình, đối tượng hướng đến, hiệu năng |
| is_active | BOOLEAN | | | X | | Trạng thái hiển thị/kinh doanh (mặc định TRUE, FALSE: ẩn) |
| created_at | TIMESTAMP | | | X | | Thời điểm tạo cấu hình PC |
| updated_at | TIMESTAMP | | | X | | Thời điểm cập nhật cấu hình PC gần nhất |

---

## 9. Bảng `pc_case_items` (Thành phần linh kiện PC Case)

Bảng liên kết các linh kiện phần cứng cụ thể cấu thành nên một bộ cấu hình PC Case.

| Tên thuộc tính | Kiểu dữ liệu | Khóa chính | Khóa ngoại | Không rỗng | Bảng tham chiếu | Diễn giải |
|---|---|:---:|:---:|:---:|---|---|
| id | UUID | X | | X | | Khóa chính, mã định danh thành phần linh kiện trong cấu hình PC |
| pc_case_id | UUID | | X | X | pc_cases(id) | Mã cấu hình PC Case sở hữu linh kiện (ON DELETE CASCADE) |
| product_id | UUID | | X | X | products(id) | Mã linh kiện cụ thể được sử dụng trong cấu hình |
| category | ENUM | | | X | | Danh mục vị trí lắp đặt (`CPU`, `MAINBOARD`, `RAM`, `GPU`, `STORAGE`, `PSU`, `CASE`, `CPU_COOLER`) |

- **Ràng buộc:** `UNIQUE(pc_case_id, category)` đảm bảo mỗi khe cắm/danh mục linh kiện trong một PC Case chỉ được chọn 1 sản phẩm.

---

## 10. Bảng `rag_documents` (Tài liệu tri thức RAG)

Bảng lưu trữ thông tin các tài liệu chính sách của cửa hàng (bảo hành, đổi trả, giao hàng...) do Admin tải lên phục vụ hệ thống RAG chatbot.

| Tên thuộc tính | Kiểu dữ liệu | Khóa chính | Khóa ngoại | Không rỗng | Bảng tham chiếu | Diễn giải |
|---|---|:---:|:---:|:---:|---|---|
| id | UUID | X | | X | | Khóa chính, mã định danh tài liệu RAG |
| title | VARCHAR(255) | | | | | Tiêu đề hoặc tên gợi nhớ của tài liệu |
| source_file | VARCHAR(255) | | | | | Tên tập tin gốc tải lên (PDF, DOCX, TXT...) |
| created_at | TIMESTAMP | | | X | | Thời điểm tải tài liệu lên hệ thống |

---

## 11. Bảng `rag_chunks` (Đoạn văn bản và vector embedding)

Bảng lưu trữ các đoạn văn bản được chia nhỏ (chunk) từ tài liệu RAG kèm vector embedding tương ứng sinh ra bởi Gemini API.

| Tên thuộc tính | Kiểu dữ liệu | Khóa chính | Khóa ngoại | Không rỗng | Bảng tham chiếu | Diễn giải |
|---|---|:---:|:---:|:---:|---|---|
| id | UUID | X | | X | | Khóa chính, mã định danh đoạn văn bản (chunk) |
| document_id | UUID | | X | X | rag_documents(id) | Mã tài liệu gốc chứa đoạn văn bản này (ON DELETE CASCADE) |
| content | TEXT | | | X | | Nội dung văn bản của đoạn chunk |
| embedding | VECTOR(768) | | | X | | Vector nhúng ngữ nghĩa chiều dài 768 tạo bởi Gemini Embedding API |
| created_at | TIMESTAMP | | | X | | Thời điểm tạo vector embedding |

- **Chỉ mục vector:** Được đánh chỉ mục `ivfflat` với metric cosine similarity `vector_cosine_ops` để tối ưu tốc độ tìm kiếm tương đồng.

---

## 12. Bảng `refresh_tokens` (Token làm mới phiên đăng nhập)

Bảng lưu trữ refresh token trong cơ sở dữ liệu để quản lý phiên đăng nhập và hỗ trợ thu hồi token (revoke) chủ động khi đăng xuất.

| Tên thuộc tính | Kiểu dữ liệu | Khóa chính | Khóa ngoại | Không rỗng | Bảng tham chiếu | Diễn giải |
|---|---|:---:|:---:|:---:|---|---|
| id | UUID | X | | X | | Khóa chính, mã định danh refresh token |
| user_id | UUID | | X | X | users(id) | Mã người dùng sở hữu token (ON DELETE CASCADE) |
| token | VARCHAR(500) | | | X | | Chuỗi refresh token duy nhất (Unique) |
| expires_at | TIMESTAMP | | | X | | Thời hạn hết hiệu lực của token (ví dụ: sau 7 đến 30 ngày) |
| revoked | BOOLEAN | | | X | | Trạng thái thu hồi token (mặc định FALSE; TRUE khi đăng xuất hoặc nghi ngờ rò rỉ) |
| created_at | TIMESTAMP | | | X | | Thời điểm cấp phát refresh token |

---

## 13. Bảng `email_verification_tokens` (Token xác thực email)

Bảng lưu trữ mã token tạm thời gửi qua dịch vụ email Resend để kích hoạt tài khoản khi người dùng mới đăng ký.

| Tên thuộc tính | Kiểu dữ liệu | Khóa chính | Khóa ngoại | Không rỗng | Bảng tham chiếu | Diễn giải |
|---|---|:---:|:---:|:---:|---|---|
| id | UUID | X | | X | | Khóa chính, mã định danh token xác thực email |
| user_id | UUID | | X | X | users(id) | Mã người dùng cần xác thực email (ON DELETE CASCADE) |
| token | VARCHAR(255) | | | X | | Chuỗi token xác thực duy nhất (Unique) gửi qua liên kết email |
| expires_at | TIMESTAMP | | | X | | Thời hạn hết hạn của token (mặc định 24 giờ) |
| used | BOOLEAN | | | X | | Đánh dấu token đã được sử dụng hay chưa (mặc định FALSE) |
| created_at | TIMESTAMP | | | X | | Thời điểm tạo và gửi token |

---

## 14. Bảng `password_reset_tokens` (Token đặt lại mật khẩu)

Bảng lưu trữ mã token tạm thời gửi qua email Resend khi người dùng yêu cầu khôi phục/đặt lại mật khẩu bị quên.

| Tên thuộc tính | Kiểu dữ liệu | Khóa chính | Khóa ngoại | Không rỗng | Bảng tham chiếu | Diễn giải |
|---|---|:---:|:---:|:---:|---|---|
| id | UUID | X | | X | | Khóa chính, mã định danh token đặt lại mật khẩu |
| user_id | UUID | | X | X | users(id) | Mã người dùng yêu cầu đặt lại mật khẩu (ON DELETE CASCADE) |
| token | VARCHAR(255) | | | X | | Chuỗi token đặt lại mật khẩu duy nhất (Unique) gửi qua liên kết email |
| expires_at | TIMESTAMP | | | X | | Thời hạn hết hạn của token (mặc định 1 giờ) |
| used | BOOLEAN | | | X | | Đánh dấu token đã được sử dụng hay chưa (mặc định FALSE) |
| created_at | TIMESTAMP | | | X | | Thời điểm tạo và gửi token |

---

## 15. Bảng `vouchers` (Mã giảm giá)

Bảng lưu trữ thông tin các mã giảm giá (voucher) do Admin tạo và quản lý để áp dụng cho đơn hàng.

| Tên thuộc tính | Kiểu dữ liệu | Khóa chính | Khóa ngoại | Không rỗng | Bảng tham chiếu | Diễn giải |
|---|---|:---:|:---:|:---:|---|---|
| id | UUID | X | | X | | Khóa chính, mã định danh duy nhất của voucher |
| code | VARCHAR(50) | | | X | | Mã voucher viết hoa, duy nhất (Unique, ví dụ: `TECH50K`) |
| name | VARCHAR(255) | | | X | | Tên chương trình khuyến mãi |
| description | TEXT | | | | | Mô tả chi tiết điều kiện áp dụng voucher |
| discount_type | VARCHAR(20) | | | X | | Loại giảm giá (`PERCENTAGE` hoặc `FIXED_AMOUNT`) |
| discount_value | DECIMAL(15, 2) | | | X | | Giá trị giảm (% hoặc số tiền VNĐ tùy theo `discount_type`) |
| max_discount_amount | DECIMAL(15, 2) | | | | | Số tiền giảm tối đa (áp dụng khi `discount_type = PERCENTAGE`) |
| min_order_amount | DECIMAL(15, 2) | | | | | Giá trị đơn hàng tối thiểu để áp dụng voucher (mặc định 0) |
| start_date | TIMESTAMP | | | X | | Thời điểm bắt đầu có hiệu lực |
| end_date | TIMESTAMP | | | X | | Thời điểm hết hạn voucher |
| usage_limit | INT | | | | | Tổng số lượt sử dụng tối đa trên toàn hệ thống |
| used_count | INT | | | X | | Số lượt đã sử dụng hiện tại (mặc định 0) |
| user_usage_limit | INT | | | | | Số lượt sử dụng tối đa trên mỗi khách hàng (mặc định 1) |
| is_active | BOOLEAN | | | | | Trạng thái kích hoạt voucher (mặc định TRUE) |
| created_at | TIMESTAMP | | | | | Thời điểm tạo voucher |
| updated_at | TIMESTAMP | | | | | Thời điểm cập nhật voucher gần nhất |

---

## 16. Bảng `voucher_usages` (Lịch sử sử dụng mã giảm giá)

Bảng lưu trữ lịch sử áp dụng mã giảm giá của khách hàng cho các đơn hàng.

| Tên thuộc tính | Kiểu dữ liệu | Khóa chính | Khóa ngoại | Không rỗng | Bảng tham chiếu | Diễn giải |
|---|---|:---:|:---:|:---:|---|---|
| id | UUID | X | | X | | Khóa chính, mã định danh lượt sử dụng voucher |
| voucher_id | UUID | | X | | vouchers(id) | Mã voucher đã được áp dụng |
| user_id | UUID | | X | | users(id) | Mã người dùng sử dụng voucher (ON DELETE CASCADE) |
| order_id | UUID | | X | | orders(id) | Mã đơn hàng áp dụng voucher (ON DELETE CASCADE) |
| discount_amount | DECIMAL(15, 2) | | | X | | Số tiền thực tế đã được giảm cho đơn hàng (VNĐ) |
| used_at | TIMESTAMP | | | X | | Thời điểm áp dụng mã giảm giá |

- **Ràng buộc:** `UNIQUE(user_id, voucher_id, order_id)` đảm bảo mỗi cặp người dùng – voucher – đơn hàng chỉ được ghi nhận tối đa một lần.
