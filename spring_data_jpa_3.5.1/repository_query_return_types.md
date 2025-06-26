#### **1. Kiểu Trả Về Đơn (Single Entity)**

| Kiểu Trả Về    | Mô tả                                                                                                                                                                                                                                      | Ví dụ                                       |
| :------------- | :----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | :------------------------------------------ |
| `T` (Thực thể) | Trả về một thực thể duy nhất. Nếu không tìm thấy kết quả nào, sẽ ném ra `EmptyResultDataAccessException`. Nếu tìm thấy nhiều hơn một kết quả, sẽ ném ra `IncorrectResultSizeDataAccessException`.                                          | `User findByUsername(String username);`     |
| `Optional<T>`  | Đây là cách được khuyến khích để xử lý các truy vấn có thể không trả về kết quả. Trả về một `Optional` chứa thực thể nếu tìm thấy, hoặc một `Optional.empty()` nếu không tìm thấy. Giúp tránh lỗi `NullPointerException` một cách tao nhã. | `Optional<User> findByEmail(String email);` |

-----

#### **2. Kiểu Trả Về Tập Hợp (Collections)**

| Kiểu Trả Về     | Mô tả                                                                                                                                          | Ví dụ                                                   |
| :-------------- | :--------------------------------------------------------------------------------------------------------------------------------------------- | :------------------------------------------------------ |
| `Iterable<T>`   | Kiểu trả về cơ bản nhất cho nhiều kết quả.                                                                                                     | `Iterable<User> findAllByRole(String role);`            |
| `Collection<T>` | Tương tự `Iterable`, là một kiểu tập hợp chung.                                                                                                | `Collection<User> findAllByLastLoginBefore(Date date);` |
| `List<T>`       | Một trong những kiểu phổ biến nhất. Trả về một danh sách các thực thể. Nếu không có kết quả, sẽ trả về một danh sách rỗng (không phải `null`). | `List<User> findByLastname(String lastname);`           |
| `Set<T>`        | Tương tự `List`, nhưng đảm bảo các phần tử trong tập hợp là duy nhất.                                                                          | `Set<User> findDistinctByLastname(String lastname);`    |

-----

#### **3. Kiểu Trả Về Nâng Cao (Advanced Types)**

| Kiểu Trả Về | Mô tả                                                                                                                                                                                                                                                                                               | Ví dụ                                                                                                                                         |
| :---------- | :-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | :-------------------------------------------------------------------------------------------------------------------------------------------- |
| `Stream<T>` | Trả về một `Stream` của Java 8, cho phép xử lý dữ liệu lớn một cách hiệu quả (lazy loading). **Quan trọng:** Phải được sử dụng bên trong một giao dịch (`@Transactional`) và phải được đóng lại sau khi sử dụng (ví dụ: dùng `try-with-resources`) để tránh rò rỉ tài nguyên kết nối cơ sở dữ liệu. | `@Transactional(readOnly = true)`<br>`try (Stream<User> stream = repository.findAllByStatus("ACTIVE")) {`<br>`   stream.forEach(...);`<br>`}` |
| `Slice<T>`  | Thường dùng cho phân trang. Một "lát cắt" của dữ liệu. Nó biết được có "lát cắt" tiếp theo hay không (`hasNext()`), nhưng không biết tổng số lượng phần tử. Hữu ích khi bạn chỉ cần duyệt qua các trang mà không cần hiển thị tổng số trang.                                                        | `Slice<User> findByLastname(String lastname, Pageable pageable);`                                                                             |
| `Page<T>`   | Kiểu trả về mạnh mẽ nhất cho phân trang. Kế thừa từ `Slice`, nhưng nó cung cấp thêm thông tin về tổng số lượng phần tử (`getTotalElements()`) và tổng số trang (`getTotalPages()`). Điều này đòi hỏi một truy vấn `COUNT` bổ sung, có thể ảnh hưởng đến hiệu năng một chút.                         | `Page<User> findByLastname(String lastname, Pageable pageable);`                                                                              |

-----

#### **4. Kiểu Trả Về Bất Đồng Bộ (Asynchronous)**

| Kiểu Trả Về            | Mô tả                                                                                                                                                                                       | Ví dụ                                                                       |
| :--------------------- | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | :-------------------------------------------------------------------------- |
| `Future<T>`            | Trả về một đối tượng `Future` đại diện cho kết quả của một phép tính bất đồng bộ. Truy vấn sẽ chạy trên một luồng khác. Để sử dụng, cần kích hoạt tính năng bất đồng bộ với `@EnableAsync`. | `@Async`<br>`Future<User> findByUsername(String username);`                 |
| `CompletableFuture<T>` | Một phiên bản `Future` mạnh mẽ và linh hoạt hơn, cho phép kết hợp các tác vụ bất đồng bộ.                                                                                                   | `@Async`<br>`CompletableFuture<List<User>> findAllByStatus(String status);` |
| `ListenableFuture<T>`  | (Spring-specific) Một `Future` cho phép đăng ký các callback sẽ được gọi khi tác vụ hoàn thành (thành công hoặc thất bại).                                                                  | `@Async`<br>`ListenableFuture<User> findOneByFirstname(String firstname);`  |

-----

#### **5. Projections (Hình Chiếu)**

Đôi khi bạn không cần toàn bộ thực thể mà chỉ cần một vài thuộc tính. Projections giúp bạn chỉ truy vấn các cột cần thiết, cải thiện hiệu năng.

| Kiểu Trả Về            | Mô tả                                                                                                                                                         | Ví dụ                                                                                                                                                                                                |
| :--------------------- | :------------------------------------------------------------------------------------------------------------------------------------------------------------ | :--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Interface-based**    | Định nghĩa một `interface` chỉ chứa các phương thức getter cho các thuộc tính bạn muốn lấy. Spring Data sẽ tự động tạo một proxy triển khai interface này.    | `interface NamesOnly {`<br>`   String getFirstname();`<br>`   String getLastname();`<br>`}`<br><br>`NamesOnly findByEmail(String email);`                                                            |
| **Class-based (DTO)**  | Tạo một lớp (DTO - Data Transfer Object) có constructor chứa các thuộc tính tương ứng. Tên tham số của constructor phải khớp với tên thuộc tính của thực thể. | `class UserSummary {`<br>`   private final String firstname;`<br>`   private final String lastname;`<br>`   // Constructor, getters...`<br>`}`<br><br>`UserSummary findByUsername(String username);` |
| **Dynamic Projection** | Cho phép chọn kiểu trả về (thực thể đầy đủ hoặc projection) một cách linh động tại thời điểm gọi phương thức.                                                 | `<T> T findByUsername(String username, Class<T> type);`<br><br>// Gọi:<br>`repository.findByUsername("dave", NamesOnly.class);`<br>`repository.findByUsername("dave", User.class);`                  |