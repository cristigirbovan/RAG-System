# Enhanced RAG System Architecture Document

## 1. Overview and Goals

### 1.1 System Purpose
The Enhanced RAG (Retrieval Augmented Generation) System is designed to provide high-quality, contextually relevant responses to user queries by leveraging multiple information retrieval methods combined with large language models (LLMs). The system offers a production-grade, highly scalable solution for enterprise knowledge management and question-answering capabilities.

### 1.2 Key Goals
- **Information Accuracy**: Ensure responses are grounded in factual information from trusted sources
- **Contextual Understanding**: Maintain context across multiple query techniques
- **Performance and Scalability**: Support high throughput with low latency
- **Security and Compliance**: Protect sensitive information and adhere to enterprise standards
- **Observability**: Comprehensive monitoring and evaluation metrics
- **Extensibility**: Support for multiple data modalities (text, images, audio, video)

## 2. System Architecture

### 2.1 High-Level Architecture
The Enhanced RAG System follows a microservices-based architecture with the following major components:

1. **API Layer**: REST endpoints for user interaction and administration
2. **Core RAG Engine**: Processing pipeline for retrieval and generation
3. **Document Processing**: Multi-modal content processing and storage
4. **Storage Layer**: Vector database and graph database for different retrieval methods
5. **Evaluation System**: Continuous evaluation and feedback incorporation
6. **Monitoring and Observability**: Metrics, logging, and tracing infrastructure

### 2.2 Architecture Diagram
See the attached PlantUML diagram for a detailed visual representation of the system architecture.

## 3. Component Details

### 3.1 API Layer
The API layer provides RESTful endpoints for interacting with the RAG system:

- **RagApiController**: Main controller for query processing and document ingestion
  - `/api/v1/query`: Process queries and generate responses
  - `/api/v1/query/stream`: Stream responses in real-time
  - `/api/v1/ingest`: Ingest documents into the system
  - `/api/v1/feedback/{queryId}`: Submit user feedback
  - `/api/v1/health`: Health check endpoint

- **AdminController**: Administrative operations
  - `/api/v1/admin/stats`: System statistics
  - `/api/v1/admin/reindex`: Trigger reindexing
  - `/api/v1/admin/evaluation/metrics`: Get evaluation metrics

- **Security Components**:
  - API key authentication
  - Role-based access control
  - Rate limiting

### 3.2 Core RAG Engine

#### 3.2.1 EnhancedRagService
Orchestrates the overall RAG workflow:
- Processes user queries
- Coordinates retrieval strategies
- Generates responses using LLMs
- Handles streaming responses

#### 3.2.2 Retrieval Pipeline
Advanced retrieval system with multiple strategies:
- **VectorStoreFactory**: Manages different vector stores
- **EnhancedWeaviateVectorStore**: Optimized vector similarity search
- **EnhancedNeo4jGraphRepository**: Graph-based context retrieval
- **RetrievalStrategy**: Interface for different retrieval methods
  - HybridStrategy: Combines vector and graph retrieval
  - HydeStrategy: Hypothetical Document Embedding
  - DecompositionStrategy: Query decomposition
  - AdvancedStrategy: Comprehensive strategy using all techniques

#### 3.2.3 LLM Integration
Integrates with OpenAI models through Spring AI:
- Query processing
- Response generation
- Entity extraction
- Document analysis

### 3.3 Document Processing

#### 3.3.1 DocumentProcessor
Processes text-based documents:
- Document chunking with configurable strategies
- Semantic chunking
- Section-aware splitting
- Metadata extraction

#### 3.3.2 MultiModalProcessor
Handles non-text content:
- Image processing and description
- Audio transcription
- Video processing

#### 3.3.3 EntityExtractor
Extracts entities and relationships from documents:
- Named entity recognition
- Relationship extraction
- Topic identification
- Caching for performance

### 3.4 Storage Layer

#### 3.4.1 Vector Database (Weaviate)
Stores document embeddings for similarity search:
- High-performance vector operations
- Efficient batch processing
- Configurable similarity algorithms
- Metadata filtering

#### 3.4.2 Graph Database (Neo4j)
Stores knowledge graph for semantic relationships:
- Document nodes
- Entity nodes
- Typed relationships
- Graph traversal capabilities

#### 3.4.3 Relational Database (PostgreSQL)
Stores system metadata:
- Evaluation results
- User feedback
- System statistics

### 3.5 Evaluation System

#### 3.5.1 EvaluationService
Evaluates RAG performance:
- Multiple evaluation metrics (relevance, completeness, faithfulness)
- Asynchronous evaluation
- Human feedback incorporation

#### 3.5.2 EvaluationRepository
Persists evaluation results:
- Multiple storage backends (database, file, in-memory)
- Historical data retention
- Statistical aggregation

### 3.6 Monitoring and Observability

#### 3.6.1 MetricsService
Collects performance metrics:
- Query processing times
- Document processing statistics
- Retrieval effectiveness
- Resource utilization

#### 3.6.2 Integrated Monitoring Stack
- Prometheus for metrics collection
- Grafana for visualization
- Loki for log aggregation
- Tempo for distributed tracing

## 4. Data Flow

### 4.1 Document Ingestion Flow
1. Documents are uploaded via the API
2. Multi-modal processing identifies document type and extracts content
3. Content is processed and split into chunks
4. Entity extraction identifies entities and relationships
5. Document embeddings are generated and stored in vector database
6. Knowledge graph is updated with document and entity nodes

### 4.2 Query Processing Flow
1. User submits query via API
2. Query is analyzed and potentially transformed or decomposed
3. Retrieval pipeline fetches relevant context:
   - Vector database provides similarity matches
   - Graph database provides semantic relationships
4. Context is assembled and formatted
5. LLM generates response based on context and query
6. Response is returned to user (standard or streaming)
7. (Optional) Response is evaluated for quality

### 4.3 Evaluation Flow
1. RAG response is generated
2. Asynchronous evaluation assesses quality metrics
3. Results are stored for monitoring and improvement
4. User feedback is incorporated into evaluation data

## 5. Technology Stack

### 5.1 Core Technologies
- **Programming Language**: Java 17
- **Framework**: Spring Boot 3.2.x
- **AI Integration**: Spring AI
- **Build System**: Gradle

### 5.2 Databases
- **Vector Database**: Weaviate
- **Graph Database**: Neo4j
- **Relational Database**: PostgreSQL
- **Caching**: Redis + Caffeine

### 5.3 AI Models
- **Text Embedding**: OpenAI text-embedding-ada-002
- **Chat Completion**: OpenAI GPT-4 or equivalent
- **Vision Analysis**: OpenAI Vision API

### 5.4 Monitoring and Observability
- **Metrics**: Prometheus + Micrometer
- **Visualization**: Grafana
- **Logging**: Logback + Loki
- **Tracing**: OpenTelemetry + Tempo

### 5.5 Infrastructure
- **Containerization**: Docker
- **Orchestration**: Kubernetes
- **CI/CD**: GitHub Actions
- **Load Balancing**: NGINX or Kubernetes Ingress
- **SSL Termination**: Cert-Manager

## 6. Deployment Architecture

### 6.1 Kubernetes Deployment
The system is designed for deployment in Kubernetes with the following components:
- RAG Application Deployment (with auto-scaling)
- Neo4j StatefulSet
- Weaviate StatefulSet
- PostgreSQL StatefulSet
- Monitoring stack (Prometheus, Grafana, etc.)
- Ingress for external access

### 6.2 Resource Requirements
Minimum recommended resources:
- **RAG Application**: 1-2 CPU cores, 2-4GB RAM per instance
- **Neo4j**: 4 CPU cores, 8GB RAM
- **Weaviate**: 4 CPU cores, 8GB RAM
- **PostgreSQL**: 2 CPU cores, 4GB RAM
- **Storage**: 100GB for vector database, 100GB for graph database

### 6.3 Scaling Approach
- Horizontal scaling for the RAG application
- Vertical scaling for databases with potential for clustering
- Auto-scaling based on CPU and memory metrics

## 7. Security Architecture

### 7.1 Authentication and Authorization
- API Key authentication for service access
- Role-based access control for administrative functions
- JWT token support for web application integration

### 7.2 Network Security
- TLS encryption for all communications
- Internal service communications within Kubernetes network
- Ingress with SSL termination

### 7.3 Data Protection
- Encrypted storage for sensitive data
- Secrets management with Kubernetes secrets
- Controlled access to document repositories

### 7.4 Rate Limiting and DoS Protection
- Token bucket algorithm for rate limiting
- Configurable limits per client/API key
- Circuit breakers for dependent services

## 8. Performance Optimization

### 8.1 Retrieval Optimization
- Efficient vector indexing with HNSW algorithm
- Graph database indexing for common query patterns
- Caching for frequent queries and embeddings

### 8.2 Processing Optimization
- Asynchronous document processing
- Batch processing for vector operations
- Parallel entity extraction

### 8.3 Response Optimization
- Streaming responses for long-running generations
- Context truncation to manage token limits
- Adjustable retrieval parameters based on query characteristics

## 9. Scalability Approach

### 9.1 Horizontal Scalability
- Stateless application tier for easy scaling
- Connection pooling for database connections
- Kubernetes autoscaling based on metrics

### 9.2 Vertical Scalability
- Configurable JVM memory settings
- Database resource allocation based on workload
- Optimized container resource requests/limits

### 9.3 Data Volume Handling
- Efficient chunking strategies for large documents
- Pagination for large result sets
- Incremental indexing for large document repositories

## 10. Monitoring and Observability

### 10.1 Application Metrics
- Query processing times
- Retrieval effectiveness
- Document processing statistics
- Evaluation scores

### 10.2 System Metrics
- JVM metrics (memory, GC, threads)
- Database performance
- API endpoint latency
- Resource utilization

### 10.3 Business Metrics
- Query success rates
- User satisfaction (via feedback)
- Content coverage
- Response quality trends

### 10.4 Alerting
- Error rate thresholds
- Latency thresholds
- Resource utilization warnings
- Integration with incident management systems

## 11. Extension Points

### 11.1 Additional Data Sources
- Integration with document management systems
- Web crawling capabilities
- Structured database connectors

### 11.2 Enhanced Retrieval Methods
- Support for additional vector databases
- Alternative embedding models
- Custom retrieval algorithms

### 11.3 Additional Modalities
- Enhanced support for tabular data
- Structured form extraction
- Interactive visualization

## 12. Operational Considerations

### 12.1 Backup and Recovery
- Database backup strategies
- Configuration version control
- Disaster recovery plan

### 12.2 Maintenance
- Reindexing procedures
- Model update strategy
- Database optimization tasks

### 12.3 Troubleshooting
- Centralized logging
- Tracing for request flows
- Diagnostic endpoints

## 13. Future Enhancements

### 13.1 Advanced Features
- Conversation memory/history
- Active learning from feedback
- Domain-specific fine-tuning

### 13.2 Integration Options
- API Gateway integration
- SSO/SAML authentication
- Enterprise data connectors

### 13.3 User Experience
- Interactive UI components
- Feedback collection improvements
- Response explanation features

```planmtuml
@startuml Enhanced RAG System Architecture

!define RECTANGLE_COLOR #F8F8F8
!define SERVICE_COLOR #C5E1A5
!define DATABASE_COLOR #BBDEFB
!define CLIENT_COLOR #FFE082
!define EXTERNAL_COLOR #D1C4E9
!define CONTAINER_COLOR #DCEDC8
!define MONITORING_COLOR #FFCCBC

skinparam componentStyle rectangle
skinparam component {
  BackgroundColor RECTANGLE_COLOR
  BorderColor black
  ArrowColor black
}

skinparam database {
  BackgroundColor DATABASE_COLOR
  BorderColor black
}

skinparam rectangle {
  BackgroundColor RECTANGLE_COLOR
  BorderColor black
  RoundCorner 10
}

skinparam cloud {
  BackgroundColor EXTERNAL_COLOR
  BorderColor black
}

' Top-level components
package "Client Layer" as ClientLayer {
  [Web Browser] as WebClient #CLIENT_COLOR
  [Mobile App] as MobileClient #CLIENT_COLOR
  [API Client] as ApiClient #CLIENT_COLOR
}

package "Infrastructure" as Infra {
  [Load Balancer] as LoadBalancer #RECTANGLE_COLOR
  [API Gateway] as ApiGateway #RECTANGLE_COLOR
  
  rectangle "Kubernetes Cluster" as K8s #CONTAINER_COLOR {
    package "External Services" as ExternalSvcs {
      [OpenAI API] as OpenAI #EXTERNAL_COLOR
      [Document Storage] as DocStorage #EXTERNAL_COLOR
    }
    
    package "API Layer" as ApiLayer {
      [RagApiController] as RagController #SERVICE_COLOR
      [AdminController] as AdminController #SERVICE_COLOR
      [UiSupportController] as UIController #SERVICE_COLOR
      [SecurityFilters] as Security #SERVICE_COLOR
    }
    
    package "Core RAG Engine" as CoreEngine {
      [EnhancedRagService] as RagService #SERVICE_COLOR
      [RetrievalPipeline] as RetrievalPipeline #SERVICE_COLOR
      [RetrievalStrategyFactory] as StrategyFactory #SERVICE_COLOR
      
      rectangle "Retrieval Strategies" as Strategies {
        [HybridStrategy] as HybridStrategy #SERVICE_COLOR
        [HydeStrategy] as HydeStrategy #SERVICE_COLOR
        [DecompositionStrategy] as DecompositionStrategy #SERVICE_COLOR
        [AdvancedStrategy] as AdvancedStrategy #SERVICE_COLOR
      }
    }
    
    package "Document Processing" as DocProcessing {
      [DocumentProcessor] as DocProcessor #SERVICE_COLOR
      [MultiModalProcessor] as MultiModalProcessor #SERVICE_COLOR
      [EntityExtractor] as EntityExtractor #SERVICE_COLOR
    }
    
    package "Evaluation System" as EvalSystem {
      [EvaluationService] as EvalService #SERVICE_COLOR
      [EvaluationRepository] as EvalRepo #SERVICE_COLOR
    }
    
    package "Storage Layer" as StorageLayer {
      database "Vector DB (Weaviate)" as VectorDB #DATABASE_COLOR {
        [EnhancedWeaviateVectorStore] as WeaviateStore #SERVICE_COLOR
        [VectorStoreFactory] as VectorFactory #SERVICE_COLOR
      }
      
      database "Graph DB (Neo4j)" as GraphDB #DATABASE_COLOR {
        [EnhancedNeo4jGraphRepository] as Neo4jRepo #SERVICE_COLOR
      }
      
      database "Relational DB (PostgreSQL)" as RelationalDB #DATABASE_COLOR {
        [EvaluationStorage] as EvalStorage #DATABASE_COLOR
      }
      
      database "Cache (Redis)" as CacheDB #DATABASE_COLOR {
        [QueryCache] as QueryCache #DATABASE_COLOR
        [EntityCache] as EntityCache #DATABASE_COLOR
      }
    }
    
    package "Monitoring and Observability" as MonitoringLayer {
      [MetricsService] as MetricsService #MONITORING_COLOR
      [Prometheus] as Prometheus #MONITORING_COLOR
      [Grafana] as Grafana #MONITORING_COLOR
      [Loki] as Loki #MONITORING_COLOR
      [Tempo] as Tempo #MONITORING_COLOR
      [OpenTelemetry Collector] as OtelCollector #MONITORING_COLOR
    }
  }
}

' Relationships - Client Layer
WebClient --> LoadBalancer
MobileClient --> LoadBalancer
ApiClient --> LoadBalancer
LoadBalancer --> ApiGateway
ApiGateway --> RagController
ApiGateway --> AdminController
ApiGateway --> UIController

' Relationships - API Layer
RagController --> Security
AdminController --> Security
UIController --> Security
Security --> RagService
Security --> EvalService

' Relationships - Core Engine
RagController --> RagService
RagService --> RetrievalPipeline
RagService --> OpenAI
RagService --> DocProcessor
RagService --> MultiModalProcessor
RagService --> EvalService
RetrievalPipeline --> StrategyFactory
StrategyFactory -down-> HybridStrategy
StrategyFactory -down-> HydeStrategy
StrategyFactory -down-> DecompositionStrategy
StrategyFactory -down-> AdvancedStrategy
HybridStrategy -down-> VectorFactory
HybridStrategy -down-> Neo4jRepo
HydeStrategy -down-> VectorFactory
HydeStrategy -down-> OpenAI
DecompositionStrategy -down-> VectorFactory
DecompositionStrategy -down-> OpenAI
AdvancedStrategy -down-> VectorFactory
AdvancedStrategy -down-> Neo4jRepo
AdvancedStrategy -down-> OpenAI

' Relationships - Document Processing
DocProcessor --> EntityExtractor
MultiModalProcessor --> DocProcessor
MultiModalProcessor --> OpenAI
DocProcessor --> DocStorage
RagController -left-> DocProcessor : Document Ingestion

' Relationships - Storage Layer
VectorFactory -down-> WeaviateStore
RetrievalPipeline --> WeaviateStore
RetrievalPipeline --> Neo4jRepo
DocProcessor --> WeaviateStore
DocProcessor --> Neo4jRepo
EntityExtractor -right-> Neo4jRepo : Entity Storage
EvalRepo --> EvalStorage
EntityExtractor -down-> EntityCache : Cache Entities
RagService -down-> QueryCache : Cache Responses

' Relationships - Evaluation System
AdminController -right-> EvalService : Admin Operations
EvalService -down-> EvalRepo
RagService -right-> EvalService : Auto-evaluate

' Relationships - Monitoring
RagService -up-> MetricsService : Record Metrics
DocProcessor -up-> MetricsService : Record Metrics
RetrievalPipeline -up-> MetricsService : Record Metrics
EvalService -up-> MetricsService : Record Metrics
MetricsService -right-> Prometheus : Export Metrics
Prometheus -right-> Grafana : Visualization
OtelCollector -right-> Tempo : Tracing
OtelCollector -right-> Loki : Logging
OtelCollector -right-> Prometheus : Metrics

' Data Flow Labels
WebClient -down-> LoadBalancer : "User Queries"
RagController -down-> RagService : "Process Query"
RagService -down-> RetrievalPipeline : "Retrieve Context"
RagService -right-> OpenAI : "Generate Response"
RetrievalPipeline -down-> WeaviateStore : "Vector Similarity Search"
RetrievalPipeline -down-> Neo4jRepo : "Knowledge Graph Query"
DocProcessor -down-> WeaviateStore : "Store Embeddings"
DocProcessor -right-> Neo4jRepo : "Update Knowledge Graph"
EntityExtractor -right-> OpenAI : "Extract Entities"

legend right
  **Enhanced RAG System Architecture**
  
  Color Legend:
  <back:CLIENT_COLOR>Client Components</back>
  <back:SERVICE_COLOR>Service Components</back>
  <back:DATABASE_COLOR>Data Storage</back>
  <back:EXTERNAL_COLOR>External Services</back>
  <back:MONITORING_COLOR>Monitoring Components</back>
  <back:CONTAINER_COLOR>Infrastructure</back>
endlegend

@enduml
```
