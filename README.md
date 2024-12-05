# oop8

### Customer Class
```java
class Customer {
    private final String name;
    private final String address;

    public Customer(String name, String address) {
        this.name = name;
        this.address = address;
    }

    public String getName() {
        return name;
    }

    public String getAddress() {
        return address;
    }
}
```

### Item Class

```java
package oop8;

class Item {
    private final String shippingWeight;
    private final String description;
    private final float price;
    private final float taxRate;

    public Item(String shippingWeight, String description, float price, float taxRate) {
        this.shippingWeight = shippingWeight;
        this.description = description;
        this.price = price;
        this.taxRate = taxRate;
    }

    public float getPriceForQuantity(int quantity) {
        return price * quantity;
    }

    public float getTax() {
        return price * taxRate;
    }

    public boolean inStock() {
        return true;
    }

    public String getShippingWeight() {
        return shippingWeight;
    }

    public String getDescription() {
        return description;
    }
}
```

### Order Class

```java
package oop8;

class Order {
    private final String date;
    private final String status;
    private final Customer customer;
    private final OrderDetail[] orderDetails;

    public Order(String date, String status, Customer customer, OrderDetail[] orderDetails) {
        this.date = date;
        this.status = status;
        this.customer = customer;
        this.orderDetails = orderDetails;
    }

    public float calcSubTotal() {
        float subTotal = 0;
        for (OrderDetail detail : orderDetails) {
            subTotal += detail.calcSubTotal();
        }
        return subTotal;
    }

    public float calcTax() {
        float tax = 0;
        for (OrderDetail detail : orderDetails) {
            tax += detail.calcTax();
        }
        return tax;
    }

    public float calcTotal() {
        return calcSubTotal() + calcTax();
    }

    public float calcTotalWeight() {
        float totalWeight = 0;
        for (OrderDetail detail : orderDetails) {
            totalWeight += detail.calcWeight();
        }
        return totalWeight;
    }

    public String getDate() {
        return date;
    }

    public String getStatus() {
        return status;
    }

    public Customer getCustomer() {
        return customer;
    }

    public OrderDetail[] getOrderDetails() {
        return orderDetails;
    }
}
```


### Order Detail Class

```java
package oop8;

class OrderDetail {
    private final int quantity;
    private final String taxStatus;
    private final Item item;

    public OrderDetail(int quantity, String taxStatus, Item item) {
        this.quantity = quantity;
        this.taxStatus = taxStatus;
        this.item = item;
    }

    public float calcSubTotal() {
        return item.getPriceForQuantity(quantity);
    }

    public float calcWeight() {
        return Float.parseFloat(item.getShippingWeight()) * quantity;
    }

    public float calcTax() {
        return item.getTax();
    }

    public int getQuantity() {
        return quantity;
    }

    public String getTaxStatus() {
        return taxStatus;
    }

    public Item getItem() {
        return item;
    }
}
```

### Payment Class

```java
package oop8;

abstract class Payment {
    protected float amount;

    public Payment(float amount) {
        this.amount = amount;
    }

    public float getAmount() {
        return amount;
    }
}
```

### Main CLass

```java
package oop8;

public class Main {
    public static void main(String[] args) {
        Item item1 = new Item("1.5", "Book", 195000.0f, 0.05f);
        Item item2 = new Item("0.5", "Lightstick", 7800000.0f, 0.05f);
        OrderDetail detail1 = new OrderDetail(2, "Taxable", item1);
        OrderDetail detail2 = new OrderDetail(1, "Taxable", item2);

        Customer customer = new Customer("Kim dan", "South Korea");
        
        Order order = new Order("2024-12-05", "Processing", customer, new OrderDetail[]{detail1, detail2});

        System.out.println("Customer: " + order.getCustomer().getName());
        System.out.println("Order Date: " + order.getDate());
        System.out.println("Order Status: " + order.getStatus());

        System.out.println();

        for (OrderDetail detail : order.getOrderDetails()) {
            Item item = detail.getItem();
            System.out.println("Item: " + item.getDescription());
            System.out.println("Quantity: " + detail.getQuantity());
            System.out.println("Price per item: " + String.format("%.0f", item.getPriceForQuantity(1)));
            System.out.println("SubTotal: " + String.format("%.0f", detail.calcSubTotal())); 
        }

        System.out.println();

        System.out.println("Order SubTotal: " + String.format("%.0f", order.calcSubTotal())); 
        System.out.println("Order Tax: " + String.format("%.0f", order.calcTax()));
        System.out.println("Order Total: " + String.format("%.0f", order.calcTotal())); 
        System.out.println("Total Weight: " + String.format("%.2f", order.calcTotalWeight()) + " lbs"); 
    }
}
```

### output
![Cuplikan layar 2024-12-05 080116](https://github.com/user-attachments/assets/c5a98bd8-890a-4edb-8f2e-ac9dfff494a9)
