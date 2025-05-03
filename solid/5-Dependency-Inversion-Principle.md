## Dependency Inversion Principle

 Dependency Inversion မှာအချက်နှစ်ခုရှိတယ်

1. High-level modules should not depend on low-level modules. Both should depend on abstractions.
2. Abstractions should not depend upon details. Details should depend on abstractions.

အဲ့တာနားလည်ရခက်တယ်မလား ကျွန်တော်သိတယ်။ သူတို့ကဘာကိုပြောချင်သလဲဆိုရင် Class or Object တွေရဲ့တစ်ခုနဲ့တစ်ခုမှီခိုမှူ့ကိုတက်နိုင်သလောက်လျှော့ချဖို့ အတွက် Abstractions ကိုပဲသုံးသင့်တယ်။ ဒီနေရာမှာပြောတဲ့ Abstractions ဆိုတာ  Interface or abstract class တွေလို့မြင်ထားရင်ရပီ။ DIP ကိုသုံးချင်အားဖြင့်ဘာတွေကောင်းလဲဆိုရင် Class or Object တွေရဲ coupling ဖစ်နေတာတွေကိုအတော်လျှော့နည်းသွားစေတယ်။ coupling ဆိုတာဘာလဲဆိုရင် Class or Object တစ်ခုဟာသူ့တာ၀န်တွေပီးမြှောက်ဖို့အတွက် ပြင်ပ Class or Object တွေကိုမှီခိုနေရတာကိုဆိုလိုတာပါ။ လုံး၀မမှီခိုရဘူးလို့မပြောပေမဲ့ coupling က မကောင်းတဲ့  pattern တစ်ခုဖစ်တယ်ဆိုတာသိထားရရင်ရပီ ဘာလို့လဲဆို နောက်ပိုင်း code တွေ refactor လုပ်ရတာအရမ်းခက်ခဲနိုင်လို့ပါပဲ။

ပိုမြင်သာသွားအောင်ဥပမာတစ်ခုပြောပြမယ်။ ပစ္စည်းတစ်ခု၀ယ်မယ်ဆိုပါဆို ပိုက်ဆံပေးတဲ့ချိန်မှာ CB and AYA နဲ့ပေးမယ် ကိုယ့်မှာ CB ပဲပါလာတယ် ကိုယ့်သူငယ်ချင်းမှာ AYA ပါလာတယ် ဘယ်လိုအခြေအနေဖစ်ဖစ် ငွေပေးချေရမှာပေးချေရမှာပဲ အဲ့တော့ CB and AYA ဆိုတဲ့ class တွေကိုမှီခိုစရာမလိုပဲ Payment Interface or abstract  ပေါ်ကိုပဲမှီခိုသင့်တယ်။

```java
interface IPaymentProcessor {
    void pay(double amount, String category);
}

class CBPaymentProcessor implements IPaymentProcessor {
    private String user;
    private CB cbPayment;

    public CBPaymentProcessor(String user) {
        this.user = user;
        this.cbPayment = new CB(user);
    }

    @Override
    public void pay(double amount, String category) {
        double transactionFee = 20;
        this.cbPayment.makePayment(amount + transactionFee, category);
    }
}

class AYAPaymentProcessor implements IPaymentProcessor {
    private String user;
    private AYA ayaPayment;

    public AYAPaymentProcessor(String user) {
        this.user = user;
        this.ayaPayment = new AYA(user);
    }

    @Override
    public void pay(double amount, String category) {
        double transactionFee = 20;
        this.ayaPayment.makePayment(amount + transactionFee, category);
    }
}

class CB {
    private String user;

    public CB(String user) {
        this.user = user;
    }
    public void makePayment(double amount, String category) {
        System.out.println(user + " bought " + category + " and made payment of " + amount + " with CB Bank.");
    }
}

class AYA {
    private String user;

    public AYA(String user) {
        this.user = user;
    }

    public void makePayment(double amount, String category) {
        System.out.println(user + " bought " + category + " and made payment of " + amount + " with AYA Bank.");
    }
}

class Store {
    private IPaymentProcessor paymentProcessor;

    public Store(IPaymentProcessor paymentProcessor) {
        this.paymentProcessor = paymentProcessor;
    }

    public void purchasePhone(int quantity) {
        paymentProcessor.pay(200 * quantity, "phone");
    }

    public void purchaseLaptop(int quantity) {
        paymentProcessor.pay(300 * quantity, "laptop");
    }
}

public class Main {
    public static void main(String[] args) {
        // CB Payment Processor Example
        Store cbStore = new Store(new CBPaymentProcessor("Batman"));
        cbStore.purchaseLaptop(1); // Batman bought laptop and made payment of 320.0 with CB Bank.
        cbStore.purchasePhone(2); // Batman bought phone and made payment of 440.0 with CB Bank.

        // AYA Payment Processor Example
        Store ayaStore = new Store(new AYAPaymentProcessor("Superman"));
        ayaStore.purchaseLaptop(3); // Superman bought laptop and made payment of 920.0 with AYA Bank.
        ayaStore.purchasePhone(2); // Superman bought phone and made payment of 440.0 with AYA Bank.
    }
}
```

