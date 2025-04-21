# 🎯 System Design Interview

## 🤔 O que é System Design Interview?

System Design Interview é uma etapa crucial no processo de entrevistas técnicas, especialmente para posições sênior e de arquitetura. Neste tipo de entrevista, o candidato é desafiado a projetar um sistema complexo em tempo real, demonstrando conhecimento em escalabilidade, confiabilidade, performance e trade-offs arquiteturais.

Este repositório oferece uma abordagem estruturada para preparação e condução de entrevistas de system design, inspirada na simplicidade e eficácia do formato Architecture Haiku, mas adaptada para o contexto de entrevistas.

## 📝 O Desafio das Entrevistas de System Design

Entrevistas de system design são desafiadoras por várias razões:

1. **Tempo Limitado**: Geralmente 45-60 minutos para resolver um problema complexo
2. **Escopo Ambíguo**: Necessidade de fazer as perguntas certas e definir requisitos
3. **Múltiplas Soluções**: Não existe uma única resposta correta
4. **Profundidade vs Amplitude**: Equilibrar detalhamento com visão geral

## 💡 Nossa Abordagem

### Princípios Fundamentais

1. **Estrutura Clara**
   - Framework consistente para análise
   - Etapas bem definidas
   - Tempo alocado para cada fase

2. **Foco nos Requisitos**
   - Funcionais vs Não-Funcionais
   - Restrições e Limitações
   - Priorização clara

3. **Iteração Progressiva**
   - Começar simples
   - Evoluir com justificativas
   - Refinar com feedback

## 📚 Framework de 6 Passos

### 1. Requisitos (5-10 min)
- Funcionais
- Não-Funcionais
- Restrições
- Estimativas

### 2. Arquitetura de Alto Nível (10-15 min)
- Componentes principais
- Fluxos de dados
- Integrações

### 3. Design Detalhado (15-20 min)
- Tecnologias específicas
- Padrões de design
- Trade-offs

### 4. Escalabilidade (10-15 min)
- Gargalos
- Soluções
- Custos

### 5. Resiliência (5-10 min)
- Pontos de falha
- Mitigações
- Recovery

### 6. Evolução (5 min)
- Próximos passos
- Melhorias futuras
- Alternativas

## 📖 Exemplos Práticos

Na pasta `/examples` você encontrará soluções detalhadas para problemas comuns de system design:

1. **URL Shortener** ([01-url-shortener.md](examples/01-url-shortener.md))
   - Sistema distribuído de encurtamento de URLs
   - Foco em performance e disponibilidade
   - Alta taxa de leitura vs escrita

2. **Chat System** ([02-chat-system.md](examples/02-chat-system.md))
   - Mensagens em tempo real
   - Presença e status online
   - Persistência e entrega garantida

3. **Rate Limiter** ([03-rate-limiter.md](examples/03-rate-limiter.md))
   - Controle de requisições
   - Algoritmos de throttling
   - Distribuição e sincronização

4. **Twitter Feed** ([04-twitter-feed.md](examples/04-twitter-feed.md))
   - Timeline em tempo real
   - Fan-out e consistência
   - Cache e performance

5. **S3-like Storage** ([05-object-storage.md](examples/05-object-storage.md))
   - Armazenamento distribuído
   - Durabilidade e replicação
   - Metadata e consistência

6. **Payment System** ([06-payment-system.md](examples/06-payment-system.md))
   - Processamento financeiro
   - Consistência e atomicidade
   - Segurança e compliance

7. **Sistema de Busca** ([search-system.md](examples/search-system.md))
   - Full-text search e indexação em tempo real
   - Latência < 200ms para queries
   - Ranking e relevância
   - Sugestões de busca
   - Analytics e caching

8. **Cache Distribuído** ([distributed-cache.md](examples/distributed-cache.md))
   - Performance < 1ms para operações
   - 10M operações/segundo
   - 1TB de dados em memória
   - Pub/sub e estruturas de dados
   - Consistência eventual
   - Operações atômicas

9. **Processamento de Logs** ([log-processing.md](examples/log-processing.md))
   - Processamento de 100TB/dia
   - Retenção de 90 dias
   - Durabilidade 99.999999%
   - Parsing e estruturação
   - Indexação e busca
   - Alertas e visualizações
   - Latência de ingestão < 5s

10. **Análise em Tempo Real** ([real-time-analytics.md](examples/real-time-analytics.md))
    - 1M eventos/segundo
    - 86TB/dia de dados processados
    - Latência < 1s (p95)
    - Stream processing
    - Agregações em tempo real
    - Dashboards interativos
    - Alertas dinâmicos

## 🎯 Template Base

Na pasta `/templates` você encontrará um template estruturado para conduzir a entrevista:

1. **Requisitos & Escopo**
   - Perguntas chave
   - Estimativas
   - Restrições

2. **Componentes & Interfaces**
   - Serviços
   - APIs
   - Dados

3. **Escalabilidade & Performance**
   - Bottlenecks
   - Soluções
   - Trade-offs

4. **Resiliência & Segurança**
   - Falhas
   - Mitigações
   - Compliance

## 📊 Números Importantes

### Latência
- Memory: 100ns
- SSD: 100μs
- Network (same region): 1ms
- Network (cross-region): 100ms

### Throughput
- SSD: 500MB/s
- Network: 1Gbps
- Memory: 10GB/s

### Capacidade
- Server RAM: 256GB
- Server Storage: 10TB
- Network/day: 1PB

## 🚫 Anti-Padrões

❌ **Evite**:
- Começar codificando
- Ignorar requisitos não-funcionais
- Assumir recursos infinitos
- Negligenciar trade-offs

✅ **Prefira**:
- Clarificar requisitos primeiro
- Começar simples e evoluir
- Justificar decisões
- Considerar custos

## 📂 Estrutura do Projeto

Este repositório contém templates e exemplos práticos para entrevistas de design de sistemas.

## Estrutura

```
.
├── templates/          # Templates base para entrevistas
└── examples/          # Exemplos práticos de design de sistemas
    ├── search-system.md     # Sistema de busca
    └── distributed-cache.md # Cache distribuído
```

## Templates

- [Template Base](templates/base-template.md) - Template básico para qualquer design de sistema

## Exemplos

- [Sistema de Busca](examples/search-system.md) - Design de um sistema de busca escalável
- [Cache Distribuído](examples/distributed-cache.md) - Design de um sistema de cache distribuído

## Como Usar

1. Comece com o template base para estruturar seu pensamento
2. Consulte os exemplos para inspiração e padrões comuns
3. Adapte os designs conforme necessário para seu caso específico

## Contribuindo

Contribuições são bem-vindas! Por favor, abra uma issue ou pull request.

## 📚 Referências

### Livros Essenciais

1. **System Design Interview – An Insider's Guide** (2020)
   - Autor: Alex Xu
   - ISBN: 978-1736049105

2. **Designing Data-Intensive Applications** (2017)
   - Autor: Martin Kleppmann
   - O'Reilly Media
   - ISBN: 978-1449373320

### Recursos Online

1. **System Design Primer**
   - [github.com/donnemartin/system-design-primer](https://github.com/donnemartin/system-design-primer)
   - Guia completo de preparação

2. **High Scalability**
   - [highscalability.com](http://highscalability.com)
   - Cases reais de arquiteturas

3. **AWS Architecture Center**
   - [aws.amazon.com/architecture](https://aws.amazon.com/architecture)
   - Padrões e práticas recomendadas# system-design-interview
