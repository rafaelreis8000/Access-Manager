# Access Manager
# Registro de Decisões Arquiteturais | Architecture Decision Records

**Documento:** AM-ARC-003
**Versão:** 1.0
**Status:** Registro inicial | Initial record
**Projeto:** Access Manager
**Última atualização:** 19/11/2026

---

# PARTE I — PORTUGUÊS

## 1. Objetivo

Este documento estabelece o processo de registro e gerenciamento das decisões arquiteturais do Access Manager.

As decisões arquiteturais influenciam diretamente a estrutura, a segurança, a manutenção, a escalabilidade e a evolução do sistema.

O objetivo dos Architecture Decision Records (ADR) é manter um histórico documentado das escolhas técnicas realizadas durante o desenvolvimento.

Cada decisão deverá apresentar seu contexto, a escolha realizada, as alternativas consideradas e suas consequências.

## 2. Estrutura de um ADR

Cada decisão arquitetural deverá seguir a seguinte estrutura:

- Identificação: código único da decisão.
- Título: descrição resumida da decisão.
- Status: situação atual da decisão.
- Contexto: problema ou necessidade que motivou a decisão.
- Decisão: solução escolhida.
- Alternativas consideradas: outras possibilidades avaliadas.
- Justificativa: motivos técnicos da escolha.
- Consequências: impactos positivos, negativos e limitações.

## 3. Estados das decisões

As decisões poderão apresentar os seguintes estados:

| Status | Descrição |
|---|---|
| Proposta | Decisão em avaliação. |
| Aprovada | Decisão aceita e adotada como diretriz. |
| Implementada | Decisão aplicada e validada no código. |
| Substituída | Decisão substituída por outra. |
| Rejeitada | Alternativa avaliada e não adotada. |

Uma decisão aprovada não deverá ser considerada implementada sem evidências técnicas.

## 4. Registro das decisões arquiteturais

### ADR-001 — Linguagem de programação e plataforma

**Status:** Aprovada

**Contexto:**

O projeto necessita de uma plataforma de desenvolvimento moderna, com suporte a orientação a objetos, tipagem estática, ferramentas de desenvolvimento e integração com aplicações desktop e serviços web.

**Decisão:**

Adotar C# como linguagem principal e .NET 10 como plataforma de desenvolvimento.

**Alternativas consideradas:**

- Java.
- Python.
- JavaScript/TypeScript.
- C# com .NET.

**Justificativa:**

C# oferece recursos robustos de orientação a objetos, tipagem estática, gerenciamento de memória, bibliotecas de segurança e integração com o ecossistema .NET.

**Consequências:**

- Padronização da linguagem em toda a solução.
- Compartilhamento de bibliotecas e contratos.
- Facilidade de manutenção.
- Dependência do ecossistema .NET.

### ADR-002 — Tecnologia da aplicação desktop

**Status:** Aprovada

**Contexto:**

A plataforma necessita de uma interface gráfica desktop, inicialmente destinada ao Windows, com possibilidade de expansão para outros sistemas operacionais.

**Decisão:**

Adotar Avalonia UI para o desenvolvimento da aplicação desktop, utilizando o padrão arquitetural MVVM.

**Alternativas consideradas:**

- Windows Presentation Foundation (WPF).
- Windows Forms.
- .NET MAUI.
- Avalonia UI.

**Justificativa:**

Avalonia permite desenvolver interfaces desktop multiplataforma utilizando C# e XAML, mantendo uma estrutura adequada à separação entre interface e lógica de apresentação.

**Consequências:**

- Possibilidade de evolução para Linux e macOS.
- Separação entre View e ViewModel.
- Maior organização da interface.
- Necessidade de testes específicos de compatibilidade entre sistemas operacionais.

### ADR-003 — Arquitetura do backend

**Status:** Aprovada

**Contexto:**

O sistema deverá permitir que diferentes usuários utilizem a plataforma em computadores distintos, compartilhando informações por meio de uma estrutura centralizada.

**Decisão:**

Adotar ASP.NET Core Web API como backend da aplicação.

**Alternativas consideradas:**

- Aplicação desktop conectada diretamente ao banco.
- Backend baseado em ASP.NET Core.
- Backend baseado em outras tecnologias web.

**Justificativa:**

A API centralizada permite separar a interface do usuário das regras de negócio, da autenticação e da persistência.

Também facilita a implementação de controles de segurança e a evolução futura do sistema.

**Consequências:**

- Comunicação centralizada.
- Controle de acesso no servidor.
- Separação entre frontend e backend.
- Necessidade de proteger e monitorar os endpoints da API.

### ADR-004 — Banco de dados

**Status:** Aprovada

**Contexto:**

O Access Manager deverá armazenar informações de usuários, permissões, categorias, metadados e dados criptografados de credenciais.

A solução necessita de um banco de dados confiável, com suporte a transações, integridade referencial e concorrência.

**Decisão:**

Adotar PostgreSQL como sistema gerenciador de banco de dados relacional.

**Alternativas consideradas:**

- Microsoft SQL Server.
- MySQL.
- MariaDB.
- PostgreSQL.

**Justificativa:**

PostgreSQL oferece recursos robustos de integridade, transações, controle de concorrência e extensibilidade.

**Consequências:**

- Persistência centralizada.
- Suporte a múltiplos usuários.
- Necessidade de políticas de backup e recuperação.
- Necessidade de gerenciamento seguro das credenciais de conexão.

### ADR-005 — Comunicação entre aplicação desktop e backend

**Status:** Aprovada

**Contexto:**

A aplicação deverá operar em múltiplos computadores sem permitir acesso direto dos clientes ao banco de dados.

**Decisão:**

Estabelecer comunicação entre Desktop e API exclusivamente por HTTPS.

O acesso ao PostgreSQL será realizado apenas pelos componentes de backend autorizados.

**Alternativas consideradas:**

- Conexão direta do Desktop ao PostgreSQL.
- Comunicação por HTTP sem criptografia.
- Comunicação por API protegida por HTTPS.

**Justificativa:**

A arquitetura centralizada permite controlar autenticação, autorização, validação e operações de persistência em um ponto controlado.

O HTTPS protege a comunicação contra interceptação e alteração dos dados durante o transporte.

**Consequências:**

- Banco de dados isolado dos clientes.
- Centralização dos controles de acesso.
- Necessidade de certificados TLS adequados.
- Necessidade de configuração segura da rede e da API.

### ADR-006 — Separação da solução em projetos

**Status:** Aprovada

**Contexto:**

O projeto possui diferentes responsabilidades, incluindo regras de negócio, segurança, persistência, contratos de comunicação, serviços web e interface gráfica.

Misturar essas responsabilidades dificultaria a manutenção e a evolução da solução.

**Decisão:**

Organizar a solução em sete projetos:

1. AccessManager.Domain.
2. AccessManager.Application.
3. AccessManager.Security.
4. AccessManager.Infrastructure.
5. AccessManager.Contracts.
6. AccessManager.Api.
7. AccessManager.Desktop.

**Alternativas consideradas:**

- Aplicação monolítica em um único projeto.
- Separação em poucos projetos por tecnologia.
- Arquitetura modular com separação por responsabilidade.

**Justificativa:**

A separação facilita o isolamento de responsabilidades, a manutenção, os testes e a evolução independente dos componentes.

**Consequências:**

- Maior organização do código.
- Melhor isolamento das responsabilidades.
- Necessidade de regras claras de dependência.
- Maior esforço inicial de organização.

### ADR-007 — Controle de versão e hospedagem do código

**Status:** Aprovada

**Contexto:**

O projeto necessita de histórico de alterações, rastreabilidade, controle de versões e possibilidade de colaboração.

**Decisão:**

Adotar Git para controle de versão e GitHub como plataforma de hospedagem do repositório.

As mensagens de commit serão bilíngues, em português e inglês, seguindo o padrão Conventional Commits.

**Alternativas consideradas:**

- Controle manual de versões.
- Git com hospedagem própria.
- Git com GitHub.

**Justificativa:**

Git permite rastrear alterações, recuperar versões anteriores e manter histórico de desenvolvimento.

GitHub oferece hospedagem e recursos de colaboração e gerenciamento do projeto.

**Consequências:**

- Histórico de desenvolvimento.
- Rastreabilidade das alterações.
- Possibilidade de colaboração.
- Necessidade de proteção do repositório e revisão de alterações.

Nenhuma credencial real, chave privada, token ou dado sensível deverá ser publicado no repositório.

### ADR-008 — Estratégia criptográfica das credenciais

**Status:** Proposta

**Contexto:**

O sistema armazenará credenciais que precisam permanecer confidenciais mesmo diante de acesso indevido ao banco de dados ou à infraestrutura do servidor.

**Decisão proposta:**

Adotar uma estratégia de criptografia de ponta a ponta para os dados sensíveis do cofre, utilizando criptografia de envelope.

A estratégia considera:

- AES-256-GCM para criptografia autenticada dos dados.
- Chaves de dados (DEK) para criptografar os conteúdos.
- Proteção das DEKs por mecanismos de encapsulamento de chaves.
- Chaves individualizadas para os usuários autorizados.
- Nonces únicos para cada operação com a mesma chave.
- Versionamento dos formatos criptográficos.
- Chave de recuperação gerada por fonte criptograficamente segura.

**Alternativas consideradas:**

- Armazenamento de senhas em texto claro.
- Criptografia convencional realizada exclusivamente no servidor.
- Criptografia com proteção das chaves no cliente.

**Justificativa:**

A criptografia realizada no cliente, com gerenciamento adequado das chaves, reduz a exposição do conteúdo sensível ao servidor.

**Consequências:**

- Maior proteção contra exposição do banco de dados.
- Maior complexidade no gerenciamento de chaves.
- Necessidade de mecanismos seguros de recuperação.
- Necessidade de revisão especializada do protocolo criptográfico.
- Impossibilidade de recuperar dados criptografados caso todas as chaves necessárias sejam perdidas.

**Observação:**

Esta decisão ainda precisa de especificação técnica detalhada, análise de ameaças, revisão criptográfica e testes de segurança antes de sua aprovação definitiva.

### ADR-009 — Autenticação multifator

**Status:** Aprovada como diretriz inicial

**Contexto:**

O acesso à plataforma deverá exigir mecanismos adicionais de verificação de identidade, reduzindo o risco de comprometimento por exposição de senhas.

**Decisão:**

Adotar autenticação multifator (MFA), inicialmente utilizando TOTP (Time-based One-Time Password).

A identidade dos usuários será associada a seus endereços de e-mail.

**Alternativas consideradas:**

- Somente senha.
- Código enviado por e-mail.
- TOTP.
- Autenticação por chaves de segurança.

**Justificativa:**

TOTP permite gerar códigos temporários por aplicativos autenticadores, sem depender de envio de mensagens a cada autenticação.

**Consequências:**

- Camada adicional de autenticação.
- Necessidade de armazenamento protegido dos segredos TOTP.
- Necessidade de fluxo seguro de ativação, recuperação e substituição do autenticador.

A implementação deverá definir políticas de recuperação e proteção contra tentativas automatizadas.

## 5. Processo de revisão das decisões

As decisões deverão ser revisadas quando:

- Uma tecnologia for substituída.
- A arquitetura sofrer alterações relevantes.
- Uma decisão apresentar riscos não considerados anteriormente.
- Novos requisitos funcionais ou não funcionais forem identificados.
- A evolução do projeto tornar uma decisão inadequada.

Decisões substituídas deverão permanecer registradas para preservar o histórico arquitetural.

## 6. Controle documental

Os ADRs deverão permanecer versionados no repositório Git.

Cada nova decisão deverá receber um identificador sequencial e um registro próprio.

Alterações de decisões existentes deverão preservar o histórico e indicar a justificativa da revisão.

---

# PART II — ENGLISH

## 1. Purpose

This document establishes the process for recording and managing architectural decisions within Access Manager.

Architectural decisions directly influence system structure, security, maintainability, scalability and evolution.

The purpose of Architecture Decision Records (ADR) is to maintain a documented history of technical choices made during development.

Each decision shall include its context, selected solution, considered alternatives and consequences.

## 2. ADR structure

Each architectural decision shall follow this structure:

- Identification: unique decision code.
- Title: brief description of the decision.
- Status: current decision status.
- Context: problem or requirement that motivated the decision.
- Decision: selected solution.
- Considered alternatives: other evaluated options.
- Rationale: technical reasons for the choice.
- Consequences: positive and negative impacts and limitations.

## 3. Decision statuses

| Status | Description |
|---|---|
| Proposed | Decision under evaluation. |
| Approved | Decision accepted as an architectural guideline. |
| Implemented | Decision applied and validated in code. |
| Superseded | Decision replaced by another decision. |
| Rejected | Evaluated alternative that was not adopted. |

An approved decision shall not be considered implemented without technical evidence.

## 4. Architectural decision records

### ADR-001 — Programming language and platform

**Status:** Approved

**Context:**

The project requires a modern development platform supporting object-oriented programming, static typing, development tools and integration with desktop applications and web services.

**Decision:**

Adopt C# as the primary programming language and .NET 10 as the development platform.

**Considered alternatives:**

- Java.
- Python.
- JavaScript/TypeScript.
- C# with .NET.

**Rationale:**

C# provides robust object-oriented programming features, static typing, memory management, security libraries and integration with the .NET ecosystem.

**Consequences:**

- Language standardization across the solution.
- Shared libraries and contracts.
- Easier maintenance.
- Dependency on the .NET ecosystem.

### ADR-002 — Desktop application technology

**Status:** Approved

**Context:**

The platform requires a graphical desktop interface initially targeting Windows, with potential expansion to other operating systems.

**Decision:**

Adopt Avalonia UI for desktop application development, using the MVVM architectural pattern.

**Considered alternatives:**

- Windows Presentation Foundation (WPF).
- Windows Forms.
- .NET MAUI.
- Avalonia UI.

**Rationale:**

Avalonia enables cross-platform desktop interfaces using C# and XAML, supporting separation between interface and presentation logic.

**Consequences:**

- Potential expansion to Linux and macOS.
- Separation between View and ViewModel.
- Better interface organization.
- Need for platform-specific compatibility testing.

### ADR-003 — Backend architecture

**Status:** Approved

**Context:**

The system must allow different users to access the platform from separate computers while sharing information through a centralized structure.

**Decision:**

Adopt ASP.NET Core Web API as the application backend.

**Considered alternatives:**

- Desktop application directly connected to the database.
- ASP.NET Core backend.
- Backend based on other web technologies.

**Rationale:**

A centralized API separates the user interface from business rules, authentication and persistence.

It also facilitates security controls and future system evolution.

**Consequences:**

- Centralized communication.
- Server-side access control.
- Separation between frontend and backend.
- Need to protect and monitor API endpoints.

### ADR-004 — Database

**Status:** Approved

**Context:**

Access Manager must store user information, permissions, categories, metadata and encrypted credential data.

The solution requires a reliable database with transaction support, referential integrity and concurrency management.

**Decision:**

Adopt PostgreSQL as the relational database management system.

**Considered alternatives:**

- Microsoft SQL Server.
- MySQL.
- MariaDB.
- PostgreSQL.

**Rationale:**

PostgreSQL provides robust integrity, transaction, concurrency control and extensibility features.

**Consequences:**

- Centralized persistence.
- Multi-user support.
- Need for backup and recovery policies.
- Need for secure database connection credential management.

### ADR-005 — Communication between desktop application and backend

**Status:** Approved

**Context:**

The application must operate across multiple computers without allowing clients direct access to the database.

**Decision:**

Establish communication between Desktop and API exclusively over HTTPS.

PostgreSQL access shall be performed only by authorized backend components.

**Considered alternatives:**

- Direct Desktop connection to PostgreSQL.
- Unencrypted HTTP communication.
- API communication protected by HTTPS.

**Rationale:**

The centralized architecture allows authentication, authorization, validation and persistence operations to be controlled at a managed point.

HTTPS protects communication against interception and modification during transmission.

**Consequences:**

- Database isolation from clients.
- Centralized access controls.
- Need for appropriate TLS certificates.
- Need for secure network and API configuration.

### ADR-006 — Solution project separation

**Status:** Approved

**Context:**

The project has different responsibilities, including business rules, security, persistence, communication contracts, web services and graphical user interface.

Mixing these responsibilities would make maintenance and solution evolution more difficult.

**Decision:**

Organize the solution into seven projects:

1. AccessManager.Domain.
2. AccessManager.Application.
3. AccessManager.Security.
4. AccessManager.Infrastructure.
5. AccessManager.Contracts.
6. AccessManager.Api.
7. AccessManager.Desktop.

**Considered alternatives:**

- Single-project monolithic application.
- Separation into a few technology-based projects.
- Modular architecture with responsibility-based separation.

**Rationale:**

Separation facilitates responsibility isolation, maintenance, testing and independent component evolution.

**Consequences:**

- Better code organization.
- Improved responsibility isolation.
- Need for clear dependency rules.
- Higher initial organization effort.

### ADR-007 — Version control and code hosting

**Status:** Approved

**Context:**

The project requires change history, traceability, version control and collaboration capabilities.

**Decision:**

Adopt Git for version control and GitHub as the repository hosting platform.

Commit messages shall be bilingual, in Portuguese and English, following the Conventional Commits standard.

**Considered alternatives:**

- Manual version control.
- Git with self-hosted hosting.
- Git with GitHub.

**Rationale:**

Git allows changes to be tracked, previous versions to be restored and development history to be maintained.

GitHub provides repository hosting, collaboration and project management capabilities.

**Consequences:**

- Development history.
- Change traceability.
- Collaboration capabilities.
- Need for repository protection and change reviews.

No real credentials, private keys, tokens or sensitive data shall be published in the repository.

### ADR-008 — Credential cryptographic strategy

**Status:** Proposed

**Context:**

The system will store credentials that must remain confidential even in the event of unauthorized access to the database or server infrastructure.

**Proposed decision:**

Adopt an end-to-end encryption strategy for sensitive vault data, using envelope encryption.

The strategy considers:

- AES-256-GCM for authenticated data encryption.
- Data Encryption Keys (DEKs) for encrypting content.
- DEK protection through key-wrapping mechanisms.
- Individualized keys for authorized users.
- Unique nonces for each operation using the same key.
- Cryptographic format versioning.
- Recovery key generated using a cryptographically secure source.

**Considered alternatives:**

- Plaintext password storage.
- Conventional encryption performed exclusively on the server.
- Client-side encryption with protected key management.

**Rationale:**

Client-side encryption, with appropriate key management, reduces server exposure to sensitive content.

**Consequences:**

- Greater protection against database exposure.
- Increased key management complexity.
- Need for secure recovery mechanisms.
- Need for specialized cryptographic protocol review.
- Encrypted data may become unrecoverable if all required keys are lost.

**Note:**

This decision still requires detailed technical specification, threat analysis, cryptographic review and security testing before final approval.

### ADR-009 — Multi-factor authentication

**Status:** Approved as an initial guideline

**Context:**

Platform access shall require additional identity verification mechanisms, reducing the risk of compromise through password exposure.

**Decision:**

Adopt multi-factor authentication (MFA), initially using TOTP (Time-based One-Time Password).

User identity shall be associated with email addresses.

**Considered alternatives:**

- Password only.
- Email-delivered codes.
- TOTP.
- Security key authentication.

**Rationale:**

TOTP generates temporary codes through authenticator applications without requiring message delivery for every authentication.

**Consequences:**

- Additional authentication layer.
- Need for protected storage of TOTP secrets.
- Need for secure authenticator activation, recovery and replacement flows.

Implementation shall define recovery policies and protection against automated attempts.

## 5. Decision review process

Decisions shall be reviewed when:

- A technology is replaced.
- The architecture undergoes significant changes.
- A decision presents previously unidentified risks.
- New functional or non-functional requirements are identified.
- Project evolution makes a decision unsuitable.

Superseded decisions shall remain recorded to preserve architectural history.

## 6. Document control

ADRs shall remain version-controlled in the Git repository.

Each new decision shall receive a sequential identifier and its own record.

Changes to existing decisions shall preserve history and indicate the rationale for the revision.

---

**Document identification:** AM-ARC-003
**Version:** 1.0
**Status:** Initial architectural record