### Strategy Pattern

Strategy Pattern ကဘယ်လိုလဲဆိုတော့ အလွယ်ကူဆုံးပြောရရင် runtime မှာ Class or Algorithm တို့ရဲ့လုပ်ငန်းတာ၀န်တွေကို ရွေးချယ်နိုင်အောင်လုပ်ပေးထားတဲ့ pattern တစ်ခုလို့ပြောလို့ရမယ်။ သူကဘယ်လိုအလုပ်လုပ်သလဲ ဆိုရင် ရှိတဲ့ algorithm အမျိုးမျိုးကို တစ်စုတစ်စည်းထဲသတ်မှတ်လိုက်ပီး တစ်ခုချင်းစီကို encapsulate လုပ်ကာ၊ အပြန်အလှန်အသုံးပြုနိုင်အောင် ပြုလုပ်ထားတာဖစ်ပါတယ်။
နည်းနည်းတော့ရှုပ်သွားတယ်မလား ကျွန်တော် ဥပမာတစ်ခု ပြောပြပေးမယ်။

ဥပမာ ကျွန်တော်တို့ online ကနေတစ်ခုခု၀ယ်ပီး ငွေပေးချေတဲ့ချိန်ရောက်ရင် ကျွန်တော်တို့၀ယ်တဲ့ website က ငွေလက်ခံတာ Kpay ရှိမယ်၊ MPU ရှိမယ်၊ တခြား payment တွေဖစ်တဲ့ Dinger ရှိမယ် သုံးခုရှိမယ်ဆိုပါစို့ သူတို့သုံးခုက တစ်ခုနဲ့တစ်ခု လုပ်ရမဲ့ အလုပ်တွေက မတူကြဘူးဟုတ်တယ်မလား ပြောရရင် တစ်ကောင်ချင်းစီရဲ့ business logic တွေမတူကြဘူး အဲ့လိုနေရာတွေမှာ ဒီ pattern ကအသုံး၀င်တယ်လို့ပြောလို့ရမယ်။ ကျွန်တော်တို့အောက်မှာ code နဲ့တစ်ချက်ကြည့်လိုက်ရအောင်။


```java
// Strategy interface
interface PaymentStrategy {
    void pay(double amount);
}
```

```java
// Concrete Strategy
class CreditCardPayment implements PaymentStrategy {
    private String cardNumber;

    public CreditCardPayment(String cardNumber) {
        this.cardNumber = cardNumber;
    }

    @Override
    public void pay(double amount) {
        System.out.println("Paid " + amount + " MMK using Credit Card: " + cardNumber);
    }
}

class KPayPayment implements PaymentStrategy {
    private String phoneNumber;

    public KPayPayment(String phoneNumber) {
        this.phoneNumber = phoneNumber;
    }

    @Override
    public void pay(double amount) {
        System.out.println("Paid " + amount + "MMK using Kpay: " + phoneNumber);
    }
}

class DingerPayment implements PaymentStrategy {
    private String accountNumber;

    public DingerPayment(String accountNumber) {
        this.accountNumber = accountNumber;
    }

    @Override
    public void pay(double amount) {
        System.out.println("Paid " + amount + "MMK using Dinger: " + accountNumber);
    }
}

```

```java
// Context class
class ShoppingCart {
    private PaymentStrategy paymentStrategy;

    public void setPaymentStrategy(PaymentStrategy paymentStrategy) {
        this.paymentStrategy = paymentStrategy;
    }

    public void checkout(double totalAmount) {
        if (paymentStrategy == null) {
            System.out.println("No payment method selected.");
        } else {
            paymentStrategy.pay(totalAmount);
        }
    }
}
```

```java
public class Main {
    public static void main(String[] args) {
        ShoppingCart cart = new ShoppingCart();

        // User selects Credit Card
        cart.setPaymentStrategy(new CreditCardPayment("1234-5678-9012-3456"));
        cart.checkout(100000.0);

        // User changes to Kpay
        cart.KPayPasetPaymentStrategyyment(new KPayPayment("09-2030111"));
        cart.checkout(150000.0);

        // User pays with Dinger
        cart.setPaymentStrategy(new DingerPayment("1A1zP1eP5QGefi2DMPTfTL5SLmv7DivfNa"));
        cart.checkout(250000.0);
    }
}
```

```csharp
Paid 100000.0MMK using Credit Card: 1234-5678-9012-3456
Paid 150000.0MMK using Kpay: 09-2030111
Paid 250000.0MMK using Dinger: 1A1zP1eP5QGefi2DMPTfTL5SLmv7DivfNa
```

အပေါ်က example ကိုကြည့်မယ်ဆိုရင် ShoppingCart ကဘယ် payment နဲ့ပေးတယ်ဆိုတာသိစရာမလိုတဲ့အတွက် main business logic တွေကို encapsulate လုပ်ထားသလိုလဲဖစ်တယ် ပီးတော့ payment method ပြောင်းလဲသွားတယ်ဆိုရင်တောင် သူ့ကိုပြင်စရာမလိုတဲ့အတွက် Open/Close Principle ကိုလိုက်နာသလိုလဲဖစ်တယ်။ အကယ်၍ ဒီနေရာမှာသာ strategy ကိုမသုံးခဲ့ဘူးဆိုရင် if else or switch case ကို payment method တစ်ခုချင်းစီအတွက်လိုအပ်လာခဲ့ပီ အဲ့ကျ အသစ်ထည့်ချင်တာဖစ်ဖစ် ဖြုတ်ချင်တာဖစ်ဖစ် checkout method သွားသွားထိနေတဲ့အတွက်ရေရှည်မှာအဆင်မပြေနိုင်ဘူး strategy ကိုသုံးလိုက်ချင်းအားဖြင့် အသစ်တစ်ခုခုထပ်ထည့်မယ်ဆိုရင် inject ထိုးလိုက်ရုံနဲ့ အကုန်လုံးအဆင်ပြေသွားမှာဖစ်တယ်။အောက်မှာ ဥပမာပြထားပေးပါတယ်။

```java
class ShoppingCart {
    private String paymentType;
    private String paymentDetail;

    public ShoppingCart(String paymentType, String paymentDetail) {
        this.paymentType = paymentType;
        this.paymentDetail = paymentDetail;
    }

    public void checkout(double amount) {
        if (paymentType.equalsIgnoreCase("CreditCard")) {
            System.out.println("Paid $" + amount + " using Credit Card: " + paymentDetail);
        } else if (paymentType.equalsIgnoreCase("Kpay")) {
            System.out.println("Paid " + amount + "MMK using Kpay: " + paymentDetail);
        } else if (paymentType.equalsIgnoreCase("Dinger")) {
            System.out.println("Paid " + amount + "MMK using Dinger: " + paymentDetail);
        } else {
            System.out.println("Invalid payment method.");
        }
    }
}

// Client code
public class Main {
    public static void main(String[] args) {
        ShoppingCart cart1 = new ShoppingCart("CreditCard", "1234-5678-9012-3456");
        cart1.checkout(100.0);

        ShoppingCart cart2 = new ShoppingCart("Kpay", "09-2030111");
        cart2.checkout(200.0);

        ShoppingCart cart3 = new ShoppingCart("Dinger", "1A1zP1...");
        cart3.checkout(300.0);
    }
}
```

ဒီနေရာမှာတစ်ခုသတိထားရမှာက တချို့ case တွေမှာ algorithms ကရိုးရှင်းပီး interchangeable လုပ်ဖို့မလိုတဲ့အခါဖစ်ဖစ် polymorphism သုံးပီးဖြေရှင်းလို့ရတဲ့အခြေအနေတွေမှာ ဆိုရင်တော့မသုံးသင့်ဘူးဗျ ဘာဖစ်လို့လဲဆိုရင် class တွေများလာလေလေ complex အရမ်းဖစ်လာပီ အဆင်မပြေဖစ်လာနိုင်ပါတယ်။