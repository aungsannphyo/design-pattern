## Liskov Substitution Principle (LSP)

Liskov Principle ဆိုတာ If S is a subtype of T, then objects of type T may be replaced with objects of type S အဲ့တာဘာကိုပြောချင်သလဲဆိုတာ အလွယ်ကူဆုံးမြင်သာအောင် ဥပမာပေးရရင် ကော်ဖီဆိုင်တစ်ဆိုင်ဖွင့်ထားတဲ့ ဦးဘနဲ့မောင်လှဆိုတဲ့သားအဖနှစ်ယောက်ရှိတယ်ဆိုပါဆို့ customer တစ်ယောက်လာပီး ကော်ဖီလာသောက်ပီပေါ့ ဦးဘ ကျွန်တော့်ကို ကော်ဖီတစ်ခွက်လောက်ဗျာဆို  ဦးဘကကော်ဖီဖျော်ပီး  လာချပေးလိမ့်မယ်နောက်တစ်ရက် ဦးဘမရှိတဲ့ချိန် ထပ်လာသောက်ပြန်ရော အဲ့ကျ မောင်လှရေ ကော်ဖီတစ်ခွက်လောက်ဆိုပီး customer ကမှာရင် ကျွန်တော်ကော်ဖီမဖျော်တက်ဘူးဗျ
အဖေမရှိသေးလို့ ရေပဲသောက်လို့ရလားဆိုပီး customer ကိုရေသွားပေးလို့မရဘူး။ Parent Class ကိုမှီခိုထားတဲ့ Child Class တွေက Parent Class တွေနေရာကိုလဲအစားထိုးနိုင်ရမယ်။ For Example , Parent Class ရဲ့ Function တစ်ခုက object တစ်ခုကို return ပြန်တယ်ဆိုရင် Child Class ရောက်မှ အဲ့ Function ကို override လုပ်ပီးတခြားတစ်ခုခုကို return ပြန်မယ်ဆိုရင် Liskov Principle ကိုချိုးဖောက်တယ်လို့ခေါ်ပါတယ်။

**Bad :**

```java
// Rectangle Class
public class Rectangle {
    protected int width;
    protected int height;

    public Rectangle() {
        this.width = 0;
        this.height = 0;
    }

    public void setColor(String color) {
    }

    public void render(int area) {
        System.out.println("Rendering rectangle with area: " + area);
    }

    public void setWidth(int width) {
        this.width = width;
    }

    public void setHeight(int height) {
        this.height = height;
    }

    public int getArea() {
        return width * height;
    }
}

// Square Class (LSP Violation with Rectangle)
public class Square extends Rectangle {

    @Override
    public void setWidth(int width) {
        this.width = width;
        this.height = width;
    }

    @Override
    public void setHeight(int height) {
        this.width = height;
        this.height = height;
    }
}

// Main Class to render Rectangles and Squares
import java.util.List;

public class Main {

    public static void renderLargeRectangles(List<Rectangle> rectangles) {
        for (Rectangle rectangle : rectangles) {
            rectangle.setWidth(4);
            rectangle.setHeight(5);
            int area = rectangle.getArea(); // This will give incorrect area for Square
            rectangle.render(area);
        }
    }

    public static void main(String[] args) {
        // Create a list of rectangles and squares
        List<Rectangle> rectangles = List.of(new Rectangle(), new Rectangle(), new Square());
        renderLargeRectangles(rectangles);
    }
}

```

**Good**

```java
abstract class Shape {
    public void setColor(String color) {
    }

    public abstract void render(int area);
    
    public abstract int getArea();
}

// Rectangle class
class Rectangle extends Shape {
    private int width;
    private int height;

    public Rectangle(int width, int height) {
        this.width = width;
        this.height = height;
    }

    @Override
    public int getArea() {
        return this.width * this.height;
    }

    @Override
    public void render(int area) {
        System.out.println("Rendering rectangle with area: " + area);
    }
}

// Square class
class Square extends Shape {
    private int length;

    public Square(int length) {
        this.length = length;
    }

    @Override
    public int getArea() {
        return this.length * this.length;
    }

    @Override
    public void render(int area) {
        System.out.println("Rendering square with area: " + area);
    }
}

import java.util.List;

public class Main {

    public static void renderLargeShapes(List<Shape> shapes) {
        for (Shape shape : shapes) {
            int area = shape.getArea();
            shape.render(area);  
        }
    }

    public static void main(String[] args) {
        List<Shape> shapes = List.of(new Rectangle(4, 5), new Rectangle(4, 5), new Square(5));
        renderLargeShapes(shapes);  
    }
}
```

