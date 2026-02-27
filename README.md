# Chatwoot + Telegram Bot

Ambiente local do Chatwoot integrado com um bot do Telegram, usando Docker.

## Pré-requisitos

- Docker e Docker Compose instalados
- [ngrok](https://ngrok.com/download) instalado e autenticado
- Token de um bot do Telegram (obtido via @BotFather)

## Configuração

1. Copie o `.env.example` e preencha as variáveis:
```bash
cp .env.example .env
```

2. Certifique-se de que `FRONTEND_URL` no `.env` está com a URL pública do ngrok (veja o passo abaixo).

## Como rodar

**1. Exponha a porta 3000 com o ngrok** (em um terminal separado):
```bash
ngrok http 3000
```
Copie a URL gerada (ex: `https://abc123.ngrok-free.dev`) e coloque no `.env`:
```
FRONTEND_URL=https://abc123.ngrok-free.dev
```

**2. Prepare o banco de dados** (necessário na primeira vez ou após apagar os volumes):
```bash
docker compose run --rm chatwoot bundle exec rails db:chatwoot_prepare
```

**3. Suba os containers:**
```bash
docker compose up -d
```

Acesse em: `http://localhost:3000`

## Configurando o inbox do Telegram

1. No Chatwoot, vá em **Settings → Inboxes → Add Inbox**
2. Escolha **Telegram** e cole o token do bot
3. Salve — o webhook será registrado automaticamente com a `FRONTEND_URL`

## Atenção

- O ngrok gera uma **URL nova a cada reinicialização** (plano gratuito). Quando isso acontecer, atualize o `FRONTEND_URL` no `.env`, reinicie os containers com `docker compose restart` e recrie o inbox do Telegram.
- Nunca suba o arquivo `.env` para o repositório. Use o `.env.example` com as variáveis sem valores.