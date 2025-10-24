stateDiagram-v2
  %% Start
  [*] --> RouteIdle

  %% RoutePlan (compact)
  state RoutePlan {
    [*] --> Created
    Created --> Assigned : assignToDriver()
    Assigned --> InProgress : driverAck() / startRoute()
    InProgress --> Completed : allStopsDone()
    InProgress --> Cancelled : cancelRoute()
    Completed --> [*]
    Cancelled --> [*]

    %% InProgress contains two concurrent activities (fork)
    state InProgress {
      direction LR
      %% concurrent subflows
      state GPSStreaming {
        [*] --> Acquiring
        Acquiring --> Streaming : signalLocked()
        Streaming --> Acquiring : signalLost()
      }
      state EventProcessing {
        [*] --> Listening
        Listening --> Processing : statusEvent()
        Processing --> Listening : loop
      }
    }
  }

  %% DeliveryStop (individual stop - compact)
  state DeliveryStop {
    [*] --> Pending
    Pending --> EnRoute : driverDeparted()
    EnRoute --> Arrived : atStop()
    Arrived --> Attempting : startDeliveryAttempt()
    Attempting --> Delivered : success()/capturePOD()
    Attempting --> Failed : failAttempt()
    Failed --> ReattemptScheduled : scheduleReattempt()
    ReattemptScheduled --> EnRoute : reattemptDispatched()
    Delivered --> Closed : updateOrder()
    Closed --> [*]
  }

  %% Driver mobile (compact)
  state DriverApp {
    [*] --> IdleApp
    IdleApp --> FetchRoute : fetchRoute()
    FetchRoute --> Navigating : routeReceived()
    Navigating --> AtStop : arrivalDetected()
    AtStop --> Report : sendStatus()
    Report --> Navigating : resume()
    Report --> IdleApp : routeComplete()
  }

  %% Key high-level connections (labelled)
  RoutePlan.Assigned --> DriverApp.FetchRoute : notifyDriver()
  DriverApp.AtStop --> DeliveryStop.Arrived : arriveAtStop()
  DriverApp.Report --> DeliveryStop.Attempting : deliveryAttempt()
  GPSStreaming.Streaming --> DriverApp.Navigating : locationStream()
  Processing.Processing --> RoutePlan.InProgress : statusUpdate()
  DeliveryStop.Delivered --> RoutePlan.InProgress : stopCompleted()
  RoutePlan.Completed --> [*]
