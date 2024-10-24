# Hotel-Mangement-


#include <iostream>
#include <string.h>

#define max 100
using namespace std;

// Class Customer
class Customer {
public:
    char name[100];
    char address[100];
    char phone[12];
    char from_date[20];
    char to_date[20];
    float payment_advance;
    int booking_id;
};

class Room {
public:
    char type;
    char stype;
    char ac;
    int roomNumber;
    int rent;
    int status; // 0 - Available, 1 - Reserved

    Customer cust;

    Room addRoom(int);
    void searchRoom(int);
    void displayRoom(Room);
};

// Global Declarations
Room rooms[max];
int count = 0;

Room Room::addRoom(int rno) {
    Room room;
    room.roomNumber = rno;
    cout << "\nType AC/Non-AC (A for AC, N for Non-AC) : ";
    cin >> room.ac;

    cout << "\nType Comfort (S for Superior, N for Normal) : ";
    cin >> room.type;

    cout << "\nType Size (B for Big, S for Small) : ";
    cin >> room.stype;

    cout << "\nDaily Rent : ";
    cin >> room.rent;
    room.status = 0;

    cout << "\nRoom Added Successfully!";
    return room;
}

void Room::searchRoom(int rno) {
    int i, found = 0;
    for (i = 0; i < count; i++) {
        if (rooms[i].roomNumber == rno) {
            found = 1;
            break;
        }
    }
    if (found == 1) {
        cout << "\nRoom Details are as follows :-\n";
        if (rooms[i].status == 1) {
            cout << "\nRoom is Reserved";
        } else {
            cout << "\nRoom is Available";
        }
        displayRoom(rooms[i]);
    } else {
        cout << "\nRoom Not Found";
    }
}

void Room::displayRoom(Room tempRoom) {
    cout << "\nRoom Number :- \t" << tempRoom.roomNumber;
    cout << "\nType AC/Non-AC (A for AC, N for Non-AC) :- " << tempRoom.ac;
    cout << "\nType Comfort (S for Superior, N for Normal) :- " << tempRoom.type;
    cout << "\nType Size (B for Big, S for Small) :- " << tempRoom.stype;
    cout << "\nDaily Room Rent: " << tempRoom.rent << "\n\n";
}

// Hotel management class
class HotelMgnt : protected Room {
public:
    void checkIn();
    void getAvailRoom();
    void searchCustomer(char*);
    void checkOut(int);
};

// Hotel management reservation of room
void HotelMgnt::checkIn() {
    int i, found = 0, rno;

    cout << "\nEnter Room number : ";
    cin >> rno;
    for (i = 0; i < count; i++) {
        if (rooms[i].roomNumber == rno) {
            found = 1;
            break;
        }
    }
    if (found == 1) {
        if (rooms[i].status == 1) {
            cout << "\nRoom is already Booked";
            return;
        }

        cout << "\nEnter booking id: ";
        cin >> rooms[i].cust.booking_id;

        cout << "\nEnter Customer Name (First Name): ";
        cin >> rooms[i].cust.name;

        cout << "\nEnter Address (only city): ";
        cin >> rooms[i].cust.address;

        cout << "\nEnter Phone: ";
        cin >> rooms[i].cust.phone;

        cout << "\nEnter From Date: ";
        cin >> rooms[i].cust.from_date;

        cout << "\nEnter To Date: ";
        cin >> rooms[i].cust.to_date;

        cout << "\nEnter Advance Payment: ";
        cin >> rooms[i].cust.payment_advance;

        rooms[i].status = 1;

        cout << "\n Customer Checked-in Successfully... \n\n";
    } else {
        cout << "\nRoom number " << rno << " is not added yet. Please add the room first before checking in.\n";
    }
    cout << "\n";
}

// Hotel management shows available rooms
void HotelMgnt::getAvailRoom() {
    int i, found = 0, availableCount = 0;
    for (i = 0; i < count; i++) {
        if (rooms[i].status == 0) {
            displayRoom(rooms[i]);
            availableCount++;
            found = 1;
        }
    }
    if (found == 0) {
        cout << "\nAll rooms are reserved";
    } else {
        cout << "\nTotal Available Rooms: " << availableCount;
    }
}

// Hotel management shows all persons that have booked room
void HotelMgnt::searchCustomer(char* pname) {
    int i, found = 0;
    for (i = 0; i < count; i++) {
        if (rooms[i].status == 1 && strcmp(rooms[i].cust.name, pname) == 0) {
            cout << "\nCustomer Name: " << rooms[i].cust.name;
            cout << "\nRoom Number: " << rooms[i].roomNumber;
            found = 1;
        }
    }
    if (found == 0) {
        cout << "\nPerson not found.\n";
    }
}

// Hotel management generates the bill of the expenses
void HotelMgnt::checkOut(int roomNum) {
    int i, found = 0, days;
    float billAmount = 0;
    for (i = 0; i < count; i++) {
        if (rooms[i].status == 1 && rooms[i].roomNumber == roomNum) {
            found = 1;
            break;
        }
    }
    if (found == 1) {
        cout << "\nEnter Number of Days:\t";
        cin >> days;
        billAmount = days * rooms[i].rent;

        cout << "\n\t######## CheckOut Details ########\n";
        cout << "\nCustomer Name : " << rooms[i].cust.name;
        cout << "\nRoom Number : " << rooms[i].roomNumber;
        cout << "\nAddress : " << rooms[i].cust.address;
        cout << "\nPhone : " << rooms[i].cust.phone;
        cout << "\nTotal Amount Due : " << billAmount << " /";
        cout << "\nAdvance Paid: " << rooms[i].cust.payment_advance << " /";
        cout << "\n*** Total Payable: " << billAmount - rooms[i].cust.payment_advance << "/ only";

        rooms[i].status = 0; // Mark room as available
    }
}

// Managing rooms (adding and searching available rooms)
void manageRooms() {
    Room room;
    int opt, rno, i, flag = 0;

    do {
        cout << "\n### Manage Rooms ###";
        cout << "\n1. Add Room";
        cout << "\n2. Search Room";
        cout << "\n3. Back to Main Menu";
        cout << "\n\nEnter Option: ";
        cin >> opt;

        switch (opt) {
            case 1:
                cout << "\nEnter Room Number: ";
                cin >> rno;
                i = 0;
                for (i = 0; i < count; i++) {
                    if (rooms[i].roomNumber == rno) {
                        flag = 1;
                    }
                }
                if (flag == 1) {
                    cout << "\nRoom Number is Present.\nPlease enter a unique Number";
                    flag = 0;
                } else {
                    rooms[count] = room.addRoom(rno);
                    count++;
                }
                break;
            case 2:
                cout << "\nEnter room number: ";
                cin >> rno;
                room.searchRoom(rno);
                break;
            case 3:
                break;
            default:
                cout << "\nPlease Enter correct option";
                break;
        }
    } while (opt != 3);
}

int main() {
    HotelMgnt hm;
    int opt, rno;
    char pname[100];

    do {
        cout << "\n######## Hotel Management System #########\n";
        cout << "\nWelcome to the Hotel Management System. Please choose from the following services:\n\n";
        cout << "\n1. Manage Rooms";
        cout << "\n2. Check-In Room";
        cout << "\n3. Available Rooms";
        cout << "\n4. Search Customer";
        cout << "\n5. Check-Out Room";
        cout << "\n\nEnter Option: ";
        cin >> opt;

        switch (opt) {
            case 1:
                manageRooms();
                break;
            case 2:
                hm.checkIn();
                break;
            case 3:
                hm.getAvailRoom();
                break;
            case 4:
                cout << "\nEnter Customer Name: ";
                cin >> pname;
                hm.searchCustomer(pname);
                break;
            case 5:
                cout << "\nEnter Room Number for Check-Out: ";
                cin >> rno;
                hm.checkOut(rno);
                break;
            default:
                cout << "\nPlease enter a valid option!";
                break;
        }
    } while (opt != 6);

    return 0;
}
