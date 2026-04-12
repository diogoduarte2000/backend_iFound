# 🍏 iFound Backend — API Server

API backend robusta para a plataforma de **Perdidos e Achados** iFound. Construída com **Node.js + Express.js** e **MongoDB**, oferecendo autenticação segura com 2FA, gestão de publicações, chat em tempo real e conformidade RGPD.

---

## 🎯 Funcionalidades do Backend

- **🔐 Autenticação 2FA**: Email + TOTP (Google Authenticator)
- **📱 Gestão de Publicações**: Criar, editar, listar perdidos/achados
- **💬 Chat em Tempo Real**: Mensagens com suporte a anexos
- **📍 Geolocalização**: Filtrar por zonas de Portugal
- **👥 Gestão de Utilizadores**: Registo, login, recuperação de password
- **🛡️ Conformidade RGPD**: Consentimento, validações de NIF
- **⚙️ Health Checks**: Endpoint de status da aplicação

---

## 🚀 Stack Técnico

| Tecnologia | Versão | Descrição |
|-----------|--------|-----------|
| **Node.js** | 18+ | Runtime JavaScript |
| **Express.js** | 5.2+ | Framework HTTP |
| **MongoDB** | N/A | Base de dados NoSQL |
| **Mongoose** | 9+ | ODM para MongoDB |
| **JWT** | 9.0+ | Autenticação stateless |
| **bcryptjs** | 3.0+ | Hashing de passwords |
| **Nodemailer** | 8.0+ | Envio de emails |
| **Speakeasy** | 2.0+ | Geração de TOTP |
| **QRCode** | 1.5+ | Geração de códigos QR |
| **express-validator** | 7.3+ | Validação de inputs |

---

## 🔒 Segurança

### Autenticação & Autorização
- ✅ JWT com expiração configurável
- ✅ Hashing com bcrypt (10+ rounds)
- ✅ 2FA via Email (60 segundos)
- ✅ 2FA via TOTP (Google Authenticator)
- ✅ Dispositivos de confiança (skip 2FA)
- ✅ Recuperação segura de password

### Validações
- ✅ express-validator em todas as rotas
- ✅ Validação de NIF português
- ✅ Validação de IMEI de dispositivos
- ✅ Sanitização de inputs
- ✅ Rate limiting (TODO)

### Conformidade
- ✅ RGPD: Consentimento obrigatório
- ✅ CORS configurável
- ✅ HTTPS ready (via nginx/Render)
- ✅ Dados sensíveis nunca em logs

---

## 📂 Estrutura de Diretórios

```
backend/
├── models/
│   ├── User.js              # Schema de utilizador (2FA, JWT, NIF)
│   ├── Publication.js       # Schema de publicação (perdido/achado)
│   └── Chat.js              # Schema de chat com mensagens
├── routes/
│   ├── auth.js              # POST /register, /login, /verify-2fa, /forgot-password
│   ├── publications.js      # CRUD de publicações
│   ├── chats.js             # CRUD de chats e mensagens
│   └── devices.js           # Gestão de dispositivos de confiança
├── middleware/
│   ├── authMiddleware.js    # Validação de JWT
│   └── totpUtils.js         # Validação de TOTP
├── utils/
│   └── imei.js              # Validação de IMEI
├── storage/
│   └── chat-attachments/    # Diretório para anexos de chat
├── data/
│   └── appleIphoneCatalog.js # Catálogo de modelos Apple
├── server.js                # Entry point com clustering
├── app.js                   # Configuração Express
├── db.js                    # Conexão MongoDB e validações
├── liveChatHub.js           # Socket.io para chat em tempo real
├── package.json             # Dependências
└── .env.example             # Variáveis de exemplo
```

---

## 🛠️ Instalação e Setup

### Pré-requisitos
- Node.js 18+ 
- MongoDB (Atlas ou local)
- npm ou yarn
- SMTP configurado (opcional, mas recomendado para 2FA)

### Passos

```bash
# 1. Clone o repositório
git clone https://github.com/diogoduarte2000/backend_iFound.git
cd backend_iFound

# 2. Instale dependências
npm install

# 3. Configure variáveis de ambiente
cp .env.example .env
# Edite o .env com suas credenciais

# 4. Inicie o servidor
npm start
# Servidor rodará em http://localhost:5000
```

### Variáveis de Ambiente Essenciais

```env
# Banco de dados
MONGODB_URI=mongodb+srv://user:pass@cluster.mongodb.net/ifound

# Autenticação
JWT_SECRET=sua-chave-secreta-muito-longa-e-aleatoria

# CORS (Frontend URL)
CLIENT_URL=http://localhost:4200

# Email SMTP (para 2FA)
SMTP_HOST=smtp.gmail.com
SMTP_PORT=465
SMTP_USER=seu-email@gmail.com
SMTP_PASS=app-password-16-chars

# Opcional
MAIL_FROM=no-reply@ifound.app
NODE_ENV=production
PORT=5000
NODE_CLUSTER_WORKERS=4
CHAT_REALTIME_SHARED_BACKPLANE=false
```

---

## 📚 Endpoints da API

### Autenticação (`/api/auth`)
```
POST   /api/auth/register              # Criar conta com 2FA
POST   /api/auth/login                 # Login com JWT
POST   /api/auth/verify-2fa            # Verificar código 2FA
POST   /api/auth/setup-totp            # Ativar Google Authenticator
POST   /api/auth/forgot-password       # Iniciar recuperação
GET    /api/auth/user                  # Obter dados do utilizador (protegido)
```

### Publicações (`/api/posts`)
```
GET    /api/posts                      # Listar todas (com filtros)
POST   /api/posts                      # Criar publicação
GET    /api/posts/:id                  # Obter detalhe
PATCH  /api/posts/:id                  # Editar (apenas autor)
DELETE /api/posts/:id                  # Eliminar (apenas autor)
```

### Chats (`/api/chats`)
```
GET    /api/chats                      # Listar chats do utilizador
POST   /api/chats                      # Criar novo chat
GET    /api/chats/:id                  # Obter mensagens
POST   /api/chats/:id/messages         # Enviar mensagem com anexos
```

### Dispositivos (`/api/devices`)
```
GET    /api/devices                    # Listar dispositivos de confiança
POST   /api/devices                    # Registar novo dispositivo
DELETE /api/devices/:id                # Remover dispositivo
```

### Status
```
GET    /api/status                     # Health check da API
GET    /                                # Payload de status completo
```

---

## 🚀 Deploy

### Render.com (Recomendado para Node.js)
```bash
# 1. Conecte seu repositório ao Render Dashboard
# 2. Configure variáveis de ambiente lá
# 3. Deploy automático em cada push
# 4. Comande: npm install && npm start
```

### Docker
```bash
# Build
docker build -t ifound-backend .

# Run
docker run -p 5000:5000 \
  -e MONGODB_URI=... \
  -e JWT_SECRET=... \
  ifound-backend
```

### Variáveis de Produção Críticas
- 🔴 `MONGODB_URI` — String de conexão com credenciais
- 🔴 `JWT_SECRET` — Chave de assinatura JWT muito segura
- 🟡 `CLIENT_URL` — URL exata do frontend
- 🟢 `SMTP_*` — Credenciais de email (com 2FA via App Password)

---

## 🧪 Testing e Desenvolvimento

```bash
# Executar em dev watch mode
npm run watch

# Testes (quando implementados)
npm test

# Linting (quando configurado)
npm run lint
```

---

## 📖 Documentação Adicional

- **[Repositório Principal](https://github.com/diogoduarte2000/iFound)** — Monorepo com frontend + backend
- **[Frontend Repository](https://github.com/diogoduarte2000/frontend_iFound)** — Angular SPA
- **API Documentation** — Swagger (TODO)

---

## 🤝 Contribuir

```bash
# 1. Fork do repositório
# 2. Branch para sua feature
git checkout -b feature/minha-feature

# 3. Commit e push
git commit -m "feat: Descrição da feature"
git push origin feature/minha-feature

# 4. Pull Request
```

---

## 📄 Licença

ISC — Veja LICENSE para detalhes.

---

**🍏 iFound Backend — Encontre o que perdeu!**  
*Segurança. Performance. Escalabilidade.*
