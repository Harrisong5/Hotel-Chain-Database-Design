# Hotel-Chain-Database-Design

## Phase 1: Requirments Gathering and Data Mapping]

###Hotels
HotelID PK – unique hotel ID
Name – specific hotel name
Location – location of hotel e.g city, country
Rating – official rating
ContactInfo – email, phone number for hotel
AvailableRooms – number of available rooms
ManagerID FK – staff ID for the current manager
Amenities – e.g pool, bar, gym

###Rooms
RoomID PK – unique room number
HotelID FK – what hotel 
Type – type of room e.g single, double, suite
Amenities – e.g TV, wifi
Price – listed price of the room
Available – if it is currently available
Capacity – max number of people that can stay 

###Guests
GuestID PK – unique guest number
Name – full name of guest 
Email – email address
Phone – phone number
Preferences – additional preferences or requirements e.g ground floor, blind
MembershipStatus – if they are currently a member or have been
History – previous bookings listed here

###Bookings
BookingID PK – unique booking number
GuestID FK – by what guest
RoomID FK – what room
CheckIn – check-in date
CheckOut – check-out date
BookedOn – date the guest booked it on
Costs – cost breakdown including any additional fees
PaymentStatus – if payment is pending, received, rejected etc

###Staff
StaffID - PK – unique number for each staff member
Name – full name
HotelID FK – works at what hotel
Role – job role e.g. receptionist, chef
Shifts – shift times e.g Monday – Friday 4pm - 12am
Salary – current salary
Hired – date they started

###Services
ServiceID PK – unique number for specific service
HotelID FK – at what hotel
ServiceType – e.g child daycare, breakfast
Cost - price
Status – if currently available
