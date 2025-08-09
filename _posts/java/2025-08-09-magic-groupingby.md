# Mastering `groupingBy` in Java Streams – The Art of Organizing Data Like a Pro


## Intro

Imagine you’ve got a big list of employees spread across multiple departments. You want quick answers to questions like:

> - *Who belongs to which department?*
> - *What’s the total salary for each department?*
> - *Who are the top earners?*
>
> Sure, you could wrestle with loops and conditionals… but why go messy?
>
> With Java Streams groupingBy(), you can transform raw, scattered data into neatly organized insights — all in a few elegant lines of code.
>

---

## 1. Employee Class

```java
import java.util.*;
import java.util.stream.Collectors;

class Employee {
    String name;
    String department;
    int salary;

    Employee(String name, String department, int salary) {
        this.name = name;
        this.department = department;
        this.salary = salary;
    }

    public String getDepartment() { return department; }
    public int getSalary() { return salary; }
    public String getName() { return name; }

    @Override
    public String toString() {
        return name + " (" + department + ", $" + salary + ")";
    }
}
```
💡 **Tip:** Overriding `toString()` ensures readable output instead of the default memory reference.

---

## 2. Grouping Employees by Department
```java
List<Employee> employees = List.of(
    new Employee("Alice", "IT", 70000),
    new Employee("Bob", "HR", 50000),
    new Employee("Charlie", "IT", 80000),
    new Employee("Diana", "Finance", 60000),
    new Employee("Evan", "HR", 55000)
);

Map<String, List<Employee>> employeesByDept = employees.stream()
        .collect(Collectors.groupingBy(Employee::getDepartment));

System.out.println(employeesByDept);
```

**Output:**
```
{Finance=[Diana (Finance, $60000)], 
 HR=[Bob (HR, $50000), Evan (HR, $55000)], 
 IT=[Alice (IT, $70000), Charlie (IT, $80000)]}
```

---

## 3. Summing Salaries by Department
```java
Map<String, Integer> salaryByDept = employees.stream()
        .collect(Collectors.groupingBy(
                Employee::getDepartment,
                Collectors.summingInt(Employee::getSalary)
        ));

System.out.println(salaryByDept);
```

**Output:**
```
{Finance=60000, HR=105000, IT=150000}
```

---

## 4. Partitioning Employees by Salary
```java
Map<Boolean, List<Employee>> highEarners = employees.stream()
        .collect(Collectors.partitioningBy(e -> e.getSalary() > 60000));

System.out.println(highEarners);
```

**Output:**
```
true  → [Alice (IT, $70000), Charlie (IT, $80000)]
false → [Bob (HR, $50000), Diana (Finance, $60000), Evan (HR, $55000)]
```

---

## Why `groupingBy()` Rocks

- Removes the need for nested loops.  
- Works seamlessly with custom objects.  
- Can be combined with other collectors like `summingInt`, `mapping`, `counting`, etc.  
- Makes code cleaner and more maintainable.

---

### Final Words
Mastering `groupingBy()` means turning scattered data into organized insights — whether it’s employees, students, products, or any dataset.

---

## 🤝 Let's Connect

I’d love to connect, collaborate, and share ideas with fellow tech enthusiasts.  

- 💻 [Blog – GitHub](https://shossain786.github.io/)
- 💼 [LinkedIn – Saddam Hussain](https://www.linkedin.com/in/hossain-mdsaddam/)  
- 💻 [GitHub – @shossain786](https://github.com/shossain786)  
- 🚀 [GitHub – Panjatan Coders](https://github.com/PanjatanCoders)  

---
