# E-mail Sino — "Bata a meta todo mês!"

E-mail marketing HTML (tabelas + CSS inline, sem JS) reproduzindo o frame `229:132` do Figma
([Landing Page Sino](https://www.figma.com/design/BW6yledW9XX39hRZZoyxna/Landing-Page-Sino?node-id=229-132)).
Largura 600px, breakpoint mobile em `max-width: 600px`.

```
index.html        pré-visualização local (imagens em assets/)
index.prod.html   versão de envio (imagens em {{ASSETS_BASE_URL}}/)
assets/           imagens usadas no e-mail
```

## Como editar

**Textos** — procure o texto no `index.html` e troque direto. Não use `<br>` para forçar quebra (quebra sozinho no mobile).
As palavras em verde são `<span style="color:#1e8484;">` (títulos) ou `#2dffd5` (sobre fundo escuro).

**URLs / placeholders** — substitua na plataforma de disparo (ou por find/replace):

| Placeholder | Onde |
|---|---|
| `{{CTA_URL}}` | os 2 botões "Quero diagnosticar minha operação comercial" (cada botão aparece 2× no HTML: versão Outlook/VML e versão padrão) |
| `{{SITE_URL}}` | logo do rodapé e "sino.com.br" |
| `{{COMPANY_ADDRESS}}` | linha de endereço no rodapé |
| `{{UNSUBSCRIBE_URL}}` | link "Descadastrar" |
| `{{ASSETS_BASE_URL}}` | só no `index.prod.html` — URL pública da pasta `assets` |

**Imagens** — troque o arquivo em `assets/` mantendo nome e proporção (os cards são 376×376 = 188×188 @2x, logos @4x).
Os cards têm cantos arredondados *dentro* do PNG (transparência), por isso são PNG.

**CTA** — o botão é "bulletproof": `<v:roundrect>` para Outlook Desktop + `<a>` com padding para os demais.
Se mudar o texto, ajuste o `width` (254px) no VML e na tabela do botão.

**Preheader** — primeiro `<div>` do `<body>`.

## Como publicar as imagens

E-mail não carrega imagem de caminho local. Suba a pasta `assets/` para um servidor/CDN público (HTTPS) e use:

```
https://dominio.com/email/assets/logo-sino-white.png
```

Gere o arquivo de envio a partir do `index.html` (ou apenas edite o `index.prod.html`):

```bash
sed 's#src="assets/#src="https://dominio.com/email/assets/#g' index.html > index.prod.html
```

Os 3 cards têm ~290 KB cada (PNG com transparência). Passe por TinyPNG/Squoosh antes de subir.

## Como testar

1. Abra `index.html` no navegador e redimensione (1440 → 320px); não deve haver scroll horizontal.
2. Envie um teste real (Litmus / Email on Acid / contas próprias) e confira em: **Gmail** (web, Android, iOS), **Outlook** (Desktop, Web, mobile), **Apple Mail** (macOS/iOS) e **Yahoo**.
3. Teste com imagens bloqueadas (os textos são HTML real; só logo, fotos e ícone dependem de imagem) e com modo escuro ligado.

## Decisões e limites conhecidos

- **Fonte**: Plus Jakarta Sans via Google Fonts (Apple Mail / iOS). Gmail e Outlook usam Arial (fallback) — quebras de linha podem mudar 1 linha.
- **Tamanho de texto**: no desktop segue o Figma (10px corpo / 13–14px títulos). No mobile sobe para 12–14px e os títulos crescem.
- **Mobile**: cards viram coluna (foto em cima, texto embaixo), botões ocupam a largura toda e as duas imagens decorativas laterais do hero são ocultadas.
- **Outlook Desktop**: botões em VML (cantos arredondados); o card escuro do CTA final fica com cantos retos.
- **Dark mode**: `color-scheme: light` fixa o Apple Mail em claro; Outlook (web/app) troca o logo do rodapé pela versão branca. O Gmail força inversão própria e não oferece controle — o logo escuro do rodapé tem menos contraste lá.
- **Rodapé**: a linha de CNPJ vem do Figma (valor de exemplo `123123123-0001/01`: troque pelo real). `{{COMPANY_ADDRESS}}` e "Descadastrar" foram adicionados abaixo dela, fora do layout do Figma, para atender requisitos de envio.
- Tamanho do HTML: ~20 KB (limite de corte do Gmail: ~102 KB).

## Cores (Figma)

`#072929` verde escuro · `#2DFFD5` destaque · `#1E8484` títulos · `#4D6B6B` texto secundário · `#F9F9F9` / `#FFFFFF` texto sobre escuro.
