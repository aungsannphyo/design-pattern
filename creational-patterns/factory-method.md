# Factory Method

### Factory Method ဆိုတာဘာလဲ?
Interface တစ်ခုပါမယ် အဲ့ Interface ကနေ Object တွေ Create လုပ်နိုင်တယ်။ Create လုပ်ထားတဲ့ Object ကနေ တစ်ဆင့်
Subclass တွေမှာလဲ ပြုပြင်ပြောင်းလဲခြင်းတွေပြုလုပ်နိုင်တယ်။ ဒီလိုမျိုး Object Creations တွေပါလို့ပဲ 
Factory Method က Creational Patterns ရဲ့အောက်ကိုရောက်သွားချင်းဖစ်တယ်။

### ဘယ်လိုနေရာတွေမှာအသုံး၀င်နိုင်သလဲ?
 ဘယ်အခြေအနေ ဘယ်လိုနေရာတွေမှာ သုံးရမလဲဆိုတာမပြောခင် ဥပမာတစ်ခုပြောပြမယ် ဆိုပါတော့ ကျွန်တော်တို့မှာ payment gateway နှစ်ခုရှိတယ်ဆိုပါတော့ KBZ and AYA ဘယ်လိုပုံစံနဲ့ရေးထားရေးထားအလုပ်ဖစ်တယ်ထားပါတော့ အဲ့ချိန်မှာ Client က CB payment ထက်ထည့်ချင်တယ် အဲ့လိုချိန်ကျရင် ကျွန်တော်တို့ KBZ ကိုပြောင်းလဲဖို့လဲလိုရင်လိုလာနိုင်တယ် အဲ့လိုမှမဟုတ် နှစ်ခုလုံးကိုပြောင်းလဲမှု့လိုတယ်ဆို code ကအတော်လေးရှုပ်ရှပ်ခက်လာနိုင်တယ် အဲ့တာကို tight coupling ဖစ်တယ်လို့ခေါ်တယ် tight coupling ဖစ်တဲ့ code တော်တော်များများက မကောင်းဘူးလို့ဆိုလို့ရတယ် ဘာကြောင့်ဆိုတစ်ခုကိုထိရင် ကျန်တာတွေပါလိုက်ထိတဲ့အတွက် Open/Close Principle ကိုလဲချိုးဖောက်ရာကျတယ်။
 ဒါကြောင့်အဲ့လိုအခြေအနေမျိုးနဲ့ကြုံလာရင် Factory Method ကအသုံး၀င်ဆုံးပဲဗျ။ ဥပမာအနေနဲ့ Payment Gateway,
 Mail Service, Notification Service။

 ### Key Concepts
 client ကနေလှမ်းခေါ်တဲ့ချိန်မှာ CB လား KBZ လားသိစရာမလိုပဲအသုံးပြုနိုင်အောင်လုပ်ပေးထားတယ်။
 ကိုယ်လိုချင်တဲ့ Servie ကို ပဲ inject လုပ်ပီးခေါ်သုံးလိုက်ရုံပဲ။
 အဓိက client ကခေါ်မဲ့ super slass ကိုအလုပ်မလုပ်စေပဲ subclass ကိုအလုပ်ပေးထားရုံပဲ၊ 
 ဘာကောင်းသွားသလဲဆိုတော့ creation code တွေက်ိုပါ encapsulate လုပ်သလိုဖစ်စေတယ်။

 ```typescript
 // Product Interface 
interface PaymentGateway {
  charge(amount: number) : void;
}
```
```typescript
// Concrete Product - KBZ
class KBZGateway implements PaymentGateway {
    charge(amount: number) {
        console.log(`Charging $${amount} with KBZ...`);
  }
}

// Concrete Product - AYA
class AYAGateway implements PaymentGateway {
    charge(amount: number) {
        console.log(`Charging $${amount} with AYA...`);
  }
}
```
```typescript
// Creator
abstract class PaymentGatewayFactory {
  abstract createGateway(): PaymentGateway;
}
```
```typescript
// Concrete Factory - KBZ
class KBZGatewayFactory extends PaymentGatewayFactory {
 createGateway(): PaymentGateway {
    return new KBZGateway();
  }
}

// Concrete Factory - AYA
class AYAGatewayFactory extends PaymentGatewayFactory {
  createGateway(): PaymentGateway {
    return new AYAGateway();
  }
}
```
```typescript
async function main() {
  
  const kbz = new KBZGatewayFactory().createGateway().charge(100); //Charging $100 with KBZ...

  const aya = new AYAGatewayFactory().createGateway().charge(100); //Charging $100 with CB...
}
```

```csharp
Client
  ↓
PaymentGatewayFactory (abstract)
  ↓
KBZGatewayFactory → KBZGateway (charge)
AYAGatewayFactory → AYAGateway (charge)
```

Factory ကိုသုံးခြင်းဖြင့်သူ့ရဲ့ကောင်းကျိုးတွေကဘာတွေလဲဆိုတော့ tight coupling ဖစ်တာကိုအတော်လျှော့သွားစေတဲ့အပြင် subclass တွေကဘယ်သူအပေါ်မှမှီခိုမှု့မရှိတော့ပဲ loose coupling ဖစ်စေတယ်။ business logic တွေကိုသူတို့နဲ့သက်ဆိုင်ရာမှာပဲရေးထားတဲ့အတွက် Single Responsibility ကိုလိုက်နာသလိုလဲဖစ်တဲ့အပြင်၊ အသစ်ထပ်ထည့်မဲ့ cb payment ကို PaymentGatewayFactory ကိုပဲ extend လုပ်ပီး အသစ်ထပ်ထည့်လိုက်ရုံပဲ အဲ့တော့ ရှိပီးသား code တွေကို ပြင်စရာမလိုတဲ့အတွက် Open/Closed Principle ကိုလဲလိုက်နာသလိုဖစ်တယ််။ 

မကောင်းတဲ့အချက်ကိုပြောပါဆိုရင်တော့ နှစ်ခုသုံးခုမကတော့ပဲ အများကြီး၀ိုင်းသုံးရမဲ့ကိစ္စတွေဖစ်လာပီဆိုရင်တော့ subclass ကတွေအများကြီးက interface တစ်ခုထဲကို ၀ိုင်းပီး implementation လုပ်ရတဲ့အတွက်က code base ကများလာတဲ့အလျှောက်
messy အရမ်းဖစ်လာနိုင်တယ်၊ ပီးတော့ product တစ်ခုချင်းစီက new concrete class and factory subclass တွေလိုအပ်တဲ့အတွက်
class explosion ဖစ်လာနိုင်ကာ တစ်ခုနဲ့တစ်ခု navigate လုပ်ဖို့ခက်ခဲလာနိုင်ပါတယ်။