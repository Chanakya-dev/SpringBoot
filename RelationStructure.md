## ✅ **Hospital Management System with Extended Entities**

### **Entities Overview**

1. **Patient**

   * Represents a patient receiving medical care.
   * Has a relationship with **Doctor**, **Beds**, and **Medical Records**.

2. **Doctor**

   * Represents a medical professional in the hospital.
   * Works with **Appointments**, **Departments**, and **Medical Records**.

3. **Beds**

   * Represents the available beds in the hospital.
   * Associated with **Patients** who are assigned to them.

4. **Canteen**

   * Represents the hospital canteen.
   * Deals with **Meals** or **Orders** for Patients.

5. **Medical Lab**

   * Represents the hospital’s laboratory services.
   * Deals with **Tests** and **Reports** for Patients.

---

### **Relationships between Entities**

| Entity Pair              | Relationship     | Description                                                                                        |
| ------------------------ | ---------------- | -------------------------------------------------------------------------------------------------- |
| `Patient` ↔ `Doctor`     | **Many-to-One**  | A patient is assigned to one doctor, but a doctor can have many patients.                          |
| `Patient` ↔ `Beds`       | **One-to-One**   | A patient can be assigned to one bed.                                                              |
| `Doctor` ↔ `Department`  | **Many-to-One**  | Many doctors can belong to a department.                                                           |
| `Patient` ↔ `Canteen`    | **Many-to-Many** | Patients can order multiple meals from the canteen, and meals can be ordered by multiple patients. |
| `Patient` ↔ `MedicalLab` | **One-to-Many**  | A patient can have many medical tests in the lab.                                                  |

---

## 📂 **Project Structure**

```
com.hospital
├── controller
│   └── PatientController.java
│   └── DoctorController.java
│   └── BedController.java
│   └── CanteenController.java
│   └── MedicalLabController.java
├── entity
│   └── Patient.java
│   └── Doctor.java
│   └── Bed.java
│   └── Canteen.java
│   └── MedicalLab.java
├── repository
│   └── PatientRepository.java
│   └── DoctorRepository.java
│   └── BedRepository.java
│   └── CanteenRepository.java
│   └── MedicalLabRepository.java
├── service
│   └── PatientService.java
│   └── DoctorService.java
│   └── BedService.java
│   └── CanteenService.java
│   └── MedicalLabService.java
├── dto
│   └── PatientDTO.java
│   └── DoctorDTO.java
├── exception
│   └── GlobalExceptionHandler.java
├── HospitalManagementApplication.java
└── application.properties
```

---

### **Entities: JPA Model Examples**

#### **1. Patient Entity**

```java
@Entity
public class Patient {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String name;
    private String contact;
    
    @ManyToOne
    @JoinColumn(name = "doctor_id")
    private Doctor doctor;

    @OneToOne
    @JoinColumn(name = "bed_id")
    private Bed bed;

    @OneToMany(mappedBy = "patient")
    private List<MedicalRecord> medicalRecords;

    @ManyToMany
    @JoinTable(
        name = "patient_canteen_order",
        joinColumns = @JoinColumn(name = "patient_id"),
        inverseJoinColumns = @JoinColumn(name = "canteen_id")
    )
    private List<Canteen> canteenOrders;
}
```

#### **2. Doctor Entity**

```java
@Entity
public class Doctor {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String name;
    private String specialty;

    @ManyToOne
    @JoinColumn(name = "department_id")
    private Department department;

    @OneToMany(mappedBy = "doctor")
    private List<Patient> patients;
}
```

#### **3. Bed Entity**

```java
@Entity
public class Bed {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String bedNumber;
    private boolean isAvailable;

    @OneToOne(mappedBy = "bed")
    private Patient patient;
}
```

#### **4. Canteen Entity**

```java
@Entity
public class Canteen {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String name;
    private String menu;

    @ManyToMany(mappedBy = "canteenOrders")
    private List<Patient> patients;
}
```

#### **5. Medical Lab Entity**

```java
@Entity
public class MedicalLab {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String testName;
    private String result;

    @ManyToOne
    @JoinColumn(name = "patient_id")
    private Patient patient;
}
```

---

## 🧩 **Key Concepts You Can Explain:**

### 🔹 **One-to-Many**: Patient ↔ Medical Records

A patient can have multiple medical records, while each record is related to one specific patient.

### 🔹 **Many-to-One**: Patient ↔ Doctor

Many patients can be assigned to one doctor, so this relationship is typically represented as a **Many-to-One** in JPA.

### 🔹 **Many-to-Many**: Patient ↔ Canteen

Patients can order multiple meals, and a meal can be ordered by multiple patients. This requires a **Many-to-Many** relationship.

### 🔹 **One-to-One**: Patient ↔ Bed

Each patient is assigned exactly one bed, and each bed is occupied by exactly one patient.

### 🔹 **JPA Cascade Types**:

You can define how operations like **save** and **delete** should cascade from one entity to others (e.g., when saving a patient, you might want to save the associated records too).
