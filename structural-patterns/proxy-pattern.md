# Proxy Pattern

Proxy Pattern ဒီကောင်က structural patterns အောက်ကကောင်ပါပဲ။ proxy ဆိုတဲ့နာမည်အတိုင်းပဲ object တွေကို
တိုက်ရိုက်အသုံးမပြုပဲ access တွေကို ထိန်းချုပ်ရန်အတွက် အစားထိုး proxy object တစ်ခုကိုအသုံးပြုတာဖစ်ပါတယ်။
ဒီ proxy က client and original object ကြား တစ်ခုတည်းကဲ့သို့ လုပ်ဆောင်ပေးတာဖစ်ပါသည်။

ဥပမာအနေနဲ့ မြင်သာအောင်ပြောပြရရင် တစ်ဦးတစ်ယောက်ထဲဖစ်ဖစ်သက်မှတ်ထားတဲ့လူဖစ်ဖစ်ပဲ ၀င်ရောက်ခွင့်ရှိတဲ့ VIP Room ရှိတယ်ဆိုပါစို့။ အဲ့အခန်းထဲကို ၀င်လိုတဲ့ လူတိုင်းအတွက် သတ်မှတ်ထားတဲ့လူဟုတ်မဟုတ် စစ်ဆေးပေးတဲ့ security gurad ကစစ်ဆေးပီမှ ၀င်ခွင့်ပေးလို့ရတယ်။ ဒီလိုအခြေအနေမှာဆိုရင် security gurad က proxy object ဖစ်ပီး room ကိုယ်တိုင်က real object ဖစ်ပါတယ်။ ကဲဒါဆိုအောင်မှာ code example ကြည့်လိုက်ရအောင်။

```java
public interface RoomAccess {
    void enterRoom();
}
```

```java
public class VIPRoom implements RoomAccess {
    @Override
    public void enterRoom() {
        System.out.println("Welcome to the VIP Room!");
    }
}
```

```java
public class SecurityGuard implements RoomAccess {
    private VIPRoom vipRoom;
    private String personName;
    private boolean hasPermission;

    public SecurityGuard(String personName, boolean hasPermission) {
        this.personName = personName;
        this.hasPermission = hasPermission;
    }

    @Override
    public void enterRoom() {
        if (hasPermission) {
            if (vipRoom == null) {
                vipRoom = new VIPRoom(); // Lazy initialization
            }
            System.out.println("Security Check Passed for " + personName);
            vipRoom.enterRoom();
        } else {
            System.out.println("Access Denied for " + personName + ". You are not allowed to enter the VIP Room.");
        }
    }
}
```

```java
public class Main {
    public static void main(String[] args) {
        RoomAccess person1 = new SecurityGuard("Batman", true);
        person1.enterRoom();

        RoomAccess person2 = new SecurityGuard("Superman", false);
        person2.enterRoom();
    }
}
```

```csharp
Security Check Passed for Batman
Welcome to the VIP Room!
Access Denied for Superman. You are not allowed to enter the VIP Room.
```

#### ဘယ်လိုအခြေအနေတွေမှာသုံးသင့်သလဲ

Proxy Design Pattern ကို သုံးသင့်တဲ့နေရာတွေက အဓိကအားဖြင့် **original object ကို တန်းတန်း access** မလုပ်ချင်တဲ့အခါ ဖြစ်ပါတယ်။ ဒါကြောင့် Proxy Pattern သုံးတာဟာ performance တိုးချင်တယ်၊ security ထပ်ပေးချင်တယ်၊ remote object ကိုတစ်ဆင့်ခေါ်ချင်တယ်၊ real object ကိုထပ်ပီး ပြင်ဆင်မှု့မလိုချင်ဘူး ဆိုတဲ့အခါတွေမှာ အတော်အသုံးဝင်ပါတယ်။

လက်ရှိ real world မှာတော့များသောအားဖြင့် 
Protection Proxy - Access control တွေလုပ်ချင်တဲ့အခါမှာ ဥပမာ user permission check 
Smart Proxy - ရှိပီးသား object ကိုပြင်ဆင်မှူ့မရှိပဲ logging, caching, or security လုပ်ချင်တဲ့ချိန်
Lazy Loading - Image Viewer app မှာ ပုံကြီးတွေ lazy load လုပ်ချင်တဲ့အခါတွေမှာများသောအားဖြင့်သုံးကြပါတယ်။