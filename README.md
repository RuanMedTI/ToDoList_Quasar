# To-Do List App

Este projeto é uma aplicação de lista de tarefas (“To-Do List”) desenvolvida com um back-end em Laravel e um front-end em Quasar Framework (Vue.js). Ele permite criar, listar, atualizar e deletar tarefas, conectando o front-end à API criada no Laravel.

## Tecnologias Utilizadas

- **Back-End:** Laravel
- **Front-End:** Quasar Framework (Vue.js)
- **Banco de Dados:** MySQL
- **Requisições HTTP:** Axios

---

## Como Rodar o Projeto

### Back-End (Laravel)

1. **Clone o repositório e acesse o diretório do back-end:**
   ```bash
   git clone https://github.com/RuanMedTI/ToDoList_Quasar.git
   cd ToDoList_Quasar/backend
   ```

2. **Instale as dependências do Composer:**
   ```bash
   composer install
   ```

3. **Configure o arquivo `.env`:**
   - Copie o arquivo de exemplo:
     ```bash
     cp .env.example .env
     ```
   - Configure as credenciais do banco de dados no arquivo `.env`:
     ```env
     DB_CONNECTION=mysql
     DB_HOST=127.0.0.1
     DB_PORT=3306
     DB_DATABASE=todo_list_db
     DB_USERNAME=root
     DB_PASSWORD=
     ```

4. **Gere a chave da aplicação:**
   ```bash
   php artisan key:generate
   ```

5. **Execute as migrations para criar as tabelas:**
   ```bash
   php artisan migrate
   ```

6. **Inicie o servidor:**
   ```bash
   php artisan serve
   ```
   O back-end estará disponível em: `http://127.0.0.1:8000`

---

### Front-End (Quasar)

1. **Acesse o diretório do front-end:**
   ```bash
   cd ../frontend
   ```

2. **Instale as dependências do Node.js:**
   ```bash
   npm install
   ```

3. **Configure a URL da API, se necessário:**
   - Verifique se a URL da API no arquivo `src/pages/TodoPage.vue` está correta:
     ```javascript
     apiUrl: 'http://127.0.0.1:8000/api/tasks'
     ```

4. **Inicie o servidor de desenvolvimento:**
   ```bash
   quasar dev
   ```
   O front-end estará disponível em: `http://localhost:8080`

---

## Estrutura do Projeto

```plaintext
ToDoList_Quasar/
├── backend/   # Código do back-end em Laravel
├── frontend/  # Código do front-end em Quasar
```

---

## Modelo Entidade-Relacionamento (MER)

O projeto utiliza um banco de dados simples com as seguintes entidades principais:

- **Tasks (Tarefas):**
  - `id` (integer, chave primária)
  - `title` (string, título da tarefa)
  - `description` (text, descrição da tarefa)
  - `status` (boolean, indica se a tarefa está concluída ou não)
  - `created_at` (timestamp, data de criação)
  - `updated_at` (timestamp, data de última atualização)

---

## Funcionalidades

- Criar novas tarefas
- Listar todas as tarefas
- Atualizar tarefas existentes (marcar como concluídas)
- Deletar tarefas

---

## Demonstração em Vídeo

Confira o vídeo com testes da aplicação no YouTube:
[https://www.youtube.com/watch?v=8_8mMoWeX_k](https://www.youtube.com/watch?v=8_8mMoWeX_k)

---

## Autor

Desenvolvido por **Ruan Medeiros**. 
