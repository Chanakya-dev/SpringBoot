Sure! Below is a **complete example of all CRUD operations (Create, Read, Update, Delete)** using **Hibernate with annotation-based configuration** (no Spring Boot yet).

---

## ✅ Entity Class – `Student.java`

```java
package com.example;

import javax.persistence.*;

@Entity
@Table(name = "students")
public class Student {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private int id;

    private String name;
    private String course;

    public Student() {}

    public Student(String name, String course) {
        this.name = name;
        this.course = course;
    }

    // Getters and Setters
    public int getId() { return id; }
    public void setId(int id) { this.id = id; }

    public String getName() { return name; }
    public void setName(String name) { this.name = name; }

    public String getCourse() { return course; }
    public void setCourse(String course) { this.course = course; }

    @Override
    public String toString() {
        return "Student [id=" + id + ", name=" + name + ", course=" + course + "]";
    }
}
```

---

## 🔧 Hibernate Config – `hibernate.cfg.xml`

Place this inside `src/main/resources/hibernate.cfg.xml`:

```xml
<!DOCTYPE hibernate-configuration PUBLIC
 "-//Hibernate/Hibernate Configuration DTD 3.0//EN"
 "http://hibernate.sourceforge.net/hibernate-configuration-3.0.dtd">
<hibernate-configuration>
 <session-factory>
    <property name="hibernate.connection.driver_class">com.mysql.cj.jdbc.Driver</property>
    <property name="hibernate.connection.url">jdbc:mysql://localhost:3306/hibernatedb</property>
    <property name="hibernate.connection.username">root</property>
    <property name="hibernate.connection.password">root</property>

    <property name="hibernate.dialect">org.hibernate.dialect.MySQL8Dialect</property>
    <property name="hibernate.show_sql">true</property>
    <property name="hibernate.hbm2ddl.auto">update</property>

    <mapping class="com.example.Student"/>
 </session-factory>
</hibernate-configuration>
```

---

## 🛠️ Hibernate Utility – `HibernateUtil.java`

```java
package com.example;

import org.hibernate.SessionFactory;
import org.hibernate.cfg.Configuration;

public class HibernateUtil {
    private static final SessionFactory sessionFactory;

    static {
        try {
            sessionFactory = new Configuration()
                    .configure("hibernate.cfg.xml")
                    .buildSessionFactory();
        } catch (Throwable ex) {
            throw new ExceptionInInitializerError(ex);
        }
    }

    public static SessionFactory getSessionFactory() {
        return sessionFactory;
    }
}
```

---

## 📦 CRUD Operations – `Main.java`

```java
package com.example;

import org.hibernate.Session;
import org.hibernate.Transaction;

public class Main {
    public static void main(String[] args) {

        // 🔸 CREATE
        createStudent("Alice", "Spring Boot");

        // 🔸 READ
        readStudent(1);

        // 🔸 UPDATE
        updateStudent(1, "Alice Johnson", "Hibernate");

        // 🔸 DELETE
        deleteStudent(1);
    }

    public static void createStudent(String name, String course) {
        Session session = HibernateUtil.getSessionFactory().openSession();
        Transaction tx = session.beginTransaction();

        Student student = new Student(name, course);
        session.save(student);

        tx.commit();
        session.close();
        System.out.println("Student Created: " + student);
    }

    public static void readStudent(int id) {
        Session session = HibernateUtil.getSessionFactory().openSession();
        Student student = session.get(Student.class, id);
        session.close();

        if (student != null)
            System.out.println("Student Fetched: " + student);
        else
            System.out.println("Student not found with ID: " + id);
    }

    public static void updateStudent(int id, String newName, String newCourse) {
        Session session = HibernateUtil.getSessionFactory().openSession();
        Transaction tx = session.beginTransaction();

        Student student = session.get(Student.class, id);
        if (student != null) {
            student.setName(newName);
            student.setCourse(newCourse);
            session.update(student);
            tx.commit();
            System.out.println("Student Updated: " + student);
        } else {
            System.out.println("Student not found for update with ID: " + id);
            tx.rollback();
        }

        session.close();
    }

    public static void deleteStudent(int id) {
        Session session = HibernateUtil.getSessionFactory().openSession();
        Transaction tx = session.beginTransaction();

        Student student = session.get(Student.class, id);
        if (student != null) {
            session.delete(student);
            tx.commit();
            System.out.println("Student Deleted with ID: " + id);
        } else {
            System.out.println("Student not found for deletion with ID: " + id);
            tx.rollback();
        }

        session.close();
    }
}
```

---

## ✅ Output Example:

```
Student Created: Student [id=1, name=Alice, course=Spring Boot]
Student Fetched: Student [id=1, name=Alice, course=Spring Boot]
Student Updated: Student [id=1, name=Alice Johnson, course=Hibernate]
Student Deleted with ID: 1
```
