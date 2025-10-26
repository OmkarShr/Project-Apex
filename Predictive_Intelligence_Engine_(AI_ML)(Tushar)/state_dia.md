```
stateDiagram-v2
title Hub (Idle/Ready/Faulted)

[*] --> Uninitialized : system_start
Uninitialized --> Ready : config_loaded / validate_config()
Uninitialized --> Faulted : config_error / log_error()
Faulted --> Uninitialized : recover / reload_config()
Ready --> [*] : standby
```

```
stateDiagram-v2
title Data Ingestion

[*] --> Fetching
Fetching --> Validating : data_received / schema_check()
Fetching --> SourceError : timeout|auth_fail / alert()
Validating --> Normalized : valid / transform()
Validating --> ValidationError : invalid / quarantine()
Normalized --> Persisted : store_ok / upsert()
Persisted --> [*]
SourceError --> [*]
ValidationError --> [*]
```

```
stateDiagram-v2
title Training (DF + PM)

[*] --> Queued
Queued --> ForkTrain : capacity_ok / allocate()
Queued --> Deferred : capacity_busy / backoff()
Deferred --> Queued : window_open / retry()

state ForkTrain <<fork>>
ForkTrain --> DF_Training
ForkTrain --> PM_Training

state DF_Training {
  [*] --> Load
  Load --> Fit : data_ready / train_DF()
  Fit --> Eval : fit_done / eval_DF()
  Eval --> Trained : metrics_ok / register_DF()
  Eval --> Failed : metrics_bad / rollback_DF()
  Trained --> [*]
  Failed --> [*]
}

state PM_Training {
  [*] --> Load
  Load --> Fit : data_ready / train_PM()
  Fit --> Eval : fit_done / eval_PM()
  Eval --> Trained : metrics_ok / register_PM()
  Eval --> Failed : metrics_bad / rollback_PM()
  Trained --> [*]
  Failed --> [*]
}

state JoinTrain <<join>>
DF_Training.Trained --> JoinTrain
PM_Training.Trained --> JoinTrain
JoinTrain --> Completed : both_done / update_registry()
Completed --> [*]
```

```
stateDiagram-v2
title Prediction Router

[*] --> Routing
