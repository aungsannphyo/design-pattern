# Open/Close Principle

Open for extension but closed for modification. အဲ့တာကဘာကိုပြောသလဲဆိုရင် ကိုယ်က Class တစ်ခုမှာ Function တွေကိုတော့အသစ်ရေးခွင့်ပေးမယ် ဒါပေမဲ့ ရှိပီးသား Function တွေကိုတော့ပြုပြင်ခွင့်မပေးဘူး။

#### **Bad**

```java
// IAdapter interface
public interface IAdapter {
    String getName();
}

// AjaxAdapter class implements IAdapter
public class AjaxAdapter implements IAdapter {
    private String name;

    public AjaxAdapter() {
        this.name = "ajaxAdapter";
    }

    @Override
    public String getName() {
        return name;
    }
}

// NodeAdapter class implements IAdapter
public class NodeAdapter implements IAdapter {
    private String name;

    public NodeAdapter() {
        this.name = "nodeAdapter";
    }

    @Override
    public String getName() {
        return name;
    }
}

// HttpRequester class
public class HttpRequester {
    private IAdapter adapter;

    public HttpRequester(IAdapter adapter) {
        this.adapter = adapter;
    }

    public void fetch(String url) {
        if (adapter.getName().equals("ajaxAdapter")) {
            makeAjaxCall(url).thenAccept(response -> {
                // transform response and return
            });
        } else if (adapter.getName().equals("nodeAdapter")) {
            makeHttpCall(url).thenAccept(response -> {
                // transform response and return
            });
        }
    }

    public CompletableFuture<String> makeAjaxCall(String url) {
        return CompletableFuture.supplyAsync(() -> "Ajax response");
    }

    public CompletableFuture<String> makeHttpCall(String url) {
        return CompletableFuture.supplyAsync(() -> "Node response");
    }
}

```

#### Good

```java
import java.util.concurrent.CompletableFuture;

// IAdapter interface
public interface IAdapter {
    String getName();
    CompletableFuture<String> request(String url);
}

// AjaxAdapter class implements IAdapter
public class AjaxAdapter implements IAdapter {
    private String name;

    public AjaxAdapter() {
        this.name = "ajaxAdapter";
    }

    @Override
    public String getName() {
        return name;
    }

    @Override
    public CompletableFuture<String> request(String url) {
        return CompletableFuture.supplyAsync(() -> {
            return "Ajax response from " + url;
        });
    }
}

// NodeAdapter class implements IAdapter
public class NodeAdapter implements IAdapter {
    private String name;

    public NodeAdapter() {
        this.name = "nodeAdapter";
    }

    @Override
    public String getName() {
        return name;
    }

    @Override
    public CompletableFuture<String> request(String url) {
        return CompletableFuture.supplyAsync(() -> {
            return "Node response from " + url;
        });
    }
}

// HttpRequester class
public class HttpRequester {
    private IAdapter adapter;

    public HttpRequester(IAdapter adapter) {
        this.adapter = adapter;
    }

    public CompletableFuture<String> fetch(String url) {
        return adapter.request(url).thenApply(response -> {
            // Transform response and return
            return "Transformed: " + response;
        });
    }
}
```

