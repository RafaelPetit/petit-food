# Petit Food Microservices 🍽️

> Proof of Concept (POC) desenvolvida durante a **Formação Microsserviços na prática - Alura**, com foco em arquitetura distribuída, mensageria e deploy automatizado na AWS.

---

## 🚀 Visão Geral

O **Petit Food Microservices** é uma POC criada para consolidar conhecimentos de arquitetura de microsserviços usando **Java + Spring Boot**, com **comunicação síncrona e assíncrona**, **infraestrutura como código (IaC)** e **deploy em nuvem**.

O projeto simula o fluxo de pedidos e pagamentos de uma aplicação real, explorando práticas modernas de desacoplamento, resiliência e escalabilidade.

---

## 🧩 Arquitetura

O sistema é composto por múltiplos microsserviços independentes, cada um com sua própria infraestrutura e pipeline de build:

- **Server (Eureka)** → Service Discovery (porta 8081)
- **Gateway** → API Gateway (ponto único de entrada, porta 8082)
- **Orders Service** → Microsserviço de pedidos
- **Payments Service** → Microsserviço de pagamentos
- **RabbitMQ** → Message broker para comunicação assíncrona
- **PostgreSQL** → Banco de dados com schemas isolados por serviço

### Comunicação

- **Síncrona** via *Feign Client* (Spring Cloud)
- **Assíncrona** via *RabbitMQ* (mensageria com filas e DLQs)
- **Balanceamento de carga** e *Service Discovery* via *Eureka Server*
- **Gateway Routing** e controle de acesso via *Spring Cloud Gateway*

---

## ☁️ Infraestrutura e Deploy

Infraestrutura provisionada com **AWS CDK (IaC)**, implementando:

- **ECS Fargate** para execução dos containers
- **ECR** para armazenamento das imagens Docker
- **RDS (MySQL/PostgreSQL)** como banco gerenciado
- **CloudWatch** para logs e monitoramento
- **Auto Scaling** configurado para ajuste automático de carga

Deploy automatizado via *stacks* do CloudFormation, permitindo replicar o ambiente completo com um único comando.

---

## 🧠 Aprendizados-Chave

- Design e organização de microsserviços
- Isolamento de infraestrutura e pipelines independentes
- Comunicação síncrona (Feign) e assíncrona (RabbitMQ)
- Resiliência com *Circuit Breaker* e *Fallback*
- Deploy automatizado com AWS CDK + ECS Fargate
- Monitoramento e métricas com CloudWatch
- Versionamento de bancos com *Migrations*

---

## 🧰 Tecnologias Utilizadas

- **Java 21**  
- **Spring Boot 3.x**
- **Spring Cloud (Eureka, Gateway)**
- **RabbitMQ 3.10**
- **PostgreSQL 15**
- **Docker & Docker Compose**
- **AWS CDK / ECS / RDS / CloudWatch**
- **Maven 3.9.9**

---

## 🧱 Estrutura do Repositório

```bash
petit-food/
├── compose.yml
├── init-db.sql
├── petit-food-infra/      # IaC e deploys AWS com CDK
├── petit-food-server/     # Service Discovery (Eureka)
├── petit-food-gateway/    # API Gateway
├── petit-food-payments/   # Microsserviço de Pagamentos
└── petit-food-orders/     # Microsserviço de Pedidos
```

## Como Executar Localmente
1. Suba os containers base:
  ```bash
  # 1. Clonar o repositório com todos os submódulos
  git clone --recurse-submodules https://github.com/RafaelPetit/petit-food.git
  
  # 2. Entrar no diretório
  cd petit-food
  
  # 3. Subir todos os serviços
  docker-compose up -d --build
  ```

2. Verifique os acessos:  
  - **Eureka:** http://localhost:8081  
  - **Gateway:** http://localhost:8082  
  - **RabbitMQ Management:** http://localhost:15672 (guest/guest)  
  - **PostgreSQL:** localhost:5432 (postgres/postgres)  

3. Ordem de Inicialização:  
  - **PostgreSQL e RabbitMQ** (com healthcheck)  
  - **Server (Eureka)** — aguarda até estar saudável  
  - **Gateway, Payments, Orders** — aguardam o Eureka estar pronto

## 🏁 Próximos Passos
- Esta POC é o ponto de partida para explorar práticas mais avançadas como:
- Monitoramento centralizado e observabilidade com Grafana + Prometheus
- Autenticação distribuída (OAuth2 / Keycloak)
- Integração de CI/CD com pipelines GitHub Actions ou Jenkins
