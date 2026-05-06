# Formulário de contato: SMTP / EmailJS (não vai no GitHub)

O site é **estático** no GitHub Pages. **Senha SMTP e credenciais de e-mail não devem ir para o repositório** (nem em `.env` commitado): qualquer valor embutido no HTML/JS público pode ser lido por terceiros.

O fluxo seguro é: **configurar o envio no painel do EmailJS** e no código do site ficam apenas `publicKey`, `serviceId` e `templateId` (o public key do EmailJS é feito para aparecer no front; restrinja por domínio no painel).

---

## 1. Criar conta e serviço de e-mail

1. Acesse [emailjs.com](https://www.emailjs.com/) e crie uma conta.
2. **Email Services → Add New Service**.

### Opção A — Gmail

- Escolha **Gmail** e conecte a conta Google que enviará os e-mails.
- Com verificação em duas etapas ativa no Google, use **senha de app** se o fluxo pedir.

### Opção B — SMTP personalizado (provedor próprio)

- Escolha **Custom SMTP** ou equivalente.
- Preencha conforme o provedor, exemplo comum **Gmail**:

  | Campo    | Valor típico              |
  |----------|---------------------------|
  | Host     | `smtp.gmail.com`          |
  | Porta    | `587`                     |
  | Segurança| TLS / STARTTLS            |
  | Usuário  | seu e-mail completo       |
  | Senha    | **senha de app** (Google) |

- **Não** grave esses dados no Git: ficam só salvos no EmailJS.

---

## 2. Template de e-mail

1. **Email Templates → Create New Template**.
2. Campos que o site já envia (`name` no formulário → variável no modelo):

   - `{{from_name}}`
   - `{{from_email}}`
   - `{{company}}`
   - `{{interest}}`
   - `{{message}}`
   - `{{privacy_accept}}` (valor `sim` quando marcado)

3. Defina **To Email** (ex.: `gilson@gcndigital.com.br`).
4. Em **Reply To**, use `{{from_email}}` para responder direto ao visitante.
5. Salve e anote o **Template ID**.

---

## 3. Chaves no site (arquivo `index.html`)

No final do `index.html`, localize:

```js
const EMAILJS = {
  publicKey: 'SUBSTITUA_PUBLIC_KEY',
  serviceId: 'SUBSTITUA_SERVICE_ID',
  templateId: 'SUBSTITUA_TEMPLATE_ID'
};
```

Substitua pelos valores do painel EmailJS (**Account → API Keys** / integrações).

Depois: `git add index.html` → `git commit` → `git push`.

---

## 4. Segurança no EmailJS

- Em **Account / Security** (ou equivalente), restrinja o uso ao domínio **`gcndigital.com.br`**.
- Monitore quota do plano gratuito se o volume aumentar.

---

## 5. O que **não** fazer no Git

- Não commitar senha SMTP, senha de app do Gmail ou segredos da API de e-mail.
- Não colocar `.env` com segredos no repositório público.

Para builds futuros com injeção por CI (GitHub Actions), o **public key** do EmailJS ainda aparece no bundle público; proteção real é **domínio permitido** + **rate limit** no EmailJS.
