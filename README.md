# Alibaba GPU Cluster Dataset 2025 Analysis

Detailed Analysis Traces for GPU-Disaggregated Deep Learning Recommendation Models

## GPU-Disaggregated DLRM Trace Dataset

This dataset records latency sensitive inference instances for GPU disaggregated serving of Deep Learning Recommendation Models. It contains per instance resource reservations and life cycle timestamps for scheduling analysis and capacity planning.

This dataset represents a groundbreaking trace collection from production GPU-disaggregated serving systems for Deep Learning Recommendation Models (DLRMs), accompanying the NSDI'25 paper on GPU-disaggregated serving at scale. The dataset captures real-world operational characteristics of inference services in a large-scale production environment, providing invaluable insights into resource allocation patterns, temporal dynamics, and system behavior for latency-sensitive ML workloads.

## Scope

- Total rows, 23871.
- Unique apps, 156.
- Role split, {'CN': 16485, 'HN': 7386}.

## Key fields

Instance id. Role. Application group. Requests and limits for CPU, GPU, RDMA, memory, disk. Density cap per node. Creation, scheduling, and deletion timestamps relative to the trace start.

## High level observations

- All workloads are marked latency sensitive. Instances are typically long running with high priority, as stated by the authors.
- Scheduling delay distribution and runtime distribution are included in the figures. Concurrency over time gives a view of system load.
- RDMA percentage and max instances per node expose placement constraints that influence packing on heterogeneous nodes.

## 🎯 Key Characteristics

### Scale and Scope

- **Total Instances**: 23,871 inference instances
- **Services**: 156 unique inference services (applications)
- **Workload Type**: 100% Latency-Sensitive (LS) workloads
- **Priority Level**: High-priority, long-running inference instances
- **System Architecture**: GPU-disaggregated architecture separating compute and GPU resources

### Instance Distribution

- **CPU Nodes (CN)**: 16,485 instances (69.1%)
  - Pure CPU-based inference workloads
  - No GPU allocation
  - Lower RDMA requirements (mean: 3.4%)
- **Heterogeneous GPU Nodes (HN)**: 7,386 instances (30.9%)
  - GPU-accelerated inference workloads
  - All instances allocated exactly 1 GPU
  - Higher RDMA requirements (mean: 20.5%)

## 💻 Resource Allocation Patterns

### CPU Resources

- **CN Instances**: Average 55.9 cores (range: 2-96 cores)
- **HN Instances**: Average 13.1 cores (range: 2-64 cores)
- Clear separation between CPU-intensive (CN) and GPU-accelerated (HN) workloads

### Memory Allocation

- **CN Instances**: Average 286.5 GiB (range: 4-600 GiB)
- **HN Instances**: Average 91.4 GiB (range: 4-460 GiB)
- Strong correlation (0.97) between CPU and memory allocation

### GPU Resources

- 30.9% of instances require GPU acceleration
- All GPU instances allocated exactly 1 GPU
- GPU instances exclusively on HN nodes

### RDMA Network

- Critical for disaggregated architecture communication
- **CN**: Lower bandwidth needs (1-50% of RNIC)
- **HN**: Higher bandwidth needs (1-100% of RNIC)
- Reflects data movement patterns in disaggregated systems

### Storage

- **CN Instances**: Average 269.9 GiB disk space
- **HN Instances**: Average 649.0 GiB disk space
- HN instances require 2.4x more storage (likely for model caching)

## ⏱️ Temporal Dynamics

### Trace Coverage

- Captures instance lifecycle events: creation, scheduling, and deletion
- Includes both pre-existing and newly created instances
- Represents production system behavior over extended period

### Scheduling Efficiency

- Most instances scheduled immediately upon creation
- Minimal scheduling delays observed
- Reflects optimized resource allocation in production

### Instance Lifetime

- Mix of short-lived and long-running instances
- Characteristic of production inference workloads
- Supports both batch and real-time serving patterns

## 🏗️ Deployment Constraints

### Density Limitations

- 56.7% of instances have no deployment density constraints
- Remaining instances have limits ranging from 1-32 instances per node
- Application-specific anti-affinity rules for fault tolerance

### Common Resource Profiles

Top 3 most frequent configurations:

1. **Profile 1** (8,236 instances): CN with 64 CPU cores, 320 GiB memory, 1% RDMA
2. **Profile 2** (3,593 instances): HN with 12 CPU cores, 1 GPU, 80 GiB memory, 25% RDMA
3. **Profile 3** (1,919 instances): CN with 48 CPU cores, 240 GiB memory, 1% RDMA

## 🔍 Key Insights

### Workload Heterogeneity

- Clear bimodal distribution between CPU and GPU workloads
- CN instances optimized for CPU-intensive operations
- HN instances balanced for GPU acceleration with supporting CPU resources

### Resource Efficiency

- Tight coupling between CPU and memory allocation (correlation: 0.97)
- Independent scaling of GPU resources from CPU/memory
- RDMA bandwidth scaled based on disaggregation communication needs

### Production Patterns

- All workloads classified as latency-sensitive
- High-priority, long-running inference services
- Immediate scheduling indicates sufficient resource availability

### Disaggregation Benefits

- Efficient resource utilization through separation of concerns
- CN nodes handle CPU-intensive preprocessing/postprocessing
- HN nodes focus on GPU-accelerated model inference
- RDMA enables efficient data movement between disaggregated components

## 📈 Research Applications

This dataset enables research in:

- **Resource Allocation**: Optimal scheduling strategies for disaggregated systems
- **Performance Modeling**: Understanding latency-throughput tradeoffs
- **System Design**: Architectural decisions for ML serving infrastructure
- **Workload Characterization**: Production DLRM inference patterns
- **Capacity Planning**: Resource provisioning for ML workloads
- **Fault Tolerance**: Instance distribution and anti-affinity strategies

## 🎓 Academic Contribution

This dataset represents one of the first publicly available production traces for GPU-disaggregated DLRM serving, providing:

- Real-world validation data for system research
- Baseline for performance comparisons
- Foundation for reproducible research in ML systems
- Insights into production-scale ML infrastructure

---

_This dataset provides a unique window into production GPU-disaggregated systems, offering researchers and practitioners valuable insights for advancing the field of large-scale ML serving infrastructure._

