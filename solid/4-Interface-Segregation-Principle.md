## Interface Segregation Principle (ISP)

Interface Segregation ကဘာလဲဆိုရင် Interface တွေကိုတစ်နေရာထဲမှာစုပြုံမရေးစေပဲ သက်ဆိုင်ရာသက်ဆိုင်ရာအလိုက်ခွဲထုတ်ရေးစေချင်တာ။အဲ့လိုမှမဟုတ်ရင် သူနဲ့မသက်ဆိုင်တဲ့အရာတွေကိုပါ implement လုပ်နေရရင် Single Responsibility စည်းကမ်းကိုလည်းချိုးဖောက်သလိုဖစ်သွားမှာပါ။



**Bad**

```typescript
interface IVehicle {
getSpeed() : number;
getVehicleType: string;
isTaxPayed() : boolean;
isLightsOn() : boolean;
isLightsOff() : boolean;
startEngine() : void;
acelerate() : number;
stopEngine() : void;
startRadio() : void;
playCd : void;
stopRadio() : void;
}
```



**Good**

```typescript
interface IVehicle {
getSpeed() : number;
getVehicleType: string;
isTaxPayed() : boolean;
isLightsOn() : boolean;
}

interface ILights {
isLightsOn() : boolean;
isLightsOff() : boolean;
}

interface IRadio {
startRadio() : void;
playCd : void;
stopRadio() : void;
}

interface IEngine {
startEngine() : void;
acelerate() : number;
stopEngine() : void;
}
```

