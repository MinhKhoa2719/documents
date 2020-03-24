# II. Structure
## 1. Adapter/ Wrapper
>Adapter Pattern là pattern giữ vai trò trung gian giữa hai lớp, chuyển đổi giao diện của một hay nhiều lớp có sẵn thành một giao diện khác, thích hợp cho lớp đang viết. Điều này cho phép các lớp có các giao diện khác nhau có thể dễ dàng giao tiếp tốt với nhau thông qua giao diện trung gian, không cần thay đổi code của lớp có sẵn cũng như lớp đang viết. Adapter Pattern còn gọi là Wrapper Pattern do cung cấp một giao diện “bọc ngoài” tương thích cho một hệ thống có sẵn, có dữ liệu và hành vi phù hợp nhưng có giao diện không tương thích với lớp đang viết

Adapter là một khái niệm rất thông dụng trong đời sống hàng ngày. Ta thường hay bắt gặp các loại adapter như: power adapter (chuyển đổi điện áp), laptop adapter (bộ sạc của laptop) hay memory card adapter… Các adapter này có nhiệm vụ chính là làm cầu nối trung gian để giúp hai đồ vật gì đó có thể hoạt động với nhau.

### 1.1. Tại sao cần sử dụng Adapter Pattern
Có khi nào bạn cảm thấy chán nản khi phải viết đi viết lại những đoạn code giống nhau từ dự án này sang dự án khác? Bạn đi đến quyết định cần phải tự viết library để tái sử dụng, tuy nhiên sẽ có tình huống interface mà bạn viết phù hợp với dự án cũ nhưng sang dự án mới thì không dùng được. Bạn lại hì hục ngồi sửa lại thư viện của mình để phù hợp với dự án mới. Adapter Pattern chính là cứu tinh của bạn trong trường hợp này. Apdater Pattern nên được sử dụng trong trường hợp :

- Bạn muốn sử dụng một Class đã có sẵn mà interface của nó lại không tương thích với interface bạn mong muốn. Chẳng hạn bạn muốn sử dụng một library của bên thứ ba nhưng interface của nó lại không phù hợp với dự án của bạn.
- Bạn muốn tạo ra một Class có khả năng tái sử dụng cao. Class của bạn có thể tương tác với các class có interface không tương thích sau này.

Để hiểu về sơ đồ mô tả Adapter Pattern thì trước hết bạn phải hiểu về 3 khái niệm:
- **Client**: Đây là lớp sẽ sử dụng đối tượng của bạn (đối tượng mà bạn muốn chuyển đổi giao diện)
- **Adaptee**: Đây là những lớp bạn muốn lớp Client sử dụng, nhưng hiện thời giao diện của nó không phù hợp
- **Adapter**: Đây là lớp trung gian, thực hiện việc chuyển đổi giao diện cho Adaptee và kết nối Adaptee với Client

### 1.2. Phân loại adapter
- **Composition**: Cấu thành. Nghĩa là một lớp B nào đó sẽ trở thành một thành phần của lớp A (một field trong lớp A). Tuy lớp A không kế thừa lại giao diện của lớp B nhưng nó có được mọi khả năng mà lớp B có
- **Inheritance**: Kế thừa. Nghĩa là một lớp Derived sẽ kế thừa từ lớp Base và thừa hưởng tất cả những gì lớp Base có. Nhờ kế thừa mà nó giúp tăng khả năng sử dụng lại code, tăng khả năng bảo trì và nâng cấp chương trình. Và do vậy kế thừa là khái niệm trọng tâm trong hướng đối tượng. Nhưng nó có một nhược điểm, đôi khi nếu chúng ta quá lạm dụng nó, nó sẽ làm cho chương trình của chúng ta phức tạp lên nhiều, điển hình là trong lập trình game. Do vậy đôi lúc trong lập trình game người ta thường có khuynh hướng thích sử dụng composition hơn

Và ứng với hai khái niệm này sẽ cho ta hai cách để chúng ta cài đặt lớp adapter: **Object Adapter** và **Class Adapter**

### 1.3. Class Adapter
Trong mô hình này, một lớp mới (Adapter) sẽ kế thừa lớp có sẵn với giao diện không tương thích (Adaptee), đồng thời cài đặt giao diện mà người dùng mong muốn (Target). Trong lớp mới, khi cài đặt các phương thức của giao diện người dùng mong muốn, phương thức này sẽ gọi các phương thức cần thiết mà nó thừa kế được từ lớp có giao diện không tương thích.

#### 1.3.1. Cấu trúc
![](./../images/class_adapter_pattern_structure.png)

#### 1.3.2. Ví dụ
- Tạo giao diện Target phù hợp cho người sử dụng
```javascript
interface Target {
    request(): string;
}
```
- Tạo lớp Adaptee chứa giao diện không phù hợp
```javascript
class Adaptee {
    specificRequest() : string {
        return ".eetpadA eht fo roivaheb laicepS";
    }
}
```
- Tạo lớp Adapter chuyển đổi giao diện sao cho phù hợp với lớp Target
```javascript
class Adapter extends Adaptee implements Target {
    constructor() {
        super();
    }
    request(){
        return "Adapter: (TRANSLATED) "+this.specificRequest().split("").reverse().join("");
    }
}
```
- Client sử dụng
```javascript
function clientCode(target: Target) {
    console.log(target.request());
}
let adaptee = new Adaptee();
console.log("Client: The Adaptee class has a weird interface. See, I don't understand it");
console.log("Adaptee: "+adaptee.specificRequest());

console.log("Client: But I can work with it via the Adapter");
let adapter = new Adapter();
clientCode(adapter);
```
- Kết quả
```
Client: The Adaptee class has a weird interface. See, I don't understand it
Adaptee: .eetpadA eht fo roivaheb laicepS
Client: But I can work with it via the Adapter
Adapter: (TRANSLATED) Special behavior of the Adaptee.
```

### 1.4. Object Adapter
Đây là một phương pháp cài đặt Adapter Pattern dựa trên ý tưởng về composition. Một lớp mới (Adapter) sẽ tham chiếu đến một (hoặc nhiều) đối tượng của lớp có sẵn với giao diện không tương thích (Adaptee), đồng thời cài đặt giao diện mà người dùng mong muốn (Target). Trong lớp mới này, khi cài đặt các phương thức của giao diện người dùng mong muốn, sẽ gọi phương thức cần thiết thông qua đối tượng thuộc lớp có giao diện không tương thích. Tiếp hợp đối tượng tránh được vấn đề đa thừa kế, không có trong các ngôn ngữ hiện đại (Java, C#). Mình khuyến khích các bạn sử dụng cách này.

#### 1.4.1. Cấu trúc Object Adapter
![](./../images/object_adapter_pattern_structure.png)

#### 1.4.2. Ví dụ
- Tạo lớp Target (lớp ban đầu mà client sử dụng)
```javascript
class Target {
    request() {
        return "Target: The default target's behavior.";
    }
}
```
- Tạo lớp Adaptee chứa giao diện không phù hợp
```javascript
class Adaptee {
    specificRequest() {
        return ".eetpadA eht fo roivaheb laicepS";
    }
}
```
- Tạo lớp Adapter chuyển đổi giao diện sao cho phù hợp với lớp Target
```javascript
class Adapter implements Target {
    constructor(private obj: Adaptee) { }
    request(){
        return "Adapter: (TRANSLATED) "+this.obj.specificRequest().split("").reverse().join("");
    }
}
```
- Client sử dụng
```javascript
function clientCode(target: Target) {
    console.log(target.request());
}
let adaptee = new Adaptee();
console.log("Client: The Adaptee class has a weird interface. See, I don't understand it");
console.log("Adaptee: "+adaptee.specificRequest());

console.log("Client: But I can work with it via the Adapter");
let adapter = new Adapter(adaptee);
clientCode(adapter);
```
- Kết quả
```
Client: The Adaptee class has a weird interface. See, I don't understand it
Adaptee: .eetpadA eht fo roivaheb laicepS
Client: But I can work with it via the Adapter
Adapter: (TRANSLATED) Special behavior of the Adaptee.
```

### 1.5. Lời kết
Adapter Design pattern thực sự rất hữu ích khi bạn code với ứng dụng lớn có sử dụng nhiều API từ bên ngoài, nó giúp bạn giảm thiểu tối đa nhưng thay đổi từ nhà cung cấp API. Nhìn thì thực sự nó hơi phức tạp vì phải tạo ra nhiều lớp và interface khác nhau nhưng nếu hệ thống lớn thì nó lại có rất nhiều hữu ích.

## 2. Bridge Pattern
>Bridge pattern được sử dụng khi chúng ta muốn tách một lớp lớn với các tính năng phức tạp thành một lớp chính (Abstraction) và nhiều giao diện của các tính năng khác nhau (Implementation) để phát triển độc lập với nhau. Chúng được nối với nhau bởi việc lớp chính trỏ đến giao diện của các tính năng gọi là bridge (cầu nối). Nhờ đó việc chỉnh sửa Abstraction sẽ không tác động đến Implementation và ngược lại.

### 2.1. Vấn đề
Giả sử bạn có một lớp **Shape** với một cặp lớp con: **Circle** và **Square**. Bạn muốn mở rộng hệ thống phân cấp lớp này để kết hợp các màu. Tuy nhiên, vì bạn đã có hai lớp con, bạn sẽ cần tạo bốn kết hợp lớp như **BlueCircle** và **RedSquare**.
Thêm các loại hình dáng và màu sắc mới vào sẽ phát triển hệ thống phân cấp của Shape theo cấp số nhân. Ví dụ: để thêm hình tam giác (Triangle), bạn phải tạo hai lớp con, mỗi lớp cho một màu. Và sau đó, việc thêm một màu mới sẽ yêu cầu tạo ba lớp con, mỗi lớp cho một loại hình dạng. Càng mở rộng, nó càng trở nên rối rắm phức tạp.
![](./../images/bridge_pattern_example_1.png)

Do đó chúng ta cần đến **Bridge pattern**

![](./../images/bridge_pattern_example_2.png)

### 2.2. Khi nào nên dùng Bridge pattern
- Dùng Bridge pattern khi bạn muốn phân chia và tổ chức một lớp có biến thể của các chức năng (Ví dụ: Muốn lớp có thể hoạt động với các máy chủ cơ sở dữ liệu khác nhau)
- Khi bạn cần mở rộng một lớp theo nhiều chiều trực giao (độc lập)
- Có thể chuyển đổi các Implementation trong thời gian chạy

### 2.3. Cấu trúc
![](./../images/bridge_pattern_structure.png)

- **Abstraction**: Cung cấp logic điều khiển mức cao. Nó dựa vào các đối tượng của Implementation để thực hiện các công việc cơ bản
- **Implementation**: Khai báo interface cho tính năng
- **Concrete Implementations**: Biến thể của Implementation
- **Refined Abstractions**: Biến thể của Abstraction
- **Client**: Người sử dụng khai báo Abstraction và các Implementation

### 2.4. Các bước thực hiện
- Xác định kích thước trực giao trong các lớp của bạn. Có thể là abstraction/platform, domain/infrastructure, front-end/back-end, hay interface/implementation
- Liệt kê những hoạt động nào client cần và khai báo chúng chúng trong lớp Abstraction
- Xác định các hoạt động cần thiết của các biến thể của Implementation mà Abstraction cần và khai báo chúng trong interface Implementation
- Đối với tất cả các biến thể, hãy tạo các lớp triển khai cụ thể, nhưng hãy đảm bảo tất cả chúng đều tuân theo interface Implementation.
- Trong lớp Abstraction, thêm trường tham chiếu cho kiểu Implementation. Abstraction đưa hầu hết các phương thức cần thiết cho đối tượng implementation được tham chiếu
- Nếu có một vài biến thể của Abstraction, hãy tạo các Refined Abstraction cho từng biến thể bằng cách extend lớp base Abstraction
- Client sẽ chuyển một đối tượng của Implementation vào constructor của Abstraction để liên kết chúng. Sau đó, Client không cần đụng đến Implementation nữa mà chỉ làm việc với đối tượng Abstraction thôi

### 2.5. Ví dụ
- Khai báo lớp Abstraction
```javascript
class Abstraction {
    constructor(protected iA : ImplementationA) { }
    operation() : string {
        return `Abstraction - Base operation with: \n${this.iA.operation_implementation()}`;
    }
}
```
- Tạo giao diện cho tính năng ImplementationA (một tính năng có nhiều biến thể)
```javascript
interface ImplementationA {
    operation_implementation() : string;
}
```
- Khai báo các biến thể của tính năng ImplementationA (Mở rộng theo chiều ngang)
```javascript
class ConcreteAImplementationA implements ImplementationA {
    operation_implementation() : string {
        return `Concrete A of Implementation A`;
    }
}
class ConcreteBImplementationA implements ImplementationA {
    operation_implementation() : string {
        return `Concrete B of ImplementationA`;
    }
}
```
- Client sử dụng
```javascript
function client_code(abstraction: Abstraction) {
    console.log(abstraction.operation());
}
let concreteAimplementationA = new ConcreteAImplementationA();
let abstraction = new Abstraction(concreteAimplementationA);
client_code(abstraction);
```
- Kết quả
```
Abstraction - Base operation with: 
Concrete A of Implementation A
```
- Ta có thể mở rộng class Abstraction (Mở rộng theo chiều dọc)
```javascript
class ExtendedAbstraction extends Abstraction {
    constructor(protected iA: ImplementationA) {
        super(iA);
    }
    operation() : string {
        return `ExtendedAbstraction - Extended operation with: \n${this.iA.operation_implementation()}`;
    }
}

let concreteBimplementationA = new ConcreteBImplementationA();
let extendedAbstraction = new ExtendedAbstraction(concreteBimplementationA);
client_code(extendedAbstraction);
```
- Kết quả
```
ExtendedAbstraction - Extended operation with: 
Concrete B of ImplementationA
```

## 3. Composite
> Composite pattern cho phép bạn làm việc với các đối tượng tương tự nhau với cấu trúc dạng cây

### 3.1. Khi nào nên dùng Composite pattern
- Khi bạn muốn xây dựng các hệ thống có cấu trúc dạng cây như sơ đồ nhân viên, danh sách bài hát...
- Khi muốn client xử lý đồng nhất cả hai yếu tố đơn giản và phức tạp thông qua một đoạn code duy nhất
### 3.2. Cấu trúc
![](./../images/composite_pattern_structure.png)

Với cách kiểu thiết kế này ta chia làm 3 loại classes chính:
- **Component**: Là những base class chúng nhằm mục đích định nghĩa các interface tổng quát cho tất cả các components khác
- **Leaf**: Là những thành phần độc lập không thể phân chia ra được những component nhỏ hơn nữa
- **Composite**: Là những loại compoments còn lại nghĩa là xếp ở vị trí higher-level, hiểu một cách đơn giản là nó đóng 2 vai trò: vừa là component và cũng là tập hợp của các component

### 3.3. Cách cài đặt
- Đảm bảo rằng mô hình cốt lõi của ứng dụng có thể được biểu diễn dưới dạng cấu trúc cây. Cố gắng chia nó thành các yếu tố đơn giản và container. Hãy nhớ rằng các container phải có khả năng chứa cả các yếu tố đơn giản và các container khác
- Khai báo `Component interface` với một danh sách các phương thức có ý nghĩa cho cả các thành phần đơn giản và phức tạp
- Tạo `Leaf` class để đại diện cho các yếu tố đơn giản. Một chương trình có thể có nhiều leaf class khác nhau
- Tạo `Container` class để biểu diễn các phần tử phức tạp. Trong class này, cung cấp một trường mảng để lưu trữ các tham chiếu đến các thành phần phụ. Mảng phải có khả năng lưu trữ cả lá và Container, vì vậy hãy chắc chắn rằng nó được khai báo với Component interface
- Cuối cùng, viết các phương thức để thêm và loại bỏ các phần tử con trong container

### 3.4. Ví dụ
- Tạo interface cho Component
```js
class Employee {
    private subordinates: Array<Employee>;
    constructor(
        private name: string,
        private dept: string,
        private salary: number
    ) {
        this.subordinates = new Array<Employee>();
    }
    add(e: Employee): void {
        this.subordinates.push(e);
    }
    remove(e: Employee): void {
        this.subordinates.splice(this.subordinates.indexOf(e), 1);
    }
    getChildren(): Array<Employee> {
        return this.subordinates;
    }
    toString(): string {
        return (`Employee :[ Name : ${this.name}, dept : ${this.dept}, salary : ${this.salary} ]`);
    }
}
```
- Triển khai Component và thêm các node cho cây (các nhân viên), với người đứng đầu là CEO
```js
let CEO: Employee = new Employee("John", "CEO", 30000);
let headSales : Employee = new Employee("Robert", "Head Sales", 20000);
let headMarketing : Employee = new Employee("Michel", "Head Marketing", 20000);

let clerk1 : Employee = new Employee("Laura", "Marketing", 10000);
let clerk2 : Employee = new Employee("Bob", "Marketing", 10000);

let salesExecutive1 : Employee = new Employee("Richard", "Sales", 10000);
let salesExecutive2 : Employee = new Employee("Rob", "Sales", 10000);

CEO.add(headSales);
CEO.add(headMarketing);

headSales.add(salesExecutive1);
headSales.add(salesExecutive2);

headMarketing.add(clerk1);
headMarketing.add(clerk2);

//print all employees of the organization
console.log(CEO);
for (let headEmployee of CEO.getChildren()) {
    console.log(headEmployee);
    for (let employee of headEmployee.getChildren())
        console.log(employee);
}
```
- Kết quả
```
Employee :[ Name : John, dept : CEO, salary : 30000 ]
Employee :[ Name : Robert, dept : Head Sales, salary : 20000 ]
Employee :[ Name : Richard, dept : Sales, salary : 10000 ]
Employee :[ Name : Rob, dept : Sales, salary : 10000 ]
Employee :[ Name : Michel, dept : Head Marketing, salary : 20000 ]
Employee :[ Name : Laura, dept : Marketing, salary : 10000 ]
Employee :[ Name : Bob, dept : Marketing, salary : 10000 ]
```
## 4. Decorator
> Decorator thường được dùng khi ta muốn thêm chức năng cho một đối tượng đã tồn tại trước đó, mà không muốn ảnh hưởng đến các đối tượng khác

### 4.1. Vấn đề
Đôi khi chúng ta cần mở rộng một phương thức trong đối tượng, và cách thông thường là chúng ta sẽ kế thừa đối tượng đó. Nhưng trong một vài trường hợp sẽ làm cho mã nguồn trở lên phức tạp hơn chúng ta mong muốn. Do đó ta cần sử dụng đến Decorator Pattern để mở rộng phương thức một cách linh hoạt.

### 4.2. Khác biệt giữa mở rộng phương thức theo cách linh động với mở rộng theo cách tĩnh
- Mở rộng theo cách tĩnh: Chúng ta thường kế thừa lại class đó và mở rộng một cách cứng nhắc, vì cách mở rộng đó đã áp dụng cho class kế thừa đó rồi nên khi cần mở rộng thêm ta lại phải kế thừa (nếu muốn lược bỏ bớt tính năng ta cũng phải làm thế), khiến hệ thống trở nên phức tạp
- Mở rộng theo cách động: Mở rộng theo cách linh động là chúng ta sẽ cung cấp một cơ cấu mà cơ cấu này cho phép chúng ta thay đổi một đối tượng đã tồn tại nhưng không làm ảnh hưởng đến các đối tượng khác của cùng lớp đó 

### 4.3. Ví dụ
Tưởng tượng rằng bạn đang làm việc cho một cửa hàng bánh **Pizza**, cửa hàng của bạn vừa làm **pizza cà chua** và **pizza phô mai**. Sau đó bạn cần đặt thêm một vài nguyên liệu nữa lên phần trên của bánh vì khách hàng có quyền lựa chọn thêm gà hoặc tiêu cho chiếc bánh của họ. Về cơ bản bạn có một số loại pizza như: **pizza gà cà chua**, **pizza cà chua hạt tiêu**, **pizza gà phô mai**, **pizza phô mai hồ tiêu**, **pizza cà chua gà hồ tiêu** và **phô mai gà hồ tiêu**. Nếu chúng ta giải quyết vấn đề này với cách mở rộng tĩnh thì bạn sẽ cần tạo ra một số lượng lớn các lớp như **TomatoChickenPizza**, **TomatoPepperPizza**... nó có nghĩa là một số lượng lớn các kết hợp sẽ bị thêm vào. Do đó chúng ta cần giải quyết theo cách động.

### 4.4. Cấu trúc
![](./../images/decorator_pattern_structure.png)

Những thành phần trong mẫu thiết kế Decorator:
- **Component (IPizza)**: Giao diện (interface) chung được dùng để triển khai các đối tượng
- **ConcreteComponent (TomatoPizza, ChickenPizza)**: Các đối tượng triển khai giao diện Component
- **Decorator (PizzaDecorator)**: Lớp trừu tượng có duy trì một tham chiếu đến đối tượng gốc và đồng thời cài đặt các phần cần thiết cho Decorator
- **ConcreteDecorator (PepperDecorator, CheeseDecorator)**: Một cài đặt của Decorator, nó cài đặt các phần mở rộng cho đối tượng được đưa vào

### 4.5. Thực hành
- Tạo giao diện Component (IPizza):
```javascript
interface IPizza {
    doPizza() : string;
}
```
- Tạo ra 2 concrete của Component:
```javascript
class TomatoPizza implements IPizza {
    doPizza(): string {
        return "I am a Tomato Pizza";
    }
}
class ChickenPizza implements IPizza {
    doPizza(): string {
        return "I am a Chicken Pizza";
    }
}
```
- Tạo ra một lớp trừu tượng Decorator làm xương sống cho các concrete Decorator khác
```javascript
abstract class PizzaDecorator implements IPizza {
    constructor(protected mPizza: IPizza) { }

    getPizza(): IPizza {
        return this.mPizza;
    }

    setPizza(mPizza: IPizza): void {
        this.mPizza = mPizza;
    }

    abstract doPizza(): string;
}
```
- Tạo ra 2 concrete của Decorator là **PepperDecorator** và **CheeseDecorator** và cài đặt các phương thức mở rộng. Ví dụ PepperDecorator sẽ thêm tiêu vào pizza. Tính năng mở rộng là được cài đặt trong phương thức **addPepper()**
```javascript
class CheeseDecorator extends PizzaDecorator {
    constructor(pizza: IPizza) {
        super(pizza);
    }

    doPizza(): string {
        let type: string = this.mPizza.doPizza();
        return type + this.addCheese();
    }

    // This is additional functionality
    // It adds cheese to an existing pizza
    addCheese(): string {
        return " + Cheese";
    }
}
class PepperDecorator extends PizzaDecorator {
    constructor(pizza: IPizza) {
        super(pizza);
    }

    doPizza(): string {
        let type: string = this.mPizza.doPizza();
        return type + this.addPepper();
    }

    // This is additional functionality
    // It adds cheese to an existing pizza
    addPepper(): string {
        return " + Pepper";
    }
}
```
- Và cuối cùng sử dụng Decorator để thêm tính năng (tiêu, sốt) cho Pizza
```javascript
let tomato : IPizza = new TomatoPizza();
let chicken : IPizza = new ChickenPizza();

console.log(tomato.doPizza());
console.log(chicken.doPizza());

// Use Decorator pattern to extend existing pizza dynamically
// Add pepper to tomato-pizza
let pepperDecorator : PepperDecorator = new PepperDecorator(tomato);
console.log(pepperDecorator.doPizza());

// Add cheese to tomato-pizza
let cheeseDecorator : CheeseDecorator = new CheeseDecorator(tomato);
console.log(cheeseDecorator.doPizza());

// Add cheese and pepper to tomato-pizza
// We combine functionalities together easily.
let cheeseDecorator2 : CheeseDecorator = new CheeseDecorator(pepperDecorator);
console.log(cheeseDecorator2.doPizza());
```
- Kết quả
```
I am a Tomato Pizza
I am a Chicken Pizza
I am a Tomato Pizza + Pepper
I am a Tomato Pizza + Cheese
I am a Tomato Pizza + Pepper + Cheese
```

## 5. Facade
> Bao bọc một hệ thống con phức tạp với một giao diện đơn giản

### 5.1. Ưu điểm của Facade Pattern?
- Giúp cho một thư viện của bạn trở nên đơn giản hơn trong việc sử dụng và đọc hiểu
- Giảm sự phụ thuộc của các mã code bên ngoài với code bên trong của thư viện, vì hầu hết các code đều dùng Facade, vì thế cho phép sự linh động trong phát triển các hệ thống
- Đóng gói tập nhiều hàm API được thiết kế không tốt bằng một hàm API có thiết kế tốt hơn

### 5.2. Vấn đề
Giả sử bạn có chuỗi các hành động được thực hiện theo thứ tự, và các hành động này lại được yêu cầu ở nhiều nơi trong phạm vi ứng dụng của bạn, vậy mỗi lúc bạn cần dùng đến nó bạn lại phải copy-paste hoặc viết lại đoạn code đó vào những nơi cần sử dụng trong ứng dụng. Điều này có vẻ ok, copy cũng nhanh nên chẳng sao, nhưng nếu bỗng nhiên làm xong bạn nhận ra cần phải thay đổi lại cấu trúc và mã xử lý trong hầu hết chuỗi hành động đó, vậy bạn sẽ làm gì ?
Đây chính là mấu chốt của vấn đề, bạn sẽ lại đi lục lại đoạn code đó ở tất cả các nơi, rồi lại sửa nó. Điều này quá tốn thời gian và hơn nữa dường như bạn đang mất đi sự kiểm soát các đoạn mã của mình và trong quá trình đó còn có nguy cơ phát sinh lỗi. Do vậy ta cần dùng đến Facade Pattern.

### 5.3. Giải pháp
Những gì bạn cần phải làm chỉ là thiết kế một Facade, và trong đó phương thức facade sẽ xử lý các đoạn code dùng đi dùng lại. Từ xu hướng quan điểm trên, chúng ta sẽ chỉ cần gọi Facade để thực thi các hành động dựa trên các parameters được cung cấp.
Bây giờ nếu chúng ta cần bất kỳ thay đổi nào trong quá trình trên, công việc sẽ đơn giản hơn rất nhiểu, chỉ cần thay đổi các xử lý trong phương thức facade của bạn và mọi thứ sẽ được đồng bộ thay vì thực hiện sự thay đổi ở những nơi sử dụng cả chuỗi các mã code đó.

### 5.4. Cấu trúc
![](./../images/facade_pattern_structure.png)

*Các subsystem bên trong Facade cũng sử dụng Facade*
### 5.5. Ví dụ
Hãy cùng xét một ví dụ khi người dùng mua hàng online trên trang web của bạn. Một quá trình kiểm tra đơn giản bao gồm các bước sau:
- Thêm sản phẩm vào giỏ hàng
- Tính toán chi phí vận chuyển
- Tính toán tiền chiết khấu
- Tạo đơn đặt hàng
```javascript
// Xử lý checkout
let productId = "123456";
let product = Product.find(productId);
if(product.length > 0) {
    // Thêm vào giỏ hàng
    let cart = new Cart();
    cart.addItem(product);
    // Tính phí ship hàng
    let shipping = new ShippingCharge(product);
    shipping.calculateCharge();
    // Tính mã giảm giá
    let discount = new Discount(product);
    discount.applyDiscount();
    // Tạo mã đơn hàng
    let order = new Order();
    order.generateOrder();
    ...
}
```
- Giả sử bạn sử dụng đoạn mã trên để xử lý đặt hàng của người dùng, có rất nhiều object sử dụng để hoàn thành chuỗi xử lý order nên việc mỗi nơi trong ứng dụng cần chuỗi xử lý này lại bê đoạn code này theo là điều hoàn toàn không nên làm
- Vậy chúng ta thử áp dụng Facade vào ví dụ trên
```javascript
class OrderFacade {
        private product: any;
        constructor(productId: string) {
            this.product = Product.find(productId);
        }
        generateOrder() {
            // Xử lý toàn bộ logic
            if(this.checkQuantity()) {
                this.addToCart();
                this.calulateShipping();
                this.applyDiscount();
                this.placeOrder();
            }

        }
        private addToCart () {
            let cart : Cart = new Cart();
            cart.addItem(this.product);
        }
        private checkQuantity() {
            return this.product.length != 0 ? true : false;
        }
        private function calulateShipping() {
            let shipping : ShippingCharge = new ShippingCharge(this.product);
            shipping.calculateCharge();
        }
        private applyDiscount() {
            let discount : Discount = new Discount();
            discount.applyDiscount(this.product);
        }
        private placeOrder() {
            let order : Order = new Order();
            order.generateOrder();
        }
    }
```
- Sử dụng
```javascript
let productId = "123456";
let order = new OrderFacade(productId);
order.generateOrder();
```
Như vậy đoạn mã phức tạp trên đã được gói gọn trong phương thức **generateOrder** và giờ mỗi khi chúng ta có thay đổi không cần phải lục lại những đoạn code như trong ví dụ ban đầu mà chỉ cần thay đổi xử lý trong Facade, và cũng không cần đem cả đoạn code dài ban đầu đi sử dụng khi cần, mà chỉ cần gọi method **generateOrder()**.

### 5.6. Kết
Facade Pattern chỉ nên thực hiện trong tình huống mà bạn cần một interface duy nhất để hoàn thành nhiều nhiệm vụ, tưởng tượng giống như bạn có một thư ký và cô ấy sẽ lên kế hoạch công việc theo trình tự giống như cách một Facade object làm để giúp bạn hoàn thành nhiều nhiệm vụ.

## 6. Proxy
> Proxy cung cấp một class ảo đứng trước class thực sự mà chúng ta muốn làm việc để xử lí, tăng thêm tính bảo mật và cải thiện performance cho hệ thống khi xử lí dữ liệu liên quan đến class mà chúng ta làm việc

### 6.1. Khi nào nên sử dụng Proxy Pattern?
- Khi muốn thêm một số bước bảo mật, quản lí sự truy cập/truy xuất dữ liệu ra vào đối tượng
- Linh hoạt cách truy xuất/truy cập dữ liệu (Lazy Loading,...)

### 6.2. Các loại ứng dụng của Proxy Pattern
- **Remote Proxy**: Cung cấp một đại diện cho object nhưng nằm trong 1 địa chỉ khác
- **Virtual Proxy**: Áp dụng cho các dữ liệu tốn nhiều chi phí khởi tạo, loại proxy này cung cấp các phương pháp lấy dữ liệu linh hoạt, chỉ thực sự khởi tạo/truy xuất dữ liệu khi được người dùng yêu cầu thay vì khởi tạo dữ liệu từ đầu. Một số ứng dụng như Lazy Loading,...
- **Protective Proxy**: Kiểm soát quyền truy cập vào object gốc. Loại proxy này tạo thêm các hàng rào kiểm soát nhằm đảo bảo bảo mật cho object
- **Smart Proxy**: Bổ sung thêm các hành động, phần mở rộng khi truy cập/truy xuất dữ liệu trong đối tượng
### 6.3. Cấu trúc
![](./../images/proxy_pattern_structure.png)

Các đối tượng tham gia vào Proxy Pattern:
- **Subject**: Interface giữ vai trò tạo xương sống cho **RealSubject** (đối tượng thực sự) và **Proxy**
- **RealSubject**: Đối tượng thực sự mà người dùng làm việc
- **Proxy**: Triển khai xương sống từ **Subject** và là class đứng trước (đại diện) **RealSubject** nhằm thực hiện các tiền/hậu xử lí, bảo mật cho **RealSubject**. Proxy duy trì một tham chiếu đến đối tượng của **RealSubject** nhằm truy cập/truy xuất dữ liệu

### 6.4. Ví dụ
- Tạo interface **IItem** và class **Item** triển khai interface đó
- **Item** là một class không tốn nhiều chi phí khởi tạo đối tượng nhưng giá trị **content** bên trong Item lại tốn nhiều chi phí khởi tạo (tưởng tượng rằng mỗi khi muốn lấy giá trị content thì phải request lên server để lấy giá trị, điều đó gây nên chi phí, thời gian kết nối đến server, có thể xem đối tượng IItem là index/id của giá trị content)
```js
interface IItem {
    delaytoResponseContent(): Promise<number>;
    content: number;
}

class Item implements IItem {
    // Giá trị của content nằm trên server
    protected _content: number = Math.round(Math.random() * 10);
    get content() {
        return this._content;
    }
    set content(n: number) {
        this._content = n;
    }
    
    // Trả về giá trị của content mất nhiều chi phí
    async delaytoResponseContent() : Promise<number> {
        await this.sleep(1000);
        return this.content;
    }
    
    // Tạo chi phí kết nối
    sleep(ms: number) {
        return new Promise((resolve) =>
            setTimeout(resolve, ms)
        )
    }
}
```
- Tạo interface **Subject** và class **RealSubject**, **Proxy**
```js
interface Subject {
    data: Array<IItem>;
    Request(index: number): any;
}

class RealSubject {
    constructor(private _data: Array<IItem>) { }
    get data() : Array<IItem> {
        return this._data;
    }

    Request(index: number) : Promise<number> {
        return this.data[index].delaytoResponseContent();
    }
}

class SubjectProxy implements Subject {
    // Khai báo cho đúng cấu trúc nhưng không sử dụng
    data!: Array<IItem>;
    // Duy trì một kết nối đến RealSubject để xử lí dữ liệu
    private realSubject!: RealSubject;

    // Kết nối đến host thông qua RealSubject
    constructor(data: Array<IItem>) {
        this.realSubject = new RealSubject(data);
    }

    // - Ghi đè lại phương thức Request
    // - Thay vì lấy về giá trị content mất nhiều chi phí
    //   thì ta chỉ lấy về index/id của giá trị đó
    // - Khi nào người dùng cần giá trị content thì mới
    //   sử dụng phương thức delaytoResponseContent để
    //   lấy giá trị content về
    Request(index: number) : IItem {
        return this.realSubject.data[index];
    }
}
```
- Tạo một host giả có chứa và index Item
```js
// Tạo một host giả có chứa content và index Item
let data : Array<IItem> = [
    new Item(),
    new Item(),
    new Item(),
    new Item(),
    new Item(),
    new Item(),
];
```
- Khi không sử dụng Proxy. Giả sử đường truyền yếu chỉ có thể tại được 1 content 1 lần và ta phải tải hết dữ liệu về để hiển thị trong khi người dùng muốn xem dữ liệu thứ 4 trước tiên
```js
let realSubject = new RealSubject(data);
(async () => {
    let wanted : number = 3;
    for(let i = 0; i < 5; i++) {
        await realSubject.Request(i).then(
            res => {
                if(i!=wanted)
                    console.log("Result: "+res)
                else
                    console.log("Wanted result: "+res)
            }
        );
    }
})();
```
- Kết quả khi không dùng Proxy
```js
Result: 4
Result: 5
Result: 5
Wanted result: 4
Result: 9
```
- Khi sử dụng Proxy và ứng dụng Lazy Loading (Virtual Proxy). Dữ liệu cần thiết sẽ được tải trước, các dữ liệu còn lại sẽ được tải sau
```js
let proxy = new SubjectProxy(data);
(async () => {
    let wanted : number = 3;
    let results : Array<IItem> = new Array<IItem>();
    for(let i = 0; i < 5; i++) {
        results.push(proxy.Request(i));
        if(i==wanted)
            results[i].delaytoResponseContent()
                .then(res => console.log("Wanted result via proxy: "+res));
    }
    for(let i = 0; i < 5; i++)
        if(i!=wanted)
            results[i].delaytoResponseContent()
                .then(res => console.log("Result via proxy: "+res))
})();
```
- Kết quả khi sử dụng Proxy
```js
Wanted result via proxy: 1
Result via proxy: 0
Result via proxy: 7
Result via proxy: 2
Result via proxy: 1
```
- Ngoài ra ta có thể cài đặt thêm các tính năng bảo mật, kiểm tra lỗi cho proxy để tăng tính bảo mật hơn cho hệ thống (Proxy Smart)

### 6.5. So sánh với pattern cùng loại (Structural Pattern)
Ta có thể thấy Proxy Pattern khá giống với **Adapter Patern** và **Decorator Pattern** nên ta cần phân biệt chúng:
- Khác với **Adapter Pattern**: Thông thường mẫu Adapter cung cấp một giao diện khác với đối tượng gốc, còn Proxy cung cấp cùng một giao diện giống như đối tượng gốc
- Khác với **Decorator Pattern**: Có thể cài đặt tương tự như Proxy, nhưng Decorator được dùng cho mục đích khác. Decorator bổ sung thêm nhiều nhiệm vụ cho một đối tượng nhưng ngược lại Proxy điều khiển truy cập đến một đối tượng. Proxy tuỳ biến theo nhiều cấp khác nhau mà có thể được cài đặt giống như một Decorator:
    - **Protection Proxy, Smart Proxy**: Có thể được cài đặt như một Decorator
    - **Remote Proxy**: Sẽ không tham chiếu trực tiếp đến đối tượng thực sự tham chiếu gián tiếp, giống như ID của host và địa chỉ trên host vậy
    - **Virtural Proxy**: Tham chiếu gián tiếp chẳng hạn như tên file, index và sẽ tham chiếu trực tiếp khi cần thiết