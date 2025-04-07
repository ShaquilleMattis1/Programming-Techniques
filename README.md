# Programming-Techniques
import datetime

class POS:
    def __init__(self):
        self.products = {
            "Milk": {"price": 150, "stock": 10},
            "Bread": {"price": 120, "stock": 8},
            "Eggs": {"price": 200, "stock": 12},
            "Rice": {"price": 500, "stock": 15},
            "Sugar": {"price": 300, "stock": 6},
            "Salt": {"price": 50, "stock": 20},
            "Soap": {"price": 250, "stock": 7},
            "Toothpaste": {"price": 450, "stock": 5},
            "Juice": {"price": 600, "stock": 9},
            "Cereal": {"price": 700, "stock": 4}
        }
        self.cart = {}

    def display_products(self):
        print("\nAvailable Products:")
        for item, details in self.products.items():
            print(f"{item}: ${details['price']} | Stock: {details['stock']}")

    def add_to_cart(self):
        while True:
            product = input("Enter product name (or type 'done' to finish): ").title()
            if product.lower() == 'done':
                break
            if product in self.products:
                quantity = int(input("Enter quantity: "))
                if quantity > 0:
                    if quantity <= self.products[product]["stock"]:
                        if product in self.cart:
                            self.cart[product]["quantity"] += quantity
                        else:
                            self.cart[product] = {"price": self.products[product]["price"], "quantity": quantity}
                        self.products[product]["stock"] -= quantity
                        print(f"{quantity} {product}(s) added to cart.")
                    else:
                        print("Insufficient stock!")
                else:
                    print("Invalid quantity!")
            else:
                print("Product not found!")

    def remove_from_cart(self):
        product = input("Enter product name to remove: ").title()
        if product in self.cart:
            quantity = int(input("Enter quantity to remove: "))
            if quantity > 0:
                if quantity >= self.cart[product]["quantity"]:
                    self.products[product]["stock"] += self.cart[product]["quantity"]
                    del self.cart[product]
                else:
                    self.cart[product]["quantity"] -= quantity
                    self.products[product]["stock"] += quantity
                print(f"{quantity} {product}(s) removed from cart.")
            else:
                print("Invalid quantity!")
        else:
            print("Product not in cart!")

    def view_cart(self):
        if self.cart:
            print("\nShopping Cart:")
            total = 0
            for item, details in self.cart.items():
                cost = details["price"] * details["quantity"]
                print(f"{item} - {details['quantity']} x ${details['price']} = ${cost}")
                total += cost
            print(f"Subtotal: ${total}")
        else:
            print("Cart is empty!")

    def checkout(self):
        if not self.cart:
            print("Cart is empty!")
            return

        subtotal = sum(details['price'] * details['quantity'] for details in self.cart.values())
        tax = round(subtotal * 0.10, 2)
        discount = round(subtotal * 0.05, 2) if subtotal > 5000 else 0
        total = subtotal + tax - discount

        print(f"\nSubtotal: ${subtotal}")
        print(f"Sales Tax (10%): ${tax}")
        if discount:
            print(f"Discount (5% on >$5000): -${discount}")
        print(f"Total Amount Due: ${total}")

        while True:
            payment = float(input("Enter payment amount: "))
            if payment >= total:
                change = payment - total
                print(f"Change: ${change}")
                self.generate_receipt(subtotal, tax, discount, total, payment, change)
                self.cart.clear()
                break
            else:
                print("Insufficient payment. Try again!")

    def generate_receipt(self, subtotal, tax, discount, total, payment, change):
        print("\n--- Best Buy Retail Store ---")
        print(f"Date: {datetime.datetime.now().strftime('%Y-%m-%d %H:%M:%S')}")
        print("--------------------------------")
        for item, details in self.cart.items():
            print(f"{item} - {details['quantity']} x ${details['price']} = ${details['quantity'] * details['price']}")
        print("--------------------------------")
        print(f"Subtotal: ${subtotal}")
        print(f"Sales Tax (10%): ${tax}")
        if discount:
            print(f"Discount (5%): -${discount}")
        print(f"Total Due: ${total}")
        print(f"Amount Paid: ${payment}")
        print(f"Change: ${change}")
        print("Thank you for shopping with us!")
        print("--------------------------------\n")

    def run(self):
        while True:
            print("\n--- Point of Sale System ---")
            print("1. View Products")
            print("2. Add to Cart")
            print("3. Remove from Cart")
            print("4. View Cart")
            print("5. Checkout")
            print("6. Exit")
            
            choice = input("Enter your choice: ")
            
            if choice == "1":
                self.display_products()
            elif choice == "2":
                self.add_to_cart()
            elif choice == "3":
                self.remove_from_cart()
            elif choice == "4":
                self.view_cart()
            elif choice == "5":
                self.checkout()
            elif choice == "6":
                print("Exiting system. Goodbye!")
                break
            else:
                print("Invalid choice! Please try again.")

if __name__ == "__main__":
    pos_system = POS()
    pos_system.run()
