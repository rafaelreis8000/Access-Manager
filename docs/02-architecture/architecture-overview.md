# Access Manager
# Visão Geral da Arquitetura | Architecture Overview

**Documento:** AM-ARC-001
**Versão:** 1.0
**Status:** Proposta inicial de arquitetura | Initial architectural proposal
**Projeto:** Access Manager
**Tecnologia principal:** C# / .NET 10
**Última atualização:** 19/11/2026

---

# PARTE I — PORTUGUÊS

## 1. Objetivo

Este documento estabelece a visão geral da arquitetura de software do Access Manager, uma plataforma destinada ao gerenciamento centralizado de credenciais de acesso, incluindo logins, senhas e informações associadas.

A arquitetura deverá proporcionar:

- Segurança e confidencialidade das credenciais armazenadas.
- Gerenciamento de usuários e permissões.
- Organização das credenciais por categorias.
- Autenticação com múltiplos fatores.
- Controle de acesso baseado em funções e permissões.
- Disponibilidade para múltiplos usuários e computadores.
- Manutenibilidade, escalabilidade e possibilidade de evolução tecnológica.

## 2. Escopo da arquitetura

O Access Manager será inicialmente desenvolvido para ambiente Windows, com arquitetura preparada para futura compatibilidade com Linux e macOS.

A solução será composta por uma aplicação desktop, uma API centralizada e um banco de dados relacional.

A comunicação entre o aplicativo desktop e a API será realizada por HTTPS. O acesso ao banco de dados será responsabilidade exclusiva da API e de seus componentes de infraestrutura.

O aplicativo desktop não terá conexão direta com o banco de dados.

## 3. Tecnologias adotadas

| Componente | Tecnologia |
|---|---|
| Linguagem | C# |
| Plataforma | .NET 10 |
| Aplicação desktop | Avalonia UI |
| Padrão da interface | MVVM |
| Backend | ASP.NET Core Web API |
| Banco de dados | PostgreSQL |
| Controle de versão | Git |
| Repositório | GitHub |
| Metodologia | Scrum |
| Arquitetura | Separação em camadas |

## 4. Organização da solução

A solução será organizada em sete projetos principais de modo a organizar e manter modularidade e praticidade na estrutura e no desenvolvimento:

### 4.1 AccessManager.Domain

Responsável pelas entidades, regras de negócio fundamentais, objetos de valor e conceitos centrais do domínio.

Deverá permanecer independente de tecnologias externas de infraestrutura.

### 4.2 AccessManager.Application

Responsável pelos casos de uso e pela coordenação das operações da aplicação.

Deverá conter as abstrações necessárias para comunicação com os componentes externos, sem depender diretamente de implementações concretas de infraestrutura.

### 4.3 AccessManager.Security

Projeto destinado aos componentes especializados de segurança, incluindo mecanismos criptográficos e funcionalidades relacionadas à proteção das credenciais.

Sua integração será definida por interfaces e contratos apropriados.

### 4.4 AccessManager.Infrastructure

Responsável pela implementação dos mecanismos de infraestrutura, incluindo persistência de dados, acesso ao PostgreSQL e integração com serviços externos.

### 4.5 AccessManager.Contracts

Responsável pelos contratos de comunicação, DTOs e estruturas de dados utilizadas na integração entre os componentes da solução.

### 4.6 AccessManager.Api

Responsável por disponibilizar os serviços do Access Manager por meio de uma API HTTP.

Deverá implementar os mecanismos de autenticação, autorização, validação de requisições e comunicação com a camada de aplicação.

### 4.7 AccessManager.Desktop

Responsável pela interface gráfica utilizada pelos usuários.

Será desenvolvido com Avalonia UI e padrão MVVM, comunicando-se com a API por HTTPS.

## 5. Visão geral da comunicação

O fluxo principal de comunicação será:

1. O usuário interage com o aplicativo desktop.
2. O aplicativo realiza requisições à API por HTTPS.
3. A API valida a identidade, as permissões e os dados recebidos.
4. A camada de aplicação executa o caso de uso solicitado.
5. Os componentes de infraestrutura realizam as operações necessárias no PostgreSQL.
6. A API devolve uma resposta ao aplicativo desktop.

A proteção criptográfica das credenciais deverá ser implementada de modo que o servidor não tenha acesso ao conteúdo em texto claro das senhas armazenadas.

## 6. Segurança da informação

A segurança será um requisito essencial da arquitetura.

Os seguintes princípios orientarão o desenvolvimento:

- Criptografia de credenciais armazenadas.
- Comunicação protegida por HTTPS.
- Autenticação multifator (MFA).
- Controle de acesso baseado em funções (RBAC).
- Separação entre autenticação da conta e desbloqueio do cofre.
- Gerenciamento seguro de chaves criptográficas.
- Proteção contra acesso não autorizado.
- Registro de eventos relevantes de segurança.
- Minimização de dados pessoais armazenados.

A estratégia criptográfica prevista considera AES-256-GCM, criptografia de envelope e gerenciamento individualizado de chaves por usuário.

Esses mecanismos ainda deverão ser especificados, revisados e testados antes de serem considerados implementados.

## 7. Proteção de dados e conformidade

O projeto deverá considerar:

- Lei Geral de Proteção de Dados Pessoais (LGPD).
- Constituição Federal de 1988, especialmente os dispositivos relacionados à privacidade e à proteção de dados.
- ISO 9001, para processos de gestão da qualidade.
- ISO/IEC 27001, como referência para gestão da segurança da informação.
- ISO/IEC 27701, como referência para gestão da privacidade da informação.

A conformidade deverá ser demonstrada por controles implementados, documentação, testes e evidências verificáveis.

## 8. Princípios arquiteturais

O desenvolvimento deverá observar:

1. Separação de responsabilidades.
2. Baixo acoplamento entre componentes.
3. Alta coesão.
4. Encapsulamento.
5. Independência de infraestrutura no domínio.
6. Segurança desde a concepção (Security by Design).
7. Privacidade desde a concepção (Privacy by Design).
8. Testabilidade.
9. Manutenibilidade.
10. Evolução incremental.

## 9. Estratégia de evolução

A arquitetura deverá permitir:

- Inclusão de novos módulos funcionais.
- Ampliação dos mecanismos de autenticação.
- Evolução das políticas de autorização.
- Integração com provedores externos.
- Ampliação da infraestrutura de hospedagem.
- Compatibilidade com outros sistemas operacionais.
- Evolução dos mecanismos de auditoria e monitoramento.

## 10. Estado atual

A estrutura inicial da solução foi criada e compilada com sucesso.

Os sete projetos estão presentes na solução .NET e o primeiro commit foi publicado no GitHub.

Entretanto, a arquitetura funcional, as integrações, os mecanismos de segurança e a persistência ainda estão em fase de especificação e desenvolvimento.

---

# PART II — ENGLISH

## 1. Purpose

This document establishes the overall software architecture of Access Manager, a platform designed for centralized management of access credentials, including usernames, passwords and associated information.

The architecture shall provide:

- Security and confidentiality of stored credentials.
- User and permission management.
- Credential organization by categories.
- Multi-factor authentication.
- Role-based access control.
- Availability for multiple users and computers.
- Maintainability, scalability and technological evolution.

## 2. Architecture scope

Access Manager will initially be developed for Windows environments, with an architecture prepared for future Linux and macOS compatibility.

The solution will consist of a desktop application, a centralized API and a relational database.

Communication between the desktop application and the API will take place over HTTPS. Database access will be exclusively handled by the API and its infrastructure components.

The desktop application will not connect directly to the database.

## 3. Adopted technologies

| Component | Technology |
|---|---|
| Programming language | C# |
| Platform | .NET 10 |
| Desktop application | Avalonia UI |
| UI pattern | MVVM |
| Backend | ASP.NET Core Web API |
| Database | PostgreSQL |
| Version control | Git |
| Repository | GitHub |
| Methodology | Scrum |
| Architecture | Layered architecture |

## 4. Solution organization

The solution will be organized into seven main projects aiming for modularity and security in developing:

### 4.1 AccessManager.Domain

Responsible for domain entities, fundamental business rules, value objects and core business concepts.

It shall remain independent of external infrastructure technologies.

### 4.2 AccessManager.Application

Responsible for application use cases and operation orchestration.

It shall contain the abstractions required for communication with external components, without directly depending on concrete infrastructure implementations.

### 4.3 AccessManager.Security

Dedicated to specialized security components, including cryptographic mechanisms and credential protection features.

Its integration shall be defined through appropriate interfaces and contracts.

### 4.4 AccessManager.Infrastructure

Responsible for infrastructure implementations, including data persistence, PostgreSQL access and external service integration.

### 4.5 AccessManager.Contracts

Responsible for communication contracts, DTOs and data structures used for integration between solution components.

### 4.6 AccessManager.Api

Responsible for exposing Access Manager services through an HTTP API.

It shall implement authentication, authorization, request validation and communication with the application layer.

### 4.7 AccessManager.Desktop

Responsible for the graphical user interface.

It will be developed using Avalonia UI and the MVVM pattern, communicating with the API over HTTPS.

## 5. Communication overview

The main communication flow will be:

1. The user interacts with the desktop application.
2. The application sends requests to the API over HTTPS.
3. The API validates identity, permissions and incoming data.
4. The application layer executes the requested use case.
5. Infrastructure components perform the required PostgreSQL operations.
6. The API returns a response to the desktop application.

Credential encryption shall be implemented so that the server cannot access stored passwords in plaintext.

## 6. Information security

Security is a cross-cutting architectural requirement.

The following principles shall guide development:

- Encryption of stored credentials.
- HTTPS-protected communication.
- Multi-factor authentication (MFA).
- Role-based access control (RBAC).
- Separation between account authentication and vault unlocking.
- Secure cryptographic key management.
- Protection against unauthorized access.
- Logging of relevant security events.
- Minimization of stored personal data.

The planned cryptographic strategy considers AES-256-GCM, envelope encryption and individualized cryptographic key management for each user.

These mechanisms must still be specified, reviewed and tested before being considered implemented.

## 7. Data protection and compliance

The project shall consider:

- Brazilian General Data Protection Law (LGPD).
- The 1988 Brazilian Federal Constitution, particularly provisions concerning privacy and data protection.
- ISO 9001, as a quality management reference.
- ISO/IEC 27001, as an information security management reference.
- ISO/IEC 27701, as a privacy information management reference.

Compliance shall be demonstrated through implemented controls, documentation, testing and verifiable evidence.

## 8. Architectural principles

Development shall follow these principles:

1. Separation of concerns.
2. Low coupling between components.
3. High cohesion.
4. Encapsulation.
5. Domain independence from infrastructure.
6. Security by Design.
7. Privacy by Design.
8. Testability.
9. Maintainability.
10. Incremental evolution.

## 9. Evolution strategy

The architecture shall support:

- Addition of new functional modules.
- Expansion of authentication mechanisms.
- Evolution of authorization policies.
- Integration with external providers.
- Infrastructure expansion.
- Compatibility with additional operating systems.
- Evolution of auditing and monitoring mechanisms.

## 10. Current status

The initial solution structure has been created and successfully compiled.

All seven projects are included in the .NET solution, and the first commit has been published to GitHub.

However, the functional architecture, integrations, security mechanisms and persistence are still in the specification and development stages.

---

**Document identification:** AM-ARC-001
**Version:** 1.0
**Status:** Initial architectural proposal