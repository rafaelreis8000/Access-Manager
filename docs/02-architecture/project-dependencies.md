# Access Manager
# Dependências entre Projetos | Project Dependencies

**Documento:** AM-ARC-002
**Versão:** 1.0
**Status:** Proposta arquitetural | Architectural proposal
**Projeto:** Access Manager
**Última atualização:** 19/11/2026

---

# PARTE I — PORTUGUÊS

## 1. Objetivo

Este documento estabelece as regras de dependência entre os sete projetos que compõem a solução Access Manager.

Seu objetivo é garantir uma arquitetura organizada, modular, sustentável e de fácil manutenção, evitando acoplamento desnecessário e dependências circulares.

## 2. Projetos envolvidos

A solução é composta pelos seguintes projetos:

1. AccessManager.Domain
2. AccessManager.Application
3. AccessManager.Security
4. AccessManager.Infrastructure
5. AccessManager.Contracts
6. AccessManager.Api
7. AccessManager.Desktop

## 3. Diretrizes arquiteturais

As dependências deverão respeitar os seguintes princípios:

- As regras de negócio não devem depender de detalhes de infraestrutura.
- A camada de domínio deve permanecer independente.
- As implementações concretas devem depender de abstrações.
- A API será responsável pela composição dos componentes da aplicação.
- O Desktop deverá se comunicar com o backend exclusivamente por meio da API.
- O Desktop não poderá acessar diretamente o banco de dados.
- Não serão permitidas dependências circulares entre projetos.

## 4. Matriz de dependências permitidas

A tabela apresenta as referências entre projetos propostas para a arquitetura.

| Projeto de origem | Projeto de destino | Permissão |
|---|---|---|
| Domain | Nenhum projeto interno | Não depende |
| Application | Domain | Permitida |
| Security | Application | Permitida |
| Infrastructure | Application | Permitida |
| Infrastructure | Domain | Permitida |
| Contracts | Nenhum projeto interno | Não depende |
| API | Application | Permitida |
| API | Infrastructure | Permitida |
| API | Security | Permitida |
| API | Contracts | Permitida |
| Desktop | Contracts | Permitida |

As dependências não listadas na matriz não deverão ser adicionadas sem justificativa arquitetural e registro de decisão.

## 5. Responsabilidades das dependências

### 5.1 Domain

O projeto Domain representa o núcleo do negócio.

Não deverá possuir referências a outros projetos da solução.

### 5.2 Application

O projeto Application utilizará o Domain para executar os casos de uso.

Também deverá definir interfaces e abstrações para serviços externos, persistência e operações especializadas.

### 5.3 Security

O projeto Security implementará os mecanismos especializados de segurança e as abstrações criptográficas definidas pela camada Application.

A implementação deverá utilizar bibliotecas criptográficas confiáveis, sem desenvolvimento de algoritmos próprios.

### 5.4 Infrastructure

O projeto Infrastructure implementará os mecanismos de persistência, comunicação com banco de dados e outros serviços externos.

Deverá implementar as interfaces definidas pela Application.

### 5.5 Contracts

O projeto Contracts manterá os contratos de comunicação entre componentes, especialmente os modelos de requisição e resposta utilizados pela API e pelo Desktop.

Não deverá depender das implementações internas de domínio, infraestrutura ou segurança.

### 5.6 API

O projeto API atuará como ponto de entrada da aplicação.

Será responsável por configurar e integrar as implementações de Application, Infrastructure e Security, além de expor os contratos HTTP.

A API será também o ponto de composição das dependências da aplicação.

### 5.7 Desktop

O projeto Desktop será responsável pela interface do usuário.

Deverá utilizar Contracts para estruturar a comunicação com a API.

Não poderá possuir referências diretas a Domain, Application, Infrastructure ou ao banco de dados.

## 6. Regras para inclusão de dependências

Toda nova referência entre projetos deverá:

1. Ter justificativa técnica.
2. Respeitar a separação de responsabilidades.
3. Não introduzir dependências circulares.
4. Ser avaliada quanto ao impacto na segurança.
5. Ser documentada quando alterar a arquitetura estabelecida.
6. Ser validada por compilação e testes.

## 7. Estratégia de composição

A API será responsável por registrar e configurar as implementações concretas das interfaces utilizadas pela aplicação.

As implementações de persistência e segurança serão integradas por meio de injeção de dependência.

Esse modelo permitirá substituir implementações sem modificar as regras centrais do negócio.

## 8. Estado de implementação

Este documento define a arquitetura de dependências pretendida.

As referências efetivamente configuradas nos arquivos `.csproj` deverão ser verificadas e ajustadas de acordo com estas diretrizes.

Nenhuma dependência deverá ser considerada implementada apenas por estar descrita neste documento.

---

# PART II — ENGLISH

## 1. Purpose

This document establishes dependency rules between the seven projects that compose the Access Manager solution.

Its purpose is to ensure an organized, modular, maintainable architecture while avoiding unnecessary coupling and circular dependencies.

## 2. Projects involved

The solution consists of:

1. AccessManager.Domain
2. AccessManager.Application
3. AccessManager.Security
4. AccessManager.Infrastructure
5. AccessManager.Contracts
6. AccessManager.Api
7. AccessManager.Desktop

## 3. Architectural guidelines

Dependencies shall follow these principles:

- Business rules shall not depend on infrastructure details.
- The domain layer shall remain independent.
- Concrete implementations shall depend on abstractions.
- The API shall be responsible for application composition.
- The Desktop application shall communicate with the backend exclusively through the API.
- The Desktop application shall not directly access the database.
- Circular project dependencies shall not be allowed.

## 4. Allowed dependency matrix

| Source project | Target project | Permission |
|---|---|---|
| Domain | No internal project | No dependency |
| Application | Domain | Allowed |
| Security | Application | Allowed |
| Infrastructure | Application | Allowed |
| Infrastructure | Domain | Allowed |
| Contracts | No internal project | No dependency |
| API | Application | Allowed |
| API | Infrastructure | Allowed |
| API | Security | Allowed |
| API | Contracts | Allowed |
| Desktop | Contracts | Allowed |

Dependencies not listed in this matrix shall not be introduced without architectural justification and a recorded decision.

## 5. Dependency responsibilities

### 5.1 Domain

Domain represents the business core.

It shall not reference other projects within the solution.

### 5.2 Application

Application shall use Domain to execute application use cases.

It shall also define interfaces and abstractions for external services, persistence and specialized operations.

### 5.3 Security

Security shall implement specialized security mechanisms and cryptographic abstractions defined by the Application layer.

Trusted cryptographic libraries shall be used instead of implementing custom cryptographic algorithms.

### 5.4 Infrastructure

Infrastructure shall implement persistence mechanisms, database communication and other external services.

It shall implement interfaces defined by Application.

### 5.5 Contracts

Contracts shall maintain communication contracts between components, especially request and response models used by the API and Desktop applications.

It shall not depend on internal Domain, Infrastructure or Security implementations.

### 5.6 API

API shall act as the application's entry point.

It shall configure and integrate Application, Infrastructure and Security implementations while exposing HTTP contracts.

API shall also serve as the application's composition root.

### 5.7 Desktop

Desktop shall be responsible for the user interface.

It shall use Contracts to structure communication with the API.

It shall not directly reference Domain, Application, Infrastructure or the database.

## 6. Rules for introducing dependencies

Every new project reference shall:

1. Have a technical justification.
2. Respect separation of concerns.
3. Avoid circular dependencies.
4. Be evaluated for security impact.
5. Be documented when changing the established architecture.
6. Be validated through compilation and testing.

## 7. Composition strategy

API shall register and configure concrete implementations of interfaces used by the application.

Persistence and security implementations shall be integrated through dependency injection.

This model will allow implementations to be replaced without modifying core business rules.

## 8. Implementation status

This document defines the intended dependency architecture.

Actual references configured in `.csproj` files shall be verified and adjusted according to these guidelines.

A dependency shall not be considered implemented merely because it is described in this document.

---

**Document identification:** AM-ARC-002
**Version:** 1.0
**Status:** Initial architectural proposal