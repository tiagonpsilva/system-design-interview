# Encurtador de URLs

## 1. Requisitos

### Funcionais
- Encurtar URLs
- Redirecionamento
- URLs customizadas
- Analytics
- Expiração

### Não Funcionais
- Latência <100ms
- Alta disponibilidade
- URLs únicas
- Links permanentes

## 2. Estimativas

### Tráfego
- 100k novos URLs/dia
- 10M redirecionamentos/dia
- 1k requests/segundo

### Armazenamento
- URL original: 2KB
- URL curta: 6-8 bytes
- Total: 100GB/ano

## 3. Design do Sistema

### Componentes Principais
1. URL Service
2. Hash Generator
3. Analytics Service
4. Cache Layer
5. Database

### Fluxo de Dados
1. Recebe URL longa
2. Gera hash único
3. Armazena mapeamento
4. Retorna URL curta
5. Redireciona acessos

## 4. Tecnologias Sugeridas

### Backend
- Node.js/Express
- Go
- Python/Flask

### Database
- PostgreSQL
- Redis
- Cassandra

### Cache
- Redis
- Memcached

## 5. Considerações

### Escalabilidade
- Distribuição de carga
- Sharding
- Caching

### Performance
- Hash collision
- Cache hit ratio
- Redirect speed

### Segurança
- Rate limiting
- URL validation
- Analytics privacy

## 6. Trade-offs

### Prós
- Simple design
- Fast redirects
- Scalable

### Contras
- Hash collisions
- Storage growth
- Cache management