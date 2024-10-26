# Bus-Reservation-System
#include <iostream>
#include <string>
#include <vector>
using namespace std;
struct Bus {
    string bus_no;
    string source;
    string destination;
    string departure_time;
    int total_seats;
    int available_seats;
};

struct Booking {
    string bus_no;
    int seat_number;
};

vector<Bus> buses;
vector<Booking> bookings;

void displayBuses() {
    cout << "Available Buses:\n";
    for (const auto& bus : buses) {
        cout << "Bus No: " << bus.bus_no << endl;
        cout << "Source: " << bus.source << endl;
        cout << "Destination: " << bus.destination << endl;
        cout << "Departure Time: " << bus.departure_time << endl;
        cout << "Available Seats: " << bus.available_seats << endl;
        cout << endl;
    }
}

void bookTicket() {
    string bus_no;
    cout << "Enter Bus No: ";
    cin >> bus_no;

    for (auto& bus : buses) {
        if (bus.bus_no == bus_no) {
            if (bus.available_seats > 0) {
                int seat_number = bus.total_seats - bus.available_seats + 1;
                bus.available_seats--;
                bookings.push_back({bus_no, seat_number});
                cout << "Ticket booked successfully! Seat Number: " << seat_number << endl;
                return;
            } else {
                cout << "No available seats on this bus.\n";
                return;
            }
        }
    }
    cout << "Bus not found.\n";
}

void cancelTicket() {
    string bus_no;
    int seat_number;
    cout << "Enter Bus No: ";
    cin >> bus_no;
    cout << "Enter Seat Number: ";
    cin >> seat_number;

    for (auto it = bookings.begin(); it != bookings.end(); ++it) {
        if (it->bus_no == bus_no && it->seat_number == seat_number) {
            bookings.erase(it);
            for (auto& bus : buses) {
                if (bus.bus_no == bus_no) {
                    bus.available_seats++;
                    cout << "Ticket canceled successfully.\n";
                    return;
                }
            }
        }
    }
    cout << "Booking not found.\n";
}

void viewBookings() {
    cout << "Your Bookings:\n";
    for (const auto& booking : bookings) {
        cout << "Bus No: " << booking.bus_no << ", Seat Number: " << booking.seat_number << endl;
    }
}

int main() {
    // Initialize some buses
    Bus bus1 = {"B1", "City A", "City B", "10:00 AM", 50, 40};
    Bus bus2 = {"B2", "City C", "City D", "12:00 PM", 40, 35};
    buses.push_back(bus1);
    buses.push_back(bus2);

    int choice;
    do {
        cout << "1. Display Buses\n";
        cout << "2. Book Ticket\n";
        cout << "3. Cancel Ticket\n";
        cout << "4. View Bookings\n";
        cout << "5. Exit\n";
        cout << "Enter your choice: ";
        cin >> choice;

        switch (choice) {
            case 1:
                displayBuses();
                break;
            case 2:
                bookTicket();
                break;
            case 3:
                cancelTicket();
                break;
            case 4:
                viewBookings();
                break;
            case 5:
                cout << "Exiting...\n";
                break;
            default:
                cout << "Invalid choice!\n";
        }
    } while (choice != 5);

    return 0;
}
