# Login API

Uma API REST simples de login desenvolvida em JavaScript com Express para estudos de teste de software.

## � Novidades

- Testes automatizados para a funcionalidade "Esqueci minha senha" (incluindo casos de e-mail inválido)
- Mudança da porta padrão para `3001` (antes era 3000)
- Integração com GitHub Actions: pipeline automatizado que executa os testes e publica o relatório no GitHub Pages

### 📊 Relatório de Testes Online
Acesse o relatório mais recente dos testes automatizados:

👉 [Relatório de Testes - GitHub Pages](https://marcelo-om30.github.io/testApi/)

## �🎯 Objetivo

Esta API foi criada especificamente para estudos de teste de software, incluindo cenários de:
- Login com sucesso
- Login inválido
- Bloqueio de conta após 3 tentativas falhadas
- Recuperação de senha ("Esqueci minha senha")

## 🚀 Funcionalidades

### ✅ Login
- Autenticação com username e senha
- Geração de token de acesso
- Controle de tentativas de login por username

### 🔒 Bloqueio de Conta
- Após 3 tentativas falhadas, a conta é bloqueada
- Status HTTP 423 (Locked) para contas bloqueadas
- Desbloqueio automático ao usar "Esqueci minha senha"

### 📧 Recuperação de Senha
- Endpoint que recebe email e busca o usuário correspondente
- Desbloqueia automaticamente a conta (reseta tentativas do username)
- Simulação de envio de email de recuperação

### 🧪 Endpoint para Testes
- Reset de tentativas de login para facilitar testes

## 📋 Pré-requisitos

- Node.js (versão 14 ou superior)
- npm ou yarn

## 🛠️ Instalação

1. Clone ou baixe o projeto
2. Instale as dependências:

```bash
npm install
```

## 🏃‍♂️ Como Executar


### Desenvolvimento (com auto-reload)
```bash
npm run dev
```

### Produção
```bash
npm start
```

O servidor estará disponível em: `http://localhost:3001`

## 📚 Documentação da API

### Swagger UI
Acesse a documentação interativa da API em:
```
http://localhost:3000/api-docs
```

### Endpoints Disponíveis

#### 1. Login
- **URL:** `POST /api/login`
- **Body:**
```json
{
  "username": "marcelo.salmeron",
  "password": "123456"
}
```

#### 2. Esqueci Minha Senha
- **URL:** `POST /api/forgot-password`
- **Body:**
```json
{
  "email": "usuario@teste.com"
}
```

#### 3. Reset de Tentativas (para testes)
- **URL:** `POST /api/reset-attempts`
- **Body:**
```json
{
  "email": "usuario@teste.com"
}
```

## 👥 Usuários de Teste

A API vem com três usuários pré-configurados:

| Username | Email | Senha | Nome |
|----------|-------|-------|------|
| `usuario` | `usuario@teste.com` | `senha123` | Usuário Teste |
| `admin` | `admin@teste.com` | `admin123` | Administrador |
| `marcelo.salmeron` | `marcelo.salmeron@teste.com` | `123456` | Marcelo Salmeron |

## 🔄 Fluxo de Funcionamento

### 🔑 Importante: Username vs Email
- **Login:** Usa `username` + `password`
- **Forgot Password:** Usa `email` (busca o usuário e reseta tentativas do `username`)
- **Reset Attempts:** Usa `email` (busca o usuário e reseta tentativas do `username`)
- **Controle de tentativas:** Armazenado por `username`

### Login Bem-sucedido
1. Envie username e senha válidos
2. Receba token de acesso
3. Contador de tentativas é resetado

### Login Falhado
1. Envie credenciais inválidas (username/senha)
2. Receba mensagem de erro
3. Contador de tentativas é incrementado por username
4. Após 3 tentativas, conta é bloqueada

### Conta Bloqueada
1. Tentativas de login retornam status 423
2. Use "Esqueci minha senha" com email para desbloquear
3. Conta é automaticamente desbloqueada

## 📊 Códigos de Status HTTP

| Código | Descrição |
|--------|-----------|
| 200 | Sucesso |
| 400 | Dados inválidos |
| 401 | Credenciais inválidas |
| 404 | Usuário não encontrado |
| 423 | Conta bloqueada |
| 500 | Erro interno do servidor |

## 🧪 Cenários de Teste


### Teste 1: Login Válido
```bash
curl -X POST http://localhost:3001/api/login \
  -H "Content-Type: application/json" \
  -d '{"username": "marcelo.salmeron", "password": "123456"}'
```

### Teste 2: Login Inválido
```bash
curl -X POST http://localhost:3001/api/login \
  -H "Content-Type: application/json" \
  -d '{"username": "usuario", "password": "senhaerrada"}'
```

### Teste 3: Bloqueio de Conta
Execute o teste 2 três vezes consecutivas para bloquear a conta.

### Teste 4: Esqueci Minha Senha (e-mail válido)
```bash
curl -X POST http://localhost:3001/api/forgot-password \
  -H "Content-Type: application/json" \
  -d '{"email": "usuario@teste.com"}'
```

### Teste 5: Esqueci Minha Senha (e-mail inválido)
```bash
curl -X POST http://localhost:3001/api/forgot-password \
  -H "Content-Type: application/json" \
  -d '{"email": "emailinvalido"}'
```

### Teste 6: Reset de Tentativas
```bash
curl -X POST http://localhost:3001/api/reset-attempts \
  -H "Content-Type: application/json" \
  -d '{"email": "usuario@teste.com"}'
```

## 🏗️ Estrutura do Projeto

```
testApi/
├── server.js              # Servidor principal + configuração Swagger
├── package.json           # Dependências e scripts
├── README.md             # Documentação
├── routes/
│   └── loginRoutes.js    # Rotas da API + documentação Swagger
├── controllers/
│   └── loginController.js # Lógica de negócio
└── test/
    └── login.test.js     # Testes automatizados
```

## 🔧 Tecnologias Utilizadas

<p align="left">
  <img src="https://img.shields.io/badge/Express.js-000?logo=express&logoColor=white" alt="Express.js"/>
  <img src="https://img.shields.io/badge/Swagger-85EA2D?logo=swagger&logoColor=black" alt="Swagger"/>
  <img src="https://img.shields.io/badge/JavaScript-F7DF1E?logo=javascript&logoColor=black" alt="JavaScript"/>
  <img src="https://img.shields.io/badge/CORS-00599C?logo=cloudflare&logoColor=white" alt="CORS"/>
  <img src="https://img.shields.io/badge/Helmet-233056?logo=helmet&logoColor=white" alt="Helmet"/>
  <img src="https://img.shields.io/badge/Rate%20Limit-6D6D6D?logo=clockify&logoColor=white" alt="Express Rate Limit"/>
</p>

- **Express.js** - Framework web
- **Swagger UI Express** / **Swagger JSDoc** - Documentação da API
- **CORS** - Cross-Origin Resource Sharing
- **Helmet** - Segurança HTTP
- **Express Rate Limit** - Limitação de taxa

## 📝 Notas Importantes

- **Dados em Memória:** Todos os dados são armazenados em memória (variáveis/constantes)
- **Sem Banco de Dados:** Não há persistência de dados
- **Fins Educacionais:** API criada especificamente para estudos de teste
- **Tokens Simples:** Tokens gerados são simulados (não são JWT reais)

## 🤝 Contribuição

Este projeto é para fins educacionais. Sinta-se à vontade para usar como base para seus estudos de teste de software.

## 📄 Licença

MIT License - veja o arquivo LICENSE para detalhes. 