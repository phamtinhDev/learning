# Builder

#### **I. Giới thiệu**

Builder là một creational design pattern cho phép bạn xây dựng các đối tượng phức tạp. Pattern này cho phép bạn tạo ra các kiểu và biểu diễn khác nhau của một object bằng cách sử dụng cùng một mã xây dựng.

#### **II. Vấn đề**

Hãy tưởng tượng một object phức tạp đòi hỏi việc khởi tạo tốn nhiều công sức, từng bước của nhiều trường và đối tượng lồng nhau. Mã khởi tạo như vậy thường được chôn bên trong một hàm tạo khổng lồ với rất nhiều tham số. Hoặc thậm chí tệ hơn: rải rác khắp mã máy khách.

![](../assets/builder/problem1.png)

Ví dụ: hãy nghĩ về cách tạo một class `House`. Để xây một ngôi nhà đơn giản, bạn cần xây bốn bức tường và một tầng, lắp một cánh cửa, lắp một cặp cửa sổ và xây một mái nhà. Nhưng điều gì sẽ xảy ra nếu bạn muốn một ngôi nhà lớn hơn, sáng sủa hơn, có sân sau và những tiện ích khác (như hệ thống sưởi, hệ thống ống nước và hệ thống dây điện)?

Giải pháp đơn giản nhất là mở rộng class `House` và tạo nhiều các class con để bao gồm tất cả các tổ hợp parameter. Nhưng cuối cùng bạn sẽ có được một số lượng lớn các class con. Bất kỳ parameter mới nào, chẳng hạn như kiểu mái hiên, sẽ yêu cầu phát triển thêm hệ thống phân cấp.

Có một cách tiếp cận khác ngoài việc tạo nhiều các class con. Bạn có thể tạo một constructor khổng lồ  trong class `House` với tất cả các tham số có thể điều khiển đối tượng ngôi nhà. Cách tiếp cận này có thể loại bỏ sự cần thiết của các class con nhưng nó lại gây ra một vấn đề khác.

![](../assets/builder/problem2.png)

Trong hầu hết các trường hợp, phần lớn các parameter sẽ không được sử dụng, khiến cho lệnh gọi constructor trở nên khá xấu. Ví dụ, chỉ một vài căn nhà mới có bể bơi, vì vậy các parameter liên quan đến bể bơi sẽ trở nên vô dụng với phần lớn ngôi nhà khác.

#### **III. Giải pháp**

Builder pattern gợi ý rằng bạn tách code của constructor ra khỏi class và di chuyển nó sang các class riêng biệt được gọi là *builders* .

![](../assets/builder/solution1.png)

Mẫu này tổ chức việc xây dựng đối tượng thành một tập hợp các bước ( `buildWalls`, `buildDoor`, v.v.). Để tạo một đối tượng, bạn thực hiện một loạt các bước này trên một object builder. Phần quan trọng là bạn không cần phải gọi tất cả các bước. Bạn chỉ có thể gọi những bước cần thiết để tạo ra một cấu hình cụ thể của một đối tượng.

Một số bước xây dựng có thể yêu cầu cách triển khai khác nhau khi bạn cần xây dựng các hình thức trình bày khác nhau của sản phẩm. Ví dụ, tường của cabin có thể được xây bằng gỗ nhưng tường của lâu đài phải được xây bằng đá.

Trong trường hợp này, bạn có thể tạo một số lớp trình xây dựng khác nhau triển khai cùng một bộ bước xây dựng nhưng theo cách khác. Sau đó, bạn có thể sử dụng các trình tạo này trong quá trình xây dựng (tức là một tập hợp lệnh gọi các bước xây dựng được sắp xếp) để tạo ra các loại đối tượng khác nhau.

![](../assets/builder/builder-comic-1-en.png)

Ví dụ: hãy tưởng tượng một người xây dựng xây dựng mọi thứ từ gỗ và thủy tinh, người thứ hai xây dựng mọi thứ bằng đá và sắt và người thứ ba sử dụng vàng và kim cương. Bằng cách gọi cùng một nhóm các bước, bạn sẽ có được một ngôi nhà thông thường từ người xây dựng đầu tiên, một lâu đài nhỏ từ người xây dựng thứ hai và một cung điện từ người xây dựng thứ ba. Tuy nhiên, điều này sẽ chỉ hoạt động nếu mã máy khách gọi các bước xây dựng có thể tương tác với các nhà xây dựng bằng giao diện chung.

**Director**

Bạn có thể đi xa hơn và trích xuất một loạt lệnh gọi đến các bước của trình tạo mà bạn sử dụng để xây dựng một sản phẩm thành một lớp riêng biệt gọi là giám đốc . Lớp giám đốc xác định thứ tự thực hiện các bước xây dựng, trong khi trình xây dựng cung cấp việc triển khai các bước đó.

![](../assets/builder/builder-comic-2-en.png)

Việc có một lớp đạo diễn trong chương trình của bạn là không thực sự cần thiết. Bạn luôn có thể gọi các bước xây dựng theo thứ tự cụ thể trực tiếp từ mã máy khách. Tuy nhiên, lớp giám đốc có thể là nơi thích hợp để đặt các quy trình xây dựng khác nhau để bạn có thể sử dụng lại chúng trong chương trình của mình.

Ngoài ra, lớp giám đốc hoàn toàn ẩn các chi tiết xây dựng sản phẩm khỏi mã máy khách. Khách hàng chỉ cần liên kết người xây dựng với giám đốc, khởi công xây dựng với giám đốc và nhận kết quả từ người xây dựng.

#### **IV. Cấu trúc**

![](../assets/builder/structure.png)

1. Giao diện Builder khai báo các bước xây dựng sản phẩm chung cho tất cả các loại trình xây dựng.
2. Concrete Builders cung cấp các cách triển khai khác nhau của các bước xây dựng. Những người xây dựng bê tông có thể tạo ra những sản phẩm không tuân theo giao diện chung.
3. Sản phẩm là đối tượng kết quả. Các sản phẩm được xây dựng bởi các nhà xây dựng khác nhau không nhất thiết phải thuộc cùng một hệ thống phân cấp hoặc giao diện lớp.
4. Lớp Director xác định thứ tự gọi các bước xây dựng để bạn có thể tạo và sử dụng lại các cấu hình cụ thể của sản phẩm.
5. Khách hàng phải liên kết một trong các đối tượng người xây dựng với giám đốc. Thông thường, nó chỉ được thực hiện một lần, thông qua các tham số của hàm tạo của giám đốc. Sau đó, giám đốc sử dụng đối tượng xây dựng đó cho tất cả các công việc xây dựng tiếp theo. Tuy nhiên, có một cách tiếp cận khác khi khách hàng chuyển đối tượng người xây dựng sang phương thức sản xuất của giám đốc. Trong trường hợp này, bạn có thể sử dụng một công cụ xây dựng khác mỗi khi bạn sản xuất thứ gì đó với đạo diễn.

#### **V. Mã giả**

* Ví dụ về Design Pattern Builder dưới đây minh họa cách bạn có thể tái sử dụng cùng một mã xây dựng đối tượng khi tạo ra các loại sản phẩm khác nhau, chẳng hạn như ô tô, và tạo ra các hướng dẫn sử dụng tương ứng cho chúng.

![](../assets/builder/example-en.png)

* Một chiếc ô tô là một đối tượng phức tạp có thể được xây dựng theo hàng trăm cách khác nhau. Thay vì làm phình to lớp Car với một hàm khởi tạo khổng lồ, chúng ta đã tách mã lắp ráp ô tô vào một lớp builder riêng biệt gọi là CarBuilder. Lớp này cung cấp một tập hợp các phương thức để cấu hình các bộ phận khác nhau của ô tô.
* Nếu mã phía khách hàng cần lắp ráp một mẫu ô tô đặc biệt và được tinh chỉnh kỹ lưỡng, nó có thể làm việc trực tiếp với builder. Mặt khác, khách hàng có thể ủy quyền việc lắp ráp cho lớp director, lớp này biết cách sử dụng builder để xây dựng một số mẫu ô tô phổ biến nhất.
* Bạn có thể ngạc nhiên, nhưng mọi chiếc ô tô đều cần một cuốn hướng dẫn sử dụng (thật đấy, ai đọc chúng chứ?). Hướng dẫn mô tả mọi tính năng của ô tô, vì vậy chi tiết trong các hướng dẫn sẽ khác nhau tùy theo từng mẫu xe. Đó là lý do tại sao việc tái sử dụng quy trình xây dựng hiện có cho cả ô tô thật và hướng dẫn sử dụng của chúng là hợp lý. Tất nhiên, việc tạo một cuốn hướng dẫn không giống với việc lắp ráp một chiếc ô tô, và đó là lý do chúng ta cần cung cấp một lớp builder khác chuyên về soạn thảo hướng dẫn sử dụng. Lớp này triển khai các phương thức xây dựng giống như lớp builder dành cho ô tô, nhưng thay vì lắp ráp các bộ phận của ô tô, nó mô tả chúng. Bằng cách truyền các builder này cho cùng một đối tượng director, chúng ta có thể tạo ra hoặc một chiếc ô tô, hoặc một cuốn hướng dẫn sử dụng.
* Phần cuối cùng là lấy đối tượng kết quả. Một chiếc ô tô bằng kim loại và một cuốn hướng dẫn sử dụng bằng giấy, mặc dù có liên quan, nhưng vẫn là hai thứ rất khác nhau. Chúng ta không thể đặt một phương thức để lấy kết quả trong director mà không làm cho director bị ràng buộc với các lớp sản phẩm cụ thể. Vì vậy, chúng ta sẽ lấy kết quả của quá trình xây dựng từ builder đã thực hiện công việc.

```typescript
// Sử dụng mẫu thiết kế Builder chỉ có ý nghĩa khi sản phẩm của bạn
// khá phức tạp và cần cấu hình chi tiết. Hai sản phẩm sau đây có liên quan,
// mặc dù chúng không có giao diện chung.
class Car {
    // Một chiếc ô tô có thể có GPS, máy tính hành trình và một số ghế ngồi.
    // Các mẫu xe khác nhau (xe thể thao, SUV, mui trần) có thể được trang bị
    // hoặc kích hoạt các tính năng khác nhau.
}

class Manual {
    // Mỗi chiếc ô tô nên có một cuốn hướng dẫn sử dụng phù hợp với cấu hình của nó
    // và mô tả tất cả các tính năng.
}

// Giao diện builder xác định các phương thức để tạo ra
// các phần khác nhau của đối tượng sản phẩm.
interface Builder {
    reset(): void;
    setSeats(seats: number): void;
    setEngine(engine: Engine): void;
    setTripComputer(hasTripComputer: boolean): void;
    setGPS(hasGPS: boolean): void;
}

// Các lớp builder cụ thể tuân theo giao diện builder và
// cung cấp các triển khai cụ thể của các bước xây dựng.
class CarBuilder implements Builder {
    private car: Car;

    // Một phiên bản builder mới phải chứa một đối tượng sản phẩm trống
    // mà nó sử dụng trong quá trình lắp ráp tiếp theo.
    constructor() {
        this.reset();
    }

    // Phương thức reset xóa sạch đối tượng đang được xây dựng.
    reset(): void {
        this.car = new Car();
    }

    // Tất cả các bước sản xuất đều làm việc với cùng một đối tượng sản phẩm.
    setSeats(seats: number): void {
        // Đặt số lượng ghế trong ô tô.
    }

    setEngine(engine: Engine): void {
        // Cài đặt một động cơ cụ thể.
    }

    setTripComputer(hasTripComputer: boolean): void {
        // Cài đặt máy tính hành trình.
    }

    setGPS(hasGPS: boolean): void {
        // Cài đặt hệ thống định vị GPS.
    }

    // Các builder cụ thể cần cung cấp phương thức riêng để lấy kết quả.
    // Điều này là do các loại builder khác nhau có thể tạo ra các sản phẩm
    // hoàn toàn khác biệt và không tuân theo cùng một giao diện.
    getProduct(): Car {
        const product = this.car;
        this.reset();
        return product;
    }
}

// Không giống như các mẫu sáng tạo khác, Builder cho phép bạn tạo ra các sản phẩm
// không tuân theo giao diện chung.
class CarManualBuilder implements Builder {
    private manual: Manual;

    constructor() {
        this.reset();
    }

    reset(): void {
        this.manual = new Manual();
    }

    setSeats(seats: number): void {
        // Ghi lại các đặc điểm của ghế xe.
    }

    setEngine(engine: Engine): void {
        // Thêm hướng dẫn cho động cơ.
    }

    setTripComputer(hasTripComputer: boolean): void {
        // Thêm hướng dẫn sử dụng máy tính hành trình.
    }

    setGPS(hasGPS: boolean): void {
        // Thêm hướng dẫn sử dụng GPS.
    }

    getProduct(): Manual {
        const product = this.manual;
        this.reset();
        return product;
    }
}

// Lớp director chỉ chịu trách nhiệm thực thi các bước xây dựng
// theo một trình tự cụ thể. Điều này rất hữu ích khi sản xuất
// các sản phẩm theo thứ tự hoặc cấu hình cụ thể.
// Nói một cách nghiêm ngặt, lớp director là không bắt buộc, vì
// khách hàng có thể điều khiển builder trực tiếp.
class Director {
    // Director làm việc với bất kỳ instance builder nào mà mã khách hàng chuyển vào.
    // Bằng cách này, mã khách hàng có thể thay đổi loại sản phẩm được lắp ráp cuối cùng.
    constructSportsCar(builder: Builder): void {
        builder.reset();
        builder.setSeats(2);
        builder.setEngine(new SportEngine());
        builder.setTripComputer(true);
        builder.setGPS(true);
    }

    constructSUV(builder: Builder): void {
        // ...
    }
}

// Mã khách hàng tạo một đối tượng builder, chuyển nó cho director,
// và sau đó khởi tạo quá trình xây dựng. Kết quả cuối cùng được lấy từ builder.
class Application {
    makeCar(): void {
        const director = new Director();

        const carBuilder = new CarBuilder();
        director.constructSportsCar(carBuilder);
        const car = carBuilder.getProduct();

        const carManualBuilder = new CarManualBuilder();
        director.constructSportsCar(carManualBuilder);
        const manual = carManualBuilder.getProduct();

        // Sản phẩm cuối cùng thường được lấy từ builder
        // vì director không biết và không phụ thuộc vào builder và sản phẩm cụ thể.
    }
}
```

#### **VI. Ứng dụng**

* Sử dụng mẫu Builder để loại bỏ 'telescoping constructor'.
* Giả sử bạn có một constructor với mười tham số tùy chọn. Việc gọi một constructor như vậy rất bất tiện; vì vậy, bạn tạo ra các constructor nạp chồng và tạo ra nhiều phiên bản ngắn hơn với ít tham số hơn. Những constructor này vẫn tham chiếu đến constructor chính, truyền vào các giá trị mặc định cho những tham số bị bỏ qua.

```typescript
class Pizza {
    size: number;
    cheese?: boolean;
    pepperoni?: boolean;

    constructor(size: number, cheese?: boolean, pepperoni?: boolean) {
        this.size = size;
        this.cheese = cheese;
        this.pepperoni = pepperoni;
    }

    // Các phương thức khác sẽ được thêm vào đây
}
```

* Mẫu Builder cho phép bạn xây dựng đối tượng từng bước một, chỉ sử dụng những bước mà bạn thực sự cần. Sau khi triển khai mẫu này, bạn không còn phải nhồi nhét hàng chục tham số vào constructor của mình nữa.
