<!-- Format của một use case -->
# Ví dụ use case Đăng nhập
| Use case | Nội dung |
|--- | --- |
| Tên use case | Đăng nhập |
| Mô tả | Use case cho phép người dùng dùng đăng nhập vào hệ thống để thực hiện chức năng của mình |
| Actor | Người dùng |
| Điều kiện kích hoạt | Khi người dùng chọn chức năng đăng nhập từ trang chủ của hệ thống |
| Tiền điều kiện | Người dùng đã có tài khoản trong hệ thống |
| Hậu điều kiện | Người dùng được xác thực và truy cập vào hệ thống |
| Luồng sự kiện chính | 1. Hệ thống hiển thị màn hình đăng nhập <br> 2. Người dùng nhập tên đăng nhập và mật khẩu <br> 3. Hệ thống kiểm tra thông tin đăng nhập <br> 4. Nếu thành công, hệ thống hiển thị màn hình đăng nhập thành công <br> 5. Kết thúc use case |
| Luồng sự kiện phụ | **A1 - Thông tin đăng nhập không hợp lệ**: người dùng nhập sai thông tin đăng nhập <br> 1. Hệ thống hiển thị lại màn hình đăng nhập để người dùng nhập lại thông tin kèm theo thông báo tên đăng nhập hoặc mật khẩu bị sai <br> 2. Quay lại bước 2 trong luồng sự kiện chính <br> **A2 - Quên mật khẩu**: người dùng chọn chức năng quên mật khẩu trên màn hình đăng nhập <br> 1. Hệ thống hiển thị màn hình để người dùng nhập email <br> 2. Người dùng nhập email và chọn nút chức năng Đặt lại mật khẩu <br> 3. Hệ thống kiểm tra email và gửi liên kết reset mật khẩu cho người dùng qua email <br> 4. Hệ thống hiển thị thông báo thành công <br> 5. Use case kết thúc |

<!-- Xác thực -->
# Đăng ký tài khoản
| Use case | Nội dung |
|--- | --- |
| Tên use case | Đăng ký tài khoản |
| Mô tả | Cho phép khách vãng lai đăng ký tài khoản mới để trở thành thành viên (User) trong hệ thống |
| Actor | Khách vãng lai (Guest) |
| Điều kiện kích hoạt | Người dùng chọn chức năng "Đăng ký" trên thanh điều hướng |
| Tiền điều kiện | Người dùng chưa đăng nhập |
| Hậu điều kiện | Tài khoản mới được lưu vào bảng `users` với vai trò `USER`, mật khẩu được mã hóa an toàn |
| Luồng sự kiện chính | 1. Người dùng chọn nút "Đăng ký" trên thanh điều hướng <br> 2. Hệ thống hiển thị biểu mẫu đăng ký (họ tên, email, mật khẩu, số điện thoại, địa chỉ) <br> 3. Người dùng nhập đầy đủ thông tin và nhấn nút "Đăng ký" <br> 4. Hệ thống kiểm tra dữ liệu hợp lệ (email đúng định dạng, mật khẩu >= 6 ký tự) và kiểm tra email chưa tồn tại trong hệ thống <br> 5. Hệ thống mã hóa mật khẩu bằng BCrypt và lưu người dùng mới vào cơ sở dữ liệu <br> 6. Hệ thống hiển thị thông báo đăng ký thành công và chuyển hướng sang trang đăng nhập <br> 7. Kết thúc use case |
| Luồng sự kiện phụ | **A1 - Email đã được sử dụng**: <br> 1. Ở bước 4, nếu email đã tồn tại, hệ thống thông báo lỗi "Email đã được sử dụng" <br> 2. Quay lại bước 3 của luồng chính <br> **A2 - Dữ liệu nhập không hợp lệ**: <br> 1. Ở bước 4, nếu thiếu thông tin bắt buộc hoặc mật khẩu quá ngắn, hệ thống thông báo yêu cầu nhập lại <br> 2. Quay lại bước 3 của luồng chính |

# Đăng nhập
| Use case | Nội dung |
|--- | --- |
| Tên use case | Đăng nhập |
| Mô tả | Cho phép người dùng đã có tài khoản đăng nhập vào hệ thống để mua sắm và sử dụng các tính năng thành viên |
| Actor | Khách hàng (User), Quản trị viên (Admin) |
| Điều kiện kích hoạt | Người dùng chọn chức năng "Đăng nhập" trên thanh điều hướng |
| Tiền điều kiện | Người dùng đã có tài khoản trong hệ thống |
| Hậu điều kiện | Người dùng được xác thực, hệ thống cấp phát token JWT và cập nhật giao diện đã đăng nhập |
| Luồng sự kiện chính | 1. Người dùng chọn nút "Đăng nhập" trên thanh điều hướng <br> 2. Hệ thống hiển thị biểu mẫu đăng nhập gồm email và mật khẩu <br> 3. Người dùng nhập email, mật khẩu và nhấn nút "Đăng nhập" <br> 4. Hệ thống kiểm tra thông tin tài khoản và so khớp mật khẩu trong cơ sở dữ liệu <br> 5. Hệ thống cấp phát cặp token JWT (Access Token lưu phía client, Refresh Token lưu trong HttpOnly Cookie) <br> 6. Hệ thống thông báo đăng nhập thành công và chuyển hướng người dùng về trang chủ (hoặc trang quản trị nếu là Admin) <br> 7. Kết thúc use case |
| Luồng sự kiện phụ | **A1 - Thông tin đăng nhập không chính xác**: <br> 1. Ở bước 4, nếu email hoặc mật khẩu không đúng, hệ thống hiển thị thông báo lỗi "Email hoặc mật khẩu không chính xác" <br> 2. Quay lại bước 3 của luồng chính |

# Đăng xuất
| Use case | Nội dung |
|--- | --- |
| Tên use case | Đăng xuất |
| Mô tả | Cho phép người dùng đang đăng nhập kết thúc phiên làm việc an toàn trên hệ thống |
| Actor | Khách hàng (User), Quản trị viên (Admin) |
| Điều kiện kích hoạt | Người dùng chọn mục "Đăng xuất" tại menu tài khoản |
| Tiền điều kiện | Người dùng đang ở trạng thái đã đăng nhập |
| Hậu điều kiện | Phiên làm việc kết thúc, Refresh Token trong cookie bị xóa bỏ và tài khoản chuyển về trạng thái khách |
| Luồng sự kiện chính | 1. Người dùng chọn chức năng "Đăng xuất" từ menu cá nhân <br> 2. Hệ thống gửi yêu cầu đăng xuất lên backend (`/api/auth/logout`) <br> 3. Hệ thống xóa Refresh Token khỏi Cookie và vô hiệu hóa phiên làm việc <br> 4. Hệ thống xóa token lưu trữ phía client, chuyển giao diện về trạng thái chưa đăng nhập và điều hướng về trang chủ <br> 5. Kết thúc use case |
| Luồng sự kiện phụ | Không có |

<!-- Thống kê số liệu -->
# Xem thống kê
| Use case | Nội dung |
|--- | --- |
| Tên use case | Xem thống kê |
| Mô tả | Cho phép quản trị viên xem tổng quan số liệu hoạt động của cửa hàng: doanh thu theo thời gian, đơn hàng theo trạng thái, top sản phẩm bán chạy và người dùng mới |
| Actor | Quản trị viên (Admin) |
| Điều kiện kích hoạt | Quản trị viên chọn mục "Thống kê" trên menu quản trị |
| Tiền điều kiện | Quản trị viên đã đăng nhập với vai trò `ADMIN` |
| Hậu điều kiện | Các số liệu và biểu đồ thống kê được hiển thị đầy đủ trên màn hình |
| Luồng sự kiện chính | 1. Quản trị viên truy cập trang "Thống kê" tại giao diện quản trị <br> 2. Hệ thống truy vấn dữ liệu từ cơ sở dữ liệu theo mốc thời gian (doanh thu từ đơn hàng `DELIVERED`, số lượng đơn hàng theo trạng thái, top sản phẩm bán chạy, số người dùng đăng ký mới) <br> 3. Hệ thống hiển thị các thẻ tổng hợp số liệu và biểu đồ trực quan tương ứng <br> 4. Quản trị viên có thể lọc hoặc chọn khoảng thời gian tùy chỉnh để xem lại số liệu <br> 5. Kết thúc use case |
| Luồng sự kiện phụ | **A1 - Không có dữ liệu trong mốc thời gian**: <br> 1. Nếu khoảng thời gian được chọn chưa phát sinh dữ liệu, hệ thống hiển thị giá trị mặc định (0) và thông báo trống trên biểu đồ <br> 2. Kết thúc use case |

<!-- Quản lý sản phẩm -->
# Quản lý sản phẩm
| Use case | Nội dung |
|--- | --- |
| Tên use case | Quản lý sản phẩm |
| Mô tả | Cho phép quản trị viên xem danh sách, thêm mới, cập nhật thông tin và xóa mềm sản phẩm linh kiện máy tính |
| Actor | Quản trị viên (Admin) |
| Điều kiện kích hoạt | Quản trị viên chọn mục "Quản lý sản phẩm" trên menu quản trị |
| Tiền điều kiện | Quản trị viên đã đăng nhập với vai trò `ADMIN` |
| Hậu điều kiện | Danh sách sản phẩm được hiển thị hoặc dữ liệu sản phẩm trong bảng `products` được thêm mới, cập nhật, vô hiệu hóa (`is_active = false`) |
| Luồng sự kiện chính | 1. Quản trị viên truy cập mục "Quản lý sản phẩm", hệ thống hiển thị danh sách sản phẩm (tìm kiếm, lọc theo category, phân trang) <br> 2. **Thêm mới**: Quản trị viên nhấn "Thêm sản phẩm", nhập thông tin chung (tên, giá, tồn kho, category, mô tả), thông số kỹ thuật (JSONB) và tải ảnh lên Cloudinary, sau đó nhấn "Lưu" <br> 3. **Cập nhật**: Quản trị viên chọn "Sửa" tại một sản phẩm, chỉnh sửa thông tin/ảnh cần thiết và nhấn "Cập nhật" <br> 4. **Xóa**: Quản trị viên chọn "Xóa" tại một sản phẩm và xác nhận để hệ thống thực hiện xóa mềm (`is_active = false`) <br> 5. Hệ thống kiểm tra dữ liệu hợp lệ, lưu thay đổi vào cơ sở dữ liệu và hiển thị thông báo thành công <br> 6. Kết thúc use case |
| Luồng sự kiện phụ | **A1 - Dữ liệu không hợp lệ hoặc lỗi tải ảnh**: <br> 1. Nếu thiếu thông tin bắt buộc, giá/tồn kho âm hoặc upload ảnh lên Cloudinary thất bại, hệ thống thông báo lỗi và yêu cầu nhập lại <br> **A2 - Quản trị viên hủy thao tác xóa**: <br> 1. Ở hộp thoại xác nhận xóa, nếu chọn "Hủy bỏ", hệ thống đóng hộp thoại và giữ nguyên sản phẩm |

<!-- Quản lý đơn hàng -->
# Quản lý đơn hàng
| Use case | Nội dung |
|--- | --- |
| Tên use case | Quản lý đơn hàng |
| Mô tả | Cho phép quản trị viên xem danh sách, xem chi tiết và cập nhật trạng thái xử lý của tất cả đơn hàng trong hệ thống |
| Actor | Quản trị viên (Admin) |
| Điều kiện kích hoạt | Quản trị viên chọn mục "Quản lý đơn hàng" trên menu quản trị |
| Tiền điều kiện | Quản trị viên đã đăng nhập với vai trò `ADMIN` |
| Hậu điều kiện | Danh sách đơn hàng được hiển thị hoặc trạng thái đơn hàng mới được lưu vào bảng `orders` |
| Luồng sự kiện chính | 1. Quản trị viên chọn mục "Quản lý đơn hàng", hệ thống hiển thị toàn bộ đơn hàng (có lọc theo trạng thái `PENDING`, `CONFIRMED`, `SHIPPING`, `DELIVERED`, `CANCELLED`) <br> 2. Quản trị viên chọn một đơn hàng để xem chi tiết thông tin giao nhận và danh sách mặt hàng <br> 3. Quản trị viên chọn trạng thái mới cho đơn hàng và nhấn "Lưu thay đổi" <br> 4. Hệ thống kiểm tra quy tắc chuyển trạng thái hợp lệ, cập nhật trạng thái vào cơ sở dữ liệu (nếu chuyển sang `CANCELLED` thì tự động hoàn lại tồn kho) và thông báo thành công <br> 5. Kết thúc use case |
| Luồng sự kiện phụ | **A1 - Chưa có đơn hàng nào**: <br> 1. Hệ thống hiển thị thông báo "Chưa có đơn hàng nào trong hệ thống" <br> **A2 - Chuyển đổi trạng thái không hợp lệ**: <br> 1. Nếu đơn hàng đã ở trạng thái `DELIVERED` hoặc `CANCELLED`, hệ thống từ chối thay đổi và hiển thị cảnh báo |

<!-- Quản lý PC Case -->
# Quản lý PC Case
| Use case | Nội dung |
|--- | --- |
| Tên use case | Quản lý PC Case |
| Mô tả | Cho phép quản trị viên xem danh sách, tạo mới, cập nhật và xóa mềm các bộ cấu hình PC Case kèm kiểm tra tương thích phần cứng tự động |
| Actor | Quản trị viên (Admin) |
| Điều kiện kích hoạt | Quản trị viên chọn mục "Quản lý PC Case" trên menu quản trị |
| Tiền điều kiện | Quản trị viên đã đăng nhập với vai trò `ADMIN` |
| Hậu điều kiện | Danh sách PC Case được hiển thị hoặc cấu hình PC Case được tạo mới, cập nhật, vô hiệu hóa (`is_active = false`) trong bảng `pc_cases` và `pc_case_items` |
| Luồng sự kiện chính | 1. Quản trị viên truy cập mục "Quản lý PC Case", hệ thống hiển thị danh sách các cấu hình đã tạo kèm linh kiện thành phần và tổng giá <br> 2. **Tạo mới**: Quản trị viên nhấn "Tạo PC Case mới", nhập tên, mục đích sử dụng, mô tả và chọn linh kiện cho từng slot (CPU, Mainboard, RAM, GPU, Storage, PSU, Case, CPU_Cooler) <br> 3. **Cập nhật**: Quản trị viên chọn "Sửa" tại một cấu hình để chỉnh sửa thông tin hoặc thay thế linh kiện <br> 4. **Xóa**: Quản trị viên chọn "Xóa" và xác nhận để vô hiệu hóa cấu hình (`is_active = false`) <br> 5. Khi tạo mới hoặc cập nhật, hệ thống tự động chạy thuật toán kiểm tra ràng buộc tương thích phần cứng và tồn kho, tính tổng giá rồi lưu vào cơ sở dữ liệu <br> 6. Kết thúc use case |
| Luồng sự kiện phụ | **A1 - Vi phạm ràng buộc tương thích hoặc hết hàng**: <br> 1. Nếu linh kiện không tương thích (socket, RAM, kích thước, công suất nguồn,...) hoặc hết hàng, hệ thống từ chối lưu và hiển thị danh sách lỗi vi phạm <br> **A2 - Quản trị viên hủy thao tác xóa**: <br> 1. Nếu chọn "Hủy bỏ" ở hộp thoại xác nhận, hệ thống giữ nguyên trạng thái cấu hình |

<!-- Quản lý tài liệu RAG -->
# Quản lý tài liệu RAG
| Use case | Nội dung |
|--- | --- |
| Tên use case | Quản lý tài liệu RAG |
| Mô tả | Cho phép quản trị viên xem danh sách, upload tài liệu chính sách (PDF, DOCX, TXT) để tạo vector embedding và xóa tài liệu khỏi hệ thống tri thức RAG |
| Actor | Quản trị viên (Admin) |
| Điều kiện kích hoạt | Quản trị viên chọn mục "Quản lý tài liệu RAG" trên menu quản trị |
| Tiền điều kiện | Quản trị viên đã đăng nhập với vai trò `ADMIN` |
| Hậu điều kiện | Danh sách tài liệu được hiển thị; tài liệu và các vector embedding tương ứng được thêm vào hoặc xóa khỏi bảng `rag_documents` và `rag_chunks` |
| Luồng sự kiện chính | 1. Quản trị viên truy cập mục "Quản lý tài liệu RAG", hệ thống hiển thị danh sách các tài liệu chính sách đã nạp <br> 2. **Upload tài liệu**: Quản trị viên nhấn "Upload tài liệu", nhập tiêu đề, chọn tệp (PDF/DOCX/TXT) và nhấn "Tải lên". Hệ thống tách đoạn (chunks), gọi Gemini Embedding API tạo vector và lưu vào PostgreSQL (pgvector) <br> 3. **Xóa tài liệu**: Quản trị viên nhấn "Xóa" tại một tài liệu và xác nhận để hệ thống xóa bản ghi tài liệu cùng toàn bộ vector chunks liên quan <br> 4. Hệ thống hiển thị thông báo thành công và cập nhật lại danh sách <br> 5. Kết thúc use case |
| Luồng sự kiện phụ | **A1 - Định dạng tệp không hợp lệ hoặc lỗi API Embedding**: <br> 1. Nếu tệp không thuộc định dạng PDF/DOCX/TXT hoặc gọi Gemini Embedding API thất bại, hệ thống thông báo lỗi và hủy thao tác upload <br> **A2 - Quản trị viên hủy thao tác xóa**: <br> 1. Nếu chọn "Hủy bỏ" ở hộp thoại xác nhận, hệ thống giữ nguyên tài liệu |

<!-- Quản lý mã giảm giá -->
# Quản lý mã giảm giá
| Use case | Nội dung |
|--- | --- |
| Tên use case | Quản lý mã giảm giá |
| Mô tả | Cho phép quản trị viên xem danh sách, tạo mới, cập nhật, bật/tắt trạng thái và xóa/vô hiệu hóa mã giảm giá (voucher) trong hệ thống |
| Actor | Quản trị viên (Admin) |
| Điều kiện kích hoạt | Quản trị viên chọn mục "Quản lý mã giảm giá" trên menu quản trị |
| Tiền điều kiện | Quản trị viên đã đăng nhập với vai trò `ADMIN` |
| Hậu điều kiện | Danh sách voucher được hiển thị hoặc thông tin voucher được tạo mới, cập nhật, thay đổi trạng thái, xóa khỏi bảng `vouchers` |
| Luồng sự kiện chính | 1. Quản trị viên truy cập mục "Quản lý mã giảm giá", hệ thống hiển thị danh sách voucher (có tìm kiếm, lọc và phân trang) cùng thống kê lượt đã sử dụng <br> 2. **Tạo mới**: Quản trị viên nhấn "Thêm mã giảm giá", nhập mã code, tên chương trình, loại giảm giá (`PERCENTAGE` hoặc `FIXED_AMOUNT`), giá trị giảm, mức giảm tối đa, đơn tối thiểu, thời gian áp dụng, giới hạn lượt dùng và nhấn "Lưu" <br> 3. **Cập nhật / Bật tắt trạng thái**: Quản trị viên chọn "Sửa" để chỉnh sửa thông tin voucher hoặc chuyển đổi nhanh trạng thái kích hoạt (`is_active`) <br> 4. **Xóa**: Quản trị viên chọn "Xóa" tại một voucher và xác nhận để xóa hoặc vô hiệu hóa voucher <br> 5. Hệ thống kiểm tra tính hợp lệ của dữ liệu, lưu vào cơ sở dữ liệu và thông báo thành công <br> 6. Kết thúc use case |
| Luồng sự kiện phụ | **A1 - Mã voucher bị trùng hoặc dữ liệu không hợp lệ**: <br> 1. Nếu mã code đã tồn tại, giá trị giảm âm hoặc ngày kết thúc nhỏ hơn ngày bắt đầu, hệ thống thông báo lỗi và yêu cầu nhập lại <br> **A2 - Quản trị viên hủy thao tác xóa**: <br> 1. Nếu chọn "Hủy bỏ" ở hộp thoại xác nhận, hệ thống giữ nguyên voucher |

<!-- Giỏ hàng & Đặt hàng -->
# Thêm vào giỏ hàng
| Use case | Nội dung |
|--- | --- |
| Tên use case | Thêm vào giỏ hàng |
| Mô tả | Cho phép người dùng thêm sản phẩm với số lượng mong muốn vào giỏ hàng cá nhân |
| Actor | Khách hàng (User) |
| Điều kiện kích hoạt | Người dùng nhấn nút "Thêm vào giỏ hàng" tại trang sản phẩm |
| Tiền điều kiện | Người dùng đã đăng nhập vào hệ thống; sản phẩm còn hàng trong kho (`stock_quantity > 0`) |
| Hậu điều kiện | Sản phẩm được thêm mới hoặc tăng số lượng trong giỏ hàng |
| Luồng sự kiện chính | 1. Người dùng chọn số lượng và nhấn nút "Thêm vào giỏ hàng" <br> 2. Hệ thống kiểm tra số lượng tồn kho của sản phẩm <br> 3. Hệ thống kiểm tra giỏ hàng: nếu sản phẩm đã có thì cộng dồn số lượng, nếu chưa thì thêm mới vào giỏ <br> 4. Hệ thống lưu giỏ hàng và hiển thị thông báo thêm thành công <br> 5. Kết thúc use case |
| Luồng sự kiện phụ | **A1 - Số lượng vượt quá tồn kho**: <br> 1. Ở bước 2, nếu số lượng yêu cầu vượt quá số lượng còn trong kho, hệ thống thông báo lỗi "Số lượng vượt quá tồn kho" <br> 2. Kết thúc use case <br> **A2 - Người dùng chưa đăng nhập**: <br> 1. Nếu phát hiện chưa đăng nhập, hệ thống chuyển hướng sang trang Đăng nhập <br> 2. Kết thúc use case |

# Xem giỏ hàng
| Use case | Nội dung |
|--- | --- |
| Tên use case | Xem giỏ hàng |
| Mô tả | Cho phép người dùng xem danh sách sản phẩm, số lượng, đơn giá và tổng số tiền tạm tính trong giỏ hàng |
| Actor | Khách hàng (User) |
| Điều kiện kích hoạt | Người dùng nhấn vào biểu tượng "Giỏ hàng" trên thanh điều hướng |
| Tiền điều kiện | Người dùng đã đăng nhập vào hệ thống |
| Hậu điều kiện | Màn hình giỏ hàng hiển thị đầy đủ thông tin các mặt hàng đã chọn |
| Luồng sự kiện chính | 1. Người dùng chọn biểu tượng Giỏ hàng <br> 2. Hệ thống truy vấn danh sách các mặt hàng trong giỏ của người dùng <br> 3. Hệ thống tính toán thành tiền từng món và tổng tiền tạm tính của giỏ hàng <br> 4. Hệ thống hiển thị giao diện giỏ hàng với thông tin sản phẩm, đơn giá, số lượng và tổng tiền <br> 5. Kết thúc use case |
| Luồng sự kiện phụ | **A1 - Giỏ hàng trống**: <br> 1. Ở bước 2, nếu giỏ hàng chưa có sản phẩm nào, hệ thống hiển thị thông báo "Giỏ hàng của bạn đang trống" kèm nút điều hướng "Tiếp tục mua sắm" <br> 2. Kết thúc use case |

# Cập nhật số lượng trong giỏ
| Use case | Nội dung |
|--- | --- |
| Tên use case | Cập nhật số lượng trong giỏ |
| Mô tả | Cho phép người dùng thay đổi số lượng (tăng, giảm hoặc nhập số lượng mới) của sản phẩm trong giỏ hàng |
| Actor | Khách hàng (User) |
| Điều kiện kích hoạt | Người dùng nhấn nút tăng/giảm hoặc nhập số lượng mới của sản phẩm tại trang giỏ hàng |
| Tiền điều kiện | Người dùng đã đăng nhập và đang ở trang Giỏ hàng |
| Hậu điều kiện | Số lượng sản phẩm được cập nhật, tổng tiền giỏ hàng được tính toán lại |
| Luồng sự kiện chính | 1. Người dùng thay đổi số lượng của một sản phẩm trong giỏ hàng <br> 2. Hệ thống kiểm tra số lượng mới hợp lệ (> 0) và không vượt quá tồn kho <br> 3. Hệ thống cập nhật lại số lượng sản phẩm trong giỏ hàng <br> 4. Hệ thống tính toán lại thành tiền và tổng giá trị đơn hàng, cập nhật lại giao diện <br> 5. Kết thúc use case |
| Luồng sự kiện phụ | **A1 - Số lượng vượt quá tồn kho**: <br> 1. Ở bước 2, nếu số lượng vượt quá tồn kho thực tế, hệ thống cảnh báo và đặt lại số lượng tối đa khả dụng <br> 2. Kết thúc use case <br> **A2 - Người dùng giảm số lượng về 0**: <br> 1. Hệ thống hỏi xác nhận xóa sản phẩm khỏi giỏ hàng <br> 2. Nếu đồng ý, thực hiện use case "Xóa khỏi giỏ hàng" <br> 3. Kết thúc use case |

# Xóa khỏi giỏ hàng
| Use case | Nội dung |
|--- | --- |
| Tên use case | Xóa khỏi giỏ hàng |
| Mô tả | Cho phép người dùng xóa một sản phẩm không còn nhu cầu mua ra khỏi giỏ hàng |
| Actor | Khách hàng (User) |
| Điều kiện kích hoạt | Người dùng nhấn nút "Xóa" cạnh sản phẩm tại trang giỏ hàng |
| Tiền điều kiện | Người dùng đã đăng nhập và giỏ hàng có sản phẩm |
| Hậu điều kiện | Sản phẩm được xóa khỏi giỏ hàng, tổng tiền giỏ hàng được cập nhật lại |
| Luồng sự kiện chính | 1. Người dùng nhấn nút "Xóa" tại sản phẩm muốn xóa trong giỏ hàng <br> 2. Hệ thống hiển thị hộp thoại yêu cầu xác nhận xóa <br> 3. Người dùng chọn xác nhận <br> 4. Hệ thống xóa sản phẩm khỏi giỏ hàng trong cơ sở dữ liệu <br> 5. Hệ thống tính lại tổng tiền, cập nhật giao diện giỏ hàng và hiển thị thông báo thành công <br> 6. Kết thúc use case |
| Luồng sự kiện phụ | **A1 - Người dùng hủy thao tác**: <br> 1. Ở bước 3, người dùng chọn "Hủy bỏ" trong hộp thoại xác nhận <br> 2. Hệ thống đóng hộp thoại và giữ nguyên sản phẩm trong giỏ hàng <br> 3. Kết thúc use case |

# Đặt hàng (COD)
| Use case | Nội dung |
|--- | --- |
| Tên use case | Đặt hàng (COD) |
| Mô tả | Cho phép người dùng tiến hành đặt mua các sản phẩm trong giỏ hàng theo phương thức thanh toán khi nhận hàng (COD) |
| Actor | Khách hàng (User) |
| Điều kiện kích hoạt | Người dùng nhấn nút "Tiến hành đặt hàng" hoặc "Thanh toán" tại trang giỏ hàng |
| Tiền điều kiện | Người dùng đã đăng nhập và giỏ hàng có ít nhất một sản phẩm còn hàng |
| Hậu điều kiện | Đơn hàng mới được tạo với trạng thái `PENDING`, tồn kho sản phẩm bị trừ, giỏ hàng được xóa các mặt hàng đã đặt |
| Luồng sự kiện chính | 1. Người dùng nhấn nút "Tiến hành đặt hàng" từ trang giỏ hàng <br> 2. Hệ thống hiển thị màn hình Đặt hàng (Checkout) gồm danh sách sản phẩm, tổng tiền và biểu mẫu thông tin giao hàng <br> 3. Người dùng nhập/kiểm tra thông tin người nhận (họ tên, số điện thoại, địa chỉ, ghi chú) và chọn phương thức thanh toán COD <br> 4. Người dùng nhấn nút "Xác nhận đặt hàng" <br> 5. Hệ thống kiểm tra tính hợp lệ của thông tin và kiểm tra lại tồn kho của tất cả sản phẩm <br> 6. Hệ thống tạo đơn hàng mới với trạng thái `PENDING`, lưu chi tiết các sản phẩm trong đơn (`order_items`) kèm giá bán tại thời điểm đặt, trừ số lượng tồn kho tương ứng và làm sạch giỏ hàng <br> 7. Hệ thống hiển thị thông báo đặt hàng thành công kèm mã đơn hàng <br> 8. Kết thúc use case |
| Luồng sự kiện phụ | **A1 - Thông tin giao hàng không hợp lệ**: <br> 1. Ở bước 5, nếu thiếu thông tin bắt buộc (họ tên, SĐT, địa chỉ), hệ thống hiển thị thông báo yêu cầu nhập đầy đủ <br> 2. Quay lại bước 3 của luồng chính <br> **A2 - Sản phẩm hết hàng hoặc không đủ tồn kho**: <br> 1. Ở bước 5, nếu có sản phẩm không còn đủ số lượng tồn kho, hệ thống thông báo lỗi chi tiết sản phẩm thiếu hàng <br> 2. Hệ thống chuyển hướng người dùng về trang giỏ hàng để cập nhật lại <br> 3. Kết thúc use case |

# Xem lịch sử đơn hàng
| Use case | Nội dung |
|--- | --- |
| Tên use case | Xem lịch sử đơn hàng |
| Mô tả | Cho phép người dùng xem danh sách toàn bộ các đơn hàng mà mình đã từng đặt trên hệ thống |
| Actor | Khách hàng (User) |
| Điều kiện kích hoạt | Người dùng chọn mục "Lịch sử đơn hàng" từ menu tài khoản hoặc thanh điều hướng |
| Tiền điều kiện | Người dùng đã đăng nhập vào hệ thống |
| Hậu điều kiện | Danh sách các đơn hàng đã đặt của người dùng được hiển thị |
| Luồng sự kiện chính | 1. Người dùng chọn mục "Lịch sử đơn hàng" <br> 2. Hệ thống truy vấn danh sách các đơn hàng của tài khoản từ cơ sở dữ liệu <br> 3. Hệ thống hiển thị danh sách đơn hàng (mã đơn hàng, ngày đặt, tổng tiền, phương thức COD, trạng thái: PENDING, CONFIRMED, SHIPPING, DELIVERED, CANCELLED) theo thứ tự thời gian mới nhất <br> 4. Kết thúc use case |
| Luồng sự kiện phụ | **A1 - Chưa có đơn hàng nào**: <br> 1. Ở bước 2, nếu người dùng chưa có đơn hàng nào, hệ thống hiển thị thông báo "Bạn chưa có đơn hàng nào" kèm nút "Mua sắm ngay" <br> 2. Kết thúc use case |

# Xem chi tiết đơn hàng
| Use case | Nội dung |
|--- | --- |
| Tên use case | Xem chi tiết đơn hàng |
| Mô tả | Cho phép người dùng xem thông tin chi tiết một đơn hàng cụ thể: thông tin người nhận, danh sách linh kiện đã mua và trạng thái đơn hàng |
| Actor | Khách hàng (User) |
| Điều kiện kích hoạt | Người dùng nhấn chọn một đơn hàng hoặc bấm nút "Xem chi tiết" tại trang lịch sử đơn hàng |
| Tiền điều kiện | Người dùng đã đăng nhập và đơn hàng thuộc về chính tài khoản đó |
| Hậu điều kiện | Thông tin chi tiết của đơn hàng được hiển thị trên màn hình |
| Luồng sự kiện chính | 1. Tại trang lịch sử đơn hàng, người dùng chọn đơn hàng muốn xem chi tiết <br> 2. Hệ thống kiểm tra quyền sở hữu và truy xuất dữ liệu đơn hàng cùng danh sách sản phẩm trong đơn <br> 3. Hệ thống hiển thị chi tiết: mã đơn hàng, ngày đặt, trạng thái đơn hàng, thông tin giao hàng (họ tên, SĐT, địa chỉ, ghi chú), danh sách sản phẩm (tên, hình ảnh, đơn giá mua, số lượng, thành tiền) và tổng tiền đơn hàng <br> 4. Kết thúc use case |
| Luồng sự kiện phụ | **A1 - Đơn hàng không tồn tại hoặc không hợp lệ**: <br> 1. Ở bước 2, nếu không tìm thấy đơn hàng hoặc đơn hàng không thuộc về người dùng, hệ thống thông báo lỗi "Không tìm thấy thông tin đơn hàng" <br> 2. Chuyển hướng người dùng về trang Lịch sử đơn hàng <br> 3. Kết thúc use case |

# Hủy đơn hàng
| Use case | Nội dung |
|--- | --- |
| Tên use case | Hủy đơn hàng |
| Mô tả | Cho phép người dùng chủ động hủy đơn hàng đã đặt khi đơn hàng vẫn đang ở trạng thái chờ xác nhận (`PENDING`) |
| Actor | Khách hàng (User) |
| Điều kiện kích hoạt | Người dùng nhấn nút "Hủy đơn hàng" tại trang lịch sử đơn hàng hoặc trang chi tiết đơn hàng |
| Tiền điều kiện | Người dùng đã đăng nhập và đơn hàng cần hủy đang ở trạng thái `PENDING` |
| Hậu điều kiện | Đơn hàng chuyển sang trạng thái `CANCELLED`, số lượng tồn kho của các linh kiện được hoàn trả |
| Luồng sự kiện chính | 1. Người dùng nhấn nút "Hủy đơn hàng" tại đơn hàng có trạng thái `PENDING` <br> 2. Hệ thống hiển thị hộp thoại xác nhận hủy đơn hàng <br> 3. Người dùng chọn xác nhận hủy <br> 4. Hệ thống kiểm tra lại trạng thái đơn hàng trong cơ sở dữ liệu: nếu vẫn là `PENDING`, hệ thống cập nhật trạng thái sang `CANCELLED` <br> 5. Hệ thống hoàn lại số lượng các linh kiện trong đơn vào lại kho hàng (`stock_quantity`) <br> 6. Hệ thống hiển thị thông báo hủy đơn hàng thành công và cập nhật lại giao diện <br> 7. Kết thúc use case |
| Luồng sự kiện phụ | **A1 - Đơn hàng không còn ở trạng thái PENDING**: <br> 1. Ở bước 4, nếu đơn hàng đã được admin xác nhận (`CONFIRMED`) hoặc đang giao (`SHIPPING`), hệ thống hiển thị thông báo lỗi "Đơn hàng đã được xác nhận hoặc đang xử lý, không thể hủy" <br> 2. Hệ thống tải lại trạng thái mới nhất của đơn hàng <br> 3. Kết thúc use case <br> **A2 - Người dùng hủy thao tác**: <br> 1. Ở bước 3, người dùng chọn đóng hộp thoại xác nhận <br> 2. Hệ thống giữ nguyên trạng thái đơn hàng <br> 3. Kết thúc use case |

<!-- Đánh giá sản phẩm -->
# Đánh giá sản phẩm
| Use case | Nội dung |
|--- | --- |
| Tên use case | Đánh giá sản phẩm |
| Mô tả | Cho phép người dùng đã mua và nhận hàng thành công đánh giá chất lượng sản phẩm bằng số sao (1–5) kèm nhận xét |
| Actor | Khách hàng (User) |
| Điều kiện kích hoạt | Người dùng nhấn nút "Viết đánh giá" tại trang chi tiết sản phẩm hoặc trang chi tiết đơn hàng đã giao |
| Tiền điều kiện | Người dùng đã đăng nhập, đã mua sản phẩm trong đơn hàng có trạng thái `DELIVERED`, và chưa từng đánh giá sản phẩm này |
| Hậu điều kiện | Đánh giá mới được lưu vào cơ sở dữ liệu (`product_reviews`), điểm sao trung bình của sản phẩm được cập nhật lại |
| Luồng sự kiện chính | 1. Người dùng chọn chức năng "Viết đánh giá" cho sản phẩm <br> 2. Hệ thống hiển thị biểu mẫu đánh giá gồm: chọn số sao (1–5 sao) và ô nhập nhận xét <br> 3. Người dùng chọn số sao, nhập nội dung nhận xét và nhấn nút "Gửi đánh giá" <br> 4. Hệ thống kiểm tra điều kiện hợp lệ: người dùng đã mua sản phẩm trong đơn hàng `DELIVERED` và chưa từng đánh giá sản phẩm này trước đó <br> 5. Hệ thống lưu đánh giá vào cơ sở dữ liệu và tính toán lại điểm đánh giá trung bình của sản phẩm <br> 6. Hệ thống hiển thị thông báo gửi đánh giá thành công và cập nhật lại giao diện <br> 7. Kết thúc use case |
| Luồng sự kiện phụ | **A1 - Chưa đủ điều kiện đánh giá**: <br> 1. Ở bước 4, nếu người dùng chưa từng mua sản phẩm hoặc đơn hàng chưa hoàn tất giao, hệ thống hiển thị thông báo "Bạn chỉ có thể đánh giá sản phẩm sau khi đã mua và nhận hàng thành công" <br> 2. Kết thúc use case <br> **A2 - Đã từng đánh giá sản phẩm này**: <br> 1. Ở bước 4, nếu người dùng đã từng đánh giá sản phẩm này trước đó, hệ thống thông báo "Mỗi sản phẩm bạn chỉ được đánh giá một lần" <br> 2. Kết thúc use case <br> **A3 - Thiếu điểm sao hoặc nội dung không hợp lệ**: <br> 1. Ở bước 4, nếu người dùng chưa chọn số sao hoặc nội dung quá dài (> 1000 ký tự), hệ thống cảnh báo yêu cầu bổ sung/chỉnh sửa <br> 2. Quay lại bước 3 của luồng chính |

# Xem đánh giá sản phẩm
| Use case | Nội dung |
|--- | --- |
| Tên use case | Xem đánh giá sản phẩm |
| Mô tả | Cho phép người dùng xem điểm sao trung bình và danh sách các nhận xét, đánh giá từ khách hàng khác về sản phẩm |
| Actor | Khách vãng lai (Guest), Khách hàng (User) |
| Điều kiện kích hoạt | Người dùng cuộn xuống phần "Đánh giá sản phẩm" tại trang chi tiết sản phẩm |
| Tiền điều kiện | Người dùng đang xem trang chi tiết của một sản phẩm cụ thể |
| Hậu điều kiện | Danh sách các đánh giá và điểm số trung bình được hiển thị đầy đủ |
| Luồng sự kiện chính | 1. Người dùng truy cập trang chi tiết của sản phẩm và cuộn đến khu vực đánh giá <br> 2. Hệ thống truy vấn danh sách các đánh giá của sản phẩm đó từ cơ sở dữ liệu (`product_reviews`) <br> 3. Hệ thống tính toán điểm sao trung bình và tổng số lượt đánh giá <br> 4. Hệ thống hiển thị: điểm sao trung bình, tổng số đánh giá, và danh sách từng đánh giá gồm tên người mua, số sao, nhận xét và ngày đánh giá <br> 5. Kết thúc use case |
| Luồng sự kiện phụ | **A1 - Sản phẩm chưa có đánh giá**: <br> 1. Ở bước 2, nếu sản phẩm chưa có lượt đánh giá nào, hệ thống hiển thị thông báo "Chưa có đánh giá nào cho sản phẩm này" <br> 2. Kết thúc use case |

<!-- Xem sản phẩm -->
# Xem danh sách sản phẩm
| Use case | Nội dung |
|--- | --- |
| Tên use case | Xem danh sách sản phẩm |
| Mô tả | Cho phép người dùng duyệt xem danh sách các linh kiện máy tính đang kinh doanh kèm theo phân trang và sắp xếp |
| Actor | Khách vãng lai (Guest), Khách hàng (User) |
| Điều kiện kích hoạt | Người dùng truy cập trang chủ hoặc chọn mục "Sản phẩm" trên thanh điều hướng |
| Tiền điều kiện | Không có |
| Hậu điều kiện | Danh sách các sản phẩm đang kinh doanh (`is_active = true`) được hiển thị trên giao diện |
| Luồng sự kiện chính | 1. Người dùng chọn mục Sản phẩm trên thanh điều hướng <br> 2. Hệ thống truy vấn danh sách sản phẩm đang hoạt động (`is_active = true`) từ cơ sở dữ liệu có áp dụng phân trang (mặc định 12 sản phẩm/trang) <br> 3. Hệ thống hiển thị danh sách sản phẩm dạng lưới: hình ảnh, tên sản phẩm, giá bán, tình trạng còn hàng và điểm sao đánh giá <br> 4. Người dùng có thể chuyển trang phân trang hoặc chọn sắp xếp theo giá (tăng/giảm dần) hoặc sản phẩm mới nhất <br> 5. Kết thúc use case |
| Luồng sự kiện phụ | **A1 - Không có sản phẩm nào**: <br> 1. Ở bước 2, nếu hệ thống chưa có sản phẩm nào đang kích hoạt, hiển thị thông báo "Hiện chưa có sản phẩm nào" <br> 2. Kết thúc use case |

# Tìm kiếm sản phẩm
| Use case | Nội dung |
|--- | --- |
| Tên use case | Tìm kiếm sản phẩm |
| Mô tả | Cho phép người dùng tìm kiếm nhanh linh kiện máy tính theo từ khóa tên sản phẩm |
| Actor | Khách vãng lai (Guest), Khách hàng (User) |
| Điều kiện kích hoạt | Người dùng nhập từ khóa vào ô tìm kiếm trên thanh điều hướng và nhấn phím Enter hoặc nút Tìm kiếm |
| Tiền điều kiện | Không có |
| Hậu điều kiện | Danh sách sản phẩm có tên khớp với từ khóa tìm kiếm được hiển thị |
| Luồng sự kiện chính | 1. Người dùng nhập từ khóa tìm kiếm (tên linh kiện, model) vào ô tìm kiếm và bấm Enter/biểu tượng Tìm kiếm <br> 2. Hệ thống gửi yêu cầu lên backend để tra cứu các sản phẩm có tên chứa từ khóa (không phân biệt chữ hoa, chữ thường) <br> 3. Hệ thống hiển thị danh sách sản phẩm khớp với từ khóa tìm kiếm kèm theo tổng số kết quả tìm thấy <br> 4. Kết thúc use case |
| Luồng sự kiện phụ | **A1 - Không tìm thấy kết quả**: <br> 1. Ở bước 2, nếu không có sản phẩm nào khớp với từ khóa, hệ thống hiển thị thông báo "Không tìm thấy sản phẩm nào phù hợp với từ khóa" và gợi ý người dùng thử lại từ khóa khác <br> 2. Kết thúc use case <br> **A2 - Từ khóa tìm kiếm để trống**: <br> 1. Nếu người dùng nhấn tìm kiếm khi ô nhập rỗng, hệ thống giữ nguyên trang hoặc hiển thị danh sách toàn bộ sản phẩm <br> 2. Kết thúc use case |

# Lọc sản phẩm
| Use case | Nội dung |
|--- | --- |
| Tên use case | Lọc sản phẩm |
| Mô tả | Cho phép người dùng thu hẹp danh sách sản phẩm theo danh mục linh kiện (CPU, RAM, GPU,...) và khoảng giá |
| Actor | Khách vãng lai (Guest), Khách hàng (User) |
| Điều kiện kích hoạt | Người dùng chọn các tiêu chí lọc (danh mục, khoảng giá) tại thanh lọc của trang sản phẩm |
| Tiền điều kiện | Người dùng đang ở trang Danh sách sản phẩm |
| Hậu điều kiện | Danh sách sản phẩm thỏa mãn đồng thời các điều kiện lọc được hiển thị |
| Luồng sự kiện chính | 1. Tại trang sản phẩm, người dùng chọn tiêu chí lọc: danh mục linh kiện (CPU, Mainboard, RAM, GPU, PSU, Case, CPU_COOLER, STORAGE) và/hoặc chọn khoảng giá <br> 2. Người dùng nhấn nút "Áp dụng" (hoặc hệ thống tự động lọc theo thao tác) <br> 3. Hệ thống truy vấn danh sách sản phẩm thỏa mãn đồng thời các điều kiện lọc <br> 4. Hệ thống cập nhật hiển thị danh sách sản phẩm tương ứng kèm số lượng kết quả tìm được <br> 5. Kết thúc use case |
| Luồng sự kiện phụ | **A1 - Không có sản phẩm phù hợp**: <br> 1. Ở bước 3, nếu không tìm thấy sản phẩm nào thỏa mãn tất cả tiêu chí, hệ thống hiển thị thông báo "Không có sản phẩm nào phù hợp với bộ lọc đã chọn" kèm nút "Đặt lại bộ lọc" <br> 2. Kết thúc use case <br> **A2 - Khoảng giá không hợp lệ**: <br> 1. Nếu người dùng nhập giá tối thiểu lớn hơn giá tối đa hoặc nhập số âm, hệ thống hiển thị cảnh báo và yêu cầu nhập lại <br> 2. Quay lại bước 1 của luồng chính |

# Xem chi tiết sản phẩm
| Use case | Nội dung |
|--- | --- |
| Tên use case | Xem chi tiết sản phẩm |
| Mô tả | Cho phép người dùng xem thông tin toàn diện về một sản phẩm, bao gồm giá, tồn kho, mô tả, thông số kỹ thuật chi tiết (JSONB) và đánh giá |
| Actor | Khách vãng lai (Guest), Khách hàng (User) |
| Điều kiện kích hoạt | Người dùng nhấn vào thẻ, tên hoặc hình ảnh của một sản phẩm trong danh sách |
| Tiền điều kiện | Sản phẩm được chọn tồn tại trong hệ thống |
| Hậu điều kiện | Trang chi tiết sản phẩm được hiển thị đầy đủ thông tin |
| Luồng sự kiện chính | 1. Người dùng nhấn chọn một sản phẩm từ danh sách hoặc kết quả tìm kiếm <br> 2. Hệ thống truy xuất dữ liệu sản phẩm từ cơ sở dữ liệu (`products`), gồm: tên, giá, số lượng tồn kho (`stock_quantity`), mô tả, hình ảnh và thông số kỹ thuật chi tiết trong trường `detail` (JSONB) <br> 3. Hệ thống hiển thị trang chi tiết: hình ảnh lớn, tên sản phẩm, giá bán, tình trạng còn/hết hàng, bảng thông số kỹ thuật theo từng category (socket, xung nhịp, công suất, kích thước,...), nút "Thêm vào giỏ hàng" và khu vực đánh giá <br> 4. Kết thúc use case |
| Luồng sự kiện phụ | **A1 - Sản phẩm không tồn tại hoặc đã ngừng bán**: <br> 1. Ở bước 2, nếu sản phẩm không tìm thấy hoặc đã bị xóa mềm (`is_active = false`), hệ thống hiển thị thông báo "Sản phẩm không tồn tại hoặc đã ngừng kinh doanh" và điều hướng về trang danh sách sản phẩm <br> 2. Kết thúc use case |

<!-- Chatbot AI -->
# Gửi tin nhắn cho chatbot
| Use case | Nội dung |
|--- | --- |
| Tên use case | Gửi tin nhắn cho chatbot |
| Mô tả | Cho phép người dùng nhập câu hỏi hoặc yêu cầu tư vấn vào khung chat để trò chuyện với Chatbot AI của cửa hàng |
| Actor | Khách vãng lai (Guest), Khách hàng (User) |
| Điều kiện kích hoạt | Người dùng mở khung chat ở góc màn hình và gửi tin nhắn văn bản |
| Tiền điều kiện | Không yêu cầu đăng nhập |
| Hậu điều kiện | Tin nhắn được tiếp nhận, hệ thống xử lý và hiển thị phản hồi từ Chatbot AI cho người dùng |
| Luồng sự kiện chính | 1. Người dùng nhấn biểu tượng Chatbot AI để mở cửa sổ trò chuyện <br> 2. Người dùng nhập câu hỏi (về chính sách shop hoặc nhu cầu tư vấn cấu hình PC) và nhấn nút "Gửi" <br> 3. Hệ thống hiển thị tin nhắn của người dùng trong khung chat và hiển thị trạng thái đang xử lý ("Bot đang trả lời...") <br> 4. Hệ thống phân tích ý định của tin nhắn: nếu là câu hỏi chính sách shop thì thực hiện use case "Trả lời chính sách shop (RAG)", nếu là yêu cầu tư vấn PC thì thực hiện use case "Gợi ý cấu hình PC" <br> 5. Hệ thống nhận câu trả lời từ phân hệ AI tương ứng và hiển thị tin nhắn phản hồi lên khung chat cho người dùng <br> 6. Kết thúc use case |
| Luồng sự kiện phụ | **A1 - Nội dung tin nhắn trống**: <br> 1. Nếu người dùng gửi tin nhắn rỗng, hệ thống không gửi yêu cầu và nhắc người dùng nhập nội dung <br> 2. Kết thúc use case <br> **A2 - Lỗi kết nối dịch vụ AI**: <br> 1. Ở bước 5, nếu gọi Gemini API gặp sự cố hoặc mất kết nối mạng, hệ thống hiển thị tin nhắn "Xin lỗi, chatbot đang bận hoặc gặp sự cố kết nối. Vui lòng thử lại sau giây lát!" <br> 2. Kết thúc use case |

# Trả lời chính sách shop (RAG)
| Use case | Nội dung |
|--- | --- |
| Tên use case | Trả lời chính sách shop (RAG) |
| Mô tả | Hệ thống tự động tra cứu tài liệu chính sách (bảo hành, đổi trả, vận chuyển, thanh toán) bằng vector embedding và sử dụng Gemini AI để trả lời chính xác, tránh ảo giác thông tin |
| Actor | Gemini AI |
| Điều kiện kích hoạt | Use case "Gửi tin nhắn cho chatbot" phân loại tin nhắn của người dùng là câu hỏi liên quan đến chính sách cửa hàng |
| Tiền điều kiện | Tài liệu chính sách đã được Admin upload và lưu trữ vector embedding trong bảng `rag_chunks` (pgvector) |
| Hậu điều kiện | Phản hồi chính xác dựa trên tài liệu chính sách của cửa hàng được sinh ra và gửi cho người dùng |
| Luồng sự kiện chính | 1. Hệ thống tiếp nhận câu hỏi chính sách từ use case "Gửi tin nhắn cho chatbot" <br> 2. Hệ thống gọi Gemini Embedding API để tạo vector biểu diễn ngữ nghĩa cho câu hỏi <br> 3. Hệ thống thực hiện tìm kiếm vector tương đồng (khoảng cách cosine) trên bảng `rag_chunks` trong PostgreSQL để lấy top các đoạn tài liệu chính sách liên quan nhất <br> 4. Hệ thống đưa các đoạn trích tài liệu này vào ngữ cảnh (Context) kèm câu lệnh ràng buộc (System Instruction: chỉ trả lời dựa trên tài liệu được cung cấp) gửi tới Gemini Chat API <br> 5. Gemini AI tổng hợp nội dung và trả về câu trả lời tự nhiên, chính xác theo tài liệu của shop <br> 6. Hệ thống chuyển tiếp câu trả lời về cho khung chat hiển thị cho người dùng <br> 7. Kết thúc use case |
| Luồng sự kiện phụ | **A1 - Không tìm thấy thông tin chính sách liên quan**: <br> 1. Ở bước 3 và 4, nếu cơ sở tri thức không có tài liệu nào liên quan đến câu hỏi, chatbot phản hồi lịch sự rằng cửa hàng chưa có thông tin về vấn đề này và hướng dẫn người dùng liên hệ trực tiếp với nhân viên hỗ trợ <br> 2. Kết thúc use case |

# Gợi ý cấu hình PC
| Use case | Nội dung |
|--- | --- |
| Tên use case | Gợi ý cấu hình PC |
| Mô tả | Hệ thống tư vấn và đề xuất 1–3 bộ PC Case hoàn chỉnh phù hợp nhất với ngân sách và nhu cầu của người dùng từ danh sách cấu hình có sẵn và còn hàng trong kho |
| Actor | Gemini AI |
| Điều kiện kích hoạt | Use case "Gửi tin nhắn cho chatbot" phân loại tin nhắn của người dùng là yêu cầu tư vấn hoặc gợi ý cấu hình PC |
| Tiền điều kiện | Hệ thống có sẵn các bộ PC Case do Admin tạo (`is_active = true`) và toàn bộ linh kiện thành phần phải còn tồn kho (`stock_quantity > 0`) |
| Hậu điều kiện | Danh sách 1–3 bộ PC Case phù hợp kèm giải thích được hiển thị trên khung chat |
| Luồng sự kiện chính | 1. Hệ thống tiếp nhận yêu cầu về ngân sách và mục đích sử dụng từ use case "Gửi tin nhắn cho chatbot" <br> 2. Hệ thống truy vấn từ bảng `pc_cases` danh sách các bộ PC Case đang kinh doanh (`is_active = true`) và kiểm tra toàn bộ linh kiện thành phần phải có `stock_quantity > 0` <br> 3. Hệ thống cấu trúc hóa danh sách PC Case hợp lệ (tên, mục đích sử dụng, tổng giá, danh sách linh kiện) thành dữ liệu ngữ cảnh đưa vào Prompt gửi tới Gemini AI kèm nguyên tắc: "Chỉ được chọn từ các bộ cấu hình trong danh sách, tuyệt đối không tự ý ghép nối linh kiện mới" <br> 4. Gemini AI phân tích nhu cầu và ngân sách của người dùng, lựa chọn ra từ 1 đến 3 bộ PC Case phù hợp nhất kèm lời giải thích lý do <br> 5. Hệ thống nhận kết quả từ Gemini API, định dạng câu trả lời kèm thẻ thông tin chi tiết từng bộ PC Case và gửi về khung chat hiển thị cho người dùng <br> 6. Kết thúc use case |
| Luồng sự kiện phụ | **A1 - Không có cấu hình nào phù hợp ngân sách hoặc nhu cầu**: <br> 1. Ở bước 4, nếu ngân sách của người dùng không đủ hoặc không có bộ máy nào phù hợp, chatbot thông báo lịch sự mức giá tối thiểu của các cấu hình hiện có và đưa ra lời khuyên phù hợp <br> 2. Kết thúc use case <br> **A2 - Các cấu hình trong tầm giá tạm hết hàng**: <br> 1. Ở bước 2, nếu các bộ PC Case phù hợp có linh kiện bị hết hàng, hệ thống thông báo tạm hết hàng và gợi ý các cấu hình gần nhất còn hàng trong kho <br> 2. Kết thúc use case |
