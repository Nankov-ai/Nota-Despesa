# Nota de Despesa — Norauto

Aplicação single-file (`index.html`) para submissão de notas de despesa de colaboradores Norauto. Sem backend — tudo corre no browser (localStorage para rascunho/histórico, html2pdf.js + pdf-lib para gerar o PDF final).

## Deploy

- Repositório: https://github.com/Nankov-ai/Nota-Despesa
- Push para `main` dispara `.github/workflows/deploy.yml`, que publica `index.html` (pasta raiz) na branch `gh-pages` via GitHub Pages.
- Não renomear `index.html` — é o ponto de entrada do Pages.

## Idiomas (PT/FR)

- Seletor no cabeçalho (`#lang-pt` / `#lang-fr`) chama `setLanguage(lang)`.
- Dicionário de traduções em `translations = { pt: {...}, fr: {...} }` no `<script>`.
- Texto estático usa `data-i18n="chave"` (e `data-i18n-placeholder` / `data-i18n-title` para atributos); `setLanguage()` percorre esses elementos.
- Conteúdo gerado dinamicamente (linhas de despesa, template do PDF, toasts) usa a função `t('chave')` diretamente no JS — não hardcoded strings.
- Linhas de despesa já existentes ao trocar de idioma são retraduzidas via classes `.row-lbl-*` dentro de `setLanguage()` (não são recriadas, para preservar valores/anexos).
- Idioma escolhido persiste em `localStorage` (`nd_lang`).
- Em francês, "Centro" (cost center) traduz-se como **"Magasin"**, não "Centre" — pedido explícito do utilizador.

## Layout

- Tailwind via CDN. Grid responsivo (`grid-cols-12`, breakpoints `md:`) — a app é usada sobretudo em smartphone, por isso qualquer alteração tem de manter o comportamento mobile-first intacto.

## Ficheiro de trabalho local

O ficheiro mestre editado normalmente está em `c:\projetos\7. Nota Despesa\Nota Despesa 11.03.2026.html` (fora deste repo git). Após editar, copiar para `geral/index.html` antes de commit/push.
