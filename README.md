# Morena Eventos

## Indice

- [1. Visão Geral]()
- [2. Arquitetura da Aplicação](#2-arquitetura-de-aplicação)
  - [2.1 Frontend (React com Next.js)]()
  - [2.2 Backend (Laravel)]()
  - [2.3 Banco de Dados (PostgreSQL)]()
- [3. Tecnologias Utilizadas](#3-tecnologias-utilizadas)
- [4. Fluxo de Trabalho](#4-fluxo-de-trabalho)
- [6. Configuração do Ambiente](#6-configuração-do-ambiente)

## 1. Visão Geral

"Morena Evento" é uma aplicação desenvolvida para gerenciar eventos, fornecendo uma interface para usuários criarem, visualizarem e gerenciarem eventos, com um backend em Laravel e no frontend React.js - Next.js.

## 2. Arquitetura de Aplicação

### 2.1 Frontend (React com Next.js)

- Descrição do uso de Next.js como framework frontend para renderização SSR e SSG.

### 2.2 Backend (Laravel)

- Descrição do uso de Laravel para a criação da API backend.

### 2.3 Banco de Dados (PostgreSQL)

- Explicação sobre o uso do PostgreSQL para armazenar dados de eventos, usuários, etc.

## 3. Tecnologias Utilizadas

- Listagem das principais tecnologias:

  - [React com Next.js para o frontend.]()
  - [Laravel para o backend.]()
  - [PostgreSQL para o banco de dados.]()
  - [Node.js & Composer para execução de scripts e gerenciamento de dependências.]()
  - [Docker (opicional)]()

## 4. Fluxo de trabalho

### 1. Página Inicial (Listagem de Eventos):

- **Requisição (GET):** Ao carregar a página inicial, o frontend envia uma requisição ao backend para obter todos os eventos elegíveis.

  - **Endpoint:** `/api/events`
  - **Response:** O backend retorna uma lista de eventos que estão abertos para inscrição, incluindo detalhes como data, local, descrição, capacidade e se o usuário já está inscrito.

### 2. Autenticação do Usuário:

- O usuário precisa estar logado para realizar ações como se inscrever em eventos ou gerenciar os próprios eventos.

**Requisição (POST):** Autenticação via API, onde o usuário envia suas credenciais para o backend.

- **Endpoint:** `/api/auth/login`
- **Response:** O backend retorna um token de autenticação (JWT ou sessão) que será utilizado nas próximas requisições.

### 3. Inscrição em um Evento:

**Requisição (POST):** O usuário, após autenticado, pode se inscrever em um evento. O backend deve garantir que o usuário não está inscrito em dois eventos no mesmo período.

- **Endpoint:** `/api/event/singUp`

- **Verificação:** O backend verifica se o usuário está disponível (não inscrito em outro evento no mesmo horário) antes de concluir a inscrição.

- **Response:** Confirmação da inscrição ou erro se houver conflito.

### 4. Gerenciamento de Eventos (Criar, Editar, Excluir):

- Apenas usuários logados podem criar, editar ou excluir eventos, e só podem gerenciar eventos que eles mesmos criaram.

**a. Criar Evento:**

- **Requisição (POST):** O usuário pode criar um evento. O backend automaticamente inscreve o criador no evento.

  - **Endpoint:** `/api/event/creatingEvent`

  - **Request Body:** Informações do evento (nome, data, local, etc.)

  - **Response:** Evento criado com sucesso, incluindo o ID e a inscrição automática do criador.

**b. Editar Evento:**

- **Requisição (PUT):** O usuário pode editar apenas eventos que ele criou.

  - **Endpoint:** `/api/events/edit/{event_id}`

  - **Verificação:** O backend verifica se o evento foi criado pelo usuário autenticado.

  - **Response:** Atualização confirmada ou erro se o usuário não for o dono do evento.

**c. Excluir Evento:**

- **Requisição (DELETE):** O usuário pode excluir apenas eventos que ele criou.

  - **Endpoint:** `/api/events/remove`
  - **Verificação:** O backend verifica se o evento foi criado pelo usuário autenticado.
  - **Response:** Confirmação de exclusão ou erro se o usuário não for o dono do evento.

## 5. Configuração do Ambiente

- Ferramentas Necessárias:
  - [Node.js](https://nodejs.org/pt)
  - [PHP](https://www.php.net/downloads.php)
  - [Postgres](https://www.postgresql.org/download/)
  - [Composer](https://getcomposer.org/download/)
  - [docker (opicional)](https://docs.docker.com/get-started/get-docker/)

**Passos de Configuração:**

- **Clone do Repositório:**

  https://github.com/phsilvadev/Desafio-Studio-Mozak-Morena-Eventos

- **Configuração de Arquivos:**

  - Criar ou modificar o arquivo `.env` com as variáveis necessárias para o ambiente Docker.

  - **Exemplo:**

    ```bash
    DB_CONNECTION=pgsql
    DB_HOST=127.0.0.1
    DB_PORT=5432
    DB_DATABASE=databases
    DB_USERNAME=morena
    DB_PASSWORD=morenaapi
    ```

    **4. Configurando o Banco de Dados com Docker Compose:**

    Dentro da pasta do backend, crie um arquivo chamado `docker-compose.yml` com o seguinte conteúdo para configurar o banco de dados:

    ```bash
    services:
      db:
        image: postgres:13
        container_name: postgres_db
        environment:
          POSTGRES_USER: postgres
          POSTGRES_PASSWORD: password
          POSTGRES_DB: mydb
        ports:
          - '5498:5432'
        volumes:
          - postgres_data:/var/lib/postgresql/data
        networks:
          - default
        restart: always

    volumes:
      postgres_data:
    ```

**5. Iniciar o Servidor:**

- Dentro da pasta do backend, digite `docker-compose up -d` para iniciar o container do banco de dados.

- Depois, use o comando `php artisan serve` para iniciar o servidor Laravel.

**6. Iniciar o Frontend:**

- Dentro da pasta do frontend, execute `npm install` para baixar as dependências.

- Após isso, use `npm run build` para construir o projeto e `npm start` para iniciar o frontend.

**7. Migrar o Banco de Dados:**

- Em Linux e Windows, execute o comando:

  ```bash
  php artisan migrate
  ```

**8. Criar Seed no Banco de Dados:**

- Em Linux e Windows, execute o comando:

  ```bash
  php artisan db:seed
  ```

**9. Definir a Chave Secreta no Arquivo .env:**

- Adicione a seguinte linha ao seu arquivo `.env`, substituindo `your-secret-key` por uma chave secreta segura:

  ```bash
  JWT_SECRET=your-secret-key
  ```

- **Gere uma nova chave secreta**

  Se você ainda não tem uma chave secreta ou deseja gerar uma nova, você pode usar o comando Artisan para gerá-la automaticamente. Execute o seguinte comando:

  - Em Linux e Windows, execute o comando:
    ```bash
    php artisan jwt:secret
    ```

  **7. Limpe o cache de configuração**

  - Em Linux e Windows, execute o comando:
    ```bash
    php artisan config:cache
    ```

  **8. Reinicar o servidor caso necessario**

  - Em Linux e Windows, execute o comando:
    ```bash
    docker compose restart
    ```

6. **Acessar a Aplicação:**

- A aplicação será acessível em `http://localhost:3000` (React com Next.js) e ` http://localhost:8000` (Laravel API).
