<h1 align="center">Design a Hotel Management System</h1>
<h3 align="center">Let's design a Hotel Management System</h3>

**We'll cover the following:**

* [System Requirements](#system-requirements)
* [Use Case Diagram](#use-case-diagram)
* [Class Diagram](#class-diagram)
* [Activity Diagrams](#activity-diagrams)
* [Code](#code)

A Hotel Management System is a software built to handle all online hotel activities easily and safely. This System will give the hotel management power and flexibility to manage the entire system from a single online portal. The system allows the manager to keep track of all the available rooms in the system as well as to book rooms and generate bills.

<p align="center">
    <img src="/media-files/hotel-management-system.png" alt="Hotel Management System">
    <br />
    Hotel Management System
</p>

### System Requirements

We’ll focus on the following set of requirements while designing the Hotel Management System:

1. The system should support the booking of different room types like standard, deluxe, family suite, etc.
2. Guests should be able to search the room inventory and book any available room.
3. The system should be able to retrieve information, such as who booked a particular room, or what rooms were booked by a specific customer.
4. The system should allow customers to cancel their booking - and provide them with a full refund if the cancelation occurs before 24 hours of the check-in date.
5. The system should be able to send notifications whenever the booking is nearing the check-in or check-out date.
6. The system should maintain a room housekeeping log to keep track of all housekeeping tasks.
7. Any customer should be able to add room services and food items.
8. Customers can ask for different amenities.
9. The customers should be able to pay their bills through credit card, check or cash.

### Use Case Diagram

Here are the main Actors in our system:

* **Guest:** All guests can search the available rooms, as well as make a booking.
* **Receptionist:** Mainly responsible for adding and modifying rooms, creating room bookings, check-in, and check-out customers.
* **System:** Mainly responsible for sending notifications for room booking, cancellation, etc.
* **Manager:** Mainly responsible for adding new workers.
* **Housekeeper:** To add/modify housekeeping record of rooms.
* **Server:** To add/modify room service record of rooms.

Here are the top use cases of the Hotel Management System:

* **Add/Remove/Edit room:** To add, remove, or modify a room in the system.
* **Search room:** To search for rooms by type and availability.
* **Register or cancel an account:** To add a new member or cancel the membership of an existing member.
* **Book room:** To book a room.
* **Check-in:** To let the guest check-in for their booking.
* **Check-out:** To track the end of the booking and the return of the room keys.
* **Add room charge:** To add a room service charge to the customer’s bill.
* **Update housekeeping log:** To add or update the housekeeping entry of a room.

Here is the use case diagram of our Hotel Management System:

<p align="center">
    <img src="/media-files/hms-use-case-diagram.svg" alt="Hotel Management System Use Case Diagram">
    <br />
    Use Case Diagram for Hotel Management System
</p>

### Class Diagram

Here are the main classes of our Hotel Management System:

* **Hotel and HotelLocation:** Our system will support multiple locations of a hotel.
* **Room:** The basic building block of the system. Every room will be uniquely identified by the room number. Each Room will have attributes like Room Style, Booking Price, etc.
* **Account:** We will have different types of accounts in the system: one will be a guest to search and book rooms, another will be a receptionist. Housekeeping will keep track of the housekeeping records of a room, and a Server will handle room service.
* **RoomBooking:** This class will be responsible for managing bookings for a room.
* **Notification:** Will take care of sending notifications to guests.
* **RoomHouseKeeping:** To keep track of all housekeeping records for rooms.
* **RoomCharge:** Encapsulates the details about different types of room services that guests have requested.
* **Invoice:** Contains different invoice-items for every charge against the room.
* **RoomKey:** Each room can be assigned an electronic key card. Keys will have a barcode and will be uniquely identified by a key-ID.

<p align="center">
    <img src="/media-files/hms-class-diagram.png" alt="Hotel Management System Class Diagram">
    <br />
    Class Diagram for Hotel Management System
</p>

<p align="center">
    <img src="/media-files/hms-uml.svg" alt="Hotel Management System UML">
    <br />
    UML for Hotel Management System
</p>

### Activity Diagrams

**Make a room booking:** Any guest or receptionist can perform this activity. Here are the set of steps to book a room:

<p align="center">
    <img src="/media-files/hms-room-booking-activity-diagram.svg" alt="Hotel Management System Room Booking">
    <br />
    Activity Diagram for Hotel Management System Room Booking
</p>

**Check in:** Guest will check in for their booking. The Receptionist can also perform this activity. Here are the steps:

<p align="center">
    <img src="/media-files/hms-check-in-activity-diagram.svg" alt="Hotel Management System Check in">
    <br />
    Activity Diagram for Hotel Management System Check in
</p>

**Cancel a booking:** Guest can cancel their booking. Receptionist can perform this activity. Here are the different steps of this activity:

<p align="center">
    <img src="/media-files/hms-cancel-booking-activity-diagram.svg" alt="Hotel Management System Cancel Booking">
    <br />
    Activity Diagram for Hotel Management System Cancel Booking
</p>

### Code

Here is the high-level definition for the classes described above.

**Enums, data types, and constants:** Here are the required enums, data types, and constants:

```java

public enum RoomStyle {
    STANDARD, DELUXE, FAMILY_SUITE, BUSINESS_SUITE
}

public enum RoomStatus {
    AVAILABLE, RESERVED, OCCUPIED, NOT_AVAILABLE, BEING_SERVICED, OTHER
}

@Entity
public class Address {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String street;
    private String city;
    private String state;
    private String zipCode;
    private String country;
}

@Entity
public class Account {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String accountId;
    private String password;

    @Enumerated(EnumType.STRING)
    private AccountStatus status;
}

@Entity
@Inheritance(strategy = InheritanceType.TABLE_PER_CLASS)
public abstract class Person {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String name;

    @OneToOne(cascade = CascadeType.ALL)
    private Address address;

    private String email;
    private String phone;

    @ManyToOne
    private Account account;
}

@Entity
public class Guest extends Person {
    private int totalRoomsCheckedIn;

    @OneToMany(mappedBy = "guest", cascade = CascadeType.ALL)
    private List<RoomBooking> bookings;
}

@Entity
public class Receptionist extends Person {
    @OneToMany(mappedBy = "receptionist", cascade = CascadeType.ALL)
    private List<RoomBooking> bookings;
}

@Entity
public class Server extends Person {
    @OneToMany(mappedBy = "server", cascade = CascadeType.ALL)
    private List<RoomCharge> roomCharges;
}

@Entity
public class HotelLocation {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String name;

    @OneToOne(cascade = CascadeType.ALL)
    private Address location;

    @OneToMany(mappedBy = "location", cascade = CascadeType.ALL)
    private List<Room> rooms;
}

@Entity
public class Hotel {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String name;

    @OneToMany(mappedBy = "hotel", cascade = CascadeType.ALL)
    private List<HotelLocation> locations;
}

@Entity
public class Room {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private int roomNumber;

    @Enumerated(EnumType.STRING)
    private RoomStyle style;

    @Enumerated(EnumType.STRING)
    private RoomStatus status;

    private double bookingPrice;
    private boolean isSmoking;

    @ManyToOne
    private HotelLocation location;

    @OneToMany(mappedBy = "room", cascade = CascadeType.ALL)
    private List<RoomKey> keys;

    @OneToMany(mappedBy = "room", cascade = CascadeType.ALL)
    private List<RoomHouseKeeping> houseKeepingLog;

    @OneToMany(mappedBy = "room", cascade = CascadeType.ALL)
    private List<RoomBooking> bookings;
}

@Entity
public class RoomKey {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String barcode;
    private boolean isActive;
    private boolean isMaster;

    @ManyToOne
    private Room room;
}

@Entity
public class RoomHouseKeeping {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String description;
    private LocalDateTime startDateTime;
    private int duration;

    @ManyToOne
    private Room room;
}

@Entity
public class RoomBooking {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String reservationNumber;
    private LocalDateTime startDate;
    private int durationInDays;

    @Enumerated(EnumType.STRING)
    private BookingStatus status;

    @ManyToOne
    private Guest guest;

    @ManyToOne
    private Room room;

    @OneToOne(mappedBy = "booking", cascade = CascadeType.ALL)
    private Invoice invoice;

    @OneToMany(mappedBy = "booking", cascade = CascadeType.ALL)
    private List<Notification> notifications;
}

@Entity
public class RoomCharge {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private LocalDateTime issueAt;

    @Enumerated(EnumType.STRING)
    private PaymentStatus paymentStatus;

    @ManyToOne
    private RoomBooking booking;
}

@Entity
public class Invoice {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @OneToMany(mappedBy = "invoice", cascade = CascadeType.ALL)
    private List<InvoiceItem> items;
}

@Entity
public class InvoiceItem {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String description;
    private double amount;

    @ManyToOne
    private Invoice invoice;
}

@Entity
public class Notification {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String message;
    private LocalDateTime sentAt;

    @ManyToOne
    private RoomBooking booking;
}

@Repository
public interface GuestRepository extends JpaRepository<Guest, Long> {
}

@Repository
public interface RoomRepository extends JpaRepository<Room, Long> {
}

@Service
public class GuestService {
    @Autowired
    private GuestRepository guestRepository;

}

@Service
public class RoomService {
    @Autowired
    private RoomRepository roomRepository;

}
```



