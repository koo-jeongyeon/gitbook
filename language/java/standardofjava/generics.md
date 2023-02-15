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

* Box.java
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

* FruitBox.java
```java
public class FruitBox<T> extends Box<T>{
}
```

* main.java
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

## 제한된 지네릭 클래스

* 매개변수 T에 지정할 수 있는 타입의 종류를 제한할 수 있는 방법
* 제한하지 않으면 모든 종류의 타입이 지정되기 때문에 fruitBox에 Toy를 담을수도 있다

### extends 사용

* FruitBoxExtendsFruit.java
```java
/**
 * 제한된 지네릭 클래스
 * Fruit타입만 타입 매개변수 T에 지정할 수 있다
 * 다형성에서 조상타입 참조변수로 자손타입 객체를 가리킬 수 있는것 처럼 Fruit와 그 자손 타입까지 가능하다
 * @param <T>
 */
public class FruitBoxExtendsFruit<T extends Fruit> extends Box {

    ArrayList<T> list = new ArrayList<T>();

}
```

* main.java
```java
FruitBoxExtendsFruit<Apple> appleFruitBox = new FruitBoxExtendsFruit<Apple>();
//Fruit클래스의 자손들만 담을 수 있다는 제약이 추가되어 Toy는 못담음
//FruitBox2<Toy> toyFruitBox = new FruitBox2<Toy>();

//여전히 상속관계의 타입들 다 가능함
FruitBoxExtendsFruit<Fruit> fruitBoxExtendsFruit = new FruitBoxExtendsFruit<Fruit>();
fruitBoxExtendsFruit.add(new Apple());
fruitBoxExtendsFruit.add(new Grape());

```

### Fruit와 Eatable 인터페이스 구현
* 인터페이스를 구현해야 한다는 제약이 존재한다면

* Eatable.interface
```java
public interface Eatable {
}

/**
 * 인터페이스도 타입 매개변수 T에 지정할 수 있다
 * extends를 사용하는 것에 주의
 * Fruit의 자손이면서 Eatable인터페이스도 구현해야한다면 다음처럼 & 기호로 연결한다
 * @param <T>
 */
public class FruitBoxExtendsFruitandEatable<T extends Fruit & Eatable> extends Box {

    ArrayList<T> list = new ArrayList<T>();

}
```

* Fruit의 자손이면서 Eatable을 구현한 클래스
```java
public class FruitImplEatable extends Fruit implements Eatable{

    public String toString(){
        return "Fruit";
    }
}
```

* main.java
```java
//Fruit의 자손이면서 Eatable을 구현하는 FruitImplEatable클래스를 타입으로 사용가능하다
FruitBoxExtendsFruitandEatable<FruitImplEatable> fr = new FruitBoxExtendsFruitandEatable<FruitImplEatable>();

fr.add(new Apple());
fr.add(new Grape());
```

## 와일드 카드

* `<? extends T>` : 와일드 카드의 상한 제한, T와 그 자손들만 가능
* `<? super T>` : 와일드 카드의 하한 제한, T와 그 조상들만 가능
* `<?>` : 제한 없음 모든 타입이 가능하다 `<? extends Object` 와 동일

### 외일드 카드의 필요성

* Juice.java
```java
public class Juice {
    String name;

    Juice(String name) {
        this.name = name + "Juice";
    }
    public String toString(){ return name; }
}
```

* Juicer.java
```java
public class Juicer {

    /**
     * 와일드 카드를 사용하면 FruitBox<Fruit>뿐만 아니라 Fruit<Apple>등 자손들도 사용가능하다.
     */
    static Juice makeJuice(FruitBox<? extends Fruit> box) {

        String tmp = "";
        for (Fruit f : box.getList()){
            tmp += f + " ";
        }
        return new Juice(tmp);
    }

    /**
     * 매개변수에 과일박스를 대입하면 주스를 만들어 반환하는 Juicer클래스
     * 이 클래스는 지네릭클래스도 아니고 static 은 타입 매개변수 T를 사용할 수 없다.
     * 그래서 Apple을 타입으로한 FruitBox를 매개변수로 넣어주면 에러가 발생..
     */
//    static Juice makeJuice(FruitBox<Fruit> box) {
//
//        String tmp = "";
//        for (Fruit f : box.getList()){
//            tmp += f + " ";
//        }
//        return new Juice(tmp);
//    }

    /**
     * Apple타입을 사용하기 위해서 오버로딩 코드를 짜면 에러가 발생한다.
     * 지네릭 타입이 다른것 만으로는 오버로딩이 성립하지 않기 때문이다.
     * 지네릭 타입은 컴파일 할때만 사용하고 제거하기 때문에 이는 메서드 중복 정의가 되어 에러가 난다.
     */
//    static Juice makeJuice(FruitBox<Apple> box) {
//
//        String tmp = "";
//        for (Fruit f : box.getList()){
//            tmp += f + " ";
//        }
//        return new Juice(tmp);
//    }
}
```

* main.java
```java
FruitBox<Fruit> fruit_FruitBox = new FruitBox<Fruit>();
FruitBox<Apple> apple_FruitBox = new FruitBox<Apple>();

//Fruit와 그 자손인 Apple도 가능함
System.out.println("Juicer.makeJuice(fruit_FruitBox) = " + Juicer.makeJuice(fruit_FruitBox));
System.out.println("Juicer.makeJuice(apple_FruitBox) = " + Juicer.makeJuice(apple_FruitBox));
```

### Collections.sort()를 이용한 정렬

* Fruit
```java
class Fruit {

    String name;
    int weight;

    Fruit(String name, int weight){
        this.name = name;
        this.weight = weight;
    }
    public String toString(){
        return name+"{"+weight+"}";
    }
}

public class FruitComp implements Comparator<Fruit> {

    public int compare(Fruit t1, Fruit t2){
        return t1.weight = t2.weight;
    }
}
```

* Apple
```java
class Apple extends Fruit {
    Apple(String name, int weight) {
        super(name, weight);
    }

    public String toString(){
        return name+"{"+weight+"}";
    }
}

public class AppleComp implements Comparator<Apple> {

    public int compare(Apple t1, Apple t2){
        return t2.weight - t1.weight;
    }
}
```

* main.java
```java
public class Main {
    public static void main(String[] args) {

        FruitBox<Apple> appleBox = new FruitBox<Apple>();

        appleBox.add(new Apple("GreenApple", 300));
        appleBox.add(new Apple("GreenApple", 100));
        appleBox.add(new Apple("GreenApple", 200));

        Collections.sort(appleBox.getList(), new AppleComp());
        System.out.println("appleBox = " + appleBox);

        Collections.sort(appleBox.getList(), new FruitComp());
        System.out.println("appleBox = " + appleBox);

    }
}
```

이는 Collections.sort()를 이용해 appleBox에 담긴 과일을 무게별로 정렬하는 것이다.
Collections의 선언부는 다음과 같다

```java
static <T> void sort (List<T> list, Comparator<? super T> c)
```
이는 지네릭 메서드이다. list는 정렬할 대상, c는 정렬할 방법이 정의 된 Comparator이다.
지금 와일드 카드가 사용되어 `new FruitComp`로도 Apple을 정렬할 수 있다.
만일 와일드 카드를 사용하지 않는다면 Apple은 `Comparator<Apple>`로 Grape는 `Comparator<Grape>`로만 정렬할 수 있을것이다.
새로운 과일이 생길때마다 ~Comp.java를 만들어줄수는 없으니 와일드카드로 하한 제한을 해주는것이다.

T에 Apple이 대입되면 다음과 같다
```java
static <Apple> void sort (List<T> list, Comparator<? super Apple> c)
```
`Comparator<? super Apple>`은 Comparator의 타입 매개변수로 Apple과 그 조상이 가능하다는거다.
그래서 `new FruitComp`로 다른 과일들도 정렬가능하다. 

몰론 과일의 조상을 Fruit로 상속해주어야 한다.


## 지네릭 메서드

* 메서드 선언부에 지네릭 타입이 선언된 메서드를 지네릭 메서드라 한다.
* 지네릭 클래스에 정의된 타입 매개변수와 지네릭 메서드에 정의 된 매개변수는 전혀 별개의 것이다. 
* static 멤버에는 타입 매개변수를 사용할 수 없지만 메서드에 지네릭 타입을 선언하고 사용하는 것은 가능하다.
* 이 타입 매개변수는 메서드 내에서만 지역적으로 사용되기 때문에 지역변수를 선언한 것과 같다고 생각하면 이해하기 쉽다. 그렇기에 static 이든 아니든 상관이 없다.

* makeJuice를 지네릭 메서드로 바꾸면 다음과 같다.
```java
 static <T extends Fruit>Juice makeJuice (FruitBox<T> box) {
	String tmp = "";
	for(Fruit f : box.getList()) {
		 tmp += f + " ";
	}
	return new Juice(tmp);
}
```

* 이 메서드를 호출할 땐 아래와 같이 타입 변수에 타입을 대입해야 한다.
* 하지만 대부분의 경우 컴파일러가 타입을 추정할 수 있어 생략해도 된다. 
* 한 가지 주의할 점은 지네릭 메서드를 호출할 때 대입된 타입을 생략할 수 없는 경우에는 참조변수나 클래스 이름을 생략할 수 없다는 것이다. 단지 기술적인 이유이므로 지켜야한다. 
```java
FruitBox<Fruit> fruit_FruitBox = new FruitBox<Fruit>();
FruitBox<Apple> apple_FruitBox = new FruitBox<Apple>();

//Fruit와 그 자손인 Apple도 가능함
System.out.println("Juicer.makeJuice(fruit_FruitBox) = " + Juicer.<Fruit>makeJuice(fruit_FruitBox));
System.out.println("Juicer.makeJuice(apple_FruitBox) = " + Juicer.<Apple>makeJuice(apple_FruitBox));
```

* 매개변수의 타입이 복잡할 때 유용하게 사용가능하다.
```java
 static void printAll (ArrayList<? extends Product> list,
                       ArrayList<? extends Product> list2) {
    for (Unit u : list){
        System.out.println(u);
    }
}
//위의 코드를 간략하게 변경
 static <T extends Product> void printAll (ArrayList<T> list,
                                           ArrayList<T> list2) {
    for (Unit u : list){
        System.out.println(u);
    }
}
```

## 지네릭 타입의 형변환

* 지네릭 타입과 지네릭 타입이 아닌 타입간의 형변환은 항상 가능하다. 
* 하지만 대입된 타입이 다른 지네릭 타입 간에는 형변환이 불가하다. 
```java
Box<Object> objBox = null;
Box<String> strBox = null;

objBox = (Box<Object>) strBox; //에러

//이미 아래와 같은 방식으로 생성이 불가능하다는 것을 배웠다 이는 위처럼 형변환이 불가능하다는 것을 간접적으로 알려준다.
Box<Object> objBox = new Box<String>();

//하지만 다음의 문장은 형변환이 가능하다
Box<? extends Object> objBox = new Box<String>();
//반대의 경우는 성립하지만 확인되지 않은 형변환이라는 경고가 발생한다.
FruitBox<? extends Fruit> box = null;
Box<Apple> appleBox = (FruitBox<Apple>) box;
```

### Optional 클래스
```java
public final class Optional<T> {

    //EMPTY에 비어있는 Optional 객체를 생성해서 저장
    private static final Optional<?> EMPTY = new Optional<>();
    //<?> -> <? extends Object>를 줄여쓴것
    //<>안에 생략된 타입은 <Object>이다
    //따라서 풀어쓰면 Optional<? extends Object> EMPTY = new Optional<Object>();
    private final T value;

    //EMPTY를 형변환해서 반환
    //EMPTY의 타입을 Optional<Object>가 아닌 Optional<?>로 한 이유는 Optional<T>로 형변환이 가능하기 때문이다. 만약 Optional<Object>면 형변환이 불가능하다. 
    public static<T> Optional<T> empty() {
        Optional<T> t = (Optional<T>) EMPTY;
        return t;
    }
}
```
* 정리하면 `Optional<Object>`를 `Optional<String>`으로 직접 형변환 하는것은 불가능하지만 와일드 카드가 포함된 지네릭 타입으로 형변환 하면 가능하다는 것이다.
* 참고로 와일드 카드가 사용된 지네릭 타입끼리도 다음과 같은 경우에 형변환이 가능하다. 다만 미확정 타입으로 형변환 하는 것이라는 경고가 뜬다.
```java
FruitBox<? extends Object> objBox = null;
FruitBox<? extends String> strBox = null;

strBox = (FruitBox<? extends String>) objBox;
objBox = (FruitBox<? extends Object>) strBox;
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
