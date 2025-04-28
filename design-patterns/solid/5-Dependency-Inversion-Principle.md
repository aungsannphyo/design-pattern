## Dependency Inversion Principle

 Dependency Inversion မှာအချက်နှစ်ခုရှိတယ်

1. High-level modules should not depend on low-level modules. Both should depend on abstractions.
2. Abstractions should not depend upon details. Details should depend on abstractions.

အဲ့တာနားလည်ရခက်တယ်မလား ကျွန်တော်သိတယ်။ သူတို့ကဘာကိုပြောချင်သလဲဆိုရင် Class or Object တွေရဲ့တစ်ခုနဲ့တစ်ခုမှီခိုမှူ့ကိုတက်နိုင်သလောက်လျှော့ချဖို့ အတွက် Abstractions ကိုပဲသုံးသင့်တယ်။ ဒီနေရာမှာပြောတဲ့ Abstractions ဆိုတာ  Interface or abstract class တွေလို့မြင်ထားရရင်စေချင်ပါတယ်။ DIP ကိုသုံးချင်အားဖြင့်ဘာတွေကောင်းလဲဆိုရင် Class or Object တွေရဲ coupling ဖစ်နေတာတွေကိုအတော်လျှော့နည်းသွားစေတယ်။ coupling ဆိုတာဘာလဲဆိုရင် Class or Object တစ်ခုဟာသူ့တာ၀န်တွေပီးမြှောက်ဖို့အတွက် ပြင်ပ Class or Object တွေကိုမှီခိုနေရတာကိုဆိုလိုတာပါ။ လုံး၀မမှီခိုရဘူးလို့မပြောပေမဲ့ coupling က မကောင်းတဲ့  pattern တစ်ခုဖစ်တယ်ဆိုတာသိထားရရင်ရပီ ဘာလို့လဲဆို နောက်ပိုင်း code တွေ refactor လုပ်ရတာအရမ်းခက်ခဲနိုင်လို့ပါပဲ။

ပိုမြင်သာသွားအောင်ဥပမာတစ်ခုပြောပြမယ်။ ပစ္စည်းတစ်ခု၀ယ်မယ်ဆိုပါဆို ပိုက်ဆံပေးတဲ့ချိန်မှာ CB and AYA နဲ့ပေးမယ် ကိုယ့်မှာ CB ပဲပါလာတယ် ကိုယ့်သူငယ်ချင်းမှာ AYA ပါလာတယ် ဘယ်လိုအခြေအနေဖစ်ဖစ် ငွေပေးချေရမှာပေးချေရမှာပဲ အဲ့တော့ CB and AYA ဆိုတဲ့ class တွေကိုမှီခိုစရာမလိုပဲ Payment Interface or abstract  ပေါ်ကိုပဲမှီခိုသင့်တယ်။

```typescript
interface IPaymentProcessor {
    pay : (amount : number, category : string) =>void
}

class CBPaymentProcessor implements IPaymentProcessor{
    user:string;
    cbPayment : CB;

    constructor(user:string){
        this.user = user;
        this.cbPayment = new CB(user);
    }
    pay(amount : number, category: string){
        const transactionFee : number= 20;
        this.cbPayment.makePayment(amount + transactionFee,category)
    }
}

class AYAPaymentProcessor implements IPaymentProcessor{
    user:string;
    cbPayment : AYA;

    constructor(user:string){
        this.user = user;
        this.cbPayment = new AYA(user);
    }
    pay(amount : number, category: string){
        const transactionFee : number= 20;
        this.cbPayment.makePayment(amount + transactionFee,category)
    }
}

class CB {
    user : string;
    constructor(user : string){
        this.user = user;
    }

    makePayment(amount : number, category : string){
        console.log(`${this.user} buy ${category} and made payment ${amount} with CB Bank`)
    }
}


class AYA {
     user : string;
    constructor(user : string){
        this.user = user;
    }

    makePayment(amount : number, category : string){
        console.log(`${this.user} buy ${category} and made payment ${amount} with AYA Bank`)
    }
}

class Store {
   private paymentProcessor : CBPaymentProcessor | AYAPaymentProcessor;
    constructor(paymentProcessor:CBPaymentProcessor | AYAPaymentProcessor){
        this.paymentProcessor = paymentProcessor;
    }

    purchasePhone(quantity : number){
        this.paymentProcessor.pay(200 * quantity,'phone')
    }

    purchaseLaptop(quantity : number){
        this.paymentProcessor.pay(300 * quantity, 'laptop')
    }
}

const cb = new Store(new CBPaymentProcessor('Batman'))
cb.purchaseLaptop(1);
cb.purchasePhone(2);


const aya = new Store(new AYAPaymentProcessor('Superman'))
aya.purchaseLaptop(3);
aya.purchasePhone(2);
```

