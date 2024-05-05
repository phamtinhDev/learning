# Factory Method

1. **Giới thiệu**

**Factory Method** là một design pattern cung cấp giao diện để tạo các objects trong superclass nhưng cho phép các subclasses thay đổi loại đối tượng sẽ được tạo.

![](assets/20240505_232730_factory-method-en.png)

2. **Vấn đề**

Hãy tưởng tượng bạn đang tạo một ứng dụng quản lý logistics. Phiên bản đầu tiên của ứng dụng của bạn chỉ có thể xử lý việc vận chuyển bằng xe tải, vì vậy phần lớn mã của bạn nằm trong class `Truck`.

Sau một thời gian, ứng dụng của bạn sẽ trở nên khá phổ biến. Mỗi ngày bạn nhận được hàng chục yêu cầu từ các công ty vận tải biển về việc đưa dịch vụ logistics đường biển vào ứng dụng.

![](assets/20240505_232800_problem1-en.png)

Tin tuyệt vời, phải không? Nhưng còn code của bạn thì sao? Hiện tại, hầu hết code của bạn đã được ghép nối với class `Truck`. Việc thêm `Ships`vào ứng dụng sẽ yêu cầu thực hiện các thay đổi đối với toàn bộ cơ sở mã. Hơn nữa, nếu sau này bạn quyết định thêm một loại phương tiện giao thông khác vào ứng dụng, có thể bạn sẽ cần phải thực hiện lại tất cả những thay đổi này.

Kết quả là, bạn sẽ có được một đoạn code khá khó chịu, chứa đầy các điều kiện chuyển đổi hành vi của ứng dụng tùy thuộc vào loại đối tượng vận chuyển.

3. **Giải pháp**

Factory Method patterngợi ý rằng bạn thay thế các lệnh gọi xây dựng đối tượng trực tiếp (sử dụng toán tử `new`) bằng các lệnh gọi đến một phương thức *factory* đặc biệt . Đừng lo lắng: các đối tượng vẫn được tạo thông qua toán tử `new`, nhưng nó được gọi từ bên trong phương thức factory. Các đối tượng được trả về bằng phương thức Factory thường được gọi là *products*.

![](assets/20240505_232819_solution1.png)

Thoạt nhìn, sự thay đổi này có vẻ vô nghĩa: chúng ta chỉ chuyển lệnh gọi hàm tạo từ phần này sang phần khác của chương trình. Tuy nhiên, hãy xem xét điều này: bây giờ bạn có thể ghi đè phương thức factory trong một subclass và thay đổi lớp products được tạo bằng phương thức này.

Tuy nhiên, có một hạn chế nhỏ: các subclasses chỉ có thể trả về các loại products khác nhau nếu các products này có base class hoặc interface. Ngoài ra, phương thức factory trong base class phải có kiểu trả về được khai báo là interface này.

![](assets/20240505_232847_solution2-en.png)

Ví dụ: cả hai `Truck`và `Ship`các class đều phải triển khai `Transport`interface, khai báo một phương thức có tên là `deliver`. Mỗi lớp thực hiện phương pháp này một cách khác nhau: xe tải giao hàng bằng đường bộ, tàu chở hàng bằng đường biển. Phương thức Factory trong class `RoadLogistics` trả về các object truck, trong khi phương thức Factory trong class `SeaLogistics` trả về object ship.

![](assets/20240505_232911_solution3-en.png)

Code sử dụng phương thức factory (thường được gọi là mã *client* ) không thấy sự khác biệt giữa các products thực tế được trả về bởi các subclasses khác nhau. Client coi tất cả các sản phẩm là abstract `Transport`. Client biết rằng tất cả các object vận chuyển đều phải có phương thức `deliver` này, nhưng chính xác cách thức hoạt động của nó không quan trọng đối với client.

4. **Cấu trúc**

![](assets/20240505_232954_structure-indexed.png)

* **Product** khai báo interface chung cho tất cả các objects có thể được tạo ra bởi người tạo và các subclasses của nó.
* **Concrete Products** là các triển khai khác nhau của product interface.
* Class **Creator** khai báo phương thức factory trả về các new product object. Điều quan trọng là kiểu trả về của phương thức này phù hợp với product interface.
  Bạn có thể khai báo factory method để `abstract`buộc tất cả các subclasses triển khai các phiên bản phương thức của riêng chúng. Thay vào đó, base factory method có thể trả về một số loại product mặc định.
  * Lưu ý, mặc dù có tên như vậy nhưng việc tạo ra product **không phải** là trách nhiệm chính của người tạo. Thông thường, class được tạo đã có sẵn một số logic nghiệp vụ cốt lõi liên quan đến product. factory method giúp tách logic này khỏi các class product cụ thể. Đây là một ví dụ tương tự: một công ty phát triển phần mềm lớn có thể có bộ phận đào tạo lập trình viên. Tuy nhiên, chức năng chính của toàn bộ công ty vẫn là viết mã chứ không phải sản xuất lập trình viên.
* **Concrete Creators** ghi đè base factory method để nó trả về một loại sản phẩm khác.
  * Lưu ý rằng factory method không nhất thiết phải **tạo** phiên bản mới mọi lúc. Nó cũng có thể trả về các đối tượng hiện có từ bộ đệm, nhóm đối tượng hoặc nguồn khác.

5. Mã giả

Ví dụ này minh họa cách **Factory Method** có thể được sử dụng để tạo các phần tử giao diện người dùng đa nền tảng mà không cần ghép mã máy khách với các lớp giao diện người dùng cụ thể.

![](assets/20240505_234232_example.png)

* Class base `Dialog`sử dụng các thành phần UI khác nhau để hiển thị cửa sổ của nó. Trong các hệ điều hành khác nhau, các thành phần này có thể trông hơi khác một chút nhưng chúng vẫn phải hoạt động nhất quán. Một button trong Windows vẫn là một button trong Linux.
* Khi factory method phát huy tác dụng, bạn không cần phải viết lại logic của class `Dialog` cho từng hệ điều hành. Nếu chúng ta khai báo một factory method tạo ra các button bên trong class base `Dialog`, thì sau này chúng ta có thể tạo một subclass trả về các button kiểu Windows từ factory method. Sau đó, subclass kế thừa hầu hết mã từ class base, nhưng nhờ factory method, có thể hiển thị các button trông giống Windows trên màn hình.
* Để pattern này hoạt động, class base `Dialog` phải hoạt động với các button abstract: một class base hoặc một interface mà tất cả các button đều tuân theo. Bằng cách này, code bên trong `Dialog`vẫn hoạt động, cho dù nó hoạt động với loại button nào.
* Tất nhiên, bạn cũng có thể áp dụng phương pháp này cho các thành phần giao diện người dùng khác. Tuy nhiên, với mỗi factory method mới mà bạn thêm vào `Dialog`, bạn sẽ tiến gần hơn đến pattern [Abstract Factory](https://refactoring.guru/design-patterns/abstract-factory) . Đừng lo, chúng ta sẽ nói về pattern này sau.

```javascript
// Class tạo ra (Creator) khai báo factory method và phải trả về một object của class product.
// Các lớp con của Creator thường cung cấp thực thi của phương thức này.
abstract class Dialog {
    // Creator cũng có thể cung cấp một số thực thi mặc định của phương thức factory.
    abstract createButton(): Button;

    // Lưu ý rằng, mặc dù tên của nó, trách nhiệm chính của Creator không phải là tạo ra sản phẩm.
    // Nó thường chứa một số logic kinh doanh cốt lõi dựa trên các đối tượng sản phẩm trả về bởi phương thức factory.
    // Các lớp con có thể thay đổi gián tiếp logic kinh doanh này bằng cách ghi đè phương thức factory và trả về một loại sản phẩm khác từ đó.
    render(): void {
        // Gọi phương thức factory để tạo một đối tượng sản phẩm.
        const okButton: Button = this.createButton();
        // Bây giờ sử dụng sản phẩm.
        okButton.onClick(this.closeDialog);
        okButton.render();
    }

    closeDialog(): void {
        console.log("Dialog closed.");
    }
}

// Các lớp tạo ra cụ thể ghi đè phương thức factory để thay đổi loại sản phẩm kết quả.
class WindowsDialog extends Dialog {
    createButton(): Button {
        return new WindowsButton();
    }
}

class WebDialog extends Dialog {
    createButton(): Button {
        return new HTMLButton();
    }
}

// Giao diện sản phẩm khai báo các hoạt động mà tất cả sản phẩm cụ thể phải thực thi.
interface Button {
    render(): void;
    onClick(f: () => void): void;
}

// Các sản phẩm cụ thể cung cấp các thực thi khác nhau của giao diện sản phẩm.
class WindowsButton implements Button {
    render(): void {
        // Hiển thị một nút theo phong cách Windows.
        console.log("Rendering Windows button");
    }
    onClick(f: () => void): void {
        // Ràng buộc sự kiện click hệ thống điều hành gốc.
        console.log("Windows button clicked.");
        f();
    }
}

class HTMLButton implements Button {
    render(): void {
        // Trả về một biểu diễn HTML của một nút.
        console.log("Rendering HTML button");
    }
    onClick(f: () => void): void {
        // Ràng buộc sự kiện click trình duyệt web.
        console.log("HTML button clicked.");
        f();
    }
}

class Application {
    private dialog: Dialog;

    // Ứng dụng chọn một loại lớp tạo ra tùy thuộc vào cấu hình hoặc cài đặt môi trường hiện tại.
    initialize(config: { OS: string }): void {
        if (config.OS === "Windows") {
            this.dialog = new WindowsDialog();
        } else if (config.OS === "Web") {
            this.dialog = new WebDialog();
        } else {
            throw new Error("Error! Unknown operating system.");
        }
    }

    // Mã client làm việc với một thể hiện của lớp tạo ra cụ thể, mặc dù thông qua giao diện cơ sở của nó.
    // Miễn là client tiếp tục làm việc với lớp tạo ra qua giao diện cơ sở, bạn có thể truyền cho nó bất kỳ lớp con của lớp tạo ra nào.
    main(config: { OS: string }): void {
        this.initialize(config);
        this.dialog.render();
    }
}
```

6. **Ứng dụng**


**Hãy sử dụng Phương thức Factory khi bạn không biết trước các loại và phụ thuộc cụ thể của các đối tượng mà mã của bạn nên làm việc với.**

* Phương thức Factory tách mã xây dựng sản phẩm ra khỏi mã thực sự sử dụng sản phẩm. Do đó, việc mở rộng mã xây dựng sản phẩm trở nên dễ dàng hơn mà không phụ thuộc vào phần còn lại của mã.
* Ví dụ, để thêm một loại sản phẩm mới vào ứng dụng, bạn chỉ cần tạo một lớp con tạo mới và ghi đè phương thức factory trong đó.


**Sử dụng Phương thức Factory khi bạn muốn cung cấp cho người dùng thư viện hoặc framework của bạn một cách để mở rộng các thành phần nội bộ của nó.**

* Kế thừa có lẽ là cách đơn giản nhất để mở rộng hành vi mặc định của một thư viện hoặc framework. Nhưng làm thế nào để framework nhận ra rằng lớp con của bạn nên được sử dụng thay vì một thành phần tiêu chuẩn?
* Giải pháp là giảm mã xây dựng các thành phần trên khắp framework thành một phương thức factory duy nhất và cho phép bất kỳ ai ghi đè phương thức này đồng thời mở rộng chính thành phần đó.
* Hãy xem cách thực hiện điều đó. Hãy tưởng tượng bạn viết một ứng dụng sử dụng một framework giao diện người dùng mã nguồn mở. Ứng dụng của bạn cần có các nút tròn, nhưng framework chỉ cung cấp các nút vuông. Bạn mở rộng lớp Button tiêu chuẩn với một lớp con RoundButton tuyệt vời. Nhưng bây giờ bạn cần thông báo cho lớp UIFramework chính để sử dụng lớp con nút mới thay vì một cái mặc định.
* Để đạt được điều này, bạn tạo một lớp con UIWithRoundButtons từ lớp cơ sở của framework và ghi đè phương thức createButton của nó. Trong khi phương thức này trả về các đối tượng Button trong lớp cơ sở, bạn làm cho lớp con của mình trả về các đối tượng RoundButton. Bây giờ sử dụng lớp UIWithRoundButtons thay vì UIFramework. Và đó là tất cả!
* **Hãy sử dụng Phương thức Factory khi bạn muốn tiết kiệm tài nguyên hệ thống bằng cách tái sử dụng các đối tượng hiện có thay vì xây dựng lại chúng mỗi lần.**

  * Bạn thường gặp phải nhu cầu này khi đối mặt với các đối tượng lớn, tiêu tốn nhiều tài nguyên như kết nối cơ sở dữ liệu, hệ thống tập tin và tài nguyên mạng.
  * Hãy suy nghĩ về những gì cần làm để tái sử dụng một đối tượng hiện có:
    * Đầu tiên, bạn cần tạo một kho lưu trữ để theo dõi tất cả các đối tượng đã được tạo.
    * Khi ai đó yêu cầu một đối tượng, chương trình nên tìm kiếm một đối tượng tự do bên trong kho đó.
    * và sau đó trả lại nó cho mã client.
    * Nếu không có đối tượng tự do nào, chương trình nên tạo một đối tượng mới (và thêm nó vào kho).
  * Đó là rất nhiều mã! Và tất cả phải được đặt vào một nơi duy nhất để bạn không làm ô nhiễm chương trình với mã trùng lặp.
  * Có lẽ nơi rõ ràng và thuận tiện nhất để đặt mã này có thể là bộ khởi tạo của lớp mà chúng ta đang cố gắng tái sử dụng các đối tượng của nó. Tuy nhiên, theo định nghĩa thì một bộ khởi tạo luôn phải trả về các đối tượng mới. Nó không thể trả về các thực thể đã tồn tại.
  * Do đó, bạn cần có một phương thức thông thường có khả năng tạo đối tượng mới cũng như tái sử dụng các đối tượng hiện có. Điều đó nghe có vẻ rất giống với một phương thức factory.

7. **Cách thực hiện**

* Định nghĩa Giao diện Sản phẩm Chung

Bắt đầu bằng cách định nghĩa một giao diện mà tất cả các sản phẩm sẽ thực hiện. Giao diện này nên bao gồm tất cả các phương thức có ý nghĩa đối với mọi loại sản phẩm trong ứng dụng của bạn.

```javascript
interface Product {
    performAction(): void;
}
```

* Thêm Phương thức Factory Rỗng

Trong lớp tạo ra, thêm một phương thức factory trả về một sản phẩm. Ban đầu, phương thức này có thể chưa được triển khai (do đó, nó có thể là trừu tượng nếu bạn sử dụng một ngôn ngữ hỗ trợ phương thức trừu tượng).

```javascript
abstract class Creator {
    // Phương thức Factory
    abstract createProduct(type: string): Product;

    someOperation(): void {
        const product = this.createProduct("default");
        product.performAction();
    }
}
```

* Thay Thế Các Lời Gọi Constructor bằng Phương thức Factory

Trong mã của người tạo, tìm tất cả các tham chiếu nơi các constructor sản phẩm được gọi trực tiếp. Thay thế các lời gọi này bằng các lời gọi đến phương thức factory. Chuyển logic tạo sản phẩm vào trong phương thức factory.

```javascript
class ConcreteCreator extends Creator {
    createProduct(type: string): Product {
        switch (type) {
            case "AirMail":
                return new Plane();
            case "GroundMail":
                return new Truck(); // Mặc định cho GroundMail
            default:
                throw new Error("Unknown product type");
        }
    }
}
```

* Xử lý Các Loại Sản phẩm Khác nhau

Nếu cần, giới thiệu một tham số cho phương thức factory để điều khiển loại sản phẩm được tạo. Điều này hữu ích khi loại sản phẩm phụ thuộc vào các điều kiện bên ngoài hoặc yêu cầu cụ thể của khách hàng.

* Tái cấu trúc Phương thức Factory

Ban đầu, phương thức factory có thể chứa một câu lệnh switch hoặc if-else quyết định lớp sản phẩm nào sẽ được khởi tạo. Mặc dù điều này có thể trông cồng kềnh, nhưng nó tập trung hóa logic tạo sản phẩm.

* Tạo các Lớp Con của Người Tạo

Đối với mỗi loại sản phẩm, hãy xem xét tạo một lớp con của người tạo thay thế phương thức factory để sản xuất một loại sản phẩm khác nhau. Điều này cho phép bạn đóng gói logic tạo cho mỗi sản phẩm cụ thể trong lớp tạo ra chuyên dụng của nó.

```javascript
class AirMailCreator extends ConcreteCreator {
    createProduct(type: string): Product {
        return new Plane();
    }
}

class GroundMailCreator extends ConcreteCreator {
    createProduct(type: string): Product {
        if (type === "Train") {
            return new Train();
        } else {
            return super.createProduct(type); // Mặc định là Truck
        }
    }
}
```

* Sử dụng Phương thức Factory

Nếu logic tạo ra đủ đa dạng và đáng để triển khai các lớp con riêng biệt, hãy thực hiện chúng. Nếu không, hãy sử dụng tham số để điều chỉnh loại sản phẩm được tạo bởi phương thức factory.

* Phương thức Factory Trừu tượng hoặc Mặc định

Nếu sau khi tái cấu trúc, phương thức factory trong lớp cơ sở không thực hiện bất kỳ khởi tạo nào vì tất cả logic đã chuyển sang các lớp con, bạn có thể làm cho nó trở thành trừu tượng. Nếu vẫn còn một số logic chung, bạn có thể đặt nó làm hành vi mặc định của phương thức.

```javascript
abstract class Creator {
    abstract createProduct(type: string): Product;
    // Hành vi mặc định có thể được triển khai ở đây nếu có
}

```
