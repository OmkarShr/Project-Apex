stateDiagram-v2
  [*] --> Operational

  %% ------------------ OPERATIONAL STATE ------------------
  state "Operational" as Operational {
    direction LR

    %% ---------- ACCOUNT LIFECYCLE ----------
    state "Account Lifecycle" as AL {
      direction TB
      [*] --> NewAccount
      state "New" as NewAccount
      state "Pending Verification" as PendingVerification
      state "Active" as ActiveAccount
      state "Suspended" as SuspendedAccount
      state "Deactivated" as DeactivatedAccount

      NewAccount --> PendingVerification: submit signup / create
      PendingVerification --> ActiveAccount: verify identity / approve
      ActiveAccount --> SuspendedAccount: policy breach / manual suspend
      SuspendedAccount --> ActiveAccount: reinstate
      ActiveAccount --> DeactivatedAccount: deactivate / offboard
      PendingVerification --> DeactivatedAccount: verification timeout / reject
    }

    %% ---------- RBAC MANAGEMENT ----------
    state "RBAC Management" as RBAC {
      direction TB
      [*] --> RoleModel
      state "Role Model Defined" as RoleModel
      state "Permission Definition" as PermissionDefinition
      state "Role Assignment" as RoleAssignment
      state "Role Change" as RoleChange

      RoleModel --> PermissionDefinition: define permissions
      PermissionDefinition --> RoleAssignment: assign roles
      RoleAssignment --> RoleChange: request role change
      RoleChange --> RoleAssignment: apply change
    }

    %% ---------- AUTH & TOKENS ----------
    state "Auth & Tokens" as AT {
      direction TB
      [*] --> AuthRequest
      state "Auth Request" as AuthRequest
      state "Authenticated" as Authenticated
      state "Token Issued" as TokenIssued
      state "Token Refresh" as TokenRefresh
      state "Token Revoked" as TokenRevoked
      state "Auth Failed" as AuthFailed

      AuthRequest --> Authenticated: valid credentials
      AuthRequest --> AuthFailed: invalid credentials
      Authenticated --> TokenIssued: issue JWT
      TokenIssued --> TokenRefresh: refresh before expiry
      TokenRefresh --> TokenIssued: new token
      TokenIssued --> TokenRevoked: logout / revoke
      AuthFailed --> [*]
    }

    %% ---------- AUTHORIZATION FLOW ----------
    state "Authorization Flow" as AZ {
      direction TB
      [*] --> AuthzQuery
      state "AuthZ Query" as AuthzQuery
      state "Permit" as Permit
      state "Deny" as Deny
      state "Rate Limited" as RateLimited
      state "Offline / Cached Decision" as OfflineFallback

      AuthzQuery --> Permit: allow / scope match
      AuthzQuery --> Deny: deny / missing permission
      AuthzQuery --> RateLimited: rate limit exceeded
      AuthzQuery --> OfflineFallback: HA offline -> cache used
      Permit --> [*]
      Deny --> [*]
      RateLimited --> Deny: fail closed
      OfflineFallback --> Deny: cached deny
    }

    %% ---------- SERVICE CLIENTS ----------
    state "Service Clients" as SC {
      direction TB
      [*] --> ServiceAuth
      state "Service Auth (mTLS/APIKey)" as ServiceAuth
      state "Robot/Agent Auth (certs)" as RobotAuth
      state "Trust Anchors / PKI" as TrustAnchors

      ServiceAuth --> RobotAuth: mutual auth flow
      RobotAuth --> TrustAnchors: validate cert chain
      ServiceAuth --> TrustAnchors: verify server cert
    }

    %% ---------- SUPPORT STATES ----------
    state "High Availability" as HA
    state "Audit & Logging" as Audit
    state "Monitoring & Alerting" as Monitoring
    state "Maintenance / Upgrade" as Maintenance
    state "Security Response / Incident" as SecurityOps

    %% ---------- CROSS-MODULE TRANSITIONS ----------
    AL --> RBAC: role assignment request
    RBAC --> AT: login triggers auth
    AT --> AZ: token included in request
    AZ --> Audit: log decision
    AT --> Audit: log revocation
    AL --> AT: suspended -> deny auth
    SecurityOps --> AL: auto suspend compromised account
    Monitoring --> HA: detect node failure
    Maintenance --> HA: degrade gracefully
    HA --> AZ: respond from replica

    [*] --> AL
  }

  %% ------------------ GLOBAL STATES ------------------
  Operational --> Maintenance: scheduled upgrade
  Maintenance --> Operational: maintenance complete
  Operational --> [*]: service shutdown

  %% ---------- INCIDENT HANDLING ----------
  state "Incident Handling" as IH {
    direction TB
    [*] --> Detect
    state "Detect Anomaly" as Detect
    state "Contain" as Contain
    state "Remediate" as Remediate
    state "Recover" as Recover

    Detect --> Contain: isolate principals
    Contain --> Remediate: revoke / rotate keys
    Remediate --> Recover: restore services
    Recover --> [*]
  }

  Audit --> IH: suspicious logs
  SecurityOps --> IH: trigger manual incident

  %% ---------- NOTES ----------
  note right of TokenIssued
    Tokens: JWTs with TTL, refresh tokens,
    revocation lists, introspection endpoint.
  end note

  note left of RoleModel
    RBAC: least privilege, role hierarchies,
    separation of duties.
  end note

  note right of AuthzQuery
    Authorization API: low-latency, cache-aware,
    batch checks supported.
  end note
