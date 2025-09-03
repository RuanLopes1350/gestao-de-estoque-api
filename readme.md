# Sistema de Gestão de Estoque

API RESTful para gerenciamento de estoque desenvolvida com Node.js, Express e MongoDB.

Este é um projeto acadêmico construído com o objetivo de aprender a desenvolver uma api REST, conceitos de arquitetura de software com Controller, Service e Repository bem como utilização de autenticação por tokens.
 
## Tecnologias

- **Node.js** - Runtime JavaScript
- **Express.js** - Framework web
- **MongoDB** - Banco de dados NoSQL
- **Mongoose** - ODM para MongoDB
- **JWT** - Autenticação por tokens
- **Winston** - Sistema de logs
- **Jest** - Testes unitários e integração
- **Swagger** - Documentação da API
- **Zod** - Validação de schemas
- **Docker** - Containerização

## Funcionalidades

### Gerenciamento de Produtos
- Cadastro completo com informações detalhadas (nome, descrição, preço, custo, categoria)
- Controle de estoque atual e estoque mínimo
- Códigos únicos de identificação
- Associação com fornecedores
- Controle de status (ativo/inativo)

### Controle de Movimentações
- Registro de entradas e saídas de produtos
- Rastreamento de origem/destino das movimentações
- Histórico completo de movimentações
- Cancelamento de movimentações com reversão automática do estoque
- Auditoria de usuário responsável pela movimentação

### Gerenciamento de Fornecedores
- Cadastro completo com dados comerciais
- Controle de endereços múltiplos
- Associação com produtos fornecidos
- Histórico de relacionamento

### Sistema de Usuários e Permissões
- Três níveis de acesso: Administrador, Gerente e Estoquista
- Autenticação segura com JWT
- Controle de permissões por rota
- Sistema de auditoria de ações por usuário

### Sistema de Envio de Emails
- Integração com microserviço de envio de emails
- Notificações de primeiro acesso para novos usuários
- Sistema de recuperação de senha via email
- Templates responsivos usando MJML e Handlebars
- Autenticação por API Key para comunicação entre serviços
- Suporte a Gmail via OAuth2 ou App Password
- Sistema de fallback em caso de falha no envio

> **Nota**: O sistema se conecta a um microserviço externo de emails. Repositório do mailsender: [RuanLopes1350/mailsender](https://github.com/RuanLopes1350/mailsender)

## Instalação

### Pré-requisitos

- Node.js (versão 18 ou superior)
- MongoDB
- npm

### Configuração

1. Clone o repositório:
```bash
git clone https://github.com/RuanLopes1350/gestao-de-estoque---api.git
cd gestao-de-estoque
```

2. Instale as dependências:
```bash
npm install
```

3. Configure as variáveis de ambiente:
```bash
cp .env.example .env
```

Edite o arquivo `.env`:
```env
DB_URL=mongodb://localhost:27017/gerenciarEstoque
APP_PORT=5011
NODE_ENV=development
JWT_SECRET=seu_jwt_secret_aqui
JWT_REFRESH_SECRET=seu_jwt_refresh_secret_aqui
JWT_SECRET_REFRESH_TOKEN=seu_token_aqui
JWT_SECRET_PASSWORD_RECOVERY=seu_token_aqui
JWT_ACCESS_TOKEN_EXPIRATION="15m"
JWT_REFRESH_TOKEN_EXPIRATION="7d"

# Configurações do microserviço de email (opcional)
MAIL_API_URL=http://localhost:5013
MAIL_API_KEY=sua_api_key_do_mailsender
MAIL_API_TIMEOUT=5000
SYSTEM_URL=http://localhost:3000
```

4. Execute os seeds (opcional):
```bash
npm run seed
```

## Como executar

### Desenvolvimento
```bash
npm run dev
```

### Produção
```bash
npm start
```

### Com Docker
```bash
docker-compose up
```

A API estará disponível em `http://localhost:5011`

## Sistema de Envio de Emails

O sistema utiliza um microserviço externo para o envio de emails. As principais funcionalidades incluem:

### Funcionalidades de Email
- **Primeiro Acesso**: Envio de código de verificação para novos usuários
- **Recuperação de Senha**: Envio de código para redefinição de senha
- **Templates Responsivos**: Uso de MJML para emails com design profissional
- **Auditoria**: Log completo de todos os envios de email

### Como Funciona
1. A API principal faz requisições HTTP para o microserviço de email
2. Autenticação via API Key configurada nas variáveis de ambiente
3. O microserviço processa templates MJML com dados dinâmicos
4. Envio via Gmail usando OAuth2 ou App Password
5. Retorno de confirmação ou erro para a API principal

### Configuração do Microserviço
Se você quiser executar o microserviço de email:

1. Configure as variáveis de ambiente do mailsender
2. Execute o microserviço na porta 5013
3. Configure a `MAIL_API_KEY` no arquivo `.env` da API principal

> **Sem o microserviço**: O sistema funcionará normalmente, mas os emails não serão enviados. Os códigos aparecerão no console para facilitar o desenvolvimento.

## Documentação

A documentação da API está disponível via Swagger em:
`http://localhost:5011/api-docs`

## Autenticação

O sistema utiliza JWT para autenticação. Para acessar rotas protegidas, inclua o token no header:

```
Authorization: Bearer <seu-token-jwt>
```

### Perfis de usuário:
- **Administrador**: Acesso completo a todas as funcionalidades
- **Gerente**: Acesso a relatórios e funções de supervisão
- **Estoquista**: Operações diárias de estoque e movimentações

## Principais Endpoints

### Autenticação
- `POST /auth/login` - Login do usuário
- `POST /auth/logout` - Logout do usuário

### Produtos
- `GET /api/produtos` - Listar produtos
- `POST /api/produtos` - Criar produto
- `GET /api/produtos/{id}` - Buscar produto por ID
- `GET /api/produtos/busca` - Buscar produtos por nome/código
- `PUT /api/produtos/{id}` - Atualizar produto
- `DELETE /api/produtos/{id}` - Excluir produto
- `GET /api/produtos/estoque-baixo` - Produtos com estoque baixo

### Movimentações
- `GET /api/movimentacoes` - Listar movimentações
- `POST /api/movimentacoes` - Registrar movimentação
- `GET /api/movimentacoes/{id}` - Buscar movimentação
- `GET /api/movimentacoes/busca` - Buscar movimentações
- `PUT /api/movimentacoes/{id}` - Editar movimentação
- `DELETE /api/movimentacoes/{id}` - Remover movimentação

### Fornecedores
- `GET /api/fornecedores` - Listar fornecedores
- `POST /api/fornecedores` - Cadastrar fornecedor
- `GET /api/fornecedores/{id}` - Buscar fornecedor
- `GET /api/fornecedores/busca` - Buscar fornecedores
- `PUT /api/fornecedores/{id}` - Atualizar fornecedor
- `DELETE /api/fornecedores/{id}` - Remover fornecedor

### Usuários
- `GET /api/usuarios` - Listar usuários
- `POST /api/usuarios` - Cadastrar usuário
- `GET /api/usuarios/{id}` - Buscar usuário
- `GET /api/usuarios/busca` - Buscar usuários
- `PUT /api/usuarios/{id}` - Atualizar usuário
- `DELETE /api/usuarios/{id}` - Remover usuário

### Grupos de Permissões
- `GET /api/grupos` - Listar grupos
- `POST /api/grupos` - Criar grupo
- `PUT /api/grupos/{id}` - Atualizar grupo

### Logs
- `GET /api/logs` - Consultar logs do sistema

## Testes

```bash
# Executar todos os testes
npm test

# Testes unitários
npm run test:unit

# Testes de integração
npm run test:integration

# Testes em modo watch
npm run test:watch

# Testes com saída detalhada
npm run test:verbose
```

## Estrutura do Projeto

```
src/
├── config/          # Configurações (banco de dados)
├── controllers/     # Controladores da API
├── dto/            # Data Transfer Objects
├── middlewares/    # Middlewares (auth, logs, permissões)
├── models/         # Modelos do MongoDB
├── repositories/   # Camada de acesso a dados
├── routes/         # Definição das rotas
├── services/       # Lógica de negócio
├── utils/          # Utilitários e helpers
├── seeds/          # Scripts de seed do banco
└── test/           # Arquivos de teste
```

## Docker

Para executar com Docker:

```bash
# Construir e executar
docker-compose up --build

# Executar em background
docker-compose up -d
```

O ambiente Docker inclui:
- API Node.js na porta 5011
- MongoDB na porta 27017

## Scripts Disponíveis

- `npm run dev` - Executa em modo desenvolvimento com nodemon
- `npm start` - Executa em modo produção
- `npm run seed` - Executa seeds do banco de dados
- `npm run seed:permissions` - Executa seed de permissões
- `npm test` - Executa testes com cobertura
- `npm run test:watch` - Executa testes em modo watch

## Licença

Este projeto está sob a licença MIT.

## Autor

**Ruan Lopes**
- GitHub: [@RuanLopes1350](https://github.com/RuanLopes1350)