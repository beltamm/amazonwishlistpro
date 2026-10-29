# ⚡ Amazon Wishlist Pro

Bookmarklet que filtra, ordena e organiza Listas de Desejos da Amazon (amazon.com.br) direto na página, sem extensão e sem enviar dados para lugar nenhum.

## Instalação

1. Abra a página do projeto no GitHub Pages: `https://SEU-USUARIO.github.io/NOME-DO-REPO/`
2. Mostre a barra de favoritos (`Ctrl+Shift+B` ou `Cmd+Shift+B`).
3. Arraste o botão laranja **⚡ Wishlist Pro** para a barra.
4. Abra uma Lista de Desejos da Amazon e clique no favorito. Clique de novo para minimizar ou expandir o painel.

**Celular ou sem arrastar:** na página, use **Copiar código do bookmarklet**, crie um favorito qualquer e cole o código no endereço dele.

## Recursos

**Filtros**
- Desconto mínimo, nota mínima e preço máximo
- Formato: sem Kindle, só físicos, só Kindle ou todos
- Frete grátis e só em estoque
- Busca sem acento, com vários termos e exclusão (`-kindle`)
- Tags: automáticas por tema e personalizadas

**Ordenação**
- Custo-benefício, maior % de desconto, maior R$ de desconto
- Menor preço, maior preço, melhor avaliação, título (A–Z) e ordem original

**Organização**
- Selos em cada item: formato, frete, desconto e tags
- Tags automáticas por regras (RPG, terror, sci-fi, HQ, design e outras)
- Adicionar e remover tags por item, ou aplicar uma tag a todos os itens visíveis
- Itens com 40% ou mais de desconto ganham uma marca lateral

**Resumo e exportação**
- Total e economia dos itens visíveis
- Meta de frete grátis: mostra quanto falta para atingir o valor que você definir
- Copiar a lista em Markdown (com links)
- Baixar CSV pronto para Excel pt-BR

**Desempenho e interface**
- Ao abrir, rola a página sozinho até carregar a lista inteira (desligável em **Carregar tudo ao abrir**) e lê os itens novos conforme aparecem
- Se o carregamento travar, o botão mostra quantos itens foram lidos e permite continuar
- Modo compacto e modo sem imagens
- Painel isolado do CSS da Amazon (Shadow DOM)
- Preferências salvas entre visitas
- O botão ✕ fecha o painel e restaura a página como estava

## Privacidade

Tudo roda no seu navegador. As preferências e as tags ficam no `localStorage` do domínio da Amazon. Nenhum dado é enviado a servidores.

## Como atualizar

O projeto é um único arquivo, `index.html`, sem dependências.

1. Edite a função `wishlistPro()` dentro do `<script>`.
2. Faça commit e push. O GitHub Pages publica em cerca de um minuto.
3. **Arraste o botão de novo** (apagando o favorito antigo). O favorito guarda uma cópia do código, então ele não se atualiza sozinho.

O link `javascript:` é gerado pela própria página a partir da função, então você não precisa gerar nada à mão.

### Regras ao editar

- Use `//` apenas em linhas que contenham só o comentário. Elas são removidas na geração do favorito. Um `//` no fim de uma linha de código vai para o favorito e pode quebrá-lo.
- Termine as instruções com `;`. As quebras de linha são preservadas, mas não conte com a inserção automática de ponto e vírgula.
- Não use `</script>` dentro da função.

### Ajustar as tags automáticas

As regras ficam no objeto `RULES`. Cada tag é uma lista de termos:

```js
'🎲 RPG': ['rpg*', 'd&d', 'pathfinder', 'livro basico', ...]
```

- Maiúsculas e acentos são ignorados, então escreva os termos sem acento.
- Um termo terminado em `*` casa por prefixo (`narrativ*` casa com "narrativa" e "narrativas").
- Os demais casam como palavra inteira (`osr` não casa com "posrp").
- Os termos são comparados com o título e o autor/editora do item.

### Ajustar a pontuação "Custo-benefício"

Os pesos estão na constante `W`, no topo da função:

```js
const W = { price: 0.6, pct: 2.5, brl: 0.8, free: 18, notFree: -15, rate: 3 };
```

## Se algo parar de funcionar

A Amazon muda o HTML com frequência. Se preço, desconto ou frete vierem vazios, abra o inspetor, copie o HTML de um item (`<li data-itemid="...">`) e ajuste os seletores em `parseItem()`.

## Limitações

- O selo **🚚 frete grátis?** (tracejado) é uma estimativa: livro físico que não menciona frete. O selo **🚚 FRETE GRÁTIS** (sólido) vem de Prime ou do texto do item.
- O percentual de desconto pode vir de um selo do item, de um texto ou do preço de lista. O tooltip do selo mostra a origem.
- Testado no Chromium com listas simuladas (inclusive 1000 itens em lotes de 20). Em listas reais, os seletores podem precisar de ajuste.

## Histórico

### v8
- Corrigidas as tags automáticas, que nunca casavam na v6 por um erro de escape nas regexes
- Carregamento automático da lista inteira ao abrir, com gatilhos de rolagem mais confiáveis e aviso quando trava
- Itens que saem do DOM deixam de ser descartados da contagem
- Remover uma tag automática agora persiste
- Painel em Shadow DOM, com preferências salvas
- Carregamento incremental com `MutationObserver`
- Busca sem acento, com exclusão de termos
- Novos filtros: nota mínima e sem imagens. Novas ordenações: maior preço e título
- Resumo dos visíveis, meta de frete e tag em lote
- Exportação em Markdown e CSV
- Botão para fechar e restaurar a página
- Instalador com cópia do código e código-fonte visível

### v6
- Versão inicial com filtros, ordenação por desconto, frete e tags
