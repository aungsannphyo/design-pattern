# Decorator Pattern

Object တစ်ခုဆီကို behaviors နဲ့ responsibilities တွေ dynamically အသစ်ထည့်ချင်တဲ့ချိန်မှာ နဂိုရှိပီးသား object ကိုထိခိုက်မှု့မရှိပဲ ပေါင်းထည့်လို့ရအောင် ပြုလုပ်ပေးတာဖစ်ပါတယ်။

ကဲဒါဆို အရင်ဆုံးကျွန်တော်ဥပမာတစ်ခုပြောပြပေးမယ် ကျွန်တော်တို့မှာ Coffee ဆိုင်တစ်ခုရှိမယ် အဲ့တာကို ပင်မ class လို့မြင်လိုက်မယ် ပီးရင် ဆိုင်ထဲမှာလဲသက်ဆိုင်ရာပစ္စည်းတွေရှိမယ် ပြောရရင် ကော်ဖီဖျော်စက်ကို method, coffee အစေ့ကို variable တွေလို့မြင်ထားလိုက်မယ်၊ ကော်ဖီဆိုင်ဆိုတော့ ဘာရောင်းမလဲ coffee ပဲရောင်းမှာပေါ့ ထားပါ menu ကတစ်ခုပဲရှိတယ်လို့ ဒီတိုင်း normal coffee ပေါ့။အဲ့အထိအဆင်ပြေသေးတယ်မလား? အဲ့မှာကျွန်တော်တို့က normal coffeeထဲကို ပြောရရင် milk နဲ့ sugar ဆိုပီးအသစ်ထည့်ချင်လာပီ အဲ့တာဆို ကျွန်တော်တို့က မူလက coffee class ကို extend လုပ်မယ် ပီးရင် လိုချင်တာတွေကို sub class ထပ်ရေးမယ်။ အဲ့တာဆို client ဘက်ကသုံးချင်တဲ့ချိန်မှာတစ်ခုချင်းဆီ ကိုလိုက်ပီး instantiate လုပ်ဖို့လိုလာပီ။

ဆိုတော့က တစ်ခါထဲနဲ့အကုန်လုံးကို လုပ်လို့မရနိုင်ဘူးလားဆိုတော့ရတယ် ဘယ်လိုလဲဆိုတော့ sub class တွေထဲမှာတခြား sub class တွေပါလာရေးထားတဲ့နည်းနဲ့ မြင်အောင်ပြောပြရရင် normal coffee ထဲမှာ milk feature ပါလာရေးထားသလိုပေါ့ အဲ့ကျဘာဖစ်သလဲဆို ကျွန်တော်တင်ထားတဲ့ composition over inheritance ဆောင်းပါးကိုဖတ်ထားဖူးရင်သိပါလိမ့်မယ် သူတို့က class တွေဆိုတော့ inheritance လုပ်ထားရမယ် တစ်ကောင်ကိုထိရင်ကျန်တဲ့ကောင်တွေပါလိုက်ထိ နိုင်တယ် ပီးတော့ class တစ်ခုက responsibilities တစ်ခုထက်မကပိုလာတဲ့အတွက် SRP ကိုလဲချိုးဖောက်သလိုဖစ်တယ်။

ဒီလိုချိန်မှာ decorator ကိုသုံးသင့်တယ် ဘာကြောင့် decorator ကိုသုံးသင့်သလဲဆို 
ကိုယ်ရဲ့ class က **is-a** လား **has-a** လားဆိုတာကြောင့်ပါ။ ဆိုတော့က decorator ကဘယ်လိုအလုပ်လုပ်သလဲဆို object composition ပုံစံနဲ့အလုပ်လုပ်ပါတယ်။ပိုပီးမြင်သာသွားအောင် အောက်မှာကျွန်တော် ဥပမာ code ရေးပြထားပေးပါမယ်။

```java
// Component
public interface Coffee {
    String getDescription();
    double getCost();
}
```

```java
// Concrete Component
public class BasicCoffee implements Coffee {
    @Override
    public String getDescription() {
        return "Basic Coffee";
    }

    @Override
    public double getCost() {
        return 2.0;
    }
}
```

```java
// Abstract Decorator
public abstract class CoffeeDecorator implements Coffee {
    protected Coffee decoratedCoffee;

    public CoffeeDecorator(Coffee coffee) {
        this.decoratedCoffee = coffee;
    }

    public String getDescription() {
        return decoratedCoffee.getDescription();
    }

    public double getCost() {
        return decoratedCoffee.getCost();
    }
}
```

```java
// Concrete Decorators
public class MilkDecorator extends CoffeeDecorator {
    public MilkDecorator(Coffee coffee) {
        super(coffee);
    }

    @Override
    public String getDescription() {
        return super.getDescription() + ", Milk";
    }

    @Override
    public double getCost() {
        return super.getCost() + 0.5;
    }
}

public class SugarDecorator extends CoffeeDecorator {
    public SugarDecorator(Coffee coffee) {
        super(coffee);
    }

    @Override
    public String getDescription() {
        return super.getDescription() + ", Sugar";
    }

    @Override
    public double getCost() {
        return super.getCost() + 0.3;
    }
}
```
```java
// Client Code
public class Main {
    public static void main(String[] args) {
        Coffee myCoffee = new BasicCoffee();                          // Basic Coffee
        myCoffee = new MilkDecorator(myCoffee);                      // + Milk
        myCoffee = new SugarDecorator(myCoffee);                     // + Sugar

        System.out.println(myCoffee.getDescription());               // Output: Basic Coffee, Milk, Sugar
        System.out.println("Total Cost: $" + myCoffee.getCost());    // Output: Total Cost: $2.8
    }
}
```

ဘယ်လိုအချိန်တွေမှာသုံးသင့်သလဲဆိုရင် objects တစ်ခုချင်းဆီရဲ့ responsibilities ကို class တစ်ခုလုံးမှာ အသစ်ထည့်ချင်တာတာမဟုတ်တဲ့ချိန်၊
subclass တွေအများကြီးအသစ်ဆောက်ဆောက်နေရတာကို avoid လုပ်ချင်တဲ့ချိန်တွေရယ် composition over inheritance ကိုသုံးချင်တဲ့ချိန်မှာအကောင်းဆုံးပါပဲ။
ဘာတွေကောင်းသွားသလဲဆိုရင် static inheritanceလုပ်တာထက်စာရင် ပိုပီး code က flexible ဖစ်သွားတယ်၊ ရှိပီးသား code တွေကို modify လုပ်စရာမလိုတဲ့အတွက် Open/Close principle ကိုလိုက်နာသလိုဖစ်သွားတယ်။

တစ်ခုသတိထားရမှာက code base များလာတဲ့အလျှောက် subclass တွေကြောင့် complicated ပိုပီးဖစ်လာနိုင်တယ်။ decorator များတဲ့တာနဲ့ order လုပ်တာတွေမမှတ်မိနိုင်တဲ့အချိန်မှာ ထုတ်ရဖျက်ရ ခက်တဲ့ issue တွေအပြင် တခြား unexpected results တွေလဲထွက်လာနိုင်ပါတယ်။