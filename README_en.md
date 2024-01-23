### Prepared project
This project contains a set of elements related to the "airline booking" area:
- Airline, Airport, Flight, and Ticket entities.
- Spring bean TicketService with implemented methods for:
    - searchFlights() - time-consuming operation for flights search.
    - saveTicket() - ticket booking and saving.

### Task
In this homework assignment, you will create the application's user interface: list and detail views of data, as well as ticket search, booking, and display screens.

#### 1. Airline and Airport reference data screens
Create new top level menu item - "Reference data" (edit menu.xml file in Studio).
- Use the Studio main screen designer.
- Set a suitable icon for the created menu item.

Create the Airline CRUD views:
- User Master-detail view template.
- Place the screen inside the Reference data menu.

Create the Airport CRUD views:
- Use Entity list and detail views template to create both views at once.
- Place created list view screen inside the Reference data menu.
- The editor screen (detail view) should always open as a modal dialog. There should be no unnecessary empty space in the dialogue.

#### 2. Flight data screens
Create the Flight CRUD screens:
- Use the Entity list and detail views template again.
- Place the list view inside the Reference data menu.
- Both screens should display related attributes: fromAirport, toAirport, Airlines.

Set up the editor screen:
- Should be opened as a modal dialog.
- Change airline selecting component to other - EntityComboBox with items fetch callback (entitySuggestionField Flex analog).
- The options displayed for selection must be selected using the "name contains" criterion, not case sensitive.

Implement a simple static filter element in the Flights List View:
- Delete a scaffolded generic Filter.
- Instead, add a horizontal GroupBox and fill it with filter fields:
    - Airport - airports list, EntityComboBox.
    - Take off from - date-time field.
    - Take off to - data-time field.
- Implement the filter's logic just inside view's XML file using jpql <condition/> in flights loader. Filtering rules:
    - "Airport" value must be the same as in suitable flight "From Airport" OR "To Airport" attribute
    - "Take off date" of the flight >= than "Take off from" value
    - "Take off date" of the flight < than "Take off to" value
- Connect the filter fields with data loader declaratively - using <dataLoadCoordinator/> facet.
Filtering should start immediately after changing any filter field value.

#### 3. Flights searching screen 
Create a screen for tickets (fights) searching and booking - "Ticket reservation".
- Use Blank view template.
- Place the screen at the top level of the menu.
- Set a suitable icon for the screen's menu item.

Screen contains multi-field user filter, flights table, and progress bar indicator.

Filter requirements:
- Placed at the top of the screen across the entire width. Implemented as a GroupBox containing three fields and a button. Group Label - Filter. Fields:
  - From - airports list, EntityComboBox.
  - To - airports list, EntityComboBox.
  - Departure date - date field. Use suitable component that shows date only.
  - Search button—starts the search process. Select the appropriate button icon.

Flights table:
- Displays filtered flights.
- Columns: Number, From airport, To airport, Airline, Take off date.
- Does not contain CRUD operations.
- Occupies all free space horizontally and vertically.
  
Table data loading:
- Logic is already implemented in a service bean - TicketService#searchFlights(). Call this method from the screen.
- The method takes some time to find flights, so use BackgroundTask to load the data.
- Loading should be started after Search button click.
- Before starting the data loading, check if at least one field is filled in.
- If no - show warning notification - "Please fill at least one filter field".
- Add an undefined progress bar component to the screen. For example, place it between the filter and the table and let it fill the entire width.
-  Show the bar while data is being loaded and hide in another cases.

#### 4. Ticket booking and view interfaces.
Create the Ticket view screen:
- Use detail view template.
- On-screen editing should be disabled.
- Show all ticket's fields: reservationId, passportNumber, passengerName, telephone.
- In addition, show flight's fields as well: number, airline, fromAirport, toAirport, takeOffDate (use data instanceContainer).

Add a ticket reservation action to the TicketReservation screen as follows:
- Action must be accessible as generated column (Renderer) in the flights table.
- Column header - "Actions".
- Button content - a link (Button) with the text “Reserve”, when clicked, the booking process begins.

Ticket booking steps order:
- Show a dialog to fill additional ticket information (user data).
- Create a Ticket, fill all fields and save it to the database using TicketService#saveTicket() method.
- After that, open the view screen for the created ticket. The screen should open in a new tab.

Ticket's additional fields dialog:
- Instead of creating standard view, add an InputDialog to the current screen and use it (dialogs.createInputDialog()).
- Dialog's header - "Reserve ticket"
- Buttons set: OK and Cancel.
- Parameters: Passenger name, Passport number, Telephone. All of them are text fields and are required.
- If dialog is closed with Cancel outcome - interrupt the ticket booking process.
- If the outcome is “OK”, fill in the entered values ​​into the created ticket instance.
