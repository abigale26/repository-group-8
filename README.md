# repository-group-8import uuid

class InventoryManager:
    def __init__(self):
        self.inventory = {}  # Use a dictionary as a hash map

    def add_item(self, label, brand, quantity):
        """Adds a new item to the inventory."""
        item_id = str(uuid.uuid4())  # Generate a unique ID using UUID
        self.inventory[item_id] = {"label": label, "brand": brand, "quantity": quantity}
        return item_id

    def delete_item(self, item_id):
        """Deletes an item from the inventory."""
        if item_id in self.inventory:
            del self.inventory[item_id]
            return True
        else:
            return False

    def sort_inventory(self, criteria):
        """Sorts the inventory based on the specified criteria."""
        if not self.inventory:
            return [] #Handle empty inventory

        items = list(self.inventory.values())
        items.sort(key=lambda item: item[criteria]) #Sort by specified criteria.  Will fail if criteria is not a key.
        self.inventory = {str(uuid.uuid4()):item for item in items} #Repopulate with new IDs to avoid key collisions after sort.


    def search_inventory(self, criteria, value):
        """Searches the inventory based on the specified criteria."""
        results = []
        for item_id, item_details in self.inventory.items():
            if criteria in item_details and item_details[criteria] == value:
                results.append((item_id, item_details))
        return results

    def display_inventory(self):
        """Displays the entire inventory."""
        if not self.inventory:
            print("Inventory is empty.")
            return

        print("Current Inventory:")
        for item_id, item_details in self.inventory.items():
            print(f"ID: {item_id}, Label: {item_details['label']}, Brand: {item_details['brand']}, Quantity: {item_details['quantity']}")


# Example usage
inventory = InventoryManager()

# Add items
item1_id = inventory.add_item("Laptop", "Dell", 10)
item2_id = inventory.add_item("Keyboard", "Logitech", 20)
item3_id = inventory.add_item("Mouse", "Logitech", 15)

inventory.display_inventory()

# Delete an item
inventory.delete_item(item1_id)

# Sort inventory by brand
inventory.sort_inventory("brand")
inventory.display_inventory()


# Search for items of a specific brand
results = inventory.search_inventory("brand", "Logitech")
print("\nSearch results:")
for item_id, item_details in results:
    print(f"ID: {item_id}, Label: {item_details['label']}, Brand: {item_details['brand']}, Quantity: {item_details['quantity']}")
