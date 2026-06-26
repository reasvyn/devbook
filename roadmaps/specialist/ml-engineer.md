# ML Engineer

## Description

What an ML engineer should know — building and deploying ML models at scale, MLOps, data pipelines, model serving, monitoring, and bridging the gap between research and production.

## Prerequisites

- [Senior Backend Developer](../senior/backend-developer.md) or [Senior Data Scientist](../senior/data-scientist.md) — strong programming, statistics, and data fundamentals

## Learning Path

### Machine Learning Fundamentals

- `🔴 CRITICAL` Supervised learning — regression, classification, tree-based models
- `🔴 CRITICAL` Unsupervised learning — clustering, dimensionality reduction
- `🔴 CRITICAL` Model evaluation — cross-validation, metrics (precision, recall, AUC, RMSE)
- `🔴 CRITICAL` Feature engineering — encoding, scaling, feature selection
- `🟠 HIGH` Deep learning basics — neural networks, CNNs, RNNs, transformers
- `🟠 HIGH` Hyperparameter tuning — grid search, random search, Bayesian optimization

### MLOps & Infrastructure

- `🔴 CRITICAL` ML pipeline orchestration — Kubeflow, MLflow, Airflow
- `🔴 CRITICAL` Experiment tracking — tracking parameters, metrics, artifacts
- `🔴 CRITICAL` Model versioning — DVC, Model Registry, containerized models
- `🔴 CRITICAL` Feature stores — Feast, Tecton, unified feature serving
- `🟠 HIGH` Training infrastructure — distributed training, GPU/TPU management
- `🟠 HIGH` Data versioning — lakeFS, DVC, Quilt

### Model Serving

- `🔴 CRITICAL` Batch inference — scheduled predictions at scale
- `🔴 CRITICAL` Real-time serving — REST endpoints, gRPC, TensorFlow Serving
- `🔴 CRITICAL` Model optimization — quantization, pruning, ONNX, TensorRT
- `🟠 HIGH` A/B testing models in production — shadow deployment, canary
- `🟠 HIGH` Model caching and prediction batching
- `🟡 MEDIUM` Edge deployment — TensorFlow Lite, Core ML, ONNX Runtime

### Monitoring & Observability

- `🔴 CRITICAL` Data drift detection — input distribution changes over time
- `🔴 CRITICAL` Model drift detection — prediction quality degradation
- `🔴 CRITICAL` Model performance monitoring — latency, throughput, error rate
- `🟠 HIGH` Automated retraining — triggering retrain on drift signals
- `🟠 HIGH` Explainability — SHAP, LIME, feature importance
- `🟡 MEDIUM` Fairness and bias detection — auditing model predictions

### Data Engineering

- `🔴 CRITICAL` Building training data pipelines — cleaning, transformation, labeling
- `🔴 CRITICAL` Feature computation — batch and streaming feature generation
- `🔴 CRITICAL` Handling imbalanced data — resampling, class weights, synthetic data
- `🟠 HIGH` Data quality for ML — schema validation, anomaly detection in features
- `🟠 HIGH` Large-scale data processing — Spark, Dask, Ray

### Production Engineering

- `🔴 CRITICAL` CI/CD for ML — testing data, features, models, and infrastructure
- `🔴 CRITICAL` Containerized ML — Docker, Kubernetes for model deployment
- `🔴 CRITICAL` Model governance — audit trails, approval workflows, compliance
- `🟠 HIGH` Cost optimization — compute cost per model, auto-scaling inference
- `🟠 HIGH` Multi-model serving — sharing infrastructure across models

### Collaboration

- `🔴 CRITICAL` Working with data scientists — productionizing research code
- `🔴 CRITICAL` Working with product teams — defining ML success metrics
- `🔴 CRITICAL` Documentation — model cards, data sheets, runbooks
- `🟠 HIGH` Code review for ML — reviewing data pipelines, model logic, infrastructure
- `🟠 HIGH` Stakeholder communication — explaining model behavior and limitations

## Next Steps

- [Software Architect](software-architect.md) — ML platform architecture at org scale
- [Engineering Manager](engineering-manager.md) — leading ML teams
