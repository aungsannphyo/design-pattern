# Observer Pattern

Observer Pattern ဆိုတာသူ့ Defination ကိုပြောပြရရင် objects တွေတစ်ခုနဲ့တစ်ခုကြားမှာ one-to-many မှီခိုနေပီးတော့ object တစ်ခုက အပြောင်းအလဲတစ်ခုခုဖစ်တာနဲ့ ကျန်မှီခိုနေတဲ့ကောင်တွေကို notified လုပ်ပီး automatically updated ဖစ်စေတယ်။
  
### ဘယ်လိုအလုပ်လုပ်သလဲ?
ကျွန်တော်တို့မှာ notification system တစ်ခုရှိမယ် user တွေကို notify လုပ်ချင်ပေမဲ့ ဘယ် user တွေကိုလုပ်မလဲဆိုမသိဘူး ပီးတော့ တစ်ခါထဲနဲ့ အကုန်လုံးကိုလဲ ပို့ချင်တဲ့ချိန်မှာ observer ကပိုအသုံး၀င်တယ်လို့ပြောလို့ရမယ်။ အောက်က ဥပမာ မှတော့ user သုံးလေးယောက်နဲ့ပဲ ရေးပြထားလိုက်ပါတယ်။

```java
public interface Observer {
    void update(String message);
}
```
```java
import java.util.ArrayList;
import java.util.List;

class Notifier {
    private final List<Observer> observers = new ArrayList<>();

    public void subscribe(Observer observer) {
        observers.add(observer);
    }

    public void sendNotification(String message) {
        for (Observer observer : observers) {
            observer.update(message);
        }
    }
}
```
```java
public class User implements Observer{
    private final String name;

    public User(String name) {
        this.name = name;
    }

    public void update(String message) {
        System.out.println(name + " received notification: " + message);
    }
}
```
```java
public class Main {
    public static void main(String[] args) {
        Notifier notifier = new Notifier();

        Observer alice = new User("Alice");
        Observer bob = new User("Bob");
        Observer charlie = new User("Charlie");

        notifier.subscribe(alice);
        notifier.subscribe(bob);
        notifier.subscribe(charlie);

        notifier.sendNotification("System maintenance at 10 PM tonight.");
    }
}
```
```java
Alice received notification: System maintenance at 10 PM tonight.
Bob received notification: System maintenance at 10 PM tonight.
Charlie received notification: System maintenance at 10 PM tonight.
```

observer pattern ကိုဘယ်လိုအချိန်တွေမှာသုံးသင့်သလဲဆိုရင် ဘယ် object လဲသိစရာမလိုပဲ notify လုပ်ချင်တဲ့ချိန်၊
object တစ်ခုနဲ့တစ်ခု decouple လုပ်ထားချင်တဲ့ချိန်၊ ကိုယ်မှာ တစ်ခုထက်မကပဲ update သိဖို့လိုတဲ့နေရာတွေမှာသုံးရင်ပိုအဆင်ပြေတယ်။ 
ပီးတော့ သူက subject and observer ကြားမှာ loose coupling ဖစ်တဲ့အတွက် real time မှာအသစ်ထည့်တာဖစ်ဖစ် ဖြုတ်တာဖစ်ဖစ် အဆင်ပြေတယ်။ တစ်ခုသတိထားရမှာက observer တွေများလာတဲ့အလျှောက် ပိုပီး complex ဖစ်လာနိုင်တယ် ပီးတော့ observer lifecycle ကို manage လုပ်ရတာ ခက်ခဲလာနိုင်တယ်။