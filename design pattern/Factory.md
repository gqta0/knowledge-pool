# Factory Pattern

`Factory` là một kiểu thiết kế cung cấp một interface để tạo ra các đối tượng, nhưng lại để cho các lớp con quyết định sẽ tạo ra đối tượng nào. Điều này giúp tách rời quá trình tạo đối tượng khỏi việc sử dụng đối tượng.

### Cách thực hiện:

- **Factory interface**: Định nghĩa một interface với một phương thức tạo ra đối tượng.
- **Concrete factory**: Các lớp thực hiện interface này và quyết định loại đối tượng nào sẽ được tạo ra.
- **Client**: Client chỉ cần gọi phương thức trong interface factory mà không cần biết chi tiết về việc tạo đối tượng.

Ví dụ

``` Java

interface Shape {
    void draw();
}

class Circle implements Shape {
    @Override
    public void draw() {
        // Vẽ hình tròn
    }
}

class Rectangle implements Shape {
    @Override
    public void draw() {
        // Vẽ hình chữ nhật
    }
}

enum ShapeType {
    CIRCLE,
    RECTANGLE;
}

class ShapeFactory {
    public Shape getShape(ShapeType shapeType) {
        switch (shapeType) {
            case CIRCLE:
                return new Circle();
            case RECTANGLE:
                return new Rectangle();
        }
    }
}

```

### Ứng dụng:

- **Tạo các đối tượng phức tạp**: Factory pattern giúp đơn giản hóa quá trình tạo các đối tượng có nhiều tham số hoặc logic khởi tạo phức tạp.
- **Tách rời việc tạo đối tượng khỏi việc sử dụng**: Điều này giúp code dễ bảo trì và mở rộng hơn.