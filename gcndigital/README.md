# GCN Digital — Site Institucional

Site institucional da GCN Digital Ltda hospedado via GitHub Pages com domínio customizado `gcndigital.com.br`.

---

## 🚀 Deploy no GitHub Pages (passo a passo)

### 1. Criar o repositório no GitHub

1. Acesse [github.com](https://github.com) e faça login
2. Clique em **New repository**
3. Nome do repositório: `gcndigital.com.br` (ou `gcn-digital-site`)
4. Marque como **Public** (obrigatório para GitHub Pages grátis)
5. Clique em **Create repository**

---

### 2. Fazer upload dos arquivos

**Via terminal (Mac):**
```bash
cd /pasta/onde/estao/os/arquivos

git init
git add .
git commit -m "Initial commit — GCN Digital site"
git branch -M main
git remote add origin https://github.com/SEU_USUARIO/gcndigital.com.br.git
git push -u origin main
```

**Via interface web do GitHub:**
- Arraste os arquivos (`index.html`, `CNAME`) diretamente para o repositório

---

### 3. Ativar o GitHub Pages

1. No repositório, clique em **Settings**
2. No menu lateral, clique em **Pages**
3. Em **Source**, selecione: `Deploy from a branch`
4. Branch: `main` | Folder: `/ (root)`
5. Clique em **Save**
6. Aguarde ~2 minutos → aparecerá a URL `https://SEU_USUARIO.github.io/repo-name`

---

### 4. Configurar o domínio gcndigital.com.br

#### No GitHub Pages (Settings > Pages):
- Em **Custom domain**, digite: `gcndigital.com.br`
- Marque **Enforce HTTPS**
- Salve

O arquivo `CNAME` já está incluído no repositório com o conteúdo correto.

#### No painel DNS do seu registrador (Registro.br ou outro):

Adicione os seguintes registros DNS:

**Registros A (apex domain — gcndigital.com.br):**
```
Tipo: A    | Host: @    | Valor: 185.199.108.153
Tipo: A    | Host: @    | Valor: 185.199.109.153
Tipo: A    | Host: @    | Valor: 185.199.110.153
Tipo: A    | Host: @    | Valor: 185.199.111.153
```

**Registro CNAME (www):**
```
Tipo: CNAME | Host: www | Valor: SEU_USUARIO.github.io
```

> ⚠️ Propagação DNS pode levar de 30 minutos a 24 horas.
> Após propagar, o HTTPS é emitido automaticamente pelo GitHub (Let's Encrypt).

---

### 5. Verificar se está tudo certo

Acesse no terminal:
```bash
dig gcndigital.com.br +noall +answer
```
Deve retornar os IPs `185.199.10x.153`.

Ou use: [dnschecker.org](https://dnschecker.org) → digite `gcndigital.com.br`

---

## 📁 Estrutura de arquivos

```
gcndigital.com.br/
├── index.html          # Site completo (HTML + CSS + JS inline)
├── CNAME               # Domínio customizado para GitHub Pages
└── README.md           # Este guia
```

---

## ✏️ Como editar o site

O site é um único arquivo `index.html` com CSS e JS embutidos.

| O que editar | Onde no arquivo |
|---|---|
| Número de WhatsApp | Linha com `wa.me/5511999999999` |
| E-mail de contato | Seção `#contato` |
| Métricas dos cases | Seção `#cases` |
| Depoimentos | Seção `#depoimentos` |
| Foto (sobre) | Substituir o placeholder `.sobre-image-placeholder` por `<img>` |
| Cores globais | Variáveis CSS no `:root` (topo do `<style>`) |

---

## 🔄 Atualizar o site após edições

```bash
git add .
git commit -m "Update: descrição da mudança"
git push origin main
```
O GitHub Pages redeploya automaticamente em ~1 minuto.

---

## 📋 Checklist pós-publicação

- [ ] Domínio `gcndigital.com.br` apontando para GitHub Pages
- [ ] HTTPS ativo (cadeado verde)
- [ ] Número de WhatsApp atualizado
- [ ] E-mail de contato atualizado
- [ ] Foto pessoal adicionada (seção Sobre)
- [ ] CNPJ adicionado no footer
- [ ] Google Search Console: adicionar propriedade `gcndigital.com.br`
- [ ] Google Analytics 4: adicionar tag GA4
- [ ] Sitemap: criar `sitemap.xml` (site de 1 página → simples)

---

*GCN Digital Ltda · São Paulo, SP*
