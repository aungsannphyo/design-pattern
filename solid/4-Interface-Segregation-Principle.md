## Interface Segregation Principle (ISP)

Interface Segregation ကဘာလဲဆိုရင် Interface တွေကိုတစ်နေရာထဲမှာစုပြုံမရေးစေပဲ သက်ဆိုင်ရာသက်ဆိုင်ရာအလိုက်ခွဲထုတ်ရေးစေချင်တာ။အဲ့လိုမှမဟုတ်ရင် သူနဲ့မသက်ဆိုင်တဲ့အရာတွေကိုပါ implement လုပ်နေရရင် Single Responsibility စည်းကမ်းကိုလည်းချိုးဖောက်သလိုဖစ်သွားမှာပါ။



**Bad**

```java
public interface IVehicle {
    int getSpeed(); 

    String getVehicleType(); 

    boolean isTaxPaid(); 

    boolean isLightsOn();

    boolean isLightsOff(); 

    void startEngine(); 

    int accelerate(); 

    void stopEngine(); 

    void startRadio(); 

    void playCd(); 

    void stopRadio(); 
}

```



**Good**

```java

public interface IVehicle {
    int getSpeed(); 
    String getVehicleType(); 
    boolean isTaxPaid(); 
}


public interface ILights {
    boolean isLightsOn(); 
    boolean isLightsOff(); 
}


public interface IRadio {
    void startRadio(); 
    void playCd(); 
    void stopRadio(); 
}


public interface IEngine {
    void startEngine(); 
    int accelerate(); 
    void stopEngine(); 
}
```

