class Contact:
    """Represents a single contact with name, phone, email, and address."""
    def __init__(self, name, phone, email, address):
        self.name = name
        self.phone = phone
        self.email = email
        self.address = address

    def __str__(self):
        """Returns a string representation of the contact."""
        return (f"Name: {self.name}\nPhone: {self.phone}\n"
                f"Email: {self.email}\nAddress: {self.address}\n")

class ContactBook:
    """Manages a collection of contacts."""
    def __init__(self):
        self.contacts = []

    def add_contact(self):
        """Prompts the user for contact details and adds a new contact."""
        print("\n--- Add New Contact ---")
        name = input("Enter Name: ").strip()
        if not name:
            print("Name cannot be empty. Contact not added.")
            return

        # Check for duplicate names
        if any(contact.name.lower() == name.lower() for contact in self.contacts):
            print(f"A contact with the name '{name}' already exists. Please use a unique name.")
            return

        phone = input("Enter Phone Number: ").strip()
        email = input("Enter Email Address: ").strip()
        address = input("Enter Address: ").strip()

        new_contact = Contact(name, phone, email, address)
        self.contacts.append(new_contact)
        print(f"Contact '{name}' added successfully!")

    def view_contact_list(self):
        """Displays a list of all saved contacts with names and phone numbers."""
        if not self.contacts:
            print("\nNo contacts to display.")
            return

        print("\n--- Your Contact List ---")
        for i, contact in enumerate(self.contacts):
            print(f"{i+1}. Name: {contact.name}, Phone: {contact.phone}")

    def search_contact(self):
        """Implements a search function to find contacts by name or phone number."""
        if not self.contacts:
            print("\nNo contacts to search.")
            return

        search_term = input("\nEnter name or phone number to search: ").strip().lower()
        found_contacts = []
        for contact in self.contacts:
            if search_term in contact.name.lower() or search_term in contact.phone.lower():
                found_contacts.append(contact)

        if found_contacts:
            print("\n--- Search Results ---")
            for contact in found_contacts:
                print(contact)
        else:
            print(f"No contacts found matching '{search_term}'.")

    def update_contact(self):
        """Enables users to update contact details."""
        if not self.contacts:
            print("\nNo contacts to update.")
            return

        self.view_contact_list()
        try:
            contact_index = int(input("Enter the number of the contact to update: ")) - 1
            if 0 <= contact_index < len(self.contacts):
                contact_to_update = self.contacts[contact_index]
                print(f"\n--- Updating Contact: {contact_to_update.name} ---")
                
                new_name = input(f"Enter new Name (current: {contact_to_update.name}): ").strip()
                if new_name:
                    # Check for duplicate names if updating the name
                    if new_name.lower() != contact_to_update.name.lower() and any(c.name.lower() == new_name.lower() for c in self.contacts):
                        print(f"A contact with the name '{new_name}' already exists. Update cancelled for name.")
                    else:
                        contact_to_update.name = new_name

                new_phone = input(f"Enter new Phone Number (current: {contact_to_update.phone}): ").strip()
                if new_phone:
                    contact_to_update.phone = new_phone
                
                new_email = input(f"Enter new Email Address (current: {contact_to_update.email}): ").strip()
                if new_email:
                    contact_to_update.email = new_email
                
                new_address = input(f"Enter new Address (current: {contact_to_update.address}): ").strip()
                if new_address:
                    contact_to_update.address = new_address
                
                print(f"Contact '{contact_to_update.name}' updated successfully!")
            else:
                print("Invalid contact number.")
        except ValueError:
            print("Invalid input. Please enter a number.")

    def delete_contact(self):
        """Provides an option to delete a contact."""
        if not self.contacts:
            print("\nNo contacts to delete.")
            return

        self.view_contact_list()
        try:
            contact_index = int(input("Enter the number of the contact to delete: ")) - 1
            if 0 <= contact_index < len(self.contacts):
                deleted_contact = self.contacts.pop(contact_index)
                print(f"Contact '{deleted_contact.name}' deleted successfully!")
            else:
                print("Invalid contact number.")
        except ValueError:
            print("Invalid input. Please enter a number.")

def display_menu():
    """Displays the main menu options to the user."""
    print("\n--- Contact Book Menu ---")
    print("1. Add Contact")
    print("2. View Contact List")
    print("3. Search Contact")
    print("4. Update Contact")
    print("5. Delete Contact")
    print("6. Exit")

def main():
    """Main function to run the Contact Book application."""
    contact_book = ContactBook()

    while True:
        display_menu()
        choice = input("Enter your choice: ").strip()

        if choice == '1':
            contact_book.add_contact()
        elif choice == '2':
            contact_book.view_contact_list()
        elif choice == '3':
            contact_book.search_contact()
        elif choice == '4':
            contact_book.update_contact()
        elif choice == '5':
            contact_book.delete_contact()
        elif choice == '6':
            print("Exiting Contact Book. Goodbye!")
            break
        else:
            print("Invalid choice. Please enter a number between 1 and 6.")

if __name__ == "__main__":
    main()
