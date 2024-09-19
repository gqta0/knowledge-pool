# Singleton

`Singleton` là một kiểu thiết kế đảm bảo rằng một lớp chỉ có duy nhất một đối tượng được tạo ra trong suốt quá trình thực thi chương trình. Điều này rất hữu ích khi chúng ta cần có một điểm truy cập duy nhất đến một nguồn tài nguyên hoặc khi chúng ta muốn đảm bảo rằng chỉ có một phiên bản của một đối tượng nào đó tồn tại.

### Cách thực hiện:

- **Private constructor**: Constructor của lớp được khai báo là private để ngăn chặn việc tạo đối tượng từ bên ngoài.
- **Static field**: Một trường static chứa tham chiếu đến đối tượng duy nhất của lớp.
- **Static method**: Một phương thức static trả về đối tượng duy nhất đó. Nếu đối tượng chưa được tạo, phương thức này sẽ tạo ra nó và lưu trữ vào trường static.

### Ví dụ

``` Java

public class Singleton {
    private static Singleton instance;
    private Singleton() {}

    public static Singleton getInstance() {
        if (instance == null) {
            instance = new Singleton();
        }
        return   
 instance;
    }
}

```


### Ứng dụng:

- **Logger**: Một lớp logger thường được thiết kế theo kiểu Singleton để đảm bảo tất cả các log đều được ghi vào một file duy nhất.
- **Configuration manager**: Một lớp quản lý cấu hình toàn cục của ứng dụng.
- **Database connection pool**: Một pool kết nối đến cơ sở dữ liệu, đảm bảo không tạo quá nhiều kết nối.

