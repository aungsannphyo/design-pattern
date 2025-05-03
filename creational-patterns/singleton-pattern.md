# Singleton Pattern

Singleton Pattern က Design Pattern တွေထဲမှာအလွယ်ကူဆုံးနဲ့အရိုးရှင်းဆုံးလို့ပြောလို့ရမယ်ဗျ
ဆိုတော့က ဒီ Pattern ကဘယ်လိုအလုပ်လုပ်သလဲဆိုရင်် မြင်သာအောင်ပြောရရင် ဥပမာ
customer တစ်ယောက်က တစ်ခုခုကို order မှာတဲ့ချိန် ဆိုင်က customer မှာတဲ့အရာ ရှိပီးသားဆိုရင်
အဲ့အရာကိုပေးလိုက်မယ် မရှိသေးဘူးဆို အသစ်တစ်ခုကို ဖန်တီးပေးလိုက်မယ် အဲ့သဘောပဲ ကဲ အဲ့တော့
singleton pattern မှာမဖစ်မနေပါရမဲ့အရာ တစ်ခုရှိတယ် instance အဲ့ instance က 
**Private** ဖစ်ကိုဖစ်ရမယ် ပီးတော့ အဲ့ instance ကို Global Access ရနေရမယ်။
အပေါ်ကပြောတာတွေက Singleton Pattern ရဲ့အကြောင်းအရာတွေပဲဗျ သူကိုအဓိက ဘယ်လိုနေရာတွေမှာ
သုံးကြသလဲဆိုရင် များသောအားဖြင့် Database Connections, Logging Mechanisms,Configuration Settings.

```java
public class Singleton {
    private static Singleton instance = null;

    // Private constructor to prevent instantiation
    private Singleton() {
        System.out.println("Singleton instance created");
    }

    // Public method to provide access to the instance
    public static synchronized Singleton getInstance() {
        if (instance == null) {
            instance = new Singleton();
        }
        return instance;
    }

    public void sayHello() {
        System.out.println("Hello from Singleton!");
    }
}
```
အပေါ်က ဥပမာမှာဆိုရင် Singleton Class ရဲ့ constructor က Private ဖစ်နေရမယ် အဲ့တာမှ
သူကို အပြင်ကနေ လှမ်းပီး instantiate လုပ်လို့မရမှာဖစ်တယ်။ ရည်ရွယ်ချက်ကိုက သူတို့ကိုပေးမှမလုပ်ချင်တာကိုး။
Static Method ကနေပီး instance created ဖစ်သလားစစ်မယ် မဖစ်ရင် အသစ်ဆောက်မယ် ဖစ်ရင် ရှိပီးသားကိုပဲ
ပြန်ပေးလိုက်မယ်။

Static Method အသုံးပြုပုံအခေါ်အ၀ေါ်ကို Lazy Initialization လို့ခေါ်တယ်ဗျ။ Lazy Initialization ဆိုတာဘာကိုပြောချင်တာလဲဆိုရင် **အမှန်တကယ်လိုအပ်တဲ့ချိန်မရောက်သေးသ၍ Object Creation အပိုင်းကို Delay လုပ်ထားတာဖစ်တယ်** ဘာတွေပိုပီးကောင်မွန်သွားသလဲဆိုရင် saves memory, startup time ကို လျှော့ချပေးနိုင် စစချင်း Object Creation မလုပ်တဲ့အတွက်။ 

Singleton Class ရဲ့ constructor က private ဖစ်ရမယ်ဆိုရင် မေးစရာကရှိလာပီ ဥပမာ Dependency Injection ကိစ္စအတွက်ကျဘယ်လိုလုပ်ကြမလဲပေါ့။အောက်မှာ ဥပမာနဲ့ပြထားပေးပါမယ်။

```java
public class Service {
    private final Singleton singletonInstance;

    public Service(Singleton singleton) {
        this.singletonInstance = singleton;
    }

    public void performAction() {
        this.singletonInstance.sayHello();
    }

    public static void main(String[] args) {
        Singleton singletonInstance = Singleton.getInstance();
        Service service = new Service(singletonInstance);
        service.performAction();
    }
}

```
ကဲဒါဆို နည်းနည်း Advance ပိုင်းလေးသွားလိုက်ရအောင်လား။ အပေါ်ကပြောခဲ့သမျှအရာအားလုံးက Single-Thread အတွက်ပဲဖစ်တယ်ဗျ ကဲဒါဆို အခုနောက်ပိုင်းလူပြောများ လူသုံးများလာတဲ့ **Multi-Thread** အတွက်ကျဘယ်လိုလုပ်မလဲပေါ့ ဆိုတော့က ဘယ်လိုလုပ်မလဲဆိုတာမပြောခင် Multi-Thread သုံးလိုက်ရင် ဘယ်လို issue တွေရှိနိုင်လဲအရင်ပြောကြတာပေါ့။
Multi-thread က getInstance() ကိုတပြိုင်ထဲခေါ်ချလိုက်တဲ့ချိန်မှာ null ဖစ်မဖစ်စစ်တဲ့ condition ကိုစစ်တယ် ပီးရင် instance နှစ်ခုကိုတစ်ပြိုင်ထဲ create လုပ်လိုက်တယ် အဲ့တာမှာပြဿနာတက်တာဗျ ဘာလို့ဆို ကျွန်တော်တို့က ရှိပီးသားဆိုပေးမယ် မရှိမှ create လုပ်မယ် တစ်ခုထဲကိုပဲထပ်တလဲလဲသုံးစေချင်တာကိုး ဒီတော့က Singleton Pattern ကိုချိုးဖောက်ရာလဲကျသွားရော။

```java

public class Singleton {
    private static volatile Singleton instance = null;

    private Singleton() {
        // Simulate some initialization if needed
    }

    public static Singleton getInstance() {
        if (instance == null) {
            synchronized (Singleton.class) {
                if (instance == null) {
                    try {
                        // Simulate delay like async context switching
                        Thread.sleep(1);
                    } catch (InterruptedException e) {
                        Thread.currentThread().interrupt(); // good practice
                    }
                    instance = new Singleton();
                }
            }
        }
        return instance;
    }
}


```
အပေါ်က ဥပမာမှာဆိုရင် instance ကို null ဖစ်မဖစ်စစ်တယ်, အကယ်၍ ဖစ်တယ်ဆိုရင် အသစ်တစ်ခုဖန်တီးပေးလိုက်တယ် ဒါပေမဲ့ အသစ်တစ်ခုပါလာတာက Delay Time အနည်းငယ်နဲ့ အဆင်ပြေသလိုရှိလှတယ်ထင်ရပေမယ် တကယ်တန်းကျတော့ Threads နှစ်ခုတစ်ပြိုင်ထဲခေါ်လိုက်တဲ့ အချိန်မှာ နှစ်ခုလုံးက null ပဲဖစ်ပီး instance နှစ်ခုကိုပဲ ဖန်တီးပေးတာဖစ်တယ်။ဒါကြောင့် 
**Double-Checked Locking Singleton** ကိုပဲသုံးသင့်တယ်။

```java
import java.util.concurrent.locks.ReentrantLock;
import java.util.concurrent.locks.Condition;

public class Singleton {
    private static Singleton instance = null;
    private static final ReentrantLock lock = new ReentrantLock();
    private static final Condition creationDone = lock.newCondition();
    private static boolean isCreating = false;

    private Singleton() {
        // Constructor logic
    }

    public static Singleton getInstance() {
        lock.lock();
        try {
            if (instance != null) {
                return instance;
            }

            while (isCreating) {
                try {
                    creationDone.await(); // Wait for creation to complete
                } catch (InterruptedException e) {
                    Thread.currentThread().interrupt();
                }
            }

            if (instance == null) {
                isCreating = true;
                try {
                    // Simulate async delay like await sleep
                    Thread.sleep(1);
                    instance = new Singleton();
                } catch (InterruptedException e) {
                    Thread.currentThread().interrupt();
                } finally {
                    isCreating = false;
                    creationDone.signalAll(); // Notify other threads
                }
            }

            return instance;
        } finally {
            lock.unlock();
        }
    }
}

```
အပေါ်က ဥပမာမှာဆိုရင် အကယ်၍ instance ရှိပီးသားဆိုရင် တစ်ခါထဲ ပြန်ပေးလိုက်မယ်၊မရှိဘူးဆိုရင် တခြား Thread က create လုပ်တာကိုစောင့်နေမယ် isCreating ကို true ပေးပီးတော့ lock ချမယ် ပီးတာနဲ့တပြိုင်နက် instance ကို create လုပ်မယ် နောက်ဆုံး Unlock ချဖို့အတွက် isCreating ကို false ပြန်ပေးလိုက်မယ်