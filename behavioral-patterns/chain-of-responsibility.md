# Chain Of Responsibility(CoR)

CoR အကြောင်းမပြောခင် ဥပမာတစ်ခုပြောပြမယ် Vending Machine တစ်ခုရှိတယ်ဆိုပါစို့ Cosumer ကနေပီး အအေးဖစ်ဖစ် အာလူးကြော်ဖစ်ဖစ်၀ယ်တော့မယ် ဆိုရင် အရင်ဆုံးတန်ဖိုးအလိုက် ငွေသားထည့်ရမယ် အဲ့ချိန် အာလူးကြော် တန်ဖိုးက
3500 ဖစ်တယ်ဆို 3500 ထည့်ရမယ် အဲ့ချိန် 1000 သုံးရွက်နဲ့ 500 တန်တစ်ရွက်ဆို ငွေလက်ခံဖို့ စက်က 1000 တန်ဖစ်လား 500 တန်ဖစ်လားဆိုပီး သိဖို့လိုလာပီ အဲ့သလို condition နည်းနည်းလေးဆို မသိသာပေမဲ့ condition တွေများလာခဲ့ရင် if else or switch case မှာ code တွေများလာပီး တစ်ခုထည့်ဖို့ပြင်ဖို့ဆို သွားသွားပီး main business logic code တွေသွားထိတဲ့အတွက်
ရေရှည်မှာအဆင်မပြေနိုင်တော့ဘူး။ အဲ့လိုမျိုး အခြေအနေတွေမှာ CoR ကပိုအသုံး၀င်တယ်လို့ ပြောလို့ရတယ်။

```java
// Currency Handler Interface
public interface KyatHandler {
    void setNext(KyatHandler handler);
    void handle(int coin);
}

```

```java
// Base Abstract Handler
public abstract class BaseKyatHandler implements KyatHandler {
    protected KyatHandler nextHandler;

    @Override
    public void setNext(KyatHandler handler) {
        this.nextHandler = handler;
    }

    @Override
    public void handle(int kyat) {
        if (nextHandler != null) {
            nextHandler.handle(kyat);
        } else {
            System.out.println(  kyat + "Kyats is not accepted.");
        }
    }
}

```

```java
// Concrete Currency Handlers
public class FiveHundredKyatHandler extends BaseKyatHandler {
    @Override
    public void handle(int kyat) {
        if (kyat == 500) {
            System.out.println("Accepted: 500 kyat");
        } else {
            super.handle(kyat);
        }
    }
}


public class OneThousandKyatHandler extends BaseKyatHandler {
    @Override
    public void handle(int kyat) {
        if (kyat == 1000) {
            System.out.println("Accepted: 1000 kyat");
        } else {
            super.handle(kyat);
        }
    }
}
public class FiveThousandKyatHandler extends BaseKyatHandler {
    @Override
    public void handle(int kyat) {
        if (kyat == 5000) {
            System.out.println("Accepted: 5000 kyat");
        } else {
            super.handle(kyat);
        }
    }
}
public class TenThousandKyatHandler extends BaseKyatHandler {
    @Override
    public void handle(int kyat) {
        if (kyat == 10000) {
            System.out.println("Accepted: 10000 kyat");
        } else {
            super.handle(kyat);
        }
    }
}

```

```java
public class ATMDeposit {
    public static void main(String[] args) {
        CoinHandler handler500 = new FiveHundredKyatHandler();
        CoinHandler handler1000 = new OneThousandKyatHandler();
        CoinHandler handler5000 = new FiveThousandKyatHandler();
        CoinHandler handler10000 = new TenThousandKyatHandler();

        // Set up the chain
        handler500.setNext(handler1000);
        handler1000.setNext(handler5000);
        handler5000.setNext(handler10000);

        // Test with some kyats
        int[] kytas = {500, 1000, 50, 1000, 1000}; // 50 is invalid

        for (int kyat : kyats) {
            handler500.handle(coin);
        }
    }
}


```

```csharp
Accepted: 500 kyat
Accepted: 1000 kyat
50 kyat is not accepted.
Accepted: 1000 kyat
Accepted: 1000 kyat
```

CoR ကိုသုံးလိုက်လို့ဘာကောင်းသွားသလဲဆိုရင် တစ်ခုနဲ့တစ်ခုမှီခိုမှု့မရှိတဲ့အတွက် coupling ဖစ်တာအတော်လျှော့သွားစေတယ်။
ပီးတော့ handler တစ်ခုချင်းစီက သူတို့လုပ်ရမဲ့အရာကိုပဲတာ၀န်ယူထားရတဲ့အတွက် အသစ်ထည့်တာဖစ်ဖစ် ပြုပြင်ပြောင်းလဲဖို့ လိုတာပဲဖစ်ဖစ်လုပ်ရတာလွယ်သွားတယ်။
မကောင်းတဲ့အချက်ကိုပြောပါဆိုရင် လုပ်ငန်းစဥ်များလာလေလေ performance ကျနိုင်တယ်။