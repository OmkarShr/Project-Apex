# IAM Service Design (Revised)

## 1. Classes and Member Methods

### Class: `IAMService`

The main service class for handling authentication and authorization.

| Member Method | Purpose |
| :--- | :--- |
| `create_user(username, password, role_name)` | Creates a new human user. |
| `create_system_actor(actor_name, role_name)` | Creates a new system actor. |
| `get_user(user_id)` | Retrieves a user's details. |
| `get_system_actor(actor_id)` | Retrieves a system actor's details. |
| `assign_role_to_user(user_id, role_name)` | Assigns a role to a user. |
| `assign_role_to_system_actor(actor_id, role_name)` | Assigns a role to a system actor. |
| `authenticate_user(username, password)` | Authenticates a user and returns a JWT. |
| `authenticate_system_actor(actor_id, api_key)` | Authenticates a system actor and returns a JWT. |
| `validate_token(token)` | Validates a JWT. |
| `check_authorization(token, required_permission)` | Checks if a token has the required permission. |

### Class: `User`

Represents a human actor that can be authenticated.

| Member Method | Purpose |
| :--- | :--- |
| `__init__(user_id, username, hashed_password, salt, status)` | Initializes a new User object. |
| `change_password(new_password)` | Updates the user's password. |
| `deactivate()` | Sets the user's status to inactive. |
| `has_permission(permission)` | Checks if the user has a specific permission. |

### Class: `SystemActor`

Represents a non-human actor (e.g., a robot or another service).

| Member Method | Purpose |
| :--- | :--- |
| `__init__(actor_id, actor_name, api_key, status)` | Initializes a new SystemActor object. |
| `regenerate_api_key()` | Generates a new API key for the system actor. |
| `deactivate()` | Sets the system actor's status to inactive. |
| `has_permission(permission)` | Checks if the system actor has a specific permission. |

### Class: `Role`

Represents a collection of permissions.

| Member Method | Purpose |
| :--- | :--- |
| `__init__(role_id, role_name)` | Initializes a new Role object. |
| `add_permission(permission)` | Adds a permission to the role. |
| `remove_permission(permission)` | Removes a permission from the role. |

### Class: `Permission`

Represents a specific action that can be performed.

| Member Method | Purpose |
| :--- | :--- |
| `__init__(permission_id, permission_name, description)` | Initializes a new Permission object. |

### Class: `Policy`

Represents a policy that enforces a set of rules.

| Member Method | Purpose |
| :--- | :--- |
| `__init__(policy_id, policy_name, rules)` | Initializes a new Policy object. |
| `evaluate(user_or_actor, action)` | Evaluates the policy against a user/actor and an action. |

### Class: `TokenService`

A service for generating and validating JWTs.

| Member Method | Purpose |
| :--- | :--- |
| `generate_token(identity, role, permissions)` | Generates a new JWT. |
| `validate_token(token)` | Validates a JWT and returns its payload. |
| `decode_token(token)` | Decodes a JWT without validating it. |