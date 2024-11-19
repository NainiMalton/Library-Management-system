Library management system

#include <iostream>
#include<iomanip>
#include <sstream>
#include <vector>
#include <string>
#include <ctime>
#include<cmath>
#include <fstream>
using namespace std;

// Book class
class Book {
private:
    int bookID;
    string title;
    string author;
    string genre;
    string status;

public:
    Book(int b, string t, string a, string g, string s) : bookID(b), title(t), author(a), genre(g), status(s) {}

   int getBookID() const { return bookID; }
    string getTitle() const { return title; }
    string getAuthor() const { return author; }
    string getGenre() const { return genre; }
    string getStatus() const { return status; }

   void setTitle(const string &t) { title = t; }
    void setAuthor(const string &a) { author = a; }
    void setGenre(const string &g) { genre = g; }
    void setStatus(const string &s) { status = s; }

  void displayDetails() const {
        cout << "BookID: " << bookID << "\nTitle: " << title << "\nAuthor: " << author
             << "\nGenre: " << genre << "\nStatus: " << status << endl;
    }

   // Serialization
    string toFile() const {
        return to_string(bookID) + "|" + title + "|" + author + "|" + genre + "|" + status + "\n";
    }

  static Book fromFile(const string &data) {
        int bID;
        string t, a, g, s;
        sscanf(data.c_str(), "%d|%[^|]|%[^|]|%[^|]|%s", &bID, t.c_str(), a.c_str(), g.c_str(), s.c_str());
        return Book(bID, t, a, g, s);
    }
};

// Member class
class Member {
private:
    int memberID;
    string name;
    string address;
    int phoneNumber;

public:
    Member(int m, string n, string a, int p) : memberID(m), name(n), address(a), phoneNumber(p) {}

  int getMemberID() const { return memberID; }
    string getName() const { return name; }
    string getAddress() const { return address; }
    int getPhoneNumber() const { return phoneNumber; }

   void setName(const string &n) { name = n; }
    void setAddress(const string &a) { address = a; }
    void setPhoneNumber(int p) { phoneNumber = p; }

   void displayMember() const {
        cout << "MemberID: " << memberID << "\nName: " << name << "\nAddress: " << address
             << "\nPhoneNumber: " << phoneNumber << endl;
    }

   // Serialization
    string toFile() const {
        return to_string(memberID) + "|" + name + "|" + address + "|" + to_string(phoneNumber) + "\n";
    }
};
class Transaction {
private:
    int transactionID;
    int memberID;
    int bookID;
    string borrowDate;
    string returnDate;

public:
    Transaction(int tID, int mID, int bID, const string &bDate, const string &rDate)
        : transactionID(tID), memberID(mID), bookID(bID), borrowDate(bDate), returnDate(rDate) {}

  int getTransactionID() const { return transactionID; }
    int getMemberID() const { return memberID; }
    int getBookID() const { return bookID; }
    string getBorrowDate() const { return borrowDate; }
    string getReturnDate() const { return returnDate; }

  void setReturnDate(const string &rDate) { returnDate = rDate; }

   void displayTransaction() const {
        cout << "TransactionID: " << transactionID << "\nMemberID: " << memberID << "\nBookID: " << bookID
             << "\nBorrowDate: " << borrowDate << "\nReturnDate: " << returnDate << endl;
    }

   // Serialization
    string toFile() const {
        return to_string(transactionID) + "|" + to_string(memberID) + "|" + to_string(bookID) + "|" + borrowDate + "|" +
               returnDate + "\n";
    }
};

// Library class
class Library {
private:
    vector<Book> books;
    vector<Member> members;
    vector<Transaction> transactions;


  void saveBooksToFile() {
        ofstream file("books.txt");
        for (const auto &book : books) file << book.toFile();
        file.close();
    }

  void saveMembersToFile() {
        ofstream file("members.txt");
        for (const auto &member : members) file << member.toFile();
        file.close();
    }
  void saveTransactionsToFile() {
        ofstream file("transactions.txt");
        for (const auto &transaction : transactions) file << transaction.toFile();
        file.close();
    }

public:
    void addBook(int bookID, const string &title, const string &author, const string &genre, const string &status) {
        books.emplace_back(bookID, title, author, genre, status);
        saveBooksToFile();
    }

   void displayDetails() const {
        for (const auto &book : books) {
            book.displayDetails();
            cout << "----------------------\n";
        }
    }

  void editBook(int bookID) {
        for (auto &book : books) {
            if (book.getBookID() == bookID) {
                string title, author, genre, status;
                cout << "Enter new title: ";
                getline(cin, title);
                cout << "Enter new author: ";
                getline(cin, author);
                cout << "Enter new genre: ";
                getline(cin, genre);
                cout << "Enter new status: ";
                getline(cin, status);
                book.setTitle(title);
                book.setAuthor(author);
                book.setGenre(genre);
                book.setStatus(status);
                saveBooksToFile();
                return;
            }
        }
        cout << "Book not found.\n";
    }
void deleteBook(int bookID)
{
        for (auto i = books.begin(); i != books.end(); ++i) {
            if (i->getBookID() == bookID) {
                books.erase(i);
                saveBooksToFile();
                cout << "Book deleted successfully.\n";
                return;
            }
        }
        cout << "Book not found.\n";
}


   void addMember(int memberID, const string &name, const string &address, int phoneNumber) {
        members.emplace_back(memberID, name, address, phoneNumber);
        saveMembersToFile();
    }

  void displayMembers() const {
        for (const auto &member : members) {
            member.displayMember();
            cout << "----------------------\n";
        }
    }

  void editMember(int memberID) {
        for (auto &member : members) {
            if (member.getMemberID() == memberID) {
                string name, address;
                int phoneNumber;
                cout << "Enter new name: ";
                getline(cin, name);
                cout << "Enter new address: ";
                getline(cin, address);
                cout << "Enter new phone number: ";
                cin >> phoneNumber;
                cin.ignore();
                member.setName(name);
                member.setAddress(address);
                member.setPhoneNumber(phoneNumber);
                saveMembersToFile();
                return;
            }
        }
        cout << "Member not found.\n";
    }

  void deleteMember(int memberID) {
        for (auto i = members.begin(); i != members.end(); ++i) {
            if (i->getMemberID() == memberID) {
                members.erase(i);
                saveMembersToFile();
                cout << "Member deleted successfully.\n";
                return;
            }
        }
        cout << "Member not found.\n";
    }
  void borrowBook(int transactionID, int memberID, int bookID, const string &borrowDate) {
        for (auto &book : books) {
            if (book.getBookID() == bookID) {
                if (book.getStatus() == "Available") {
                    book.setStatus("Borrowed");
                    transactions.emplace_back(transactionID, memberID, bookID, borrowDate, "Not returned");
                    saveTransactionsToFile();
                    cout << "Book borrowed successfully.\n";
                    return;
                } else {
                    cout << "Book is already borrowed.\n";
                    return;
                }
            }
        }
        cout << "Book not found.\n";
    }

void returnBook(int transactionID, const string &returnDate) {
        for (auto &transaction : transactions) {
            if (transaction.getTransactionID() == transactionID && transaction.getReturnDate() == "Not returned") {
                for (auto &book : books) {
                    if (book.getBookID() == transaction.getBookID()) {
                        book.setStatus("Available");
                        transaction.setReturnDate(returnDate);
                        saveTransactionsToFile();
                        cout << "Book returned successfully.\n";
                        return;
                    }
                }
            }
        }
        cout << "Transaction not found or already returned.\n";
    }

void displayTransactions() const
     {
        for (const auto &transaction : transactions) {
            transaction.displayTransaction();
            cout << "----------------------\n";
        }
    }
// Search functionality for books and members
Book* searchBookByID(int bookID)
    {
        for (auto &book : books)
            { if (book.getBookID() == bookID) { return &book; }
        } return nullptr;
    }
vector<Book> searchBooksByTitle(const string &title)
    { vector<Book> foundBooks;
    for (auto &book : books)
    {
            if (book.getTitle().find(title) != string::npos)
            { foundBooks.push_back(book); }
    }
    return foundBooks;
    }
    Member* searchMemberByID(int memberID)
    {
        for (auto &member : members)
        {
                if (member.getMemberID() == memberID)
                { return &member; }
        }
        return nullptr;
    }
vector<Member> searchMembersByName(const string &name)
    {
        vector<Member> foundMembers;
        for (auto &member : members)
        {
                if (member.getName().find(name) != string::npos)
                { foundMembers.push_back(member); }
        }
        return foundMembers;
    }
    // Calculate overdue fee
bool parseDate(const string &dateString, struct tm &dateTm)
    {
        istringstream dateStream(dateString);
        dateStream >> get_time(&dateTm, "%Y-%m-%d");
        return !dateStream.fail();
    }
double calculateOverdueFee(const string &borrowDate, const string &returnDate, double dailyFee = 1.0)
{
        struct tm borrowTm = {};
        struct tm returnTm = {};
        if (!parseDate(borrowDate, borrowTm) || !parseDate(returnDate, returnTm))
            {
                cerr << "Error: Invalid date format" << endl;
                return 0.0;
            }
        time_t borrowTime = mktime(&borrowTm);
        time_t returnTime = mktime(&returnTm);
        double daysDifference = difftime(returnTime, borrowTime) / (60 * 60 * 24);
        double overdueDays = daysDifference - 14; // Assuming a 2-week borrowing period
        return (overdueDays > 0) ? ceil(overdueDays) * dailyFee : 0.0;
}

// Display overdue members and their outstanding fees
void displayOverdueMembers()
{
        struct tm todayTm = {};
        time_t t = time(nullptr);
         localtime_s(&todayTm, &t);
         char todayStr[11];
         strftime(todayStr, sizeof(todayStr), "%Y-%m-%d", &todayTm);
         string today(todayStr);

   for (const auto &transaction : transactions)
    {
                if (transaction.getReturnDate() == "Not returned")
            {
                    double overdueFee = calculateOverdueFee(transaction.getBorrowDate(), today);
  if (overdueFee > 0)
            {
                for (const auto &member : members)
                {
                    if (member.getMemberID() == transaction.getMemberID())
                    {
                        cout << "Member: " << member.getName() << " - Overdue Fee: $" << overdueFee << "\n";
                        break;
                    }
                }
           }
        }
    }
}
// Generate and display reports
void generateReports()
{
    cout << "All Available Books:\n";
    for (const auto &book : books)
        {
            if (book.getStatus() == "Available")
                {
                    book.displayDetails();
                    cout << "----------------------\n";
                }
        }
cout << "\nBooks Currently Borrowed:\n";
    for (const auto &book : books)
        {
             if (book.getStatus() == "Borrowed")
                 {
                     book.displayDetails();
                     cout << "----------------------\n";
                 }
        }


 cout << "\nMembers with Overdue Books:\n";
        struct tm todayTm = {};
        time_t t = time(nullptr);
        localtime_s(&todayTm, &t);
        char todayStr[11];
        strftime(todayStr, sizeof(todayStr), "%Y-%m-%d", &todayTm);
        string today(todayStr);

   for (const auto &transaction : transactions)
         {
            if (transaction.getReturnDate() == "Not returned")
            {
                double overdueFee = calculateOverdueFee(transaction.getBorrowDate(), today);
                     if (overdueFee > 0)
                     {
                         for (const auto &member : members)
                         {
                        if (member.getMemberID() == transaction.getMemberID())
                        {
                            cout << "Member: " << member.getName() << " - Overdue Fee: $" << overdueFee << "\n";
                            break;
                        }
                    }
                }
            }
        }
}
};

// Main Function
int main() {
    Library library;

   while (true) {
        int choice;
        cout << "\nLibrary Management System\n";
        cout << "1. Add a New Book\n2. Edit Book\n3. Display Books\n4. Delete a book\n5. Add a New Member\n6. Edit Member\n";
        cout << "7. Display Members\n8. Delete Member\n9. Borrow a book\n10. Return a book\n11. Display transactions\n12. Search for a Book\n13. Search for a member\n14. Display overdue members\n15. Generate Reports\n16. Exit\n Enter your choice: ";
        cin >> choice;
        cin.ignore();

   switch (choice) {
            case 1: {
                int bookID;
                string title, author, genre, status;
                cout << "Enter Book ID: ";
                cin >> bookID;
                cin.ignore();
                cout << "Enter Title: ";
                getline(cin, title);
                cout << "Enter Author: ";
                getline(cin, author);
                cout << "Enter Genre: ";
                getline(cin, genre);
                cout << "Enter Status: ";
                getline(cin, status);
                library.addBook(bookID, title, author, genre, status);
                break;
            }
            case 2: {
                int bookID;
                cout << "Enter Book ID to edit: ";
                cin >> bookID;
                cin.ignore();
                library.editBook(bookID);
                break;
            }
            case 3:
                library.displayDetails();
                break;
            case 4:
                int bookID;
                cout << "Enter Book ID to delete: ";
                cin >> bookID;
                library.deleteBook(bookID);
                break;

   case 5: {
                int memberID, phoneNumber;
                string name, address;
                cout << "Enter Member ID: ";
                cin >> memberID;
                cin.ignore();
                cout << "Enter Name: ";
                getline(cin, name);
                cout << "Enter Address: ";
                getline(cin, address);
                cout << "Enter Phone Number: ";
                cin >> phoneNumber;
                cin.ignore();
                library.addMember(memberID, name, address, phoneNumber);
                break;
            }
            case 6: {
                int memberID;
                cout << "Enter Member ID to edit: ";
                cin >> memberID;
                cin.ignore();
                library.editMember(memberID);
                break;
            }
            case 7:
                library.displayMembers();
                break;
            case 8: {
                int memberID;
                cout << "Enter Member ID to delete: ";
                cin >> memberID;
                library.deleteMember(memberID);
                break;
            }
            case 9: {
                int transactionID, memberID, bookID;
                string borrowDate;
                cout << "Enter Transaction ID: ";
                cin >> transactionID;
                cout << "Enter Member ID: ";
                cin >> memberID;
                cout << "Enter Book ID: ";
                cin >> bookID;
                cout << "Enter Borrow Date (YYYY-MM-DD): ";
                cin >> borrowDate;
                library.borrowBook(transactionID, memberID, bookID, borrowDate);
                break;
            }
            case 10: {
                int transactionID;
                string returnDate;
                cout << "Enter Transaction ID: ";
                cin >> transactionID;
                cout << "Enter Return Date (YYYY-MM-DD): ";
                cin >> returnDate;
                library.returnBook(transactionID, returnDate);
                break;
            }
            case 11:
                library.displayTransactions();
                break;
            case 12:{
                int bookID;
                string bookTitle;

  cout << "Enter book ID to search: ";
                cin >> bookID;
                cin.ignore(); // Ignore leftover newline character
                Book* bookByID = library.searchBookByID(bookID);
                if (bookByID)
                    {
                        cout << "Book found by ID:\n";
                        bookByID->displayDetails();
                    }
                    else
                        { cout << "Book not found by ID.\n"; }
                cout << "Enter book title to search: ";
                getline(cin, bookTitle);
                vector<Book> booksByTitle = library.searchBooksByTitle(bookTitle);
                if (!booksByTitle.empty())
                    {
                        cout << "Books found by title:\n";
                for (const auto &book : booksByTitle)
                    {
                        book.displayDetails();
                       cout << "----------------------\n";
                    }
                    }
                    else { cout << "No books found by title.\n"; }
                    break;
            }
              case 13:
                  {
                string memberName;
                cout << "Enter member name to search: ";
                getline(cin, memberName);
                vector<Member> membersByName = library.searchMembersByName(memberName);
                if (!membersByName.empty())
                    {
                        cout << "Members found by name:\n";
                for (const auto &member : membersByName)
                    {
                        member.displayMember();
                        cout << "----------------------\n";
                    } }
                    else
                        { cout << "No members found by name.\n"; }
                    break;
                  }
            case 14:
              {

   cout<< "Overdue members are:\n";
                library.displayOverdueMembers();
                break;
              }
            case 15:
                library.generateReports();

   case 16:
                cout << "Exiting...\n";
                return 0;
            default:
                cout << "Invalid choice. Try again.\n";
        }
    }
}


