# Adapter Pattern

Adapter Pattern ကဘယ်လိုအလုပ်လုပ်သလဲဆိုရင် မတူညီတဲ့ interface ရှိတဲ့ objects တွေကို အတူတကွအလုပ်လုပ်လို့ရအောင်ကြားခံအနေနဲ့ တံတားတစ်ခုလို လုပ်ဆောင်ပေးတာဖစ်တယ်။

### ဘယ်လိုနေရာတွေမှာအသုံး၀င်သလဲ?

ဥပမာဆိုပါတော့ ကျွန်တော်တို့မှာ ဖုန်းနှစ်လုံးရှိတယ်ဆိုပါစို့ တစ်လုံးက iPhone နောက်တစ်လုံးက Android ဖုန်းတွေအားသွင်းဖို့အတွက် လျှပ်စစ်မီးရှိပါရဲ့နဲ့ ဒီတိုင်းသွင်းလို့ကမရဘူး ဟုတ်တယ်မလား သူတို့ကိုအားသွင်းဖို့ အားသွင်းကြိုးဆိုတာလိုလာပီ၊ အဲ့တော့ အားသွင်းကြိုးနဲ့သွင်းလိုက်တာနဲ့ လျှပ်စစ်မီးကနေတိုက်ရိုက် အားသွင်းတာမဟုတ်ပဲ အားသွင်းကြိုးက
တစ်ဆင့်ကြားခံသွင်းလိုက်တာဖစ်ပါတယ်။ Adapter ဆိုတာကလဲထိုနည်းလည်းကောင်းပါပဲ။

ပိုပီးမြင်သာအောင်ပြောပြရရင် Old System တစ်ခုရှိမယ် သူကဘာတွေလုပ်နေရသလဲဆို ပုံမှန််လုပ်ပေးနေကြအရာတစ်ခုရှိမယ်ဆိုပါစို့ ဥပမာ doSomething() အဲ့မှာနောက်ထပ် system တစ်ခုရောက်လာခဲ့မယ် သူ့ကိုတော့ New System ပေါ့ သူကဘာလုပ်မလဲဆို doSomething() ကိုမလုပ်ချင်ဘူး doNothing() ပဲလုပ်ချင်တာဆိုအဆင်မပြေတော့ဘူး အဲ့တော့သူအတွက်အဆင်ပြေအောင် ကြားထဲကနေ adapter တစ်ခု 
doNothingToSomething() ဆိုပီးထည့်လိုက်ချင်းအားဖြင့် နဂိုရှိပီးသား interface and adapter တွဲသုံးပီးအလုပ်လုပ်သွားစေတာဖစ်ပါတယ်။

Adapter မှာနှစ်မျိုးရှိတယ် 

**Object Adapter**
interface object တစ်ခုကနေ အခြား objects တွေကို composition လုပ်ချိန်မှာသုံးတယ်။

```java
//Target Interface
public interface Charger {
    void charge();
}
```

```java
//Adaptee
public class IPhone {
    public void chargeWithLightning() {
        System.out.println("Charging iPhone with Lightning cable");
    }
}

public class AndroidPhone implements Charger {
    @Override
    public void charge() {
        System.out.println("Charging Android phone with USB-C cable");
    }
}
```

```java
//Adapter using Composition
public class IPhoneObjectAdapter implements Charger {
    private IPhone iPhone;

    public IPhoneObjectAdapter(IPhone iPhone) {
        this.iPhone = iPhone;
    }

    @Override
    public void charge() {
        iPhone.chargeWithLightning();
    }
}
```
```java
//client
public class ObjectAdapterDemo {
    public static void main(String[] args) {
        Charger androidPhone = new AndroidPhone();
        androidPhone.charge();

        IPhone iPhone = new IPhone();
        Charger iPhoneAdapter = new IPhoneAdapter(iPhone);
        iPhoneAdapter.charge();
    }
}
```

```csharp
Charging Android phone with USB-C cable
Charging iPhone with Lightning cable
```

**Class Adapter**
Inheritance လုပ်ချိန်မှာသုံးတယ်။


```java
//Target Interface
public interface Charger {
    void charge();
}
```

```java
//Adaptee
public class IPhone {
    public void chargeWithLightning() {
        System.out.println("Charging iPhone with Lightning cable");
    }
}

public class AndroidPhone implements Charger {
    @Override
    public void charge() {
        System.out.println("Charging Android phone with USB-C cable");
    }
}
```

```java
//Adapter using Inheritance
public class IPhoneClassAdapter extends IPhone implements Charger {
    @Override
    public void charge() {
        chargeWithLightning();
    }
}
```
```java
//client
public class ClassAdapterDemo {
    public static void main(String[] args) {
        Charger charger = new IPhoneClassAdapter();
        charger.charge(); // Charging iPhone with Lightning cable
    }
}
```

Adapter Pattern သုံးလိုက်လို့ဘာကောင်းသွားသလဲဆိုရင် Adapter business code နဲ့ တခြား code တွေကို ခွဲခြားထားနိုင်တယ် ဒါကြောင့် အသစ်ထည့်တာတွေဖြုတ်တာတွေပြင်တာတွေကို Interface and Objects ထိခိုက်မှု့မရှိပဲပြုပြင်ပြောင်းလဲနိုင်တယ်။ တချို့တချို့သော ကိစ္စတွေမှာ Adapter မလိုပဲတစ်ခါထဲအသုံးပြုနိုင်တဲ့အခြေအနေတွေရှိတယ် အဲ့လိုအခြေအနေတွေနဲ့ကြုံလာခဲ့လို့ Adapter တွေဆောက်မယ်ဆိုရင်မလိုအပ်ပဲ Code တွေဖောင်းပွလာနိုင်ပါတယ်။
