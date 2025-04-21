# 💬 Design de Sistema: Chat System

## 1. Requisitos & Escopo

### 1.1 Perguntas Chave
- [x] Objetivo principal: Sistema de chat em tempo real
- [x] Usuários principais: Usuários finais, moderadores
- [x] Volume: 10M usuários ativos, 1B mensagens/dia
- [x] Latência: < 100ms para entrega de mensagens
- [x] Custo: Otimizado para escala

### 1.2 Requisitos Funcionais
- [x] Chat 1:1 e grupos
- [x] Status online/offline
- [x] Histórico de mensagens
- [x] Notificações push
- [x] Leitura/entrega confirmação

### 1.3 Requisitos Não-Funcionais
- [x] Disponibilidade: 99.99%
- [x] Escalabilidade: Horizontal
- [x] Latência: < 100ms p95
- [x] Consistência: Eventual
- [x] Durabilidade: 100% para mensagens

### 1.4 Estimativas
- [x] DAU: 10M
- [x] QPS: ~10k mensagens/s
- [x] Storage: ~1KB/msg * 1B = 1TB/dia
- [x] Bandwidth: ~2KB/msg * 1B = 2TB/dia
- [x] Conexões: ~1M simultâneas

### 1.5 Restrições & Limitações
- [x] Mensagens: máx 10KB
- [x] Grupos: máx 1000 membros
- [x] Histórico: 1 ano
- [x] Distribuição global

## 2. Design de Alto Nível

### 2.1 Componentes Principais
- [x] Chat Service
- [x] Presence Service
- [x] Notification Service
- [x] Media Service
- [x] WebSocket Gateway
- [x] Message Store

### 2.2 Fluxos de Dados
- [x] Mensagens: WebSocket → Chat Service → Message Store
- [x] Presença: Heartbeat → Presence Service → Cache
- [x] Notificações: Async via Queue
- [x] Mídia: Upload → CDN

### 2.3 APIs & Interfaces
```typescript
interface IChatService {
  sendMessage(msg: Message): Promise<MessageID>;
  getMessages(chatId: string, options: PaginationOpts): Promise<Message[]>;
  createChat(participants: UserID[]): Promise<ChatID>;
  updateChat(chatId: string, updates: ChatUpdates): Promise<void>;
}

interface IPresenceService {
  updateStatus(userId: string, status: UserStatus): Promise<void>;
  getStatus(userIds: string[]): Promise<Map<UserID, UserStatus>>;
  subscribe(userIds: string[]): Observable<StatusUpdate>;
}

interface Message {
  id: string;
  chatId: string;
  senderId: string;
  content: string;
  type: MessageType;
  timestamp: Date;
  status: MessageStatus;
}

interface ChatUpdates {
  title?: string;
  participants?: UserID[];
  settings?: ChatSettings;
}
```

### 2.4 Modelo de Dados
- [x] Messages Collection
  ```sql
  CREATE TABLE messages (
    id UUID PRIMARY KEY,
    chat_id UUID,
    sender_id UUID,
    content TEXT,
    type VARCHAR(20),
    created_at TIMESTAMP,
    status VARCHAR(20),
    metadata JSONB
  );
  CREATE INDEX idx_chat_time ON messages(chat_id, created_at);
  ```
- [x] Particionamento: Por chat_id
- [x] Replicação: Multi-região

## 3. Design Detalhado

### 3.1 Tecnologias Específicas
- [x] Backend: Node.js/Go
- [x] Database: Cassandra
- [x] Cache: Redis
- [x] Queue: Kafka
- [x] WebSocket: Socket.io

### 3.2 Padrões de Design
- [x] Pub/Sub para mensagens
- [x] CQRS para leitura/escrita
- [x] Event Sourcing
- [x] Fan-out on write

### 3.3 Trade-offs
| Decisão | Prós | Contras | Justificativa |
|---------|------|---------|---------------|
| WebSocket | Real-time, Eficiente | Complexo | Necessário para chat |
| Cassandra | Escala linear | Eventual consistency | Volume de dados alto |
| Fan-out write | Leitura rápida | Write amplification | UX priorizada |

## 4. Escalabilidade

### 4.1 Gargalos
- [x] WebSocket connections
- [x] Message fan-out
- [x] Presence updates
- [x] Storage growth

### 4.2 Soluções
- [x] Connection pooling
- [x] Message batching
- [x] Status cache
- [x] Data archival

### 4.3 Custos
- [x] Infra: ~$20k/mês
  - Compute: $8k
  - Storage: $5k
  - Network: $5k
  - Outros: $2k

## 5. Resiliência

### 5.1 Pontos de Falha
- [x] WebSocket servers
- [x] Message delivery
- [x] Presence tracking
- [x] Database

### 5.2 Mitigações
- [x] Connection recovery
- [x] Message persistence
- [x] Status reconciliation
- [x] Multi-region deployment

### 5.3 Monitoramento
- [x] Métricas
  - Message latency
  - Connection count
  - Error rates
  - Queue depth
- [x] Alertas para SLO breaches
- [x] Distributed tracing
- [x] Log aggregation

## 6. Evolução

### 6.1 MVP
- [x] Basic messaging
- [x] Presence
- [x] Push notifications
- [x] Message history

### 6.2 Melhorias Futuras
- [ ] E2E encryption
- [ ] Rich media
- [ ] Search
- [ ] Analytics

### 6.3 Alternativas Consideradas
- [ ] REST polling
- [ ] MongoDB
- [ ] Server-side rendering

## Notas & Observações

- Considerar sharding por região
- Avaliar necessidade de message queuing
- Monitorar latência em diferentes regiões
- Planejar estratégia de backup