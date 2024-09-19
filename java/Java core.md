### Làm sao để viết Functional Interface, Khi nào nó được dùng ?
- Câu trả lời: Functional Interface trong Java là một interface chỉ có một phương thức trừu tượng. Nó thường được sử dụng khi chúng ta muốn truyền một biểu thức lambda hoặc một reference đến một phương thức. Có thể khai báo bằng annotation `@FunctionalInterface`. Functional Interface thường được sử dụng trong các hàm xử lý sự kiện hoặc các tác vụ đơn giản trong các luồng xử lý song song.

### Lambda? Thường sử dụng Lambda khi nào ?
- Câu trả lời: Lambda Expression là một cách viết ngắn gọn giúp đơn giản hóa việc viết các lớp ẩn danh hoặc các interface đơn phương thức. Thường được dùng khi làm việc với các Functional Interface như Runnable, Callable, hoặc trong stream API để xử lý tập dữ liệu.

    ```Java
    // Ví dụ sử dụng Lambda với Runnable
    Runnable r = () -> System.out.println("Hello Lambda!");
    new Thread(r).start();
    ```
### Stream trong java là gì
- Stream là 1 API để xử lý dữ liệu tuân theo `phong cách lập trình hàm`. Stream không lưu trữ dữ liệu mà chỉ là một chuỗi các phép toán được thực thi trên một nguồn dữ liệu.Các phép toán trên Stream chỉ được thực thi khi bạn gọi một phép toán cuối cùng (terminal operation) như collect, forEach,...

- Stream giúp code ngắn gọn và rõ ràng, dồng thời cũng cung cấp các built-in function giúp dễ ràng hóa quá trình xử lý dữ liệu như filter, map, reduce....

- Stream không hỗ trợ thay đổi dữ liệu gốc


### Collection trong java

- `Là một cấu trúc dữ liệu`: Collection đại diện cho một tập hợp các đối tượng, có thể là các đối tượng đồng nhất hoặc không đồng nhất.
- `Lưu trữ và quản lý dữ liệu `: Collection được sử dụng để lưu trữ và quản lý dữ liệu trong suốt quá trình thực thi chương trình.
- Các loại Collection: List, Set, Map là những loại Collection phổ biến, mỗi loại có những đặc điểm và cách sử dụng riêng.
- `Thay đổi được`: Bạn có thể thêm, xóa, sửa đổi các phần tử trong một Collection.


### Sự khác nhau giữa Stream và Collection
- Câu trả lời: Stream là một API để xử lý dữ liệu tuần tự theo phong cách lập trình hàm, cho phép filter, map, reduce dữ liệu. Trong khi đó, Collection là các cấu trúc dữ liệu lưu trữ phần tử. Supplier cung cấp dữ liệu, Consumer tiêu thụ dữ liệu mà không trả lại kết quả, còn Predicate là biểu thức logic trả về giá trị boolean.

| **Tiêu chí**                   | **Stream**                                               | **Collection**                                          |
|---------------------------------|----------------------------------------------------------|---------------------------------------------------------|
| **Loại**                        | Được sử dụng để xử lý dữ liệu theo kiểu pipeline (luồng)  | Lưu trữ và quản lý các đối tượng trong bộ nhớ           |
| **Chế độ xử lý**                | Xử lý theo kiểu **lazily** (chỉ xử lý khi cần thiết)      | Xử lý dữ liệu **eagerly** (ngay khi thêm vào)           |
| **Chức năng**                   | Cung cấp các thao tác xử lý dữ liệu kiểu functional       | Chủ yếu để quản lý và thao tác dữ liệu (CRUD)            |
| **Tính thay đổi (Mutable)**     | Không hỗ trợ chỉnh sửa phần tử (immutable)                | Hỗ trợ thêm, xóa, sửa phần tử                           |
| **Kích thước**                  | Không lưu trữ dữ liệu, không có khái niệm kích thước      | Có kích thước rõ ràng (size)                            |
| **Nguồn dữ liệu**               | Có thể được tạo từ các nguồn khác nhau (Collection, I/O)  | Chỉ lưu trữ trong bộ nhớ                                |
| **Tính tuần tự hoặc song song** | Hỗ trợ xử lý tuần tự và song song với API song song       | Thường được xử lý tuần tự, không hỗ trợ song song       |
| **Hỗ trợ Iterator**             | Chỉ hỗ trợ iterator một lần (single-use)                  | Hỗ trợ iterator nhiều lần                               |
| **Tính kết thúc (Finite/Infinite)**| Có thể là luồng vô hạn (infinite stream)                | Luôn có kích thước hữu hạn (finite)                     |
| **Hoạt động**                   | Các phép biến đổi không thay đổi nguồn gốc                | Các phép biến đổi ảnh hưởng trực tiếp đến collection     |
| **Lazy Evaluation**             | Có, chỉ thực hiện khi cần                                  | Không, thực hiện ngay khi gọi                           |
| **Hỗ trợ lập trình song song**  | Có, với API đơn giản (`parallelStream()`)                 | Không hỗ trợ natively                                   |

### So sánh Array List và LinkedList ?

| **Tiêu chí**                        | **ArrayList**                                          | **LinkedList**                                          |
|-------------------------------------|--------------------------------------------------------|---------------------------------------------------------|
| **Cấu trúc dữ liệu**                | Dựa trên mảng động (dynamic array)                     | Dựa trên danh sách liên kết đôi (doubly linked list)     |
| **Truy cập phần tử (get/set)**      | Nhanh hơn vì có thể truy cập ngẫu nhiên (O(1))         | Chậm hơn, phải duyệt qua các node từ đầu (O(n))         |
| **Thêm/xóa ở giữa**                 | Chậm hơn do cần dịch chuyển các phần tử (O(n))          | Nhanh hơn vì chỉ cần thay đổi liên kết (O(1))            |
| **Thêm/xóa ở cuối**                 | Nhanh, O(1) khi không cần mở rộng kích thước mảng       | Nhanh, O(1) vì chỉ cần thay đổi liên kết                 |
| **Thêm/xóa ở đầu**                  | Chậm, O(n) do phải dịch chuyển các phần tử              | Nhanh, O(1) vì chỉ cần thay đổi liên kết                 |
| **Bộ nhớ sử dụng**                  | Ít tốn bộ nhớ hơn vì chỉ lưu trữ dữ liệu                | Tốn bộ nhớ hơn do cần lưu trữ cả liên kết giữa các node  |
| **Tính năng**                       | Tốt cho các thao tác truy xuất và thêm phần tử cuối     | Tốt cho các thao tác thêm/xóa ở đầu hoặc giữa danh sách  |
| **Thích hợp sử dụng**               | Khi cần truy cập ngẫu nhiên thường xuyên               | Khi cần thêm/xóa ở đầu, giữa hoặc cuối thường xuyên      |
| **Chi phí khi mở rộng**             | Khi vượt quá dung lượng, cần tạo mảng mới và copy dữ liệu | Không cần copy dữ liệu, chỉ cần tạo node mới            |
| **Duyệt (iteration)**               | Tốt với ListIterator và truy cập theo chỉ số (index)    | Chậm hơn khi duyệt qua vì không hỗ trợ truy cập chỉ số trực tiếp |

### So sánh List và Set ?

| **Tiêu chí**                    | **List**                                                | **Set**                                                 |
|----------------------------------|---------------------------------------------------------|---------------------------------------------------------|
| **Cấu trúc**                     | Cho phép lưu trữ các phần tử theo thứ tự                | Không đảm bảo thứ tự lưu trữ phần tử                    |
| **Phần tử trùng lặp**            | Cho phép phần tử trùng lặp                              | Không cho phép phần tử trùng lặp                        |
| **Truy cập theo chỉ số (index)** | Có thể truy cập phần tử thông qua chỉ số (index)        | Không hỗ trợ truy cập theo chỉ số                       |
| **Thứ tự phần tử**               | Duy trì thứ tự thêm vào                                 | Không duy trì thứ tự thêm vào (ngoại trừ `LinkedHashSet`)|
| **Hiệu suất tìm kiếm**           | Tìm kiếm chậm hơn vì phải duyệt qua toàn bộ danh sách (O(n)) | Tìm kiếm nhanh hơn, đặc biệt trong `HashSet` (O(1))      |
| **Thao tác thêm/xóa**            | Chậm hơn khi thêm/xóa phần tử giữa danh sách (O(n))     | Nhanh hơn khi thêm/xóa phần tử (O(1) cho `HashSet`)      |
| **Sử dụng**                      | Dùng khi cần lưu trữ các phần tử có thứ tự hoặc cần truy cập qua chỉ số | Dùng khi cần lưu trữ các phần tử duy nhất, không trùng lặp|
| **Triển khai phổ biến**          | `ArrayList`, `LinkedList`, `Vector`                     | `HashSet`, `LinkedHashSet`, `TreeSet`                   |

### Map, hashCode(), equals()
- Câu trả lời: Map là một cấu trúc dữ liệu lưu trữ các cặp key-value. Phương thức `hashCode()` được sử dụng để sinh ra mã băm cho đối tượng, trong khi `equals()` kiểm tra sự bằng nhau giữa hai đối tượng. Khi override `equals()`, cần phải override `hashCode()` để đảm bảo tính nhất quán khi dùng đối tượng đó trong các cấu trúc dữ liệu như HashMap.


### So sánh Abstract class và Interface?
- Câu trả lời: Abstract class có thể chứa cả phương thức đã triển khai và chưa triển khai, còn Interface chỉ chứa các phương thức trừu tượng (trước Java 8). Kể từ Java 8, Interface có thể chứa các phương thức mặc định và phương thức tĩnh. Abstract class phù hợp khi các lớp con cần chia sẻ các phương thức hoặc trạng thái chung.


### LifeCycle của Thread
- Câu trả lời: Thread có các trạng thái sau: New, Runnable, Blocked, Waiting, Timed Waiting, và Terminated. Chu kỳ này mô tả quá trình tạo, chạy và kết thúc của một luồng (Thread) trong Java.

    ```Java
    // Ví dụ tạo thread đơn giản
    Thread t = new Thread(() -> System.out.println("Thread running"));
    t.start();
    ```

### So sánh Comparable và Comparator
- `Comparable` được sử dụng để xác định thứ tự tự nhiên của các đối tượng bằng cách implement phương thức `compareTo()`. 
- `Comparator` cho phép bạn định nghĩa nhiều cách so sánh khác nhau giữa các đối tượng bằng cách implement `compare()`. Sử dụng Comparator khi muốn sắp xếp đối tượng theo nhiều tiêu chí.

    ```Java
    // Comparable
    class Person implements Comparable<Person> {
        int age;
        public int compareTo(Person p) { return this.age - p.age; }
    }
    // Comparator

    class Student {
    String name;
    int age;

    // Constructor và getter/setter
    }

    class NameComparator implements Comparator<Student> {
        @Override
        public int compare(Student s1, Student s2) {
            return s1.getName().compareTo(s2.getName());   

        }
    }
    ```

### Deadlock trong thread là gì, làm sao để ngăn chặn deadlock
- Câu trả lời: Deadlock xảy ra khi hai hay nhiều thread bị kẹt chờ lẫn nhau giữ khóa. Để ngăn chặn, có thể sử dụng các cơ chế như: sử dụng `tryLock()` với thời gian timeout, hoặc đảm bảo thứ tự khóa nhất quán.

### Mutex problem solving
- Câu trả lời: Mutex là cơ chế để đảm bảo chỉ một thread có thể truy cập tài nguyên chung tại một thời điểm. Để giải quyết vấn đề Mutex, sử dụng các kỹ thuật như synchronized block hoặc ReentrantLock trong Java.

### Thread pool trong Java?
- Câu trả lời: Thread pool là tập hợp các thread được quản lý sẵn, giúp thực thi các tác vụ mà không cần tạo thread mới mỗi khi có yêu cầu. Thread pool giúp tối ưu hiệu suất và quản lý số lượng luồng trong ứng dụng.

### String pool trong Java?
- Câu trả lời: String pool là tập hợp các chuỗi được lưu trữ và quản lý trong bộ nhớ heap. Khi tạo một chuỗi mới bằng cách khai báo trực tiếp, Java sẽ kiểm tra xem chuỗi đó có trong String pool hay chưa, nếu có, sẽ dùng lại thay vì tạo mới, giúp tiết kiệm bộ nhớ.

    ```Java
    String s1 = "Hello";
    String s2 = "Hello";  // s1 và s2 trỏ đến cùng một đối tượng trong String pool
    ```

### Nêu khái niệm Immutable và Mutable?
- Câu trả lời: Immutable (bất biến) là đối tượng không thể thay đổi sau khi đã được tạo ra, ví dụ như String trong Java. Mutable (có thể thay đổi) là các đối tượng có thể sửa đổi được sau khi khởi tạo, ví dụ như StringBuilder.

### Nêu hiểu biết về Heap và Stack?
- Câu trả lời: Heap là vùng nhớ được sử dụng để lưu trữ các đối tượng động (new), trong khi Stack là vùng nhớ dùng để lưu trữ các biến cục bộ và lời gọi hàm theo cơ chế LIFO (Last In, First Out). Stack có kích thước cố định, trong khi heap có thể thay đổi.

### OOP là gì? Các tính chất của OOP?
- Câu trả lời: OOP (Object-Oriented Programming) là lập trình hướng đối tượng, gồm 4 tính chất chính: Encapsulation (đóng gói), Inheritance (kế thừa), Polymorphism (đa hình), và Abstraction (trừu tượng hóa).

### Access modifier trong Java?
- Câu trả lời: Access modifier xác định phạm vi truy cập của các thành phần trong lớp. Các mức gồm: `public`, `protected`, `default` (gói) và `private`. `public` cho phép truy cập ở mọi nơi, `private` chỉ trong chính lớp đó, `protected` truy cập trong cùng package và các lớp con, trong khi `default` chỉ cho phép truy cập trong cùng package.

### Final, finally, finalize trong Java?
- `final` là từ khóa dùng để khai báo biến không thay đổi, phương thức không override, và lớp không kế thừa. 
- `finally` là khối mã luôn được thực thi sau try-catch, dùng để dọn dẹp tài nguyên. 
- `finalize()` là phương thức được gọi trước khi đối tượng bị garbage collector thu gom.

### Optional trong Java?
- Câu trả lời: Optional là một class giới thiệu trong Java 8, giúp tránh lỗi NullPointerException bằng cách bọc giá trị có thể null và cung cấp các phương thức để xử lý trường hợp giá trị có hoặc không tồn tại.

    ```Java
    Optional<String> name = Optional.ofNullable(null);
    name.ifPresent(System.out::println);  // không in ra gì vì giá trị null
    ```


### Giải thích về Generics trong Java

Generics là một tính năng trong Java cho phép các lớp, interface và phương thức hoạt động với các kiểu dữ liệu tham số hóa. Thay vì làm việc với các kiểu cụ thể, bạn có thể sử dụng các tham số kiểu để viết mã chung hơn và có tính linh hoạt cao hơn.

**Lợi ích của Generics:**
- **Type Safety**: Giúp phát hiện lỗi kiểu dữ liệu tại thời điểm biên dịch.
- **Code Reusability**: Tạo ra các lớp và phương thức có thể dùng lại cho nhiều kiểu dữ liệu khác nhau mà không cần viết lại mã.
- **Avoiding Type Casting**: Loại bỏ việc ép kiểu thủ công, vì kiểu dữ liệu đã được xác định trước tại compile time.

Ví dụ về Generics:

```Java
// Ví dụ về sử dụng Generics trong lớp
public class Box<T> {
    private T value;

    public void set(T value) {
        this.value = value;
    }

    public T get() {
        return value;
    }
}

```

### Tại sao lại sử dụng final?

Khi một lớp được đánh dấu là final, không lớp nào khác có thể kế thừa từ lớp đó. Điều này đảm bảo rằng lớp của bạn sẽ không bị thay đổi thông qua kế thừa và hành vi của nó sẽ được bảo toàn

- `Bảo vệ tính toàn vẹn của lớp`: Ngăn chặn việc các lớp con vô tình hoặc cố ý thay đổi hành vi của lớp cha.
- `Tăng hiệu suất`: Đôi khi, trình biên dịch có thể tối ưu hóa mã của các lớp `final` tốt hơn.
- `Làm rõ ý định thiết kế`: Cho biết rõ rằng lớp này không được thiết kế để làm lớp cơ sở cho các lớp khác.

### Phân biệt giữa Checked và Unchecked Exceptions.

| **Tiêu chí**                    | **Checked Exceptions**                                  | **Unchecked Exceptions**                                |
|----------------------------------|---------------------------------------------------------|---------------------------------------------------------|
| **Kiểm tra tại**                 | Được kiểm tra tại thời điểm biên dịch (compile-time)    | Được kiểm tra tại thời điểm runtime                     |
| **Lớp cha**                      | Kế thừa từ `java.lang.Exception` (ngoại trừ `RuntimeException`) | Kế thừa từ `java.lang.RuntimeException`                 |
| **Ví dụ**                        | `IOException`, `SQLException`, `FileNotFoundException`  | `NullPointerException`, `ArrayIndexOutOfBoundsException`, `ArithmeticException` |
| **Bắt buộc xử lý**               | Phải được xử lý hoặc khai báo trong phương thức với `throws` | Không bắt buộc phải xử lý hoặc khai báo                 |
| **Mục đích**                     | Dùng cho các lỗi có thể dự đoán và khắc phục được       | Dùng cho các lỗi lập trình, thường là lỗi không thể dự đoán hoặc khó khắc phục |
| **Xử lý**                        | Phải sử dụng `try-catch` hoặc khai báo `throws`         | Không yêu cầu bắt buộc sử dụng `try-catch`               |
| **Thời điểm phát sinh**          | Thường xảy ra do các điều kiện ngoại cảnh như tệp tin không tồn tại hoặc lỗi mạng | Thường xảy ra do các lỗi lập trình, chẳng hạn như chia cho 0 hoặc truy cập mảng ngoài giới hạn |

### Khi nào thì sử dụng extends và super trong Generics?

| **Tiêu chí**          | **`extends`**                                              | **`super`**                                               |
|-----------------------|-------------------------------------------------------------|------------------------------------------------------------|
| **Sử dụng khi**       | Để khai báo một kiểu giới hạn trên hoặc dưới một lớp hoặc interface  | Để khai báo một kiểu có thể là lớp cha của một lớp cụ thể |
| **Ký hiệu**           | `<? extends T>`: Giới hạn kiểu đối tượng là một lớp con của `T` hoặc là chính `T` | `<? super T>`: Giới hạn kiểu đối tượng là một lớp cha của `T` hoặc là chính `T` |
| **Đọc dữ liệu**       | Có thể đọc dữ liệu từ danh sách nhưng chỉ có thể đọc dữ liệu dưới dạng `T` hoặc `T` (tức là không biết chính xác kiểu dữ liệu) | Có thể đọc dữ liệu dưới dạng `Object` vì không biết chính xác kiểu dữ liệu  |
| **Ghi dữ liệu**       | Không thể ghi dữ liệu vào danh sách vì không biết chính xác kiểu dữ liệu | Có thể ghi dữ liệu vào danh sách (đặc biệt khi kiểu cụ thể là `T`) |
| **Ví dụ sử dụng**     | `List<? extends Number>`: Có thể chứa `Integer`, `Double`, v.v. nhưng không thể thêm bất kỳ giá trị nào vào danh sách này | `List<? super Integer>`: Có thể chứa `Integer` và các lớp cha của `Integer`, có thể thêm `Integer` vào danh sách này |


### Sự khác biệt giữa `List<Object>` và `List<?>` là gì? 

| **Tiêu chí**                       | **`List<Object>`**                                        | **`List<?>`**                                             |
|------------------------------------|---------------------------------------------------------|---------------------------------------------------------|
| **Khai báo**                       | `List<Object>`: Danh sách có thể chứa bất kỳ loại đối tượng nào | `List<?>`: Danh sách có thể chứa bất kỳ loại đối tượng nào, nhưng loại đối tượng cụ thể không được biết |
| **Thêm phần tử**                  | Có thể thêm bất kỳ loại đối tượng nào                  | Không thể thêm phần tử vào danh sách vì không biết kiểu đối tượng cụ thể |
| **Truy cập phần tử**              | Có thể truy cập và thao tác với các phần tử như `Object` | Có thể đọc các phần tử, nhưng không thể thực hiện thao tác cụ thể (phải cast về kiểu cụ thể) |
| **Lấy kiểu đối tượng**            | Có thể lấy kiểu đối tượng cụ thể từ `List<Object>`     | Không thể lấy kiểu đối tượng cụ thể từ `List<?>`     |
| **Sử dụng với Generics**           | Có thể sử dụng các phương thức generics với `List<Object>` | Thường dùng trong trường hợp generic wildcard (`List<?>`) để viết mã linh hoạt |
| **Khả năng mở rộng**               | Có thể mở rộng thành `List<String>`, `List<Integer>`, v.v. | Không thể mở rộng thành các kiểu cụ thể vì kiểu đối tượng không được biết |
| **Ví dụ sử dụng**                  | `List<Object> list = new ArrayList<>();`                | `List<?> list = new ArrayList<String>();`              |

### Serializable là gì và làm sao để ngăn không cho một trường trong object bị serializable?
Serializable trong Java là một cơ chế cho phép bạn chuyển đổi một đối tượng thành một luồng byte và ngược lại. Điều này rất hữu ích cho việc lưu trữ, truyền tải dữ liệu hoặc phân tán đối tượng. Tuy nhiên, đôi khi bạn không muốn tất cả các trường trong một đối tượng được serializable.


Có hai cách chính để ngăn chặn một trường trong object bị serializable: 

- Sử dụng keyword `transient`:
    - Cách thức: Đặt từ khóa transient trước khai báo trường.
    - Khi bạn chỉ muốn đơn giản loại bỏ một số trường khỏi quá trình serializable.
    - Khi không cần kiểm soát quá trình serializable một cách phức tạp.
- Sử dụng custom serialization:
    - Cách thức: Viết lại các phương thức writeObject và readObject của lớp.
    - Khi bạn cần kiểm soát chặt chẽ quá trình serializable.
    - Khi bạn muốn thực hiện các logic phức tạp trước hoặc sau khi serializable.

### Sự khác biệt giữa Overloading và Overriding là gì?

| **Tiêu chí**                  | **Overloading (Nạp chồng phương thức)**                 | **Overriding (Ghi đè phương thức)**                     |
|-------------------------------|----------------------------------------------------------|----------------------------------------------------------|
| **Mục đích**                  | Định nghĩa nhiều phương thức cùng tên nhưng khác tham số | Cung cấp một triển khai cụ thể cho phương thức của lớp cha|
| **Lớp**                       | Có thể xảy ra trong cùng một lớp hoặc các lớp con        | Xảy ra giữa lớp cha và lớp con                            |
| **Tham số**                   | Các phương thức phải có danh sách tham số khác nhau      | Phương thức ghi đè phải có danh sách tham số giống hệt phương thức của lớp cha |
| **Kiểu trả về**               | Có thể khác nhau                                         | Phải giống hệt hoặc kiểu trả về tương thích với lớp cha   |
| **Truy cập**                  | Có thể có các mức truy cập khác nhau                     | Mức truy cập phải giống hoặc rộng hơn (không thể thu hẹp) |
| **Thời điểm quyết định**       | Quyết định tại thời điểm biên dịch (compile-time)        | Quyết định tại thời điểm runtime                          |
| **Tính đa hình (Polymorphism)**| Không hỗ trợ đa hình                                     | Hỗ trợ đa hình (runtime polymorphism)                     |
| **Annotation**                | Không yêu cầu sử dụng bất kỳ annotation nào              | Thường sử dụng `@Override` để chỉ rõ ghi đè               |
| **Ví dụ**                     | `void add(int a, int b)` và `void add(double a, double b)` | `class Child extends Parent { @Override void display() }`  |


### Fail-fast là gì?

Fail-fast là một khái niệm trong lập trình và thiết kế hệ thống, có nghĩa là hệ thống sẽ báo cáo lỗi ngay lập tức khi phát hiện ra vấn đề tiềm ẩn, thay vì cố gắng tiếp tục hoạt động và có thể dẫn đến lỗi nghiêm trọng hơn.

Trong lập trình:

- Iterator fail-fast: Một iterator sẽ ném ra ConcurrentModificationException nếu phát hiện ra rằng cấu trúc dữ liệu được nó duyệt đang bị thay đổi bởi một luồng khác. Điều này giúp tránh các lỗi đồng thời.
- Fail-fast assertion: Một số ngôn ngữ lập trình có cơ chế kiểm tra điều kiện trước khi thực thi một đoạn code. Nếu điều kiện không thỏa mãn, chương trình sẽ dừng ngay lập tức với một thông báo lỗi.


### HashMap và Hashtable khác nhau như thế nào?

| **Tiêu chí**                     | **HashMap**                                           | **Hashtable**                                         |
|-----------------------------------|-------------------------------------------------------|-------------------------------------------------------|
| **Đồng bộ (Synchronization)**    | Không đồng bộ, không an toàn khi chạy đa luồng        | Đồng bộ, an toàn khi chạy đa luồng                    |
| **Hiệu suất**                    | Nhanh hơn do không có cơ chế đồng bộ                  | Chậm hơn do cơ chế đồng bộ                             |
| **Cho phép phần tử null**        | Cho phép một `null` key và nhiều `null` values        | Không cho phép `null` key và `null` values            |
| **Kế thừa từ**                   | `AbstractMap`                                         | `Dictionary`                                          |
| **Giới thiệu từ phiên bản**      | Java 1.2                                              | Java 1.0                                              |
| **Iterator**                     | Sử dụng `Iterator`, fail-fast                         | Sử dụng `Enumerator`, không fail-fast                 |
| **Mức độ an toàn**               | Không an toàn khi chạy đa luồng (cần sử dụng `Collections.synchronizedMap()` để đồng bộ nếu cần) | An toàn khi chạy đa luồng nhờ tính năng đồng bộ sẵn   |
| **Ưu tiên sử dụng**              | Thường được sử dụng trong các ứng dụng đơn luồng hoặc khi không yêu cầu đồng bộ | Sử dụng trong các ứng dụng yêu cầu đồng bộ dữ liệu    |
| **Thay thế**                     | Không bị thay thế                                     | Dần bị thay thế bởi `HashMap` vì hiệu suất cao hơn     |

### So sánh nhanh List, Set và Map.

| **Tính năng** | 	**List** |	**Set**  |	**Map** |
|---------------|------------|------------|---------|
| **Thứ tự**	| Có	| Không | 	Không |
| **Phần tử trùng lặp** | 	Cho phép |	Không cho phép| Không cho phép (key)|
| **Truy cập**	|Bằng chỉ số| Bằng iterator	| Bằng key | 
| **Sử dụng** | Danh sách các phần tử có thứ tự |	Tập hợp các phần tử duy nhất |	Từ điển, cấu hình|


### Runnable và Callable khác nhau như thế nào?


Runnable và Callable là hai interface trong Java được sử dụng để định nghĩa các tác vụ thực thi trong các thread riêng biệt. Tuy nhiên, chúng có những điểm khác biệt quan trọng về kiểu trả về và cách xử lý ngoại lệ.

| **Tiêu chí**                    | **Runnable**                                           | **Callable**                                         |
|----------------------------------|--------------------------------------------------------|------------------------------------------------------|
| **Giới thiệu từ phiên bản**      | Java 1.0                                               | Java 1.5                                             |
| **Giao diện (Interface)**        | `java.lang.Runnable`                                   | `java.util.concurrent.Callable`                      |
| **Phương thức chính**            | `void run()`                                           | `V call()`                                           |
| **Kiểu trả về**                  | Không trả về giá trị (kiểu `void`)                     | Trả về một giá trị tổng quát (generic type `V`)      |
| **Ngoại lệ**                     | Không thể ném ngoại lệ checked                         | Có thể ném ngoại lệ checked                          |
| **Sử dụng với ExecutorService**  | Sử dụng với `ExecutorService.submit(Runnable)`         | Sử dụng với `ExecutorService.submit(Callable)`       |
| **Kết quả trả về**               | Không trả về kết quả                                   | Có thể trả về kết quả và nhận được qua `Future`      |
| **Ưu tiên sử dụng**              | Khi không cần trả về kết quả sau khi thực thi          | Khi cần trả về kết quả hoặc ném ngoại lệ              |
| **Ví dụ sử dụng**                | `Runnable task = () -> System.out.println("Running");` | `Callable<Integer> task = () -> { return 123; };`    |

Lựa chọn giữa Runnable và Callable phụ thuộc vào yêu cầu cụ thể của bài toán. Nếu bạn chỉ cần thực hiện một tác vụ đơn thuần, Runnable là đủ. Tuy nhiên, nếu bạn cần lấy kết quả của một tác vụ hoặc muốn xử lý các ngoại lệ một cách chi tiết, Callable là lựa chọn tốt hơn.

```Java
// Code sample
// Runnable
Runnable task = () -> {
    System.out.println("Hello from Runnable!");
};

// Callable
Callable<String> taskWithResult = () -> {
    return "Hello from Callable!";
};

ExecutorService executor = Executors.newSingleThreadExecutor();
Future<String> future = executor.submit(taskWithResult);
try {
    String result = future.get();
    System.out.println(result);
} catch (InterruptedException | ExecutionException e) {
    e.printStackTrace();
}
```


### Garbage Collection là gì và cách thức hoạt động của nó trong Java?

`Garbage Collection (GC)` là một quá trình tự động trong Java nhằm giải phóng bộ nhớ được cấp phát cho các đối tượng không còn được sử dụng nữa. Điều này giúp ngăn chặn tình trạng rò rỉ bộ nhớ và đảm bảo hiệu suất của ứng dụng.

Cơ chế hoạt động của Garbage Collection
1. Đánh dấu và quét (Mark and Sweep):

Đánh dấu: GC bắt đầu bằng việc đánh dấu tất cả các đối tượng có thể truy cập được từ các biến tham chiếu đang hoạt động (ví dụ: biến cục bộ, biến thành viên của đối tượng).
Quét: Sau đó, GC quét qua toàn bộ heap (vùng nhớ chứa các đối tượng). Các đối tượng không được đánh dấu sẽ bị coi là "rác" và được thu hồi bộ nhớ.
2. Phân vùng:

- Heap được chia thành các vùng khác nhau (young generation, old generation, permanent generation).
- Các đối tượng mới được tạo ra thường được đặt vào young generation.
- Các đối tượng sống sót qua nhiều lần GC sẽ được di chuyển vào old generation.

3. Các thuật toán GC:
- Serial GC: Thuật toán đơn giản, dừng tất cả các luồng để thực hiện GC.
- Parallel GC: Sử dụng nhiều luồng để thực hiện GC, tăng hiệu suất.
- Concurrent Mark Sweep (CMS): Thực hiện GC song song với các luồng ứng dụng, giảm thời gian dừng ứng dụng.
- G1 GC: Chia heap thành các vùng nhỏ, linh hoạt hơn trong việc chọn vùng để thu thập rác.

Khi nào Garbage Collection được kích hoạt?
- Khi heap đầy: Khi không còn đủ không gian để tạo ra các đối tượng mới.
- Gọi System.gc(): Mặc dù không đảm bảo rằng GC sẽ được thực hiện ngay lập tức, nhưng nó sẽ gợi ý cho JVM thực hiện GC.

Điều bạn cần biết về Garbage Collection
- Không đảm bảo thời gian: Bạn không thể dự đoán chính xác khi nào GC sẽ xảy ra.
- Không thể giải phóng tất cả: Một số đối tượng có thể không được giải phóng ngay lập tức, đặc biệt là các đối tượng có tham chiếu vòng tròn.
- Ảnh hưởng đến hiệu suất: Quá trình GC có thể gây ra gián đoạn trong ứng dụng, đặc biệt là khi heap lớn hoặc GC xảy ra thường xuyên.

Cách tối ưu hóa Garbage Collection
- Tránh rò rỉ bộ nhớ: Đảm bảo rằng các đối tượng không còn được sử dụng nữa sẽ được đặt thành null để GC có thể thu hồi bộ nhớ.
- Sử dụng các đối tượng pooling: Tái sử dụng các đối tượng thay vì tạo ra các đối tượng mới liên tục.
- Điều chỉnh kích thước heap: Điều chỉnh kích thước heap phù hợp với nhu cầu của ứng dụng.
Sử dụng các thuật toán GC phù hợp: Chọn thuật toán GC phù hợp với đặc điểm của ứng dụng.


### Các giai đoạn trong vòng đời của một đối tượng
1. Tạo đối tượng:
    **Khởi tạo**: Khi bạn sử dụng từ khóa new để tạo một đối tượng, một vùng nhớ trên heap sẽ được cấp phát để lưu trữ các trường và phương thức của đối tượng đó.
    **Khởi tạo các trường**: Các trường của đối tượng được khởi tạo theo giá trị mặc định hoặc giá trị được cung cấp trong constructor.
    **Gọi constructor**: Constructor của đối tượng được gọi để thực hiện các công việc khởi tạo khác.

2. Sử dụng đối tượng:

    **Truy cập các trường**: Bạn có thể truy cập và thay đổi giá trị của các trường của đối tượng.
    **Gọi các phương thức**: Bạn có thể gọi các phương thức của đối tượng để thực hiện các hành động.
    **Truyền tham chiếu**: Bạn có thể truyền tham chiếu đến đối tượng cho các đối tượng khác.

3. Hủy đối tượng:

    **Không còn tham chiếu**: Khi không còn bất kỳ tham chiếu nào trỏ đến đối tượng, đối tượng đó được coi là rác.
    **Garbage Collector**: Bộ thu rác (Garbage Collector) của JVM sẽ tự động tìm và thu hồi các đối tượng rác này.
    **Finalize**: Trước khi đối tượng bị thu hồi, phương thức finalize() (nếu có) của đối tượng sẽ được gọi một lần. Tuy nhiên, không nên dựa vào finalize() để giải phóng tài nguyên, vì nó không được đảm bảo sẽ được gọi.

``` Java
public class Person {
    String name;
    int age;

    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }

    // Các phương thức khác
}

public class Main {
    public static void main(String[] args) {
        Person person = new Person("John Doe", 30); // Tạo đối tượng
        person = null; // Không còn tham chiếu đến đối tượng
        // Garbage Collector sẽ thu hồi đối tượng person
    }
}
```

**Lưu ý quan trọng**
- **Tham chiếu**: Trong Java, bạn không làm việc trực tiếp với đối tượng mà thông qua các tham chiếu. Tham chiếu là một biến chứa địa chỉ của đối tượng trên heap.
- **Garbage Collector**: Garbage Collector là một tiến trình tự động, bạn không thể kiểm soát chính xác thời điểm nó hoạt động.
- **Rò rỉ bộ nhớ**: Nếu có quá nhiều đối tượng không được giải phóng, có thể dẫn đến tình trạng rò rỉ bộ nhớ.
- **Finalize**: Không nên dựa vào finalize() để giải phóng tài nguyên, hãy sử dụng các cơ chế quản lý tài nguyên khác như try-with-resources hoặc các khối finally.
