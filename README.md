# Site — Embracon Consórcios · Franquia Guarulhos

Site de conversão para captação de leads de consórcio (imóveis, carros, motos e pesados), com simulador de parcelas, formulário de captação (nome, e-mail, celular, CEP), envio de leads por WhatsApp e e-mail, blog com artigos e páginas por produto.

## Tecnologia

**HTML + CSS + JavaScript puros, em um único arquivo (`index.html`).**
Não há build, dependências, banco de dados ou backend — é um site 100% estático.
A navegação entre as "páginas" (produtos, blog, artigos) é feita por um router de hash (`#/carros`, `#/blog` etc.) dentro do próprio arquivo, então funciona em qualquer hospedagem estática e até aberto localmente.

## Estrutura

```
site-embracon-guarulhos/
├── index.html   ← o site inteiro (páginas, estilos, scripts, logos embutidos)
├── README.md
├── .gitignore
└── assets/      ← arquivos de marca (não são usados pelo site; logos já estão
    │              embutidos em base64 no index.html — esta pasta é para
    │              materiais de marketing e edições futuras)
    ├── logo_embracon.png            (logo original completo, alta resolução)
    ├── logo-franquia-guarulhos.svg  (versão vetorial recriada)
    ├── logo-franquia-guarulhos.png
    ├── logo-mark-160.png            (símbolo recortado, usado no header)
    └── logo-fav-64.png              (favicon)
```

## Como executar localmente

Basta abrir o `index.html` no navegador (duplo clique).

Para testar o envio de e-mail (FormSubmit), que exige contexto de servidor, rode um servidor local:

```bash
# opção 1 (Python)
python -m http.server 8000

# opção 2 (Node)
npx serve .
```

E acesse `http://localhost:8000`.

## Configuração (antes de publicar!)

Toda a configuração fica no início do `<script>` no final do `index.html`:

| Constante        | O que é                                                        | Status |
|------------------|----------------------------------------------------------------|--------|
| `WHATSAPP`       | Número que recebe os leads (55 + DDD + número, só dígitos)     | ⚠️ PLACEHOLDER — trocar `5511900000000` |
| `EMAIL_FRANQUIA` | E-mail que recebe cada simulação (via FormSubmit.co)           | ✅ `franquia.guarulhos@embracon.com.br` |
| `URL_CRM`        | (Opcional) endpoint de CRM/planilha para receber o lead em JSON | vazio  |
| `TIPOS`          | Parâmetros do simulador por segmento (taxa adm., fundo, prazo) | valores ilustrativos — calibrar com as tabelas Embracon |

Também procure por `(11) 9XXXX-XXXX` no rodapé e troque pelo número real.

**Ativação do e-mail:** na primeira simulação enviada com o site publicado, o FormSubmit.co manda um e-mail de confirmação para `EMAIL_FRANQUIA` — clique em "Activate" uma única vez. Sem isso os e-mails não chegam. O envio por e-mail não funciona abrindo o arquivo localmente sem servidor.

## Como publicar

### GitHub Pages (grátis)

```bash
git init
git add .
git commit -m "Site Embracon Franquia Guarulhos"
git branch -M main
git remote add origin https://github.com/SEU-USUARIO/site-embracon-guarulhos.git
git push -u origin main
```

Depois, no GitHub: **Settings → Pages → Source: Deploy from a branch → Branch: main / (root) → Save**.
O site fica no ar em `https://SEU-USUARIO.github.io/site-embracon-guarulhos/`.

### Alternativas

- **Netlify**: arraste a pasta em https://app.netlify.com/drop — no ar em segundos.
- **Vercel**: `npx vercel` na pasta do projeto.
- **Domínio próprio** (ex.: `embraconguarulhos.com.br`): configure o domínio no GitHub Pages/Netlify/Vercel e aponte o DNS.

## Segurança e privacidade

- O projeto **não contém senhas, tokens ou chaves de API** — o FormSubmit.co não usa chave e o WhatsApp usa link público `wa.me`.
- Por ser um site estático, tudo no `index.html` é público por natureza. O e-mail e o WhatsApp da franquia são informações públicas de contato — isso é esperado.
- **Não há variáveis de ambiente** neste projeto (por isso não existe `.env`/`.env.example`). Se um dia integrar um CRM com chave secreta, faça isso em um backend/função serverless — nunca coloque a chave no `URL_CRM` do front-end.
- Leads: o site envia os dados apenas para o WhatsApp/e-mail da franquia. Trate-os conforme a LGPD.

## Aviso regulatório

Consórcio é regulado pelo Banco Central (Lei nº 11.795/2008). Os textos legais do rodapé (vínculo com a administradora, CNPJ, aviso de contemplação sem data garantida e simulações ilustrativas) **não devem ser removidos**.
