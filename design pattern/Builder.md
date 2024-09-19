# Builder Pattern

`Builder Pattern` là một mẫu thiết kế tạo ra các đối tượng phức tạp từng bước một, cho phép bạn kiểm soát quá trình xây dựng và tạo ra các đối tượng khác nhau từ cùng một quá trình xây dựng.

### Khi nào sử dụng:

Khi bạn cần tạo ra các đối tượng phức tạp với nhiều thuộc tính.
Khi bạn muốn tách rời việc xây dựng một đối tượng khỏi biểu diễn của nó.
Khi bạn muốn tạo ra các đối tượng không thể thay đổi sau khi được tạo ra.

### Cấu trúc:

Product: Định nghĩa một interface cho đối tượng được tạo ra.
Builder: Định nghĩa một interface để tạo ra các phần khác nhau của một Product.
Concrete Builder: Thực hiện interface Builder và tạo ra các phần cụ thể của Product.
Director/Client: Chỉ đạo việc xây dựng một Product bằng cách sử dụng một Concrete Builder.

### Ví dụ:

``` Java
package inf;

import java.util.ArrayList;
import java.util.List;

// Product
interface Person{
  String getName();
  int getAge();
  String getJob();
}



// Builder and Concret
class PersonBuilder {
  private String name;
  private int age;
  private String job;

  public PersonBuilder setName(String name) {
    this.name = name;
    return this;
  }

  public PersonBuilder setAge(int age) {
    this.age = age;
    return this;
  }

  public PersonBuilder setJob(String job) {
    this.job = job;
    return this;
  }

  public Person build() {
    return new PersonImpl(name, age, job);
  }
}

class PersonImpl implements Person {
  private final String name;
  private final int age;
  private final String job;

  public PersonImpl(String name, int age, String job) {
    this.name = name;
    this.age = age;
    this.job = job;
  }

  @Override
  public String getName() {
    return name;
  }

  @Override
  public int getAge() {
    return age;
  }

  @Override
  public String getJob() {
    return job;
  }
}

// Person là interface cho đối tượng được tạo ra.
// PersonBuilder là builder and ConcreteBuilder, cung cấp các phương thức để xây dựng từng phần của một Person.
// PersonImpl là triển khai cụ thể của Person.
// Main là director, chỉ đạo việc xây dựng một Person bằng cách sử dụng PersonBuilder.

public class Main {
  public static void main(String[] args) {
    // Tạo một người trưởng thành đi làm
    Person adult = new PersonBuilder()
        .setName("Alice")
        .setAge(35)
        .setJob("Project Manager").build();

    System.out.println(adult.getName() + " is " + adult.getAge() + " years old and works as a " + adult.getJob());

    // Tạo một đứa trẻ đi học
    Person child = new PersonBuilder()
        .setName("Bob")
        .setAge(10)
        .setJob("Student").build();

    System.out.println(child.getName() + " is " + child.getAge() + " years old and is a " + child.getJob());
  }
}

```

### Ưu điểm của Builder Pattern:

- Tách rời việc xây dựng một đối tượng khỏi biểu diễn của nó.
- Cho phép tạo ra các đối tượng phức tạp từng bước một.
- Hỗ trợ việc tạo ra các đối tượng không thể thay đổi sau khi được tạo ra.
- Dễ dàng mở rộng để tạo ra các loại đối tượng khác nhau.
- Giảm bớt số lượng constructor, không cần truyền giá trị null cho các tham số không sử dụng.
- Ít bị lỗi do việc gán sai tham số khi mà có nhiều tham số trong constructor: bởi vì người dùng đã biết được chính xác giá trị gì khi gọi phương thức tương ứng.

###  Nhược điểm của Builder Pattern:

- Builder Pattern có nhược điểm là duplicate code khá nhiều: do cần phải copy tất cả các thuộc tính từ class Product sang class Builder.

- Tăng độ phức tạp của code (tổng thể) do số lượng class tăng lên.

### Builder lombok có được xây dựng trên Builder Pattern hay không?
`Lombok @Builder` được xây dựng dựa trên nguyên tắc của Builder Pattern. Tuy nhiên, nó cung cấp một cách thức đơn giản và tự động hóa hơn để tạo ra các builder trong Java.