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
2. Campos que o site envia (o `name` de cada input vira variável `{{...}}` no modelo):

   - `{{from_name}}`
   - `{{from_email}}`
   - `{{company}}`
   - `{{interest}}`
   - `{{message}}`
   - `{{privacy_accept}}` (valor `sim` quando marcado)

**Importante:** o template padrão “Contact Us” do EmailJS costuma usar `{{name}}`, `{{email}}`, `{{title}}` — **isso não bate com o site**. Troque no editor do EmailJS para os nomes acima, senão nome e resposta ficam vazios.

**Exemplo (aba Content):**

- **Subject:** `Contato gcndigital.com.br — {{interest}}`
- **Corpo (pode ser texto simples):**

```text
Novo contato pelo site.

Nome: {{from_name}}
E-mail: {{from_email}}
Empresa: {{company}}
Interesse: {{interest}}
Aceite LGPD (política): {{privacy_accept}}

Mensagem:
{{message}}
```

- **To Email:** o seu e-mail de recebimento.
- **From Name:** `GCN Digital — site` (fixo) ou `{{from_name}}` (aparece o visitante como remetente “nome”).
- **From Email:** deixe o padrão do serviço (Gmail) se a opção “Use Default Email Address” existir.
- **Reply To:** `{{from_email}}` (essencial para responder ao visitante com um clique).

3. Salve o template (**Save**) — o **Template ID** aparece na lista (ex.: `template_7g51m7w`).

---

## 3. Chaves no site (arquivo `index.html`)

No final do `index.html`, localize:

```js
const EMAILJS = {
  publicKey: 'SUA_PUBLIC_KEY',
  serviceId: 'service_7x5ac2h',
  templateId: 'template_7g51m7w'
};
```

**Private Key** do EmailJS **não** entra no site nem no Git — uso restrito a integrações server-side. Se ela vazar, gere outra no painel.

Substitua `SUA_PUBLIC_KEY` pelo valor em **Account → API Keys**, se ainda não estiver preenchido no arquivo.

Depois: `git add index.html` → `git commit` → `git push`.

---

## 4. Segurança no EmailJS

- Em **Account / Security** (ou equivalente), restrinja o uso ao domínio **`gcndigital.com.br`**.
- Monitore quota do plano gratuito se o volume aumentar.

---

## 5. O que **não** fazer no Git

- Não commitar senha SMTP, senha de app do Gmail, **Private Key** do EmailJS ou segredos de API.
- Não colocar `.env` com segredos no repositório público.

Para builds futuros com injeção por CI (GitHub Actions), o **public key** do EmailJS ainda aparece no bundle público; proteção real é **domínio permitido** + **rate limit** no EmailJS.
