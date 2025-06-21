# Spring Cloud Netflix - Arquitetura de Microserviços

Este repositório organiza e referencia os principais projetos que compõem uma arquitetura baseada em Spring Cloud Netflix.

## 🔗 Repositórios Individuais

- 🧭 [Eureka Server (Service Discovery)](https://github.com/marcellopedrosa/eureka-server)
- 🛠️ [Config Server (Centralização de Configs)](https://github.com/marcellopedrosa/config-server)
- 🔐 [Auth Server (OAuth2 / JWT)](https://github.com/marcellopedrosa/auth-server)
- 🌐 [API Gateway (Spring Cloud Gateway)](https://github.com/marcellopedrosa/api-gateway)
- 📌 [Microserviço de Teste](https://github.com/marcellopedrosa/teste-service)

## 🔗 Ordem de inicialização recomendada

- Eureka Server: 8761
- Config Server: 8888
- Auth Server: 9000 (ou outras APIs)
- Gateway Server: 8080
- Microserviço Teste Service: 8082

Cada repositório contém sua própria documentação com instruções detalhadas de uso e configuração.

## 📌 Sobre

Este projeto foi criado para fornecer uma base sólida e escalável para o desenvolvimento de microsserviços utilizando o ecossistema do Spring Cloud.
