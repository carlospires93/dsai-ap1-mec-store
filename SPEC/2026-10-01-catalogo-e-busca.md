# Catálogo e busca (2026-09-30)

## O quê e por quê

O cliente precisa encontrar roupas rapidamente. Esta parte cobre a listagem de produtos, a navegação por categorias, os filtros, a ordenação e a busca por texto. É a porta de entrada da loja e alimenta as partes de página de produto, favoritos e recomendações.

## Modelo de dados (resumo)

- **Categoria:** nome, slug, categoria-pai opcional (ex.: Feminino > Vestidos).
- **Produto:** nome, slug, descrição, marca, categoria, preço em centavos, preço promocional opcional, ativo (sim/não), data de criação.
- **Variante:** produto, tamanho, cor, SKU, quantidade em estoque.
- **Imagem:** produto, cor, URL, ordem.

## Critérios de aceitação

### Listagem
- A página do catálogo lista apenas produtos ativos.
- Produto sem nenhuma variante com estoque maior que zero aparece com a etiqueta "Esgotado" e não é exibido por padrão quando o filtro "Somente disponíveis" está ligado.
- A listagem é paginada com 24 produtos por página.
- Cada card mostra imagem principal, nome, marca, preço e, quando houver promoção, o preço antigo riscado e o percentual de desconto arredondado.
- Se a lista estiver vazia, a página mostra a mensagem "Nenhum produto encontrado" e um botão para limpar filtros.

### Categorias
- Cada categoria tem uma URL própria baseada no slug (`/c/feminino/vestidos`).
- Ao abrir uma categoria-pai, aparecem os produtos da própria categoria e de todas as subcategorias.
- A página mostra o caminho de navegação (breadcrumb) até a categoria atual.

### Filtros
- É possível filtrar por: categoria, tamanho, cor, marca, faixa de preço e "em promoção".
- Filtros de grupos diferentes se combinam com E (tamanho M **e** cor azul). Valores dentro do mesmo grupo se combinam com OU (tamanho M **ou** G).
- O filtro de tamanho considera apenas variantes com estoque maior que zero.
- A faixa de preço usa o preço promocional quando existir.
- Cada opção de filtro mostra a quantidade de produtos resultantes.
- Os filtros ativos são refletidos na URL, de modo que copiar o link reproduz o mesmo resultado.
- Existe o botão "Limpar filtros", que remove todos de uma vez.

### Ordenação
- Opções: Relevância (padrão), Menor preço, Maior preço, Mais recentes, Maiores descontos.
- A ordenação por preço usa o preço promocional quando existir.
- Produtos com o mesmo valor de ordenação mantêm ordem estável (desempate por data de criação, depois por id).

### Busca por texto
- A busca considera nome, marca, descrição e nome da categoria.
- A busca ignora maiúsculas, minúsculas e acentos ("blusa", "BLUSA" e "blúsa" retornam o mesmo resultado).
- Termos com várias palavras exigem que todas estejam presentes em algum dos campos.
- Resultados com o termo no nome vêm antes dos que têm o termo apenas na descrição.
- Busca vazia ou só com espaços não dispara pesquisa.
- Se não houver resultados, a página sugere até 3 categorias populares.
- A caixa de busca mostra até 5 sugestões de produtos enquanto a pessoa digita, com espera de 300 ms entre teclas.

### Desempenho e robustez
- A listagem responde em até 800 ms com 5.000 produtos no banco (medido em teste de integração local).
- Parâmetros inválidos na URL (página negativa, preço não numérico, ordenação desconhecida) são ignorados e a página usa os valores padrão, sem erro.
- Entradas de busca com caracteres especiais não causam erro nem permitem injeção de comandos no banco.

### Acessibilidade e responsividade
- Em telas pequenas, os filtros ficam em um painel que abre por botão.
- Todos os controles de filtro e ordenação são acessíveis por teclado e têm rótulos para leitores de tela.
- Imagens dos cards têm texto alternativo com o nome do produto.

## Testes esperados

- Unidade: montagem da consulta de filtros, normalização de texto, cálculo de desconto, ordenação estável.
- Integração: endpoints de listagem e busca com banco de teste.
- Ponta a ponta: abrir categoria, aplicar filtro, ordenar, buscar e abrir um produto.

## Fora do escopo

- Busca por imagem
- Correção ortográfica automática e sinônimos
- Personalização da ordem por histórico do cliente (tratada em `recomendacoes`)
- Comparação de produtos lado a lado
