## Interface Segregation Principle (ISP)

Interface Segregation ကဘာလဲဆိုရင် Interface တွေကိုတစ်နေရာထဲမှာစုပြုံမရေးစေပဲ သက်ဆိုင်ရာသက်ဆိုင်ရာအလိုက်ခွဲထုတ်ရေးစေချင်တာ။ဥပမာအနေနဲ့ဆိုရရင်



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

