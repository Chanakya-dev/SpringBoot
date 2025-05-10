### **Java-Based Configuration using Annotations in Spring**

In Spring, annotations are used for defining beans and configuring the application context. The main advantages of annotation-based configuration are that it reduces the need for verbose XML files, promotes Java-based configuration, and allows for better integration with modern IDE features.

### **Steps for Annotation-Based Configuration**

1. **Define your beans using annotations** like `@Component`, `@Service`, `@Repository`, `@Controller`, and `@Bean`.
2. **Use `@Autowired` for Dependency Injection** to automatically inject beans into your class.
3. **Use `@Configuration` to define configuration classes** that replace `beans.xml`.
4. **Use `@ComponentScan` to tell Spring where to scan for components** (beans) in your project.
5. **Create a main application class** to load the context and call your beans.

---

### **Main Java Class with Annotation-Based Configuration**

#### **1. Configuration Class (`AppConfig.java`)**

```java
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.ComponentScan;
import org.springframework.context.annotation.Configuration;

@Configuration  // Marks this class as a source of bean definitions
@ComponentScan(basePackages = "com.example")  // Scans the specified package for beans
public class AppConfig {

    @Bean  // Marks the method as a bean definition
    public Mobile samsungMobile() {
        return new Samsung(snapdragonProcessor());
    }

    @Bean
    public Mobile lavaMobile() {
        return new Lava(exynosProcessor());
    }

    @Bean
    public Processor snapdragonProcessor() {
        return new Snapdragon();
    }

    @Bean
    public Processor exynosProcessor() {
        return new Exynos();
    }
}
```

* **`@Configuration`**: This annotation tells Spring that this class is a source of bean definitions. It is the equivalent of the `beans.xml` file in XML-based configuration.
* **`@ComponentScan`**: This annotation automatically scans the specified base packages for components, so you don’t have to define each bean manually. It looks for classes with annotations like `@Component`, `@Service`, etc.
* **`@Bean`**: This annotation marks methods as producing beans to be managed by the Spring container. Each method annotated with `@Bean` returns a bean that Spring will manage.

#### **2. Bean Classes (`Mobile`, `Samsung`, `Lava`, `Processor`, `Snapdragon`, `Exynos`)**

##### **Mobile.java** (Interface)

```java
public interface Mobile {
    void makeCall();
}
```

##### **Samsung.java** (Bean)

```java
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Component;

@Component
public class Samsung implements Mobile {
    
    private Processor processor;
    
    @Autowired  // Automatically injects the dependency
    public Samsung(Processor processor) {
        this.processor = processor;
    }

    @Override
    public void makeCall() {
        System.out.println("Making a call using Samsung Mobile with " + processor.getProcessorType() + " processor.");
    }
}
```

##### **Lava.java** (Bean)

```java
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Component;

@Component
public class Lava implements Mobile {
    
    private Processor processor;
    
    @Autowired
    public Lava(Processor processor) {
        this.processor = processor;
    }

    @Override
    public void makeCall() {
        System.out.println("Making a call using Lava Mobile with " + processor.getProcessorType() + " processor.");
    }
}
```

##### **Processor.java** (Interface)

```java
public interface Processor {
    String getProcessorType();
}
```

##### **Snapdragon.java** (Processor Bean)

```java
import org.springframework.stereotype.Component;

@Component
public class Snapdragon implements Processor {

    @Override
    public String getProcessorType() {
        return "Snapdragon";
    }
}
```

##### **Exynos.java** (Processor Bean)

```java
import org.springframework.stereotype.Component;

@Component
public class Exynos implements Processor {

    @Override
    public String getProcessorType() {
        return "Exynos";
    }
}
```

#### **Explanation of the Classes:**

* **`Mobile`** is an interface that provides the `makeCall()` method.
* **`Samsung`** and **`Lava`** are concrete implementations of the `Mobile` interface. They have dependencies on `Processor`, which are injected through constructors.
* **`Processor`** is an interface with a method `getProcessorType()`.
* **`Snapdragon`** and **`Exynos`** are implementations of the `Processor` interface that return the processor type as a string.

#### **3. Main Application Class (`MainApp.java`)**

```java
import org.springframework.context.annotation.AnnotationConfigApplicationContext;

public class MainApp {
    public static void main(String[] args) {
        // Create the application context using Java configuration
        AnnotationConfigApplicationContext context = new AnnotationConfigApplicationContext(AppConfig.class);

        // Retrieve the Samsung and Lava beans from the context
        Mobile samsung = context.getBean(Samsung.class);
        Mobile lava = context.getBean(Lava.class);

        // Use the beans
        samsung.makeCall();
        lava.makeCall();

        // Close the context
        context.close();
    }
}
```

#### **Explanation of MainApp:**

* **`AnnotationConfigApplicationContext`**: This is used to load the application context using Java-based configuration. It takes the `AppConfig.class` as a parameter to read the configuration.
* **`context.getBean()`**: This retrieves the beans from the context. In this case, we use `getBean(Samsung.class)` and `getBean(Lava.class)` to fetch the respective beans.
* **`samsung.makeCall()` and `lava.makeCall()`**: These method calls demonstrate the functionality of the `Mobile` beans.
* **`context.close()`**: This closes the application context when no longer needed, releasing resources.

---

### **Directory Structure Example:**

```
src
├── main
│   ├── java
│   │   └── com
│   │       └── example
│   │           ├── AppConfig.java
│   │           ├── MainApp.java
│   │           ├── Mobile.java
│   │           ├── Samsung.java
│   │           ├── Lava.java
│   │           ├── Processor.java
│   │           ├── Snapdragon.java
│   │           └── Exynos.java
```

### **Summary of Steps for Annotation-based Configuration:**

1. **Create a Configuration Class**: Use `@Configuration` to mark the class as a source of bean definitions.
2. **Define Beans with `@Bean`**: Use `@Bean` for methods that return beans. Beans are created and managed by Spring.
3. **Use `@Component`**: Mark classes as components (beans) using `@Component` or specialized annotations like `@Service`, `@Repository`, etc.
4. **Use `@Autowired` for Dependency Injection**: Automatically inject dependencies into beans using `@Autowired` on constructors or fields.
5. **Use `AnnotationConfigApplicationContext`**: Load the configuration and retrieve beans using Java-based configuration.
6. **Invoke Methods on Beans**: Access the beans and call their methods to demonstrate functionality.
