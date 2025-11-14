sequenceDiagram
    actor Dispatcher as MasterRobotics<br>ControlDispatcher
    participant Gateway as SoftwareGateway
    participant Task as Task
    participant Protocol as VendorProtocol
    participant ASRS as ASRS
    participant Crane as Crane (Hardware)
    participant Error as Error

    %% --- Scenario 1: Successful Retrieval ---
    opt Successful 'retrieveItem' Command [cite: 14]
        Dispatcher->>+Gateway: sendCommand(retrieveItem, bin123)
        
        create participant Task
        Gateway->>Task: new('Retrieve', bin123)
        Gateway->>Gateway: track() / manageState() [cite: 19, 54]

        %% Translation Step
        create participant Protocol
        Gateway->>Protocol: new(vendorID)
        Gateway->>Protocol: translate("retrieveItem") [cite: 15, 49]
        Protocol-->>Gateway: "vendor_specific_cmd_A"
        destroy Protocol

        %% Hardware Interaction
        Gateway->>+ASRS: execute(vendor_specific_cmd_A)
        ASRS->>+Crane: move(bin123) [cite: 67]
        Crane-->>-ASRS: itemRetrieved
        
        ASRS-->>-Gateway: taskComplete
        
        %% Confirmation and Cleanup
        Gateway->>Task: complete() [cite: 85]
        Gateway->>Gateway: confirm() / update() [cite: 22, 59, 61]
        Gateway-->>-Dispatcher: respond(success, bin123) [cite: 44]
        
        destroy Task
    end

    %% --- Scenario 2: Failed Storage ---
    opt Failed 'storeItem' Command [cite: 14]
        Dispatcher->>+Gateway: sendCommand(storeItem, bin456, loc7)
        
        create participant Task
        Gateway->>Task: new('Store', bin456)
        Gateway->>Gateway: track() / manageState() [cite: 19, 54]

        %% Translation Step
        create participant Protocol
        Gateway->>Protocol: new(vendorID)
        Gateway->>Protocol: translate("storeItem") [cite: 15, 49]
        Protocol-->>Gateway: "vendor_specific_cmd_B"
        destroy Protocol

        %% Hardware Interaction with Failure
        Gateway->>+ASRS: execute(vendor_specific_cmd_B)
        ASRS->>+Crane: store(bin456, loc7) [cite: 66]
        Crane-->>-ASRS: error: gripper_failed
        
        ASRS-->>-Gateway: taskFailed(gripper_failed)
        
        %% Error Handling Step [cite: 21, 29]
        Gateway->>Gateway: detect() error [cite: 55]
        
        create participant Error
        Gateway->>Error: new(E_GRIPPER, "Gripper Failed")
        Gateway->>Error: report() [cite: 56, 91]
        
        Gateway->>Task: fail() [cite: 86]
        Gateway-->>-Dispatcher: respond(error, "Gripper Failed") [cite: 21, 44]
        
        destroy Task
        destroy Error
    end