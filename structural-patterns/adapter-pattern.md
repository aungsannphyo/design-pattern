# Adapter Pattern

Adapter Pattern ကဘယ်လိုအလုပ်လုပ်သလဲဆိုရင် မတူညီတဲ့ interface ရှိတဲ့ objects တွေကို အတူတကွအလုပ်လုပ်လို့ရအောင်ကြားခံအနေနဲ့ တံတားတစ်ခုလို လုပ်ဆောင်ပေးတာဖစ်တယ်။

### ဘယ်လိုနေရာတွေမှာအသုံး၀င်သလဲ?

ဥပမာဆိုပါတော့ ကျွန်တော်တို့ MacBook အားသွင်းကြိုးက 3pin ခေါင်းနဲ့လာတယ် အားသွင်းဖို့အတွက် ရှိတဲ့အပေါက်က 2pin ပဲရှိတယ်ဆိုပါတော့ အဲ့ကျကျွန်တော်တို့ဘယ်လိုအားသွင်းကြမလဲ 3pin to 2pin ပြောင်းပေးဖို့အတွက်ကြားခံ ပလပ်ခေါ်င်းလေးတပ်ပီးသုံးလိုက်မယ်ဆို အဆင်မပြေသွားဘူးလား adapter ဆိုတာကလဲအဲ့သလိုပဲဗျ မတူညီတဲ့ 
interface တွေက အချင်းချင်း communication လုပ်လို့မရပေမဲ့ ကြားကနေ adapter တစ်ခုထည့်လိုက်တာနဲ့ အဆင်ပြေအောင်လုပ်နိုင်ပါတယ်။


## Adapter မှာနှစ်မျိုးရှိတယ်

Object Adapter interface object တစ်ခုကနေ အခြား objects တွေကို composition လုပ်ချိန်မှာသုံးတယ်။
Class Adapter Inheritance လုပ်ချိန်မှာသုံးတယ်။

```java
public interface Socket {
    void input();
}

public class TwoPinSocket implements Socket {
    @Override
    public void input() {
        System.out.println("two pin input");
    }
}

public class Conversion {
    public void changeToThreePin() {
        System.out.println("changed to two pin => three pin input");
    }
}

public class ThreePinSocketObjectAdapter implements Socket {
    private final Conversion conversion;

    public ThreePinSocketObjectAdapter(Conversion conversion) {
        this.conversion = conversion;
    }

    @Override
    public void input() {
        conversion.changeToThreePin();
    }
}
```
```java
public abstract class AbstractSocket {
    public abstract void input();
}

public class ThreePinSocketClassAdapter extends Conversion implements Socket {
    @Override
    public void input() {
        changeToThreePin();
    }
}
```

```java
public class Main {
    public static void main(String[] args) {
        // Direct usage of two pin socket
        Socket twoPin = new TwoPinSocket();
        twoPin.input();

        // Object adapter usage
        Conversion conversion = new Conversion();
        Socket threePinObjectAdapter = new ThreePinSocketObjectAdapter(conversion);
        threePinObjectAdapter.input();

        // Class adapter usage (only works with inheritance-based model)
        Socket threePinClassAdapter = new ThreePinSocketClassAdapter();
        threePinClassAdapter.input();
    }
}
```

```csharp
two pin input
changed to two pin => three pin input
changed to two pin => three pin input
```

Adapter Pattern သုံးလိုက်လို့ဘာကောင်းသွားသလဲဆိုရင် Adapter business code နဲ့ တခြား code တွေကို ခွဲခြားထားနိုင်တယ် ဒါကြောင့် အသစ်ထည့်တာတွေဖြုတ်တာတွေပြင်တာတွေကို Interface and Objects ထိခိုက်မှု့မရှိပဲပြုပြင်ပြောင်းလဲနိုင်တယ်။ တချို့တချို့သော ကိစ္စတွေမှာ Adapter မလိုပဲတစ်ခါထဲအသုံးပြုနိုင်တဲ့အခြေအနေတွေရှိတယ် အဲ့လိုအခြေအနေတွေနဲ့ကြုံလာခဲ့လို့ Adapter တွေဆောက်မယ်ဆိုရင်မလိုအပ်ပဲ Code တွေဖောင်းပွလာနိုင်ပါတယ်။