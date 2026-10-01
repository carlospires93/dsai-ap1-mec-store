# Visão geral (2026-09-30)

> M&C Store

## O quê e por quê

M&C Store é uma loja online de roupas, no estilo de grandes varejistas de moda (como a C&A), com vitrine, carrinho, checkout e acompanhamento de pedidos. O objetivo é que uma pessoa consiga descobrir roupas, escolher tamanho e cor, comprar e acompanhar a entrega, e que a equipe da loja consiga gerenciar produtos, estoque e pedidos.

O projeto é a AP1 da disciplina de Desenvolvimento de Software Apoiado por IA e é construído com o fluxo spec, plan, tasks. Cada parte do sistema tem uma spec própria em `SPEC/`, escrita antes do código que ela descreve.

## Perfis de usuário

- **Visitante:** navega no catálogo, busca, vê produtos e usa o carrinho sem conta.
- **Cliente:** tem conta, favorita produtos, finaliza compras, vê pedidos e avalia produtos.
- **Administrador:** gerencia catálogo, estoque, cupons e pedidos pelo painel admin.

## Partes do sistema (uma spec por parte)

1. `visao-geral` (este documento)
2. `cadastro-e-login`
3. `catalogo-e-busca`
4. `pagina-de-produto`
5. `carrinho`
6. `checkout-e-frete`
7. `pagamento-simulado`
8. `pedidos-e-rastreio`
9. `favoritos`
10. `cupons-e-promocoes`
11. `painel-admin`
12. `avaliacoes-e-perguntas`
13. `notificacoes`
14. `recomendacoes`
15. `acessibilidade-e-responsividade`

## Stack

- Next.js com TypeScript (front-end e API no mesmo projeto)
- Prisma com Postgres gerenciado (Neon ou Supabase)
- Tailwind CSS
- Vitest (unidade e integração) e Playwright (ponta a ponta)
- Deploy na Vercel, em URL pública

## Fluxo principal (usado na apresentação)

1. O visitante abre a URL e vê a vitrine.
2. Filtra por categoria, tamanho e faixa de preço.
3. Abre um produto, escolhe tamanho e cor, adiciona ao carrinho.
4. Cria conta ou entra.
5. Informa o CEP, escolhe o frete e paga (simulado).
6. Vê o pedido criado e o status de rastreio.

## Critérios de aceitação gerais

- A aplicação abre com um clique em uma URL pública, sem configuração prévia.
- O fluxo principal acima funciona do começo ao fim em tela de celular e de computador.
- Nenhum dado real de pagamento é solicitado nem armazenado.
- Nenhum segredo (chave de API, senha de banco) é versionado no repositório.
- Cada parte do sistema possui testes automatizados cobrindo seus critérios de aceitação.

## Convenções

- Valores monetários são guardados em centavos (inteiros) e exibidos em reais (R$).
- Datas são guardadas em UTC e exibidas no fuso de Brasília.
- Interface em português do Brasil.
- Cada commit traz os trailers `Agent:` e `Spec:`.

## Fora do escopo

- Integração com gateway de pagamento real
- Emissão de nota fiscal
- Integração com transportadoras reais (o frete é calculado por regras internas)
- Aplicativo móvel nativo
- Múltiplas lojas ou marketplace
