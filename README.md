# Overview

OpenRoad is a transport management system and we as developers have to integrate plenty of third party services (transfer information to brokers, other carriers, etc).  At this point, we have more than 15 different integrations done. As a result, we need to maintain and debug all of the code associated with integrations, so it must be easy to maintain.

We were requested by our customers to have an integration with SpaceWagon which is one of the leading companies in the USA.

# Requirements

For you to better understand the business logic, let's take a look:

* Carriers (companies which use OpenRoad) have shipments, trucks, trailers,
  customers and drivers;
* Each shipment has at least one driver, truck and trailer assigned, and exactly one
  customer;
* Shipments have statuses so it is easier to track them ("booked", "assigned", "en_route_to_shipper", "at_shipper", "picked_up", "delayed", "en_route_to_consignee", "at_consignee", "delivered", "delivered_pending", "exported");
* All trucks and trailers also have GPS devices and we have access to the real time location;

Basically, carriers use our system to keep track of most of the things occurring in their business but brokers only require small piece of information sent to them through third-party services (e.g. status, location, driver ID, truck ID).

## Technical assumptions

* When receiving geocoordinates from GPS devices, city, state, country, zip code and all of needed information is included;
* BOL# (bill of lading ID) is unique to each shipment;
* SCAC is a Standard Carrier Alpha Code which is unique for each carrier (4 letter code);
* There is no multi-tenancy and only 1 carrier is using the system;

## Technical requirements
* Create a Ruby on Rails application;
* Your application should include models mentioned above;
* Setup your application so you may fullfill the SpaceWagon's integration from the business perspective;
* Implement SpaceWagon's service;

### Tips
* As mentioned before, we have way too many integrations, so your code should be
  easy to read, understand and maintain;
* Your code should be tested (feel free to use any framework, we use RSpec);
* Do not waste time on application's architecture as you have not enough
  information to make correct decisions;
* Homework assignment might seem uninformative and missing pieces but that is
  what we are testing - your creativity and ability to perform;

If you have any questions, contact your associated person.

## SpaceWagon specification

Below is the exact information we received from SpaceWagon.

### File Layout

#### Headers
Headers are not required, but can be included.

#### Status instructions

* Load numbers and truck locations should be included on the file once a truck has been assigned to a load.
* A delivery timestamp should populate on the file once the truck is empty. The load can be dropped off the file once a delivery timestamp has been populated.

#### Layout
The column numbering below is flexible. Required fields are indicated.
* Column 1 (Required)
  Customer ID -Billing code or customer code. Must remain constant across all rows for each customer. (Example: Ryder)
* Column 2 (Required)
  BOL# -matching reference field as indicated by SpaceWagon and customer.
* Column 3 (Required)
  SCAC Code
* Column 4 (Preferred)
  Truck #
* Column 5 (Required)
  Latest Latitude
* Column 6 (Required)
  Latest Longitude
* Column 7 (Required)
  Timestamp of given latitude and longitude above
* Column 8 (Required for troubleshooting purposes)
  Current City
* Column 9 (Required for troubleshooting purposes)
  Current State
* Column 10 (Required when load is delivered)
  Timestamp of Delivery Date and Time

##### Acceptable Timestamp formats

|Timestamp    |
|---          |
|201508270014 |
|2015-08-26 22:06:41.000|
|2015-08-21-10.01.00|
|Aug 25 2015 4:33PM|
|201508171635|
|2015-08-20 05:33:01|
|2015-08-14-09.31.03.000000|
|2015-08-24-11.44.23|
|2015-08-24-05.01.00|
|2015-06-30-17.45.00.000000|
|2015-08-26 18:33:00.000|
|2015-08-25-16.01.00|
|2015-08-24-04.13.36|
|2015-08-03-13.19.00|
|2015-08-24-14.12.00|
|2015-08-14-08.44.00.000000|
|2015-08-24-07.50.00|
|2015-08-08 11.26.00|

#### Frequencyand Method of Delivery

##### Frequency
The file should be sent every 15 minutes. Please talk with a SpaceWagon associate if this is not possible.

##### FTP Instructions
* Files should be placed in the "new" folder;
* We accept CSV, XLS, XLSX;
* File names must be unique. We recommend adding timestamp to the end of a standard file name;
* Credentials will be suplied by the SpaceWagon operations team upon request;
* Note: The file name should not include a period in the name;
