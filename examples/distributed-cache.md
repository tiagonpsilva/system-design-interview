# 🗄️ Design de Sistema: Distributed Cache

## 1. Requisitos & Escopo

### Perguntas Chave
- Qual o volume de dados a ser cacheado?
- Quais os padrões de acesso esperados?
- Requisitos de consistência?
- Política de evicção necessária?
- Necessidade de persistência?

### Requisitos Funcionais
- Get/Set operações básicas
- TTL por chave
- Batch operations
- Cache invalidation
- Estatísticas de uso
- Monitoramento

### Requisitos Não-Funcionais
- Latência < 1ms p99
- Disponibilidade 99.99%
- Escalabilidade horizontal
- Consistência eventual
- Tolerância a falhas

### Estimativas
- 100GB dados total
- 100K QPS
- 1KB média por item
- 100K chaves únicas
- 1ms max latência

## 2. Design de Alto Nível

### Componentes Principais
- Cache Nodes
- Proxy Layer
- Client SDK
- Monitoring Service
- Config Service

### Fluxo de Dados
```
[Cliente] -> [Client SDK]
  -> [Proxy Layer]
    -> [Cache Nodes]
      -> [Storage Layer]
```

### APIs
```
# Cache Service API
SET key value [EX seconds]
GET key
DEL key
MGET key [key ...]
MSET key value [key value ...]
EXPIRE key seconds
STATS

# Admin API
ADD_NODE node_id config
REMOVE_NODE node_id
REBALANCE
GET_STATS
```

### Modelo de Dados
```
# Cache Entry
{
  "key": string,
  "value": bytes,
  "ttl": int,
  "created_at": timestamp,
  "last_accessed": timestamp,
  "version": int
}

# Node Config
{
  "node_id": string,
  "host": string,
  "port": int,
  "capacity": int,
  "region": string,
  "status": string
}
```

## 3. Design Detalhado

### Tecnologias
- Redis como base
- Envoy para proxy
- gRPC para comunicação
- Prometheus para métricas
- Consul para service discovery

### Padrões de Design
- Consistent hashing
- Write-through cache
- Circuit breaker
- Bulkhead pattern
- Sidecar pattern

### Trade-offs
- Consistência vs Disponibilidade
  - Eventual consistency
  - Local caching
- Complexidade vs Performance
  - Sharding simples
  - Replicação básica
- Memória vs Hit ratio
  - LRU eviction
  - Tamanho fixo por node

## 4. Escalabilidade

### Gargalos
- Memória por node
- Network bandwidth
- Hot keys
- Rebalancing overhead

### Soluções
- Horizontal sharding
- Consistent hashing
- Hot key replication
- Background rebalancing
- Compression

### Custos
- Storage: ~$1000/mês
- Compute: ~$3000/mês
- Network: ~$500/mês
- Total: ~$4500/mês

## 5. Resiliência

### Pontos de Falha
- Node failure
- Network partition
- Client overload
- Data corruption

### Mitigações
- Multi-AZ deployment
- Replication
- Rate limiting
- Circuit breakers
- Checksums

### Métricas
- Hit/miss ratio
- Latency (p50, p95, p99)
- Memory usage
- Eviction rate
- Error rate

## 6. Evolução

### MVP
- Basic Get/Set
- Simple LRU
- Single region
- Manual scaling
- Basic monitoring

### Melhorias Futuras
- Multi-region support
- Smart replication
- Auto-scaling
- Advanced eviction
- Better analytics

### Alternativas
- Memcached vs Redis
- Custom vs managed
- RAM vs SSD
- Sync vs async replication 