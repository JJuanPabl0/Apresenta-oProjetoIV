# 📌 Apresentação – CRUD de Cards e Tasks (Backend)

##  Objetivo

Board criado para mostrar a implementação do **CRUD do backend de Cards e Tasks** do sistema **PentaChaos – Sistema de Gestão de Estágios**.

O foco desta entrega foi:

* Estruturação de **endpoints REST**
* Aplicação de **regras de negócio por perfil de usuário (ADMIN e USER)**
* Controle de acesso com **Spring Security**
* Organização do projeto em **arquitetura em camadas**

---

## 🏗️ Estrutura do Projeto

Arquitetura bem definida, separando responsabilidades para facilitar a manutenção e evolução do sistema.

```
backend/sge-app
 ├── controller   # Camada REST (entrada da aplicação)
 ├── services     # Regras de negócio
 ├── repository   # Acesso a dados (JPA)
 ├── models       # Entidades do sistema
 ├── security     # Autenticação e autorização
 ├── resources    # Configurações e data.sql
```

---

## 🧩 Conceitos: Card vs Task

### 🃏 Card

* Representa uma **visualização simples** da tarefa
* Campos principais:

  * título
  * descrição
  * data de criação

### ✅ Task

* Representa a **tarefa completa do sistema**
* Campos:

  * título e descrição
  * status (PENDENTE, EM_ANDAMENTO, CONCLUIDA)
  * prioridade (ALTA, MEDIA, BAIXA, URGENTE)
  * responsável (Estagiário)
  * criado por (Coordenador)
* Possui **controle de acesso por perfil**

---

## 🎮 Controllers

### 📌 CardController

Responsável por expor os endpoints de cards.

**Endpoints:**

* `POST /api/v1/cards`

  * Acesso: **ADMIN**
  * Cria um novo card

* `GET /api/v1/cards`

  * Acesso: **ADMIN e USER**
  * Lista todos os cards

O controle de acesso é feito diretamente no controller com `@PreAuthorize`.

---

### 📌 TaskController

Responsável pelo gerenciamento completo das tarefas.

**Endpoints:**

* `GET /api/v1/tasks`

  * ADMIN: lista todas as tarefas
  * USER: lista apenas tarefas atribuídas a ele

* `GET /api/v1/tasks/{id}`

  * ADMIN: pode visualizar qualquer tarefa
  * USER: apenas tarefas atribuídas

* `POST /api/v1/tasks`

  * Acesso: **ADMIN**
  * Cria uma nova tarefa

* `PUT /api/v1/tasks/{id}`

  * Acesso: **ADMIN**
  * Atualiza uma tarefa existente

* `DELETE /api/v1/tasks/{id}`

  * Acesso: **ADMIN**
  * Remove uma tarefa

---

## 🧠 Services (Regras de Negócio)

### 🔧 CardService

* Camada simples
* Responsável por:

  * Criar cards
  * Listar cards

Como o Card não possui responsável, todos os usuários autenticados podem visualizar.

---

### 🔧 TaskService

Camada mais importante do sistema, onde estão concentradas as regras de negócio.

**Principais responsabilidades:**

* Validação de permissões por perfil
* Filtro de tarefas para usuários USER
* Validação de existência do responsável
* Definição automática de campos (status e data)

**Regras principais:**

* ADMIN:

  * Pode criar, editar, deletar e visualizar todas as tarefas
* USER:

  * Pode visualizar apenas tarefas atribuídas a ele

Exceções são lançadas quando regras de negócio ou permissões não são atendidas.

---

## 🗄️ Repository

Os repositórios utilizam **Spring Data JPA**, eliminando a necessidade de SQL manual.

Exemplos de consultas:

* `findByResponsavel_Id`
* `findByCriadoPor_Id`

Isso garante código limpo, legível e de fácil manutenção.

---

## 🧪 Carga Inicial de Dados (data.sql)

O arquivo `data.sql` é utilizado para subir o sistema já com dados de teste.

Ele inclui:

* Usuários ADMIN (Coordenadores)
* Usuários USER (Estagiários)
* Roles do sistema
* Tarefas com diferentes status e prioridades
* Registros de ponto (clock entries)

Essa abordagem facilita testes e demonstrações no Postman.

---

### 🃏 Cards

* Criar card (ADMIN)
* Listar cards (ADMIN / USER)

---

### ✅ Tasks (CRUD Completo)

**ADMIN:**

* Criar tarefa
* Listar todas as tarefas
* Atualizar tarefa
* Deletar tarefa

**USER:**

* Listar apenas suas tarefas
* Tentativas de criar, editar ou deletar retornam **403 Forbidden**

Essa validação demonstra que o controle de acesso está funcionando corretamente.

---

## 🚀 Conclusão

Com esta implementação, foi entregue um **CRUD completo de Cards e Tasks**, com:

* Arquitetura em camadas
* Controle de acesso por perfil
* Regras de negócio bem definidas
* Pronto para integração com frontend
