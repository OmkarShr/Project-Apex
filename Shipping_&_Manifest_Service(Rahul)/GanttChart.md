```mermaid
gantt
    title Project Schedule: Apex Shipping Manifest Service
    dateFormat  YYYY-MM-DD
    axisFormat  %d/%m
    excludes    weekends
    section Future Design Refinements
    Requirements review           :active, req_review, 2025-10-04, 7d
    Business logic fine-tuning    :finetune, after req_review, 7d
    API contract updates          :api_update, after finetune, 7d
    Documentation improvements    :docs_update, after api_update, 7d

    section Implementation
    Carrier API integration       :active, api_integration, 2025-11-01, 5d
    Label/manifest engine         :label_engine, after api_integration, 5d
    Order-packing hooks           :packing_hooks, after label_engine, 5d
    Customer notifications        :notify, after packing_hooks, 5d

    %% Final milestone
    Final completion              :milestone, complete, 2025-11-20, 0d
    %% Vertical marker for deadline
    verticleLine deadline         :2025-11-20
```
