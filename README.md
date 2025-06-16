# Spring Cloud Netflix Boilerplate

Este repositório fornece uma base sólida para iniciar projetos com microserviços usando **Spring Cloud Netflix**. Os módulos aqui apresentados implementam os componentes centrais de uma arquitetura distribuída: *Service Discovery*, *Centralized Configuration*, *API Gateway* e *Autenticação OAuth2*.

## ⚠️ Alertas Críticos

> ⚠️ **1. O `auth-server` deve estar completamente fora da `api-gateway`.**  
> Ele é um serviço de autenticação e não deve passar por filtros ou roteamento do gateway.

> ⚠️ **2. O `auth-server` não deve ter nenhuma rota configurada no `api-gateway`.**  
> Tokens devem ser emitidos diretamente e o tráfego do gateway deve usar esses tokens para validação, sem expor o servidor de autenticação.

> ⚠️ **3. O `eureka-server` não precisa e não deve ter configurações de segurança.**  
> Ele deve estar acessível internamente para todos os microsserviços e não deve ser exposto via `api-gateway`.

---

## 📦 Módulos Inclusos

### 1. 🧭 eureka-server

**Descrição:**  
O *Eureka Server* atua como um **registry** (registro de serviços). Todos os microserviços clientes se registram nele e podem descobrir outros serviços registrados para comunicação entre si.

**Responsabilidades:**
- Registro de instâncias de microserviços (Service Registry).
- Atualizações periódicas (heartbeats) dos serviços registrados.
- Remoção automática de instâncias inativas.

**Dependência principal:**
```xml
<dependency>
  <groupId>org.springframework.cloud</groupId>
  <artifactId>spring-cloud-starter-netflix-eureka-server</artifactId>
</dependency>
```

**Anotação necessária no main:**
```java
@EnableEurekaServer
```

> ✅ **Não adicione autenticação ao `eureka-server`**. Ele deve ser acessível livremente na rede interna entre os serviços.

---

### 2. 🛠️ config-server

**Descrição:**  
Responsável por centralizar os arquivos de configuração dos microserviços. As configurações podem ser armazenadas em um repositório Git, tornando-as versionadas e seguras.

**Responsabilidades:**
- Servir arquivos de configuração para os clientes via HTTP.
- Suporte a perfis (`dev`, `prod`, etc.).
- Recarregamento automático (quando combinado com Spring Cloud Bus).

**Dependência principal:**
```xml
<dependency>
  <groupId>org.springframework.cloud</groupId>
  <artifactId>spring-cloud-config-server</artifactId>
</dependency>
```

**Anotação necessária no main:**
```java
@EnableConfigServer
```

**Exemplo de estrutura de repositório remoto (Git):**
```
configs/
├── ms-cliente-dev.yml
├── ms-cliente-prod.yml
├── ms-pagamento-dev.yml
└── application.yml
```

---

### 3. 🌐 api-gateway

**Descrição:**  
O *API Gateway* atua como ponto de entrada único para os microserviços. Ele lida com roteamento, balanceamento de carga, segurança e logging centralizado.

**Responsabilidades:**
- Roteamento dinâmico baseado em URIs.
- Balanceamento de carga via **Eureka**.
- Suporte a filtros (pré e pós-processamento).
- Integração com OAuth2 / JWT para segurança.

**Dependência principal:**
```xml
<dependency>
  <groupId>org.springframework.cloud</groupId>
  <artifactId>spring-cloud-starter-gateway</artifactId>
</dependency>
```

**Exemplo de configuração (`application.yml`):**
```yaml
spring:
  cloud:
    gateway:
      routes:
        - id: ms-cliente
          uri: lb://ms-cliente
          predicates:
            - Path=/clientes/**
        - id: ms-pagamento
          uri: lb://ms-pagamento
          predicates:
            - Path=/pagamentos/**
```

> ❌ **Não adicione rota para o `auth-server` no gateway.**

---

### 4. 🔐 auth-server

**Descrição:**  
Responsável pela autenticação e emissão de tokens JWT usando **OAuth2 Authorization Server**. Permite que os microserviços se autentiquem via API Gateway e forneçam segurança baseada em token.

**Responsabilidades:**
- Autenticar usuários e clientes.
- Emitir e validar tokens JWT.
- Integrar com banco de dados ou LDAP (opcional).

**Dependência principal (Spring Authorization Server):**
```xml
<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-oauth2-authorization-server</artifactId>
</dependency>
```

**Exemplo de endpoints:**
- `/oauth2/token`: emissão de tokens.
- `/oauth2/jwks`: chaves públicas para validação.

**Exemplo de configuração básica:**
```yaml
spring:
  security:
    oauth2:
      authorizationserver:
        issuer: http://localhost:9000
```

> 🔐 **Deve ser acessado diretamente pelos clientes front-end ou trusted services, sem passar pelo `api-gateway`.**

---

## 📚 Pré-requisitos

- Java 17+ (ou 21 se preferir)
- Spring Boot 3.1+ ou 3.5+ (compatível com Spring Cloud 2025.x)
- Maven 3.9.x
- Git (para versionamento remoto das configs)

---

## 🚀 Como Executar

1. Clone o repositório:
   ```bash
   git clone https://github.com/seu-usuario/spring-cloud-netflix.git
   ```

2. Suba os serviços na seguinte ordem:
   - `eureka-server`
   - `config-server`
   - `auth-server`
   - `api-gateway`
   - Demais microsserviços (clientes Eureka)

3. Acesse:
   - Eureka: http://localhost:8761
   - Gateway: http://localhost:8080
   - Config Server: http://localhost:8888/{app-name}/{profile}
   - Auth Server: http://localhost:9000

---

## 🧱 Estrutura Recomendada

```
spring-cloud-netflix/
├── eureka-server/
├── config-server/
├── auth-server/
├── api-gateway/
└── README.md
```

---

## 📌 Observações

- Todos os microserviços devem incluir o `spring-cloud-starter-config`, `spring-cloud-starter-netflix-eureka-client` e autenticação via OAuth2.
- Utilize `spring.profiles.active` para controlar ambientes.
- Use o actuator e Spring Cloud Bus para recarregar configurações em tempo real.

---

## 📖 Referências

- [Spring Cloud Netflix Docs](https://spring.io/projects/spring-cloud-netflix)
- [Spring Cloud Config](https://spring.io/projects/spring-cloud-config)
- [Spring Cloud Gateway](https://spring.io/projects/spring-cloud-gateway)
- [Spring Authorization Server](https://spring.io/projects/spring-authorization-server)
