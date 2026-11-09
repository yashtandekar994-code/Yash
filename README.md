# Inventory management system
inventory = []
next_id = 1101


def add_item():
    global next_id

    name = input("Enter item name: ").strip()
    if not name:
        print("Invalid item name.")
        return

    try:
        quantity = int(input("Enter quantity: "))
        price = float(input("Enter price: "))

        if quantity < 0:
            print("Invalid quantity. Quantity must be a non-negative whole number.")
            print("Item not added.")
            return

        if price < 0:
            print("Invalid price. Price must be non-negative.")
            print("Item not added.")
            return

    except ValueError:
        print("Invalid input. Enter a whole number for quantity and a number for price.")
        print("Item not added.")
        return

    item = {
        "id": next_id,
        "name": name,
        "quantity": quantity,
        "price": price
    }

    inventory.append(item)

    print("Item added.")
    print(
        f"ID: {item['id']} | {item['name']} | "
        f"Qty: {item['quantity']} | Price: {item['price']:g}"
    )

    if quantity < 5:
        print(f"Low stock alert: {name} ({quantity})")

    next_id += 1


def view_items():
    if not inventory:
        print("Inventory is empty.")
        return

    print("\n--- Inventory Items ---")
    print(f"{'ID':<8}{'Name':<20}{'Quantity':<12}{'Price':<10}")

    for item in inventory:
        print(
            f"{item['id']:<8}{item['name']:<20}"
            f"{item['quantity']:<12}{item['price']:<10g}"
        )


def search_item():
    try:
        item_id = int(input("Enter item ID to search: "))
    except ValueError:
        print("Invalid item ID.")
        return

    for item in inventory:
        if item["id"] == item_id:
            print(
                f"ID: {item['id']} | {item['name']} | "
                f"Qty: {item['quantity']} | Price: {item['price']:g}"
            )
            return

    print(f"Item ID {item_id} not found.")


def update_item():
    try:
        item_id = int(input("Enter item ID to update: "))
    except ValueError:
        print("Invalid item ID.")
        return

    for item in inventory:
        if item["id"] == item_id:
            print("Leave a field blank to keep its current value.")

            name = input(
                f"Enter new name [{item['name']}]: "
            ).strip()

            quantity_input = input(
                f"Enter new quantity [{item['quantity']}]: "
            ).strip()

            price_input = input(
                f"Enter new price [{item['price']:g}]: "
            ).strip()

            try:
                new_quantity = (
                    int(quantity_input)
                    if quantity_input else item["quantity"]
                )

                new_price = (
                    float(price_input)
                    if price_input else item["price"]
                )

                if new_quantity < 0:
                    print("Invalid quantity. Item not updated.")
                    return

                if new_price < 0:
                    print("Invalid price. Item not updated.")
                    return

            except ValueError:
                print("Invalid input. Item not updated.")
                return

            if name:
                item["name"] = name

            item["quantity"] = new_quantity
            item["price"] = new_price

            print("Item updated successfully.")

            if item["quantity"] < 5:
                print(
                    f"Low stock alert: "
                    f"{item['name']} ({item['quantity']})"
                )
            return

    print(f"Item ID {item_id} not found.")


def delete_item():
    try:
        item_id = int(input("Enter item ID to delete: "))
    except ValueError:
        print("Invalid item ID.")
        return

    for item in inventory:
        if item["id"] == item_id:
            deleted_name = item["name"]
            inventory.remove(item)

            print(
                f"Item '{deleted_name}' deleted. "
                f"Items left: {len(inventory)}"
            )
            return

    print(f"Item ID {item_id} not found.")


def total_stock_value():
    total = sum(
        item["quantity"] * item["price"]
        for item in inventory
    )

    print(f"Total stock value: {total:g}")


def low_stock_report():
    low_stock_items = [
        item for item in inventory
        if item["quantity"] < 5
    ]

    print("\n--- Low Stock Report (Quantity below 5) ---")

    if not low_stock_items:
        print("No low-stock items.")
        return

    for item in low_stock_items:
        print(
            f"ID: {item['id']} | {item['name']} | "
            f"Qty: {item['quantity']}"
        )


def main():
    while True:
        print("\n===== INVENTORY MANAGEMENT SYSTEM =====")
        print("1. Add Item")
        print("2. View Items")
        print("3. Update Item")
        print("4. Delete Item")
        print("5. Total Stock Value")
        print("6. Low Stock Report")
        print("7. Search Item")
        print("8. Exit")

        choice = input("Enter your choice (1-8): ").strip()

        if choice == "1":
            add_item()
        elif choice == "2":
            view_items()
        elif choice == "3":
            update_item()
        elif choice == "4":
            delete_item()
        elif choice == "5":
            total_stock_value()
        elif choice == "6":
            low_stock_report()
        elif choice == "7":
            search_item()
        elif choice == "8":
            print("Thank you for using the Inventory Management System.")
            break
        else:
            print("Invalid choice. Please select 1 to 8.")


if __name__ == "__main__":
    main()
