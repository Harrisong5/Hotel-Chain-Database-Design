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
## Phase 3: Data Insertion

```
INSERT INTO Hotels (HotelID, Name, Location, Rating, ContactInfo, AvailableRooms, ManagerStaffID, Amenities)
VALUES 
(1, 'Lake View Resort', 'Lake District, Windermere, United Kingdom', 4.6, '123-456-7890', 22, 1, 'Resturant, Spa, Gym, Conference Room, Golf Course'),
(2, 'Royal Buddha', 'Luang Prabang, Laos', 4.1, '234-567-8910', 47, 42, 'Pool, Spa, Gym, Tennis Courts, Gardens'),
(3, 'Techno Heaven', 'Berlin, Germany', 3.9, '345,678,9101', 60, 86, 'Spa, Gym, Resturant'),
(4, 'Andean Valley', 'Temuco, Chile', 4.8, '456-789-1011', 18, 120, 'Plunge Pool, Spa, Gym, Resturant'),
(5, 'Nature Retreat', 'Tholo, Lesotho', 4.9, '567-891-0112', 82, 155, 'Safari Platform, Pool, Spa, Resturant, Education Centre'),

INSERT INTO Rooms (RoomID, HotelID, Type, Amenities, Price, Available, Capacity)
VALUES
(1, 1, 'suite', 'TV', 861.24, true),
(2, 2, 'penthouse', 'Coffee maker', 836.34, true),
(3, 5, 'suite', 'Hair dryer', 795.75, true);
-- First 3 of 100

INSERT INTO Guests (GuestID, Name, Email, Phone, Preferences, MembershipStatus, History)
VALUES
'Hiro Yamamoto', 'ahandling0@google.es', '\+039 142 656 6871', 'service animal', 'suspended', '62, 94'),
(2, 'John Smith', 'blauchlan1@google.co.uk', '\+373 064 371 7475', null, 'suspended', 'null'),
(3, 'Maria Garcia', 'bmattaus2@sohu.com', '\+703 267 479 0053', 'nut allergy', 'active', '188');
--First 3 of 200

INSERT INTO Bookings (BookingID, GuestID, RoomID, CheckIn, CheckOut, BookedOn, Costs, PaymentStatus)
VALUES
(1, 42, '2|84|100', '2/20/2024', '1/12/2001', '4/14/2021', 'mini bar', 'paid'),
(2, 145, '9|72|100', '11/24/2023', null, '8/24/2021', 'late check-out', 'late'),
(3, 132, '5|96|100', '6/28/2024', '1/12/2001', '5/5/2021', 'spa services', null);
--First 3 of 300

INSERT INTO Staff (StaffID, Name, HotelID, Role, Shifts, Salary, Hired)
VALUES
(1, 'John Smith', 2, 'Hotel Manager', 'Monday-Friday 9am-5pm', 193785.44, '10/19/2023'),
(2, 'Maria Garcia', 5, 'Housekeeper', 'Monday-Friday 9am-5pm', 149507.98, '9/28/2024'),
(3, 'Hiro Yamamoto', 5, 'Housekeeper', 'Tuesday-Saturday 1pm-9pm', 127805.87, '2/26/2024');
--First 3 of 50

INSERT INTO Services (ServiceID, HotelID, ServiceType, Cost, Status)
VALUES 
(1, 2, 'Valet parking', 283.02, 'available'),
(2, 3, 'Spa treatment', 415.33, 'available'),
(3, 4, 'Room cleaning', 309.04, 'out of service'),
(4, 1, 'Room service', 136.99, 'unavailable'),
(5, 5, 'Valet parking', 353.08, 'available');
```
## Phase 4: Querying and Business Analysis

### Basic Queries:

- List all available rooms at a specific hotel (Hotel 1 chosen)
```sql
SELECT RoomID, Type, Price, Capacity, Amenities
FROM Rooms
WHERE HotelID = 1 AND Available = TRUE;
```
- Retrieve guest details and preferences for all guests who stayed in the last month:
```sql
SELECT Guests.GuestID, Guests.Name, Guests.Email, Guests.Phone, Guests.Preferences
FROM Guests
JOIN Bookings ON Guests.GuestID = Bookings.GuestID
WHERE Bookings.CheckOut >= DATEADD(month, -1, GETDATE());
```
- Calculate total revenue generated by each hotel property in a specific time period: (01/05/2024 and 01/09/2024 chosen)
```sql
SELECT Hotels.HotelID, Hotels.Name, SUM(CAST(LEFT(Bookings.Costs, CHARINDEX(' ', Costs) - 1) AS numeric)) AS TotalRevenue
FROM Bookings
JOIN Rooms ON Bookings.RoomID = Rooms.RoomID
JOIN Hotels ON Rooms.HotelID = Hotels.HotelID
WHERE Bookings.BookedOn BETWEEN '2024-05-01' AND '2024-09-01'
GROUP BY Hotels.HotelID, Hotels.Name;
```
### Advanced Queries:
- Determine which room types generate the most revenue across the chain:
```sql
SELECT Rooms.Type, SUM(CAST(LEFT(Bookings.Costs, CHARINDEX(' ', Costs) - 1) AS numeric)) AS TotalRevenue
FROM Bookings
JOIN Rooms ON Bookings.RoomID = Rooms.RoomID
GROUP BY Rooms.Type
ORDER BY TotalRevenue DESC;
```
- Identify guests who have stayed in more than two hotels within the chain:
```sql
SELECT Guests.GuestID, Guests.Name, COUNT(DISTINCT Hotels.HotelID) AS HotelCount
FROM Guests
JOIN Bookings ON Guests.GuestID = Bookings.GuestID
JOIN Rooms ON Bookings.RoomID = Rooms.RoomID
JOIN Hotels ON Rooms.HotelID = Hotels.HotelID
GROUP BY Guests.GuestID, Guests.Name
HAVING COUNT(DISTINCT Hotels.HotelID) > 2;
```
- Calculate the average occupancy rate for each hotel over the past six months:
```sql
SELECT Hotels.HotelID, Hotels.Name, 
       ROUND((CAST(COUNT(Bookings.RoomID) AS FLOAT) / (COUNT(Rooms.RoomID) * 180)) * 100, 2) AS AvgOccupancyRate
FROM Hotels
JOIN Rooms ON Hotels.HotelID = Rooms.HotelID
LEFT JOIN Bookings ON Rooms.RoomID = Bookings.RoomID AND Bookings.CheckIn BETWEEN DATEADD(month, -6, GETDATE()) AND GETDATE()
GROUP BY Hotels.HotelID, Hotels.Name;
```
### Business Analysis:
- Analysis on the most requested services and their revenue contribution:
```sql
SELECT Services.ServiceType, COUNT(Bookings.BookingID) AS RequestCount, 
       SUM(Services.Cost) AS TotalRevenue
FROM Services
JOIN Hotels ON Services.HotelID = Hotels.HotelID
JOIN Bookings ON Services.HotelID = Hotels.HotelID
GROUP BY Services.ServiceType
ORDER BY TotalRevenue DESC;
```
- Identify peak booking periods and recommend staff scheduling optimizations:
```sql
SELECT DATEPART(month, Bookings.CheckIn) AS Month, COUNT(Bookings.BookingID) AS BookingCount
FROM Bookings
GROUP BY DATEPART(month, Bookings.CheckIn)
ORDER BY BookingCount DESC;
```
