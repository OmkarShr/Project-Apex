gantt
    title Automated Storage Management and Retrieval System (AS/RS) Gateway — Plan (10 Oct 2025 → 20 Nov 2025)
    dateFormat  YYYY-MM-DD
    axisFormat  %d-%b
    excludes weekends

    %% --- Design refinements ---
    section Future Design Refinements
    Review System Architecture           :done,    des1, 2025-10-10, 3d
    Optimize Class Hierarchy             :active,  des2, after des1, 5d
    Refine UML (multiplicity, abstraction):         des3, after des2, 5d
    Update Protocol Translation Model    :         des4, after des3, 4d
    Finalize Design Documentation        :         des5, after des4, 3d

    %% milestone for design completion
    section Milestones
    Design Phase Complete                :milestone, m-des, 2025-11-03, 0d

    %% --- Implementation ---
    section Implementation of Design Parts
    Implement SoftwareGateway Module     :crit, impl1, 2025-11-04, 7d
    Implement ASRS Integration (Crane/Shuttle/VLM) :impl2, after impl1, 7d
    Implement Error & Protocol Classes   :impl3, after impl2, 4d
    Integrate with MasterRoboticsDispatcher :impl4, after impl3, 4d
    Testing and Validation               :impl5, after impl4, 6d
    Documentation & Deployment           :impl6, 2025-11-18, 3d

    %% implementation milestones
    Implementation Complete              :milestone, m-impl, 2025-11-17, 0d
    Project Complete                     :milestone, m-end, 2025-11-20, 0d
