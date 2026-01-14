# PRD: Business Central Event Registration Management Extension

## Document Metadata
- **Version**: 1.0
- **Date**: 2025-01-14
- **Target Platform**: Microsoft Dynamics 365 Business Central
- **Language**: AL (Application Language)
- **Minimum BC Version**: 23.0

---

## 1. Executive Summary

### 1.1 Purpose
Develop a Business Central extension to import, store, visualize, and expose via API event registration data exported from Eventbrite. The extension enables AI agents and external systems to query attendee information for badges, communications, check-in, and reporting.

### 1.2 Scope
- Data structure for event registrations (Eventbrite format)
- Excel import functionality
- User interface (List and Card pages)
- RESTful APIs for external consumption

### 1.3 Out of Scope
- Direct Eventbrite API integration
- Automatic synchronization
- Check-in management
- Badge generation within BC

---

## 2. Source Data Analysis

### 2.1 Eventbrite Export Format
The Excel export contains 34 columns organized in these categories:

**Order Information:**
- Order ID, Order date

**Buyer Information:**
- Buyer first name, Buyer last name, Buyer email, Phone number

**Location Information:**
- Purchaser city, Purchaser state, Purchaser country
- Billing zip code, Billing country

**Event Information:**
- Event name, Event ID, Event start date, Event start time
- Event timezone, Event location

**Ticket Information:**
- Ticket quantity, Add-ons quantity

**Financial Information:**
- Currency, Payment status, Payment type, Payment details
- Gross sales, Eventbrite service fee, Eventbrite payment processing fee
- Eventbrite tax, Organizer tax, Royalty
- Ticket revenue, Add-ons revenue, Ticket + add-ons revenue, Net sales

**Other:**
- Guest (Yes/No)

---

## 3. Technical Specifications

### 3.1 Object ID Range
```
Starting ID: 50200
Reserved Range: 50200-50299
```

### 3.2 Table Definitions

#### 3.2.1 Table: VSS Event (50200)

Master table for events (to support multiple events).

| Field No | Field Name | Data Type | Length | Description |
|----------|------------|-----------|--------|-------------|
| 1 | Code | Code | 20 | Primary Key |
| 2 | Name | Text | 100 | Event name |
| 3 | External Event ID | Text | 50 | Eventbrite Event ID |
| 4 | Event Date | Date | - | Event start date |
| 5 | Event Time | Time | - | Event start time |
| 6 | Timezone | Text | 50 | Event timezone |
| 7 | Location | Text | 100 | Event location/venue |
| 8 | Active | Boolean | - | Enable/disable event |

**Keys:**
- Primary Key: Code
- Key 2: External Event ID

---

#### 3.2.2 Table: VSS Event Registration (50201)

Main table storing registration/order data from Eventbrite.

| Field No | Field Name | Data Type | Length | Description |
|----------|------------|-----------|--------|-------------|
| 1 | Entry No. | Integer | - | Primary Key (AutoIncrement) |
| 2 | Order ID | Text | 50 | Eventbrite Order ID |
| 3 | Order DateTime | DateTime | - | Order date and time |
| 4 | Event Code | Code | 20 | FK to VSS Event |
| 10 | Buyer First Name | Text | 50 | First name |
| 11 | Buyer Last Name | Text | 50 | Last name |
| 12 | Buyer Full Name | Text | 100 | Calculated: First + Last |
| 13 | Buyer Email | Text | 80 | Email address |
| 14 | Phone Number | Text | 30 | Phone |
| 20 | City | Text | 50 | Purchaser city |
| 21 | State | Text | 30 | Purchaser state/province |
| 22 | Country Code | Code | 10 | Purchaser country |
| 23 | Billing Zip Code | Text | 20 | Billing postal code |
| 24 | Billing Country | Code | 10 | Billing country |
| 30 | Event Name | Text | 100 | Event name (denormalized) |
| 31 | External Event ID | Text | 50 | Eventbrite Event ID |
| 32 | Event Start Date | Date | - | Event date |
| 33 | Event Start Time | Time | - | Event time |
| 34 | Event Timezone | Text | 50 | Timezone |
| 35 | Event Location | Text | 100 | Venue |
| 40 | Ticket Quantity | Integer | - | Number of tickets |
| 41 | Addons Quantity | Integer | - | Number of add-ons |
| 50 | Currency Code | Code | 10 | Transaction currency |
| 51 | Payment Status | Text | 30 | Payment status |
| 52 | Payment Type | Text | 30 | Payment method |
| 53 | Payment Details | Text | 100 | Additional payment info |
| 60 | Gross Sales | Decimal | - | Gross sales amount |
| 61 | Service Fee | Decimal | - | Eventbrite service fee |
| 62 | Processing Fee | Decimal | - | Payment processing fee |
| 63 | Eventbrite Tax | Decimal | - | Platform tax |
| 64 | Organizer Tax | Decimal | - | Organizer tax |
| 65 | Royalty | Decimal | - | Royalty amount |
| 66 | Ticket Revenue | Decimal | - | Ticket revenue |
| 67 | Addons Revenue | Decimal | - | Add-ons revenue |
| 68 | Total Revenue | Decimal | - | Ticket + add-ons |
| 69 | Net Sales | Decimal | - | Net sales amount |
| 80 | Is Guest | Boolean | - | Guest flag |
| 90 | Imported DateTime | DateTime | - | When imported to BC |
| 91 | Imported By User ID | Code | 50 | Who imported |

**Keys:**
- Primary Key: Entry No.
- Key 2: Order ID (Unique)
- Key 3: Event Code
- Key 4: Buyer Email
- Key 5: City, Country Code
- Key 6: Order DateTime

**Field Groups:**
- DropDown: Entry No., Buyer Full Name, Buyer Email, Ticket Quantity
- Brick: Buyer Full Name, Buyer Email, City, Ticket Quantity

---

### 3.3 Page Definitions

#### 3.3.1 Page: VSS Events (50200)
- **PageType**: List
- **SourceTable**: VSS Event
- **ApplicationArea**: All
- **UsageCategory**: Lists
- **CardPageId**: VSS Event Card

**Fields to display:**
- Code
- Name
- Event Date
- Event Time
- Location
- Active

---

#### 3.3.2 Page: VSS Event Card (50201)
- **PageType**: Card
- **SourceTable**: VSS Event

**Layout:**
- General FastTab: All fields

---

#### 3.3.3 Page: VSS Event Registrations (50202)
- **PageType**: List
- **SourceTable**: VSS Event Registration
- **ApplicationArea**: All
- **UsageCategory**: Lists
- **CardPageId**: VSS Event Registration Card
- **Editable**: false

**Fields to display:**
- Entry No.
- Order ID
- Buyer Full Name
- Buyer Email
- City
- Country Code
- Ticket Quantity
- Payment Status
- Order DateTime

**Actions:**
- Process group:
  - Import from Excel (calls import codeunit)
- Navigate group:
  - Event Card

**Views (Saved filters):**
- By Event: Group by Event Code
- By City: Group by City
- Multiple Tickets: Ticket Quantity > 1

---

#### 3.3.4 Page: VSS Event Registration Card (50203)
- **PageType**: Card
- **SourceTable**: VSS Event Registration
- **Editable**: false

**Layout:**

**Order Information FastTab:**
- Entry No.
- Order ID
- Order DateTime
- Event Code

**Buyer Information FastTab:**
- Buyer First Name
- Buyer Last Name
- Buyer Full Name
- Buyer Email
- Phone Number

**Location FastTab:**
- City
- State
- Country Code
- Billing Zip Code
- Billing Country

**Event Details FastTab:**
- Event Name
- Event Start Date
- Event Start Time
- Event Location
- Event Timezone

**Tickets FastTab:**
- Ticket Quantity
- Addons Quantity
- Is Guest

**Financial FastTab:**
- Currency Code
- Payment Status
- Payment Type
- Gross Sales
- Service Fee
- Processing Fee
- Net Sales

**Import Info FastTab:**
- Imported DateTime
- Imported By User ID

---

#### 3.3.5 Page: VSS Import Event Registrations (50204)
- **PageType**: StandardDialog
- **Purpose**: Dialog for Excel import

**Layout:**
- File Selection field
- Event Code selection (optional, for manual assignment)
- Import Options:
  - Update Existing (Boolean)
  - Skip Duplicates (Boolean)

**Actions:**
- Import

---

### 3.4 Codeunit: VSS Event Registration Mgmt (50200)

**Main Procedures:**

```al
procedure ImportFromExcel(EventCode: Code[20]; UpdateExisting: Boolean): Integer
// Opens file dialog, processes Excel, returns count of imported records
// Parameters:
//   EventCode: Optional, assign all records to this event
//   UpdateExisting: If true, update existing Order IDs; if false, skip duplicates
// Returns: Number of records imported

procedure ProcessExcelBuffer(var TempExcelBuffer: Record "Excel Buffer" temporary; EventCode: Code[20]; UpdateExisting: Boolean): Integer
// Processes Excel Buffer and creates/updates registration records

procedure MapExcelRowToRegistration(var TempExcelBuffer: Record "Excel Buffer" temporary; RowNo: Integer; var Registration: Record "VSS Event Registration")
// Maps a single Excel row to registration record fields

procedure FindOrCreateEvent(EventName: Text[100]; EventID: Text[50]; EventDate: Date; EventTime: Time; Location: Text[100]): Code[20]
// Finds existing event by External ID or creates new one
// Returns Event Code

procedure GetColumnMapping(): Dictionary of [Text, Integer]
// Returns mapping of field names to Excel column numbers
// Handles Eventbrite column naming

procedure ValidateRegistration(var Registration: Record "VSS Event Registration"): Boolean
// Validates required fields before insert

procedure GetRegistrationByOrderID(OrderID: Text[50]; var Registration: Record "VSS Event Registration"): Boolean
// Finds registration by Eventbrite Order ID

procedure GetRegistrationsByEmail(Email: Text[80]; var Registration: Record "VSS Event Registration"): Boolean
// Finds registrations by email (may return multiple)

procedure GetRegistrationsByEvent(EventCode: Code[20]; var Registration: Record "VSS Event Registration")
// Filters registrations by event

procedure GetEventStatistics(EventCode: Code[20]; var TotalRegistrations: Integer; var TotalTickets: Integer; var TotalRevenue: Decimal)
// Returns summary statistics for an event
```

**Helper Procedures:**

```al
local procedure ParseDateTime(DateTimeText: Text): DateTime
// Parses Eventbrite datetime format "YYYY-MM-DD HH:MM:SS"

local procedure ParseDate(DateText: Text): Date
// Parses date from "YYYY-MM-DD"

local procedure ParseTime(TimeText: Text): Time
// Parses time from "HH:MM:SS"

local procedure CleanText(InputText: Text): Text
// Removes extra spaces, trims

local procedure BuildFullName(FirstName: Text; LastName: Text): Text[100]
// Combines first and last name
```

---

### 3.5 Excel Column Mapping

```al
// Eventbrite Export Column Names -> BC Field mapping
procedure GetColumnHeaders(): List of [Text]
begin
    exit(List.Of(
        'Order ID',
        'Order date',
        'Buyer first name',
        'Buyer last name',
        'Buyer email',
        'Phone number',
        'Purchaser city',
        'Purchaser state',
        'Purchaser country',
        'Billing zip code',
        'Billing country',
        'Event name',
        'Event ID',
        'Event start date',
        'Event start time',
        'Event timezone',
        'Event location',
        'Ticket quantity',
        'Add-ons quantity',
        'Currency',
        'Payment status',
        'Payment type',
        'Payment details',
        'Gross sales',
        'Eventbrite service fee',
        'Eventbrite payment processing fee',
        'Eventbrite tax',
        'Organizer tax',
        'Royalty',
        'Ticket revenue',
        'Add-ons revenue',
        'Ticket + add-ons revenue',
        'Net sales',
        'Guest'
    ));
end;
```

---

### 3.6 API Definitions

#### 3.6.1 API Page: VSS Event Registrations API (50205)

```al
page 50205 "VSS Event Registrations API"
{
    PageType = API;
    APIPublisher = 'vssistemas';
    APIGroup = 'eventManagement';
    APIVersion = 'v1.0';
    EntityName = 'eventRegistration';
    EntitySetName = 'eventRegistrations';
    SourceTable = "VSS Event Registration";
    DelayedInsert = false;
    InsertAllowed = false;
    ModifyAllowed = false;
    DeleteAllowed = false;
    ODataKeyFields = "Entry No.";
    
    layout
    {
        area(Content)
        {
            repeater(Group)
            {
                field(entryNo; Rec."Entry No.") { }
                field(orderId; Rec."Order ID") { }
                field(orderDateTime; Rec."Order DateTime") { }
                field(eventCode; Rec."Event Code") { }
                field(buyerFirstName; Rec."Buyer First Name") { }
                field(buyerLastName; Rec."Buyer Last Name") { }
                field(buyerFullName; Rec."Buyer Full Name") { }
                field(buyerEmail; Rec."Buyer Email") { }
                field(phoneNumber; Rec."Phone Number") { }
                field(city; Rec.City) { }
                field(state; Rec.State) { }
                field(countryCode; Rec."Country Code") { }
                field(eventName; Rec."Event Name") { }
                field(eventStartDate; Rec."Event Start Date") { }
                field(eventLocation; Rec."Event Location") { }
                field(ticketQuantity; Rec."Ticket Quantity") { }
                field(addonsQuantity; Rec."Addons Quantity") { }
                field(currencyCode; Rec."Currency Code") { }
                field(paymentStatus; Rec."Payment Status") { }
                field(grossSales; Rec."Gross Sales") { }
                field(netSales; Rec."Net Sales") { }
                field(isGuest; Rec."Is Guest") { }
            }
        }
    }
}
```

**API Endpoint:**
```
GET: /api/vssistemas/eventManagement/v1.0/eventRegistrations
```

**Common Filters:**
```
# All registrations for an event
$filter=eventCode eq 'WINTERFEST2026'

# Search by email
$filter=buyerEmail eq 'john@example.com'

# Registrations from a specific city
$filter=city eq 'Valencia'

# Multiple tickets only
$filter=ticketQuantity gt 1

# Combine filters
$filter=eventCode eq 'WINTERFEST2026' and city eq 'Madrid'
```

---

#### 3.6.2 API Page: VSS Events API (50206)

```al
page 50206 "VSS Events API"
{
    PageType = API;
    APIPublisher = 'vssistemas';
    APIGroup = 'eventManagement';
    APIVersion = 'v1.0';
    EntityName = 'event';
    EntitySetName = 'events';
    SourceTable = "VSS Event";
    DelayedInsert = true;
    ODataKeyFields = Code;
    
    layout
    {
        area(Content)
        {
            repeater(Group)
            {
                field(code; Rec.Code) { }
                field(name; Rec.Name) { }
                field(externalEventId; Rec."External Event ID") { }
                field(eventDate; Rec."Event Date") { }
                field(eventTime; Rec."Event Time") { }
                field(timezone; Rec.Timezone) { }
                field(location; Rec.Location) { }
                field(active; Rec.Active) { }
            }
        }
    }
}
```

**API Endpoint:**
```
GET: /api/vssistemas/eventManagement/v1.0/events
```

---

### 3.7 Permission Sets

#### 3.7.1 VSS Event Registration - All (50200)
```al
permissionset 50200 "VSS Event Reg - All"
{
    Caption = 'Event Registration - Full Access';
    Assignable = true;
    
    Permissions =
        table "VSS Event" = X,
        table "VSS Event Registration" = X,
        tabledata "VSS Event" = RIMD,
        tabledata "VSS Event Registration" = RIMD,
        page "VSS Events" = X,
        page "VSS Event Card" = X,
        page "VSS Event Registrations" = X,
        page "VSS Event Registration Card" = X,
        page "VSS Import Event Registrations" = X,
        page "VSS Event Registrations API" = X,
        page "VSS Events API" = X,
        codeunit "VSS Event Registration Mgmt" = X;
}
```

#### 3.7.2 VSS Event Registration - View (50201)
```al
permissionset 50201 "VSS Event Reg - View"
{
    Caption = 'Event Registration - View Only';
    Assignable = true;
    
    Permissions =
        table "VSS Event" = X,
        table "VSS Event Registration" = X,
        tabledata "VSS Event" = R,
        tabledata "VSS Event Registration" = R,
        page "VSS Events" = X,
        page "VSS Event Card" = X,
        page "VSS Event Registrations" = X,
        page "VSS Event Registration Card" = X,
        page "VSS Event Registrations API" = X,
        page "VSS Events API" = X;
}
```

---

## 4. Business Rules

### 4.1 Import Rules
- Order ID is unique identifier - no duplicates allowed unless UpdateExisting = true
- If Event Code not specified, auto-create/find event from Excel data
- Buyer Full Name auto-calculated from First Name + Last Name
- Imported DateTime and User auto-populated on import
- Empty rows in Excel are skipped

### 4.2 Data Validation
- Order ID: Required, must be unique
- Buyer Email: Required for valid registration
- Ticket Quantity: Default to 1 if empty or invalid

### 4.3 Event Auto-Creation
When importing, if no Event Code specified:
1. Check if event exists by External Event ID
2. If not found, create new event from Excel data
3. Assign Event Code to all imported registrations

---

## 5. App Manifest (app.json)

```json
{
  "id": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
  "name": "VS Event Registration Management",
  "publisher": "VS Sistemas",
  "version": "1.0.0.0",
  "brief": "Event Registration Import and API for Business Central",
  "description": "Import Eventbrite registrations, visualize in BC, and expose via API for agents and external systems.",
  "privacyStatement": "https://vssistemas.com/privacy",
  "EULA": "https://vssistemas.com/eula",
  "help": "https://vssistemas.com/help/event-registration",
  "url": "https://vssistemas.com",
  "logo": "./res/logo.png",
  "platform": "23.0.0.0",
  "application": "23.0.0.0",
  "idRanges": [
    {
      "from": 50200,
      "to": 50299
    }
  ],
  "resourceExposurePolicy": {
    "allowDebugging": true,
    "allowDownloadingSource": false,
    "includeSourceInSymbolFile": true
  },
  "runtime": "12.0",
  "features": [
    "TranslationFile"
  ],
  "target": "Cloud"
}
```

---

## 6. File Structure

```
/src
  /table
    Tab50200.VSSEvent.al
    Tab50201.VSSEventRegistration.al
  /page
    Pag50200.VSSEvents.al
    Pag50201.VSSEventCard.al
    Pag50202.VSSEventRegistrations.al
    Pag50203.VSSEventRegistrationCard.al
    Pag50204.VSSImportEventRegistrations.al
  /api
    Pag50205.VSSEventRegistrationsAPI.al
    Pag50206.VSSEventsAPI.al
  /codeunit
    Cod50200.VSSEventRegistrationMgmt.al
  /permissionset
    PermSet50200.VSSEventRegAll.al
    PermSet50201.VSSEventRegView.al
/res
  logo.png
/translations
  VSSEventRegistration.g.xlf
app.json
```

---

## 7. API Usage Examples for Agents

### 7.1 Get All Registrations for Winter Fest
```http
GET /api/vssistemas/eventManagement/v1.0/eventRegistrations?$filter=eventCode eq 'WINTERFEST2026'&$orderby=buyerLastName
```

### 7.2 Search Registration by Email (Check-in)
```http
GET /api/vssistemas/eventManagement/v1.0/eventRegistrations?$filter=buyerEmail eq 'attendee@company.com'
```

### 7.3 Get Registrations from Valencia
```http
GET /api/vssistemas/eventManagement/v1.0/eventRegistrations?$filter=city eq 'Valencia'
```

### 7.4 Get Attendees with Multiple Tickets
```http
GET /api/vssistemas/eventManagement/v1.0/eventRegistrations?$filter=ticketQuantity gt 1
```

### 7.5 Get Only Names and Emails (for badges)
```http
GET /api/vssistemas/eventManagement/v1.0/eventRegistrations?$select=buyerFullName,buyerEmail,city,ticketQuantity&$filter=eventCode eq 'WINTERFEST2026'
```

### 7.6 Count Registrations by Event
```http
GET /api/vssistemas/eventManagement/v1.0/eventRegistrations?$filter=eventCode eq 'WINTERFEST2026'&$count=true
```

### 7.7 Get List of Events
```http
GET /api/vssistemas/eventManagement/v1.0/events?$filter=active eq true
```

---

## 8. Testing Checklist

### 8.1 Import Tests
- [ ] Import Excel with all 34 columns
- [ ] Import with missing optional columns
- [ ] Import with duplicate Order IDs (skip mode)
- [ ] Import with duplicate Order IDs (update mode)
- [ ] Auto-create event from import
- [ ] Assign to existing event on import
- [ ] Handle empty rows
- [ ] Handle special characters in names/cities

### 8.2 Page Tests
- [ ] Registration List loads correctly
- [ ] Registration Card displays all fields
- [ ] Event List and Card work
- [ ] Import dialog opens and processes
- [ ] Filters and views work

### 8.3 API Tests
- [ ] GET all registrations
- [ ] GET with $filter by event
- [ ] GET with $filter by email
- [ ] GET with $filter by city
- [ ] GET with $select (limited fields)
- [ ] GET with $orderby
- [ ] GET events list
- [ ] Verify read-only (no POST/PATCH/DELETE)

---

## 9. Sample Data Reference

Based on the Winter Fest Excel:

| Field | Sample Value |
|-------|--------------|
| Order ID | 13793568833 |
| Order date | 2025-11-23 13:36:54 |
| Buyer first name | (varies) |
| Buyer last name | (varies) |
| Event name | Business Central & Agents Winter Fest |
| Event start date | 2026-01-17 |
| Event start time | 09:30:00 |
| Event location | AZZ Valencia Congress Hotel & SPA |
| Ticket quantity | 1-3 (mostly 1) |
| Currency | EUR |
| Payment status | Free Order |
| Countries | ES, AD |
| Cities | Valencia, Madrid, Barcelona, etc. |

**Total Records:** 36 registrations

---

## 10. Future Enhancements (Phase 2)

- Direct Eventbrite API integration
- Automatic periodic sync
- Check-in tracking within BC
- Badge generation/printing
- Email communications
- Attendee portal
- Power BI integration
- Multi-currency support

---

## Appendix A: Naming Conventions

| Object Type | Prefix | Example |
|-------------|--------|---------|
| Table | VSS | VSS Event Registration |
| Page | VSS | VSS Event Registrations |
| Codeunit | VSS | VSS Event Registration Mgmt |
| API Page | VSS | VSS Event Registrations API |

---

*End of Document*
