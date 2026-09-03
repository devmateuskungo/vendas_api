#Store API

REST API para gestão de uma loja online, construída com **Node.js**, **Express** e **MongoDB**.

Permite o registo e autenticação de clientes (JWT), a gestão de produtos com upload de imagens e o processamento de pedidos (orders).

## Tecnologias

- **Node.js / Express** — servidor e rotas
- **MongoDB + Mongoose** — persistência de dados
- **JSON Web Token** — autenticação de clientes
- **Multer** — upload de imagens
- **Supabase Storage** — armazenamento das imagens dos produtos
- **Resend** — envio de e-mails de boas-vindas

## Estrutura do projeto

```
src/
├── app.js                    # Configuração do Express e ligação ao MongoDB
├── config.js                 # Variáveis de ambiente (chaves, string de ligação)
├── bin/server.js             # Arranque do servidor
├── controller/               # Lógica dos endpoints (produtos, clientes, pedidos)
├── models/                   # Esquemas Mongoose (Product, Customer, Order)
├── repositores/              # Acesso aos dados
├── routes/                   # Definição das rotas
├── services/                 # auth, e-mail e imagem
└── validators/               # Validação dos dados de entrada
```

## Instalação

Requisitos: **Node.js** e uma instância (ou cluster) **MongoDB**.

```bash
# clonar o repositório
git clone <url-do-repositorio>
cd myapi

# instalar as dependências
npm install

# configurar as variáveis de ambiente em src/config.js:
#   connectionString        -> URI do MongoDB
#   global.SALT_KEY         -> segredo usado para assinar os JWTs
#   supabaseUrl / supabaseKey / supabaseBucket -> storage das imagens
#   resendKey / emailFrom   -> envio de e-mails

# iniciar o servidor
npm start
```

Por predefinição, a API fica disponível em `http://localhost:3000`.

## Endpoints

### Produtos
| Método | Rota            | Descrição                          | Autenticação |
|--------|-----------------|------------------------------------|--------------|
| GET    | `/products`     | Lista todos os produtos            | Não          |
| GET    | `/products/:slug` | Busca um produto pelo slug       | Não          |
| GET    | `/products/tag/:tag` | Busca produtos por tag        | Não          |
| GET    | `/products/admin/:id` | Busca um produto pelo id     | Não          |
| POST   | `/products`     | Cria um produto (upload de imagens)| Sim          |
| PUT    | `/products/:id` | Atualiza um produto                | Sim          |
| DELETE | `/products/:id` | Remove um produto                  | Sim          |

### Clientes
| Método | Rota                        | Descrição                          | Autenticação |
|--------|-----------------------------|------------------------------------|--------------|
| GET    | `/customers`                | Lista todos os clientes            | Não          |
| POST   | `/customers`                | Regista um novo cliente            | Não          |
| POST   | `/customers/authenticate`   | Autentica e devolve um token JWT   | Não          |
| DELETE | `/customers/:id`            | Remove um cliente                  | Não          |

### Pedidos
| Método | Rota      | Descrição                    | Autenticação |
|--------|-----------|------------------------------|--------------|
| GET    | `/orders` | Lista todos os pedidos       | Sim          |
| POST   | `/orders` | Cria um novo pedido          | Sim          |

## Autenticação

Os endpoints protegidos exigem um token JWT no header:

```
x-access-token: <seu-token-jwt>
```

O token é obtido através de `POST /customers/authenticate` com `email` e `password`.

## Exemplo: criar um pedido

```
POST /orders
x-access-token: <token>

{
  "items": [
    { "quantity": 2, "price": 10, "product": "<id-do-produto>" }
  ]
}
```

> O cliente e o número do pedido são preenchidos automaticamente a partir do token autenticado.

## Licença

ISC
