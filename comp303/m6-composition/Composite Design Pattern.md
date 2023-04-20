Say you have a group of objects, related in some way (perhaps by composition), and you want to treat some or all of them like a single entity. 

In the Composite pattern, there are two types of objects: "composite" objects and "leaf" objects. 
- A **composite object** is an object that contains one or more other objects.
- A **leaf object** is an object that has no children and represents an indivisible element of the hierarchy.

- both composite and leaf objects should have a ***common interface*** that allows them to be treated in the same way. 
	- This interface can include methods for adding and removing child objects, as well as methods for accessing the children of a composite object. OR the adding of child objects can be reserved for composite objects only, and done in the constructor. 

- Composite object == aggregate object

- an **aggregate object** is an object that contains a collection of other objects, which can be either of the same type or of different types.
	- aggregate object does not have a predefined structure or hierarchy. Instead, it simply contains a collection of objects that are related in some way.

## Example
```java
public interface Employee {
    String getName();
    void setName(String name);
    void print();
}

public class Developer implements Employee {
    private String name;

    public Developer(String name) {
        this.name = name;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public void print() {
        System.out.println("Developer: " + name);
    }
}

public class Manager implements Employee {
    private String name;
    private List<Employee> employees = new ArrayList<>();

    public Manager(String name) {
        this.name = name;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public void addEmployee(Employee employee) {
        employees.add(employee);
    }

    public void removeEmployee(Employee employee) {
        employees.remove(employee);
    }

    public void print() {
        System.out.println("Manager: " + name);
        for (Employee employee : employees) {
            employee.print();
        }
    }
}
```

In this example, the `Employee` interface defines the common methods that are used to manipulate employees. The `Developer` class is a leaf object that represents an individual developer, while the `Manager` class is a composite object that represents a manager who oversees a group of employees.

The `Manager` class has methods for adding and removing child employees, as well as a `print()` method that prints out the name of the manager and all the employees under them. This allows us to treat the entire hierarchy of employees as if it were a single object.

## Combining Composite and Decorator
[[Decorator Design Pattern#Combining Composite and Decorator]]
