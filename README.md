# Projeto de Extensão: Sistema de Publicação Centralizada para pequenos negócios.

Workflow de automação usando n8n + API do Mercado Livre.

Preencha um formulário, clique em enviar e o anúncio é publicado automaticamente. Sem acessar a plataforma manualmente, sem copiar e colar entre abas. A arquitetura foi projetada para escalar para múltiplos marketplaces a partir de um único formulário.

---

## Como funciona

```
[Formulário] → [Validação] → [URLs das fotos] → [Payload] → [POST /items] → [Resultado]
```

1. O vendedor preenche um formulário com nome, descrição, preço, estoque e foto
2. O n8n valida os campos obrigatórios
3. As URLs das imagens são preparadas para a API
4. O payload é montado seguindo a especificação oficial da API do Mercado Livre
5. O produto é publicado via requisição autenticada com OAuth2
6. O link do anúncio é retornado em menos de 2 segundos

---

## Tecnologias

| Tecnologia | Função |
|---|---|
| n8n | Orquestração do workflow |
| Mercado Livre REST API | Publicação real do anúncio via POST /items |
| OAuth2 | Autenticação segura com a API |
| Docker | Ambiente de execução self-hosted |
| ngrok | Redirect OAuth2 em ambiente local |
| JavaScript | Transformação de dados e montagem do payload |


---

## Como rodar

### Pré-requisitos

- [Docker](https://www.docker.com/products/docker-desktop) instalado
- Conta no [ngrok](https://ngrok.com) (plano gratuito funciona)
- Conta no [Mercado Livre Developers](https://developers.mercadolivre.com.br)

### 1. Subir o n8n com Docker

```bash
docker run -d \
  --name n8n \
  --restart unless-stopped \
  -p 5678:5678 \
  -v n8n_data:/home/node/.n8n \
  -e WEBHOOK_URL=https://SUA_URL_NGROK \
  -e N8N_HOST=SEU_DOMINIO_NGROK \
  -e N8N_PROTOCOL=https \
  n8nio/n8n
```

### 2. Expor o n8n com ngrok

```bash
ngrok http 5678
```

Copie a URL gerada e atualize o WEBHOOK_URL no comando Docker acima.

### 3. Importar o workflow

No n8n: **Workflows → Import from file** → selecione `workflow_extensao_final.json`

### 4. Configurar credenciais

Crie uma credential OAuth2 API no n8n com os dados do seu app do Mercado Livre:

```
Authorization URL: https://auth.mercadolibre.com/authorization
Access Token URL:  https://api.mercadolibre.com/oauth/token
Scope:             write:items read:items
```

---

## Resultado

Anúncio publicado via API com status 201 Created em 1,7 segundos.

---

## Contexto acadêmico

Projeto desenvolvido para a disciplina de Projeto de Extensão, sob orientação do professor Raphael Lopes de Souza, no curso de Inteligência Artificial da USCS. Foca em automação de e-commerce para pequenos negócios.

---

## Equipe

Projeto desenvolvido por alunos.

| Nome | GitHub |
|------|--------|
| Larissa Pavan | [@lacpavan](https://github.com/lacpavan) |
| Beatriz Mayumi | [@BiaMayumi](https://github.com/BiaMayumi) |
| Thiago Alves Serra | [@thiagoexpressoDev](https://github.com/thiagoexpressoDev) |
| Guilherme Carvalho | [@guilhermecarvalho173](https://github.com/guilhermecarvalho173) |
| Samuel de Castro | — |
| Luana Goto | [@luanagoto](https://github.com/luanagoto) |
| Maria Clara Esteves | [@uusaagii](https://github.com/uusaagii) |
