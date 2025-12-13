# Deploy automático no Netlify via GitHub

Este repositório contém um site estático (index.html na raiz). A seguir há instruções para publicar automaticamente no Netlify ao enviar alterações para o GitHub.

## Passo a passo (mais simples: conectar GitHub -> Netlify)

1. Crie um repositório no GitHub.
2. No seu terminal, dentro da pasta do projeto, execute:

```bash
# inicializa repositório local (se necessário)
git init
git add .
git commit -m "Initial commit"
# adicione o repositório remoto (substitua URL)
git remote add origin https://github.com/SEU_USUARIO/SEU_REPO.git
# envia a branch principal
git push -u origin main
```

3. No Netlify:
- Acesse `https://app.netlify.com` e faça login.
- Escolha "Add new site" → "Import from Git" → selecione GitHub.
- Autorize o Netlify a acessar seu repositório e selecione o repositório.
- Em **Build settings**, se o site for estático (index.html na raiz), deixe `Build command` em branco e `Publish directory` como `.` (a raiz).
- Clique em "Deploy site" — o Netlify fará o primeiro build e deploy.

Agora, sempre que você der push para a branch configurada (ex.: `main`), o Netlify fará automaticamente um novo deploy.

## Como ter deploy via GitHub Actions (alternativa, opcional)

Se preferir, você pode usar um workflow do GitHub Actions para publicar no Netlify diretamente. Isso permite maior controle e pode ser útil em pipelines com steps customizados.

### Configuração

1. Crie um token no Netlify em **User settings > Applications > Personal access tokens**.
2. No GitHub, abra o repositório → Settings → Secrets → `Actions` → `New repository secret` e adicione:
   - `NETLIFY_AUTH_TOKEN`: o token criado no Netlify.
   - `NETLIFY_SITE_ID`: você encontra em Site settings → Site information → "Site ID".
3. O workflow já incluído aqui (`.github/workflows/netlify-deploy.yml`) usa esses segredos para fazer deploy automático quando você der push para `main`.

### Executando um deploy manual via GitHub Actions
- Faça push para a branch `main` ou abra uma PR (dependendo do gatilho configurado) e verifique o job no GitHub Actions.

## Arquivo `netlify.toml`

O arquivo `netlify.toml` nesta pasta informa ao Netlify para publicar o diretório raiz. Se você estiver usando uma ferramenta de build (ex.: React/Vite/Next), atualize `command` e `publish` conforme necessário.

## Dicas rápidas
- Para previews de PR (Deploy Previews) mantenha o Netlify conectado ao GitHub. Ele cria automaticamente previews.
- Para deploys manuais ou pré-visualizações você pode usar `npx netlify-cli`.
