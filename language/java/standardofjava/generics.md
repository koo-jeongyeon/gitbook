# generics

* 지네릭스는 JDK1.5에서 처음 도입되었다. 이젠 지네릭스를 모르고는 JAVA API문서를 제대로 보기 어려울만큼 중요한위치를 차지하였다.

## 지네릭스란?

* 메서드나 컬렉션클래스에 컴파일시의 타입체크를 해주는 기능이다.
* 객체의 타입 안정성을 높이고 형변환의 번거로움을 줄여준다.
* 타입안정성 = 의도하지 않은 타입의 객체가 저장되는것을 막고 저장된 객체를 꺼내올때 원래의 타입과 다른 타입으로 잘못 형변환되어 발생할 수 있는 오류를 줄여준다.

## 지네릭 클래스 선언

```java
class Box<T> {
	T item;

	void setItem(T item){
		this.item = item;
	}
	T getItem() { 
		return item; 
	}
}
```

* T : 타입변수, T가 아닌 다른것을 사용해도 된다. 이는 임의의 참조형 타입을 의미한다.
* 기존에는 Object로 참조변수를 많이 사용했는데 그로인해 형변환이 불가피 했다 허나 이젠 Object 대신 원하는 타입을 지정하기만 하면 된다.

### 타입을 지정해주지 않을때

```java
Box b = mew Box(); // T가 Object로 간주되어 허용된다.
b.setItem("ABC"); // 경고
```

* 위처럼 지네릭이 도입되기 이전의 코드와의 호환을 위해 예전방식으로 객체를 생성하는것이 허용된다만, 타입을 지정하지 않아 안전하지 않다는 경고가 표시된다.
* 왠만하면 반드시 타입을 지정해주자

### 매개변수와의 유사성

* Box\<String>과 Box\<Integer>는 지네릭 클래스 Box\<T>에 서로 다른 타입을 대입해 호출한 것일 뿐, 이 둘이 별개의 클래스를 의미하지 않는다. (같은 클래스라는 말이다.)
* 컴파일 후에 둘다 모두 이들의 원시타입인 Box로 바뀐다. 지네릭 타입이 제거된다는 의미이다.

### 지네릭 클래스의 제한

* static 멤버에 타입변수 T를 사용할 수 없다.
  * T는 인스턴스 변수로 간주되는데 static 멤버는 인스턴스 변수를 참조할 수 없다.
  * static멤버는 타입이 동일한 것이여야 한다. 어떤 객체에서 호출해도 모두 동일하게 동작하며 공유되기 때문이다.
* 지네릭 타입의 배열을 생성하는것도 허용되지 않는다.
  * 그 이유는 new 연산자 때문인데 이 연산자는 컴파일 시점에 타입T가 뭔지 정확히 알아야한다. Box\<T>를 컴파일 하는 시점에 T가 어떤 타입이 될지 알수 없기 때문에 instanceof 도 같은 이유로 사용할수 없다

```java
class Box<T> {
	T[] itemArr;

	T[] toArray() {
		T[] tmpArr = new T[itemArr.length]; //에러 지네릭 배열 생성불가
		...
	}
}
```

## 지네릭 클래스의 객체 생성과 사용

#### Box.java

```java
import java.util.ArrayList;

public class Box<T> {
    ArrayList<T> list =new ArrayList<T>();

    void add(T item) { list.add(item); }
    T get (int i) { return list.get(i); }
    ArrayList<T> getList() { return list; }
    int size() { return list.size(); }
    public String toString() { return list.toString(); }

}
```

#### FruitBox.java

```java
public class FruitBox<T> extends Box<T>{
}
```

#### main.java

```java
public class Main {
    public static void main(String[] args) {
        Box<Apple> appleBox = new Box<Apple>();
        Box<Fruit> fruitBox = new Box<Fruit>();

        appleBox.add(new Apple());
        //당연하지만 Apple타입으로 생성한 참조변수니 Apple객체만 넣을수있다
        //appleBox.add(new Grape());

        //상속관계의 타입들 다 가능, void add(Fruit item)
        fruitBox.add(new Fruit());
        fruitBox.add(new Apple());
        fruitBox.add(new Grape());

        //참조변수와 생성자에 대입된 타입이 일치해야한다. 일치하지 않으면 에러
        //Box<Grape> appleBox2 = new Box<Apple>();

        //상속관계에 있더라도 에러
        //Box<Fruit> FruitBox = new Box<Apple>();

        //지네릭 클래스가 상속관계에 있는건 괜찮다
        Box<Apple> appleBox1 = new FruitBox<Apple>();
    }
}

class Fruit {
    public String toString(){
        return "Fruit";
    }
}
class Grape extends Fruit{
    public String toString(){
        return "Grape";
    }
}
class Apple extends Fruit{
    public String toString(){
        return "Apple";
    }
}
class Toy {
    public String toString(){
        return "Toy";
    }
}
```

## 지네릭 타입의 제거

* 컴파일러는 지네릭 타입을 이용해 소스파일을 체크 한뒤 필요한 곳에 형변환을 넣어준다. 그리고 지네릭 타입을 제거한다.
* 즉 컴파일된 .class 파일에는 지네릭 타입에 대한 정보가 없다.
* 그 이유는 지네릭이 도입되기 이전의 소스코드와의 호환성을 유지하기 위해서이다.
* 지네릭 타입의 제거 과정은 꽤 복잡하다. 기본적인 제거 과정만 알아보자.

1. 지네릭 타입의 경게를 제거한다. 지네릭 타입이 \<T extends Fruit> 라면 T는 Fruit로 치환되고 \<T>인 경우는 Object로 치환된다. 클래스옆의 선언은 제거된다.
2. 지네릭 타입을 제거한 후에 타입이 일치하지 않으면 형변환을 추가한다.

```java
 static Juice makeJuice (FruitBox<? extends Fruit> box) {
	String tmp = "";
	for(Fruit f : box.getList()) {
		 tmp += f + " ";
	}
	return new Juice(tmp);
}
```

* 위와같이 와일드 카드가 포함되어 있는 경우에 다음과 같이 적절한 타입으로의 형변환이 추가된다.

```java
 static Juice makeJuice (FruitBox box) {
	String tmp = "";
	Iterator it = box.getList().iterator();
	while(it.hasNext()) {
		 tmp += (Fruit)it.next() + " ";
	}
	return new Juice(tmp);
}
```
