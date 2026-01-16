# PRD: Business Central Equipment Loan Management Extension

## Document Metadata
- **Version**: 1.0
- **Date**: 2025-01-16
- **Target Platform**: Microsoft Dynamics 365 Business Central
- **Language**: AL (Application Language)
- **Minimum BC Version**: 23.0

---

## 1. Executive Summary

### 1.1 Purpose
Develop a simple Business Central extension to manage equipment loans (laptops, projectors, cameras) to employees and external users. Track availability, who has what, and loan history.

### 1.2 Scope
- Equipment catalog with types and statuses
- Loan registration and return
- Simple UI for IT/Admin staff

### 1.3 Out of Scope
- Barcode/QR scanning
- Advance reservations
- Email notifications
- External APIs

---

## 2. Functional Requirements

### 2.1 Core Entities

#### 2.1.1 Equipment (Primary Entity)
The main table storing equipment information.

#### 2.1.2 Equipment Loan (Child Entity)
Loan/return transactions for each equipment.

---

## 3. Technical Specifications

### 3.1 Object ID Range
```
Starting ID: 50000
Reserved Range: 50000-50099
```

### 3.2 Table Definitions

#### 3.2.1 Table: VSS Equipment (50000)

| Field No | Field Name | Data Type | Length | Description |
|----------|------------|-----------|--------|-------------|
| 1 | Code | Code | 20 | Primary Key (ej: LAP-001) |
| 2 | Description | Text | 100 | Brand, model, details |
| 3 | Equipment Type | Enum | - | Laptop, Projector, Tablet, etc. |
| 4 | Status | Enum | - | Available, On Loan, In Repair, Retired |
| 5 | Location | Text | 50 | Storage location when available |
| 6 | Serial Number | Text | 50 | Manufacturer serial number |
| 7 | Purchase Date | Date | - | When acquired |
| 8 | Purchase Cost | Decimal | - | Original cost |
| 9 | Notes | Text | 250 | Additional information |
| 10 | Current Loan No. | Code | 20 | FK to active Equipment Loan |

**Keys:**
- Primary Key: Code
- Key 2: Equipment Type, Status

**Field Groups:**
- DropDown: Code, Description, Equipment Type, Status

**FlowFields:**
- Total Loans: Count of loans for this equipment
- Currently On Loan: Boolean (Status = On Loan)

---

#### 3.2.2 Table: VSS Equipment Loan (50001)

| Field No | Field Name | Data Type | Length | Description |
|----------|------------|-----------|--------|-------------|
| 1 | Loan No. | Code | 20 | Primary Key (No. Series) |
| 2 | Equipment Code | Code | 20 | FK to Equipment |
| 3 | Equipment Description | Text | 100 | Denormalized for quick view |
| 4 | Equipment Type | Enum | - | Denormalized |
| 5 | Borrower Type | Enum | - | Employee, External |
| 6 | Borrower Code | Code | 20 | Employee No. or custom code |
| 7 | Borrower Name | Text | 100 | Name of borrower |
| 8 | Borrower Email | Text | 80 | Contact email |
| 9 | Loan Date | Date | - | When loaned (auto-filled today) |
| 10 | Expected Return Date | Date | - | Expected return date |
| 11 | Actual Return Date | Date | - | When actually returned |
| 12 | Status | Enum | - | Active, Returned, Overdue |
| 13 | Notes | Text | 250 | Purpose, observations |
| 14 | Created By User ID | Code | 50 | Auto-populated |
| 15 | Returned By User ID | Code | 50 | Who registered return |

**Keys:**
- Primary Key: Loan No.
- Key 2: Equipment Code, Loan Date (for history)
- Key 3: Status (for active loans)
- Key 4: Borrower Code

**Field Groups:**
- DropDown: Loan No., Equipment Description, Borrower Name, Status

---

### 3.3 Enum Definitions

#### 3.3.1 Enum: VSS Equipment Type (50000)
```al
enum 50000 "VSS Equipment Type"
{
    Extensible = true;
    
    value(0; Laptop) { Caption = 'Laptop'; }
    value(1; Projector) { Caption = 'Projector'; }
    value(2; Tablet) { Caption = 'Tablet'; }
    value(3; Camera) { Caption = 'Camera'; }
    value(4; Monitor) { Caption = 'Monitor'; }
    value(5; Keyboard) { Caption = 'Keyboard/Mouse'; }
    value(6; Other) { Caption = 'Other'; }
}
```

#### 3.3.2 Enum: VSS Equipment Status (50001)
```al
enum 50001 "VSS Equipment Status"
{
    Extensible = true;
    
    value(0; Available) { Caption = 'Available'; }
    value(1; "On Loan") { Caption = 'On Loan'; }
    value(2; "In Repair") { Caption = 'In Repair'; }
    value(3; Retired) { Caption = 'Retired'; }
}
```

#### 3.3.3 Enum: VSS Equipment Loan Status (50002)
```al
enum 50002 "VSS Equipment Loan Status"
{
    Extensible = true;
    
    value(0; Active) { Caption = 'Active'; }
    value(1; Returned) { Caption = 'Returned'; }
    value(2; Overdue) { Caption = 'Overdue'; }
}
```

#### 3.3.4 Enum: VSS Borrower Type (50003)
```al
enum 50003 "VSS Borrower Type"
{
    Extensible = true;
    
    value(0; Employee) { Caption = 'Employee'; }
    value(1; External) { Caption = 'External'; }
}
```

---

### 3.4 Page Definitions

#### 3.4.1 Page: VSS Equipment List (50000)
- **PageType**: List
- **SourceTable**: VSS Equipment
- **ApplicationArea**: All
- **UsageCategory**: Lists
- **CardPageId**: VSS Equipment Card

**Fields to display:**
- Code
- Description
- Equipment Type
- Status (with StyleExpr)
- Location
- Current Loan No. (if On Loan)

**Actions:**
- Process group:
  - New Loan
  - View Active Loan (if On Loan)
- Navigate group:
  - Loan History

**Views:**
- Available: Status = Available
- On Loan: Status = On Loan
- All Equipment

---

#### 3.4.2 Page: VSS Equipment Card (50001)
- **PageType**: Card
- **SourceTable**: VSS Equipment

**Layout:**

**General FastTab:**
- Code
- Description
- Equipment Type
- Status
- Location

**Details FastTab:**
- Serial Number
- Purchase Date
- Purchase Cost
- Notes

**FactBoxes:**
- Loan History (ListPart)

**Actions:**
- Create Loan
- View Active Loan

---

#### 3.4.3 Page: VSS Equipment Loans (50002)
- **PageType**: List
- **SourceTable**: VSS Equipment Loan
- **ApplicationArea**: All
- **UsageCategory**: Lists
- **CardPageId**: VSS Equipment Loan Card
- **Editable**: false

**Fields:**
- Loan No.
- Equipment Code
- Equipment Description
- Borrower Name
- Loan Date
- Expected Return Date
- Actual Return Date
- Status (with StyleExpr)

**Actions:**
- Process group:
  - Register Return
- Navigate group:
  - Equipment Card

**Views:**
- Active Loans: Status = Active
- Overdue Loans: Status = Overdue
- All Loans

---

#### 3.4.4 Page: VSS Equipment Loan Card (50003)
- **PageType**: Card
- **SourceTable**: VSS Equipment Loan

**Layout:**

**General FastTab:**
- Loan No. (Editable = false after insert)
- Equipment Code
- Equipment Description (Editable = false)
- Equipment Type (Editable = false)
- Status

**Borrower FastTab:**
- Borrower Type
- Borrower Code
- Borrower Name
- Borrower Email

**Dates FastTab:**
- Loan Date
- Expected Return Date
- Actual Return Date

**Notes FastTab:**
- Notes (MultiLine)

**Actions:**
- Register Return

---

#### 3.4.5 Page: VSS Equipment Loan History (50004)
- **PageType**: ListPart
- **SourceTable**: VSS Equipment Loan
- **SourceTableView**: Sorting by Loan Date descending

Used as FactBox in Equipment Card.

---

### 3.5 Setup Extension

#### 3.5.1 Table Extension: VSS Equipment Setup (50002)
Extends: "Sales & Receivables Setup" (table 311)

| Field No | Field Name | Data Type | Length | Description |
|----------|------------|-----------|--------|-------------|
| 50000 | Equipment Loan Nos. | Code | 20 | No. Series for loans |

---

#### 3.5.2 Page Extension: VSS Equipment Setup Ext (50005)
Extends: "Sales & Receivables Setup" (page 459)

Add field "Equipment Loan Nos." in new group "Equipment Management".

---

### 3.6 Codeunit: VSS Equipment Management (50000)

**Procedures:**

```al
procedure CreateLoan(
    EquipmentCode: Code[20];
    BorrowerType: Enum "VSS Borrower Type";
    BorrowerCode: Code[20];
    BorrowerName: Text[100];
    BorrowerEmail: Text[80];
    ExpectedReturnDate: Date;
    Notes: Text[250]
): Code[20]
// Creates new loan and updates equipment status
// Returns the new Loan No.

procedure RegisterReturn(LoanNo: Code[20])
// Registers return, updates equipment status to Available

procedure CheckOverdueLoans()
// Updates loan status to Overdue if past expected return date

procedure CanLoanEquipment(EquipmentCode: Code[20]): Boolean
// Validates if equipment can be loaned (status = Available)

procedure GetActiveLoanForEquipment(EquipmentCode: Code[20]): Code[20]
// Returns active loan number for equipment (if any)

procedure GetLoanHistory(EquipmentCode: Code[20]; var EquipmentLoan: Record "VSS Equipment Loan")
// Filters loan history for specific equipment
```

---

## 4. Business Rules

### 4.1 Equipment Loan Creation
- Equipment must have Status = Available
- Loan No. auto-assigned from No. Series
- Loan Date auto-filled with today
- Equipment Status changes to "On Loan"
- Equipment.Current Loan No. set to new loan
- Borrower Name required

### 4.2 Return Registration
- Only Active loans can be returned
- Actual Return Date auto-filled with today
- Equipment Status returns to "Available"
- Equipment.Current Loan No. cleared
- Loan Status changes to "Returned"

### 4.3 Overdue Detection
- Loan Status automatically changes to "Overdue" when:
  - Status = Active
  - Expected Return Date < Today
  - Actual Return Date is empty

### 4.4 Data Validation
- Equipment Code must be unique
- Cannot delete equipment with active loan
- Cannot delete/modify returned loans (audit trail)

---

## 5. App Manifest (app.json)

```json
{
  "id": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
  "name": "VS Equipment Loan Management",
  "publisher": "VS Sistemas",
  "version": "1.0.0.0",
  "brief": "Simple Equipment Loan Tracking for Business Central",
  "description": "Manage equipment loans (laptops, projectors, cameras) with status tracking and loan history.",
  "privacyStatement": "https://vssistemas.com/privacy",
  "EULA": "https://vssistemas.com/eula",
  "help": "https://vssistemas.com/help/equipment-loan",
  "url": "https://vssistemas.com",
  "logo": "./res/logo.png",
  "platform": "23.0.0.0",
  "application": "23.0.0.0",
  "idRanges": [
    {
      "from": 50000,
      "to": 50099
    }
  ],
  "resourceExposurePolicy": {
    "allowDebugging": true,
    "allowDownloadingSource": false,
    "includeSourceInSymbolFile": true
  },
  "runtime": "12.0",
  "target": "Cloud"
}
```

---

## 6. File Structure

```
/src
  /table
    Tab50000.VSSEquipment.al
    Tab50001.VSSEquipmentLoan.al
  /tableextension
    TabExt50002.VSSEquipmentSetup.al
  /page
    Pag50000.VSSEquipmentList.al
    Pag50001.VSSEquipmentCard.al
    Pag50002.VSSEquipmentLoans.al
    Pag50003.VSSEquipmentLoanCard.al
    Pag50004.VSSEquipmentLoanHistory.al
  /pageextension
    PagExt50005.VSSEquipmentSetupExt.al
  /codeunit
    Cod50000.VSSEquipmentManagement.al
  /enum
    Enum50000.VSSEquipmentType.al
    Enum50001.VSSEquipmentStatus.al
    Enum50002.VSSEquipmentLoanStatus.al
    Enum50003.VSSBorrowerType.al
  /permissionset
    PermSet50000.VSSEquipmentAll.al
    PermSet50001.VSSEquipmentView.al
/res
  logo.png
app.json
```

---

## 7. Permission Sets

### 7.1 VSS Equipment - All (50000)
Full access to all equipment management objects.

### 7.2 VSS Equipment - View (50001)
Read-only access for reporting purposes.

---

## 8. Testing Checklist

### 8.1 Table Tests
- [ ] Create Equipment with all types
- [ ] Create Equipment Loan
- [ ] Equipment status changes when loaned
- [ ] Cannot loan equipment already on loan
- [ ] Return equipment
- [ ] Equipment status returns to Available

### 8.2 Page Tests
- [ ] Equipment List shows all statuses
- [ ] Equipment Card opens and saves
- [ ] Create Loan action works
- [ ] Register Return action works
- [ ] Loan History FactBox displays

### 8.3 Business Logic Tests
- [ ] Auto-assign Loan No from No. Series
- [ ] Overdue detection works
- [ ] Cannot loan unavailable equipment
- [ ] Return clears Current Loan No
- [ ] Loan history preserved after return

---

*End of Document*
