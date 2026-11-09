# Requirements

Requirements catalog for the reservation, check-in/check-out, and room cleaning system of a ten-room hotel, derived
from [vision.md](vision.md). Domain terms follow the [glossary](glossary.md).

## Functional Requirements

| ID     | Title                        | User Story                                                                                                                                                                      | Priority | Status |
|--------|------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------|--------|
| FR-001 | Search Availability          | As a guest, I want to see which room categories are available for my arrival date, departure date, and number of persons, with the total price, so that I can choose a room.    | High     | Open   |
| FR-002 | Make Reservation             | As a guest, I want to reserve a room category for a date range by entering my name and email address, without creating an account, so that a room is held for my stay.          | High     | Open   |
| FR-003 | Receive Confirmation         | As a guest, I want to receive a confirmation email with my reservation number after reserving so that I have proof of my reservation.                                           | High     | Open   |
| FR-004 | View Own Reservation         | As a guest, I want to view my reservation by entering my reservation number and email address so that I can check its dates and price.                                          | Medium   | Open   |
| FR-005 | Cancel Own Reservation       | As a guest, I want to cancel my reservation by entering my reservation number and email address so that the room is released when I cannot come.                                | Medium   | Open   |
| FR-006 | Create Reservation for Guest | As a receptionist, I want to create a reservation on behalf of a guest who calls or walks in so that reservations not made online are recorded in the same system.             | High     | Open   |
| FR-007 | Change Reservation           | As a receptionist, I want to change the dates or the room category of a reservation so that I can handle a guest's change request.                                             | High     | Open   |
| FR-008 | Cancel Reservation for Guest | As a receptionist, I want to cancel a reservation on behalf of a guest so that the room becomes available again.                                                               | High     | Open   |
| FR-009 | Search Reservations          | As a receptionist, I want to search reservations by guest name, reservation number, or date so that I can find a reservation quickly when a guest contacts me.                  | High     | Open   |
| FR-010 | View Arrival List            | As a receptionist, I want to see the list of guests arriving on a given day so that I can prepare their check-in.                                                              | High     | Open   |
| FR-011 | View Departure List          | As a receptionist, I want to see the list of guests departing on a given day so that I can prepare their check-out.                                                            | High     | Open   |
| FR-012 | Check In Guest               | As a receptionist, I want to check in a guest by assigning a clean room of the reserved room category so that the guest knows their room and the room is shown as occupied.| High     | Open   |
| FR-013 | Record Guest Registration    | As a receptionist, I want to record the guest registration data (address, nationality, date of birth, identity document) at check-in so that the hotel meets its registration duty. | Medium   | Open   |
| FR-014 | Check Out Guest              | As a receptionist, I want to check out a guest and see the total price of the stay so that I can collect the payment and release the room for cleaning.                         | High     | Open   |
| FR-015 | Record Payment               | As a receptionist, I want to record that a guest paid the total price at check-out so that the revenue of the stay is known.                                                   | High     | Open   |
| FR-016 | Record No-Show               | As a receptionist, I want to mark a reservation as no-show when the guest did not arrive by the end of the arrival day so that the room is released for the following nights.  | Medium   | Open   |
| FR-017 | View Room Overview           | As a receptionist, I want to see for each room whether it is occupied and its room status so that I know which rooms I can assign.                                              | High     | Open   |
| FR-018 | View Cleaning List           | As a housekeeper, I want to see the list of rooms with room status Dirty so that I know which rooms to clean.                                                                  | High     | Open   |
| FR-019 | Mark Room Clean              | As a housekeeper, I want to set the room status of a room to Clean after cleaning it so that the receptionist can assign it to the next guest.                                 | High     | Open   |
| FR-020 | Report Room Defect           | As a housekeeper, I want to report a defect in a room with a short description so that the manager can arrange the repair.                                                     | Low      | Open   |
| FR-021 | Define Room Category         | As a manager, I want to create a room category with a name, a maximum number of persons, and a nightly price so that guests can reserve it.                                     | High     | Open   |
| FR-022 | Change Nightly Price         | As a manager, I want to change the nightly price of a room category so that new reservations use the current price.                                                            | High     | Open   |
| FR-023 | Define Room                  | As a manager, I want to create a room with its room number and room category so that it can be reserved and assigned.                                                          | High     | Open   |
| FR-024 | Set Room Out of Service      | As a manager, I want to set a room out of service for a date range so that it is not counted as available while it is being repaired.                                          | Medium   | Open   |
| FR-025 | View Occupancy Report        | As a manager, I want to see the occupancy rate per month so that I can judge how well the hotel is used.                                                                       | Medium   | Open   |
| FR-026 | View Revenue Report          | As a manager, I want to see the revenue per month and per room category so that I can review the hotel's earnings.                                                               | Medium   | Open   |
| FR-027 | Manage Staff Members         | As a manager, I want to create a staff member account with the role Receptionist, Housekeeper, or Manager so that each staff member can sign in with the functions of their role.   | High     | Open   |
| FR-028 | Deactivate Staff Member      | As a manager, I want to deactivate a staff member account so that a staff member who left the hotel can no longer sign in.                                                                     | High     | Open   |

## Non-Functional Requirements

| ID      | Title                  | Requirement                                                                                                                               | Category        | Priority | Status |
|---------|------------------------|-------------------------------------------------------------------------------------------------------------------------------------------|-----------------|----------|--------|
| NFR-001 | Response Time          | 95% of server requests (page navigations, availability searches) must be answered within 500 milliseconds, measured at the server. | Performance     | High     | Open   |
| NFR-002 | Concurrent Sessions    | With 50 simultaneous sessions of guests and staff members, the system must still meet the response time of NFR-001. | Scalability     | Medium   | Open   |
| NFR-003 | Availability           | Online reservation (FR-001 to FR-005) must be available 99.5% of the time per calendar month, excluding downtimes announced 48 hours in advance.   | Availability    | High     | Open   |
| NFR-004 | Data Backup            | The database must be backed up at least once every hour (at most 1 hour of data lost), and a restore must be completed within 2 hours. | Availability    | High     | Open   |
| NFR-005 | Encrypted Transport    | All traffic between browser and server must use HTTPS with TLS 1.2 or higher; plain HTTP requests must be redirected to HTTPS.          | Security        | High     | Open   |
| NFR-006 | Staff Authentication   | Every function except FR-001 to FR-005 must require a staff member to sign in with a username and a password of at least 12 characters. | Security        | High     | Open   |
| NFR-007 | Role-Based Access      | A staff member must reach only the functions assigned to their role: a Receptionist and a Housekeeper reach the functions whose user story names their role, a Manager reaches all staff functions. 100% of role/function combinations must be verified by tests. | Security        | High     | Open   |
| NFR-008 | Guest Access Check     | A guest must reach a reservation only with the matching reservation number and email address. Reservation numbers must be random, non-sequential, and at least 8 characters long. After 5 failed attempts for one reservation number within 15 minutes, further attempts for it must be refused for 15 minutes. | Security        | High     | Open   |
| NFR-009 | Change Log             | Every creation, change, and cancellation of a reservation, every check-in, check-out, recorded payment, and no-show, every change of guest registration data, and every creation and deactivation of a staff member account must be logged with timestamp and staff member (or "guest"), kept for 12 months. | Security        | Medium   | Open   |
| NFR-010 | Accessibility          | The pages for FR-001 to FR-005 must conform to WCAG 2.1 level AA.                                                                         | Usability       | Medium   | Open   |
| NFR-011 | Mobile Layout          | All pages must be usable without horizontal scrolling on screens from 360 px width, so housekeepers can work on a phone.                 | Usability       | High     | Open   |
| NFR-012 | Languages              | All pages and the confirmation email must be available in German and English.                                                   | Usability       | Medium   | Open   |
| NFR-013 | Browser Support        | All pages must work in the latest two versions of Chrome, Firefox, Safari, and Edge on the desktop, and of Safari on iOS and Chrome on Android. | Portability     | High     | Open   |
| NFR-014 | Email Resilience       | A reservation must succeed even when the SMTP server of C-007 is unavailable; the confirmation email must then be retried for at least 24 hours. | Reliability     | Medium   | Open   |

## Constraints

| ID    | Title                   | Constraint                                                                                                                         | Category    | Source                     | Priority | Status |
|-------|-------------------------|------------------------------------------------------------------------------------------------------------------------------------|-------------|----------------------------|----------|--------|
| C-001 | Runtime Platform        | The backend must run on Java 25 with Spring Boot 4.                                                                                | Technical   |                            | High     | Open   |
| C-002 | UI Framework            | All pages must be built with Vaadin 25.                                                                                   | Technical   |                            | High     | Open   |
| C-003 | Data Access             | Database access must use jOOQ, and every schema change must be a Flyway migration.                                                 | Technical   |                            | High     | Open   |
| C-004 | Single Hotel            | The system serves exactly one hotel with ten rooms; multiple properties are out of scope.                                          | Business    |                            | High     | Open   |
| C-005 | No Online Payment       | Payment is out of scope: the guest pays at the hotel, and the system neither processes nor stores card data.                       | Business    |                            | High     | Open   |
| C-006 | No Guest Account        | Guests reserve without an account; the reservation number and email address identify a reservation.                                | Business    |                            | High     | Open   |
| C-007 | Email Delivery          | Confirmation emails must be sent through an SMTP server provided by the hotel.                                                     | Operational |                            | Medium   | Open   |
| C-008 | Information Duty        | Before a guest submits a reservation, the system must show which personal data is collected, for which purpose, and who is responsible. | Regulatory | nDSG Art. 19             | High     | Open   |
| C-009 | Right of Access         | On request, the manager must be able to export all personal data stored about a guest, so it can be handed over within 30 days.  | Regulatory  | nDSG Art. 25               | High     | Open   |
| C-010 | Data Minimization       | Personal data of a guest must be anonymized 24 months after the departure date; the amounts of stays and payments are kept for 10 years without personal data. | Regulatory  | nDSG Art. 6(4), OR Art. 958f | High     | Open   |
| C-011 | Data Security           | Personal data must be protected against unauthorized access at least by the measures of NFR-005 to NFR-009.| Regulatory  | nDSG Art. 8                | High     | Open   |
| C-012 | Database                | The system stores its data in an H2 database in file mode; no separate database server is operated. | Technical   |                            | High     | Open   |
