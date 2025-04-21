# Design de Sistema: Plataforma de Streaming de Vídeo

## 1. Requisitos & Escopo

### 1.1 Perguntas Chave
- [x] Objetivo principal: Streaming de vídeo sob demanda (VOD)
- [x] Usuários principais: Consumidores de conteúdo, Produtores, Anunciantes
- [x] Volume: 10M usuários ativos, 1M horas de conteúdo
- [x] Latência: < 500ms para início do streaming
- [x] Qualidade: Adaptativa (144p até 4K)

### 1.2 Requisitos Funcionais
- [x] Upload de vídeos
- [x] Transcodificação multi-qualidade
- [x] Streaming adaptativo
- [x] Sistema de recomendação
- [x] Analytics em tempo real
- [x] Monetização (ads/assinaturas)

### 1.3 Requisitos Não-Funcionais
- [x] Disponibilidade: 99.99%
- [x] Escalabilidade: Global
- [x] Latência: Buffer < 1s
- [x] Consistência: Eventual
- [x] Durabilidade: 100%
- [x] DRM e Proteção de Conteúdo

### 1.4 Estimativas
- [x] Storage: 1M horas * 2GB/h * 5 qualidades = 10PB
- [x] Bandwidth: 10M * 2GB/h * 0.1 (concurrent) = 2Tbps
- [x] Processamento: 1000 horas upload/dia * 5 qualidades
- [x] CDN: 80% do tráfego

### 1.5 Restrições & Limitações
- [x] Compliance com direitos autorais
- [x] Regulamentações por região
- [x] Largura de banda variável
- [x] Dispositivos heterogêneos

## 2. Design de Alto Nível

### 2.1 Componentes Principais
- [x] Frontend (Web/Mobile/TV)
- [x] API Gateway
- [x] Media Processing Service
- [x] Content Delivery Network
- [x] Authentication Service
- [x] Recommendation Engine
- [x] Analytics Service
- [x] Storage Layer

### 2.2 Fluxos de Dados
```mermaid
graph LR
    A[Upload] --> B[Processing]
    B --> C[Storage]
    C --> D[CDN]
    D --> E[Streaming]
    E --> F[Analytics]
    F --> G[Recommendations]
```

### 2.3 APIs & Interfaces
```typescript
interface IMediaService {
  uploadVideo(file: File, metadata: VideoMetadata): Promise<VideoId>;
  getStreamingUrl(videoId: string, quality: Quality): Promise<StreamingUrl>;
  getMetadata(videoId: string): Promise<VideoMetadata>;
  updateMetadata(videoId: string, metadata: Partial<VideoMetadata>): Promise<void>;
}

interface VideoMetadata {
  title: string;
  description: string;
  duration: number;
  qualities: Quality[];
  tags: string[];
  visibility: 'public' | 'private' | 'unlisted';
  monetization: MonetizationConfig;
}

interface StreamingConfig {
  protocol: 'HLS' | 'DASH';
  drm?: DRMConfig;
  startTime?: number;
  quality?: Quality;
}
```

### 2.4 Modelo de Dados
- [x] Videos Collection (MongoDB)
```javascript
{
  _id: ObjectId,
  title: String,
  description: String,
  userId: ObjectId,
  duration: Number,
  qualities: [String],
  views: Number,
  status: String,
  createdAt: Date,
  metadata: {
    tags: [String],
    language: String,
    category: String
  },
  streams: {
    hls: String,
    dash: String
  }
}
```

## 3. Design Detalhado

### 3.1 Tecnologias Específicas
- [x] Frontend: React/React Native
- [x] Backend: Node.js/Go
- [x] Transcoding: FFmpeg
- [x] Storage: S3 + MongoDB
- [x] CDN: CloudFront/Akamai
- [x] Cache: Redis
- [x] Queue: Kafka
- [x] Search: Elasticsearch

### 3.2 Padrões de Design
- [x] CQRS para metadata
- [x] Event Sourcing para analytics
- [x] Circuit Breaker para serviços
- [x] Saga para processamento
- [x] Publisher/Subscriber para eventos

### 3.3 Trade-offs
| Decisão | Prós | Contras | Justificativa |
|---------|------|---------|---------------|
| HLS vs DASH | Maior compatibilidade | Maior latência | Suporte iOS nativo |
| MongoDB vs SQL | Esquema flexível | Consistência eventual | Metadata dinâmica |
| FFmpeg vs Cloud | Controle total | Manutenção complexa | Customização necessária |

## 4. Escalabilidade

### 4.1 Gargalos
- [x] Upload de arquivos grandes
- [x] Processamento de vídeo
- [x] Picos de demanda
- [x] Hot videos

### 4.2 Soluções
- [x] Upload em chunks
- [x] Auto-scaling para transcoding
- [x] CDN multi-região
- [x] Cache em camadas
- [x] Replicação de metadados

### 4.3 Custos
- [x] Infra: ~$500k/mês
  - Storage: $200k
  - CDN: $150k
  - Compute: $100k
  - Outros: $50k

## 5. Resiliência

### 5.1 Pontos de Falha
- [x] CDN outage
- [x] Transcoding failures
- [x] Database overload
- [x] Network congestion

### 5.2 Mitigações
- [x] Multi-CDN strategy
- [x] Retry com backoff
- [x] Circuit breakers
- [x] Quality degradation
- [x] Regional failover

### 5.3 Monitoramento
- [x] Métricas
  - Buffer ratio
  - Start time
  - Error rate
  - Bitrate switches
- [x] Logging distribuído
- [x] APM
- [x] Real user monitoring

## 6. Evolução

### 6.1 MVP
- [x] Upload básico
- [x] Transcodificação simples
- [x] Playback HLS
- [x] Auth básica
- [x] Analytics básicos

### 6.2 Melhorias Futuras
- [ ] Live streaming
- [ ] Social features
- [ ] AI para conteúdo
- [ ] Multi-language
- [ ] Advanced DRM

### 6.3 Alternativas Consideradas
- [ ] P2P streaming
- [ ] Edge computing
- [ ] Blockchain para direitos

## Notas & Observações

- Considerar cold storage para conteúdo antigo
- Implementar feature flags para rollouts
- Monitorar custos de CDN
- Planejar estratégia de cache invalidation
- Avaliar parceiros de CDN por região 