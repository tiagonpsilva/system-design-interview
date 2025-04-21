# 📊 Design de Sistema: Sistema de Processamento de Logs

```mermaid
graph TD
    subgraph Log Sources
        A[Applications]
        B[Services]
        C[Infrastructure]
        D[Security]
    end

    subgraph Collection Layer
        E[Vector Agents]
        F[Fluentd Collectors]
        G[Filebeat]
    end

    subgraph Ingestion Layer
        H[Kafka Cluster]
        I[Schema Validator]
        J[Rate Limiter]
    end

    subgraph Processing Layer
        K[Flink Streaming]
        L[Parser Service]
        M[Enrichment Service]
    end

    subgraph Storage Layer
        N[(Elasticsearch - Hot)]
        O[(S3/Parquet - Cold)]
        P[(Redis - Cache)]
        Q[Prometheus - Metrics]
    end

    subgraph Query Layer
        R[Search Service]
        S[Analytics Service]
        T[Archival Service]
    end

    subgraph Presentation Layer
        U[API Gateway]
        V[Kibana/Grafana]
        W[Alert Manager]
    end

    A & B & C & D --> E & F & G
    E & F & G --> H
    H --> I --> J
    J --> K
    K --> L --> M
    
    M --> N & O
    M --> Q
    K --> P
    
    R --> N & P
    S --> N & O & Q
    T --> O
    
    U --> R & S & T
    V --> R & S
    W --> S

    style A fill:#f9f,stroke:#333,stroke-width:2px
    style B fill:#f9f,stroke:#333,stroke-width:2px
    style C fill:#f9f,stroke:#333,stroke-width:2px
    style D fill:#f9f,stroke:#333,stroke-width:2px
    style E fill:#bbf,stroke:#333,stroke-width:2px
    style F fill:#bbf,stroke:#333,stroke-width:2px
    style G fill:#bbf,stroke:#333,stroke-width:2px
    style H fill:#dfd,stroke:#333,stroke-width:2px
    style I fill:#dfd,stroke:#333,stroke-width:2px
    style J fill:#dfd,stroke:#333,stroke-width:2px
```

## 1. Requisitos & Escopo

### 1.1 Perguntas Chave
- [x] Objetivo principal: Coleta, processamento e análise de logs em larga escala
- [x] Usuários principais: Desenvolvedores, SREs, analistas
- [x] Volume: 100TB/dia de logs
- [x] Retenção: 90 dias
- [x] Disponibilidade: 99.95%

### 1.2 Requisitos Funcionais
- [x] Ingestão de logs
- [x] Parsing & estruturação
- [x] Indexação & busca
- [x] Agregações & métricas
- [x] Alertas & notificações
- [x] Retenção & archival
- [x] Visualização

### 1.3 Requisitos Não-Funcionais
- [x] Disponibilidade: 99.95%
- [x] Latência ingestão: < 5s
- [x] Latência busca: < 3s
- [x] Durabilidade: 99.999999%
- [x] Consistência: Eventual

### 1.4 Estimativas
- [x] Volume: 100TB/dia
- [x] Throughput: 1M eventos/s
- [x] Storage: 9PB total
- [x] Network: 100Gbps
- [x] Queries: 10k/s

### 1.5 Restrições & Limitações
- [x] Custo de storage
- [x] Network bandwidth
- [x] Complexidade de queries
- [x] Tempo de retenção
- [x] Regulamentações

## 2. Design de Alto Nível

### 2.1 Componentes Principais
- [x] Collectors
- [x] Parser Service
- [x] Stream Processor
- [x] Index Service
- [x] Query Service
- [x] Storage Service
- [x] Alert Service
- [x] API Gateway

### 2.2 Fluxos de Dados
```mermaid
graph LR
    A[Apps] --> B[Collectors]
    B --> C[Parser]
    C --> D[Stream]
    D --> E[Storage]
    D --> F[Index]
    F --> G[Query]
    D --> H[Alert]
```

### 2.3 APIs & Interfaces
```typescript
interface ILogService {
  ingest(logs: LogEntry[]): Promise<r>;
  search(query: Query): Promise<SearchResult>;
  aggregate(query: AggQuery): Promise<AggResult>;
  getStats(timeRange: Range): Promise<Stats>;
  createAlert(alert: Alert): Promise<void>;
  archive(criteria: Criteria): Promise<void>;
  
}

interface LogEntry {
  timestamp: Date;
  source: string;
  level: string;
  message: string;
  metadata: Record<string, any>;
}

interface Query {
  timeRange: Range;
  filters: Filter[];
  fields: string[];
  sort?: Sort;
  limit?: number;
}
```

### 2.4 Modelo de Dados
- [x] Log Entry
```typescript
interface LogDocument {
  id: string;          // UUID
  timestamp: number;   // Unix timestamp
  source: string;      // Log source
  level: string;       // Log level
  message: string;     // Raw message
  parsed: any;         // Parsed fields
  metadata: {          // Metadata
    host: string;
    service: string;
    version: string;
    region: string;
    tags: string[];
  };
  index: string;       // Index name
  shard: number;       // Shard number
}

interface Index {
  name: string;        // Index name
  mapping: Schema;     // Field mapping
  settings: Settings;  // Index settings
  stats: Stats;        // Index stats
}
```

## 3. Design Detalhado

### 3.1 Tecnologias Específicas
- [x] Ingestão: Vector/Fluentd
- [x] Stream: Kafka
- [x] Processing: Flink
- [x] Storage: S3 + Parquet
- [x] Index: Elasticsearch
- [x] Query: Presto
- [x] Metrics: Prometheus
- [x] Visualization: Grafana

### 3.2 Padrões de Design
- [x] Lambda Architecture
- [x] Time-based Partitioning
- [x] Write-ahead Logging
- [x] Compaction
- [x] Materialized Views
- [x] Circuit Breaker

### 3.3 Trade-offs
| Decisão | Prós | Contras | Justificativa |
|---------|------|---------|---------------|
| S3 Storage | Custo, Durabilidade | Latência | Dados frios |
| Elasticsearch | Flexibilidade | Recursos | Busca complexa |
| Parquet | Compressão | Complexidade | Análise eficiente |

## 4. Escalabilidade

### 4.1 Gargalos
- [x] Ingestão rate
- [x] Index size
- [x] Query complexity
- [x] Storage capacity

### 4.2 Soluções
- [x] Sharding
- [x] Compression
- [x] Caching
- [x] Partitioning
- [x] Query optimization

### 4.3 Custos
- [x] Infra: ~$500k/mês
  - Storage: $300k
  - Compute: $150k
  - Network: $50k

## 5. Resiliência

### 5.1 Pontos de Falha
- [x] Collectors
- [x] Kafka cluster
- [x] Processing jobs
- [x] Storage nodes
- [x] Query nodes

### 5.2 Estratégias
- [x] Multi-AZ
- [x] Auto-scaling
- [x] Circuit breakers
- [x] Rate limiting
- [x] Retry policies
- [x] Dead letter queues
- [x] Fallback paths

### 5.3 Disaster Recovery
- [x] RPO: 15 minutos
- [x] RTO: 1 hora
- [x] Backup strategy
- [x] Recovery playbooks
- [x] Failover tests

## 6. Monitoramento

### 6.1 Métricas
- [x] Ingestão: rate, latência, erros
- [x] Processing: throughput, lag, falhas
- [x] Storage: utilização, IOPS, latência
- [x] Queries: rate, latência, timeouts
- [x] Alertas: volume, latência

### 6.2 Dashboards
- [x] Operacional
- [x] Capacidade
- [x] SLOs
- [x] Custos
- [x] Alertas

### 6.3 Alertas
- [x] Thresholds
- [x] Anomalias
- [x] SLO breaches
- [x] Custos
- [x] Segurança

## 7. Evolução

### 7.1 Melhorias Futuras
- [x] ML para anomalias
- [x] Query optimization
- [x] Schema evolution
- [x] Cost optimization
- [x] Security enhancements

### 7.2 Roadmap
1. MVP com features core
2. Otimizações de performance
3. Melhorias de UX
4. Integrações extras
5. Features avançadas