---
title: "继承 | 类结构"
weight: 5
---

# 继承

在Java中，**继承（Inheritance）** 是面向对象编程的核心概念之一，它允许基于现有类构建新类，实现代码重用和层次化设计。以下是关键概念详解：

---

### **1. 核心概念**

| **术语**               | **定义**                                             | **示例**                                             |
| ---------------------- | ---------------------------------------------------- | ---------------------------------------------------- |
| **类（Class）**        | 对象的模板，定义属性和方法。<br>（如：`Animal` 类）  | `class Animal { ... }`                               |
| **超类（Superclass）** | 被继承的类（父类/基类）。<br>子类继承其属性和方法。  | `class Dog extends Animal { ... }` → `Animal` 是超类 |
| **子类（Subclass）**   | 继承超类的类（派生类）。<br>可扩展或修改超类的功能。 | `Dog` 是 `Animal` 的子类                             |

---

### **2. 继承的基本思想**

- **代码复用**  
  子类自动继承超类的字段和方法，无需重复编写相同代码。

  ```java
  class Animal {
      void eat() { System.out.println("Eating..."); }
  }
  
  class Dog extends Animal {}  // Dog 自动获得 eat() 方法
  ```

- **扩展功能**  
  子类可添加新字段/方法，或**重写（Override）** 超类方法。

  ```java
  class Dog extends Animal {
      void bark() { System.out.println("Barking!"); }  // 新增方法
      @Override
      void eat() { System.out.println("Dog eats bones"); }  // 重写方法
  }
  ```

- **多态支持**  
  超类引用可指向子类对象，实现运行时行为动态绑定。

  ```java
  Animal myPet = new Dog();  // 多态
  myPet.eat();  // 输出 "Dog eats bones"（调用子类重写的方法）
  ```

---

### **3. 继承的语法**

- 使用 `extends` 关键字建立继承关系：

  ```java
  class Subclass extends Superclass {
      // 子类特有属性和方法
  }
  ```

- **`super` 关键字**  
  子类中访问超类的成员或构造器：

  ```java
  class Dog extends Animal {
      Dog() {
          super();  // 调用超类构造器（必须位于子类构造器第一行）
      }
      void callSuperEat() {
          super.eat();  // 调用超类的 eat() 方法（避免重写覆盖）
      }
  }
  ```

  > [!NOTE]
  >
  > # 继承中子类字段与父类字段的关系、区别及注意事项
  >
  > 在 Java 继承体系中，子类字段和父类字段的关系需要仔细理解，以避免常见的错误和混淆。
  >
  > ## 核心关系
  >
  > 1. **继承关系**：
  >
  >    - 子类**继承**父类的所有非私有字段（`public`、`protected` 和包级私有）
  >    - 父类的私有字段（`private`）子类**无法直接访问**，但可通过父类提供的公共方法访问
  >
  > 2. **内存分配**：
  >
  >    - 创建子类对象时，内存中会同时包含：
  >      - 父类字段（包括私有字段）
  >      - 子类新增字段
  >
  >    ```mermaid
  >    classDiagram
  >      direction LR
  >      class Parent {
  >        - privateField
  >        + publicField
  >        # protectedField
  >      }
  >    
  >      class Child {
  >        + childField
  >      }
  >    
  >      Parent <|-- Child
  >    ```
  >
  > ## 字段访问规则
  >
  > ### 1. 访问父类字段
  >
  > ```java
  > class Parent {
  >     protected String name = "Parent";
  >     private int id = 100;
  > 
  >     public int getId() {
  >         return id;
  >     }
  > }
  > 
  > class Child extends Parent {
  >     private String name = "Child"; // 隐藏父类name
  > 
  >     public void print() {
  >         // 访问当前类的name（子类字段）
  >         System.out.println("Child name: " + name); 
  > 
  >         // 访问父类name（使用super）
  >         System.out.println("Parent name: " + super.name);
  > 
  >         // 访问父类私有字段（通过公共方法）
  >         System.out.println("Parent ID: " + getId());
  >     }
  > }
  > ```
  >
  > ### 2. 字段隐藏（Field Hiding）
  >
  > - 当子类声明与父类**同名字段**时：
  >   - 子类字段**隐藏**父类字段（不是覆盖！）
  >   - 在子类内部：
  >     - 直接访问字段名 → 访问子类字段
  >     - 使用 `super.字段名` → 访问父类字段
  >   - 外部代码：
  >     - 通过子类引用访问 → 访问子类字段
  >     - 通过父类引用访问 → 访问父类字段
  >
  > ```java
  > public static void main(String[] args) {
  >     Child child = new Child();
  >     child.print();
  > 
  >     // 通过子类引用访问
  >     System.out.println(child.name); // 输出 "Child"
  > 
  >     // 通过父类引用访问
  >     Parent parent = child;
  >     System.out.println(parent.name); // 输出 "Parent"
  > }
  > ```
  >
  > ## 关键区别
  >
  > | 特性           | 父类字段                 | 子类字段           |
  > | -------------- | ------------------------ | ------------------ |
  > | **访问范围**   | 子类可继承访问（非私有） | 仅限子类及后代     |
  > | **内存位置**   | 对象内存的前半部分       | 对象内存的后半部分 |
  > | **初始化顺序** | 父类构造器先初始化       | 子类构造器后初始化 |
  > | **多态行为**   | 无多态（静态绑定）       | 无多态             |
  > | **可见性**     | 可能被隐藏               | 可能隐藏父类字段   |
  >
  > ## 重要注意事项
  >
  > ### 1. 初始化顺序
  >
  > 字段初始化顺序：
  >
  > 1. 父类静态字段和静态块
  > 2. 子类静态字段和静态块
  > 3. 父类实例字段初始化
  > 4. 父类构造器
  > 5. 子类实例字段初始化
  > 6. 子类构造器
  >
  > ```java
  > class Parent {
  >     int value = init("Parent field");
  > 
  >     Parent() {
  >         System.out.println("Parent constructor");
  >     }
  > 
  >     int init(String msg) {
  >         System.out.println(msg);
  >         return 1;
  >     }
  > }
  > 
  > class Child extends Parent {
  >     int childValue = init("Child field");
  > 
  >     Child() {
  >         System.out.println("Child constructor");
  >     }
  > }
  > 
  > // 创建Child对象输出：
  > // Parent field
  > // Parent constructor
  > // Child field
  > // Child constructor
  > ```
  >
  > ### 2. 构造器注意事项
  >
  > - 在子类构造器中：
  >
  >   - 父类字段必须通过 `super(...)` 初始化
  >   - 子类字段在 `super(...)` 调用后初始化
  >
  > - 错误示例：
  >
  >   ```java
  >   class Child extends Parent {
  >       int value;
  >   
  >       Child(int value) {
  >           this.value = value; // 允许，但应在super后
  >           super(value * 2);   // 错误！super必须第一句
  >       }
  >   }
  >   ```
  >
  > ### 3. 字段隐藏的风险
  >
  > - 字段隐藏易导致混淆，应尽量避免
  >
  > - 替代方案：
  >
  >   - 使用不同的字段名
  >   - 使用访问器方法（getter/setter）
  >
  >   ```java
  >   class BetterChild extends Parent {
  >       private String childName; // 使用不同名称
  >   
  >       @Override
  >       public String getName() {
  >           return childName;
  >       }
  >   }
  >   ```
  >
  > ### 4. 序列化考虑
  >
  > - 序列化子类对象时：
  >   - 父类字段也会被序列化
  >   - 如果父类未实现 Serializable：
  >     - 父类必须有无参构造器
  >     - 父类字段不会被序列化，反序列化时调用无参构造器初始化
  >
  > ## 最佳实践
  >
  > 1. **避免字段隐藏**：
  >
  >    - 使用不同的字段名称
  >    - 优先使用方法覆盖而非字段隐藏
  >
  > 2. **封装原则**：
  >
  >    - 将字段声明为 `private`
  >    - 提供受保护/公共的访问方法
  >
  >    ```java
  >    class Parent {
  >        private String name;
  >    
  >        protected void setName(String name) {
  >            this.name = name;
  >        }
  >    
  >        protected String getName() {
  >            return name;
  >        }
  >    }
  >    ```
  >
  > 3. **使用构造器链**：
  >
  >    ```java
  >    class Parent {
  >        protected final String id;
  >    
  >        Parent(String id) {
  >            this.id = id;
  >        }
  >    }
  >    
  >    class Child extends Parent {
  >        private final String type;
  >    
  >        Child(String id, String type) {
  >            super(id); // 初始化父类字段
  >            this.type = type; // 初始化子类字段
  >        }
  >    }
  >    ```
  >
  > 4. **final字段的特殊性**：
  >
  >    - 父类final字段子类不能修改
  >    - 子类可以声明自己的同名final字段（但不推荐）
  >
  > ```java
  > class FinalExample {
  >     public static void main(String[] args) {
  >         Parent p = new Child();
  >         System.out.println(p.name); // 输出 "Parent"（字段静态绑定）
  >         p.printName();             // 输出 "Child" （方法动态绑定）
  >     }
  > }
  > 
  > class Parent {
  >     String name = "Parent";
  > 
  >     void printName() {
  >         System.out.println(name);
  >     }
  > }
  > 
  > class Child extends Parent {
  >     String name = "Child"; // 隐藏父类字段
  > 
  >     @Override
  >     void printName() {
  >         System.out.println(name); // 使用子类字段
  >     }
  > }
  > ```
  >
  > ## 总结
  >
  > 1. 子类继承父类非私有字段，可添加新字段
  > 2. 同名字段导致隐藏而非覆盖 - 使用 `super` 访问父类版本
  > 3. 字段访问是静态绑定（编译时决定），方法调用是动态绑定（运行时决定）
  > 4. 初始化顺序：父类字段 → 父类构造器 → 子类字段 → 子类构造器
  > 5. 最佳实践：避免字段隐藏，优先使用方法覆盖，遵循封装原则
  > 6. 子类可以继承父类的字段，但受限于访问权限。
  > 7. 子类定义同名字段会导致字段隐藏，访问时可能会出现混淆，建议避免这种情况。
  > 8. 在继承中，最好避免直接操作字段，应该通过方法（如 getter 和 setter）来间接访问。
  > 9. 使用封装和合适的访问权限控制来保护字段的安全性。
  >
  > 理解这些概念可以帮助开发者避免常见的继承陷阱，并设计出更健壮的面向对象系统。

  

---

### **4. 继承规则与特性**

| **规则**                 | **说明**                                                     |
| ------------------------ | ------------------------------------------------------------ |
| **单继承**               | Java不支持多继承（一个子类只能有一个直接超类）。             |
| **构造器链**             | 创建子类对象时，先调用超类构造器（默认调用 `super()`），再执行子类构造器。 |
| **访问权限**             | 子类可访问超类的 `public`/`protected` 成员，<br>但不能直接访问 `private` 成员。 |
| **方法重写（Override）** | 子类重写超类方法时：<br>① 方法名和参数列表必须相同；<br>② 访问权限不能更严格；<br>③ 返回类型需兼容（协变类型）。 |
| **`final` 类/方法**      | `final` 类禁止被继承；`final` 方法禁止被重写。               |

---

### **5. 继承 vs 组合**

| **继承（is-a 关系）** | **组合（has-a 关系）** |
| --------------------- | ---------------------- |
| “狗**是**一种动物”    | “汽车**有**发动机”     |
| 强调类之间的层次关系  | 强调对象之间的包含关系 |
| 通过 `extends` 实现   | 通过将类作为字段实现   |
| 可能造成过度耦合      | 更灵活，降低耦合度     |

---

### **6. 继承的最佳实践**

1. **遵循 Liskov 替换原则**  
   子类必须能替代超类（不破坏超类行为）。

2. **避免深度继承链**  
   超过 3 层的继承易导致代码脆弱（优先使用组合）。

3. **使用抽象类**  
   若超类不应被实例化，声明为 `abstract`：

   ```java
   abstract class Animal {
       abstract void makeSound();  // 抽象方法（子类必须实现）
   }
   ```

---

### **7. 示例：完整继承链**

```java
// 超类
class Animal {
    void eat() { System.out.println("Animal eats"); }
}

// 子类
class Dog extends Animal {
    @Override
    void eat() { System.out.println("Dog eats bones"); }
    void bark() { System.out.println("Woof!"); }
}

// 测试
public class Main {
    public static void main(String[] args) {
        Animal obj = new Dog();  // 多态
        obj.eat();               // 输出 "Dog eats bones"
        // obj.bark();           // 错误！Animal 类无 bark() 方法
    }
}
```

> **关键输出**：  
> `Dog eats bones`（子类重写方法生效，体现多态性）

---

### **总结**

- **继承目的**：代码复用 + 扩展功能 + 多态支持。
- **核心关系**：子类 **is-a** 超类（如 `Dog` 是一种 `Animal`）。
- **慎用场景**：避免为“复用代码”而强行继承（优先考虑组合）。
- **最终目标**：构建层次清晰、易于维护的类结构。

> [!IMPORTANT]
>
> 在 Java 继承体系中，子类构造器和 `super` 关键字的使用是构建对象初始化流程的核心机制。以下是详细说明：
>
> ---
>
> ### **一、构造器调用规则**
>
> 1. **隐式调用**  
>    子类构造器**必须调用父类构造器**（直接或间接）
>    - 如果子类构造器**没有显式调用** `super(...)` 或 `this(...)`
>    - 编译器会**自动插入** `super()`（调用父类无参构造器）
>
> 2. **显式调用**  
>    使用 `super(...)` **显式调用**特定父类构造器时：
>    - 必须是构造器中的**第一条语句**
>    - 不能与 `this(...)` 同时使用（二者互斥）
>
> ---
>
> ### **二、`super` 关键字的两种用途**
>
> #### 1. **调用父类构造器**（构造器内部）
>
> ```java
> class Animal {
>     private String type;
> 
>     public Animal(String type) {
>         this.type = type;
>     }
> }
> 
> class Dog extends Animal {
>     private String breed;
> 
>     public Dog(String type, String breed) {
>         super(type);  // 必须第一句！调用父类有参构造器
>         this.breed = breed;
>     }
> }
> ```
>
> #### 2. **访问父类成员**（方法内部）
>
> ```java
> class Bird {
>     void move() {
>         System.out.println("Flying");
>     }
> }
> 
> class Penguin extends Bird {
>     @Override
>     void move() {
>         super.move();  // 先调用父类方法
>         System.out.println("Swimming");
>     }
> }
> ```
>
> ---
>
> ### **三、关键使用场景**
>
> #### 场景 1：父类**有无参构造器**
>
> ```java
> class Parent {
>     Parent() {  // 无参构造器
>         System.out.println("Parent无参构造");
>     }
> }
> 
> class Child extends Parent {
>     Child() {
>         // 编译器自动添加 super();
>         System.out.println("Child构造");
>     }
> }
> ```
>
> ▶ 输出：  
>
> ```
> Parent无参构造
> Child构造
> ```
>
> #### 场景 2：父类**没有无参构造器**（必须显式调用）
>
> ```java
> class Parent {
>     Parent(String name) {  // 只有有参构造器
>         System.out.println("Parent构造: " + name);
>     }
> }
> 
> class Child extends Parent {
>     Child() {
>         super("强制传递");  // 必须显式调用
>         System.out.println("Child构造");
>     }
> }
> ```
>
> ▶ 输出：  
>
> ```
> Parent构造: 强制传递
> Child构造
> ```
>
> #### 场景 3：**多级继承**的构造链
>
> ```java
> class Grandparent {
>     Grandparent() {
>         System.out.println("Grandparent构造");
>     }
> }
> 
> class Parent extends Grandparent {
>     Parent() {
>         // 自动调用 super(); → Grandparent()
>         System.out.println("Parent构造");
>     }
> }
> 
> class Child extends Parent {
>     Child() {
>         // 自动调用 super(); → Parent()
>         System.out.println("Child构造");
>     }
> }
> ```
>
> ▶ 输出：  
>
> ```
> Grandparent构造
> Parent构造
> Child构造
> ```
>
> ---
>
> ### **四、重要规则总结**
>
> 1. **构造器链原则**  
>    子类构造器必须直接/间接调用父类构造器，直至 `Object` 类
>
> 2. **第一语句规则**  
>    `super(...)` 或 `this(...)` 必须是构造器中的第一条语句
>
> 3. **默认调用限制**  
>    父类若无无参构造器，子类**必须显式调用**父类有参构造器
>
> 4. **访问权限约束**  
>    `super(...)` 调用的父类构造器必须对子类可见（非 `private`）
>
> ---
>
> ### **五、常见错误示例**
>
> #### 错误 1：未调用父类构造器
>
> ```java
> class Parent {
>     Parent(int x) {}  // 无无参构造器
> }
> 
> class Child extends Parent {
>     Child() {}  // 错误！没有显式调用 super(int)
> }
> // 编译报错：There is no default constructor available in 'Parent'
> ```
>
> #### 错误 2：`super` 不是第一条语句
>
> ```java
> class Child extends Parent {
>     Child() {
>         int x = 10;
>         super(x);  // 错误！super 必须是第一句
>     }
> }
> // 编译报错：Call to super must be first statement in constructor
> ```
>
> #### 错误 3：同时使用 `super` 和 `this`
>
> ```java
> class Child extends Parent {
>     Child() {
>         this(10);  // 尝试调用本类其他构造器
>         super();   // 错误！不能与 super 共存
>     }
> 
>     Child(int x) {}
> }
> // 编译报错：Call to 'super()' must be first statement
> ```
>
> ---
>
> ### **六、最佳实践**
>
> 1. **显式调用原则**  
>    即使父类有无参构造器，也建议显式写出 `super()` 增强可读性
>
> 2. **构造器参数设计**  
>    父类构造器参数应包含子类公共属性
>
> 3. **避免构造器代码重复**  
>    使用 `this(...)` 重载构造器，最后集中调用 `super(...)`
>
> ```java
> class Vehicle {
>     private int wheels;
> 
>     public Vehicle(int wheels) {
>         this.wheels = wheels;
>     }
> }
> 
> class Car extends Vehicle {
>     private String model;
> 
>     public Car(String model) {
>         this(4, model);  // 调用本类其他构造器
>     }
> 
>     public Car(int wheels, String model) {
>         super(wheels);  // 最终调用父类构造器
>         this.model = model;
>     }
> }
> ```
>
> 通过正确使用构造器和 `super` 关键字，可以确保对象初始化过程符合面向对象的设计原则，避免出现初始化错误或逻辑混乱。



## Object：所有类的超类以及Objects工具类

在Java中，`Object`类和`Objects`类是两个相关但完全不同的概念：

### 1. **`Object`类**  

- **位置**：`java.lang.Object`

- **性质**：Java中所有类的**根父类**（超类）。所有类默认继承`Object`（除非显式继承其他类）。

- **核心方法**：

  | 方法                          | 作用               | 说明                                         |
  | ----------------------------- | ------------------ | -------------------------------------------- |
  | `toString()`                  | 返回对象字符串表示 | 默认返回`类名@哈希码`，建议重写              |
  | `equals(Object obj)`          | 比较对象内容       | 默认比较引用地址（`==`），需重写实现逻辑相等 |
  | `hashCode()`                  | 返回对象哈希码     | 重写`equals()`时必须重写此方法               |
  | `getClass()`                  | 返回对象的运行时类 | `Class<?>`对象，反射基础                     |
  | `clone()`                     | 创建对象的副本     | 需实现`Cloneable`接口                        |
  | `notify()/notifyAll()/wait()` | 线程同步方法       | 用于线程间通信                               |
  | `finalize()`                  | 垃圾回收前的清理   | **已废弃（Java 9+）**                        |

**示例**：

```java
public class Person {
    private String name;
    private int age;

    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (o == null || getClass() != o.getClass()) return false;
        Person person = (Person) o;
        return age == person.age && Objects.equals(name, person.name);
    }

    @Override
    public int hashCode() {
        return Objects.hash(name, age); // 使用Objects工具类
    }
}
```

---

### 2. **`Objects`工具类**  

- **位置**：`java.util.Objects`

- **性质**：Java 7+引入的**工具类**，提供静态方法操作对象（**空指针安全**）。

- **核心方法**：

  | 方法                         | 作用           | 示例                                                  |
  | ---------------------------- | -------------- | ----------------------------------------------------- |
  | `equals(Object a, Object b)` | 安全比较对象   | `Objects.equals(null, obj)` 不抛NPE                   |
  | `hashCode(Object o)`         | 安全哈希码     | `Objects.hashCode(null)` 返回0                        |
  | `hash(Object... values)`     | 生成组合哈希码 | `Objects.hash(name, age)`                             |
  | `toString(Object o)`         | 安全转字符串   | `Objects.toString(null)` → `"null"`                   |
  | `requireNonNull(T obj)`      | 非空校验       | 验证参数：`this.name = Objects.requireNonNull(name);` |
  | `isNull(Object o)`           | 判空           | `Objects.isNull(obj)`                                 |
  | `nonNull(Object o)`          | 判非空         | `Objects.nonNull(obj)`                                |

**示例**：

```java
import java.util.Objects;

public class Validation {
    private String data;

    public Validation(String data) {
        // 若data为null则抛NullPointerException（含自定义消息）
        this.data = Objects.requireNonNull(data, "data不能为null");
    }

    @Override
    public int hashCode() {
        return Objects.hash(data); // 自动处理null
    }
}
```

> [!IMPORTANT]
>
> 在 Java 中，`equals` 和 `hashCode` 是 `Object` 类中非常重要的方法，通常我们会根据实际需求对它们进行 **重写**。重写这两个方法时，有一些 **规则** 和 **要求**，确保它们的正确实现，特别是与集合框架（如 `HashMap`、`HashSet`）兼容时。
>
> - 在 Java 中，重写 `equals()` 和 `hashCode()` 方法必须遵循严格的约定，否则会导致集合类（如 `HashMap`、`HashSet`）行为异常。以下是核心要求和最佳实践：
>
>   ---
>
>   ### **重写 `equals()` 的要求**
>
>   1. **自反性 (Reflexive)**  
>      `x.equals(x)` 必须返回 `true`
>
>   2. **对称性 (Symmetric)**  
>      若 `x.equals(y)` 为 `true`，则 `y.equals(x)` 必须为 `true`
>
>   3. **传递性 (Transitive)**  
>      若 `x.equals(y)` 为 `true` 且 `y.equals(z)` 为 `true`，则 `x.equals(z)` 必须为 `true`
>
>   4. **一致性 (Consistent)**  
>      多次调用 `x.equals(y)` 应始终返回相同结果（前提：对象未修改）
>
>   5. **非空性 (Non-nullity)**  
>      `x.equals(null)` 必须返回 `false`
>
>   ---
>
>   ### **重写 `hashCode()` 的要求**
>
>   1. **一致性**  
>      同一对象多次调用 `hashCode()` 必须返回相同值（前提：对象未修改）
>
>   2. **等价对象必须有相同哈希码**  
>      若 `x.equals(y)` 为 `true`，则 `x.hashCode() == y.hashCode()`
>
>   3. **不等对象不要求哈希码不同**  
>      但不同哈希码能提升散列表性能
>
>   ---
>
>   ### **重写步骤 & 最佳实践**
>
>   #### 1. 重写 `equals()` 模板
>
>   ```java
>   @Override
>   public boolean equals(Object o) {
>       // 1. 检查是否同一对象
>       if (this == o) return true;
>   
>       // 2. 检查 null 和类型
>       if (o == null || getClass() != o.getClass()) return false;
>   
>       // 3. 类型转换
>       MyClass other = (MyClass) o;
>   
>       // 4. 逐个字段比较（使用 Objects.equals 保证空安全）
>       return Objects.equals(field1, other.field1)
>           && field2 == other.field2
>           && Arrays.equals(arrayField, other.arrayField); // 数组比较
>   }
>   ```
>
>   #### 2. 重写 `hashCode()` 模板
>
>   ```java
>   @Override
>   public int hashCode() {
>       // 使用 Objects.hash() 自动处理 null 和基本类型
>       return Objects.hash(field1, field2, Arrays.hashCode(arrayField));
>   }
>   ```
>
>   ---
>
>   ### **关键注意事项**
>
>   1. **必须同时重写两者**  
>      只重写 `equals()` 不重写 `hashCode()` 会违反约定（等价对象哈希码不同）
>
>   2. **不可变字段优先**  
>      哈希码计算应基于参与 `equals()` 比较的**不可变字段**，否则对象放入集合后可能"丢失"
>
>   3. **数组字段处理**  
>      - 比较数组：`Arrays.equals()`
>      - 计算哈希：`Arrays.hashCode()`
>
>   4. **继承场景**  
>      - 若子类添加新字段，需重写 `equals()`/`hashCode()`
>      - 或用 `instanceof` 代替 `getClass()` 允许子类相等（但会破坏对称性）
>
>   5. **性能优化**  
>      先比较哈希码（不等则对象不等），但不要直接用哈希码判断相等
>
>   ---
>
>   ### **示例：完整实现**
>
>   ```java
>   public class Person {
>       private final String name;
>       private final int age;
>       private final String[] hobbies;
>   
>       @Override
>       public boolean equals(Object o) {
>           if (this == o) return true;
>           if (o == null || getClass() != o.getClass()) return false;
>           Person person = (Person) o;
>           return age == person.age
>               && Objects.equals(name, person.name)
>               && Arrays.equals(hobbies, person.hobbies);
>       }
>   
>       @Override
>       public int hashCode() {
>           int result = Objects.hash(name, age);
>           result = 31 * result + Arrays.hashCode(hobbies);
>           return result;
>       }
>   }
>   ```
>
>   ---
>
>   ### **为什么用 `31` 作为乘数？**
>
>   - 奇素数特性：减少哈希碰撞
>   - 优化计算：`31 * i = (i << 5) - i`（JVM 自动优化）
>   - 传统习惯（非强制要求）
>
>   > ⚠️ **避免常见错误**：  
>   >
>   > - 忘记重写 `hashCode()`  
>   > - 在 `equals()` 中使用 `instanceof` 但未处理子类逻辑  
>   > - 修改参与哈希计算的字段（导致对象在集合中"丢失"）
>
> 这两者的正确实现对于在基于哈希的集合类（如 `HashMap`、`HashSet`）中存储和查找对象至关重要。



---

### 关键区别总结

| 特性       | `Object`类                       | `Objects`工具类        |
| ---------- | -------------------------------- | ---------------------- |
| **类型**   | 基类（可继承）                   | 工具类（仅静态方法）   |
| **包**     | `java.lang`                      | `java.util`            |
| **空安全** | 方法可能抛`NullPointerException` | **所有方法空指针安全** |
| **用途**   | 定义对象基础行为                 | 提供对象操作的实用方法 |
| **版本**   | Java 1.0+                        | Java 7+                |

> [!note]
>
> **最佳实践**：  
>
> - 重写`equals()`/`hashCode()`时，优先使用`Objects`工具类简化代码并保证空安全。  
> - 使用`Objects.requireNonNull()`替代手动`null`检查，提高代码健壮性。





# 抽象类

## 抽象类详解：概念、特性与最佳实践

## 一、抽象类核心概念

### 1. 定义

- **抽象类**是用 `abstract` 关键字声明的类
- 它是**不能被实例化**的类，只能被继承
- 目的：为子类提供**通用模板**和**部分实现**

### 2. 核心特性

| 特性           | 说明                                               |
| -------------- | -------------------------------------------------- |
| 实例化限制     | 不能直接创建对象：`new AbstractClass()` 会编译错误 |
| 可包含抽象方法 | 用 `abstract` 声明的方法，无方法体                 |
| 可包含具体实现 | 可以有普通方法和完整实现的方法                     |
| 成员变量       | 可以包含各种访问修饰符的字段                       |
| 构造器         | 可以有构造器（供子类初始化使用）                   |
| 继承要求       | 子类必须实现所有抽象方法（除非子类也是抽象类）     |

```java
// 抽象类示例
public abstract class Animal {
    // 字段
    protected String species;
    
    // 构造器
    public Animal(String species) {
        this.species = species;
    }
    
    // 抽象方法（无实现）
    public abstract void makeSound();
    
    // 具体方法（有实现）
    public void breathe() {
        System.out.println(species + " is breathing...");
    }
}
```

## 二、抽象方法详解

### 1. 定义规则

```java
// 正确声明
public abstract void move();

// 错误声明
private abstract void eat();  // 不能private
abstract static void sleep(); // 不能static
abstract final void run();    // 不能final
```

### 2. 实现要求

- 子类**必须实现**<u>所有抽象方法（除非子类也是抽象类）</u>
- 实现方法需满足：
  - 相同方法签名
  - 相同或更宽松的访问修饰符
  - 兼容的返回类型

```java
public class Dog extends Animal {
    public Dog() {
        super("Canine");
    }
    
    // 必须实现抽象方法
    @Override
    public void makeSound() {
        System.out.println("Woof! Woof!");
    }
}
```

## 三、抽象类 vs 接口 (Java 8+)

| 特性           | 抽象类          | 接口                                     |
| -------------- | --------------- | ---------------------------------------- |
| **实例化**     | 不能            | 不能                                     |
| **方法类型**   | 抽象+具体方法   | Java 8前：全抽象；Java 8+：默认/静态方法 |
| **字段**       | 任意类型字段    | 默认 `public static final`（常量）       |
| **构造器**     | 有              | 无                                       |
| **多继承**     | 单继承          | 多实现                                   |
| **访问修饰符** | 任意            | 默认 `public`（Java 9+ 支持private）     |
| **设计目的**   | 代码复用 + 多态 | 行为规范 + 解耦                          |

## 四、抽象类使用场景

### 1. 典型应用场景

- **模板方法模式**：定义算法骨架

  ```java
  public abstract class Game {
      // 模板方法（final防止子类覆盖算法结构）
      public final void play() {
          initialize();
          startPlay();
          endPlay();
      }
      
      abstract void initialize();
      abstract void startPlay();
      abstract void endPlay();
  }
  ```

- **部分实现共享**：提供基础功能

  ```java
  public abstract class ListAdapter {
      // 通用实现
      public int size() { /*...*/ }
      
      // 需要子类实现
      public abstract Object get(int index);
  }
  ```

- **代码复用**：减少重复代码

  ```java
  public abstract class Vehicle {
      protected Engine engine;
      
      // 通用初始化
      public Vehicle(Engine engine) {
          this.engine = engine;
      }
      
      // 子类共用方法
      public void startEngine() {
          engine.ignite();
      }
      
      // 子类自定义行为
      public abstract void move();
  }
  ```

## 五、关键注意事项

### 1. 构造器使用

- 抽象类可以有构造器（供子类初始化使用）

- 但无法直接调用：`super()` 只能在子类构造器中使用

  ```java
  public abstract class Shape {
      private String color;
      
      public Shape(String color) {
          this.color = color;
      }
  }
  
  public class Circle extends Shape {
      private double radius;
      
      public Circle(String color, double radius) {
          super(color);  // 调用抽象类构造器
          this.radius = radius;
      }
  }
  ```

### 2. 成员限制

- **可以包含**：
  - 静态方法/字段
  - final方法/字段
  - private方法/字段
- **不可包含**：
  - 私有抽象方法（无意义）
  - 静态抽象方法（违反抽象方法本质）

### 3. 继承规则

- 抽象类可以继承：
  - 具体类：`abstract class A extends ConcreteClass`
  - 抽象类：`abstract class B extends AbstractClass`
  - 实现接口：`abstract class C implements Interface`

### 4. 设计陷阱

- **过度抽象**：不需要抽象时创建抽象类会增加复杂度

- **巨型抽象类**：包含太多功能违反单一职责原则

- **抽象泄漏**：子类被迫了解父类实现细节

  ```java
  // 反例：抽象泄漏
  public abstract class Database {
      protected String connectionString;
      
      public abstract void connect();
      
      // 强制子类处理细节
      public abstract void handleTimeout();
  }
  ```

## 六、最佳实践

### 1. 命名规范

- 使用 `Abstract` 前缀增强可读性：

  ```java
  public abstract class AbstractController {...}
  public abstract class AbstractDao {...}
  ```

### 2. 访问控制

- 保护关键方法：

  ```java
  public abstract class PaymentProcessor {
      // 核心流程设为final
      public final void processPayment() {
          validate();
          executePayment();
          logTransaction();
      }
      
      protected abstract void executePayment();
      
      // 钩子方法（可选覆盖）
      protected void logTransaction() {
          // 默认实现
      }
  }
  ```

### 3. 与接口结合

- 优先组合使用：

  ```java
  // 定义行为规范
  public interface Flyable {
      void fly();
  }
  
  // 提供部分实现
  public abstract class Bird implements Flyable {
      @Override
      public void fly() {
          System.out.println("Flapping wings");
      }
      
      public abstract void buildNest();
  }
  ```

### 4. 版本演进

- 向后兼容：在抽象类中添加新方法时

  - 添加具体方法 → 安全（子类自动继承）
  - 添加抽象方法 → 破坏现有子类（需全部实现）

  ```java
  // 安全添加
  public abstract class Legacy {
      // 原始方法
      public abstract void oldMethod();
      
      // 新方法（提供默认实现）
      public void newMethod() {
          // 默认实现（空或基础逻辑）
      }
  }
  ```

## 七、典型应用案例

### 1. Java集合框架中的抽象类

```java
// java.util.AbstractList
public abstract class AbstractList<E> {
    // 必须实现的抽象方法
    public abstract E get(int index);
    
    // 基于抽象方法的默认实现
    public boolean contains(Object o) {
        for (int i=0; i<size(); i++) {
            if (Objects.equals(o, get(i))) {
                return true;
            }
        }
        return false;
    }
}
```

### 2. Servlet API

```java
// javax.servlet.http.HttpServlet
public abstract class HttpServlet extends GenericServlet {
    // 模板方法
    protected void service(HttpServletRequest req, HttpServletResponse resp) {
        String method = req.getMethod();
        if ("GET".equals(method)) {
            doGet(req, resp);
        } 
        // ...其他HTTP方法处理
    }
    
    // 抽象方法（子类实现具体行为）
    protected abstract void doGet(HttpServletRequest req, HttpServletResponse resp);
}
```

## 总结：何时使用抽象类

1. **需要共享代码**：多个相关类有共同行为
2. **需要部分实现**：提供基础功能但保留扩展点
3. **控制子类行为**：通过模板方法定义算法结构
4. **演进式设计**：预计未来需要添加通用方法

> **黄金法则**：  
>
> - 当需要 **"是一个"** 的关系且需要**代码复用**时 → 选抽象类  
> - 当需要 **"能做什么"** 的关系且需要**多态行为**时 → 选接口

通过合理使用抽象类，可以构建出灵活且易于维护的面向对象系统，在规范与实现之间取得最佳平衡。





# 枚举类

### 枚举类（Enum）介绍

枚举类（`enum`）是 Java 5 引入的特殊数据类型，**用于定义一组固定的常量**。它提供了类型安全、可读性高、功能强大的常量管理方式，替代了传统的 `public static final` 常量定义。

#### 核心特性

1. **类型安全**  
   编译器会检查类型，避免无效值。
2. **代码可读性**  
   常量名称具有自解释性（如 `Color.RED`）。
3. **内置方法**  
   自动提供 `values()`, `valueOf()` 等方法。
4. **可实现接口**  
   可定义抽象方法实现多态行为。
5. **支持 `switch`**  
   可直接在 `switch` 语句中使用。

---

### 枚举类基本语法

```java
public enum Season {
    // 枚举常量（必须放在第一行）
    SPRING("春天"), 
    SUMMER("夏天"), 
    AUTUMN("秋天"), 
    WINTER("冬天");

    private final String description; // 字段

    // 构造方法（默认 private）
    Season(String description) {
        this.description = description;
    }

    // 普通方法
    public String getDescription() {
        return description;
    }
}
```

---

### 关键注意点

#### 1. 构造方法必须是私有

枚举的构造器**默认私有**且只能私有，禁止外部实例化：

```java
private Season() { ... } // 正确
public Season() { ... }  // 编译错误！
```

#### 2. 常量声明必须在第一行

枚举常量必须在枚举类的**首行声明**，以逗号分隔，分号结尾：

```java
// ✅ 正确
RED, GREEN, BLUE;

// ❌ 错误（常量不在首行）
private int code; 
RED, GREEN; // 编译报错
```

#### 3. 常量本质是单例对象

每个枚举常量都是枚举类的**唯一实例**（JVM 保证线程安全）：

```java
System.out.println(Season.SPRING == Season.SPRING); // true
```

#### 4. 支持自定义方法

##### (1) 普通方法

```java
public String toLowerCase() {
    return this.name().toLowerCase();
}
```

##### (2) 抽象方法（每个常量需实现）

```java
public enum Operation {
    PLUS { public int apply(int a, int b) { return a + b; } },
    MINUS { public int apply(int a, int b) { return a - b; } };

    public abstract int apply(int a, int b); // 抽象方法
}
```

#### 5. 实现接口

```java
public interface Printable {
    void print();
}

public enum Color implements Printable {
    RED {
        public void print() { System.out.println("红色"); }
    },
    GREEN {
        public void print() { System.out.println("绿色"); }
    };
}
```

#### 6. 使用 `switch` 的便捷性

```java
Color color = Color.RED;
switch (color) {
    case RED -> System.out.println("红色");
    case GREEN -> System.out.println("绿色");
}
```

---

### 常用内置方法

| 方法                   | 作用                      | 示例                                   |
| ---------------------- | ------------------------- | -------------------------------------- |
| `values()`             | 返回所有枚举常量数组      | `Season[] seasons = Season.values();`  |
| `valueOf(String name)` | 根据名称返回枚举常量      | `Season s = Season.valueOf("SPRING");` |
| `name()`               | 返回常量名称（字符串）    | `"SPRING"`                             |
| `ordinal()`            | 返回常量序号（从 0 开始） | `SPRING.ordinal()` → `0`               |
| `toString()`           | 默认返回 `name()`，可重写 | 重写后返回自定义描述                   |

---

### 枚举的最佳实践

1. **优先枚举代替常量**  
   替代 `public static final int` 型常量，避免魔法数字。

2. **避免保存易变状态**  
   枚举通常设计为**不可变**（字段用 `final` 修饰）。

3. **慎用 `ordinal()`**  
   序号依赖声明顺序，重构时易出错。建议用自定义字段：

   ```java
   enum Status {
       PENDING(1), APPROVED(2);
       private final int code;
       Status(int code) { this.code = code; }
   }
   ```

4. **单例模式实现**  
   利用枚举天然单例特性实现线程安全单例：

   ```java
   public enum Singleton {
       INSTANCE;
       public void doWork() { ... }
   }
   ```

---

### 枚举的进阶用法

#### 1. 策略模式

```java
enum FileFormat {
    CSV {
        public void export(Data data) { /* CSV 导出逻辑 */ }
    },
    PDF {
        public void export(Data data) { /* PDF 导出逻辑 */ }
    };

    public abstract void export(Data data);
}
```

#### 2. 状态机

```java
enum OrderStatus {
    NEW {
        public OrderStatus next() { return PROCESSING; }
    },
    PROCESSING {
        public OrderStatus next() { return SHIPPED; }
    };

    public abstract OrderStatus next();
}
```

---

### 总结

**使用场景**：  
✅ 固定集合（状态、类型、模式等）  
✅ 需要类型安全的常量  
✅ 替代常量接口/类  

**规避场景**：  
❌ 需要动态创建实例的类型  
❌ 常量需要频繁扩展（违反开闭原则）  

枚举通过限定值域和提供丰富功能，显著提升代码的健壮性和可维护性，是 Java 中管理常量的首选方案。





# 密封类

## 密封类（Sealed Classes）介绍

密封类是 Java 17 正式引入的关键特性（Java 15/16 作为预览特性），**用于精确控制类的继承层次结构**。它允许类或接口的作者明确声明哪些类可以继承/实现它，从根本上解决了传统面向对象设计中"无限扩展性"带来的问题。

### 核心价值

- ✅ **限制可扩展性**：明确指定允许的子类集合
- ✅ **增强模式匹配**：为 `switch` 模式匹配提供完备性检查
- ✅ **提升代码安全**：防止未知子类破坏业务逻辑
- ✅ **明确设计意图**：在代码中显式表达领域模型约束

---

### 基本语法

```java
// 1. 使用 sealed 修饰符声明密封类
public sealed class Shape 
    // 2. 通过 permits 指定允许的子类
    permits Circle, Rectangle, Triangle { 
    
    // 类定义...
}

// 3. 子类必须是以下之一：
final class Circle extends Shape { /*...*/ }          // 最终类
non-sealed class Rectangle extends Shape { /*...*/ }  // 非密封类
sealed class Triangle extends Shape permits EquilTriangle { /*...*/ } // 密封子类
```

---

### 关键规则与注意事项

#### 1. 子类修饰符要求

被允许的子类**必须**显式声明以下修饰符之一：

| 修饰符       | 含义                   | 示例                             |
| ------------ | ---------------------- | -------------------------------- |
| `final`      | 禁止进一步继承         | `final class Circle ...`         |
| `sealed`     | 形成新的密封层次       | `sealed class Triangle ...`      |
| `non-sealed` | 开放继承（回归普通类） | `non-sealed class Rectangle ...` |

#### 2. 包和模块约束

- 所有子类必须与密封类**在同一模块**中
- 若密封类未声明模块，则需在**同一包**中
- 禁止跨包/跨模块继承（确保编译期可验证）

#### 3. 类声明位置

- `permits` 子句中声明的类必须存在
- 子类必须是密封类的**直接子类**（不能是孙子类）
- 密封类本身可以是抽象类或具体类

#### 4. 与 record 类的协作

密封类完美配合 record 类（不可变数据载体）：

```java
public sealed interface Expr 
    permits ConstantExpr, PlusExpr, MinusExpr {}

public record ConstantExpr(int i) implements Expr {}
public record PlusExpr(Expr a, Expr b) implements Expr {}
```

---

### 典型应用场景

#### 场景 1：精确建模领域对象

```java
// 银行账户体系
public sealed abstract class BankAccount
    permits SavingsAccount, CheckingAccount, LoanAccount {
    
    // 公共逻辑...
}

public final class SavingsAccount extends BankAccount { /* 专属逻辑 */ }
public final class CheckingAccount extends BankAccount { /* 专属逻辑 */ }
public non-sealed class LoanAccount extends BankAccount { /* 允许特殊贷款类型扩展 */ }
```

#### 场景 2：实现完备的模式匹配

```java
// 配合 switch 模式匹配（Java 17+）
double area(Shape shape) {
    return switch (shape) {
        case Circle c -> Math.PI * c.radius() * c.radius();
        case Rectangle r -> r.width() * r.height();
        case Triangle t -> 0.5 * t.base() * t.height();
        // 不需要 default！编译器验证完备性
    };
}
```

#### 场景 3：安全的状态机实现

```java
// 订单状态机
public sealed interface OrderState 
    permits Created, Paid, Shipped, Delivered, Cancelled {}

public record Created() implements OrderState {}
public record Paid(LocalDateTime paymentTime) implements OrderState {}
public record Shipped(String trackingNo) implements OrderState {}
```

---

### 设计最佳实践

1. **最小化开放扩展**  
   优先使用 `final` 或 `sealed`，仅在必要时用 `non-sealed`

2. **配合 record 使用**  
   对数据载体类使用 `record` + `sealed` 组合

3. **避免深层嵌套**  
   密封层次不宜超过 2-3 层，防止过度复杂

4. **显式表达设计约束**  
   在领域模型中用密封类表达业务规则（如："只有这三种支付方式"）

5. **单元测试验证约束**  
   添加测试确保无法创建未授权子类：

   ```java
   @Test(expected = CompilationException.class)
   public void testIllegalSubclass() {
       // 尝试编译未授权的子类
   }
   ```

---

### 与传统设计的对比

| 特性           | 普通类 / 接口        | 密封类 / 接口         |
| -------------- | -------------------- | --------------------- |
| 可扩展性       | 无限扩展             | 受限扩展 (白名单机制) |
| 模式匹配安全性 | 需要 `default` 分支  | 编译器验证分支完备性  |
| 领域约束表达力 | 通过文档约定         | 代码强制约束          |
| 子类控制粒度   | 全有或全无 (`final`) | 精细控制              |
| 多态风险       | 可能被未知子类破坏   | 防止未知子类          |

---

### 进阶特性

#### 1. 密封接口

```java
public sealed interface DataSource 
    permits DatabaseSource, FileSource, NetworkSource {}
```

#### 2. 匿名子类限制

```java
Shape circle = new Shape() {}; // 编译错误！密封类禁止匿名子类
```

#### 3. 反射 API 支持

Java 17+ 反射 API 新增方法：

```java
Class<?>[] permitted = Shape.class.getPermittedSubclasses();
boolean isSealed = Shape.class.isSealed();
```

---

### 总结

**何时使用密封类**：

- 需要限制类的扩展范围时
- 构建领域驱动设计（DDD）中的受限模型
- 配合模式匹配要求分支完备性
- 设计状态机或表达式树

**优势**：

- 🛡️ 增强代码健壮性
- 📐 精确表达领域约束
- 🔍 提升模式匹配安全性
- 📚 改善代码可维护性

密封类通过提供受控的继承机制，填补了 Java 类型系统的关键空白，是实现**安全多态**和**精确领域建模**的利器。