# Observer Pattern

`Observer` là một kiểu thiết kế định nghĩa một mối quan hệ một-nhiều giữa các đối tượng, trong đó khi một đối tượng thay đổi trạng thái, tất cả các đối tượng đang theo dõi nó sẽ được thông báo và tự động cập nhật.

### Cách thực hiện:

- **Subject**: Đối tượng được quan sát.
- **Observer**: Đối tượng quan sát.
- **Subject interface**: Định nghĩa các phương thức để thêm, xóa và thông báo cho các observer.
- **Observer interface**: Định nghĩa phương thức update() để cập nhật trạng thái của observer khi subject thay đổi.


### Ví dụ:

``` Java 
interface Observer {
  void update(String message);
}

interface Subject {
  void registerObserver(Observer o);
  void removeObserver(Observer o);
  void notifyObservers(String message);
}

class Inventory implements Subject {
  private List<Observer> observers = new ArrayList<>();
  private int quantity;
  

  @Override
  public void registerObserver(Observer o) {
    observers.add(o);
  }

  @Override
  public void removeObserver(Observer o) {
    observers.remove(o);
  }

  @Override
  public void notifyObservers(String message) {
    for (Observer observer : observers) {
      observer.update(message);
    }
  }

  public void setQuantity(int quantity) {
    this.quantity = quantity;
    notifyObservers("Số lượng hàng trong kho đã thay đổi");
  }


}

class SalesApp implements Observer {
  private Inventory inventory;

  public SalesApp(Inventory inventory) {
    this.inventory = inventory;
    inventory.registerObserver(this);
  }

  public void update(String message) {
    System.out.println("Ứng dụng bán hàng nhận được thông báo: " + message);
    // Cập nhật giao diện bán hàng dựa trên số lượng hàng tồn kho
  }
}

```

### Ứng dụng:

- **Event-driven programming**: Observer pattern được sử dụng rộng rãi trong các hệ thống sự kiện, chẳng hạn như GUI, các hệ thống phân tán.
- **Model-View-Controller (MVC)**: View (giao diện người dùng) đóng vai trò là observer của Model (dữ liệu). Khi Model thay đổi, View sẽ được cập nhật tự động.