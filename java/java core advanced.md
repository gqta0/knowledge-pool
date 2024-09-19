### HashMap xử lý xung đột hash key như thế nào?

Xung đột hash key xảy ra khi hai hoặc nhiều key khác nhau lại có cùng một giá trị hash. Điều này là do hàm hash không thể ánh xạ một cách duy nhất tất cả các key vào một bảng có kích thước hữu hạn.

Các phương pháp giải quyết xung đột hash key trong HashMap
Java sử dụng phương pháp chaining để giải quyết xung đột này. Cụ thể:

- Mỗi bucket trong `HashMap` là một `Linked List`: Khi có nhiều key có cùng giá trị hash, chúng sẽ được thêm vào cùng một bucket. Để phân biệt các key này, Java sử dụng một `Linked List` tại mỗi bucket.
- So sánh bằng phương thức `equals()`: Khi tìm kiếm một key, HashMap sẽ tính toán giá trị hash của key đó, xác định bucket tương ứng và sau đó duyệt qua linked list tại bucket đó, so sánh từng phần tử bằng phương thức `equals()` để tìm key cần tìm.

Cách giảm thiểu xung đột hash key
- Chọn hàm hash tốt: Hàm hash tốt sẽ `phân phối các key một cách đồng đều` vào các bucket, giảm thiểu khả năng xảy ra xung đột.
- Điều chỉnh độ lớn của bảng hash: Khi số lượng phần tử trong HashMap tăng lên, nên tăng kích thước của bảng hash để giảm mật độ các phần tử trong mỗi bucket.
- Sử dụng lớp `HashMap` phù hợp: Java cung cấp các lớp HashMap khác nhau với các thuật toán giải quyết xung đột khác nhau. Chọn lớp phù hợp với yêu cầu của ứng dụng.
- Cân nhắc sử dụng các cấu trúc dữ liệu khác: Nếu xung đột hash key gây ra vấn đề nghiêm trọng, có thể cân nhắc sử dụng các cấu trúc dữ liệu khác như TreeMap, LinkedHashMap.


### ArrayList hoạt động như thế nào?
ArrayList là một lớp trong Java được sử dụng để tạo ra các danh sách động (dynamic arrays). Nó cho phép bạn lưu trữ một tập hợp các đối tượng và kích thước của danh sách có thể thay đổi linh hoạt trong quá trình thực thi chương trình.

**Cấu trúc bên trong**
- Mảng cơ bản: ArrayList sử dụng một mảng để lưu trữ các phần tử. Mảng này có thể tự động tăng kích thước khi cần thiết.
- Kích thước và sức chứa:
    - Kích thước: Là số lượng phần tử thực tế đang có trong ArrayList.
    - Sức chứa: Là số lượng phần tử tối đa mà mảng cơ bản có thể chứa mà không cần phải tăng kích thước.

**Cơ chế hoạt động**
- Khởi tạo: Khi bạn tạo một ArrayList, một mảng cơ bản có kích thước mặc định sẽ được tạo.
- Thêm phần tử: Khi bạn thêm một phần tử vào ArrayList, phần tử đó sẽ được thêm vào vị trí cuối cùng của mảng.
    - Nếu mảng còn đủ chỗ: Phần tử mới sẽ được thêm vào ngay.
    - Nếu mảng đầy:
        - Một mảng mới có kích thước lớn hơn sẽ được tạo.
        - Tất cả các phần tử cũ sẽ được sao chép sang mảng mới.
        - Phần tử mới sẽ được thêm vào mảng mới.
- Xóa phần tử: Khi bạn xóa một phần tử, các phần tử phía sau sẽ được dịch chuyển lên để lấp đầy khoảng trống.
- Truy cập phần tử: Bạn có thể truy cập một phần tử bằng chỉ số của nó. Việc truy cập các phần tử bằng chỉ số có hiệu suất rất cao.

**Ưu điểm của ArrayList**
- **Kích thước động**: Có thể tăng hoặc giảm kích thước tùy ý.
- **Truy cập nhanh**: Truy cập các phần tử bằng chỉ số rất nhanh.
- **Đa dạng các phương thức**: ArrayList cung cấp nhiều phương thức để thực hiện các thao tác như thêm, xóa, tìm kiếm, sắp xếp...

**Nhược điểm của ArrayList**
- **Không đồng bộ**: ArrayList không an toàn cho đa luồng. Nếu nhiều luồng cùng truy cập và - sửa đổi một ArrayList, có thể dẫn đến các vấn đề về đồng bộ hóa.
- **Chi phí khi tăng kích thước**: Khi mảng cơ bản cần tăng kích thước, một mảng mới sẽ được tạo và tất cả các phần tử sẽ được sao chép sang mảng mới. Điều này có thể tốn kém về mặt hiệu năng nếu thực hiện thường xuyên.



## Đa luồng

### Từ khóa synchronized hoạt động như thế nào trong Java?


Hiện bản thảo

`Synchronized` là một từ khóa quan trọng trong Java, được sử dụng để bảo vệ các đoạn mã khỏi việc truy cập đồng thời bởi nhiều luồng. Khi một đoạn mã được đánh dấu là synchronized, chỉ có một luồng duy nhất có thể thực thi đoạn mã đó tại một thời điểm. Điều này giúp đảm bảo tính nhất quán của dữ liệu khi nhiều luồng cùng truy cập vào các tài nguyên chia sẻ.

Cơ chế hoạt động của synchronized
1. **Khóa (lock)**: Khi một luồng muốn thực thi một phương thức hoặc khối mã được đánh dấu là synchronized, nó sẽ cố gắng lấy một khóa.
2. **Kiểm tra khóa**: Nếu khóa đang bị một luồng khác giữ, luồng hiện tại sẽ bị chặn và chờ cho đến khi khóa được giải phóng.
3. **Thực thi**: Khi luồng lấy được khóa, nó sẽ thực thi đoạn mã được bảo vệ và các luồng khác sẽ phải chờ.
4. **Giải phóng khóa**: Khi luồng hoàn thành việc thực thi, nó sẽ giải phóng khóa, cho phép các luồng khác có cơ hội để lấy khóa.

```Java 

public synchronized void method() {
    // Code được bảo vệ
}

public void method() {
    synchronized(this) {
        // Code được bảo vệ
    }
}

```

Lưu ý khi sử dụng synchronized
- **Hiệu suất**: Sử dụng synchronized có thể làm giảm hiệu suất của chương trình, vì các luồng khác phải chờ đợi. Vì vậy, chỉ nên sử dụng synchronized khi cần thiết.
- **Deadlock**: Nếu các luồng bị khóa lẫn nhau, có thể dẫn đến deadlock.
- **Livelock**: Các luồng liên tục thay đổi trạng thái nhưng không tiến bộ được.
- **Starvation**: Một luồng không bao giờ được cấp phát tài nguyên.


Các giải pháp thay thế
- **ReentrantLock**: Cung cấp các tính năng linh hoạt hơn so với synchronized, như khả năng chờ đợi có thời hạn, công bằng, v.v.
- **Atomic variables**: Dùng để thực hiện các thao tác nguyên tử trên các biến số đơn giản, như tăng, giảm, so sánh và đặt giá trị.
- **Concurrent collections**: Các lớp collection như ConcurrentHashMap, CopyOnWriteArrayList cung cấp các phương thức an toàn cho đa luồng.

### Atomic variables

`Atomic variables` là các biến được thiết kế để thực hiện các thao tác nguyên tử (atomic operations) trên các kiểu dữ liệu cơ bản như `int, long, boolean, reference và các kiểu dữ liệu nguyên tử khác`. Điều này có nghĩa là các thao tác trên các biến atomic sẽ được thực hiện một cách không thể chia cắt, đảm bảo tính nhất quán của dữ liệu trong môi trường đa luồng.

**Tại sao cần Atomic Variables?**
- **Bảo vệ dữ liệu**: Atomic variables giúp bảo vệ dữ liệu khỏi các vấn đề về đồng bộ hóa trong môi trường đa luồng.
- **Hiệu suất**: Các thao tác trên atomic variables thường được tối ưu hóa để có hiệu suất cao hơn so với việc sử dụng synchronized cho các khối mã.
- **Dễ sử dụng**: Atomic variables cung cấp các phương thức tiện lợi để thực hiện các thao tác nguyên tử, giúp đơn giản hóa việc viết mã đa luồng.


## JVM và Performance

### Giải thích khái niệm Classloader trong Java
`Classloader` là một thành phần quan trọng trong Java Virtual Machine (JVM), có nhiệm vụ tải các lớp (class) từ các file .class vào bộ nhớ khi chương trình được thực thi. Nói cách khác, `Classloader` là "người đưa" các lớp vào thế giới của JVM để chúng có thể được sử dụng.

Tại sao cần Classloader?
- Tách biệt các lớp: Classloader cho phép tách biệt các lớp được tải từ các nguồn khác nhau, giúp tạo ra các môi trường thực thi độc lập.
- Đại diện cho các namespace khác nhau: Mỗi Classloader tạo ra một namespace riêng biệt, giúp tránh xung đột tên giữa các lớp có cùng tên nhưng được tải từ các Classloader khác nhau.
- Nạp động: Classloader cho phép nạp các lớp một cách động trong thời gian chạy, tăng tính linh hoạt của ứng dụng.
Các loại Classloader

Java cung cấp ba loại Classloader chính:

- Bootstrap Classloader:
    - Là Classloader cấp cao nhất, được viết bằng ngôn ngữ native (C/C++).
    - Chịu trách nhiệm tải các lớp cốt lõi của Java (như java.lang, java.util, ...) từ thư mục jre/lib.
- Extension Classloader:
    - Tải các lớp từ thư mục jre/lib/ext.
    - Được sử dụng để mở rộng chức năng của JVM.
- System Classloader (hay Application Classloader):
    - Tải các lớp từ classpath được chỉ định khi khởi động JVM.
    - Là Classloader mặc định được sử dụng để tải các lớp của ứng dụng.
Quy trình nạp lớp
1. Kiểm tra: Khi một lớp cần được tải, JVM sẽ yêu cầu Classloader kiểm tra xem lớp đó đã được tải chưa. Nếu đã tải, JVM sẽ trả về một tham chiếu đến lớp đó.
2. Tìm kiếm: Nếu lớp chưa được tải, Classloader sẽ tìm kiếm lớp đó trong các thư mục hoặc jar file đã được cấu hình.
3. Định nghĩa: Khi tìm thấy lớp, Classloader sẽ đọc bytecode của lớp đó và tạo ra một đối tượng Class đại diện cho lớp đó trong bộ nhớ.
4. Liên kết: Classloader liên kết các tham chiếu trong lớp với các lớp khác và các thư viện.
5. Khởi tạo: Classloader khởi tạo các biến static và thực thi các khối static của lớp.


**Ưu điểm của Classloader**
- **Tải động**: Có thể tải các lớp trong thời gian chạy.
- **Tách biệt namespace**: Tránh xung đột tên giữa các lớp.
- **An toàn**: Bảo vệ ứng dụng khỏi các mã độc hại.
- **Linh hoạt**: Hỗ trợ các mô hình tải lớp phức tạp.


```java 

// Tạo một Classloader tùy chỉnh
public class CustomClassLoader extends ClassLoader {
    @Override
    protected Class<?> findClass(String name) throws ClassNotFoundException {
        // Logic tìm và tải lớp từ nguồn tùy chỉnh
        byte[] b = ...;
        return defineClass(name, b, 0, b.length);
    }
}

// Sử dụng Classloader tùy chỉnh
CustomClassLoader loader = new CustomClassLoader();
Class<?> myClass = loader.loadClass("com.example.MyClass");
```