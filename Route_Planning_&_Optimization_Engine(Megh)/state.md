```mermaid
stateDiagram-v2
direction TB

[*] --> PlanningRequest : receiveRoutePlanningRequest
PlanningRequest: entry / collectOrderData & constraints

PlanningRequest --> ValidatingInputs : inputsReady
ValidatingInputs: entry / checkAddresses & fleetStatus
ValidatingInputs --> Error : invalidInput / notify("Invalid orders or vehicle status")
ValidatingInputs --> RouteComputation : validInput

RouteComputation: entry / computeOptimalRoutes()
RouteComputation --> ItineraryAssignment : success
RouteComputation --> HandleFailure : noSolution

ItineraryAssignment: entry / assignItinerariesToDrivers()
ItineraryAssignment --> Fulfilled : allAssigned
ItineraryAssignment --> PartialAssigned : notAllAssigned

PartialAssigned: entry / notifyPartialAssignment
PartialAssigned --> Fulfilled : allAssignedLater
PartialAssigned --> Cancelled : remainingUnassigned / escalateCancellation

Fulfilled: entry / sendDispatchNotifications
Fulfilled --> [*]
HandleFailure: entry / logAndAllowRetry
HandleFailure --> PlanningRequest : customerRetries
HandleFailure --> Cancelled : maxRetriesExceeded

Cancelled: entry / cancelPlan & notifyOps
Cancelled --> [*]

Error --> [*]

%% --- Optional Reroute Flow after incident or live update
Fulfilled --> RerouteFlow : recomputeRequired
state RerouteFlow {
  [*] --> RerouteRequested : triggerReroute
  RerouteRequested: entry / evaluateIncidentImpact
  RerouteRequested --> Rerouting : impactConfirmed
  Rerouting: do / runRerouteAlgorithm
  Rerouting --> Rerouted : rerouteSuccess
  Rerouting --> Failed : rerouteFailed
  Rerouted: entry / notifyDriverUpdates
  Failed --> [*]
  Rerouted --> [*]
}
%% --- Optional Analytics/Reporting Flow after fulfillment
Fulfilled --> AnalyticsFlow : analyticsTriggered
state AnalyticsFlow {
  [*] --> Reporting : pushRouteData
  Reporting: entry / aggregateAndSend
  Reporting --> [*]
}

