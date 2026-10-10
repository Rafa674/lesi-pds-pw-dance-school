# Viva D'Arte

Plataforma web de apoio à gestão de uma escola de dança. Permite aos alunos e encarregados de educação consultar a agenda e pedir aulas de coaching, aos professores gerir disponibilidades e pedidos, e à administração acompanhar a actividade da escola.

## Funcionalidades

- Registo, início de sessão e recuperação de palavra-passe.
- Gestão de utilizadores com autenticação por token JWT e permissões por perfil.
- Consulta e gestão de horários, anos lectivos, modalidades, salas e professores.
- Pedidos e gestão de aulas de coaching, incluindo disponibilidades dos professores.
- Consulta de agenda, gestão de figurinos/inventário, facturação e auditoria.
- Painel de administração.

## Tecnologias

- **Frontend:** React, React Router e Vite.
- **Backend:** Node.js, Express e JavaScript.
- **Base de dados:** Microsoft SQL Server, acedido através do pacote `mssql`.
- **Testes do backend:** Vitest e Supertest.

## Estrutura do projecto

- `frontend/` — aplicação web React.
- `backend/` — API REST, autenticação e lógica de negócio.
- `backend/sql/dancaF.sql` — script SQL para criar a estrutura da base de dados.

## Pré-requisitos

- Node.js e npm.
- Uma instância de Microsoft SQL Server acessível.
- Credenciais de acesso à base de dados e, se a validação CAPTCHA estiver activa, a chave secreta do reCAPTCHA.

## Configuração

### 1. Base de dados

Crie uma base de dados no SQL Server e execute nela o script `backend/sql/dancaF.sql` para criar as tabelas. Confirme que a instância e a base de dados estão acessíveis a partir do computador onde vai correr o backend.

### 2. Backend

Crie o ficheiro `backend/.env` com as configurações seguintes:

```env
PORT=3000
DB_SERVER=localhost
# Opcional, se a instância usar uma porta TCP explícita:
# DB_PORT=1433
DB_NAME=nome_da_base_de_dados
DB_USER=utilizador
DB_PASSWORD=palavra-passe
JWT_SECRET=uma_chave_secreta_forte
# Opcional, necessário para as operações que validam reCAPTCHA:
RECAPTCHA_SECRET_KEY=chave_secreta_recaptcha
```

`DB_SERVER` também pode indicar uma instância nomeada do SQL Server, por exemplo `localhost\SQLEXPRESS`. Se definir `DB_PORT`, a ligação usa essa porta em vez do nome da instância.

Instale as dependências e inicie o servidor:

```bash
cd backend
npm install
npm run dev
```

O backend arranca em `http://localhost:3000`. A API só inicia depois de estabelecer ligação à base de dados. O endpoint `GET /api/health` permite verificar o estado do servidor e da ligação à base de dados.

Para iniciar sem o modo de desenvolvimento, use `npm start`.

### 3. Frontend

Numa segunda janela de terminal:

```bash
cd frontend
npm install
npm run dev
```

O Vite apresenta no terminal o endereço local da aplicação. Por omissão, a API utilizada pelo frontend é `https://api-vivadarte.onrender.com`. Para apontar para o backend local, crie `frontend/.env.local` com:

```env
VITE_API_URL=http://localhost:3000
```

Reinicie o servidor Vite depois de alterar variáveis de ambiente.

## Comandos úteis

Executar a suite de testes do backend:

```bash
cd backend
npm test
```

Executar os testes uma vez, sem modo de observação:

```bash
npm run test:run
```

Criar a versão de produção do frontend:

```bash
cd frontend
npm run build
```

Pré-visualizar localmente a versão compilada:

```bash
npm run preview
```

Gerar a documentação JSDoc dos controladores, modelos e rotas do backend:

```bash
cd backend
npm run docs
```

## API

A API está organizada sob o prefixo `/api` e inclui os módulos seguintes:

- `/api/auth` — autenticação e gestão da conta.
- `/api/escola` — estrutura e horários da escola.
- `/api/coaching` — pedidos, aulas e disponibilidades de coaching.
- `/api/agenda` — agenda.
- `/api/inventario` — inventário e figurinos.
- `/api/auditoria` — registos de auditoria.
- `/api` — operações de facturação.

A maioria das rotas exige autenticação; algumas operações estão limitadas a perfis específicos. Consulte os ficheiros em `backend/src/routes/` para conhecer os endpoints e permissões disponíveis.
