# Chain Of Responsibility(CoR)

CoR အကြောင်းမပြောခင် ဥပမာတစ်ခုပြောပြမယ် Vending Machine တစ်ခုရှိတယ်ဆိုပါစို့ Cosumer ကနေပီး အအေးဖစ်ဖစ် အာလူးကြော်ဖစ်ဖစ်၀ယ်တော့မယ် ဆိုရင် အရင်ဆုံးတန်ဖိုးအလိုက် ငွေသားထည့်ရမယ် အဲ့ချိန် အာလူးကြော် တန်ဖိုးက
3500 ဖစ်တယ်ဆို 3500 ထည့်ရမယ် အဲ့ချိန် 1000 သုံးရွက်နဲ့ 500 တန်တစ်ရွက်ဆို ငွေလက်ခံဖို့ စက်က 1000 တန်ဖစ်လား 500 တန်ဖစ်လားဆိုပီး သိဖို့လိုလာပီ အဲ့သလို condition နည်းနည်းလေးဆို မသိသာပေမဲ့ condition တွေများလာခဲ့ရင် if else or switch case မှာ code တွေများလာပီး တစ်ခုထည့်ဖို့ပြင်ဖို့ဆို သွားသွားပီး main business logic code တွေသွားထိတဲ့အတွက်
ရေရှည်မှာအဆင်မပြေနိုင်တော့ဘူး။ အဲ့လိုမျိုး အခြေအနေတွေမှာ CoR ကပိုအသုံး၀င်တယ်လို့ ပြောလို့ရတယ်။

```typescript
// Currency Handler Interface
interface kyatHandler {
  setNext(handler: kyatHandler): kyatHandler;
  handle(coin: number): void;
}
```

```typescript
// Base Abstract Handler
abstract class BaseKyatHandler implements kyatHandler {
  private nextHandler: kyatHandler | null = null;

  setNext(handler: kyatHandler): kyatHandler {
    this.nextHandler = handler;
    return handler;
  }

  handle(kyat: number): void {
    if (this.nextHandler) {
      this.nextHandler.handle(kyat);
    } else {
      console.log(`${kyat} Kyat is not accepted.`);
    }
  }
}
```

```typescript
// Concrete Currency Handlers
class FiveHundredKyatHandler extends BaseKyatHandler {
  handle(kyat: number): void {
    if (kyat === 500) {
      console.log("Accepted: 500 kyat");
    } else {
      super.handle(kyat);
    }
  }
}

class OneThousandKyatHandler extends BaseKyatHandler {
  handle(kyat: number): void {
    if (kyat === 1000) {
      console.log("Accepted: 1000 kyat");
    } else {
      super.handle(kyat);
    }
  }
}

class FiveThousandKyatHandler extends BaseKyatHandler {
  handle(kyat: number): void {
    if (kyat === 5000) {
      console.log("Accepted: 5000 kyat");
    } else {
      super.handle(kyat);
    }
  }
}

class TenThousandKyatHandler extends BaseKyatHandler {
  handle(kyat: number): void {
    if (kyat === 10000) {
      console.log("Accepted: 10000 kyat");
    } else {
      super.handle(kyat);
    }
  }
}
```

```typescript
const fiveHunderHandler = new FiveHunderdKyatHandler()
const oneThousandKyatHandler = new OneThousandKyatHandler()
const fiveThousandKyatHandler = new FiveThousandKyatHandler()
const tenThousandKyatHandler = new TenThousandKyatHandler()

fiveHunderHandler.setNext(oneThousandKyatHandler)
                 .setNext(fiveThousandKyatHandler)
                 .setNext(tenThousandKyatHandler);

const kyats = [500, 1000, 1000, 1000, 50]; 

kyats.forEach(kyats => {
  fiveHunderHandler.handle(kyat);
});

```

```csharp
Accepted: 500 kyat
Accepted: 1000 kyat
Accepted: 1000 kyat
Accepted: 1000 kyat
50 kyat is not accepted.
```

CoR ကိုသုံးလိုက်လို့ဘာကောင်းသွားသလဲဆိုရင် တစ်ခုနဲ့တစ်ခုမှီခိုမှု့မရှိတဲ့အတွက် coupling ဖစ်တာအတော်လျှော့သွားစေတယ်။
ပီးတော့ handler တစ်ခုချင်းစီက သူတို့လုပ်ရမဲ့အရာကိုပဲတာ၀န်ယူထားရတဲ့အတွက် အသစ်ထည့်တာဖစ်ဖစ် ပြုပြင်ပြောင်းလဲဖို့ လိုတာပဲဖစ်ဖစ်လုပ်ရတာလွယ်သွားတယ်။
မကောင်းတဲ့အချက်ကိုပြောပါဆိုရင် လုပ်ငန်းစဥ်များလာလေလေ performance ကျနိုင်တယ်။