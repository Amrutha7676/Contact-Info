# Contact-Info
import csv

class Contact:
    def __init__(self, name, phone, email):
        self.name = name
        self.phone = phone
        self.email = email
    
    def __str__(self):
        return f"{self.name}, {self.phone}, {self.email}"

class ContactBook:
    def __init__(self, filename="contacts.csv"):
        self.filename = filename
        self.contacts = self.load_contacts()

    def load_contacts(self):
        """Load contacts from the CSV file."""
        contacts = []
        try:
            with open(self.filename, mode='r') as file:
                reader = csv.reader(file)
                for row in reader:
                    if row:  # Skip empty rows
                        contacts.append(Contact(row[0], row[1], row[2]))
        except FileNotFoundError:
            pass
        return contacts

    def save_contacts(self):
        """Save the contacts to the CSV file."""
        with open(self.filename, mode='w', newline='') as file:
            writer = csv.writer(file)
            for contact in self.contacts:
                writer.writerow([contact.name, contact.phone, contact.email])

    def add_contact(self, name, phone, email):
        """Add a new contact."""
        self.contacts.append(Contact(name, phone, email))
        self.save_contacts()
        print(f"Contact {name} added successfully.")

    def search_contact(self, name):
        """Search for a contact by name."""
        results = [contact for contact in self.contacts if name.lower() in contact.name.lower()]
        if results:
            print(f"Contacts found for '{name}':")
            for contact in results:
                print(contact)
        else:
            print(f"No contacts found for '{name}'.")

    def update_contact(self, name, phone=None, email=None):
        """Update a contact's information."""
        for contact in self.contacts:
            if contact.name.lower() == name.lower():
                if phone:
                    contact.phone = phone
                if email:
                    contact.email = email
                self.save_contacts()
                print(f"Contact {name} updated successfully.")
                return
        print(f"Contact '{name}' not found.")

    def delete_contact(self, name):
        """Delete a contact by name."""
        for contact in self.contacts:
            if contact.name.lower() == name.lower():
                self.contacts.remove(contact)
                self.save_contacts()
                print(f"Contact {name} deleted successfully.")
                return
        print(f"Contact '{name}' not found.")

    def display_all_contacts(self):
        """Display all contacts."""
        if self.contacts:
            print("All Contacts:")
            for contact in self.contacts:
                print(contact)
        else:
            print("No contacts available.")

def main():
    contact_book = ContactBook()
    
    while True:
        print("\n--- Contact Book ---")
        print("1. Add Contact")
        print("2. Search Contact")
        print("3. Update Contact")
        print("4. Delete Contact")
        print("5. Display All Contacts")
        print("6. Exit")
        
        choice = input("Choose an option: ")
        
        if choice == "1":
            name = input("Enter name: ")
            phone = input("Enter phone number: ")
            email = input("Enter email address: ")
            contact_book.add_contact(name, phone, email)
        
        elif choice == "2":
            name = input("Enter name to search: ")
            contact_book.search_contact(name)
        
        elif choice == "3":
            name = input("Enter name to update: ")
            phone = input("Enter new phone number (or press Enter to skip): ")
            email = input("Enter new email address (or press Enter to skip): ")
            contact_book.update_contact(name, phone if phone else None, email if email else None)
        
        elif choice == "4":
            name = input("Enter name to delete: ")
            contact_book.delete_contact(name)
        
        elif choice == "5":
            contact_book.display_all_contacts()
        
        elif choice == "6":
            print("Goodbye!")
            break
        else:
            print("Invalid option. Please try again.")

if __name__ == "__main__":
    main()
