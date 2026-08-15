# Como publicar — 5 minutos

## Opção A · GitHub Pages (URL: `https://SEU-USUARIO.github.io/transla/`)

1. Vá em **github.com/new**. Nome do repositório: `transla`. Marque **Public**. Crie.
2. Na página seguinte, clique em **uploading an existing file**.
3. Arraste **todo o conteúdo desta pasta** (o `index.html`, e as pastas `casos/` e `dados/`).
   Arraste os arquivos e pastas, não a pasta `transla` inteira — o `index.html` precisa
   ficar na raiz do repositório.
4. **Commit changes**.
5. **Settings → Pages** → em *Source* escolha **Deploy from a branch** → branch **main**,
   pasta **/ (root)** → **Save**.
6. Espere ~1 minuto e recarregue. A URL aparece no topo dessa mesma página.

## Opção B · Netlify Drop (mais rápido ainda, sem conta obrigatória)

1. Vá em **app.netlify.com/drop**.
2. Arraste a pasta `transla` inteira para a área indicada.
3. A URL sai na hora. Dá para renomear o subdomínio em *Site settings → Change site name*.

## Depois de publicar — 3 substituições no README.md

Troque `SEU-USUARIO` pela sua conta do GitHub nos **três** lugares em que aparece
(link do topo, e a seção *Como citar*), e preencha `SOBRENOME, N.` com o seu nome
na citação.

## Confira antes de citar na monografia

- [ ] A página inicial abre e mostra a capa com os quatro casos
- [ ] Os quatro links de caso abrem
- [ ] Os dois botões de exemplo ("Exemplo 1" e "Exemplo 2") rodam a conversa demonstrativa
- [ ] O rodapé mostra AUROC 0,816
