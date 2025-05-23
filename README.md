class Book:
    def __init__(self, book_id, title, author):
        self.book_id = book_id
        self.title = title
        self.author = author
        self.is_issued = False

    def __str__(self):
        status = "Issued" if self.is_issued else "Available"
        return f"ID: {self.book_id}, Title: {self.title}, Author: {self.author}, Status: {status}"

class Member:
    def __init__(self, member_id, name):
        self.member_id = member_id
        self.name = name
        self.issued_books = []

    def __str__(self):
        return f"ID: {self.member_id}, Name: {self.name}, Books Issued: {len(self.issued_books)}"

class LibraryManagementSystem:
    def __init__(self):
        self.books = {}
        self.members = {}

    def add_book(self):
        book_id = input("Enter Book ID: ")
        if book_id in self.books:
            print("⚠️ Book ID already exists.")
            return
        title = input("Enter Book Title: ")
        author = input("Enter Author Name: ")
        self.books[book_id] = Book(book_id, title, author)
        print("✅ Book added successfully!")

    def add_member(self):
        member_id = input("Enter Member ID: ")
        if member_id in self.members:
            print("⚠️ Member ID already exists.")
            return
        name = input("Enter Member Name: ")
        self.members[member_id] = Member(member_id, name)
        print("✅ Member added successfully!")

    def issue_book(self):
        member_id = input("Enter Member ID: ")
        if member_id not in self.members:
            print("❌ Member not found.")
            return
        book_id = input("Enter Book ID to issue: ")
        if book_id not in self.books:
            print("❌ Book not found.")
            return
        book = self.books[book_id]
        if book.is_issued:
            print("⚠️ Book is already issued.")
            return
        book.is_issued = True
        self.members[member_id].issued_books.append(book)
        print(f"✅ Book '{book.title}' issued to {self.members[member_id].name}.")

    def return_book(self):
        member_id = input("Enter Member ID: ")
        if member_id not in self.members:
            print("❌ Member not found.")
            return
        book_id = input("Enter Book ID to return: ")
        member = self.members[member_id]
        for book in member.issued_books:
            if book.book_id == book_id:
                book.is_issued = False
                member.issued_books.remove(book)
                print(f"✅ Book '{book.title}' returned successfully.")
                return
        print("⚠️ This member did not issue this book.")

    def view_books(self):
        if not self.books:
            print("No books available.")
        else:
            print("\n--- Books in Library ---")
            for book in self.books.values():
                print(book)

    def view_members(self):
        if not self.members:
            print("No members registered.")
        else:
            print("\n--- Library Members ---")
            for member in self.members.values():
                print(member)

def main():
    system = LibraryManagementSystem()

    while True:
        print("\n--- Library Management System ---")
        print("1. Add Book")
        print("2. Add Member")
        print("3. Issue Book")
        print("4. Return Book")
        print("5. View All Books")
        print("6. View All Members")
        print("7. Exit")

        choice = input("Enter your choice: ")

        if choice == '1':
            system.add_book()
        elif choice == '2':
            system.add_member()
        elif choice == '3':
            system.issue_book()
        elif choice == '4':
            system.return_book()
        elif choice == '5':
            system.view_books()
        elif choice == '6':
            system.view_members()
        elif choice == '7':
            print("Exiting... Goodbye!")
            break
        else:
            print("Invalid choice. Please try again.")

if __name__ == "__main__":
    main()
