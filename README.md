# Gateway - Sabores Conectados

## Visão Geral
Gateway responsável pelo roteamento de requisições para os microserviços do sistema Sabores Conectados.

## Funcionalidades
- ✅ Roteamento automático para microserviços
- ✅ Descoberta de serviços via Eureka
- ✅ Load balancing
- ✅ Proxy reverso
- ✅ Integração com Eureka Server

## Tecnologias
- **Java 17**
- **Spring Boot 3.3.4**
- **Spring Cloud Gateway 2023.0.3**
- **Spring Cloud Netflix Eureka Client**

## Estrutura do Projeto
```
Gateway/
├── Gateway/                     # Projeto Spring Boot
│   ├── src/main/java/
│   │   └── br/com/saboresconectados/gateway/
│   │       └── GatewayApplication.java
│   ├── src/main/resources/
│   │   └── application.properties
│   └── src/test/                # Testes unitários
├── Dockerfile                   # Container Docker
└── README.md                    # Este arquivo
```

## Como Executar

### Pré-requisitos
- Java 17+
- Maven 3.6+
- Eureka Server rodando

### Execução Local
```bash
cd Gateway/Gateway
mvn spring-boot:run
```

### Execução com Docker
```bash
docker build -t gateway-service .
docker run -p 8084:8084 gateway-service
```

## Configuração

### Porta
- **Porta**: 8084

### Roteamento
O gateway está configurado para rotear requisições para:

| Rota | Microserviço | Porta |
|------|-------------|-------|
| `/cardapio/**` | cardapio | 8082 |
| `/pedidos/**` | pedidos | 8083 |
| `/pagamentos/**` | pagamentos | 8081 |

### Exemplo de Uso
```bash
# Acessar cardápio via gateway
curl http://localhost:8084/cardapio/todos

# Acessar pedidos via gateway
curl http://localhost:8084/pedidos

# Acessar pagamentos via gateway
curl http://localhost:8084/pagamentos
```

## Descoberta de Serviços
O gateway utiliza o Eureka Server para descobrir automaticamente os microserviços disponíveis. Certifique-se de que:

1. O Eureka Server está rodando na porta 8761
2. Todos os microserviços estão registrados no Eureka
3. O gateway está configurado para se conectar ao Eureka

## Configuração Avançada
Para configurar rotas adicionais ou modificar o comportamento do gateway, edite o arquivo `application.properties`:

```properties
# Rota personalizada
spring.cloud.gateway.routes[0].id=meu-servico
spring.cloud.gateway.routes[0].uri=lb://meu-servico
spring.cloud.gateway.routes[0].predicates[0]=Path=/meu-servico/**
```

## Monitoramento
- **Health Check**: http://localhost:8084/actuator/health
- **Eureka Dashboard**: http://localhost:8761

## Troubleshooting

### Problemas Comuns
1. **Serviços não encontrados**: Verifique se o Eureka Server está rodando
2. **Erro 503**: Verifique se os microserviços estão registrados no Eureka
3. **Timeout**: Verifique a conectividade de rede

### Logs
Para debug, verifique os logs do gateway:
```bash
tail -f logs/gateway.log
```

## Desenvolvimento
Para contribuir com o desenvolvimento:
1. Clone o repositório
2. Configure o ambiente de desenvolvimento
3. Execute os testes: `mvn test`
4. Faça suas alterações
5. Execute os testes novamente
6. Faça commit das alterações
