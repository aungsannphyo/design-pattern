# Open/Close Principle

Open for extension but closed for modification. အဲ့တာကဘာကိုပြောသလဲဆိုရင် ကိုယ်က Class တစ်ခုမှာ Function တွေကိုတော့အသစ်ရေးခွင့်ပေးမယ် ဒါပေမဲ့ ရှိပီးသား Function တွေကိုတော့ပြုပြင်ခွင့်မပေးဘူး။

#### **Bad**

```typescript
interface IAdapter {
	name : string
}

class AjaxAdapter implements IAdapter {
   name : string;
  constructor() {
    this.name = "ajaxAdapter";
  }
}

class NodeAdapter implements IAdapter {
  name : string;
  constructor() {
    this.name = "nodeAdapter";
  }
}

class HttpRequester {
  adapter : any;
  constructor(adapter : any) {
    this.adapter = adapter;
  }

  fetch(url : string) {
    if (this.adapter.name === "ajaxAdapter") {
      return makeAjaxCall(url).then(response => {
        // transform response and return
      });
    } else if (this.adapter.name === "nodeAdapter") {
      return makeHttpCall(url).then(response => {
        // transform response and return
      });
    }
  }
}

function makeAjaxCall(url : string) {
  // request and return promise
}

function makeHttpCall(url : string) {
  // request and return promise
}

```

#### Good

```typescript
interface IAdapter {
	name : string
}

class AjaxAdapter implements IAdapter {
  name : string
  constructor() {
    this.name = "ajaxAdapter";
  }

  request(url : string) {
    // request and return promise
  }
}

class NodeAdapter implements Adapter {
  name : string
  constructor() {
    this.name = "nodeAdapter";
  }

  request(url : string) {
    // request and return promise
  }
}

class HttpRequester {
  adapter : any
  constructor(adapter : any) {
    this.adapter = adapter;
  }

  fetch(url : string) {
    return this.adapter.request(url).then(response => {
      // transform response and return
    });
  }
}

```

