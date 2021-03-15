---
layout: post
title: Java_and_C++_polymorphism
---


1. 首先调用基类的构造函数，这个步骤被递归地重复，这样一来类层次的顶级父类会被最先构造，然后是它的派生类，以此类推，直到最底层的派生类。
2. 按声明顺序初始化成员。
3. 调用派生类构造器的方法体。

```Java
// polymorphism/Frog.java
// Cleanup and inheritance
// {java polymorphism.Frog}
package polymorphism;

class Characteristic {
    private String s;

    Characteristic(String s) {
        this.s = s;
        System.out.println("Creating Characteristic " + s);
    }

    protected void dispose() {
        System.out.println("disposing Characteristic " + s);
    }
}

class Description {
    private String s;

    Description(String s) {
        this.s = s;
        System.out.println("Creating Description " + s);
    }

    protected void dispose() {
        System.out.println("disposing Description " + s);
    }
}

class LivingCreature {
    private Characteristic p = new Characteristic("is alive");
    private Description t = new Description("Basic Living Creature");

    LivingCreature() {
        System.out.println("LivingCreature()");
    }

    protected void dispose() {
        System.out.println("LivingCreature dispose");
        t.dispose();
        p.dispose();
    }
}

class Animal extends LivingCreature {
    private Characteristic p = new Characteristic("has heart");
    private Description t = new Description("Animal not Vegetable");

    Animal() {
        System.out.println("Animal()");
    }

    @Override
    protected void dispose() {
        System.out.println("Animal dispose");
        t.dispose();
        p.dispose();
        super.dispose();
    }
}

class Amphibian extends Animal {
    private Characteristic p = new Characteristic("can live in water");
    private Description t = new Description("Both water and land");

    Amphibian() {
        System.out.println("Amphibian()");
    }

    @Override
    protected void dispose() {
        System.out.println("Amphibian dispose");
        t.dispose();
        p.dispose();
        super.dispose();
    }
}

public class Frog extends Amphibian {
    private Characteristic p = new Characteristic("Croaks");
    private Description t = new Description("Eats Bugs");

    public Frog() {
        System.out.println("Frog()");
    }

    @Override
    protected void dispose() {
        System.out.println("Frog dispose");
        t.dispose();
        p.dispose();
        super.dispose();
    }

    public static void main(String[] args) {
        Frog frog = new Frog();
        System.out.println("Bye!");
        frog.dispose();
    }
}
```


## 清理共享对象

```Java

class Shared {
  private int refcount = 0;
  private static long counter = 0;
  private final long id = counter++;
  Shared() {
    System.out.println("Creating " + this);
  }
  public void addRef() { refcount++; }
  protected void dispose() {
    if(--refcount == 0) //共享模式，最后消除
      System.out.println("Disposing " + this);
  }
  @Override
  public String toString() {
    return "Shared " + id;
  }
}

class Composing {
  private Shared shared;  //初始化为null
  private static long counter = 0;
  private final long id = counter++;
  Composing(Shared shared) {
    System.out.println("Creating " + this);
    System.out.println("Creating " + this.shared);

    this.shared = shared;
    this.shared.addRef();
  }
  protected void dispose() {
    System.out.println("disposing " + this);
    shared.dispose();
  }
  @Override
  public String toString() {
    return "Composing " + id;
  }
}

public class ReferenceCounting {
  public static void main(String[] args) {
    Shared shared = new Shared();
    Composing[] composing = {
      new Composing(shared),
      new Composing(shared),
      new Composing(shared),
      new Composing(shared),
      new Composing(shared)
    };
    for(Composing c : composing)
      c.dispose();
  }
}
/* Output:
Creating Shared 0
Creating Composing 0
Creating Composing 1
Creating Composing 2
Creating Composing 3
Creating Composing 4
disposing Composing 0
disposing Composing 1
disposing Composing 2
disposing Composing 3
disposing Composing 4
Disposing Shared 0
*/

```

## 构造器内部多态方法的行为
```Java
// polymorphism/PolyConstructors.java
// Constructors and polymorphism
// don't produce what you might expect
class Glyph {
    void draw() {
        System.out.println("Glyph.draw()");
    }

    Glyph() {
        System.out.println("Glyph() before draw()");
        draw();
        System.out.println("Glyph() after draw()");
    }
}

class RoundGlyph extends Glyph {
    private int radius = 1;

    RoundGlyph(int r) {
        radius = r;
        System.out.println("RoundGlyph.RoundGlyph(), radius = " + radius);
    }

    @Override
    void draw() {
        System.out.println("RoundGlyph.draw(), radius = " + radius);
    }
}

public class PolyConstructors {
    public static void main(String[] args) {
        new RoundGlyph(5);
    }
}

// Glyph() before draw()
// RoundGlyph.draw(), radius = 0
// Glyph() after draw()
// RoundGlyph.RoundGlyph(), radius = 5


```
前面一小节描述的初始化顺序并不十分完整，而这正好是解决谜团的关键所在。初始化的实际过程是：

1. 在所有事发生前，分配给对象的存储空间会被初始化为二进制0。
2. 如前所述调用基类构造器。此时调用重写后的draw()方法(是的，在调用RoundGraph构造器之前调用)，由步骤1可知，radius的值为0。
3. 按声明顺序初始化成员。
4. 最终调用派生类的构造器。

在基类的构造器中能安全调用的只有基类的 final 方法（这也适用于可被看作是 final 的 private 方法）。这些方法不能被重写，因此不会产生意想不到的结果。你可能无法永远遵循这条规范，但应该朝着它努力。

## 首先选择组合

```Java
// polymorphism/Transmogrify.java
// Dynamically changing the behavior of an object
// via composition (the "State" design pattern)
class Actor {
    public void act() {}
}

class HappyActor extends Actor {
    @Override
    public void act() {
        System.out.println("HappyActor");
    }
}

class SadActor extends Actor {
    @Override
    public void act() {
        System.out.println("SadActor");
    }
}

class Stage {
    private Actor actor = new HappyActor();

    public void change() {
        actor = new SadActor();
    }

    public void performPlay() {
        actor.act();
    }
}

public class Transmogrify {
    public static void main(String[] args) {
        Stage stage = new Stage();
        stage.performPlay();
        stage.change();
        stage.performPlay();
    }
}
// HappyActor
// SadActor

```


有一条通用准则：使用继承表达行为的差异，使用属性表达状态的变化。在上个例子中，两者都用到了。通过继承得到的两个不同类在 act() 方法中表达了不同的行为，Stage 通过组合使自己的状态发生变化。这里状态的改变产生了行为的改变。


