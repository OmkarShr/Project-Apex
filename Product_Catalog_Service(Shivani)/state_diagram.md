````mermaid
stateDiagram-v2
    [*] --> Idle

    Idle --> Creating_Product      : Create Product Request
    Idle --> Viewing_Product      : Query/Read Request
    Idle --> Editing_Product      : Edit Request
    Idle --> Deleting_Product     : Delete Request
    Idle --> Updating_Attribute   : Update Attribute Request
    Idle --> Reporting            : Generate Report Request

    %% Create flow
    Creating_Product --> Validating_Info    : Enter Product Data
    Validating_Info --> Creating_Product    : Missing/Invalid Info
    Validating_Info --> Product_Active      : Valid Info / Created
    Product_Active --> Editing_Product      : Edit Request
    Product_Active --> Deleting_Product     : Delete Request
    Product_Active --> Updating_Attribute   : Attribute Edit Request
    Product_Active --> Viewing_Product      : Read/Query Request

    %% Edit flow
    Editing_Product --> Validating_Edit     : Edit Details
    Validating_Edit --> Product_Active      : Valid Edit
    Validating_Edit --> Editing_Product     : Edit Invalid

    %% Update Attribute
    Updating_Attribute --> Validating_Update   : Enter Updated Value
    Validating_Update --> Product_Active       : Valid Update
    Validating_Update --> Updating_Attribute   : Invalid Update

    %% Report flow
    Reporting --> Idle                     : Report Generated

    %% View/read flow
    Viewing_Product --> Idle               : Return to Idle

    %% Delete flow
    Deleting_Product --> Deleted           : Confirm Delete
    Deleted --> [*]

    %% Special requirement substate (fork/join example)
    state Product_Active {
        [*] --> Normal
        Normal --> SpecialHandlingRequired : Special Requirement Detected
        SpecialHandlingRequired --> Normal : Requirement Cleared/Handled
    }

````
