## Liskov Substitution Principle (LSP)

Liskov Principle ဆိုတာ If S is a subtype of T, then objects of type T may be replaced with objects of type S အဲ့တာဘာကိုပြောချင်သလဲဆိုတာ အလွယ်ကူဆုံးမြင်သာအောင် ဥပမာပေးရရင် ကော်ဖီဆိုင်တစ်ဆိုင်ဖွင့်ထားတဲ့ ဦးဘနဲ့မောင်လှဆိုတဲ့သားအဖနှစ်ယောက်ရှိတယ်ဆိုပါဆို့ customer တစ်ယောက်လာပီး ကော်ဖီလာသောက်ပီပေါ့ ဦးဘ ကျွန်တော့်ကို ကော်ဖီတစ်ခွက်လောက်ဗျာဆို  ဦးဘကကော်ဖီဖျော်ပီး  လာချပေးလိမ့်မယ်နောက်တစ်ရက် ဦးဘမရှိတဲ့ချိန် ထပ်လာသောက်ပြန်ရော အဲ့ကျ မောင်လှရေ ကော်ဖီတစ်ခွက်လောက်ဆိုပီး customer ကမှာရင် ကျွန်တော်ကော်ဖီမဖျော်တက်ဘူးဗျ ရေပဲသောက်လို့ရလားဆိုပီး customer ကိုရေသွားပေးလို့မရဘူး။ Parent Class ကိုမှီခိုထားတဲ့ Child Class တွေက Parent Class တွေနေရာကိုလဲအစားထိုးနိုင်ရမယ်။ For Example , Parent Class ရဲ့ Function တစ်ခုက object တစ်ခုကို return ပြန်တယ်ဆိုရင် Child Class ရောက်မှ အဲ့ Function ကို override လုပ်ပီးတခြားတစ်ခုခုကို return ပြန်မယ်ဆိုရင် Liskov Principle ကိုချိုးဖောက်တယ်လို့ခေါ်ပါတယ်။

**Bad :**

```javascript
class Rectangle {
  constructor() {
    this.width = 0;
    this.height = 0;
  }

  setColor(color) {
    // ...
  }

  render(area) {
    // ...
  }

  setWidth(width) {
    this.width = width;
  }

  setHeight(height) {
    this.height = height;
  }

  getArea() {
    return this.width * this.height;
  }
}

class Square extends Rectangle {
  setWidth(width) {
    this.width = width;
    this.height = width;
  }

  setHeight(height) {
    this.width = height;
    this.height = height;
  }
}

function renderLargeRectangles(rectangles) {
  rectangles.forEach(rectangle => {
    rectangle.setWidth(4);
    rectangle.setHeight(5);
    const area = rectangle.getArea(); // BAD: Returns 25 for Square. Should be 20.
    rectangle.render(area);
  });
}

const rectangles = [new Rectangle(), new Rectangle(), new Square()];
renderLargeRectangles(rectangles);

```

**Good**

```javascript
class Shape {
  setColor(color) {
    // ...
  }

  render(area) {
    // ...
  }
}

class Rectangle extends Shape {
  constructor(width, height) {
    super();
    this.width = width;
    this.height = height;
  }

  getArea() {
    return this.width * this.height;
  }
}

class Square extends Shape {
  constructor(length) {
    super();
    this.length = length;
  }

  getArea() {
    return this.length * this.length;
  }
}

function renderLargeShapes(shapes) {
  shapes.forEach(shape => {
    const area = shape.getArea();
    shape.render(area);
  });
}

const shapes = [new Rectangle(4, 5), new Rectangle(4, 5), new Square(5)];
renderLargeShapes(shapes);

```

