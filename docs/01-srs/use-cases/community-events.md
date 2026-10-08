# Use case: Community và Events

> **Người viết:** TV3 (PiLo257) | **Reviewer:** TV1 (Tuấn Nguyễn Thanh) | **Task Jira:** SCRUM-20 | **Hạn nộp review:** Thứ Năm 8/10
> **Trạng thái:** Nháp 
> Cách làm: tạo nhánh `docs/<mã SCRUM>-<tên-ngắn>` từ `develop`, điền vào file này, mở Pull Request vào `develop`. Xem `docs/README.md`.

## 1. Phạm vi và Actor

Module Community và Events cung cấp các chức năng phục vụ việc chia sẻ nội dung, tương tác trong gia đình và tổ chức các sự kiện gia đình.

### Actor

| Actor | Vai trò |
|---|---|
| Thành viên (MEMBER) | Tạo bài đăng, bình luận, reaction, chia sẻ ảnh, tham gia sự kiện |
| Trưởng chi (BRANCH_ADMIN) | Quản lý nội dung/thông báo theo quyền của chi |
| Người tạo sự kiện | Tạo và quản lý người tham dự sự kiện |
| Hệ thống | Gửi nhắc nhở sự kiện |

### Quy tắc truy cập

- Người dùng phải đăng nhập để sử dụng các chức năng dành cho thành viên.
- Chỉ thành viên đã được xác minh mới được truy cập các nội dung yêu cầu quyền thành viên.
- Quyền tạo, sửa và xóa nội dung được kiểm tra theo vai trò và quyền của người dùng.
- Người dùng chỉ được sửa hoặc xóa nội dung do mình tạo, hoặc nội dung mà vai trò của mình được phép quản lý.
- Nội dung của gia đình chỉ được hiển thị cho thành viên đã được xác minh của đúng gia đình.

### Phân trang

Các API trả về danh sách sử dụng phân trang theo `page` và `size`.

Ví dụ:

`?page=0&size=20&sort=createdAt,desc`

Thông tin phân trang được trả về trong trường `meta` theo quy ước API chung của FamilyConnect.

## 2. Danh sách use case

| Mã UC | Tên | Actor | Mã FR | Ưu tiên |
|---|---|---|---|---|
| UC-COM-01 | Tạo, sửa, xóa bài đăng | Thành viên | FR-COM-01 | Must |
| UC-COM-02 | Bình luận | Thành viên | FR-COM-02 | Must |
| UC-COM-03 | Thả reaction | Thành viên | FR-COM-02 | Must |
| UC-COM-04 | Chia sẻ tin tức gia đình | Thành viên | FR-COM-03 | Must |
| UC-COM-05 | Tải và chia sẻ ảnh | Thành viên | FR-COM-04 | Must |
| UC-COM-06 | Đăng thông báo (announcement) | Trưởng chi | FR-COM-05 | Must |
| UC-EVT-01 | Tạo sự kiện | Thành viên, Trưởng chi | FR-EVT-01 | Must |
| UC-EVT-02 | RSVP (Tham dự / Không / Có thể) | Thành viên | FR-EVT-02 | Must |
| UC-EVT-03 | Quản lý người tham dự | Người tạo sự kiện | FR-EVT-03 | Must |
| UC-EVT-04 | Thư viện ảnh sự kiện | Thành viên | FR-EVT-04 | Must |
| UC-EVT-05 | Nhắc nhở sự kiện | Hệ thống | FR-EVT-05 | Must |

## 3. Đặc tả từng use case

### UC-COM-01: Tạo, sửa, xóa bài đăng

| Mục | Nội dung |
|---|---|
| Mã FR liên quan | FR-COM-01 |
| Actor | Thành viên |
| Mô tả ngắn | Thành viên tạo bài đăng và có thể sửa hoặc xóa bài đăng của mình. |
| Tiền điều kiện | Người dùng đã đăng nhập và là thành viên đã được xác minh. |
| Luồng chính | 1. Thành viên chọn chức năng tạo bài đăng.<br>2. Thành viên nhập nội dung bài đăng và gửi yêu cầu.<br>3. Hệ thống kiểm tra dữ liệu và quyền truy cập.<br>4. Hệ thống lưu bài đăng và hiển thị kết quả. |
| Luồng thay thế / ngoại lệ | 2a. Nội dung không hợp lệ, hệ thống trả về lỗi và yêu cầu nhập lại.<br>3a. Người dùng không có quyền, hệ thống từ chối yêu cầu. |
| Hậu điều kiện | Bài đăng được tạo và có thể hiển thị cho các thành viên được phép xem. |
| Quy tắc nghiệp vụ | BR-01 |

### UC-COM-02: Bình luận

| Mục | Nội dung |
|---|---|
| Mã FR liên quan | FR-COM-02 |
| Actor | Thành viên |
| Mô tả ngắn | Thành viên thêm bình luận vào bài đăng mà mình được phép xem. |
| Tiền điều kiện | Người dùng đã đăng nhập và đã được xác minh thành viên. Bài đăng tồn tại và người dùng được phép xem. |
| Luồng chính | 1. Thành viên mở một bài đăng.<br>2. Thành viên nhập nội dung bình luận và gửi.<br>3. Hệ thống kiểm tra quyền và dữ liệu.<br>4. Hệ thống lưu bình luận và hiển thị bình luận. |
| Luồng thay thế / ngoại lệ | 2a. Nội dung bình luận không hợp lệ, hệ thống yêu cầu nhập lại.<br>3a. Người dùng không có quyền xem bài đăng, hệ thống từ chối yêu cầu. |
| Hậu điều kiện | Bình luận được lưu vào bài đăng. |
| Quy tắc nghiệp vụ | BR-02 |

### UC-COM-03: Thả reaction

| Mục | Nội dung |
|---|---|
| Mã FR liên quan | FR-COM-02 |
| Actor | Thành viên |
| Mô tả ngắn | Thành viên thêm hoặc thay đổi reaction trên bài đăng được phép xem. |
| Tiền điều kiện | Người dùng đã đăng nhập, đã được xác minh và có quyền xem bài đăng. |
| Luồng chính | 1. Thành viên mở bài đăng.<br>2. Thành viên chọn reaction.<br>3. Hệ thống kiểm tra quyền truy cập.<br>4. Hệ thống lưu reaction và cập nhật kết quả. |
| Luồng thay thế / ngoại lệ | 2a. Thành viên bỏ reaction hiện tại, hệ thống xóa reaction tương ứng.<br>3a. Người dùng không có quyền xem bài đăng, hệ thống từ chối yêu cầu. |
| Hậu điều kiện | Reaction của thành viên được cập nhật trên bài đăng. |
| Quy tắc nghiệp vụ | BR-03 |

### UC-COM-04: Chia sẻ tin tức gia đình

| Mục | Nội dung |
|---|---|
| Mã FR liên quan | FR-COM-03 |
| Actor | Thành viên |
| Mô tả ngắn | Thành viên chia sẻ một tin tức liên quan đến gia đình. |
| Tiền điều kiện | Người dùng đã đăng nhập và đã được xác minh thành viên. |
| Luồng chính | 1. Thành viên chọn chức năng chia sẻ tin tức gia đình.<br>2. Thành viên nhập nội dung tin tức.<br>3. Hệ thống kiểm tra dữ liệu và quyền truy cập.<br>4. Hệ thống lưu tin tức và hiển thị cho thành viên được phép xem. |
| Luồng thay thế / ngoại lệ | 2a. Nội dung không hợp lệ, hệ thống yêu cầu nhập lại.<br>3a. Người dùng không có quyền, hệ thống từ chối yêu cầu. |
| Hậu điều kiện | Tin tức gia đình được lưu và hiển thị theo quyền truy cập. |
| Quy tắc nghiệp vụ | BR-04 |

### UC-COM-05: Tải và chia sẻ ảnh

| Mục | Nội dung |
|---|---|
| Mã FR liên quan | FR-COM-04 |
| Actor | Thành viên |
| Mô tả ngắn | Thành viên tải ảnh lên và chia sẻ ảnh trong phạm vi gia đình. |
| Tiền điều kiện | Người dùng đã đăng nhập và đã được xác minh thành viên. |
| Luồng chính | 1. Thành viên chọn chức năng tải ảnh.<br>2. Thành viên chọn tệp ảnh và gửi yêu cầu.<br>3. Hệ thống kiểm tra tệp và quyền truy cập.<br>4. Hệ thống lưu thông tin ảnh và hiển thị ảnh. |
| Luồng thay thế / ngoại lệ | 2a. Tệp không hợp lệ, hệ thống từ chối tệp.<br>3a. Người dùng không có quyền, hệ thống từ chối yêu cầu. |
| Hậu điều kiện | Ảnh được lưu và có thể được xem bởi các thành viên được phép. |
| Quy tắc nghiệp vụ | BR-05 |

### UC-COM-06: Đăng thông báo (announcement)

| Mục | Nội dung |
|---|---|
| Mã FR liên quan | FR-COM-05 |
| Actor | Trưởng chi |
| Mô tả ngắn | Trưởng chi đăng thông báo cho các thành viên trong phạm vi được quản lý. |
| Tiền điều kiện | Người dùng đã đăng nhập và có quyền Trưởng chi. |
| Luồng chính | 1. Trưởng chi chọn chức năng tạo thông báo.<br>2. Trưởng chi nhập nội dung thông báo.<br>3. Hệ thống kiểm tra quyền và dữ liệu.<br>4. Hệ thống lưu thông báo và hiển thị cho đối tượng được phép xem. |
| Luồng thay thế / ngoại lệ | 2a. Nội dung không hợp lệ, hệ thống yêu cầu nhập lại.<br>3a. Người dùng không có quyền Trưởng chi, hệ thống từ chối yêu cầu. |
| Hậu điều kiện | Thông báo được lưu và hiển thị theo phạm vi được phép. |
| Quy tắc nghiệp vụ | BR-06 |

### UC-EVT-01: Tạo sự kiện

| Mục | Nội dung |
|---|---|
| Mã FR liên quan | FR-EVT-01 |
| Actor | Thành viên, Trưởng chi |
| Mô tả ngắn | Người dùng có quyền tạo sự kiện nhập thông tin và tạo một sự kiện gia đình. |
| Tiền điều kiện | Người dùng đã đăng nhập và có quyền tạo sự kiện. |
| Luồng chính | 1. Người dùng chọn chức năng tạo sự kiện.<br>2. Người dùng nhập thông tin sự kiện.<br>3. Hệ thống kiểm tra dữ liệu và quyền.<br>4. Hệ thống lưu sự kiện và hiển thị thông tin sự kiện. |
| Luồng thay thế / ngoại lệ | 2a. Thông tin sự kiện không hợp lệ, hệ thống yêu cầu nhập lại.<br>3a. Người dùng không có quyền, hệ thống từ chối yêu cầu. |
| Hậu điều kiện | Sự kiện được tạo và có thể được thành viên được phép xem. |
| Quy tắc nghiệp vụ | BR-07 |

### UC-EVT-02: RSVP

| Mục | Nội dung |
|---|---|
| Mã FR liên quan | FR-EVT-02 |
| Actor | Thành viên |
| Mô tả ngắn | Thành viên chọn trạng thái tham dự một sự kiện. |
| Tiền điều kiện | Người dùng đã đăng nhập, đã được xác minh và có quyền xem sự kiện. |
| Luồng chính | 1. Thành viên mở thông tin sự kiện.<br>2. Thành viên chọn trạng thái Tham dự, Không tham dự hoặc Có thể.<br>3. Hệ thống kiểm tra quyền và trạng thái sự kiện.<br>4. Hệ thống lưu trạng thái RSVP. |
| Luồng thay thế / ngoại lệ | 2a. Thành viên thay đổi lựa chọn, hệ thống cập nhật trạng thái mới.<br>3a. Sự kiện không còn cho phép RSVP, hệ thống thông báo lỗi. |
| Hậu điều kiện | Trạng thái RSVP của thành viên được lưu. |
| Quy tắc nghiệp vụ | BR-08 |

### UC-EVT-03: Quản lý người tham dự

| Mục | Nội dung |
|---|---|
| Mã FR liên quan | FR-EVT-03 |
| Actor | Người tạo sự kiện |
| Mô tả ngắn | Người tạo sự kiện xem và quản lý danh sách người tham dự. |
| Tiền điều kiện | Sự kiện tồn tại và người dùng là người tạo sự kiện. |
| Luồng chính | 1. Người tạo sự kiện mở sự kiện.<br>2. Hệ thống hiển thị danh sách người tham dự và trạng thái RSVP.<br>3. Người tạo sự kiện thực hiện thao tác quản lý được phép.<br>4. Hệ thống kiểm tra quyền và cập nhật thông tin. |
| Luồng thay thế / ngoại lệ | 2a. Không có người tham dự, hệ thống hiển thị danh sách trống.<br>3a. Người dùng không phải người tạo sự kiện, hệ thống từ chối thao tác quản lý. |
| Hậu điều kiện | Danh sách người tham dự được cập nhật theo thao tác hợp lệ. |
| Quy tắc nghiệp vụ | BR-09 |

### UC-EVT-04: Thư viện ảnh sự kiện

| Mục | Nội dung |
|---|---|
| Mã FR liên quan | FR-EVT-04 |
| Actor | Thành viên |
| Mô tả ngắn | Thành viên xem và chia sẻ ảnh thuộc một sự kiện được phép truy cập. |
| Tiền điều kiện | Người dùng đã đăng nhập, đã được xác minh và có quyền xem sự kiện. |
| Luồng chính | 1. Thành viên mở sự kiện.<br>2. Thành viên chọn thư viện ảnh.<br>3. Hệ thống kiểm tra quyền truy cập.<br>4. Hệ thống hiển thị các ảnh thuộc sự kiện. |
| Luồng thay thế / ngoại lệ | 2a. Sự kiện chưa có ảnh, hệ thống hiển thị thư viện trống.<br>3a. Người dùng không có quyền, hệ thống từ chối truy cập. |
| Hậu điều kiện | Thành viên xem được các ảnh sự kiện mà mình có quyền truy cập. |
| Quy tắc nghiệp vụ | BR-10 |

### UC-EVT-05: Nhắc nhở sự kiện

| Mục | Nội dung |
|---|---|
| Mã FR liên quan | FR-EVT-05 |
| Actor | Hệ thống |
| Mô tả ngắn | Hệ thống gửi nhắc nhở cho người dùng liên quan đến sự kiện. |
| Tiền điều kiện | Sự kiện tồn tại và có thông tin thời gian phù hợp để gửi nhắc nhở. |
| Luồng chính | 1. Hệ thống kiểm tra các sự kiện sắp diễn ra.<br>2. Hệ thống xác định người dùng cần nhận nhắc nhở.<br>3. Hệ thống tạo và gửi thông báo nhắc nhở.<br>4. Hệ thống lưu thông tin thông báo. |
| Luồng thay thế / ngoại lệ | 2a. Không có người dùng phù hợp, hệ thống không gửi thông báo.<br>3a. Gửi thông báo thất bại, hệ thống ghi nhận trạng thái lỗi. |
| Hậu điều kiện | Người dùng liên quan nhận được nhắc nhở hoặc hệ thống ghi nhận trạng thái gửi. |
| Quy tắc nghiệp vụ | BR-11 |
## 4. Quy tắc nghiệp vụ

| Mã | Quy tắc |
|---|---|
| BR-01 | Chỉ thành viên đã đăng nhập và được xác minh mới được tạo và tương tác với nội dung trong phạm vi yêu cầu thành viên. |
| BR-02 | Thành viên chỉ được bình luận trên bài đăng mà mình có quyền xem. |
| BR-03 | Thành viên chỉ được thêm, thay đổi hoặc xóa reaction của chính mình. |
| BR-04 | Tin tức gia đình chỉ được hiển thị cho các thành viên có quyền truy cập vào gia đình tương ứng. |
| BR-05 | Ảnh và tệp tải lên phải được kiểm tra trước khi lưu và chỉ được chia sẻ trong phạm vi người dùng có quyền truy cập. |
| BR-06 | Chỉ người dùng có quyền Trưởng chi mới được đăng thông báo theo phạm vi được quản lý. |
| BR-07 | Chỉ người dùng có quyền tạo sự kiện mới được tạo sự kiện. |
| BR-08 | Mỗi thành viên có một trạng thái RSVP cho một sự kiện và có thể cập nhật trạng thái của mình. |
| BR-09 | Chỉ người tạo sự kiện mới được thực hiện các thao tác quản lý người tham dự theo quyền được cấp. |
| BR-10 | Chỉ thành viên có quyền truy cập sự kiện mới được xem thư viện ảnh của sự kiện. |
| BR-11 | Hệ thống chỉ gửi nhắc nhở cho những người dùng liên quan đến sự kiện theo thông tin sự kiện và quyền truy cập. |
| BR-12 | Nội dung bị xóa được xử lý theo cơ chế xóa mềm với trường `deleted_at`. |
## 5. Thiết kế bảng cơ sở dữ liệu của module

> Gợi ý tên bảng ở dưới. Với mỗi bảng, điền cột, kiểu dữ liệu, khóa, ràng buộc. **Gửi cho TV2 (Trí) gộp vào ERD tổng muộn nhất đầu tuần 2.** Quy ước: tên bảng `snake_case` số ít, có `created_at`, `updated_at`, `deleted_at` (xóa mềm).

| Bảng | Mục đích | Trạng thái |
|---|---|---|
| `post` | Bài đăng (có trường loại: POST / NEWS) | Hoàn thành |
| `comment` | Bình luận | Hoàn thành |
| `reaction` | Reaction | Hoàn thành |
| `media` | Ảnh và tệp tải lên | Hoàn thành |
| `announcement` | Thông báo ghim | Hoàn thành |
| `notification` | Thông báo gửi người dùng | Hoàn thành |
| `event` | Sự kiện | Hoàn thành |
| `event_participant` | Người tham dự và trạng thái RSVP | Hoàn thành |

**Bảng `post`**

| Cột | Kiểu | Khóa / Ràng buộc | Mô tả |
|---|---|---|---|
| id | UUID | PK | Mã bài đăng |
| family_id | UUID | NOT NULL | Gia đình sở hữu bài đăng |
| content | TEXT | NOT NULL | Nội dung bài đăng |
| type | VARCHAR(20) | NOT NULL | Loại bài đăng: POST hoặc NEWS |
| created_by | UUID | NOT NULL | Người tạo bài đăng |
| updated_by | UUID | NULL | Người cập nhật gần nhất |
| created_at | TIMESTAMP | NOT NULL | Thời gian tạo |
| updated_at | TIMESTAMP | NOT NULL | Thời gian cập nhật |
| deleted_at | TIMESTAMP | NULL | Xóa mềm |

**Bảng `comment`**

| Cột | Kiểu | Khóa / Ràng buộc | Mô tả |
|---|---|---|---|
| id | UUID | PK | Mã bình luận |
| post_id | UUID | NOT NULL, FK | Bài đăng được bình luận |
| user_id | UUID | NOT NULL, FK | Người bình luận |
| content | TEXT | NOT NULL | Nội dung bình luận |
| created_by | UUID | NOT NULL | Người tạo |
| updated_by | UUID | NULL | Người cập nhật |
| created_at | TIMESTAMP | NOT NULL | Thời gian tạo |
| updated_at | TIMESTAMP | NOT NULL | Thời gian cập nhật |
| deleted_at | TIMESTAMP | NULL | Xóa mềm |

**Bảng `reaction`**

| Cột | Kiểu | Khóa / Ràng buộc | Mô tả |
|---|---|---|---|
| id | UUID | PK | Mã reaction |
| post_id | UUID | NOT NULL, FK | Bài đăng được reaction |
| user_id | UUID | NOT NULL, FK | Người reaction |
| type | VARCHAR(20) | NOT NULL | Loại reaction |
| created_by | UUID | NOT NULL | Người tạo |
| updated_by | UUID | NULL | Người cập nhật |
| created_at | TIMESTAMP | NOT NULL | Thời gian tạo |
| updated_at | TIMESTAMP | NOT NULL | Thời gian cập nhật |
| deleted_at | TIMESTAMP | NULL | Xóa mềm |

**Ràng buộc:** Một người dùng không có nhiều reaction đang hoạt động trên cùng một bài đăng.

**Bảng `media`**

| Cột | Kiểu | Khóa / Ràng buộc | Mô tả |
|---|---|---|---|
| id | UUID | PK | Mã tệp |
| post_id | UUID | NULL, FK | Bài đăng liên quan |
| file_name | VARCHAR(255) | NOT NULL | Tên tệp |
| file_url | TEXT | NOT NULL | Đường dẫn tệp |
| file_type | VARCHAR(100) | NOT NULL | Loại tệp |
| created_by | UUID | NOT NULL | Người tải lên |
| updated_by | UUID | NULL | Người cập nhật |
| created_at | TIMESTAMP | NOT NULL | Thời gian tạo |
| updated_at | TIMESTAMP | NOT NULL | Thời gian cập nhật |
| deleted_at | TIMESTAMP | NULL | Xóa mềm |

**Bảng `announcement`**

| Cột | Kiểu | Khóa / Ràng buộc | Mô tả |
|---|---|---|---|
| id | UUID | PK | Mã thông báo |
| family_id | UUID | NOT NULL | Gia đình hoặc chi được thông báo |
| title | VARCHAR(255) | NOT NULL | Tiêu đề thông báo |
| content | TEXT | NOT NULL | Nội dung thông báo |
| created_by | UUID | NOT NULL | Người tạo |
| updated_by | UUID | NULL | Người cập nhật |
| created_at | TIMESTAMP | NOT NULL | Thời gian tạo |
| updated_at | TIMESTAMP | NOT NULL | Thời gian cập nhật |
| deleted_at | TIMESTAMP | NULL | Xóa mềm |

**Bảng `notification`**

| Cột | Kiểu | Khóa / Ràng buộc | Mô tả |
|---|---|---|---|
| id | UUID | PK | Mã thông báo |
| user_id | UUID | NOT NULL, FK | Người nhận thông báo |
| type | VARCHAR(50) | NOT NULL | Loại thông báo |
| title | VARCHAR(255) | NOT NULL | Tiêu đề |
| content | TEXT | NOT NULL | Nội dung |
| is_read | BOOLEAN | NOT NULL DEFAULT FALSE | Trạng thái đã đọc |
| created_by | UUID | NULL | Nguồn tạo thông báo nếu có |
| updated_by | UUID | NULL | Người cập nhật |
| created_at | TIMESTAMP | NOT NULL | Thời gian tạo |
| updated_at | TIMESTAMP | NOT NULL | Thời gian cập nhật |
| deleted_at | TIMESTAMP | NULL | Xóa mềm |

**Bảng `event`**

| Cột | Kiểu | Khóa / Ràng buộc | Mô tả |
|---|---|---|---|
| id | UUID | PK | Mã sự kiện |
| family_id | UUID | NOT NULL | Gia đình tổ chức |
| title | VARCHAR(255) | NOT NULL | Tên sự kiện |
| description | TEXT | NULL | Mô tả sự kiện |
| start_at | TIMESTAMP | NOT NULL | Thời gian bắt đầu |
| end_at | TIMESTAMP | NULL | Thời gian kết thúc |
| location | VARCHAR(255) | NULL | Địa điểm |
| created_by | UUID | NOT NULL | Người tạo sự kiện |
| updated_by | UUID | NULL | Người cập nhật |
| created_at | TIMESTAMP | NOT NULL | Thời gian tạo |
| updated_at | TIMESTAMP | NOT NULL | Thời gian cập nhật |
| deleted_at | TIMESTAMP | NULL | Xóa mềm |

**Ràng buộc:** Nếu có `end_at` thì thời gian kết thúc phải lớn hơn `start_at`.

**Bảng `event_participant`**

| Cột | Kiểu | Khóa / Ràng buộc | Mô tả |
|---|---|---|---|
| id | UUID | PK | Mã bản ghi tham dự |
| event_id | UUID | NOT NULL, FK | Sự kiện |
| user_id | UUID | NOT NULL, FK | Người tham dự |
| rsvp_status | VARCHAR(20) | NOT NULL | Trạng thái: GOING, NOT_GOING hoặc MAYBE |
| created_by | UUID | NOT NULL | Người tạo bản ghi |
| updated_by | UUID | NULL | Người cập nhật |
| created_at | TIMESTAMP | NOT NULL | Thời gian tạo |
| updated_at | TIMESTAMP | NOT NULL | Thời gian cập nhật |
| deleted_at | TIMESTAMP | NULL | Xóa mềm |

**Ràng buộc:** Một người dùng chỉ có một trạng thái RSVP đang hoạt động cho một sự kiện.
## 6. API của module

> Chi tiết viết trong `../../04-api/openapi/community-events.yaml`. Tóm tắt endpoint ở đây.

| Phương thức | Đường dẫn | Mô tả | Quyền (role) |
|---|---|---|---|
| POST | `/api/v1/posts` | Tạo bài đăng | MEMBER đã xác minh |
| GET | `/api/v1/posts` | Xem danh sách bài đăng | MEMBER đã xác minh |
| GET | `/api/v1/posts/{id}` | Xem chi tiết bài đăng | MEMBER có quyền xem |
| PATCH | `/api/v1/posts/{id}` | Sửa bài đăng | Người tạo hoặc người có quyền |
| DELETE | `/api/v1/posts/{id}` | Xóa bài đăng | Người tạo hoặc người có quyền |
| POST | `/api/v1/posts/{id}/comments` | Thêm bình luận | MEMBER đã xác minh |
| POST | `/api/v1/posts/{id}/reactions` | Thêm hoặc thay đổi reaction | MEMBER đã xác minh |
| DELETE | `/api/v1/posts/{id}/reactions` | Xóa reaction | MEMBER đã xác minh |
| POST | `/api/v1/posts/{id}/media` | Tải ảnh hoặc tệp lên bài đăng | MEMBER đã xác minh |
| POST | `/api/v1/announcements` | Tạo thông báo | BRANCH_ADMIN |
| GET | `/api/v1/announcements` | Xem danh sách thông báo | MEMBER có quyền xem |
| POST | `/api/v1/events` | Tạo sự kiện | MEMBER có quyền tạo sự kiện |
| GET | `/api/v1/events` | Xem danh sách sự kiện | MEMBER đã xác minh |
| GET | `/api/v1/events/{id}` | Xem chi tiết sự kiện | MEMBER có quyền xem |
| PATCH | `/api/v1/events/{id}` | Cập nhật sự kiện | Người tạo hoặc người có quyền |
| DELETE | `/api/v1/events/{id}` | Xóa sự kiện | Người tạo hoặc người có quyền |
| PUT | `/api/v1/events/{id}/rsvp` | Cập nhật trạng thái RSVP | MEMBER đã xác minh |
| GET | `/api/v1/events/{id}/participants` | Xem danh sách người tham dự | Người tạo sự kiện hoặc người có quyền |
| GET | `/api/v1/events/{id}/media` | Xem thư viện ảnh sự kiện | MEMBER có quyền xem |
## 7. Màn hình liên quan

Màn hình và wireframe của module Community và Events được quản lý trong tài liệu UI chung của nhóm:

`docs/05-ui/figma-links.md`

Link Figma và ảnh wireframe sẽ được bổ sung theo tiến độ thiết kế UI của nhóm.
## 8. Câu hỏi còn mở
- [ ] TV2 xác nhận thiết kế các bảng Community và Events để gộp vào ERD tổng.
- [ ] TV1 review tài liệu và góp ý nếu cần.
- [ ] Thống nhất cách liên kết ảnh của sự kiện với `media`.
