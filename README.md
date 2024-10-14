# Hotel-Chain-Database-Design

## Phase 1: Requirments Gathering and Data Mapping]

### Hotels
HotelID PK – unique hotel ID \
Name – specific hotel name \
Location – location of hotel e.g city, country \
Rating – official rating \
ContactInfo – email, phone number for hotel \
AvailableRooms – number of available rooms \
ManagerID FK – staff ID for the current manager \
Amenities – e.g pool, bar, gym

### Rooms
RoomID PK – unique room number \
HotelID FK – what hotel  \
Type – type of room e.g single, double, suite \
Amenities – e.g TV, wifi \
Price – listed price of the room \
Available – if it is currently available \
Capacity – max number of people that can stay 

### Guests
GuestID PK – unique guest number \
Name – full name of guest \
Email – email address \
Phone – phone number \
Preferences – additional preferences or requirements e.g ground floor, blind \
MembershipStatus – if they are currently a member or have been \
History – previous bookings listed here

### Bookings
BookingID PK – unique booking number \
GuestID FK – by what guest \
RoomID FK – what room \
CheckIn – check-in date \
CheckOut – check-out date \
BookedOn – date the guest booked it on \
Costs – cost breakdown including any additional fees \
PaymentStatus – if payment is pending, received, rejected etc

### Staff
StaffID - PK – unique number for each staff member \
Name – full name \
HotelID FK – works at what hotel \
Role – job role e.g. receptionist, chef \
Shifts – shift times e.g Monday – Friday 4pm - 12am \
Salary – current salary \
Hired – date they started

### Services
ServiceID PK – unique number for specific service \
HotelID FK – at what hotel \
ServiceType – e.g child daycare, breakfast \
Cost - price \
Status – if currently available

## Phase 2: Database Schema Design

![database schema](/images/databaseschema.png)

### SQL code, ENUM variables have been changed to VARCHAR(50) as may not be native support in some SQL databases
```sql
-- Table: Hotels
CREATE TABLE Hotels (
    HotelID INTEGER PRIMARY KEY,
    Name VARCHAR(255),
    Location VARCHAR(255),
    Rating NUMERIC,
    ContactInfo VARCHAR(255),
    AvailableRooms INT,
    ManagerStaffID INTEGER UNIQUE,
    Amenities VARCHAR(255),
    FOREIGN KEY (ManagerStaffID) REFERENCES Staff(StaffID)
);

-- Table: Rooms
CREATE TABLE Rooms (
    RoomID INTEGER PRIMARY KEY,
    HotelID INTEGER,
    Type VARCHAR(100),
    Amenities VARCHAR(255),
    Price NUMERIC,
    Available BOOLEAN,
    Capacity SMALLINT,
    FOREIGN KEY (HotelID) REFERENCES Hotels(HotelID) ON DELETE CASCADE
);

-- Table: Guests
CREATE TABLE Guests (
    GuestID INTEGER PRIMARY KEY,
    Name VARCHAR(255),
    Email VARCHAR(255),
    Phone BIGINT,
    Preferences TEXT,
    MembershipStatus VARCHAR(50),
    History INTEGER,
    FOREIGN KEY (History) REFERENCES Bookings(BookingID) ON DELETE CASCADE
);

-- Table: Bookings
CREATE TABLE Bookings (
    BookingID INTEGER PRIMARY KEY,
    GuestID INTEGER,
    RoomID INTEGER,
    CheckIn DATETIME,
    CheckOut DATETIME,
    BookedOn DATETIME,
    Costs TEXT,
    PaymentStatus VARCHAR(50),
    FOREIGN KEY (GuestID) REFERENCES Guests(GuestID) ON DELETE CASCADE,
    FOREIGN KEY (RoomID) REFERENCES Rooms(RoomID) ON DELETE CASCADE
);

-- Table: Staff
CREATE TABLE Staff (
    StaffID INTEGER PRIMARY KEY,
    Name VARCHAR(255),
    HotelID INTEGER,
    Role VARCHAR(100),
    Shifts VARCHAR(100),
    Salary INTEGER,
    Hired DATETIME,
    FOREIGN KEY (HotelID) REFERENCES Hotels(HotelID) ON DELETE CASCADE
);

-- Table: Services
CREATE TABLE Services (
    ServiceID INTEGER PRIMARY KEY,
    HotelID INTEGER,
    ServiceType VARCHAR(100),
    Cost NUMERIC,
    Status VARCHAR(50),
    FOREIGN KEY (HotelID) REFERENCES Hotels(HotelID) ON DELETE CASCADE
);
```
