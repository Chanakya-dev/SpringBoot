### **Bean Scopes in Spring Framework**

In Spring, the **scope** of a bean defines its lifecycle and visibility within the Spring container. Spring provides several types of bean scopes, which allow you to manage how and when beans are created, how they are shared, and how their lifecycle is managed. Understanding bean scopes is crucial for designing applications that handle resources effectively and ensure correct behavior in different environments.

Here is an explanation of each bean scope in **Spring**:

---

### 1. **Singleton Scope**

* **Default Scope** in Spring.
* **Definition**: In Singleton scope, Spring container creates only **one instance** of the bean per Spring IoC container. All requests for this bean will return the **same instance**.
* **Use Case**: This scope is ideal for stateless beans or services that are used across the entire application, like service classes, repositories, etc.
* **Lifecycle**: The bean is created **once** when the application context is initialized and destroyed when the container is destroyed (at application shutdown).

#### Example:

```xml
<bean id="myBean" class="com.example.MyClass" scope="singleton"/>
```

* **Key Point**: A singleton bean is shared across multiple components of the application, ensuring resource efficiency and consistency.

---

### 2. **Prototype Scope**

* **Definition**: In Prototype scope, Spring creates a **new instance** of the bean each time it is requested. Unlike singleton beans, each request gets a **different instance**.
* **Use Case**: Useful when a bean maintains state or when the bean's properties should not be shared across multiple usages.
* **Lifecycle**: The bean is created every time it is requested, and Spring does not manage the destruction of these beans; it's up to the user to handle destruction if needed.

#### Example:

```xml
<bean id="myBean" class="com.example.MyClass" scope="prototype"/>
```

* **Key Point**: The bean is created **multiple times** depending on how many requests are made, ensuring that state is not shared across instances.

---

### 3. **Request Scope**

* **Definition**: The **request scope** is specific to **web applications**. A new instance of the bean is created for each **HTTP request** made to the server. Each bean instance is available throughout the lifetime of a single HTTP request, and the bean is discarded once the request is processed.
* **Use Case**: Typically used in web applications when each HTTP request needs a separate bean, for example, a user session object that should be isolated per request.
* **Lifecycle**: The bean is created when an HTTP request is received and destroyed when the request is completed (i.e., the request ends).

#### Example:

```xml
<bean id="myBean" class="com.example.MyClass" scope="request"/>
```

* **Key Point**: This scope ensures that each HTTP request gets a **unique instance** of the bean.

---

### 4. **Session Scope**

* **Definition**: The **session scope** is also specific to **web applications**. A new instance of the bean is created once per **HTTP session**. The bean is tied to a particular user's session and is available for the entire duration of the session (typically until the user logs out or the session expires).
* **Use Case**: This scope is ideal for objects that need to store information specific to a user's session, like a shopping cart or user preferences.
* **Lifecycle**: The bean is created when a session starts and destroyed when the session ends.

#### Example:

```xml
<bean id="myBean" class="com.example.MyClass" scope="session"/>
```

* **Key Point**: The bean persists across multiple requests in the same session, but it is **not shared** across sessions.

---

### 5. **Application Scope**

* **Definition**: The **application scope** is also used in **web applications**. A bean in this scope is created **once per servlet context** (i.e., once per web application). All users accessing the application will share the same instance of the bean.
* **Use Case**: This scope is used for beans that need to be shared across multiple sessions and requests within the application. For example, application-wide configuration settings or logging services.
* **Lifecycle**: The bean is created when the application starts (i.e., when the servlet context is initialized) and is destroyed when the application is shut down.

#### Example:

```xml
<bean id="myBean" class="com.example.MyClass" scope="application"/>
```

* **Key Point**: The bean is shared across multiple sessions and requests, providing a **single shared instance** for the entire web application.

---

### 6. **Websocket Scope**

* **Definition**: This scope is used in **websocket-based applications**. A new bean instance is created for each **websocket connection**.
* **Use Case**: Ideal for handling data that needs to be shared only within the lifecycle of a websocket connection.
* **Lifecycle**: The bean is created when the websocket connection starts and destroyed when the connection is closed.

#### Example:

```xml
<bean id="myBean" class="com.example.MyClass" scope="websocket"/>
```

* **Key Point**: The bean is scoped to a **specific websocket connection**.

---

### **Comparison of Bean Scopes in Spring**

| **Scope**       | **Instance Creation**                 | **Context**                                      | **Lifecycle**                                                       |
| --------------- | ------------------------------------- | ------------------------------------------------ | ------------------------------------------------------------------- |
| **Singleton**   | One instance per container            | Shared across the entire container               | Created once on startup and destroyed on container shutdown         |
| **Prototype**   | New instance for each request         | Used when each request needs a new bean instance | Created every time requested, not managed by Spring for destruction |
| **Request**     | One instance per HTTP request         | Web applications (per HTTP request)              | Created per request and destroyed when the request ends             |
| **Session**     | One instance per HTTP session         | Web applications (per HTTP session)              | Created per session and destroyed when the session ends             |
| **Application** | One instance per web application      | Web applications (shared across the application) | Created on application startup and destroyed on shutdown            |
| **WebSocket**   | One instance per websocket connection | WebSocket-based applications                     | Created per websocket connection and destroyed when closed          |
