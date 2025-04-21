# 💳 Design de Sistema: Payment System

## 1. Requisitos & Escopo

### 1.1 Perguntas Chave
- [x] Objetivo principal: Processamento seguro de pagamentos
- [x] Usuários principais: Compradores, Vendedores
- [x] Volume: 1M transações/dia
- [x] Latência: < 2s para autorização
- [x] Custo: Otimizado para confiabilidade

### 1.2 Requisitos Funcionais
- [x] Processamento de pagamentos
- [x] Múltiplos métodos de pagamento
- [x] Histórico de transações
- [x] Reembolsos e chargebacks
- [x] Relatórios financeiros

### 1.3 Requisitos Não-Funcionais
- [x] Disponibilidade: 99.999%
- [x] Escalabilidade: Horizontal
- [x] Latência: < 2s p99
- [x] Consistência: Forte
- [x] Durabilidade: 100%

### 1.4 Estimativas
- [x] DAU: 100k merchants
- [x] TPS: ~100 transactions/s
- [x] Storage: ~1KB/tx * 1M = 1GB/dia
- [x] Bandwidth: ~2KB/tx * 1M = 2GB/dia
- [x] Concorrência: ~1000 tx/s pico

### 1.5 Restrições & Limitações
- [x] PCI DSS compliance
- [x] Regulações financeiras
- [x] Multi-moeda
- [x] Fraude < 0.1%

## 2. Design de Alto Nível

### 2.1 Componentes Principais
- [x] Payment Gateway
- [x] Transaction Service
- [x] Account Service
- [x] Fraud Detection
- [x] Reporting Service
- [x] Notification Service

### 2.2 Fluxos de Dados
- [x] Pagamento: Gateway → Transaction → Account
- [x] Fraude: Async check via ML
- [x] Relatórios: Batch processing
- [x] Notificações: Event-driven

### 2.3 APIs & Interfaces
```typescript
interface IPaymentService {
  authorize(payment: PaymentRequest): Promise<AuthResult>;
  capture(authId: string): Promise<CaptureResult>;
  refund(txId: string, amount: Money): Promise<RefundResult>;
  void(authId: string): Promise<VoidResult>;
}

interface IAccountService {
  getBalance(accountId: string): Promise<Balance>;
  hold(accountId: string, amount: Money): Promise<HoldResult>;
  transfer(from: string, to: string, amount: Money): Promise<TransferResult>;
}

interface PaymentRequest {
  merchantId: string;
  amount: Money;
  currency: string;
  paymentMethod: PaymentMethod;
  metadata: Map<string, string>;
}

interface Money {
  amount: number;
  currency: string;
}
```

### 2.4 Modelo de Dados
- [x] Transactions Table
  ```sql
  CREATE TABLE transactions (
    id UUID PRIMARY KEY,
    merchant_id UUID,
    amount DECIMAL,
    currency VARCHAR(3),
    status VARCHAR(20),
    created_at TIMESTAMP,
    payment_method JSONB,
    metadata JSONB
  );
  CREATE INDEX idx_merchant_time ON transactions(merchant_id, created_at);
  ```
- [x] Particionamento: Por merchant_id
- [x] Replicação: Multi-região sync

## 3. Design Detalhado

### 3.1 Tecnologias Específicas
- [x] Backend: Java/Kotlin
- [x] Database: PostgreSQL
- [x] Cache: Redis
- [x] Queue: RabbitMQ
- [x] Search: Elasticsearch

### 3.2 Padrões de Design
- [x] SAGA para transações
- [x] CQRS para relatórios
- [x] Circuit Breaker
- [x] Rate Limiting

### 3.3 Trade-offs
| Decisão | Prós | Contras | Justificativa |
|---------|------|---------|---------------|
| SQL | ACID, Schemas | Custo de escala | Consistência crítica |
| Sync Replication | Consistência | Latência | Requisito regulatório |
| SAGA | Isolamento | Complexidade | Necessário para atomicidade |

## 4. Escalabilidade

### 4.1 Gargalos
- [x] DB writes
- [x] Payment gateway
- [x] Fraud checks
- [x] Reporting queries

### 4.2 Soluções
- [x] Write-through cache
- [x] Gateway pooling
- [x] Async fraud checks
- [x] CQRS para relatórios

### 4.3 Custos
- [x] Infra: ~$50k/mês
  - Compute: $20k
  - Storage: $10k
  - Network: $10k
  - Segurança: $10k

## 5. Resiliência

### 5.1 Pontos de Falha
- [x] Gateway timeout
- [x] DB failure
- [x] Network partition
- [x] Service crash

### 5.2 Mitigações
- [x] Multiple gateways
- [x] Multi-region DB
- [x] Circuit breakers
- [x] Idempotency

### 5.3 Monitoramento
- [x] Métricas
  - Transaction success rate
  - Latency p95/p99
  - Error rates
  - Fraud rates
- [x] Alertas para anomalias
- [x] Audit logging
- [x] Transaction tracing

## 6. Evolução

### 6.1 MVP
- [x] Basic payments
- [x] Core reporting
- [x] Simple fraud rules
- [x] Basic notifications

### 6.2 Melhorias Futuras
- [ ] ML fraud detection
- [ ] Real-time analytics
- [ ] Enhanced reporting
- [ ] More payment methods

### 6.3 Alternativas Consideradas
- [ ] NoSQL database
- [ ] Serverless architecture
- [ ] GraphQL API

## Notas & Observações

- Regular security audits
- Compliance reviews
- Disaster recovery tests
- Performance optimization