# Single Responsibility Principle (SRP)

Class တစ်ခုမှာသူနဲ့သက်ဆိုင်တဲ့ တာ၀န်တစ်ခုသာရှိရမယ်ဖစ်တယ်။ အဲ့ Class ကိုတစ်ခုခုပြောင်းလဲမှုပြုလုပ်ဖို့အတွက် အကြောင်းပြချက်တစ်ခုထက်မပိုရဘူး ။ဥပမာပြောရရင် UserSettings Class မှာဆို User Setting နဲ့ပက်သက်တဲ့ အရာတွေပဲရှိသင့်တယ် ကျန်တဲ့အရာတွေမရှိသင့်ဘူး။ Car Class မှာဆို Car နဲ့သက်ဆိုင်တာတွေပဲရှိသင့်တယ် Bike နဲ့ပက်သက်တာတွေမရှိသင့်ဘူး။

**Bad :**

```typescript
class UserSettings {
  constructor(user) {
    this.user = user;
  }

  changeSettings(settings) {
    if (this.verifyCredentials()) {
      // ...
    }
  }

  verifyCredentials() {
    // ...
  }
}

```



**Good :**

```typescript
class UserAuth {
  constructor(user) {
    this.user = user;
  }

  verifyCredentials() {
    // ...
  }
}

class UserSettings {
  constructor(user) {
    this.user = user;
    this.auth = new UserAuth(user);
  }

  changeSettings(settings) {
    if (this.auth.verifyCredentials()) {
      // ...
    }
  }
}

```

