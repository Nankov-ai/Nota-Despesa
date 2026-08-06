# Nota de Despesa — Norauto

Aplicação single-file (`index.html`) para submissão de notas de despesa de colaboradores Norauto. Sem backend — tudo corre no browser (localStorage para rascunho/histórico, html2pdf.js + pdf-lib para gerar o PDF final).

## Deploy

- Repositório: https://github.com/Nankov-ai/Nota-Despesa
- Site público: https://nodeflow.pt/Nota-Despesa/ (domínio próprio apontado ao GitHub Pages).
- Origem do Pages = **GitHub Actions** (`build_type: workflow`, configurado via API a 2026-08-06 — Settings → Pages → Source → "GitHub Actions"). Não voltar a mudar para "Deploy from a branch": esse modo legado ficou com o build encravado em `"building"` indefinidamente (nunca progrediu, `duration: 0`) e o site inteiro passou a `"errored"`; forçar rebuild via API (`POST /pages/builds`) não resolveu — só a troca de `build_type` desbloqueou.
- Push para `main` publica automaticamente via `.github/workflows/deploy.yml` (`actions/configure-pages` + `actions/upload-pages-artifact` + `actions/deploy-pages`).
- Não renomear `index.html` — é o ponto de entrada do Pages.
- Para confirmar que o deploy realmente chegou ao ar (o cache do browser engana), verificar o HTML servido diretamente, não confiar só no estado do workflow:
  ```
  curl -s "https://nodeflow.pt/Nota-Despesa/?_=$(date +%s)" | grep 'data-i18n='
  ```

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
