# Kubestor Architecture

Kubestor is a Kubernetes-native distributed storage management system composed of several microservices that work together to provide scalable and reliable storage orchestration.

## System Components

### 1. Storage Node
- **Deployment Type**: DaemonSet (runs on each node)
- **Purpose**: Manages local storage resources on each Kubernetes node
- **Key Files**:
  - `storage_manager.py`: Handles local storage operations
  - `main.py`: Entry point for the storage node service
- **Responsibilities**:
  - Local storage provisioning and management
  - Storage health monitoring
  - Resource metrics collection

### 2. Controller
- **Deployment Type**: Deployment (single instance)
- **Purpose**: Central orchestration and consensus management
- **Key Components**:
  - `consensus.py`: Handles distributed consensus
  - `orchestrator.py`: Manages storage orchestration
  - `api.py`: Exposes REST API endpoints
- **Responsibilities**:
  - Storage pool orchestration
  - Consensus management
  - API endpoint provision

### 3. Operator
- **Deployment Type**: Deployment
- **Purpose**: Kubernetes operator for custom resource management
- **Key Files**:
  - `handlers.py`: Custom resource event handlers
  - `main.py`: Operator entry point
- **Responsibilities**:
  - Custom Resource Definition (CRD) management
  - Storage pool lifecycle management
  - Kubernetes event handling

### 4. UI
- **Deployment Type**: Deployment
- **Purpose**: Web interface for storage management
- **Key Features**:
  - Storage pool management interface
  - Resource monitoring dashboard
  - Status visualization
- **Components**:
  - React-based frontend
  - Material UI components
  - REST API client

## Component Interactions

1. **Storage Pool Creation Flow**:
   - User creates storage pool via UI
   - Request handled by Controller API
   - Operator processes CRD changes
   - Storage Nodes provision local resources

2. **Monitoring Flow**:
   - Storage Nodes collect metrics
   - Controller aggregates data
   - UI displays metrics and status

3. **Resource Management**:
   - Operator watches for CRD changes
   - Controller orchestrates changes
   - Storage Nodes execute operations

## Security

- RBAC policies for each component
- Secure communication between services
- Kubernetes-native security model

## Deployment Architecture

The system follows a microservices architecture pattern:
- Each component runs as a separate Kubernetes deployment
- Storage nodes run as DaemonSet for node-level operations
- Components communicate via Kubernetes service networking
- UI served through ingress for external access

## Scalability

- Storage Nodes scale horizontally with cluster nodes
- Controller uses consensus for reliability
- Operator handles multiple CRD instances
- UI deployment can scale based on load

## Development

The project uses:
- Python for backend services
- TypeScript/React for frontend
- Kubernetes manifests for deployment
- Docker for containerization