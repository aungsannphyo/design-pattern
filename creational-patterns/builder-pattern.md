# Builder Pattern

Builder Pattern ကလဲ creational patterns အောက်ကကောင်ပါပဲ။ အဓိကဘာလုပ်သလဲဆိုရင် 
complex ဖစ်တဲ့ object တွေကို အဆင့်ဆင့်တည်ဆောက်ရာမှာသုံးတယ်။

ဥပမာ ကျွန်တော်တို့ ဘာဂါဆိုင်ကိုသွားပီး ဘာဂါတစ်လုံးမှာမယ်ဆိုပါစို့ ဘာဂါဆိုဒ်က small, medium, large လားရှိမယ်၊ cheese ထည့်မလားမထည့်ဘူးလား၊​ bacon ထည့်မလားမထည့်ဘူးလား၊ ဆလပ်ရွက်ထည့်ချင်လားမထည့်ချင်ဘူးလား၊
ကြက်ဥရောထည့်မလား မထည့်ဘူးလား အဲ့သလိုရွေးချယ်စရာတွေအများကြီးဖစ်နေတာကို programming ရှု့ထောင့်ကနေကြည့်ရင် complex object လို့သတ်မှတ်လို့ရတယ်။ ဆိုတော့က Burger ဆိုတဲ့ object ကိုကျွန်တော်တို့ခေါ်သုံးတဲ့အခါ စောနက toppings တွေကို parameters တွေလို့မှတ်လို့ရမယ် အဲ့တော့ကိုယ်လို့ချင်တာပဲမှာတဲ့အခါ ပါတာကပါမယ် မပါတာကမပါဘူးဆိုတော့ နောက်ဆက်တွဲ ဥပမာ ဘာဘာဂါ ညာဘာဂါရယ်ဆိုပီး implement လုပ်ရတဲ့ချိန် အသစ်ထပ်ထည့်နေသ၍ implementation code တွေကပိုပိုဖောင်းပွလာနိုင်တယ်။ အဲ့လိုချိန်မှာ builder pattern ကပိုလို့အသုံး၀င်တယ်။ ဒီနေရာမှာတစ်ခုသတိထားရမှာက ဒီ pattern ကိုသုံးမယ်ဆို optional parameters များတဲ့ချိန် ပီးရင် immutable objects တွေကပိုလို့အဆင်ပြေလိမ့်မယ်။

builder အကြောင်းပြောမှာ ကြားထဲကတစ်ခုဖစ်တဲ့ director အကြောင်းပါတလက်စထဲထည့်ပြောလိုက်ပါအူးမယ်။
director ကအထူးအဆန်းတော့မဟုတ်ပါဘူး သူကဘာလုပ်ပေးသလဲဆိုရင် complex object တွေအတွက်အသုံးပြုထားတဲ့
builder တွေကို သူက control လုပ်ပီး ဘယ်လိုခေါ်မယ် ဘယ်လို order လုပ်မယ်ဆိုတာစီစဥ်ပေးတဲ့ကောင်ဖစ်တယ်။

```java
public interface FoodItem {
    String getName();
    double getPrice();
}
```
```java
public class Burger implements FoodItem {
    private final String size;
    private final boolean cheese;
    private final boolean bacon;
    private final int quantity;

    private Burger(Builder builder) {
        this.size = builder.size;
        this.cheese = builder.cheese;
        this.bacon = builder.bacon;
        this.quantity = builder.quantity;
    }

    public static class Builder {
        private String size = "Medium";
        private boolean cheese = false;
        private boolean bacon = false;
        private int quantity = 1;

        public Builder size(String size) {
            this.size = size;
            return this;
        }

        public Builder addCheese() {
            this.cheese = true;
            return this;
        }

        public Builder addBacon() {
            this.bacon = true;
            return this;
        }

        public Builder quantity(int quantity) {
            this.quantity = quantity;
            return this;
        }

        public Burger build() {
            return new Burger(this);
        }
    }

    @Override
    public String getName() {
        //do something
    }

    @Override
    public double getPrice() {
       //do something
    }
}

```

```java
public class BurgerDirector {
    private final Burger.Builder builder;

    public BurgerDirector(Burger.Builder builder) {
        this.builder = builder;
    }

    // Method to create a default burger
    public Burger createDefaultBurger() {
        return builder
                .size("Medium")
                .addCheese()
                .quantity(1)
                .build();
    }

    // Method to create a large burger with bacon
    public Burger createLargeBaconBurger() {
        return builder
                .size("Large")
                .addBacon()
                .quantity(2)
                .build();
    }

    // Method to create a custom burger
    public Burger createCustomBurger(String size, boolean cheese, boolean bacon, int quantity) {
        return builder
                .size(size)
                .addCheese(cheese)
                .addBacon(bacon)
                .quantity(quantity)
                .build();
    }
}
```

```java
public class Main {
    public static void main(String[] args) {
        // Using the builder directly
        Burger burger = new Burger.Builder()
                .size("Large")
                .addCheese()
                .addBacon()
                .quantity(2)
                .build();

        // Using the Director to create default burgers
        BurgerDirector director = new BurgerDirector(new Burger.Builder());

        // Default medium burger with cheese
        Burger defaultBurger = director.createDefaultBurger();
        
        // Large burger with bacon
        Burger largeBaconBurger = director.createLargeBaconBurger();

        // Custom burger
        Burger customBurger = director.createCustomBurger("Small", true, false, 3);
    }
}

```

builder ကိုနေရာတစ်ကာမှာ ခေါ်ခေါ်သုံးနေတာထက်စာရင် director ကအသင့်တော်ဆုံးပဲ director ကိုဘာကြောင့်သုံးသင့်သလဲဆိုရင် director ကိုကြားခံလိုက်ချင်းအားဖြင့် builder ထဲမှာရှိတဲ့ configuration logic တွေကို encapsulates လုပ်သလိုဖစ်စေတယ်။ builderလို တူညီတဲ့ အရာကိုထပ်တလဲလဲ object ဆောက်ဆောက်ပီး ခေါ်သုံးနေမဲ့အစား director ကိုကြားခံလိုက်တာက ပိုပီးအဆင်ပြေသလို တစ်ခုခု ပြုပြင်ပြောင်းလဲတာအတွက်လဲ ပိုပီးလွယ်ကူစေတယ်။ တစ်ခုသတိထားစေချင်တာ director မှာ implementation detail တွေမပါသင့်ဘူး ဘာလို့ဆို သူရည်ရွယ်ချက်ကိုက builder တွေကို order လုပ်ဖို့အတွက်ပဲမို့။
