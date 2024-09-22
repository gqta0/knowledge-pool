## 1. Câu hỏi về SQL cơ bản:
### Câu lệnh `SELECT` hoạt động như thế nào? Bạn có thể giải thích các toán tử WHERE, `ORDER BY`, `GROUP BY`, và `HAVING` không?

Câu lệnh `SELECT` được sử dụng để truy vấn dữ liệu từ một hoặc nhiều bảng trong cơ sở dữ liệu. Nó hoạt động bằng cách chỉ định các cột bạn muốn lấy dữ liệu và từ bảng nào.

Các toán tử khác có vai trò như sau:

* `WHERE`: Dùng để lọc các dòng dữ liệu dựa trên một điều kiện cụ thể. Chỉ những dòng thỏa mãn điều kiện mới được trả về.
* `ORDER BY`: Sắp xếp kết quả truy vấn theo thứ tự tăng dần hoặc giảm dần dựa trên một hoặc nhiều cột.
* `GROUP BY`: Gom nhóm các dòng có giá trị giống nhau ở các cột được chỉ định, thường được sử dụng cùng với các hàm tổng hợp (như `SUM`, `COUNT`, `AVG`) để tính toán trên từng nhóm.
* `HAVING`: Lọc các nhóm dữ liệu sau khi đã được gom nhóm bởi `GROUP BY`, tương tự như `WHERE` nhưng áp dụng cho các nhóm thay vì từng dòng.

``` sql 

-- Ví dụ sử dụng SELECT với các toán tử
SELECT TenNhanVien, Luong
FROM NhanVien
WHERE Luong > 50000
ORDER BY Luong DESC;

SELECT PhongBan, COUNT(*) AS SoNhanVien
FROM NhanVien
GROUP BY PhongBan
HAVING COUNT(*) > 10;

```

### Sự khác biệt giữa `INNER JOIN`, `LEFT JOIN`, `RIGHT JOIN`, và `FULL OUTER JOIN` là gì? Hãy cho ví dụ.
Các toán tử `JOIN` được sử dụng để kết hợp dữ liệu từ hai hoặc nhiều bảng dựa trên một điều kiện liên kết (thường là khóa ngoại).

* `INNER JOIN`: Chỉ trả về các dòng dữ liệu có sự khớp giữa cả hai bảng.
* `LEFT JOIN`: Trả về tất cả các dòng từ bảng bên trái và các dòng khớp từ bảng bên phải. Nếu không có sự khớp, các cột từ bảng bên phải sẽ có giá trị `NULL`.
* `RIGHT JOIN`: Tương tự như `LEFT JOIN`, nhưng trả về tất cả các dòng từ bảng bên phải.
* `FULL OUTER JOIN`: Trả về tất cả các dòng từ cả hai bảng. Nếu không có sự khớp, các cột từ bảng còn lại sẽ có giá trị `NULL`.

``` sql
    -- Ví dụ: Giả sử có hai bảng `customers` và `orders`
    -- INNER JOIN: Lấy danh sách khách hàng đã đặt hàng
    SELECT c.customer_name, o.order_id
    FROM customers c
    INNER JOIN orders o ON c.customer_id = o.customer_id;

    -- LEFT JOIN: Lấy danh sách tất cả khách hàng và các đơn hàng của họ (nếu có)
    SELECT c.customer_name, o.order_id
    FROM customers c
    LEFT JOIN orders o ON c.customer_id = o.customer_id;
``` 

### Làm thế nào để viết một truy vấn con (subquery) và khi nào bạn nên sử dụng nó?

Truy vấn con là một truy vấn được lồng bên trong một truy vấn khác. Nó có thể được sử dụng trong mệnh đề `WHERE`, `HAVING`, `FROM`, hoặc thậm chí trong câu lệnh `SELECT`. Bạn nên sử dụng truy vấn con khi cần lọc hoặc tính toán dữ liệu dựa trên kết quả của một truy vấn khác.

``` sql
    -- Ví dụ: Lấy danh sách các sản phẩm có giá cao nhất
    SELECT product_name, price
    FROM products
    WHERE price = (SELECT MAX(price) FROM products);
```

### Bạn có thể giải thích về `CTE` (Common Table Expressions) và lợi ích của nó so với subqueries?

`CTE` là một cấu trúc tạm thời đại diện cho một tập kết quả, có thể được tham chiếu nhiều lần trong một truy vấn. Lợi ích của `CTE` so với subqueries bao gồm:

* Cải thiện khả năng đọc và bảo trì của truy vấn, đặc biệt là đối với các truy vấn phức tạp.
* Cho phép tái sử dụng cùng một tập kết quả nhiều lần trong truy vấn.
* Có thể đệ quy, cho phép thực hiện các truy vấn trên các cấu trúc dữ liệu phân cấp.

``` sql
    -- Ví dụ: Tính tổng doanh thu cho mỗi khách hàng, sử dụng CTE
    WITH customer_totals AS (
      SELECT customer_id, SUM(order_total) AS total_revenue
      FROM orders
      GROUP BY customer_id
    )
    SELECT c.customer_name, ct.total_revenue
    FROM customers c
    JOIN customer_totals ct ON c.customer_id = ct.customer_id;
```

### Khi nào bạn nên sử dụng `UNION` thay vì `JOIN`? Hãy giải thích sự khác nhau giữa hai phương pháp này.

`UNION` được sử dụng để kết hợp các tập kết quả từ hai hoặc nhiều câu lệnh `SELECT` thành một tập kết quả duy nhất. `JOIN` được sử dụng để kết hợp các dòng dữ liệu từ hai hoặc nhiều bảng dựa trên một điều kiện liên kết.

Bạn nên sử dụng `UNION` khi:

* Muốn kết hợp dữ liệu từ các bảng có cấu trúc tương tự nhưng không có mối quan hệ trực tiếp.
* Muốn loại bỏ các dòng trùng lặp trong tập kết quả cuối cùng (`UNION` sẽ tự động loại bỏ các dòng trùng lặp, trong khi `UNION ALL` sẽ giữ lại tất cả các dòng).

``` sql
    -- Ví dụ: Kết hợp danh sách khách hàng từ hai bảng khác nhau
    SELECT customer_name FROM customers1
    UNION
    SELECT customer_name FROM customers2;
```

## 2. Câu hỏi về Indexes và tối ưu hóa

### Index là gì? Làm thế nào để index cải thiện hiệu suất truy vấn?

Index (chỉ mục) là một cấu trúc dữ liệu được tạo ra để tăng tốc độ truy vấn bằng cách tạo ra một con đường nhanh chóng đến dữ liệu cần tìm kiếm. Nó hoạt động tương tự như mục lục của một cuốn sách, giúp bạn tìm kiếm thông tin nhanh chóng thay vì phải đọc toàn bộ cuốn sách.

Index cải thiện hiệu suất truy vấn bằng cách:

* Giảm số lượng hàng cần quét: Thay vì phải quét toàn bộ bảng để tìm kiếm dữ liệu, index cho phép hệ thống quản trị cơ sở dữ liệu (DBMS) chỉ cần quét một phần nhỏ của bảng.
* Sắp xếp dữ liệu: Index thường sắp xếp dữ liệu theo một thứ tự cụ thể, giúp DBMS nhanh chóng định vị dữ liệu cần tìm.
* Lưu trữ dữ liệu trực tiếp: Một số loại index (như clustered index) lưu trữ trực tiếp dữ liệu, giúp DBMS truy cập dữ liệu ngay lập tức mà không cần phải truy cập vào bảng gốc.

### Bạn có thể giải thích sự khác biệt giữa clustered index và non-clustered index?

| Đặc điểm | Clustered Index | Non-clustered Index |
|---|---|---|
| Sắp xếp dữ liệu | Sắp xếp lại dữ liệu vật lý trên đĩa theo thứ tự của cột được đánh index. | Không sắp xếp lại dữ liệu vật lý, chỉ tạo một cấu trúc riêng biệt chứa các con trỏ trỏ đến vị trí dữ liệu trên đĩa. |
| Số lượng | Chỉ có thể có một clustered index trên mỗi bảng. | Có thể có nhiều non-clustered index trên mỗi bảng. |
| Tốc độ truy vấn | Nhanh hơn cho các truy vấn tìm kiếm dựa trên cột được đánh index, đặc biệt là các truy vấn tìm kiếm phạm vi. | Nhanh hơn cho các truy vấn tìm kiếm dựa trên cột được đánh index, nhưng có thể chậm hơn clustered index cho các truy vấn tìm kiếm phạm vi. |
| Tốc độ cập nhật dữ liệu | Chậm hơn do phải sắp xếp lại dữ liệu vật lý khi có cập nhật. | Nhanh hơn do chỉ cần cập nhật cấu trúc index. |

### Khi nào thì việc tạo index trên một cột có thể gây ra tác động tiêu cực đến hiệu suất?

Việc tạo index trên một cột có thể gây ra tác động tiêu cực đến hiệu suất trong các trường hợp sau:

* Bảng có kích thước nhỏ: Đối với các bảng có kích thước nhỏ, việc quét toàn bộ bảng có thể nhanh hơn so với việc sử dụng index.
* Cột thường xuyên được cập nhật: Việc cập nhật dữ liệu trong cột được đánh index sẽ làm chậm quá trình cập nhật do phải cập nhật lại cấu trúc index.
* Cột có nhiều giá trị trùng lặp: Index trên các cột có nhiều giá trị trùng lặp sẽ không hiệu quả vì DBMS vẫn phải quét nhiều hàng để tìm kiếm dữ liệu.
* Truy vấn hiếm khi được sử dụng: Nếu một truy vấn hiếm khi được sử dụng, việc tạo index cho truy vấn đó có thể không mang lại lợi ích đáng kể.

### Bạn sẽ tối ưu hóa một câu truy vấn chậm như thế nào? Bạn có thể mô tả quá trình phân tích execution plan không?

**Quá trình tối ưu hóa một câu truy vấn chậm:**

1. Xác định câu truy vấn chậm: Sử dụng các công cụ giám sát hiệu suất để xác định câu truy vấn đang gây ra sự chậm trễ.
2. Phân tích execution plan: Sử dụng câu lệnh `EXPLAIN` hoặc các công cụ trực quan để xem cách DBMS thực thi câu truy vấn. Execution plan sẽ cho biết các bước DBMS thực hiện để trả về kết quả, bao gồm các bảng được truy cập, các index được sử dụng, các phép nối được thực hiện, và ước tính chi phí của từng bước.
3. Tìm kiếm các vấn đề: Dựa trên execution plan, tìm kiếm các vấn đề có thể gây ra sự chậm trễ, chẳng hạn như:
    * Thiếu index: Nếu execution plan cho thấy DBMS đang thực hiện quét toàn bộ bảng (full table scan) thay vì sử dụng index, có thể cần tạo index cho các cột được sử dụng trong điều kiện `WHERE`.
    * Index không hiệu quả: Nếu execution plan cho thấy DBMS đang sử dụng index nhưng vẫn phải quét nhiều hàng, có thể cần xem xét lại cấu trúc index hoặc tạo index mới.
    * Phép nối tốn kém: Nếu execution plan cho thấy các phép nối đang tiêu tốn nhiều thời gian, có thể cần xem xét lại cách viết câu truy vấn hoặc tạo index cho các cột được sử dụng trong phép nối.
4. Thực hiện các thay đổi: Dựa trên kết quả phân tích, thực hiện các thay đổi để cải thiện hiệu suất truy vấn, chẳng hạn như:
    * Tạo index mới.
    * Chỉnh sửa cấu trúc index hiện có.
    * Viết lại câu truy vấn.
    * Cập nhật thống kê bảng.
5. Kiểm tra lại hiệu suất: Sau khi thực hiện các thay đổi, kiểm tra lại hiệu suất truy vấn để đảm bảo rằng nó đã được cải thiện.

**Mô tả quá trình phân tích execution plan:**

1. Chạy câu lệnh `EXPLAIN`: Chạy câu lệnh `EXPLAIN` trước câu truy vấn cần phân tích. Kết quả sẽ là một execution plan dạng văn bản hoặc đồ họa, tùy thuộc vào DBMS và công cụ được sử dụng.
2. Đọc execution plan từ dưới lên trên: Execution plan thường được hiển thị dưới dạng cây, với các bước thực hiện được sắp xếp từ dưới lên trên. Bắt đầu phân tích từ bước cuối cùng (trả về kết quả) và lần ngược lên trên để hiểu cách DBMS thực thi câu truy vấn.
3. Chú ý các thông tin quan trọng: Chú ý các thông tin quan trọng trong execution plan, chẳng hạn như:
    * Các bảng được truy cập: Xác định các bảng được truy cập trong câu truy vấn.
    * Các index được sử dụng: Xác định các index được sử dụng để truy cập dữ liệu.
    * Các phép nối được thực hiện: Xác định các phép nối được thực hiện và loại phép nối (nested loop, hash join, merge join).
    * Ước tính chi phí: Chú ý ước tính chi phí của từng bước để xác định các bước tốn kém nhất.
4. Tìm kiếm các vấn đề: Dựa trên các thông tin trên, tìm kiếm các vấn đề có thể gây ra sự chậm trễ, chẳng hạn như quét toàn bộ bảng, index không hiệu quả, hoặc phép nối tốn kém.

```sql
-- Ví dụ về câu lệnh EXPLAIN
EXPLAIN SELECT * FROM customers WHERE customer_id = 12345;
```

## 3. Câu hỏi về Transactions và Isolation Levels:
### Bạn có thể giải thích khái niệm ACID trong cơ sở dữ liệu không?

ACID là một tập hợp các tính chất đảm bảo tính toàn vẹn và nhất quán của dữ liệu trong cơ sở dữ liệu, đặc biệt là trong môi trường đa người dùng và đa tác vụ. ACID là viết tắt của:

* **Atomicity (Tính nguyên tử):** Một transaction được coi là một đơn vị công việc không thể chia nhỏ. Hoặc toàn bộ transaction được thực hiện thành công, hoặc không có thay đổi nào được áp dụng lên cơ sở dữ liệu.
* **Consistency (Tính nhất quán):** Một transaction phải đưa cơ sở dữ liệu từ một trạng thái nhất quán sang một trạng thái nhất quán khác. Điều này có nghĩa là transaction phải tuân thủ tất cả các ràng buộc, quy tắc và điều kiện được định nghĩa trong cơ sở dữ liệu.
* **Isolation (Tính cô lập):** Các transaction đồng thời phải được thực hiện như thể chúng đang chạy tuần tự, độc lập với nhau. Điều này ngăn chặn các vấn đề như đọc dữ liệu bẩn (dirty read), đọc không lặp lại (non-repeatable read) và đọc ảo (phantom read).
* **Durability (Tính bền vững):** Khi một transaction đã được commit, các thay đổi của nó phải được lưu trữ vĩnh viễn trong cơ sở dữ liệu, ngay cả khi xảy ra lỗi hệ thống hoặc mất điện.

### Cấp độ cô lập (isolation level) là gì? Sự khác nhau giữa các cấp độ như Read Uncommitted, Read Committed, Repeatable Read, và Serializable là gì?

Cấp độ cô lập xác định mức độ mà một transaction có thể nhìn thấy các thay đổi do các transaction khác thực hiện đồng thời. Các cấp độ cô lập khác nhau cung cấp sự cân bằng giữa tính nhất quán của dữ liệu và hiệu suất của hệ thống.

* **Read Uncommitted:** Mức cô lập thấp nhất. Cho phép một transaction đọc các thay đổi chưa được commit của các transaction khác. Điều này có thể dẫn đến đọc dữ liệu bẩn, nhưng cung cấp hiệu suất cao nhất.
* **Read Committed:** Ngăn chặn đọc dữ liệu bẩn bằng cách chỉ cho phép một transaction đọc các thay đổi đã được commit của các transaction khác. Tuy nhiên, vẫn có thể xảy ra đọc không lặp lại, tức là một transaction có thể đọc cùng một dữ liệu nhiều lần và nhận được kết quả khác nhau.
* **Repeatable Read:** Ngăn chặn đọc không lặp lại bằng cách đảm bảo rằng một transaction sẽ luôn đọc cùng một dữ liệu trong suốt quá trình thực thi của nó, ngay cả khi dữ liệu đó đã được thay đổi bởi các transaction khác và đã được commit. Tuy nhiên, vẫn có thể xảy ra đọc ảo, tức là một transaction có thể thấy các dòng mới được thêm vào bởi các transaction khác.
* **Serializable:** Mức cô lập cao nhất. Ngăn chặn tất cả các vấn đề về đồng thời, bao gồm đọc dữ liệu bẩn, đọc không lặp lại và đọc ảo. Điều này đạt được bằng cách sử dụng khóa hoặc các kỹ thuật khác để đảm bảo rằng các transaction thực thi tuần tự một cách logic. Tuy nhiên, Serializable có thể ảnh hưởng đến hiệu suất do tăng khả năng xung đột giữa các transaction.

### Khi nào bạn sử dụng `ROLLBACK` trong một transaction? Hãy cho ví dụ về cách sử dụng nó.

`ROLLBACK` được sử dụng để hoàn tác tất cả các thay đổi đã được thực hiện trong một transaction. Bạn nên sử dụng `ROLLBACK` khi:

* Xảy ra lỗi trong quá trình thực thi transaction và bạn muốn đảm bảo tính nhất quán của dữ liệu.
* Bạn muốn hủy bỏ các thay đổi đã thực hiện trong transaction vì một lý do nào đó.

```sql
-- Ví dụ về cách sử dụng ROLLBACK
BEGIN TRANSACTION;

UPDATE SanPham SET Gia = Gia * 1.1; -- Tăng giá sản phẩm 10%

-- Nếu xảy ra lỗi hoặc bạn muốn hủy thay đổi
IF @@ERROR <> 0 
BEGIN
    ROLLBACK TRANSACTION;
    PRINT 'Đã xảy ra lỗi, transaction đã được rollback.';
END
ELSE
BEGIN
    COMMIT TRANSACTION;
    PRINT 'Transaction đã được commit thành công.';
END

```

## 4. Câu hỏi về Normalization và Denormalization

### Bạn có thể giải thích các mức chuẩn hóa (normal forms) từ 1NF đến 3NF không? Mục tiêu của chuẩn hóa là gì?

Chắc chắn rồi, hãy cùng tìm hiểu về các mức chuẩn hóa cơ bản và mục tiêu của chúng:

**Các mức chuẩn hóa (Normal Forms)**

* **1NF (First Normal Form - Dạng chuẩn 1):**
    * Mỗi cột trong bảng chỉ chứa một giá trị nguyên tử (atomic value), không có các nhóm giá trị lặp lại hoặc các tập hợp.
    * Bảng phải có một khóa chính (primary key) để định danh duy nhất từng hàng.

* **2NF (Second Normal Form - Dạng chuẩn 2):**
    * Bảng phải ở dạng 1NF.
    * Tất cả các thuộc tính không khóa (non-key attributes) phải phụ thuộc đầy đủ vào toàn bộ khóa chính, không chỉ một phần của khóa chính.

* **3NF (Third Normal Form - Dạng chuẩn 3):**
    * Bảng phải ở dạng 2NF.
    * Không có thuộc tính không khóa nào phụ thuộc bắc cầu (transitively dependent) vào khóa chính. Tức là, không có trường hợp một thuộc tính không khóa phụ thuộc vào một thuộc tính không khóa khác, mà thuộc tính không khóa đó lại phụ thuộc vào khóa chính.

**Mục tiêu của chuẩn hóa**

* **Giảm thiểu dư thừa dữ liệu:** Chuẩn hóa giúp loại bỏ các thông tin lặp lại không cần thiết, tiết kiệm không gian lưu trữ và giảm khả năng xảy ra lỗi không nhất quán dữ liệu.
* **Đảm bảo tính toàn vẹn dữ liệu:** Bằng cách tổ chức dữ liệu một cách hợp lý, chuẩn hóa giúp duy trì tính chính xác và nhất quán của dữ liệu, đặc biệt là trong các thao tác cập nhật, xóa và thêm dữ liệu.
* **Cải thiện hiệu suất truy vấn:** Mặc dù có thể dẫn đến việc phải sử dụng nhiều bảng hơn trong các truy vấn, nhưng chuẩn hóa thường giúp cải thiện hiệu suất truy vấn bằng cách giảm thiểu lượng dữ liệu cần truy cập và xử lý.
* **Tăng tính linh hoạt của cơ sở dữ liệu:** Một cơ sở dữ liệu được chuẩn hóa tốt sẽ dễ dàng thích ứng với các thay đổi trong yêu cầu nghiệp vụ mà không cần phải tái cấu trúc toàn bộ cơ sở dữ liệu.

### Trong tình huống nào bạn có thể sử dụng denormalization và tại sao?

**Denormalization** là quá trình cố ý đưa một số dư thừa dữ liệu trở lại vào cơ sở dữ liệu, thường bằng cách kết hợp các bảng hoặc thêm các cột tính toán. Mặc dù đi ngược lại các nguyên tắc của chuẩn hóa, denormalization có thể được sử dụng trong một số tình huống cụ thể để cải thiện hiệu suất truy vấn.

Bạn có thể xem xét sử dụng denormalization khi:

* **Truy vấn thường xuyên yêu cầu kết hợp nhiều bảng:** Nếu một truy vấn phức tạp liên quan đến nhiều bảng và các phép nối (join) tốn nhiều thời gian, việc denormalization bằng cách kết hợp một số bảng có thể giúp giảm thời gian truy vấn.
* **Cần tính toán các giá trị tổng hợp thường xuyên:** Nếu bạn thường xuyên cần tính toán các giá trị tổng hợp (như tổng, trung bình, số lượng) trên một tập dữ liệu lớn, việc thêm các cột tính toán vào bảng có thể giúp tăng tốc độ truy vấn.
* **Yêu cầu về hiệu suất đọc cao hơn hiệu suất ghi:** Trong các hệ thống mà việc đọc dữ liệu diễn ra thường xuyên hơn nhiều so với việc ghi dữ liệu, denormalization có thể chấp nhận được để cải thiện hiệu suất đọc, ngay cả khi nó làm giảm hiệu suất ghi một chút.

Tuy nhiên, cần lưu ý rằng denormalization cũng có những nhược điểm:

* **Tăng dư thừa dữ liệu:** Denormalization làm tăng khả năng xảy ra lỗi không nhất quán dữ liệu và làm phức tạp việc cập nhật dữ liệu.
* **Tốn thêm không gian lưu trữ:** Dữ liệu dư thừa chiếm thêm không gian lưu trữ.

Vì vậy, quyết định sử dụng denormalization cần được cân nhắc kỹ lưỡng, đánh giá giữa lợi ích về hiệu suất và các nhược điểm tiềm ẩn.

### Hãy giải thích sự khác nhau giữa một mô hình dữ liệu được chuẩn hóa hoàn toàn và một mô hình dữ liệu đã denormalized.

* **Mô hình dữ liệu được chuẩn hóa hoàn toàn:**
    * Tuân thủ các quy tắc chuẩn hóa, thường là đến 3NF hoặc cao hơn.
    * Tối thiểu hóa dư thừa dữ liệu và đảm bảo tính toàn vẹn dữ liệu.
    * Có thể yêu cầu nhiều bảng và các phép nối phức tạp trong các truy vấn.
    * Thường được sử dụng trong các hệ thống OLTP (Online Transaction Processing) nơi tính toàn vẹn dữ liệu là quan trọng nhất.

* **Mô hình dữ liệu đã denormalized:**
    * Cố ý đưa một số dư thừa dữ liệu trở lại để cải thiện hiệu suất truy vấn.
    * Có thể có ít bảng hơn và các truy vấn đơn giản hơn.
    * Có thể làm giảm tính toàn vẹn dữ liệu và làm phức tạp việc cập nhật dữ liệu.
    * Thường được sử dụng trong các hệ thống OLAP (Online Analytical Processing) nơi hiệu suất truy vấn là ưu tiên hàng đầu.

**Tóm lại:** Sự lựa chọn giữa mô hình dữ liệu được chuẩn hóa hoàn toàn và mô hình dữ liệu đã denormalized phụ thuộc vào yêu cầu cụ thể của ứng dụng và sự cân bằng giữa hiệu suất truy vấn, tính toàn vẹn dữ liệu và độ phức tạp của việc bảo trì dữ liệu.

## 5. Câu hỏi về Stored Procedures, Functions và Triggers:

### Khi nào bạn nên sử dụng stored procedures thay vì câu lệnh SQL trực tiếp?

Stored procedures là các đoạn mã SQL được biên dịch trước và lưu trữ trên máy chủ cơ sở dữ liệu. Bạn nên cân nhắc sử dụng stored procedures trong các trường hợp sau:

* **Tái sử dụng logic:** Khi bạn có một đoạn mã SQL phức tạp được sử dụng nhiều lần trong ứng dụng, stored procedure giúp tránh việc lặp lại mã, tăng tính nhất quán và dễ bảo trì.
* **Bảo mật:** Stored procedures cho phép bạn kiểm soát truy cập vào dữ liệu bằng cách cấp quyền thực thi procedure thay vì quyền truy cập trực tiếp vào bảng.
* **Hiệu suất:** Vì stored procedures được biên dịch trước, chúng có thể thực thi nhanh hơn so với việc gửi các câu lệnh SQL riêng lẻ từ ứng dụng, đặc biệt là khi có nhiều câu lệnh hoặc logic phức tạp.
* **Giảm tải mạng:** Stored procedures giảm lưu lượng mạng giữa ứng dụng và cơ sở dữ liệu vì chỉ cần gửi tên procedure và các tham số cần thiết.

### Sự khác biệt giữa stored procedure và function trong SQL là gì?

Cả stored procedure và function đều là các đoạn mã SQL được lưu trữ trên máy chủ, nhưng có một số điểm khác biệt quan trọng:

* **Giá trị trả về:** Function luôn trả về một giá trị, trong khi stored procedure có thể trả về nhiều giá trị hoặc không trả về giá trị nào.
* **Sử dụng trong truy vấn:** Function có thể được sử dụng trong các câu lệnh SQL như một phần của biểu thức, trong khi stored procedure không thể.
* **Thay đổi dữ liệu:** Function thường được sử dụng để tính toán và trả về giá trị, không nên thay đổi dữ liệu trực tiếp. Stored procedure có thể thực hiện các thao tác thay đổi dữ liệu như `INSERT`, `UPDATE`, `DELETE`.

### Trigger là gì và khi nào bạn sẽ sử dụng trigger trong cơ sở dữ liệu?

Trigger là một đoạn mã SQL đặc biệt được tự động thực thi khi có sự kiện cụ thể xảy ra trên một bảng, chẳng hạn như `INSERT`, `UPDATE` hoặc `DELETE`. 

Bạn có thể sử dụng trigger để:

* **Đảm bảo tính toàn vẹn dữ liệu:** Thực hiện các kiểm tra phức tạp hoặc cập nhật các bảng liên quan để duy trì tính nhất quán của dữ liệu.
* **Kiểm soát truy cập:** Ngăn chặn hoặc ghi lại các thay đổi trái phép đối với dữ liệu.
* **Tự động hóa các tác vụ:** Thực hiện các tác vụ liên quan đến thay đổi dữ liệu, chẳng hạn như gửi thông báo hoặc cập nhật nhật ký.

```sql
-- Ví dụ về trigger
CREATE TRIGGER trg_CapNhatSanPham
ON SanPham
AFTER UPDATE
AS
BEGIN
    -- Cập nhật bảng lịch sử giá sản phẩm
    INSERT INTO LichSuGiaSanPham (MaSanPham, GiaCu, GiaMoi, ThoiGianCapNhat)
    SELECT d.MaSanPham, d.Gia, i.Gia, GETDATE()
    FROM deleted d
    JOIN inserted i ON d.MaSanPham = i.MaSanPham;
END
```

## 6. Câu hỏi về thiết kế cơ sở dữ liệu quan hệ (Relational Database Design)

### Bạn có thể giải thích cách thiết kế một sơ đồ ER (Entity-Relationship Diagram) không? Các kiểu quan hệ chính trong ERD là gì?

Sơ đồ ER (Entity-Relationship Diagram) là một công cụ trực quan quan trọng trong thiết kế cơ sở dữ liệu quan hệ, mô tả cấu trúc dữ liệu và mối quan hệ giữa chúng. Dưới đây là các bước cơ bản để thiết kế một sơ đồ ER:

1. **Xác định các thực thể (Entities):** Thực thể là những đối tượng hoặc khái niệm quan trọng trong hệ thống mà bạn muốn lưu trữ thông tin. Ví dụ: Nhân viên, Sản phẩm, Đơn hàng.
2. **Xác định các thuộc tính (Attributes):** Thuộc tính là các đặc điểm hoặc thông tin mô tả thực thể. Ví dụ: Mã nhân viên, Tên nhân viên, Giá sản phẩm.
3. **Xác định các mối quan hệ (Relationships):** Mối quan hệ thể hiện cách các thực thể liên kết với nhau. Ví dụ: Nhân viên làm việc cho một Phòng ban, Đơn hàng chứa nhiều Sản phẩm.
4. **Xác định lực lượng tham gia (Cardinality) và tính bắt buộc tham gia (Participation):** Lực lượng tham gia chỉ ra số lượng thực thể có thể tham gia vào một mối quan hệ (một-một, một-nhiều, nhiều-nhiều). Tính bắt buộc tham gia chỉ ra liệu một thực thể có bắt buộc phải tham gia vào mối quan hệ hay không.

**Các kiểu quan hệ chính trong ERD:**

* **Một-một (One-to-One):** Mỗi thực thể ở một bên của mối quan hệ chỉ có thể liên kết với tối đa một thực thể ở bên kia. Ví dụ: Mỗi nhân viên chỉ có thể có một số bảo hiểm xã hội duy nhất.
* **Một-nhiều (One-to-Many):** Một thực thể ở một bên của mối quan hệ có thể liên kết với nhiều thực thể ở bên kia, nhưng mỗi thực thể ở bên kia chỉ có thể liên kết với tối đa một thực thể ở bên này. Ví dụ: Một phòng ban có thể có nhiều nhân viên, nhưng mỗi nhân viên chỉ làm việc cho một phòng ban.
* **Nhiều-nhiều (Many-to-Many):** Mỗi thực thể ở một bên của mối quan hệ có thể liên kết với nhiều thực thể ở bên kia, và ngược lại. Ví dụ: Một đơn hàng có thể chứa nhiều sản phẩm, và một sản phẩm có thể xuất hiện trong nhiều đơn hàng.

### Primary Key và Foreign Key khác nhau như thế nào? Tại sao chúng quan trọng trong việc thiết kế cơ sở dữ liệu?

* **Primary Key:** Là một cột hoặc tập hợp các cột dùng để định danh duy nhất từng hàng trong một bảng.
    * Mỗi bảng chỉ có thể có một primary key.
    * Giá trị của primary key không được null và phải là duy nhất.
* **Foreign Key:** Là một cột hoặc tập hợp các cột trong một bảng tham chiếu đến primary key của một bảng khác.
    * Foreign key tạo ra mối liên kết giữa các bảng, đảm bảo tính toàn vẹn tham chiếu (referential integrity).
    * Giá trị của foreign key có thể null hoặc trùng lặp, nhưng phải tồn tại trong bảng được tham chiếu (hoặc là null).

**Tầm quan trọng của Primary Key và Foreign Key:**

* **Đảm bảo tính toàn vẹn dữ liệu:** Primary key ngăn chặn việc chèn các hàng trùng lặp, đảm bảo mỗi hàng có một định danh duy nhất. Foreign key đảm bảo rằng các giá trị trong cột foreign key phải tồn tại trong bảng được tham chiếu, ngăn chặn việc tạo ra các liên kết không hợp lệ.
* **Tạo mối quan hệ giữa các bảng:** Foreign key cho phép bạn kết nối các bảng với nhau, tạo ra một cấu trúc dữ liệu có ý nghĩa và hỗ trợ các truy vấn phức tạp liên quan đến nhiều bảng.
* **Hỗ trợ các thao tác cập nhật và xóa dữ liệu:** Cơ sở dữ liệu có thể sử dụng các ràng buộc về primary key và foreign key để tự động cập nhật hoặc xóa các hàng liên quan khi có thay đổi trong bảng chính, đảm bảo tính nhất quán của dữ liệu.

### Làm thế nào để bạn xử lý mối quan hệ nhiều-nhiều (many-to-many) trong cơ sở dữ liệu?

Mối quan hệ nhiều-nhiều không thể được biểu diễn trực tiếp trong mô hình cơ sở dữ liệu quan hệ. Để xử lý mối quan hệ nhiều-nhiều, bạn cần tạo một bảng trung gian (còn gọi là bảng liên kết hoặc bảng giao nhau) có chứa foreign key tham chiếu đến primary key của cả hai bảng liên quan.

**Ví dụ:**

Giả sử bạn có hai bảng: `SanPham` và `DonHang`, và một sản phẩm có thể xuất hiện trong nhiều đơn hàng, đồng thời một đơn hàng có thể chứa nhiều sản phẩm.

Để xử lý mối quan hệ nhiều-nhiều này, bạn cần tạo một bảng trung gian `ChiTietDonHang` có các cột sau:

* `MaSanPham` (foreign key tham chiếu đến `SanPham.MaSanPham`)
* `MaDonHang` (foreign key tham chiếu đến `DonHang.MaDonHang`)
* `SoLuong` (hoặc các thông tin khác liên quan đến mối quan hệ giữa sản phẩm và đơn hàng)

Bảng trung gian này sẽ lưu trữ thông tin về từng sản phẩm có trong mỗi đơn hàng, cho phép bạn truy vấn và quản lý mối quan hệ nhiều-nhiều một cách hiệu quả.

## 7. Câu hỏi về SQL Performance Tuning:

### Khi nào bạn nên sử dụng partitioning hoặc sharding trong cơ sở dữ liệu? Hãy giải thích ưu và nhược điểm của từng phương pháp.

**Partitioning (Phân vùng)**

* **Khi nào nên sử dụng:**
    * Khi bảng dữ liệu của bạn rất lớn và bạn thường xuyên truy vấn hoặc quản lý dữ liệu dựa trên một tiêu chí cụ thể (ví dụ: theo tháng, năm, khu vực địa lý).
    * Khi bạn muốn cải thiện hiệu suất truy vấn bằng cách chỉ truy cập vào phân vùng dữ liệu cần thiết thay vì toàn bộ bảng.
    * Khi bạn muốn đơn giản hóa việc quản lý và bảo trì dữ liệu bằng cách thực hiện các thao tác như sao lưu, khôi phục hoặc di chuyển dữ liệu trên từng phân vùng riêng lẻ.

* **Ưu điểm:**
    * Cải thiện hiệu suất truy vấn bằng cách giảm lượng dữ liệu cần truy cập.
    * Đơn giản hóa việc quản lý và bảo trì dữ liệu lớn.
    * Có thể cải thiện khả năng mở rộng của hệ thống bằng cách phân phối dữ liệu trên nhiều đĩa hoặc máy chủ.

* **Nhược điểm:**
    * Tăng độ phức tạp trong thiết kế và quản lý cơ sở dữ liệu.
    * Có thể gây ra sự không đồng đều về kích thước các phân vùng, ảnh hưởng đến hiệu suất.
    * Cần xem xét kỹ lưỡng chiến lược phân vùng để tránh các truy vấn không hiệu quả phải truy cập nhiều phân vùng.

**Sharding (Phân mảnh)**

* **Khi nào nên sử dụng:**
    * Khi cơ sở dữ liệu của bạn phát triển đến mức không thể mở rộng theo chiều dọc (scale up) trên một máy chủ duy nhất.
    * Khi bạn cần phân phối dữ liệu trên nhiều máy chủ để tăng khả năng xử lý và lưu trữ.
    * Khi bạn muốn cải thiện khả năng chịu lỗi và tính sẵn sàng của hệ thống bằng cách có nhiều bản sao dữ liệu trên các máy chủ khác nhau.

* **Ưu điểm:**
    * Tăng khả năng mở rộng theo chiều ngang (scale out) của hệ thống.
    * Cải thiện khả năng chịu lỗi và tính sẵn sàng của hệ thống.
    * Có thể cải thiện hiệu suất truy vấn bằng cách phân tán tải trên nhiều máy chủ.

* **Nhược điểm:**
    * Tăng độ phức tạp trong thiết kế, triển khai và quản lý hệ thống.
    * Cần xem xét kỹ lưỡng chiến lược sharding để đảm bảo phân phối dữ liệu hợp lý và tránh các truy vấn không hiệu quả phải truy cập nhiều shard.
    * Có thể gây khó khăn trong việc duy trì tính toàn vẹn và nhất quán của dữ liệu trên các shard khác nhau.

### Bạn làm gì khi phát hiện một truy vấn thực hiện quét toàn bảng (full table scan)?

Quét toàn bảng xảy ra khi cơ sở dữ liệu phải đọc toàn bộ bảng để tìm kiếm dữ liệu thỏa mãn điều kiện truy vấn, thay vì sử dụng index. Điều này thường dẫn đến hiệu suất truy vấn kém, đặc biệt là với các bảng lớn. Khi phát hiện một truy vấn thực hiện quét toàn bảng, bạn có thể thực hiện các bước sau để tối ưu hóa nó:

1. **Phân tích truy vấn và execution plan:** Xem xét kỹ câu truy vấn và execution plan để hiểu lý do tại sao cơ sở dữ liệu không sử dụng index.
2. **Tạo hoặc điều chỉnh index:** Nếu truy vấn không sử dụng index hoặc sử dụng index không hiệu quả, hãy xem xét tạo index mới trên các cột được sử dụng trong mệnh đề `WHERE` hoặc điều chỉnh các index hiện có.
3. **Viết lại truy vấn:** Đôi khi, việc viết lại truy vấn theo một cách khác có thể giúp cơ sở dữ liệu sử dụng index hiệu quả hơn. Ví dụ, tránh sử dụng các hàm trên cột được đánh index, hoặc sử dụng các toán tử phù hợp với kiểu dữ liệu của cột.
4. **Tối ưu hóa thống kê:** Đảm bảo rằng cơ sở dữ liệu có thông tin thống kê cập nhật về các bảng và cột, giúp nó đưa ra quyết định tốt hơn về việc sử dụng index.
5. **Cân nhắc các yếu tố khác:** Nếu các biện pháp trên không giải quyết được vấn đề, hãy xem xét các yếu tố khác như cấu hình phần cứng, tải hệ thống, hoặc thiết kế cơ sở dữ liệu.

### Làm thế nào để quản lý việc kết nối hiệu quả với cơ sở dữ liệu lớn thông qua connection pooling?

Connection pooling là một kỹ thuật giúp cải thiện hiệu suất và khả năng mở rộng của ứng dụng bằng cách tái sử dụng các kết nối đến cơ sở dữ liệu thay vì tạo mới mỗi khi cần. Khi sử dụng connection pooling:

1. **Ứng dụng yêu cầu một kết nối từ pool:** Khi ứng dụng cần truy cập cơ sở dữ liệu, nó yêu cầu một kết nối từ pool thay vì tạo một kết nối mới.
2. **Pool cung cấp một kết nối sẵn có hoặc tạo mới:** Nếu có kết nối sẵn có trong pool, nó sẽ được cung cấp cho ứng dụng. Nếu không, pool sẽ tạo một kết nối mới và thêm vào pool.
3. **Ứng dụng sử dụng kết nối:** Ứng dụng sử dụng kết nối để thực hiện các truy vấn và thao tác trên cơ sở dữ liệu.
4. **Ứng dụng trả kết nối về pool:** Sau khi hoàn thành, ứng dụng trả kết nối về pool để có thể được tái sử dụng bởi các yêu cầu khác.

**Lợi ích của connection pooling:**

* **Giảm thiểu chi phí tạo kết nối:** Việc tạo kết nối đến cơ sở dữ liệu thường tốn kém về thời gian và tài nguyên. Connection pooling giúp giảm thiểu chi phí này bằng cách tái sử dụng các kết nối hiện có.
* **Cải thiện hiệu suất:** Ứng dụng có thể nhận được kết nối nhanh chóng từ pool, giảm thời gian chờ đợi và tăng tốc độ xử lý yêu cầu.
* **Tăng khả năng mở rộng:** Bằng cách giới hạn số lượng kết nối đồng thời, connection pooling giúp ngăn ngừa tình trạng quá tải cơ sở dữ liệu và đảm bảo rằng ứng dụng có thể xử lý nhiều yêu cầu đồng thời.

**Cách quản lý connection pooling:**

* **Cấu hình pool:** Đặt các thông số như kích thước tối thiểu và tối đa của pool, thời gian chờ tối đa để lấy kết nối, và các chính sách quản lý kết nối.
* **Giám sát pool:** Theo dõi các chỉ số như số lượng kết nối đang sử dụng, số lượng kết nối nhàn rỗi, và thời gian chờ trung bình để lấy kết nối.
* **Điều chỉnh pool:** Dựa trên thông tin giám sát, điều chỉnh các thông số của pool để tối ưu hóa hiệu suất và khả năng mở rộng của ứng dụng.

**Lưu ý:** Hầu hết các hệ quản trị cơ sở dữ liệu và framework phát triển ứng dụng đều cung cấp các thư viện hoặc công cụ hỗ trợ connection pooling. Hãy tìm hiểu và sử dụng các công cụ này để quản lý kết nối đến cơ sở dữ liệu lớn một cách hiệu quả.

## 8. Câu hỏi về bảo mật trong SQL:

### SQL Injection là gì? Làm thế nào để phòng chống các cuộc tấn công này?

**SQL Injection là gì?**

SQL Injection là một kỹ thuật tấn công phổ biến, trong đó kẻ tấn công lợi dụng lỗ hổng trong việc xử lý dữ liệu đầu vào của ứng dụng để chèn các câu lệnh SQL độc hại vào truy vấn SQL. Điều này có thể cho phép kẻ tấn công truy cập, sửa đổi hoặc xóa dữ liệu trái phép, thậm chí kiểm soát toàn bộ cơ sở dữ liệu.

**Làm thế nào để phòng chống các cuộc tấn công SQL Injection?**

* **Sử dụng Prepared Statements (Câu lệnh chuẩn bị):** Đây là cách hiệu quả nhất để ngăn chặn SQL Injection. Prepared statements tách biệt mã SQL khỏi dữ liệu đầu vào, đảm bảo rằng dữ liệu đầu vào được xử lý an toàn và không thể ảnh hưởng đến cấu trúc của truy vấn.
* **Validate (Xác thực) và Sanitize (Làm sạch) dữ liệu đầu vào:** Kiểm tra kỹ lưỡng tất cả dữ liệu đầu vào từ người dùng trước khi sử dụng chúng trong truy vấn SQL. Loại bỏ hoặc thoát các ký tự đặc biệt có thể được sử dụng để thực hiện SQL Injection.
* **Sử dụng Stored Procedures:** Stored procedures giúp giảm thiểu rủi ro SQL Injection bằng cách đóng gói logic truy vấn trên máy chủ cơ sở dữ liệu và chỉ cho phép người dùng thực thi chúng với các tham số được xác định trước.
* **Phân quyền truy cập tối thiểu:** Cấp cho người dùng chỉ những quyền cần thiết để thực hiện công việc của họ, hạn chế khả năng gây thiệt hại nếu tài khoản của họ bị xâm nhập.
* **Cập nhật và vá lỗi hệ thống:** Đảm bảo rằng hệ quản trị cơ sở dữ liệu và các ứng dụng liên quan luôn được cập nhật với các bản vá lỗi bảo mật mới nhất.

### Bạn sẽ phân quyền cho người dùng truy cập vào cơ sở dữ liệu như thế nào? Hãy giải thích cách sử dụng GRANT và REVOKE.

Phân quyền truy cập là quá trình kiểm soát quyền của người dùng đối với các đối tượng trong cơ sở dữ liệu, chẳng hạn như bảng, view, stored procedure. Các câu lệnh `GRANT` và `REVOKE` được sử dụng để cấp và thu hồi các quyền này.

* **GRANT:** Cấp quyền cho người dùng hoặc vai trò (role).

```sql
-- Cấp quyền SELECT trên bảng NhanVien cho người dùng 'user1'
GRANT SELECT ON NhanVien TO 'user1';

-- Cấp quyền thực thi stored procedure sp_CapNhatSanPham cho vai trò 'NhanVien'
GRANT EXECUTE ON sp_CapNhatSanPham TO 'NhanVien';


* **REVOKE:**Thu hồi quyền đã cấp cho người dùng hoặc vai trò.
```sql

-- Thu hồi quyền SELECT trên bảng NhanVien từ người dùng 'user1'
REVOKE SELECT ON NhanVien FROM 'user1';

-- Thu hồi quyền thực thi stored procedure sp_CapNhatSanPham từ vai trò 'NhanVien'
REVOKE EXECUTE ON sp_CapNhatSanPham FROM 'NhanVien';

```


## 9. Câu hỏi về các hệ quản trị cơ sở dữ liệu (RDBMS)

### Sự khác nhau giữa MySQL, PostgreSQL, Oracle và SQL Server là gì? Bạn có thể chỉ ra một số điểm mạnh và yếu của từng hệ quản trị không?

Mỗi hệ quản trị cơ sở dữ liệu quan hệ (RDBMS) đều có những đặc điểm riêng, phù hợp với các nhu cầu và môi trường khác nhau. Dưới đây là một số so sánh cơ bản về MySQL, PostgreSQL, Oracle và SQL Server:

| Đặc điểm        | MySQL                                | PostgreSQL                              | Oracle                                 | SQL Server                           |
|-----------------|----------------------------------------|-------------------------------------------|-----------------------------------------|--------------------------------------|
| Nhà phát triển    | Oracle Corporation                     | Cộng đồng nguồn mở                        | Oracle Corporation                     | Microsoft                             |
| Giấy phép        | GPL và thương mại                       | PostgreSQL                                | Thương mại                              | Thương mại                              |
| Điểm mạnh        | - Dễ sử dụng và cài đặt<br> - Hiệu năng cao với các truy vấn đơn giản<br> - Cộng đồng lớn và nhiều tài liệu hỗ trợ | - Tuân thủ SQL tiêu chuẩn cao<br> - Hỗ trợ nhiều tính năng nâng cao (ví dụ: JSON, full-text search)<br> - Khả năng mở rộng tốt | - Hiệu năng cao và ổn định<br> - Hỗ trợ nhiều tính năng doanh nghiệp<br> - Bảo mật mạnh mẽ | - Tích hợp tốt với các sản phẩm của Microsoft<br> - Dễ quản lý và giám sát<br> - Hỗ trợ nhiều công cụ BI |
| Điểm yếu        | - Hỗ trợ các tính năng nâng cao hạn chế<br> - Khả năng mở rộng theo chiều ngang không tốt bằng | - Hiệu năng có thể kém hơn MySQL trong một số trường hợp<br> - Cộng đồng nhỏ hơn MySQL | - Chi phí cao<br> - Độ phức tạp cao | - Chi phí cao<br> - Khả năng mở rộng theo chiều ngang không tốt bằng |
| Phù hợp cho      | - Ứng dụng web nhỏ và vừa<br> - Hệ thống nhúng<br> - Các dự án cần sự đơn giản và dễ sử dụng | - Ứng dụng web lớn và phức tạp<br> - Hệ thống yêu cầu tính năng nâng cao<br> - Các dự án cần tuân thủ SQL tiêu chuẩn | - Ứng dụng doanh nghiệp lớn<br> - Hệ thống quan trọng, yêu cầu độ ổn định và bảo mật cao | - Ứng dụng doanh nghiệp vừa và lớn trong môi trường Microsoft |

### Khi nào bạn nên chọn PostgreSQL thay vì MySQL (hoặc ngược lại)?

* **Chọn PostgreSQL khi:**
    * Bạn cần tuân thủ SQL tiêu chuẩn cao và hỗ trợ nhiều tính năng nâng cao (ví dụ: JSON, full-text search, window functions).
    * Ứng dụng của bạn có cấu trúc dữ liệu phức tạp và yêu cầu tính toàn vẹn dữ liệu cao.
    * Bạn cần khả năng mở rộng tốt cả về chiều dọc và chiều ngang.
    * Bạn sẵn sàng đầu tư thời gian để tìm hiểu và cấu hình PostgreSQL.

* **Chọn MySQL khi:**
    * Bạn cần một hệ quản trị cơ sở dữ liệu đơn giản, dễ sử dụng và cài đặt.
    * Ứng dụng của bạn chủ yếu thực hiện các truy vấn đơn giản và không yêu cầu nhiều tính năng nâng cao.
    * Bạn cần hiệu năng cao cho các truy vấn đọc.
    * Bạn muốn sử dụng một hệ quản trị cơ sở dữ liệu miễn phí và có cộng đồng hỗ trợ lớn.

### Hãy giải thích cách một hệ thống quản trị cơ sở dữ liệu xử lý đồng thời (concurrency).

Concurrency (Đồng thời) là khả năng của một hệ thống quản trị cơ sở dữ liệu cho phép nhiều người dùng hoặc ứng dụng truy cập và thao tác trên cùng một dữ liệu cùng một lúc mà không gây ra xung đột hoặc mất mát dữ liệu. Để xử lý đồng thời, các hệ quản trị cơ sở dữ liệu thường sử dụng các kỹ thuật sau:

* **Khóa (Locking):** Cơ chế khóa ngăn chặn nhiều transaction cùng truy cập và sửa đổi cùng một dữ liệu tại cùng một thời điểm. Các loại khóa phổ biến bao gồm khóa chia sẻ (shared lock) cho phép đọc đồng thời và khóa độc quyền (exclusive lock) ngăn chặn mọi truy cập khác khi đang sửa đổi dữ liệu.
* **Đánh dấu thời gian (Timestamping):** Mỗi transaction được gán một đánh dấu thời gian duy nhất. Khi có xung đột, hệ thống sẽ quyết định transaction nào được phép tiếp tục dựa trên thứ tự thời gian của chúng.
* **Kiểm soát phiên bản đa đồng thời (Multiversion Concurrency Control - MVCC):** Mỗi transaction làm việc trên một bản sao (snapshot) của dữ liệu tại thời điểm bắt đầu transaction. Điều này cho phép đọc đồng thời mà không cần khóa, nhưng có thể làm tăng độ phức tạp của việc quản lý các phiên bản dữ liệu.

**Các vấn đề thường gặp trong xử lý đồng thời và cách giải quyết:**

* **Dirty read:** Một transaction đọc dữ liệu đã bị sửa đổi nhưng chưa được commit bởi một transaction khác. Giải pháp: Sử dụng các cấp độ cô lập (isolation level) cao hơn như Read Committed hoặc Repeatable Read.
* **Non-repeatable read:** Một transaction đọc cùng một dữ liệu nhiều lần nhưng nhận được kết quả khác nhau do dữ liệu đã bị sửa đổi bởi một transaction khác. Giải pháp: Sử dụng cấp độ cô lập Repeatable Read hoặc Serializable.
* **Phantom read:** Một transaction thực hiện hai lần truy vấn giống nhau nhưng nhận được kết quả khác nhau do có thêm dữ liệu mới được chèn vào bởi một transaction khác. Giải pháp: Sử dụng cấp độ cô lập Serializable.

**Lưu ý:** Việc lựa chọn kỹ thuật xử lý đồng thời và cấp độ cô lập phù hợp phụ thuộc vào yêu cầu cụ thể của ứng dụng, cân nhắc giữa tính nhất quán dữ liệu, hiệu suất và khả năng mở rộng.