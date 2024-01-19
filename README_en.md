### Prepared project
This project contains a list of elements related to flight tickets booking area:
- Airline, Airport, Flight, and Ticket entities.
- Spring bean TicketService with implemented methods for:
    - searchFlights() - time-consuming operation for flights search.
    - saveTicket() - ticket booking and saving.

### Task
In this homework you need to create application's user interface: list and detail views for data, as well as searching, booking and showing screens.

#### 1. Airline and Airport reference data screens
Create new top level menu item - "Reference data" (use menu.xml in Studio).
- Use Studio main screen designer.
- Select and add a suitable icon for created menu.

Create Airline CRUD views:
- User Master-detail view template.
- Put the screen inside Reference data menu.
- In the screen set "iataCode" text field to transform to upper case automatically.

Create Airport CRUD views:
- Use Entity list and detail views template to create both views at once.
- Put the list view screen to Reference data menu.
- Editor screen (detail view) should always be opened as a modal dialog. Dialog should not have any redundant empty space.
- In the editor screen text field "code" must be converted to upper case, too.

#### 2. Flight data screens
Create Flight CRUD screens:
- Use the Entity list and detail views template again.
- Put the list view to the Reference data menu.
- Both screens must have linked attributes: fromAirport, toAirport, airline.

Set up the editor screen:
- Should be opened as modal dialog.
- Regroup the form - add second column and move there two fields: fromAirport и toAirport.
- Change airline selecting component to other - EntityComboBox with items fetch callback (entitySuggestionField Flex analog).
  Options to show must be selected by "name contains" criteria, case-insensitive.

Implement a simple static filter element in Flights List View:
- Delete scaffolded generic Filter .
- Instead of it add horizontal GroupBox and fill it with filter fields:
    - Airport - airports list, EntityComboBox.
    - Take off from - date-time field.
    - Take off to - data-time field.
- Implement the filter's logic just inside view's XML file using jpql <condition/> in flights loader. Filtering rules:
    - "Airport" value must be the same as in suitable flight "From Airport" OR "To Airport" attribute
    - "Take off date" of the flight >= than "Take off from" value
    - "Take off date" of the flight < than "Take off to" value
- Connect the filter fields with data loader declaratively - using dataLoadCoordinator facet.
  Filtering must be triggered immediately after any of filter field changes value.

#### 3. Flights searching screen 
Create a screen to tickets (fights) searching and tickets booking - "Ticket reservation".
- Use Blank view template.
- Put the screen on the top menu level.
- Add a suitable icon for the screen's menu item.

Screen contains multi-field user filter, flights table, and progress bar indicator.

Filter's requirements:
- Placed at the top of the screen with full width. Realised as a GroupBox, that contains three fields and a button. Group label - Filter. Fields:
- From - airports list, EntityComboBox.
- To - airports list, EntityComboBox.
- Departure date - date field. Use suitable component that shows date only.
- Search button - launches the search process. Select button's obvious icon.

Flights table:
- Shows filtered flights.
- Columns: Number, From airport, To airport, Airline, Take off date.
- Does not contain CRUD operations.
- Takes all horizontal and vertical free space.

Table data loading:
- Logic is already implemented in service bean - TicketService#searchFlights(). Call this method from screen.
- Method takes some time to find the flights, so use a BackgroundTask to deal with data loading.
- Loading should be started after Search button click.
- Before loading check, that at least one field is filled with data.
  If no - then show warning notification - "Please fill at least one filter field".
- Add to the screen progress bar component, undefined. For example, place it between filter and table, and let it take full width.
  Show the bar while data is being loaded and hide in another cases.

#### 4. Ticket booking and view interfaces.
Create Ticket view screen:
- Use detail view template.
- Editing on the screen must be turned off.
- Show all ticket's fields: reservationId, passportNumber, passengerName, telephone.
- In addition, show flight's fields as well: number, airline, fromAirport, toAirport, takeOffDate (use data instanceContainer).

Add to the TicketReservation screen ticket booking action:
- Action must be accessible as generated column (Renderer) in the flights table.
- Column header - "Actions".
- Button's content - link (Button) with "Reserve" text, when clicked - started booking process.

Ticket booking actions order:
- Show a dialog to fill additional ticket information (user's data).
- Create a Ticket, fill all fields and save it to database using TicketService#saveTicket() method.
- After this open ticket screen to show created ticket. Should be opened in the new tab.

Ticket's additional fields dialog:
- Instead of creating standard view, add an InputDialog to current screen and use it (dialogs.createInputDialog()).
- Dialog's header - "Reserve ticket"
- Buttons set: OK and Cancel.
- Parameters: Passenger name, Passport number, Telephone. All of them are text fields and required.
- If dialog is closed with Cancel outcome - interrupt the ticket booking process.
- Fill inputted values into created Ticket instance - if outcome is OK.