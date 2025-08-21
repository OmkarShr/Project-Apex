# Fleet & Vehicle Management Service Design (Revised)

## 1. Classes and Member Methods

### Class: `FleetManagementService`

The main service for managing the vehicle fleet.

| Member Method | Purpose |
| :--- | :--- |
| `add_vehicle(vehicle_data)` | Adds a new vehicle to the fleet. |
| `get_vehicle(vehicle_id)` | Retrieves a vehicle from the fleet. |
| `add_driver(driver_data)` | Adds a new driver. |
| `get_driver(driver_id)` | Retrieves a driver's details. |
| `assign_driver_to_vehicle(driver_id, vehicle_id)` | Assigns a driver to a vehicle. |
| `schedule_maintenance(vehicle_id, date, description)` | Schedules maintenance for a vehicle. |
| `record_inspection(vehicle_id, date, notes)` | Records an inspection for a vehicle. |
| `update_vehicle_status(vehicle_id, new_status)` | Updates the status of a vehicle. |

### Class: `Vehicle`

Represents a delivery vehicle in the fleet.

| Member Method | Purpose |
| :--- | :--- |
| `__init__(vehicle_id, license_plate, make, model, capacity, status)` | Initializes a new Vehicle object. |
| `update_status(new_status)` | Updates the status of the vehicle. |

### Class: `Driver`

Represents a driver of a delivery vehicle.

| Member Method | Purpose |
| :--- | :--- |
| `__init__(driver_id, name, license_number)` | Initializes a new Driver object. |

### Class: `MaintenanceRecord`

Represents a maintenance record for a vehicle.

| Member Method | Purpose |
| :--- | :--- |
| `__init__(record_id, vehicle_id, date, description, cost)` | Initializes a new MaintenanceRecord object. |

### Class: `InspectionRecord`

Represents an inspection record for a vehicle.

| Member Method | Purpose |
| :--- | :--- |
| `__init__(record_id, vehicle_id, date, notes, passed)` | Initializes a new InspectionRecord object. |

### Class: `Insurance`

Represents the insurance policy for a vehicle.

| Member Method | Purpose |
| :--- | :--- |
| `__init__(policy_id, vehicle_id, provider, policy_number, expiration_date)` | Initializes a new Insurance object. |

### Class: `VehicleStatus`

Represents the status of a vehicle.

| Member Method | Purpose |
| :--- | :--- |
| `__init__(status_id, status_name)` | Initializes a new VehicleStatus object. |

### Class: `DriverSchedule`

Represents the schedule for a driver.

| Member Method | Purpose |
| :--- | :--- |
| `__init__(schedule_id, driver_id, start_time, end_time)` | Initializes a new DriverSchedule object. |