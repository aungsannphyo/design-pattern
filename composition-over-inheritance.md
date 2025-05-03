# Composition over Inheritance

Composition အကြောင်းကိုမပြောခင် Inheritance အကြောင်းကိုအရင်ပြောပြမှဖစ်မယ်ဗျ။ What is Inheritance?  Inheritance ဆိုတာ အလွယ်ပြောရရင် အမွေဆက်ခံတယ်ပေါ့။ ဥပမာ ပြောပြရရင် မိဘတွေရဲ့ပိုင်ဆိုင်မှု့အားလုံးကို သူတို့မွေးထားတဲ့သားသမီးတွေ အကုန်ပိုင်ဆိုင်ခွင့်ရှိတယ်။ Programming အနေနဲ့ပြောရရင် အထူးသဖြင့် OOP language တွေမှာဆိုရင် Parent Class ကို inheritance လုပ်ထားတဲ့ Child Class တွေအားလုံးက မိဘထဲမှာရှိတဲ့အရာတွေကို လှမ်းခေါ်သုံးနိုင်မှာဖစ်တယ်။ 

```java
class Animal {
    int age;

    public Animal(int age) {
        this.age = age;
    }

    public void eat() {
        System.out.println("Eating");
    }

    public void sleep() {
        System.out.println("Sleeping");
    }

    public void walk() {
        System.out.println("Walking");
    }
}

class Dog extends Animal {
    public Dog(int age) {
        super(age);
    }
}

class Cat extends Animal {
    public Cat(int age) {
        super(age);
    }
}

public class Main {
    public static void main(String[] args) {
        Dog dog = new Dog(3);
        System.out.println("Dog age: " + dog.age); // Dog age: 3
        dog.eat(); // Eating
        dog.sleep(); // Sleeping
        dog.walk(); // Walking
    }
}

```

အပေါ်က Example မှာဆိုရင်  Dog Class and Cat Class က Animal Class ကို extends (inheritance) လုပ်ထားတဲ့အတွက်မိခင် Classဖစ်တဲ့ Animal Classထဲကအရာတွေကိုခေါ်သုံးလို့ရတာဖစ်တယ်။ အခုထိ So far So good Right? Problem ကအခုမှလာမှာ အကယ်၍ကျွန်တော်တို့က နောက်ထပ် Class တစ်ခုတိုးမယ်ဆိုပါစို့ ဥပမာ Fish Class ပေါ့သူကလဲ Animal ဖစ်တဲ့အတွက် Animal Class ကို အမွေဆက်ခံမယ်ဆိုရင် အဲ့မှာကျွန်တော်တို့စဥ်းစားရပီ Fish က လမ်းလျှောက်တဲ့ Function သူ့အတွက် မလိုဘူး ဟုတ်တယ်မလား ငါးတွေကလမ်းမှမလျှောက်တက်တာ သူလိုတာက swim() ရေကူးတဲ့ Function လိုတာ ဟုတ်တယ်ဟုတ်? အဲ့တော့ကျွန်တော်တို့ရဲ့ Design ကိုပြောင်းဖို့လိုလာပီဘယ်လိုပြောင်းလဲဆိုတာအောက်မှာ ဆက်ကြည့်လိုက်ရအောင်

```java
 class Animal {
    int age;

    public Animal(int age) {
        this.age = age;
    }

    public void eat() {
        System.out.println("Eating");
    }

    public void sleep() {
        System.out.println("Sleeping");
    }
}

class Dog extends Animal {
    public Dog(int age) {
        super(age);
    }

    public void walk() {
        System.out.println("Dog is Walking");
    }
}

class Cat extends Animal {
    public Cat(int age) {
        super(age);
    }

    public void walk() {
        System.out.println("Cat is Walking");
    }
}

class Fish extends Animal {
    public Fish(int age) {
        super(age);
    }

    public void swim() {
        System.out.println("Fish is Swimming");
    }
}

public class Main {
    public static void main(String[] args) {
        Fish fish = new Fish(1);
        System.out.println(fish.age);  // 1
        fish.swim();                   // "Fish is Swimming"
        fish.eat();                    // "Eating"
        fish.sleep();                  // "Sleeping"

        Dog dog = new Dog(3);
        System.out.println(dog.age);   // 3
        dog.eat();                     // "Eating"
        dog.sleep();                   // "Sleeping"
        dog.walk();                    // "Dog is Walking"
    }
}

```

Inheritance ရဲ့ Architecture ကအရမ်းမိုက်ပါတယ် ကုတ်တွေအများကြီးရေးစရာမလိုပဲသူ့ကိုအမွေဆက်ခံထားတဲ့ Child Class တွေကနေအကုန်လှမ်းသုံးလို့ရတယ်။ code တွေကိုလဲကြည့်ရဖတ်ရတာအဆင်ပြေတယ်  ပြောရရင် Flexible and Maintainable ဖစ်တယ်  ၊ code base များတဲ့ချိန်ဆို code base နည်းသွားအောင်တူညီတဲ့ Class တွေကို ခွဲထုတ်ပီးရေးလို့ရတယ် သူ့ကောင်းကျိုးတွေအနေနဲ့အကြမ်းဖျင်းအားဖြင့်အဲ့တာတွေပေါ့။ 

အရာရာတိုင်းမှာအကောင်းရယ်အဆိုးရယ်ဆိုပီး ဒွန်တွဲနေတက်တော့ ဒုတိယ Example လိုအခြေအနေနဲ့ကြုံလာရင် code က ပိုဖောင်းဖွလာနိုင်တယ် ဘာလို့ဆို Dog and Cat က walk() လမ်းလျှောက်ရမဲ့ Function ကိုရှိကိုရှိရမဲ့အခြေအနေမှာ Parent ဖစ်တဲ့ Animal မှာရေးရင်ရနေပီကို သူတို့တစ်ကောင်ချင်းစီမှာ လာရေးထားရတယ်။ အဓိကအချက်ကတော့ Child Class ဖစ်သူတွေက Parent Classတွေကိုမှိခိုနေရတဲ့အတွက် အကယ်၍ ကျွန်တော်တို့က Parent Animal Class ထဲကတစ်ခုခုကို ပြောင်းသည်ဖစ်စေ ဖျက်သည်ဖစ်စေ တစ်ခုခုများ အပြောင်းအလဲရှိခဲ့ရင် Child Class တွေကိုတိုက်ရိုက်ထိတဲ့အတွက် နောက်ပိုင်း Maintain ဖို့တွေခက်လာနိုင်တယ်။ 

အဲ့လိုမျိုး နောက်ဆက်တွဲ ဆိုးကျိုးတွေရှိလာနိုင်တဲ့အခြေအနေမျိုးတွေဆိုရင် Inheritance အစား Composition ကိုပဲသုံးသင့်တယ်ဗျ။

Composition ဆိုတာ OOP ရဲ့အခြေခံ concepts တစ်ခုပဲ မြင်သာအောင်ပြောရရင် ငယ်ငယ်ကဆော့ခဲ့ဖူးတဲ့ အရုပ်ဆက်တဲ့အရာလေးတွေဆိုပါဆို ကိုယ်လိုချင်တဲ့ ကျားပုံဖစ်စေ စက်ရုပ်ပုံဖစ်စေ အပိုင်းအစသေးသေးလေးတွေကို ဆက်ပီးနောက်ဆုံးကိုယ်ဆက်တဲဲ့အရုပ်ပုံလေးထွက်လာတယ်တယ်။



Inheritance ဆိုတာက "is-a" ပြောရရင်	**Car is a vehicle.**

Composition ဆိုတာက "has a" ပြောရရင် **The car *has a* steering wheel.**



```java
interface Sleep {
    void sleep();
}

interface Eat {
    void eat();
}

interface Walk {
    void walk();
}

interface Swim {
    void swim();
}

class Dog implements Sleep, Eat, Walk {

    public void sleep() {
        // Implementation
    }

    public void eat() {
        // Implementation
    }

    public void walk() {
        // Implementation
    }
}

class Fish implements Sleep, Eat, Swim {

    public void sleep() {
        // Implementation
    }

    public void eat() {
        // Implementation
    }

    public void swim() {
        // Implementation
    }
}

```

တကယ်လို့ SOLID principle ကိုသိမယ်ဆိုရင် interface segregation principle ကိုလဲသိမယ်ထင်ပါတယ်။ အပေါ်က Example မှာဆိုရင် interface segregation principle ကိုသုံးပီး လုပ်ဆောင်ချက်တစ်ခုချင်းဆီကို Interface တစ်ခုချင်းဆီခွဲထုတ်ပီးရေးထားလိုက်တယ်ပီးတော့ သူတို့သက်ဆိုင်ရာ သက်ဆိုင်ရာ Class တစ်ခုချင်းဆီမှာ သူတို့လိုအပ်တဲ့ လုပ်ဆောင်ချက်တွေကို implements လုပ်ပေးလိုက်သောအားဖြင့် Parent and Child ဆိုပီး မှီခိုမှု့မရှိတဲ့အတွက် ပိုပီး ပြုပြင်ထိန်းရလွယ်ကူတယ်။

```java
interface Swim {
    void swim();
}

interface Walk {
    void walk();
}

abstract class Animal {
    String name;
    int age;

    public Animal(String name, int age) {
        this.name = name;
        this.age = age;
    }

    public void sleep() {
        System.out.println("Sleeping");
    }

    public void eat() {
        System.out.println("Eating");
    }
}

class Dog extends Animal implements Walk {

    public Dog(String name, int age) {
        super(name, age);
    }

    @Override
    public void walk() {
        System.out.println("Walking");
    }
}

class Fish extends Animal implements Swim {

    public Fish(String name, int age) {
        super(name, age);
    }

    @Override
    public void swim() {
        System.out.println("Swimming");
    }
}
```
အနှစ်ချုပ်အနေနဲ့ ပြောရရင် Inheritance ရဲ့ အားသာချက်က code တွေကိုအများကြီးရေးစရာမလိုပဲပြန်သုံးနေလို့ရတယ် မကောင်းတဲ့အချက်အနေနဲ့ဆို တစ်ခုနဲ့တစ်ခုမှီခိုနေတဲ့အတွက် Parent ကိုထိရင် Child ကိုပါလာထိနိုင်တယ်။ Composition ရဲ့အားသားချက်ကိုပြောရရင် တစ်ခုနဲ့တစ်ခုမမှီခိုဘူး ပိုပီး Flexibility ဖစ်တယ်။ မကောင်းတဲ့အချက်အနေနဲ့ဆိုရင်နည်းနည်းနားလည်ရခက်တယ် code base များလာတဲ့အလျှောက်။

ဘယ်လိုအခြေအနေမှာ Inheritance ကိုသုံးမလဲ Composition ဆိုတာ ကိုယ်ကြုံလာတဲ့ အရာပေါ်မူတည်ပီး "is a" လား "has a" လားဆိုတာကိုယ့်ရဲ့ requirements ပေါ်မူတည်ပီးသုံးစေချင်ပါတယ်။
