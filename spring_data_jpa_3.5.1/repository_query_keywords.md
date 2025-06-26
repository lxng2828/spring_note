### **Các từ khóa chính để bắt đầu phương thức truy vấn**

| Từ khóa                      | Mô tả                                                                     | Ví dụ                               |
| ---------------------------- | ------------------------------------------------------------------------- | ----------------------------------- |
| `find...By`                  | Tìm kiếm một hoặc nhiều thực thể.                                         | `findByName(String name)`           |
| `read...By`                  | Tương tự `find...By`, nhưng thường được sử dụng cho các truy vấn chỉ đọc. | `readByEmail(String email)`         |
| `get...By`                   | Tương tự `find...By`, một cách khác để biểu thị việc lấy dữ liệu.         | `getByAge(int age)`                 |
| `query...By`                 | Một từ khóa chung khác để bắt đầu một phương thức truy vấn.               | `queryByAddress(String address)`    |
| `count...By`                 | Đếm số lượng thực thể phù hợp với điều kiện.                              | `countByStatus(String status)`      |
| `exists...By`                | Kiểm tra xem có thực thể nào phù hợp với điều kiện không.                 | `existsByUsername(String username)` |
| `delete...By`, `remove...By` | Xóa các thực thể phù hợp với điều kiện.                                   | `deleteByEmail(String email)`       |

### **Các từ khóa điều kiện (Predicate Keywords)**

| Từ khóa                | Biểu thức tương đương     | Ví dụ                                                           |
| ---------------------- | ------------------------- | --------------------------------------------------------------- |
| `And`                  | `... AND ...`             | `findByLastnameAndFirstname(String lastname, String firstname)` |
| `Or`                   | `... OR ...`              | `findByLastnameOrFirstname(String lastname, String firstname)`  |
| `Is`, `Equals`         | `= (bằng)`                | `findByFirstnameIs(String firstname)`                           |
| `Between`              | `... BETWEEN ...`         | `findByStartDateBetween(Date start, Date end)`                  |
| `LessThan`             | `<`                       | `findByAgeLessThan(int age)`                                    |
| `LessThanEqual`        | `<=`                      | `findByAgeLessThanEqual(int age)`                               |
| `GreaterThan`          | `>`                       | `findByAgeGreaterThan(int age)`                                 |
| `GreaterThanEqual`     | `>=`                      | `findByAgeGreaterThanEqual(int age)`                            |
| `After`, `IsAfter`     | `> (cho ngày tháng)`      | `findByStartDateAfter(Date date)`                               |
| `Before`, `IsBefore`   | `< (cho ngày tháng)`      | `findByStartDateBefore(Date date)`                              |
| `IsNull`, `Null`       | `IS NULL`                 | `findByAgeIsNull()`                                             |
| `IsNotNull`, `NotNull` | `IS NOT NULL`             | `findByAgeIsNotNull()`                                          |
| `Like`                 | `LIKE`                    | `findByFirstnameLike(String firstname)`                         |
| `NotLike`              | `NOT LIKE`                | `findByFirstnameNotLike(String firstname)`                      |
| `StartingWith`         | `LIKE '...%'`             | `findByFirstnameStartingWith(String prefix)`                    |
| `EndingWith`           | `LIKE '%...'`             | `findByFirstnameEndingWith(String suffix)`                      |
| `Containing`           | `LIKE '%...%'`            | `findByFirstnameContaining(String infix)`                       |
| `OrderBy...Asc/Desc`   | `ORDER BY ... ASC/DESC`   | `findByAgeOrderByLastnameDesc(int age)`                         |
| `Not`                  | `<>`                      | `findByLastnameNot(String lastname)`                            |
| `In`                   | `IN`                      | `findByAgeIn(Collection<Integer> ages)`                         |
| `NotIn`                | `NOT IN`                  | `findByAgeNotIn(Collection<Integer> ages)`                      |
| `True`                 | `= true`                  | `findByActiveTrue()`                                            |
| `False`                | `= false`                 | `findByActiveFalse()`                                           |
| `IgnoreCase`           | `UPPER(...) = UPPER(...)` | `findByFirstnameIgnoreCase(String firstname)`                   |

### **Các từ khóa đặc biệt khác**

| Từ khóa                        | Mô tả                                          | Ví dụ                                     |
| ------------------------------ | ---------------------------------------------- | ----------------------------------------- |
| `Distinct`                     | Trả về các kết quả duy nhất (không trùng lặp). | `findDistinctByLastname(String lastname)` |
| `First<number>`, `Top<number>` | Giới hạn số lượng kết quả trả về.              | `findFirst10ByLastname(String lastname)`  |

### **Lưu ý quan trọng**

* **Quy ước đặt tên:** Tên phương thức phải tuân theo quy ước camelCase (ví dụ: `findByLastnameAndFirstname`).
* **Tham số phương thức:** Các tham số của phương thức phải khớp với các thuộc tính của thực thể (entity).
* **Annotation `@Query`:** Đối với các truy vấn phức tạp hơn mà không thể biểu diễn bằng các từ khóa trên, bạn có thể sử dụng annotation `@Query` để viết các câu truy vấn JPQL hoặc SQL thuần.
* **Phân trang và sắp xếp:** Spring Data JPA cũng hỗ trợ phân trang (`Pageable`) và sắp xếp (`Sort`) một cách dễ dàng.