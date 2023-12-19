목차

1. 객체 지향 프로그래밍이란?

2. OOP의 4가지 원칙

3. 더 알아두면 좋은 개념

4. OOP 모범 사례

## 객체 지향 프로그래밍이란?

객체 지향 프로그래밍이 등장하기 전 사용되던 구조적 프로그래밍은 더 복잡한 어플리케이션에 대한 수요가 증가하면서 구조화된 프로그래밍의 한계가 나타나기 시작했습니다. 복잡한 어플리케이션을 위해서는 실제 세계처럼 더 밀접한 모델링 방식이 필요했기 때문입니다. 이를 위해 나타난 방식이 객체 지향 프로그래밍(Object Oriented Programming)입니다. OOP의 핵심은 객체와 클래스라고 할 수 있습니다. 이들은 실제 개체와 같은 **데이터**와 **행동**이라는 2가지 특징을 같고 있습니다.

- **데이터**는 객체의 속성과 상태를 나타낼 수 있습니다.
- **행동**은 스스로 변화하고 다른 물체와 소통 할 수 있는 능력입니다.

### 클래스와 객체

객체는 클래스의 인스턴스 입니다. 각각의 객체는 상태,행동 그리고 식별자를 갖고 있습니다. 또한 객체들은 서로간의 호출을 통해 통신할 수 있으며 이를 message passing 이라고 합니다.

하나의 클래스를 통해 필요로 하는 어플리케이션에 여러개의 객체를 생성할 수 있습니다. 각 객체의 식별은 일반적으로 JVM에 의해 유지되며 Java 객체를 만들 때마다 JVM은 객체에 대한 해시코드를 만들고 할당하게 됩니다. 이를통해 JVM은 모든 객체가 고유하게 식별되도록 합니다.

생성자는 반환 값이 없는 특수한 메서드 입니다. 생성자의 이름은 항상 클래스의 이름과 동일하며 초기 객체 상태를 설정하기 위한 매개 변수를 사용할 수도 있습니다. 생성자를 작성하지 않는 경우 JVM은 기본 생성자를 할당하며 이는 매개변수를 허용하지 않기 때문에 매개변수가 필요한 경우에는 개발자가 직접 생성자를 작성해야 합니다.

## OOP의 4가지 원칙

### 추상화(Abstraction)

추상화는 컨텍스트와 관련이 업는 정보를 숨기거나 관련된 정보만 알 수 있도록 하는 것입니다.

일반적인 추상화는 **데이터 추상화** 와 **제어 추상화**로 볼 수 있는데 데이터 추상화는 복잡한 데이터 형태를 생성하기 위해 여러 작은 데이터 타입을 사용하는 방법을 의미합니다.

```
// 데이터 추상화
public class Employee
{
    private Department department;
    private Address address;
    private Education education;
    //So on...
}
```

제어 추상화는 어떤 클래스의 메소드를 사용하는 사용자에게 해당 메소드의 작동방식과 같은 로직을 숨기기 위함입니다. 만일 메소드 내 로직이 변경된더라도 실제 사용자는 변경된 내용이 어떤것인지 알 필요 없이 이전과 동일하게 메소드를 사용할 수 있습니다. 따라서 로직이 변경되더라도 사용자에게 영향을 주지 않습니다.

```
//제어 추상화
public class EmployeeManager
{
    public Address getPrefferedAddress(Employee e)
    {
        //Get all addresses from database
        //Apply logic to determine which address is preferred
        //Return address
    }
}
```

### 캡슐화(Encapsulation)

캡슐화는 관련이 있는 변수와 함수를 하나의 클래스로 묶고 외부에서 쉽게 접근하지 못하도록 은닉하는 것입니다. 객체의 직접적인 접근을 막고 외부에서 내부의 정보에 직접접근하거나 변경할 수 없고 객체가 제공하는 필드와 메소드를 통해서만 접근이 가능합니다. 캡슐화에는 정보은닉과 구현은닉을 모두 갖고 있습니다.

정보은닉의 방법으로는 접근제어자를 사용하여 외부에서 접근할 수 없도록 하며 인터페이스를 통해 구현은닉을 달성하는 것입니다. 구현은닉은 객체가 책임을 이행하는 방식을 수정할 수 있도록 개발자에게 자유를 제공합니다. 이는 설계가 변경 될 때 유용하게 작용할 수 있습니다.

```
//정보 은닉
class InformationHiding
{
    //Restrict direct access to inward data
    private ArrayList items = new ArrayList();

    //Provide a way to access data - internal logic can safely be changed in future
    public ArrayList getItems(){
        return items;
    }
}
```

```
//implementation hiding
interface ImplemenatationHiding {
    Integer sumAllItems(ArrayList items);
}
class InformationHiding implements ImplemenatationHiding
{
    //Restrict direct access to inward data
    private ArrayList items = new ArrayList();

    //Provide a way to access data - internal logic can safely be changed in future
    public ArrayList getItems(){
        return items;
    }

    public Integer sumAllItems(ArrayList items) {
        //Here you may do N number of things in any sequence
        //Which you do not want your clients to know
        //You can change the sequence or even whole logic
        //without affecting the client
    }
}
```

### 상속(Inheritance)

자바에서의 상속은 하나의 클래스가 부모클래스의 속성과 행동을 얻게 되는 방법입니다. 상속은 코드의 재사용성과 유지보수를 위해 사용됩니다. 상속을 사용하기 위해서는 **extends** 키워드를 상속 받을 클래스에 명시하여 사용할 수 있습니다. 상속되는 클래스는 **super** 클래스라 부르고 새롭게 생성된 클래스를 **sub** 클래스라 합니다.

sub클래스는 super클래스의 non-private 멤버들을 상속 받게되며 생성는 멤버가 아니기 때문에 상속되지 않습니다. 하지만 sub클래스에서 super클래스의 생성자를 호출할 수 있습니다.

### 다형성(polymorphism)

다형성은 같은 자료형에 여러가지 객체를 대입하여 다양한 결과를 얻어내는 성질을 의미합니다. 이를 통해 동일한 이름을 같은 여러 형태의 매소드를 만들 수 있습니다. 자바에서는 다형성을 다루는 근본적인 방법이 2가지 있는데 **compile time polymorphism** 과 **runtime polymorphism** 입니다.

compile time polymorphism은 컴파일러가 필요한 모든 정보를 가지고 있고 프로그램 컴파일 중에 호출할 방법을 알기 때문에 컴파일 시간에 적절한 메소드를 각각의 객체에 바인딩할 수 있습니다. 정적바인딩이나 early binding이라고도 불립니다. 자바에서는 메소드 오버로딩을 통해 사용됩니다. 메소드 오버로딩을 통해 메소드의 매개변수의 형태가 달라질 수 있습니다.

runtime polymorphism은 동적 바인딩이라고 불리며 메소드 오버라이딩과 연관있습니다. 일반적으로 런타임 다형성은 부모 클래스와 자식 클래스가 존재할때 사용되는데 부모자식 클래스에 존재하는 메소드를 실행시키게 되면 런타임과정에서 해당 인스턴스에 맞는 메소드를 호출하게 됩니다.

```
class Animal {
   public void sound() {
         System.out.println("Some sound");
   }
}

class Lion extends Animal {
   public void sound() {
         System.out.println("Roar");
   }
}

class Main
{
   public static void main(String[] args)
   {
        //Parent class reference is pointing to a parent object
        Animal animal = new Animal();
        animal.sound(); //Some sound

        //Parent class reference is pointing to a child object
        Animal animal = new Lion();
        animal.sound(); //Roar
   }
}
```

## 더 알아두면 좋은 개념

### 결합도(Coupling)

결합도는 클래스간의 상호 의존 정도를 의미합니다. 즉 얼마나 강하게 클래스들 사이에 연관이 되어있는지를 뜻하게 됩니다. 좋은 소프트웨어는 낮은 결합도(low coupling)를 갖고 있는 것입니다. 즉 하나의 클래스는 특정 기능에 독립적이여야하며 EmailSender 라는 클래스의 경우 이메일을 전송하는 기능에만 집중해야합니다.

### 응집도(Cohesion)

응집도는 하나의 클래스가 기능에 집중하기 위한 모든 정보와 역할을 갖고 있어야 한다는 의미입니다. 만일 어떤 기능이 작동하기 위해 여러 클래스가 필요다면 유지보수적인 측면에서 좋지 않고 재사용성을 낮추게 됩니다. 좋은 소프트웨어는 높은 응집도(high cohesion)을 유지해야합니다.

Association은 독립적인 라이프사이클을 갖고 있는 객체들의 관계를 의미합니다. 클래스들간의 연관은 이루어지지만 한 클래스의 생성과 제거에 의해 다른 클래스가 영향 받지 않는 속성을 뜻합니다.

Aggregation은 객체들 간의 관계에서 독립적인 라이프사이클은 존재하지만 관계의 소유권이 존재하는 성질입니다. 부모클래스와 자식클래스 사이에서 자식 클래스는 다른 부모 클래스에 속할 수 없습니다. 하지만 자식클래스 자체 라이프사이클은 독립적으로 존재합니다.

Composition은 객체간의 관계에서 독립적인 라이프사이클이 존재하지 않는 성질입니다. 만약 부모 객체가 삭제된다면 모든 자식 객체들 또한 삭제 됩니다.

## OOP 모범 사례

### 상속보단 Composition

composition을 구현하기 위해서는 다양한 인터페이스의 생성이 이루어집니다. 상속은 어떤 클래스의 기능을 확장시키기 위한 목적으로 볼 수 있고 인터페이스는 해당 시스템이 작동하는데 필요한 필수적인 변수와 메소드들을 명시해줍니다. 인터페이스 구현을 통해 필수적인 기능외에 필요한 비지니스 로직들이 추가되고 사용자는 인터페이스에 존재하는 메소드를 통해 시스템을 작동 시킬 수 있기 때문입니다.

### 인터페이스 사용하여 구현하기

인터페이스를 사용하게 되면 코드의 유연성이 증가합니다. 인터페이스는 super class와 같은 역할을 하기 때문에 인터페이스를 통해 레거시 코드를 변경하지 않고 메소드의 로직을 새롭게 생성 할 수 있습니다.

### 코드 반복하지 않기

중복되는 코드를 사용하지 않고 추상화나 추상메소드를 사용하여 해결한다. 2곳 이상에서 동일한 코드가 작성된다면 함수로 분리하여 호출하는 방식으로 사용할 것

### 변경 사항 캡슐화

향후 변경 가능성이 있는 코드를 캡슐화하여 사용자가 코드를 직접 변경하는 일을 줄일 수 있습니다. 접근제어자를 사용하여 캡슐화를 진행하고 캡슐화를 달성하기 위해서는 디자인 패턴중 팩토리 디자인 패턴을 사용할 수 있습니다.

### SRP(Single Responsibility Principle)

SRP는 OOP의 solid 원칙중 하나 입니다. SPR는 하나의 클래스가 단 하나의 책임을 가져야 한다는 것을 의미합니다. 즉 하나의 목적으로 클래스를 사용해야하며 이는 low coupling 과 high cohesion과 연관지어 생각할 수 있습니다.

### OCP(Open Closed Principle)

OCP 또한 solid 원칙 중 하나이며 확장에는 열려있고 변경에는 닫혀있어야 한다는 의미입니다. 즉 기존 코드를 변경하지 않으면서 새로운 기능을 추가하는 것에는 유연해야 한다는 의미입니다.

## SOLID 설계 원칙

### S : SRP, Single Responsibility Principle 단일 책임 원칙

객체는 단 하나의 책임만 가져야 한다.

### O : OCP, Open Close Prinicple 개방 폐쇄 원칙

기존의 코드를 변경하지 않으면서 기능을 추가할 수 있도록 설계가 되어야 한다.

### L : LSP, Liskov Substitution Principle 리스코프 치환 원칙

일반화 관계에 대한 이야기며, 자식 클래스는 최소한 자신의 부모 클래스 에서 가능한 행위는 수행할 수 있어야 한다.

### I : ISP, Interface Segregation Principle 인터페이스 분리 원칙

인터페이스를 클라이언트에 특화되도록 분리시키라는 설계 원칙.

### D : DIP, Dependency Inversion Principle의존 역전 원칙

고수준 모듈은 저수준 모듈의 구현에 의존해서는 안된다. 의존 관계를 맺을 때 변화하기 쉬운 것 또는 자주 변화하는 것보다는 변화하기 어려운 것, 거의 변화가 없는 것에 의존하라는 것이다.

참고1: https://howtodoinjava.com/java/oops/object-oriented-programming/
참고2: https://www.notion.so/05-OOP-4-OOP-5-51d3e07ca00b48a99b838b32bdc86146
