# 🏗 System Design Interview

## 🔤 O que é System Design Interview?

System Design Interview é uma etapa crucial no processo de entrevistas técnicas, especialmente para posições sênior e de arquitetura. Neste tipo de entrevista, o candidato é desafiado a projetar um sistema complexo em tempo real, demonstrando conhecimento em escalabilidade, confiabilidade, performance e trade-offs arquiteturais.

Este repositório oferece uma abordagem estruturada para preparação e condução de entrevistas de system design, inspirada na simplicidade e eficácia do formato Architecture Haiku, mas adaptada para o contexto de entrevistas.

## 🔝 O Desafio das Entrevistas de System Design

Entrevistas de system design são desafiadoras por várias razões:

1. **Tempo Limitado**: Geralmente 45-60 minutos para resolver um problema complexo
2. **Escopo Ambíguo**: Necessidade de fazer as perguntas certas e definir requisitos
3. **Múltiplas Soluções**: Não existe uma única resposta correta
4. **Profundidade vs Amplitude**: Equilibrar detalhamento com visão geral

## 🎡 Abordagem

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

## 🔚 Framework de 6 Passos

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

| Sistema | Descrição |
|---------|-----------|
| [URL Shortener](examples/url-shortener.md) | Sistema distribuído de encurtamento de URLs com foco em performance e disponibilidade, alta taxa de leitura vs escrita |
| [Chat System](examples/chat-system.md) | Mensagens em tempo real, presença e status online, persistência e entrega garantida |
| [Rate Limiter](examples/auth-system.md) | Controle de requisições, algoritmos de throttling, distribuição e sincronização |
| [Real-time Messaging](examples/real-time-messaging.md) | Timeline em tempo real, fan-out e consistência, cache e performance |
| [Distributed Cache](examples/distributed-cache.md) | Armazenamento distribuído, durabilidade e replicação, metadata e consistência |
| [Payment System](examples/payment-system.md) | Processamento financeiro, consistência e atomicidade, segurança e compliance |
| [Search System](examples/search-system.md) | Full-text search e indexação em tempo real, latência < 200ms para queries, ranking e relevância, sugestões de busca, analytics e caching |
| [Log Processing](examples/log-processing.md) | Processamento de 100TB/dia, retenção de 90 dias, durabilidade 99.99999%, parsing e estruturação, indexação e busca, alertas e visualizações, latência de ingestão < 5s |
| [Real-time Analytics](examples/real-time-analytics.md) | 1M eventos/segundo, 86TB/dia de dados processados, latência < 1s (p95), stream processing, agregações em tempo real, dashboards interativos, alertas dinâmicos |


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

## 📚 Referências

### Livros Essenciais

1. **System Design Interview – An Insider's Guide** (2020)
   - Autor: Alex Xu
   - ISBN: 978-1736049105
   - Referência fundamental para entrevistas de system design
   - Aborda padrões e anti-padrões comuns
   - Inclui casos de estudo detalhados

2. **System Design Interview – An Insider's Guide: Volume 2** (2022)
   - Autor: Alex Xu
   - ISBN: 978-1736049112
   - Aprofundamento em tópicos avançados
   - Novos casos de estudo e padrões
   - Foco em escalabilidade e performance

3. **Designing Data-Intensive Applications** (2017)
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
   - Padrões e práticas recomendadas
