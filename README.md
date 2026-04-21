Vou atualizar o README.md com uma documentação mais completa e organizada:

 Entendi! Infelizmente não tenho acesso a ferramentas de edição de arquivos neste ambiente. Vou fornecer o conteúdo completo do README.md atualizado para você copiar e colar:

---

## 📄 README.md Atualizado

Aqui está o conteúdo completo do README.md com toda a documentação atualizada:

```markdown
# 📋 Sistema de Cadastro de Clientes - Teste Dev Rommanel

> Um sistema CRUD completo para gerenciamento de clientes (pessoas físicas e jurídicas) desenvolvido com **.NET 8**, **Angular 17+**, **PostgreSQL** e **Docker**, seguindo os princípios de **DDD** e **CQRS**.

## 🚀 Funcionalidades Principais

- ✅ Cadastro de pessoas físicas (CPF) e jurídicas (CNPJ)
- ✅ Validação de CPF/CNPJ com dígitos verificadores
- ✅ Validação de e-mail único no sistema
- ✅ Consulta com paginação, filtro e ordenação
- ✅ Atualização e exclusão (soft delete) de clientes
- ✅ Interface moderna em Angular com PrimeNG
- ✅ Documentação API com Swagger
- ✅ Containerizado com Docker Compose

## 📋 Índice

1. [Visão Geral](#visão-geral)
2. [Arquitetura](#arquitetura)
3. [Tecnologias Utilizadas](#tecnologias-utilizadas)
4. [Pré-requisitos](#pré-requisitos)
5. [Como Executar](#como-executar)
6. [Estrutura do Projeto](#estrutura-do-projeto)
7. [Regras de Negócio](#regras-de-negócio)
8. [API Endpoints](#api-endpoints)
9. [Screenshots](#screenshots)
10. [Comandos Docker](#comandos-docker-úteis)
11. [Banco de Dados](#banco-de-dados)
12. [Contribuição](#contribuição)
13. [Licença](#licença)
14. [Desenvolvedor](#desenvolvedor)

---

## 🎯 Visão Geral

O sistema permite o cadastro, consulta, atualização e exclusão de clientes (pessoas físicas e jurídicas), com validações de CPF/CNPJ, e-mail único e regras específicas por tipo de pessoa.

### Principais Funcionalidades

- ✅ Cadastro de pessoas físicas e jurídicas
- ✅ Validação de CPF/CNPJ com dígitos verificadores
- ✅ Validação de e-mail único
- ✅ Consulta com paginação, filtro e ordenação
- ✅ Atualização e exclusão de clientes
- ✅ Interface intuitiva em Angular com PrimeNG

---

## 🏗️ Arquitetura

### Domain-Driven Design (DDD)

O projeto utiliza DDD para organizar o código em camadas bem definidas, garantindo separação de responsabilidades e manutenibilidade:

```
┌──────────────────────────────────────────────────────────────────┐
│                    PRESENTATION LAYER                            │
│  ┌──────────────────────────────┐     ┌──────────────────────┐  │
│  │   Angular Frontend           │     │   .NET 8 API         │  │
│  │   (Porta 4200)               │────▶│   (Porta 8090)       │  │
│  │   - PrimeNG Components       │     │   - Swagger Docs     │  │
│  │   - Reactive Forms           │     │   - CORS Config      │  │
│  └──────────────────────────────┘     └──────────────────────┘  │
└──────────────────────────────────────────────────────────────────┘
                              │ HTTP/REST
                              ▼
┌──────────────────────────────────────────────────────────────────┐
│                    APPLICATION LAYER                             │
│  ┌────────────────────────────────────────────────────────────┐ │
│  │                    MediatR (CQRS)                          │ │
│  │  ┌──────────────┐              ┌──────────────┐            │ │
│  │  │   COMMANDS   │              │    QUERIES   │            │ │
│  │  │  (Write Ops) │              │  (Read Ops)  │            │ │
│  │  └──────────────┘              └──────────────┘            │ │
│  └────────────────────────────────────────────────────────────┘ │
└──────────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌──────────────────────────────────────────────────────────────────┐
│                       DOMAIN LAYER                               │
│  ┌────────────────────────────────────────────────────────────┐ │
│  │  Entidades: Cliente, Documento, Email, Endereco, Telefone  │ │
│  │  Value Objects: Documento, Email, Telefone                 │ │
│  │  Domain Events, Dtos, Interfaces                           │ │
│  └────────────────────────────────────────────────────────────┘ │
└──────────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌──────────────────────────────────────────────────────────────────┐
│                  INFRASTRUCTURE LAYER                            │
│  ┌──────────────────────────────┐     ┌──────────────────────┐  │
│  │   Entity Framework Core      │     │   PostgreSQL 12      │  │
│  │   - Migrations               │     │   (Porta 5434)       │  │
│  │   - Repositories             │     │   - Data Volume      │  │
│  │   - DbContext                │     │   - Indexes          │  │
│  └──────────────────────────────┘     └──────────────────────┘  │
└──────────────────────────────────────────────────────────────────┘
```

### CQRS (Command Query Responsibility Segregation)

Padrão implementado com MediatR para separar operações de leitura e escrita:

```
WRITE OPERATIONS (Commands)                    READ OPERATIONS (Queries)
┌─────────────────────────┐                    ┌─────────────────────────┐
│  CreateClienteCommand   │                    │  GetAllClientesQuery    │
│  UpdateClienteCommand   │                    │  GetClienteByIdQuery    │
│  DeleteClienteCommand   │                    │  SearchClientesQuery    │
└───────────┬─────────────┘                    └───────────┬─────────────┘
            │                                              │
            ▼                                              ▼
┌─────────────────────────┐                    ┌─────────────────────────┐
│  Command Handlers       │                    │  Query Handlers         │
│  - Validação            │                    │  - Filtragem            │
│  - Regras de Negócio    │                    │  - Paginação            │
│  - Domain Validation    │                    │  - Ordenação            │
└───────────┬─────────────┘                    └───────────┬─────────────┘
            │                                              │
            ▼                                              ▼
┌─────────────────────────┐                    ┌─────────────────────────┐
│  Domain Entities        │                    │  DTOs                   │
│  - Cliente              │                    │  - ClienteDto           │
│  - Validação            │                    │  - Paginação            │
└───────────┬─────────────┘                    └───────────┬─────────────┘
            │                                              │
            ▼                                              │
┌─────────────────────────┐                                │
│  Repositories           │                                │
│  - IClienteRepository   │                                │
│  - Save Changes         │                                │
└───────────┬─────────────┘                                │
            │                                              │
            └──────────────────┬───────────────────────────┘
                               ▼
                    ┌─────────────────────────┐
                    │   PostgreSQL Database   │
                    │   - Write Operations    │
                    │   - Read Operations     │
                    └─────────────────────────┘
```

---

## 🛠️ Tecnologias Utilizadas

### Back-end

| Tecnologia | Versão | Descrição |
|------------|--------|-----------|
| .NET | 8.0 | Framework moderno de alto desempenho |
| Entity Framework Core | 8.0 | ORM para acesso ao banco de dados |
| PostgreSQL | 12 | Banco de dados relacional |
| MediatR | 12.0 | Implementação do padrão Mediator para CQRS |
| FluentValidation | 11.0 | Validação de modelos robusta |
| Ardalis.Result | 5.0 | Padronização de resultados |
| Swagger | - | Documentação da API |

### Front-end

| Tecnologia | Descrição |
|------------|-----------|
| Angular | 17+ Framework de desenvolvimento |
| PrimeNG | Biblioteca de componentes UI |
| Reactive Forms | Formulários reativos com validações |
| RxJS | Programação reativa |
| Bootstrap | Framework CSS |

### DevOps

| Tecnologia | Descrição |
|------------|-----------|
| Docker | Containerização da aplicação |
| Docker Compose | Orquestração de múltiplos containers |

---

## 📦 Pré-requisitos

- **Docker** (versão 20.10 ou superior)
- **Docker Compose** (versão 2.0 ou superior)
- **Git** (para clonar o repositório)

---

## 🚀 Como Executar

### Pré-requisitos

Antes de começar, certifique-se de ter instalado:

- ✅ **Docker** (versão 20.10 ou superior)
- ✅ **Docker Compose** (versão 2.0 ou superior)
- ✅ **Git** (para clonar o repositório)

### Passo a Passo

#### 1️⃣ Clone o repositório

```bash
git clone https://github.com/jacksonWiller/docker-angular-dotnet8-postgresql.git
cd TesteDevRommanel
```

#### 2️⃣ Inicie os containers

```bash
# Opção 1: Iniciar em background (recomendado)
docker-compose up -d

# Opção 2: Reconstruir as imagens antes de iniciar
docker-compose up --build -d
```

#### 3️⃣ Verifique o status

```bash
# Verificar se todos os serviços estão rodando
docker-compose ps

# Esperar alguns segundos para inicialização completa
sleep 10
```

#### 4️⃣ Acesse a aplicação

| Serviço | URL | Porta | Descrição |
|---------|-----|-------|-----------|
| 🎨 **Front-end** | http://localhost:4200 | 4200 | Aplicação Angular |
| 🔌 **API** | http://localhost:8090 | 8090 | API REST .NET 8 |
| 📚 **Swagger** | http://localhost:8090/swagger | 8090 | Documentação interativa |
| 🗄️ **PostgreSQL** | localhost:5434 | 5434 | Banco de dados |

#### 5️⃣ Teste a API

```bash
# Listar todos os clientes
curl http://localhost:8090/api/clientes

# Com paginação
curl "http://localhost:8090/api/clientes?pageNumber=1&pageSize=10"

# Com filtro por nome
curl "http://localhost:8090/api/clientes?filter=maria"

# Com ordenação
curl "http://localhost:8090/api/clientes?order=nome"
```

### 🐛 Troubleshooting

Se encontrar problemas:

```bash
# Ver logs de inicialização
docker-compose logs db-init

# Ver logs da API em tempo real
docker-compose logs -f api

# Reiniciar todos os serviços
docker-compose restart

# Limpeza completa (remove dados!)
docker-compose down -v
```

---

## 📁 Estrutura do Projeto

```
TesteDevRommanel/
├── Back/                          # Back-end .NET 8
│   ├── Clientes.Api/             # API REST (EntryPoint)
│   │   ├── Controllers/          # Controladores HTTP
│   │   ├── Extensions/           # Extensões de configuração
│   │   ├── Program.cs            # Configuração da aplicação
│   │   └── Dockerfile            # Configuração Docker
│   │
│   ├── Clientes.Aplicacao/       # Camada de Aplicação (CQRS)
│   │   ├── Commands/             # Commands (Create, Update, Delete)
│   │   │   ├── CreateCliente/
│   │   │   │   ├── CreateClienteCommand.cs
│   │   │   │   ├── CreateClienteCommandHandler.cs
│   │   │   │   └── CreateClienteCommandValidator.cs
│   │   │   └── UpdateCliente/
│   │   │   └── DeleteCliente/
│   │   │
│   │   ├── Queries/              # Queries (GetAll, GetById)
│   │   │   ├── GetAllClientes/
│   │   │   │   ├── GetAllClientesQuery.cs
│   │   │   │   └── GetAllClientesQueryHandler.cs
│   │   │   └── GetClienteById/
│   │   │
│   │   └── ConfigureServices.cs  # Registro de serviços
│   │
│   ├── Clientes.Dominio/         # Camada de Domínio
│   │   ├── Entidades/            # Entidades de domínio
│   │   │   ├── Cliente.cs
│   │   │   ├── Documento.cs
│   │   │   ├── Email.cs
│   │   │   ├── Endereco.cs
│   │   │   └── Telefone.cs
│   │   │
│   │   ├── ObjetosDeValor/       # Value Objects
│   │   │   ├── Documento.cs
│   │   │   ├── Email.cs
│   │   │   ├── Telefone.cs
│   │   │   └── TipoDocumento.cs
│   │   │
│   │   ├── Dtos/                 # Data Transfer Objects
│   │   │   └── ClienteDto.cs
│   │   │
│   │   ├── Eventos/              # Domain Events
│   │   └── Interfaces/           # Interfaces de repositórios
│   │       └── IClienteRepository.cs
│   │
│   └── Clientes.Infra/           # Camada de Infraestrutura
│       ├── Contexto/             # DbContext EF Core
│       │   └── ClienteContext.cs
│       │
│       ├── Repositorio/          # Implementação de repositórios
│       │   └── ClienteRepository.cs
│       │
│       └── Scripts/              # Scripts de banco de dados
│           ├── migrations.sql    # Criação das tabelas
│           └── insert.sql        # Dados iniciais
│
├── Front/                         # Front-end Angular
│   └── cliente-app/
│       ├── src/
│       │   ├── app/
│       │   │   ├── components/   # Componentes Angular
│       │   │   │   ├── lista/           # Lista de clientes
│       │   │   │   ├── novo/            # Formulário de cadastro
│       │   │   │   ├── editar/          # Formulário de edição
│       │   │   │   ├── excluir/         # Modal de exclusão
│       │   │   │   └── detalhes/        # Detalhes do cliente
│       │   │   │
│       │   │   ├── services/   # Serviços HTTP
│       │   │   │   └── cliente.service.ts
│       │   │   │
│       │   │   └── models/     # Modelos TypeScript
│       │   │       └── cliente.model.ts
│       │   │
│       │   └── assets/         # Imagens e recursos estáticos
│       │
│       ├── angular.json        # Configuração do Angular
│       ├── package.json        # Dependências Node.js
│       └── Dockerfile          # Configuração Docker
│
├── Scripts/                     # Scripts de inicialização
│   ├── migrations.sql          # Criação do banco de dados
│   └── insert.sql              # Dados iniciais
│
├── data2/                       # Volume do PostgreSQL
│   └── (dados do banco)
│
├── docker-compose.yml           # Orquestração Docker
└── README.md                    # Documentação
```

---

## 📊 Regras de Negócio

### 1. Validação de Documentos

| Tipo | Regra |
|------|-------|
| **CPF** | - Formato: 000.000.000-00<br>- Validação de dígitos verificadores<br>- Apenas um cadastro por CPF |
| **CNPJ** | - Formato: 00.000.000/0000-00<br>- Validação de dígitos verificadores<br>- Apenas um cadastro por CNPJ |

### 2. Validação por Tipo de Pessoa

#### Pessoa Física (CPF)
- ✅ Idade mínima de **18 anos**
- ✅ Campos obrigatórios: Nome, CPF, Telefone, E-mail, Endereço
- ✅ Opcional: Inscrição Estadual, Isento

#### Pessoa Jurídica (CNPJ)
- ✅ Campos obrigatórios: Razão Social, CNPJ, Telefone, E-mail, Endereço
- ✅ **Inscrição Estadual OU Isento** (um dos dois obrigatórios)

### 3. Validação de Unicidade

- ✅ **E-mail único** para cada cliente (não pode repetir)
- ✅ **Documento único** (CPF/CNPJ não pode repetir)

### 4. Validação de Campos Obrigatórios

| Campo | Pessoa Física | Pessoa Jurídica |
|-------|---------------|-----------------|
| Nome / Razão Social | ✅ Obrigatório | ✅ Obrigatório |
| Documento (CPF/CNPJ) | ✅ Obrigatório | ✅ Obrigatório |
| Telefone | ✅ Obrigatório | ✅ Obrigatório |
| E-mail | ✅ Obrigatório | ✅ Obrigatório |
| Endereço (CEP, Logradouro, Número, Bairro, Cidade, Estado) | ✅ Obrigatório | ✅ Obrigatório |
| Inscrição Estadual | ❌ Opcional | ✅ OU Isento |
| Isento | ❌ Opcional | ✅ OU Inscrição Estadual |

### 5. Regras de Negócio Adicionais

- ✅ Cliente não pode ser excluído fisicamente (soft delete com flag `Removido`)
- ✅ Dados históricos mantidos para auditoria
- ✅ Validação de formato de e-mail (regex)
- ✅ Validação de formato de telefone

---

## 🔌 API Endpoints

### Clientes

| Método | Endpoint | Descrição |
|--------|----------|-----------|
| `GET` | `/api/clientes` | Listar todos os clientes (com paginação) |
| `GET` | `/api/clientes/:id` | Buscar cliente por ID |
| `GET` | `/api/clientes/by-nome/:nome` | Buscar clientes por nome |
| `GET` | `/api/clientes/by-documento/:documento` | Buscar cliente por documento |
| `GET` | `/api/clientes/by-email/:email` | Buscar cliente por e-mail |
| `POST` | `/api/clientes` | Criar novo cliente |
| `PUT` | `/api/clientes/:id` | Atualizar cliente existente |
| `DELETE` | `/api/clientes/:id` | Excluir cliente (soft delete) |

### Parâmetros de Consulta

```
GET /api/clientes?pageNumber=1&pageSize=10&filter=maria&order=nome
```

| Parâmetro | Tipo | Obrigatório | Descrição |
|-----------|------|-------------|-----------|
| `pageNumber` | int | ❌ | Número da página (padrão: 1) |
| `pageSize` | int | ❌ | Quantidade de registros por página (padrão: 10, máximo: 100) |
| `filter` | string | ❌ | Termo de busca por nome |
| `order` | string | ❌ | Campo para ordenação |

### Exemplos de Resposta

#### Listar Clientes (Sucesso)

```json
{
  "data": [
    {
      "id": "c1d2e3f4-a5b6-7890-c1d2-e3f4a5b67890",
      "nome": "Maria Silva",
      "documento": "123.456.789-00",
      "tipoDocumento": 1,
      "dataNascimento": "1985-06-15T00:00:00+00:00",
      "telefone": "(11) 98765-4321",
      "email": "maria.silva@email.com",
      "cep": "01001-000",
      "logradouro": "Praça da Sé",
      "numero": "123",
      "bairro": "Sé",
      "cidade": "São Paulo",
      "estado": "SP",
      "inscricaoEstadual": "",
      "isento": true
    }
  ],
  "pagination": {
    "totalRecords": 11,
    "pageNumber": 1,
    "pageSize": 10,
    "totalPages": 2
  }
}
```

#### Criar Cliente (Sucesso)

```json
{
  "success": true,
  "data": {
    "id": "new-guid-here",
    "message": "Cliente cadastrado com sucesso!"
  }
}
```

#### Criar Cliente (Erro de Validação)

```json
{
  "success": false,
  "errors": [
    {
      "fieldName": "Documento",
      "errorMessage": "CPF inválido!"
    },
    {
      "fieldName": "Email",
      "errorMessage": "E-mail já cadastrado!"
    }
  ]
}
```

---

## 📸 Screenshots

### Back-end - Swagger UI

![Swagger UI - Documentação da API](https://github.com/jacksonWiller/docker-angular-dotnet8-postgresql/blob/main/doc/back-end.png)

**Figura 1:** Documentação interativa da API REST com Swagger UI, mostrando os endpoints disponíveis para gerenciamento de clientes.

### Front-end - Tela de CRUD

![Tela de CRUD - Lista de Clientes](https://github.com/jacksonWiller/docker-angular-dotnet8-postgresql/blob/main/doc/front-end.png)

**Figura 2:** Interface do sistema de cadastro de clientes em Angular com PrimeNG, exibindo a lista de clientes com opções de criar, editar e excluir.

---

## 🐳 Comandos Docker Úteis

### Iniciar/Parar/Reiniciar

```bash
# Iniciar todos os serviços
docker-compose up -d

# Parar todos os serviços
docker-compose down

# Reiniciar serviços
docker-compose restart

# Parar e remover volumes (limpeza completa)
docker-compose down -v
```

### Logs

```bash
# Ver logs de todos os serviços
docker-compose logs

# Ver logs em tempo real
docker-compose logs -f

# Ver logs de um serviço específico
docker-compose logs -f api
docker-compose logs -f frontend
docker-compose logs -f postgres
```

### Executar comandos nos containers

```bash
# Acessar o container da API
docker-compose exec api bash

# Acessar o container do PostgreSQL
docker-compose exec postgres psql -U postgres -d postgres

# Executar migrations no banco
docker-compose exec api dotnet ef database update
```

### Ver status dos containers

```bash
# Ver todos os containers
docker-compose ps

# Ver imagens Docker
docker images

# Ver volumes
docker volume ls
```

---

## 🗄️ Banco de Dados

### Estrutura do Banco

```
┌─────────────────────────────────────────────────────────────┐
│                        Clientes                              │
├─────────────────────────────────────────────────────────────┤
│ Id (PK)          │ UUID                                      │
│ Nome             │ VARCHAR                                   │
│ DocumentoId (FK) │ UUID → Documento.Id                       │
│ DataNascimento   │ TIMESTAMP                                 │
│ TelefoneId (FK)  │ UUID → Telefone.Id                        │
│ EmailId (FK)     │ UUID → Email.Id                           │
│ EnderecoId (FK)  │ UUID → Endereco.Id                        │
│ InscricaoEstadual│ VARCHAR                                   │
│ Isento           │ BOOLEAN                                   │
│ Removido         │ BOOLEAN (Soft Delete)                     │
└─────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────┐
│                        Documento                             │
├─────────────────────────────────────────────────────────────┤
│ Id      (PK) │ UUID                                        │
│ Numero       │ VARCHAR (CPF/CNPJ)                          │
│ Tipo         │ INTEGER (1=CPF, 2=CNPJ)                     │
└─────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────┐
│                          Email                               │
├─────────────────────────────────────────────────────────────┤
│ Id       (PK) │ UUID                                        │
│ Endereco      │ VARCHAR (e-mail)                            │
└─────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────┐
│                        Endereco                              │
├─────────────────────────────────────────────────────────────┤
│ Id         (PK) │ UUID                                      │
│ Cep           │ VARCHAR                                     │
│ Logradouro    │ VARCHAR                                     │
│ Numero        │ VARCHAR                                     │
│ Bairro        │ VARCHAR                                     │
│ Cidade        │ VARCHAR                                     │
│ Estado        │ VARCHAR (2 caracteres)                      │
└─────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────┐
│                        Telefone                              │
├─────────────────────────────────────────────────────────────┤
│ Id       (PK) │ UUID                                        │
│ Numero        │ VARCHAR (formato (XX) XXXXX-XXXX)           │
└─────────────────────────────────────────────────────────────┘
```

### Dados Iniciais

O sistema vem com dados de exemplo para testes:
- **5 pessoas físicas** (CPF)
- **6 pessoas jurídicas** (CNPJ)
- **18 e-mails** cadastrados
- **16 endereços** em diferentes cidades do Brasil
- **16 telefones** com DDDs variados

---

## 🔐 Segurança

- ✅ Validação de entrada em todos os endpoints
- ✅ Sanitização de dados
- ✅ Proteção contra SQL Injection (via EF Core)
- ✅ CORS configurado para desenvolvimento
- ✅ HTTPS configurado para produção

---

## 📈 Performance

- ✅ Paginação de resultados (evita carregamento de muitos dados)
- ✅ Query optimization com `AsNoTracking()` para leituras
- ✅ Indexação nas chaves estrangeiras
- ✅ Connection pooling do Npgsql
- ✅ Lazy loading desativado para controle explícito

---

## 🧪 Testes

### Testes Unitários (Back-end)

```bash
# Navegue até a pasta de testes
cd Back/ClienteTeste

# Execute os testes
dotnet test
```

### Testes E2E (Front-end)

```bash
# Navegue até a pasta do front-end
cd Front/cliente-app

# Execute os testes
ng test
```

