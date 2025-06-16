Este repositório organiza e referencia os principais projetos que compõem uma arquitetura baseada em Spring Cloud Netflix.

## 🔗 Repositórios Individuais

- 🔐 [Auth Server (OAuth2 / JWT)](https://github.com/marcellopedrosa/auth-server)
- 🌐 [API Gateway (Spring Cloud Gateway)](https://github.com/marcellopedrosa/api-gateway)
- 🧭 [Eureka Server (Service Discovery)](https://github.com/marcellopedrosa/eureka-server)
- 🛠️ [Config Server (Centralização de Configs)](https://github.com/marcellopedrosa/config-server)

Cada repositório contém sua própria documentação com instruções detalhadas de uso e configuração.

## 📌 Sobre

Este projeto foi criado para fornecer uma base sólida e escalável para o desenvolvimento de microsserviços utilizando o ecossistema do Spring Cloud.
""",

    "auth-server/README.md": """# Auth Server - Spring Authorization Server

Este projeto implementa um servidor de autenticação OAuth2 usando **Spring Authorization Server**, responsável pela emissão e validação de tokens JWT.

## 🔐 Funcionalidades

- Emissão de tokens OAuth2 (Password, Client Credentials, Authorization Code).
- Endpoints para descoberta e chave pública (JWK).
- Integração com banco de dados para autenticação de usuários.
- Pode ser usado com o API Gateway para validação de tokens.

## 🚫 Importante

- **Não deve estar atrás do API Gateway.**
- **Não deve ter rota registrada no API Gateway.**
- Deve ser acessível diretamente por apps frontend ou serviços confiáveis.

## 🧭 Requisitos

- Java 17+
- Spring Boot 3.1+
- Maven 3.9+

Consulte o [repositório](https://github.com/marcellopedrosa/auth-server) para detalhes de configuração.
""",

    "api-gateway/README.md": """# API Gateway - Spring Cloud Gateway

Este projeto atua como o ponto de entrada da arquitetura de microsserviços, utilizando **Spring Cloud Gateway**.

## 🌐 Funcionalidades

- Roteamento dinâmico com base em URI.
- Integração com Eureka para descoberta de serviços.
- Validação de segurança com JWT (Auth Server externo).
- Suporte a filtros de autenticação e logging.

## ⚠️ Restrições

- **Não deve expor o `auth-server`.**
- Deve validar tokens emitidos pelo Auth Server.
- A segurança deve ser configurada com filtros globais ou por rota.

## 🧭 Requisitos

- Java 17+
- Spring Boot 3.1+
- Spring Cloud Gateway
- Maven 3.9+

Mais informações: [repositório](https://github.com/marcellopedrosa/api-gateway)
""",

    "eureka-server/README.md": """# Eureka Server - Service Discovery

Este projeto implementa um servidor de descoberta de serviços com **Spring Cloud Netflix Eureka**.

## 🧭 Funcionalidades

- Registro de instâncias de microserviços.
- Descoberta de serviços para comunicação entre eles.
- Painel web para visualização das instâncias registradas.

## 🔐 Segurança

- **Não adicionar autenticação ou segurança.**
- Deve ser acessado livremente pelos microserviços da rede interna.
- **Não expor via API Gateway.**

## 🧭 Requisitos

- Java 17+
- Spring Boot 3.1+
- Maven 3.9+

Veja o projeto em: [repositório](https://github.com/marcellopedrosa/eureka-server)
""",

    "config-server/README.md": """# Config Server - Centralização de Configurações

Este projeto fornece configuração centralizada para todos os microserviços através do **Spring Cloud Config Server**.

## 🛠️ Funcionalidades

- Leitura de arquivos YAML ou Properties em um repositório Git.
- Suporte a múltiplos perfis de ambiente (`dev`, `prod`).
- Integração com Spring Cloud Bus (opcional) para atualização automática.

## 🧭 Requisitos

- Java 17+
- Spring Boot 3.1+
- Maven 3.9+
- Git ou GitHub com repositório de configurações

Repositório: [config-server](https://github.com/marcellopedrosa/config-server)
