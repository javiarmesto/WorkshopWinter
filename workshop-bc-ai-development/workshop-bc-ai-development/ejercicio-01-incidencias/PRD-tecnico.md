# PRD: Business Central Incident Management Extension

## Document Metadata
- **Version**: 1.0
- **Date**: 2025-01-14
- **Target Platform**: Microsoft Dynamics 365 Business Central
- **Language**: AL (Application Language)
- **Minimum BC Version**: 23.0

---

## 1. Executive Summary

### 1.1 Purpose
Develop a Business Central extension to manage and track incidents received from external channels (APIs, integrations, agents). The extension provides a complete incident lifecycle management system with external API exposure for integration with AI agents and external systems.

### 1.2 Scope
- Data structures for incident management
- User interface (List and Card pages)
- RESTful APIs for external consumption
- Basic workflow support (status management)

### 1.3 Out of Scope
- Channel-specific integrations (handled externally)
- Email notifications
- Advanced workflow/approvals
- Document attachments (Phase 2)

---

## 2. Functional Requirements

### 2.1 Core Entities

#### 2.1.1 Incident (Primary Entity)
The main table storing incident information.

#### 2.1.2 Incident Category (Lookup Entity)
Classification categories for incidents.

#### 2.1.3 Incident Comment (Child Entity)
Comments and activity log for each incident.

---

## 3. Technical Specifications

### 3.1 Object ID Range
```
Starting ID: 50100
Reserved Range: 50100-50199
```

### 3.2 Table Definitions

#### 3.2.1 Table: VSS Incident Category (50100)

| Field No | Field Name | Data Type | Length | Description |
|----------|------------|-----------|--------|-------------|
| 1 | Code | Code | 20 | Primary Key |
| 2 | Description | Text | 100 | Category description |
| 3 | Default Priority | Option | - | Low,Medium,High,Critical |
| 4 | Active | Boolean | - | Enable/disable category |

**Keys:**
- Primary Key: Code

---

#### 3.2.2 Table: VSS Incident (50101)

| Field No | Field Name | Data Type | Length | Description |
|----------|------------|-----------|--------|-------------|
| 1 | No. | Code | 20 | Primary Key (No. Series) |
| 2 | Description | Text | 100 | Short description |
| 3 | Detailed Description | Blob | - | SubType: Text, full description |
| 4 | Category Code | Code | 20 | FK to Incident Category |
| 5 | Status | Enum | - | See Enum definition |
| 6 | Priority | Enum | - | Low,Medium,High,Critical |
| 7 | Source Channel | Text | 50 | Origin channel identifier |
| 8 | External Reference | Text | 100 | External system reference |
| 9 | Customer No. | Code | 20 | FK to Customer (optional) |
| 10 | Customer Name | Text | 100 | Denormalized or manual entry |
| 11 | Contact Name | Text | 100 | Reporter name |
| 12 | Contact Email | Text | 80 | Reporter email |
| 13 | Contact Phone | Text | 30 | Reporter phone |
| 14 | Assigned To User ID | Code | 50 | FK to User Setup |
| 15 | Created DateTime | DateTime | - | Auto-populated |
| 16 | Created By User ID | Code | 50 | Auto-populated |
| 17 | Modified DateTime | DateTime | - | Auto-populated |
| 18 | Resolution DateTime | DateTime | - | When resolved |
| 19 | Resolution Notes | Blob | - | SubType: Text |
| 20 | Due Date | Date | - | Expected resolution date |
| 21 | No. Series | Code | 20 | No. Series code used |

**Keys:**
- Primary Key: No.
- Key 2: Status, Priority (for filtering)
- Key 3: Customer No. (for customer-related queries)
- Key 4: Assigned To User ID (for workload views)
- Key 5: Created DateTime (for chronological sorting)

**Field Groups:**
- DropDown: No., Description, Status, Priority
- Brick: No., Description, Status, Priority, Customer Name

---

#### 3.2.3 Table: VSS Incident Comment (50102)

| Field No | Field Name | Data Type | Length | Description |
|----------|------------|-----------|--------|-------------|
| 1 | Incident No. | Code | 20 | FK to Incident |
| 2 | Line No. | Integer | - | Auto-increment within incident |
| 3 | Comment | Text | 250 | Comment text |
| 4 | Comment Type | Enum | - | Note,StatusChange,Assignment,Resolution |
| 5 | Created DateTime | DateTime | - | Auto-populated |
| 6 | Created By User ID | Code | 50 | Auto-populated |
| 7 | Is System Generated | Boolean | - | True for auto-comments |

**Keys:**
- Primary Key: Incident No., Line No.

---

### 3.3 Enum Definitions

#### 3.3.1 Enum: VSS Incident Status (50100)
```al
enum 50100 "VSS Incident Status"
{
    Extensible = true;
    
    value(0; New) { Caption = 'New'; }
    value(1; "In Progress") { Caption = 'In Progress'; }
    value(2; "Pending Customer") { Caption = 'Pending Customer'; }
    value(3; "Pending Internal") { Caption = 'Pending Internal'; }
    value(4; Resolved) { Caption = 'Resolved'; }
    value(5; Closed) { Caption = 'Closed'; }
    value(6; Cancelled) { Caption = 'Cancelled'; }
}
```

#### 3.3.2 Enum: VSS Incident Priority (50101)
```al
enum 50101 "VSS Incident Priority"
{
    Extensible = true;
    
    value(0; Low) { Caption = 'Low'; }
    value(1; Medium) { Caption = 'Medium'; }
    value(2; High) { Caption = 'High'; }
    value(3; Critical) { Caption = 'Critical'; }
}
```

#### 3.3.3 Enum: VSS Incident Comment Type (50102)
```al
enum 50102 "VSS Incident Comment Type"
{
    Extensible = true;
    
    value(0; Note) { Caption = 'Note'; }
    value(1; "Status Change") { Caption = 'Status Change'; }
    value(2; Assignment) { Caption = 'Assignment'; }
    value(3; Resolution) { Caption = 'Resolution'; }
}
```

---

### 3.4 Page Definitions

#### 3.4.1 Page: VSS Incident Categories (50100)
- **PageType**: List
- **SourceTable**: VSS Incident Category
- **ApplicationArea**: All
- **UsageCategory**: Lists
- **CardPageId**: VSS Incident Category Card

**Fields to display:**
- Code
- Description
- Default Priority
- Active

---

#### 3.4.2 Page: VSS Incident Category Card (50101)
- **PageType**: Card
- **SourceTable**: VSS Incident Category

**Layout:**
- General FastTab: All fields

---

#### 3.4.3 Page: VSS Incidents (50102)
- **PageType**: List
- **SourceTable**: VSS Incident
- **ApplicationArea**: All
- **UsageCategory**: Lists
- **CardPageId**: VSS Incident Card
- **Editable**: false

**Fields to display:**
- No.
- Description
- Category Code
- Status (with StyleExpr based on status)
- Priority (with StyleExpr based on priority)
- Customer Name
- Assigned To User ID
- Created DateTime
- Due Date

**Actions:**
- Process group:
  - Set In Progress
  - Set Resolved
  - Assign to Me
- Navigate group:
  - Comments
  - Customer Card (if Customer No. filled)

**Views (Saved filters):**
- My Open Incidents: Assigned To = CurrentUser, Status in (New, In Progress, Pending)
- All Open Incidents: Status in (New, In Progress, Pending)
- Critical Incidents: Priority = Critical, Status not in (Closed, Cancelled)

---

#### 3.4.4 Page: VSS Incident Card (50103)
- **PageType**: Card
- **SourceTable**: VSS Incident

**Layout:**

**General FastTab:**
- No. (Editable = false after insert)
- Description
- Category Code
- Status
- Priority
- Due Date

**Details FastTab:**
- Detailed Description (MultiLine = true, using helper functions for Blob)

**Contact Information FastTab:**
- Customer No.
- Customer Name
- Contact Name
- Contact Email
- Contact Phone

**Source FastTab:**
- Source Channel
- External Reference

**Assignment FastTab:**
- Assigned To User ID

**Resolution FastTab (visible when Status in Resolved, Closed):**
- Resolution DateTime
- Resolution Notes (MultiLine)

**FactBoxes:**
- Incident Comments Part (ListPart)

**Actions:**
- Process group:
  - Set In Progress
  - Set Pending Customer
  - Set Resolved
  - Assign to Me
- Navigate group:
  - Comments

---

#### 3.4.5 Page: VSS Incident Comments (50104)
- **PageType**: ListPart
- **SourceTable**: VSS Incident Comment

**Fields:**
- Line No. (visible = false)
- Comment Type
- Comment
- Created DateTime
- Created By User ID

---

#### 3.4.6 Page: VSS Incident Comment List (50105)
- **PageType**: List
- **SourceTable**: VSS Incident Comment
- **Editable**: true

Used for full-page comment management.

---

### 3.5 Setup Extension

#### 3.5.1 Table Extension: VSS Incident Setup (50103)
Extends: "Sales & Receivables Setup" (table 311)

| Field No | Field Name | Data Type | Length | Description |
|----------|------------|-----------|--------|-------------|
| 50100 | Incident Nos. | Code | 20 | No. Series for incidents |

---

#### 3.5.2 Page Extension: VSS Incident Setup Ext (50106)
Extends: "Sales & Receivables Setup" (page 459)

Add field "Incident Nos." in a new group "Incident Management" after existing content.

---

### 3.6 Codeunit: VSS Incident Management (50100)

**Procedures:**

```al
procedure CreateIncident(
    Description: Text[100];
    CategoryCode: Code[20];
    Priority: Enum "VSS Incident Priority";
    CustomerNo: Code[20];
    ContactName: Text[100];
    ContactEmail: Text[80];
    SourceChannel: Text[50];
    ExternalReference: Text[100]
): Code[20]
// Returns the new Incident No.

procedure UpdateStatus(
    IncidentNo: Code[20];
    NewStatus: Enum "VSS Incident Status"
)
// Updates status and creates system comment

procedure AssignIncident(
    IncidentNo: Code[20];
    UserID: Code[50]
)
// Assigns incident and creates system comment

procedure AddComment(
    IncidentNo: Code[20];
    CommentText: Text[250];
    CommentType: Enum "VSS Incident Comment Type"
)
// Adds a comment to the incident

procedure ResolveIncident(
    IncidentNo: Code[20];
    ResolutionNotes: Text
)
// Sets status to Resolved, populates resolution fields

procedure GetNextCommentLineNo(IncidentNo: Code[20]): Integer
// Helper to get next line number for comments

procedure SetDetailedDescription(var Incident: Record "VSS Incident"; NewDescription: Text)
// Helper to set Blob field

procedure GetDetailedDescription(Incident: Record "VSS Incident"): Text
// Helper to read Blob field
```

---

### 3.7 API Definitions

#### 3.7.1 API Page: VSS Incidents API (50107)

```al
page 50107 "VSS Incidents API"
{
    PageType = API;
    APIPublisher = 'vssistemas';
    APIGroup = 'incidentManagement';
    APIVersion = 'v1.0';
    EntityName = 'incident';
    EntitySetName = 'incidents';
    SourceTable = "VSS Incident";
    DelayedInsert = true;
    ODataKeyFields = "No.";
    
    layout
    {
        area(Content)
        {
            repeater(Group)
            {
                field(number; Rec."No.") { }
                field(description; Rec.Description) { }
                field(detailedDescription; DetailedDescriptionText) { }
                field(categoryCode; Rec."Category Code") { }
                field(status; Rec.Status) { }
                field(priority; Rec.Priority) { }
                field(sourceChannel; Rec."Source Channel") { }
                field(externalReference; Rec."External Reference") { }
                field(customerNo; Rec."Customer No.") { }
                field(customerName; Rec."Customer Name") { }
                field(contactName; Rec."Contact Name") { }
                field(contactEmail; Rec."Contact Email") { }
                field(contactPhone; Rec."Contact Phone") { }
                field(assignedToUserId; Rec."Assigned To User ID") { }
                field(createdDateTime; Rec."Created DateTime") { }
                field(dueDate; Rec."Due Date") { }
                field(resolutionDateTime; Rec."Resolution DateTime") { }
            }
        }
    }
    
    var
        DetailedDescriptionText: Text;
    
    trigger OnAfterGetRecord()
    begin
        DetailedDescriptionText := GetDetailedDescription(Rec);
    end;
}
```

**API Endpoint:**
```
GET/POST/PATCH/DELETE: /api/vssistemas/incidentManagement/v1.0/incidents
```

---

#### 3.7.2 API Page: VSS Incident Categories API (50108)

```al
page 50108 "VSS Incident Categories API"
{
    PageType = API;
    APIPublisher = 'vssistemas';
    APIGroup = 'incidentManagement';
    APIVersion = 'v1.0';
    EntityName = 'incidentCategory';
    EntitySetName = 'incidentCategories';
    SourceTable = "VSS Incident Category";
    DelayedInsert = true;
    ODataKeyFields = Code;
}
```

**API Endpoint:**
```
GET/POST/PATCH/DELETE: /api/vssistemas/incidentManagement/v1.0/incidentCategories
```

---

#### 3.7.3 API Page: VSS Incident Comments API (50109)

```al
page 50109 "VSS Incident Comments API"
{
    PageType = API;
    APIPublisher = 'vssistemas';
    APIGroup = 'incidentManagement';
    APIVersion = 'v1.0';
    EntityName = 'incidentComment';
    EntitySetName = 'incidentComments';
    SourceTable = "VSS Incident Comment";
    DelayedInsert = true;
    ODataKeyFields = "Incident No.", "Line No.";
}
```

**API Endpoint:**
```
GET/POST: /api/vssistemas/incidentManagement/v1.0/incidentComments
Filter by: $filter=incidentNo eq 'INC-00001'
```

---

#### 3.7.4 API Codeunit Actions (50101)

For complex operations, expose bound actions:

```al
codeunit 50101 "VSS Incident API Actions"
{
    procedure UpdateIncidentStatus(IncidentNo: Code[20]; NewStatus: Text)
    // Callable via API custom action
    
    procedure AssignIncidentToUser(IncidentNo: Code[20]; UserID: Code[50])
    // Callable via API custom action
    
    procedure AddIncidentComment(IncidentNo: Code[20]; CommentText: Text)
    // Callable via API custom action
}
```

---

## 4. Business Rules

### 4.1 Incident Creation
- No. is auto-assigned from No. Series defined in Setup
- Created DateTime auto-populated with CurrentDateTime
- Created By User ID auto-populated with UserId
- Initial Status = New
- If Category has Default Priority and Priority not specified, use category default

### 4.2 Status Transitions
Valid transitions:
```
New → In Progress, Pending Customer, Pending Internal, Cancelled
In Progress → Pending Customer, Pending Internal, Resolved, Cancelled
Pending Customer → In Progress, Resolved, Cancelled
Pending Internal → In Progress, Resolved, Cancelled
Resolved → Closed, In Progress (reopen)
Closed → (terminal state)
Cancelled → (terminal state)
```

### 4.3 Status Change Actions
- Any status change creates automatic system comment
- Resolved status requires Resolution DateTime (auto-set if empty)
- Assignment change creates automatic system comment

### 4.4 Data Validation
- Contact Email: Validate email format if not empty
- Customer No.: If filled, validate exists in Customer table
- Due Date: Must be >= Today if filled on creation

---

## 5. App Manifest (app.json)

```json
{
  "id": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
  "name": "VS Incident Management",
  "publisher": "VS Sistemas",
  "version": "1.0.0.0",
  "brief": "Incident Management System for Business Central",
  "description": "Complete incident tracking and management with API integration for external agents and systems.",
  "privacyStatement": "https://vssistemas.com/privacy",
  "EULA": "https://vssistemas.com/eula",
  "help": "https://vssistemas.com/help/incident-management",
  "url": "https://vssistemas.com",
  "logo": "./res/logo.png",
  "platform": "23.0.0.0",
  "application": "23.0.0.0",
  "idRanges": [
    {
      "from": 50100,
      "to": 50199
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
    Tab50100.VSSIncidentCategory.al
    Tab50101.VSSIncident.al
    Tab50102.VSSIncidentComment.al
  /tableextension
    TabExt50103.VSSIncidentSetup.al
  /page
    Pag50100.VSSIncidentCategories.al
    Pag50101.VSSIncidentCategoryCard.al
    Pag50102.VSSIncidents.al
    Pag50103.VSSIncidentCard.al
    Pag50104.VSSIncidentComments.al
    Pag50105.VSSIncidentCommentList.al
  /pageextension
    PagExt50106.VSSIncidentSetupExt.al
  /api
    Pag50107.VSSIncidentsAPI.al
    Pag50108.VSSIncidentCategoriesAPI.al
    Pag50109.VSSIncidentCommentsAPI.al
  /codeunit
    Cod50100.VSSIncidentManagement.al
    Cod50101.VSSIncidentAPIActions.al
  /enum
    Enum50100.VSSIncidentStatus.al
    Enum50101.VSSIncidentPriority.al
    Enum50102.VSSIncidentCommentType.al
  /permissionset
    PermSet50100.VSSIncidentMgmtAll.al
    PermSet50101.VSSIncidentMgmtView.al
/res
  logo.png
/translations
  VSSIncidentManagement.g.xlf
app.json
```

---

## 7. Permission Sets

### 7.1 VSS Incident Mgmt - All (50100)
Full access to all incident management objects.

### 7.2 VSS Incident Mgmt - View (50101)
Read-only access for reporting purposes.

---

## 8. API Usage Examples for Agents

### 8.1 Create New Incident (POST)
```http
POST /api/vssistemas/incidentManagement/v1.0/incidents
Content-Type: application/json

{
  "description": "Customer cannot access portal",
  "categoryCode": "SUPPORT",
  "priority": "High",
  "customerNo": "C00010",
  "contactName": "John Smith",
  "contactEmail": "john@customer.com",
  "sourceChannel": "ChatBot",
  "externalReference": "CHAT-2025-0001"
}
```

### 8.2 Get Open Incidents (GET with filter)
```http
GET /api/vssistemas/incidentManagement/v1.0/incidents?$filter=status ne 'Closed' and status ne 'Cancelled'&$orderby=createdDateTime desc
```

### 8.3 Get Incidents by Customer (GET with filter)
```http
GET /api/vssistemas/incidentManagement/v1.0/incidents?$filter=customerNo eq 'C00010'
```

### 8.4 Update Incident Status (PATCH)
```http
PATCH /api/vssistemas/incidentManagement/v1.0/incidents('INC-00001')
Content-Type: application/json
If-Match: *

{
  "status": "In Progress",
  "assignedToUserId": "ADMIN"
}
```

### 8.5 Add Comment (POST)
```http
POST /api/vssistemas/incidentManagement/v1.0/incidentComments
Content-Type: application/json

{
  "incidentNo": "INC-00001",
  "comment": "Contacted customer, waiting for additional info",
  "commentType": "Note"
}
```

### 8.6 Get Incident with Comments (GET with expand)
```http
GET /api/vssistemas/incidentManagement/v1.0/incidents('INC-00001')?$expand=incidentComments
```

---

## 9. Testing Checklist

### 9.1 Table Tests
- [ ] Create Incident Category
- [ ] Create Incident with all fields
- [ ] Create Incident with minimum fields (auto No. Series)
- [ ] Add Comment to Incident
- [ ] Blob fields (Detailed Description, Resolution Notes) read/write

### 9.2 Page Tests
- [ ] Incident List loads and displays correctly
- [ ] Incident Card opens and saves
- [ ] Status change actions work
- [ ] Assignment action works
- [ ] Comments FactBox displays correctly
- [ ] Setup page extension displays No. Series field

### 9.3 API Tests
- [ ] GET all incidents
- [ ] GET single incident by No.
- [ ] GET incidents with $filter
- [ ] POST new incident
- [ ] PATCH existing incident
- [ ] POST new comment
- [ ] GET comments filtered by incident

### 9.4 Business Logic Tests
- [ ] Status transition validation
- [ ] Auto-comment on status change
- [ ] Auto-comment on assignment change
- [ ] Resolution DateTime auto-set
- [ ] No. Series assignment

---

## 10. Future Enhancements (Phase 2)

- Document attachments support
- Email notifications on status change
- SLA tracking and escalation
- Dashboard/Role Center cue tiles
- Power BI reports integration
- Approval workflows
- Customer portal integration

---

## Appendix A: Naming Conventions

| Object Type | Prefix | Example |
|-------------|--------|---------|
| Table | VSS | VSS Incident |
| Page | VSS | VSS Incidents |
| Codeunit | VSS | VSS Incident Management |
| Enum | VSS | VSS Incident Status |
| API Page | VSS | VSS Incidents API |

---

## Appendix B: Translation Keys

All captions should use CaptionML or Caption property with Comment for translators. Primary language: English (ENU). Secondary: Spanish (ESM).

---

*End of Document*
