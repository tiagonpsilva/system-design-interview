# Template Base para Design de Sistema

## 1. Requisitos & Escopo

### Perguntas Chave
- Qual o problema específico a ser resolvido?
- Quais são os casos de uso principais?
- Quais são as restrições do sistema?
- Quais são as métricas de sucesso?

### Requisitos Funcionais
- Lista de funcionalidades essenciais
- Comportamentos esperados
- Interações com usuários/sistemas

### Requisitos Não-Funcionais
- Performance
- Escalabilidade
- Disponibilidade
- Consistência
- Latência
- Durabilidade

### Estimativas
- Volume de dados
- Tráfego esperado
- Latência máxima
- Taxa de crescimento
- Custos operacionais

## 2. Design de Alto Nível

### Componentes Principais
- Lista de serviços/componentes
- Responsabilidades de cada componente
- Interações entre componentes

### Fluxo de Dados
- Diagrama de fluxo
- Protocolos de comunicação
- Formatos de dados

### APIs
- Endpoints principais
- Parâmetros
- Respostas
- Códigos de erro

### Modelo de Dados
- Entidades principais
- Relacionamentos
- Schema
- Índices

## 3. Design Detalhado

### Tecnologias
- Linguagens
- Frameworks
- Bancos de dados
- Message brokers
- Caching

### Padrões de Design
- Architectural patterns
- Design patterns
- Data patterns
- Integration patterns

### Trade-offs
- Performance vs Consistência
- Complexidade vs Flexibilidade
- Custo vs Escalabilidade
- Make vs Buy

## 4. Escalabilidade

### Gargalos
- Pontos de contenção
- Limites do sistema
- Recursos críticos

### Soluções
- Estratégias de scaling
- Otimizações
- Caching
- Particionamento

### Custos
- Infraestrutura
- Operacional
- Licenças
- Equipe

## 5. Resiliência

### Pontos de Falha
- Single points of failure
- Cascading failures
- Resource exhaustion
- Data loss

### Mitigações
- Redundância
- Circuit breakers
- Rate limiting
- Fallbacks

### Métricas
- SLIs/SLOs
- Alertas
- Dashboards
- Logs

## 6. Evolução

### MVP
- Funcionalidades iniciais
- Simplificações
- Quick wins

### Melhorias Futuras
- Roadmap
- Otimizações
- Novas features
- Tech debt

### Alternativas
- Outras abordagens consideradas
- Pros/cons
- Trade-offs

## Notas & Observações

- Pontos importantes discutidos
- Decisões pendentes
- Riscos identificados
- Questões em aberto 