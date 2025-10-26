classDiagram
   class PredictiveIntelligenceEngine {
       +dict models
       +ModelTrainer model_trainer
       +InventoryOptimizationModule inventory_module
       +AlertingService alert_service
       +ReportingDashboard dashboard
       --
       +forecast_demand(product_id, horizon): Prediction
       +predict_maintenance(equipment_id): Prediction
       +train_model(model_type, data_source)
       +monitor_data_drift()
       +trigger_retraining(model_id)
       +configure_alerts()
       +provide_insights()
   }


   class AI_MLModel {
       <<abstract>>
       +string model_id
       +bool trained
       --
       +train(data)
       +predict(input_data): dict
       +evaluate(metrics): dict
       +save(path)
       +load(path)
   }


   class DemandForecastingModel {
       +dict seasonality_params
       --
       +train(sales_data)
       +predict(product_id, horizon): dict
   }


   class PredictiveMaintenanceModel {
       +dict maintenance_history
       --
       +train(equipment_data)
       +predict(equipment_id): dict
   }


   class ModelTrainer {
       +list trained_models
       --
       +train(model, data_source)
       +evaluate_model(model, validation_data)
   }


   class Prediction {
       +string prediction_id
       +string model_id
       +dict input_data
       +dict output_data
       +datetime timestamp
   }


   class DataSource {
       <<abstract>>
       --
       +get_data(): dict
   }


   class SalesDataSource {
       --
       +get_data(): dict
   }


   class EquipmentDataSource {
       --
       +get_data(): dict
   }


   class InventoryOptimizationModule {
       +update_inventory(demand_data)
   }


   class AlertingService {
       +send_alert(event_type, data)
   }


   class ReportingDashboard {
       +generate_report(model_id, period)
   }


   PredictiveIntelligenceEngine "1" *-- "many" AI_MLModel : aggregates
   PredictiveIntelligenceEngine "1" o-- "1" ModelTrainer : associates with
   PredictiveIntelligenceEngine "1" o-- "1" InventoryOptimizationModule : associates with
   PredictiveIntelligenceEngine "1" o-- "1" AlertingService : associates with
   PredictiveIntelligenceEngine "1" o-- "1" ReportingDashboard : associates with


   AI_MLModel <|-- DemandForecastingModel
   AI_MLModel <|-- PredictiveMaintenanceModel


   ModelTrainer "1" o-- "1" AI_MLModel : trains
   ModelTrainer "1" o-- "1" DataSource : gets data from


   DataSource <|-- SalesDataSource
   DataSource <|-- EquipmentDataSource


   PredictiveIntelligenceEngine --> Prediction : produces
   DemandForecastingModel --> Prediction : produces
   PredictiveMaintenanceModel --> Prediction : produces


   DemandForecastingModel ..> SalesDataSource : uses data from
   PredictiveMaintenanceModel ..> EquipmentDataSource : uses data from


   PredictiveIntelligenceEngine "1" o-- "1" InventoryOptimizationModule : informs
   PredictiveIntelligenceEngine "1" o-- "1" AlertingService : triggers alerts
   PredictiveIntelligenceEngine "1" o-- "1" ReportingDashboard : provides insights
