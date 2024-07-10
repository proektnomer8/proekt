# proekt za online magazin
replit live demo: https://replit.com/@swx8gg0/ProektOnlineMagazin#main.py
кодът на задачата:


import uuid
from typing import List

class Product:
    def __init__(self, Name, Category, Price, StockQuantity, Description):
        self.ProductId = str(uuid.uuid4())
        self.Name = Name
        self.Category = Category
        self.Price = Price
        self.StockQuantity = StockQuantity
        self.Description = Description
        self.IsAvailable = StockQuantity > 0

    def __str__(self):
        return (f'Product Id: {self.ProductId}\n'
                f'Name: {self.Name}\n'
                f'Category: {self.Category}\n'
                f'Price: {self.Price:.2f}\n'
                f'StockQuantity: {self.StockQuantity}\n'
                f'Description: {self.Description}\n'
                f'IsAvailable: {"Yes" if self.IsAvailable else "No"}')

class Customer:
    def __init__(self, FullName, Email, PhoneNumber, ShippingAddress):
        self.CustomerId = str(uuid.uuid4())
        self.FullName = FullName
        self.Email = Email
        self.PhoneNumber = PhoneNumber
        self.ShippingAddress = ShippingAddress

    def __str__(self):
        return (f'Customer ID: {self.CustomerId}\n'
                f'Full Name: {self.FullName}\n'
                f'Email: {self.Email}\n'
                f'Phone Number: {self.PhoneNumber}\n'
                f'Shipping Address: {self.ShippingAddress}')

class Order:
    def __init__(self, CustomerId, OrderItems: List[Product], status="waiting"):
        self.OrderId = str(uuid.uuid4())
        self.CustomerId = CustomerId
        self.OrderDate = datetime.datetime.now()
        self.OrderItems = OrderItems
        self.TotalAmount = self.totalamount()
        self.status = status
    
    def totalamount(self):
        return sum(item.Price for item in self.OrderItems)

    def __str__(self):
        item_str = "\n".join([f' - {item.Name}: {item.Price}' for item in self.OrderItems])
        return (f'Order Id: {self.OrderId}\n'
                f'Customer Id: {self.CustomerId}\n'
                f'Order Date: {self.OrderDate}\n'
                f'Total Amount: {self.TotalAmount:.2f}\n'
                f'Status: {self.status}\n'
                f'Order Items:\n{item_str}')

class Payment:
    def __init__(self, OrderId, PaymentDate, Amount, PaymentMethod):
        self.PaymentId = str(uuid.uuid4())
        self.OrderId = OrderId
        self.PaymentDate = PaymentDate
        self.Amount = Amount
        self.PaymentMethod = PaymentMethod

    def __str__(self):
        return (f'Payment ID: {self.PaymentId}\n'
                f'Order Id: {self.OrderId}\n'
                f'Payment Date: {self.PaymentDate}\n'
                f'Amount: {self.Amount:.2f}\n'
                f'Payment Method: {self.PaymentMethod}')

class Category:
    def __init__(self, Name, Description, Products: List[Product]):
        self.CategoryId = str(uuid.uuid4())
        self.Name = Name
        self.Description = Description
        self.Products = Products

    def __str__(self):
        return (f'Category ID: {self.CategoryId}\n'
                f'Name: {self.Name}\n'
                f'Description: {self.Description}\n'
                f'Number of Products: {len(self.Products)}')

class OnlineStore:
    def __init__(self):
        self.products = []
        self.customers = []
        self.orders = []
        self.categories = []
        self.payments = []

    def AddProduct(self, product: Product):
        self.products.append(product)
        print(f"Product '{product.Name}' added successfully!")

    def RemoveProduct(self, productId: str):
        for p in self.products:
            if p.ProductId == productId:
                self.products.remove(p)
                print(f"Product with ID '{productId}' removed successfully!")
                return
        print(f"Product with ID '{productId}' not found.")

    def SearchProductByName(self, name: str):
        found_products = [p for p in self.products if name.lower() in p.Name.lower()]
        if found_products:
            print(f"Found {len(found_products)} product(s) with name '{name}':")
            for prod in found_products:
                print(prod)
        else:
            print(f"No products found with name '{name}'.")

    def ListAllProducts(self):
        if self.products:
            print("All Products:")
            for prod in self.products:
                print(prod)
        else:
            print("No products in the store.")

    def AddCustomer(self, customer: Customer):
        self.customers.append(customer)
        print(f"Customer '{customer.FullName}' added successfully!")

    def RemoveCustomer(self, CustomerId: str):
        for c in self.customers:
            if c.CustomerId == CustomerId:
                self.customers.remove(c)
                print(f"Customer with ID '{CustomerId}' removed successfully!")
                return
        print(f"Customer with ID '{CustomerId}' not found.")

    def SearchCustomerByName(self, name: str):
        found_customers = [c for c in self.customers if name.lower() in c.FullName.lower()]
        if found_customers:
            print(f"Found {len(found_customers)} customer(s) with name '{name}':")
            for cust in found_customers:
                print(cust)
        else:
            print(f"No customers found with name '{name}'.")

    def PlaceOrder(self, order: Order):
        self.orders.append(order)
        print(f"Order placed successfully! Order ID: {order.OrderId}")

    def UpdateOrderStatus(self, orderId: str, status: str):
        for order in self.orders:
            if order.OrderId == orderId:
                order.status = status
                print(f"Order status updated successfully! New status: {status}")
                return
        print(f"Order with ID '{orderId}' not found.")

    def AddCategory(self, category: Category):
        self.categories.append(category)
        print(f"Category '{category.Name}' added successfully!")

    def RemoveCategory(self, categoryId: str):
        for cat in self.categories:
            if cat.CategoryId == categoryId:
                self.categories.remove(cat)
                print(f"Category with ID '{categoryId}' removed successfully!")
                return
        print(f"Category with ID '{categoryId}' not found.")

    def ListAllCategories(self):
        if self.categories:
            print("All Categories:")
            for cat in self.categories:
                print(f"Category ID: {cat.CategoryId}, Name: {cat.Name}")
        else:
            print("No categories in the store.")

    def ProcessPayment(self, payment: Payment):
        self.payments.append(payment)
        print(f"Payment processed successfully! Payment ID: {payment.PaymentId}")

    def GenerateSalesReport(self):
        report = "Sales Report\n"
        report += "=" * 20 + "\n"
        total_sales = 0
        for order in self.orders:
            report += f"Order ID: {order.OrderId}\n"
            report += f"Customer ID: {order.CustomerId}\n"
            report += f"Order Date: {order.OrderDate}\n"
            report += f"Total Amount: {order.TotalAmount:.2f}\n"
            report += "-" * 20 + "\n"
            total_sales += order.TotalAmount
        report += f"\nTotal Sales: {total_sales:.2f}\n"
        return report

    def GenerateCustomerReport(self):
        report = "Customer Report\n"
        report += "=" * 20 + "\n"
        for customer in self.customers:
            report += f"Customer ID: {customer.CustomerId}\n"
            report += f"Full Name: {customer.FullName}\n"
            report += f"Email: {customer.Email}\n"
            report += f"Phone Number: {customer.PhoneNumber}\n"
            report += f"Shipping Address: {customer.ShippingAddress}\n"
            report += "-" * 20 + "\n"
        return report

if __name__ == "__main__":
    store = OnlineStore()

    while True:
        print("\nWelcome to the Online Shoe Store!")
        print("Please select an option:")
        print("1. Add Product")
        print("2. Remove Product")
        print("3. Search Product by Name")
        print("4. List All Products")
        print("5. Add Customer")
        print("6. Remove Customer")
        print("7. Search Customer by Name")
        print("8. Place Order")
        print("9. Update Order Status")
        print("10. Add Category")
        print("11. Remove Category")
        print("12. List All Categories")
        print("13. Process Payment")
        print("14. Generate Sales Report")
        print("15. Generate Customer Report")
        print("16. Exit")

        choice = input("Enter your choice (1-16): ")

        if choice == '1': 
            name = input("Enter product name: ")
            category = input("Enter product category: ")
            price = float(input("Enter product price: "))
            stock = int(input("Enter stock quantity: "))
            description = input("Enter product description: ")
            product = Product(Name=name, Category=category, Price=price, StockQuantity=stock, Description=description)
            store.AddProduct(product)

        elif choice == '2':  
            productId = input("Enter product ID to remove: ")
            store.RemoveProduct(productId)

        elif choice == '3':  
            name = input("Enter product name to search: ")
            store.SearchProductByName(name)

        elif choice == '4':  
            store.ListAllProducts()

        elif choice == '5':  
            name = input("Enter customer full name: ")
            email = input("Enter customer email: ")
            phone = input("Enter customer phone number: ")
            address = input("Enter customer shipping address: ")
            customer = Customer(FullName=name, Email=email, PhoneNumber=phone, ShippingAddress=address)
            store.AddCustomer(customer)

        elif choice == '6':  
            customerId = input("Enter customer ID to remove: ")
            store.RemoveCustomer(customerId)

        elif choice == '7':  
            name = input("Enter customer name to search: ")
            store.SearchCustomerByName(name)

        elif choice == '8':  
            customerId = input("Enter customer ID placing the order: ")
            products = []
            while True:
                productId = input("Enter product ID to add to the order (or 'done' to finish): ")
                if productId.lower() == 'done':
                    break
                else:
                    product = next((p for p in store.products if p.ProductId == productId), None)
                    if product:
                        products.append(product)
                    else:
                        print(f"Product with ID '{productId}' not found.")
            if products:
                order = Order(CustomerId=customerId, OrderItems=products)
                store.PlaceOrder(order)

        elif choice == '9':  
            orderId = input("Enter order ID to update status: ")
            status = input("Enter new status for the order: ")
            store.UpdateOrderStatus(orderId, status)

        elif choice == '10':  
            name = input("Enter category name: ")
            description = input("Enter category description: ")
            category = Category(Name=name, Description=description, Products=[])
            store.AddCategory(category)

        elif choice == '11':  
            categoryId = input("Enter category ID to remove: ")
            store.RemoveCategory(categoryId)

        elif choice == '12':  
            store.ListAllCategories()

        elif choice == '13':  
            orderId = input("Enter order ID for payment: ")
            order = next((o for o in store.orders if o.OrderId == orderId), None)
            if order:
                payment_amount = order.TotalAmount
                payment_method = input("Enter payment method: ")
                payment = Payment(OrderId=orderId, PaymentDate=datetime.datetime.now(), Amount=payment_amount, PaymentMethod=payment_method)
                store.ProcessPayment(payment)
            else:
                print(f"Order with ID '{orderId}' not found.")

        elif choice == '14':  
            report = store.GenerateSalesReport()
            print(report)

        elif choice == '15':  
            report = store.GenerateCustomerReport()
            print(report)

        elif choice == '16':  
            print("Goodbye!")
            break

        else:
            print("Please enter a number from 1 to 16.")
     
