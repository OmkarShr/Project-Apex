classDiagram
    class Warehouse {
        +inventory
        +automationWorkflow
        +storeItem()
        +retrieveItem()
        +upgrade()
        +replace()
    }

    class MasterRoboticsControlDispatcher {
        +taskQueue
        +commandInterface
        +sendCommand()
        +use()
        +respond()
    }

    class SoftwareGateway {
        +hardwareProtocols
        +systemState
        +errorLog
        +act()
        +translate()
        +hide()
        +offer()
        +communicate()
        +track()
        +manage()
        +detect()
        +report()
        +handle()
        +ensure()
        +update()
        +monitor()
        +confirm()
        +adapt()
    }

    class ASRS {
        +equipmentList
        +vendorSystem
        +itemLocations
        +store()
        +move()
        +work()
        +respond()
    }

    class Hardware {
        +type
        +vendor
        +state
        +understand()
        +do()
    }

    class Crane
    class Shuttle
    class VerticalLiftModule

    class Item {
        +binId
        +location
        +retrieve()
    }

    class Task {
        +taskId
        +status
        +assignedEquipment
        +complete()
        +fail()
    }

    class Error {
        +errorCode
        +errorMessage
        +detect()
        +report()
    }

    class Protocol {
        +vendorId
        +commandSet
        +translate()
    }

    %% Relationships with multiplicity & aggregation/composition
    Warehouse "1" --> "1" MasterRoboticsControlDispatcher : manages
    MasterRoboticsControlDispatcher "1" --> "1" SoftwareGateway : sendsCommands
    SoftwareGateway "1" --> "1" ASRS : translatesFor
    ASRS "1" o-- "*" Hardware : aggregation
    Hardware <|-- Crane
    Hardware <|-- Shuttle
    Hardware <|-- VerticalLiftModule
    ASRS "1" *-- "*" Item : composition
    ASRS "1" *-- "*" Task : composition
    SoftwareGateway "1" --> "*" Error : handles
    SoftwareGateway "1" --> "*" Protocol : uses
