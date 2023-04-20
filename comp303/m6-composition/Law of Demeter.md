## Delegation Chains Antipattern
The Law of Demeter, also known as the principle of least knowledge, is a design principle in object-oriented programming that ***aims to reduce the coupling between objects***. Reduced coupling makes it easier to maintain and update parts of the system without unwanted side effects. 

- Message chain antipattern: you don't want to be reaching through a large chain of object references in one go. 
	- For example: `aFoundations.getPile(FIRST).getCards().add(pCard);`
The "Law of Demeter" is a tool for avoiding this antipattern:

## Law of Demeter 
- The code of a method should only access the following instance variables: 
	- implicit parameter = `this`. 
		- So basically it just means the instance variables of the object that the method belongs to. 
	- The arguments passed to the method;
	- Any new object created within the method; 
	- (If need be) globally available objects.

To respect this guideline it becomes necessary to **provide additional services** in classes that occupy an intermediate position in an aggregation/delegation chain so that the clients do not need to manipulate the internal objects encapsulated by these objects.

## Bad Practice Example
```java
public class Customer {
    private String name;
    private List<Order> orders;

    public Customer(String name, List<Order> orders) {
        this.name = name;
        this.orders = orders;
    }

    public List<Order> getOrders() {
        return orders;
    }
}

public class Order {
    private List<Item> items;

    public Order(List<Item> items) {
        this.items = items;
    }

    public List<Item> getItems() {
        return items;
    }
}

public class Item {
    private String name;
    private double price;

    public Item(String name, double price) {
        this.name = name;
        this.price = price;
    }

    public double getPrice() {
        return price;
    }
}

// Client code
Customer customer = new Customer("John Doe", Arrays.asList(
    new Order(Arrays.asList(new Item("Apple", 1.0), new Item("Orange", 1.5))),
    new Order(Arrays.asList(new Item("Banana", 0.5), new Item("Pineapple", 3.0)))
));

double total = 0.0;
for (Order order : customer.getOrders()) {
    for (Item item : order.getItems()) {
        total += item.getPrice();
    }
}
System.out.println("Total: $" + total);

```
In this example, the `Customer` class has a list of `Order` objects, and the `Order` class has a list of `Item` objects. The client code accesses the `Customer` object and iterates over its `Order` objects, and then iterates over each `Item` object in each `Order` object to calculate the total price.

This violates the Law of Demeter, because the client code is accessing the `Order` and `Item` objects indirectly through the `Customer` object. Instead, the `Customer` class should provide a method to return the total price of all orders, so that the client code only needs to interact with the `Customer` object directly:

## Good Practice Example
```java
public class Customer {
    private String name;
    private List<Order> orders;

    public Customer(String name, List<Order> orders) {
        this.name = name;
        this.orders = orders;
    }

    public double getTotalPrice() {
        double total = 0.0;
        for (Order order : orders) {
            total += order.getTotalPrice();
        }
        return total;
    }
}

public class Order {
    private List<Item> items;

    public Order(List<Item> items) {
        this.items = items;
    }

    public double getTotalPrice() {
        double total = 0.0;
        for (Item item : items) {
            total += item.getPrice();
        }
        return total;
    }
}

public class Item {
    private String name;
    private double price;

    public Item(String name, double price) {
        this.name = name;
        this.price = price;
    }

    public double getPrice() {
        return price;
    }
}

// Client code
Customer customer = new Customer("John Doe", Arrays.asList(
    new Order(Arrays.asList(new Item("Apple", 1.0), new Item("Orange", 1.5))),
    new Order(Arrays.asList(new Item("Banana", 0.5), new Item("Pineapple", 3.0)))
));
double total = 0.0;
total = customer.getTotalPrice();

```