---
date: '2025-08-07T16:28:56+08:00'
title: '概述'
weight: 1
pre: /java笔记
---

第一章目录：

<!-- {{<back-to-top>}} -->

- [[Overview]]
- [[Getting Started]]
- [[基本程序设计结构]]

# <a id = top>I. Overview</a>


## 简述

Java是一种广泛使用的编程语言，由Sun Microsystems（现为Oracle公司的一部分）于1995年发布。它的设计目标是“一次编写，到处运行”（Write Once, Run Anywhere - WORA），这意味着<u>Java程序可以在多个平台上运行而无需重新编译</u>，这得益于**Java虚拟机（JVM）**的实现。

### 核心特性

- **面向对象**：Java是一种纯粹的面向对象编程语言（除了基本数据类型）。它支持类、对象、继承、封装、多态等特性。

- **平台无关性**：Java源代码通过**JVM（Java 虚拟机）**被编译成字节码（`.class文件`），然后在JVM上运行。只要有对应平台的JVM，就可以运行Java程序，实现跨平台。

- **健壮性**：Java强调早期错误检查（编译时）和运行时检查，避免了很多编程错误。它还有自动内存管理（垃圾回收机制），减少了内存泄漏等问题。
  - **强类型语言**：编译时严格检查数据类型。

- **安全性**：Java提供了安全管理器，可以设置安全策略，限制代码的访问权限，适用于网络环境。

- **多线程支持**：Java内置了多线程功能，内置线程 API（`java.lang.Thread`），允许同时执行多个任务，提高程序效率。

- **分布式**：Java的网络库支持TCP/IP、HTTP、FTP等协议，便于开发分布式应用。

- **动态性**：Java支持动态加载类，运行时可以加载需要的类。

### 基本语法

- \- **程序结构**：Java程序由类（`class`）组成。每个程序至少有一个类，且包含一个`main`方法作为程序入口。

```java
  public class HelloWorld {
	 // main方法程序入口
     public static void main(String[] args) {
          System.out.println("Hello, World!");
     }

  }
```

- \- **数据类型**：分为**基本数据类型**（如`int`, `double`, `char`, `boolean`等）和**引用数据类型**（类、接口、数组）。

- \- **控制结构**：包括条件语句（`if-else`, `switch`）、循环语句（`for`, `while`, `do-while`）和跳转语句（`break`, `continue`, `return`）。
- \-**多线程**：
  - 创建方式：继承 `Thread` 或实现 `Runnable`。
  - 同步机制：`synchronized` 关键字、`Lock` 接口。

- \- **异常处理**：使用`try-catch-finally`块来处理异常，保证程序在遇到错误时也能优雅处理。
  - **异常体系**：
    - 受检异常（如 `IOException`）：必须处理。
    - 非受检异常（如 `NullPointerException`）：运行时错误。

- \- **集合框架**：提供了一套丰富的**集合类**（如`List`, `Set`, `Map`等接口，具体实现类 `ArrayList`, `HashMap`），用于存储和操作数据集合。

- \- **输入输出**：通过java.io包中的类进行文件读写和流操作。
  - **I/O 流**：
    - 字节流：`InputStream`/`OutputStream`（处理二进制数据）。
    - 字符流：`Reader`/`Writer`（处理文本）。

###  面向对象特性

- \- **类与对象**：类是对象的模板，对象是类的实例（`new ClassName()`）。

- \- **继承**：子类可以继承父类的属性和方法（单继承），使用`extends`关键字。
  - `extends` 实现类继承（单继承），支持方法重写（`@Override`）。
  - **抽象类**：`abstract class` 包含抽象方法和具体方法。

- \- **接口**：定义一组方法签名（Java 8后可以有默认方法），类通过`implements`实现接口，支持多继承。
  - `interface` 定义抽象方法（Java 8 支持默认方法）。

- \- **多态**：同一个方法调用可以根据对象的不同而具有不同的行为（通过方法重写和接口实现）。
  - 父类引用指向子类对象（如 `Animal a = new Dog()`）。

- \- **封装**：通过访问修饰符（`public`, `protected`, `private`）控制对类成员的访问。
  - 通过 `private` 隐藏数据，`getter/setter` 提供访问。

### 开发工具和平台

**技术体系**

- \-**JDK（Java Development Kit）**开发工具包，包含：
- \-**JRE（Java Runtime Environment）**：运行环境（含 JVM 和核心库）。

- \-**编译器**（`javac`）、调试器、文档生成器（`javadoc`）等。

- **JVM（Java Virtual Machine）：**
  执行字节码的核心组件，提供内存管理和优化。

- \- **开发工具**：如Eclipse、IntelliJ IDEA、NetBeans等，提供代码编辑、调试、测试等功能。

- \-**构建工具**：Maven、Gradle。

- \-**框架：**

  - Spring（依赖注入、微服务）。

  - MyBatis（ORM 数据库映射）。

  - Apache Commons（工具库）。
  - **...**

### 应用领域

- **企业应用**：`Spring` 框架开发后端服务。
- **移动开发**：Android 应用（基于 Java/Kotlin）。
- **大数据**：Hadoop、Spark 的底层实现。
- **Web 开发**：`Servlet`、Thymeleaf技术（如 Tomcat 服务器）。
- **桌面应用**：JavaFX 或 Swing 开发 GUI。

### 版本演进 

- \-**Java 8（2014）**：里程碑版本，引入：
  - **Lambda 表达式**：简化函数式编程。
  - **Stream API**：集合操作（如过滤、映射）。
  - **新的日期时间 API**：`java.time` 包。
- \-**Java 11（2018）**：首个长期支持（LTS）版本。
- \-**Java 17（2021）**：最新 LTS，支持密封类（`sealed class`）、模式匹配等。

### 总结

Java以其**稳定性**、**跨平台能力**和**强大的生态系统**，成为企业级应用和Android开发的主流语言。尽管面临其他语言的竞争（如Kotlin、Go、Python等），但Java凭借其持续更新和庞大的社区支持，仍然保持着极高的流行度。





## 搭建开发环境

### Java 环境搭建与配置指南（Windows/macOS/Linux）

#### **一、安装 JDK**

1. **下载 JDK**

   - 官网地址：[Oracle JDK](https://www.oracle.com/java/technologies/downloads/) 或 [OpenJDK](https://adoptium.net/)
   - 选择版本：推荐 **JDK 17**（LTS长期支持版）

2. **安装步骤**

   - **Windows**：

     1. 运行安装包（如 `jdk-17_windows-x64_bin.exe`）
     2. 按向导安装（默认路径：`C:\Program Files\Java\jdk-17\`）
     3. 勾选"安装公共JRE"（可选）

   - **macOS**：

     ```bash
     # 使用 Homebrew 安装
     brew install openjdk@17
     # 或下载 .dmg 安装包双击安装
     ```

   - **Linux**：

     ```bash
     # Ubuntu/Debian
     sudo apt install openjdk-17-jdk
     
     # CentOS/RHEL
     sudo yum install java-17-openjdk-devel
     ```

---

#### **二、配置环境变量**

> :classical_building: 关键变量：`JAVA_HOME`、`PATH`

- **Windows 配置**：

  1. 右键"此电脑" → 属性 → 高级系统设置 → 环境变量
  2. **新建系统变量**：
     - 变量名：`JAVA_HOME`
     - 变量值：JDK安装路径（如 `C:\Program Files\Java\jdk-17`）
  3. **编辑 `PATH` 变量**：
     - 新增条目：`%JAVA_HOME%\bin`

  ![Windows环境变量配置示意图](https://example.com/java_env_windows.png)

- **macOS/Linux 配置**：

  1. 编辑配置文件：

     ```bash
     # macOS
     nano ~/.zshrc   # 或 ~/.bash_profile
     
     # Linux
     nano ~/.bashrc  # 或 /etc/profile
     ```

  2. 添加以下内容：

     ```bash
     export JAVA_HOME=$(/usr/libexec/java_home -v 17)  # macOS专用写法
     # 或直接指定路径（通用）
     export JAVA_HOME=/usr/lib/jvm/java-17-openjdk-amd64
     export PATH=$JAVA_HOME/bin:$PATH
     ```

  3. 生效配置：

     ```bash
     source ~/.zshrc   # 对应修改的文件
     ```

---

#### **三、验证安装**

打开终端/命令提示符，执行：

```bash
java -version
javac -version
```

成功示例输出：

```
java version "17.0.8" 2023-07-18 LTS
Java(TM) SE Runtime Environment (build 17.0.8+9-LTS-211)
Java HotSpot(TM) 64-Bit Server VM (build 17.0.8+9-LTS-211, mixed mode)

javac 17.0.8
```

---

#### **四、开发工具配置（可选）**

1. **IDE 安装**：

   - [IntelliJ IDEA](https://www.jetbrains.com/idea/)：社区版免费
   - [Eclipse](https://www.eclipse.org/downloads/)
   - [VS Code](https://code.visualstudio.com/) + Java扩展包

2. **项目构建工具**：

   - Maven：

     ```bash
     # macOS/Linux 安装
     brew install maven
     
     # 验证
     mvn -v
     ```

   - Gradle：

     ```bash
     # macOS/Linux 安装
     brew install gradle
     ```

---

#### **五、常见问题解决**

1. **`java` 命令无效**：

   - 检查 `PATH` 是否包含 `%JAVA_HOME%\bin`
   - 重启终端或计算机

2. **版本冲突**：

   - 多版本管理工具：

     - Windows：[JEnv Win](https://github.com/FelixSelter/JEnv-for-Windows)

     - macOS/Linux：[jEnv](http://www.jenv.be/)

       ```bash
       # jEnv 使用示例
       jenv add /usr/lib/jvm/java-17-openjdk-amd64
       jenv global 17.0
       ```

3. **权限问题（Linux/macOS）**：

   ```bash
   sudo chown -R $(whoami) $JAVA_HOME
   ```

---

### 环境验证示例程序

创建 `HelloWorld.java`：

```java
public class HelloWorld {
    public static void main(String[] args) {
        System.out.println("Java环境配置成功！");
    }
}
```

编译运行：

```bash
javac HelloWorld.java
java HelloWorld
```

输出结果：

```bash
Java环境配置成功！
```

> [!tip]
>
> 提示：建议定期更新JDK版本（通过 `java -version` 检查安全补丁）。