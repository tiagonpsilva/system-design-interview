# 🔗 Design de Sistema: URL Shortener

## 1. Requisitos & Escopo

### 1.1 Perguntas Chave
- [x] Objetivo principal: Criar URLs curtas e redirecionáveis para URLs longas
- [x] Usuários principais: Desenvolvedores, Marketing, Usuários finais
- [x] Volume: 100M URLs/mês, 1B redirecionamentos/mês
- [x] Latência: < 100ms para redirecionamento
- [x] Custo: Otimizado para escala

### 1.2 Requisitos Funcionais
- [x] Gerar URL curta para uma URL longa
- [x] Redirecionar URL curta para URL original
- [x] Customização de URLs (opcional)
- [x] Expiração de URLs
- [x] Analytics básicas

### 1.3 Requisitos Não-Funcionais
- [x] Disponibilidade: 99.99%
- [x] Escalabilidade: Horizontal
- [x] Latência: < 100ms p95
- [x] Consistência: Eventual
- [x] Durabilidade: 100% para URLs ativas

### 1.4 Estimativas
- [x] DAU: 1M
- [x] QPS: ~10k writes, ~100k reads
- [x] Storage: ~100 bytes/URL * 100M = 10GB/mês
- [x] Bandwidth: ~1KB/request * 1B = 1TB/mês
- [x] Cache ratio: 20% hot URLs = 80% hits

### 1.5 Restrições & Limitações
- [x] URLs curtas: máx 7 caracteres
- [x] Tecnologias cloud-native
- [x] Compliance com GDPR
- [x] Distribuição global

## 2. Design de Alto Nível

### 2.1 Componentes Principais
- [x] API Gateway
- [x] URL Service
- [x] Cache Service (Redis)
- [x] Database (PostgreSQL)
- [x] Analytics Service
- [x] CDN

### 2.2 Fluxos de Dados
- [x] Criação: API → URL Service → DB
- [x] Redirecionamento: CDN → Cache → URL Service → DB
- [x] Analytics: Async via Queue
- [x] Protocolo: REST + gRPC interno

### 2.3 APIs & Interfaces
```typescript
interface IUrlService {
  shorten(url: string, options?: UrlOptions): Promise<ShortUrl>;
  redirect(shortCode: string): Promise<string>;
  customize(shortCode: string, url: string): Promise<ShortUrl>;
  getStats(shortCode: string): Promise<UrlStats>;
}

interface UrlOptions {
  expiresAt?: Date;
  customCode?: string;
  tags?: string[];
}

interface ShortUrl {
  code: string;
  originalUrl: string;
  shortUrl: string;
  expiresAt?: Date;
  createdAt: Date;
}

interface UrlStats {
  clicks: number;
  uniqueClicks: number;
  referrers: Map<string, number>;
  countries: Map<string, number>;
}
```

### 2.4 Modelo de Dados
- [x] Tabela URLs
  ```sql
  CREATE TABLE urls (
    id BIGSERIAL PRIMARY KEY,
    short_code VARCHAR(7) UNIQUE,
    original_url TEXT NOT NULL,
    created_at TIMESTAMP DEFAULT NOW(),
    expires_at TIMESTAMP,
    user_id UUID,
    custom BOOLEAN DEFAULT FALSE
  );
  CREATE INDEX idx_short_code ON urls(short_code);
  ```
- [x] Particionamento: Por range de short_code
- [x] Replicação: Multi-AZ

## 3. Design Detalhado

### 3.1 Tecnologias Específicas
- [x] Backend: Go/Rust
- [x] Database: PostgreSQL
- [x] Cache: Redis Cluster
- [x] Cloud: AWS/GCP
- [x] Monitoring: Prometheus + Grafana

### 3.2 Padrões de Design
- [x] Arquitetura: Microserviços
- [x] Cache-Aside + Write-Through
- [x] Circuit Breaker para DB
- [x] Rate Limiting por IP/User

### 3.3 Trade-offs
| Decisão | Prós | Contras | Justificativa |
|---------|------|---------|---------------|
| SQL vs NoSQL | ACID, Schemas | Scaling complexo | Volume gerenciável, necessidade de consistência |
| Cache-Aside | Simples, Resiliente | Eventual consistency | Latência crítica para reads |
| Base62 vs UUID | URLs curtas | Sequencial | Compromisso tamanho/aleatoriedade |

## 4. Escalabilidade

### 4.1 Gargalos
- [x] Read path: Cache hit ratio
- [x] Write path: DB writes
- [x] Analytics: Async processing
- [x] Storage growth

### 4.2 Soluções
- [x] Cache em múltiplas camadas (L1/L2)
- [x] Write-behind para analytics
- [x] DB read replicas
- [x] CDN para redirecionamentos populares

### 4.3 Custos
- [x] Infra: ~$5k/mês
  - Compute: $2k
  - Storage: $1k
  - CDN: $1k
  - Outros: $1k

## 5. Resiliência

### 5.1 Pontos de Falha
- [x] DB master failure
- [x] Cache miss cascade
- [x] Network partition
- [x] Region failure

### 5.2 Mitigações
- [x] Multi-AZ deployment
- [x] Cache warming
- [x] Circuit breakers
- [x] Rate limiting

### 5.3 Monitoramento
- [x] Métricas
  - Latência p95/p99
  - Cache hit ratio
  - Error rates
  - QPS
- [x] Alertas para SLO breaches
- [x] Distributed tracing
- [x] Structured logging

## 6. Evolução

### 6.1 MVP
- [x] Basic URL shortening
- [x] Redirecionamento
- [x] Cache layer
- [x] Basic analytics

### 6.2 Melhorias Futuras
- [ ] Custom domains
- [ ] A/B testing
- [ ] Deep analytics
- [ ] API monetization

### 6.3 Alternativas Consideradas
- [ ] Pure NoSQL solution
- [ ] Serverless architecture
- [ ] Custom base62 encoding

## Notas & Observações

- Considerar implementação de feature flags
- Avaliar necessidade de warm-up para cache
- Monitorar crescimento de custom URLs
- Planejar estratégia de limpeza para URLs expiradas