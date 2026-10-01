# Cadastro e login (2026-09-30)

## O quê e por quê

Para comprar, acompanhar pedidos e favoritar produtos, a pessoa precisa de uma conta. Esta parte cobre cadastro, login, logout, recuperação de senha, perfil e endereços. Também define os papéis (cliente e administrador) usados pelo painel admin.

## Modelo de dados (resumo)

- **Usuário:** nome, e-mail (único), hash da senha, papel (`CLIENTE` ou `ADMIN`), CPF opcional, telefone opcional, data de criação.
- **Endereço:** usuário, apelido, CEP, rua, número, complemento, bairro, cidade, UF, padrão (sim/não).
- **Token de recuperação:** usuário, hash do token, expiração, usado (sim/não).
- **Sessão:** usuário, identificador, expiração.

## Critérios de aceitação

### Cadastro
- O cadastro exige nome, e-mail e senha.
- O e-mail deve ter formato válido e é normalizado (minúsculas, sem espaços nas pontas) antes de ser salvo.
- Se o e-mail já estiver cadastrado, o sistema informa o erro sem revelar dados da conta existente além do fato de o e-mail estar em uso.
- A senha deve ter no mínimo 8 caracteres, com pelo menos uma letra e um número.
- A senha é guardada somente como hash com sal (bcrypt ou argon2). Nunca em texto puro e nunca em logs.
- Todo cadastro novo recebe o papel `CLIENTE`. Não existe cadastro público de administrador.
- Após o cadastro, a pessoa já fica autenticada.

### Login e logout
- O login aceita e-mail e senha. Credenciais inválidas retornam a mesma mensagem genérica, sem indicar se o e-mail existe.
- Após 5 tentativas falhas seguidas para o mesmo e-mail em 15 minutos, novas tentativas são bloqueadas por 15 minutos.
- A sessão usa cookie `HttpOnly`, `Secure` e `SameSite=Lax`, com validade de 7 dias.
- O logout invalida a sessão no servidor e remove o cookie.
- Rotas que exigem login (pedidos, favoritos, checkout) redirecionam visitantes para o login e, depois de autenticar, devolvem a pessoa à página original.

### Carrinho do visitante
- Ao fazer login, o carrinho criado como visitante é mesclado ao carrinho da conta, somando quantidades iguais da mesma variante respeitando o limite de estoque.

### Recuperação de senha
- A pessoa informa o e-mail e recebe um link de redefinição. A resposta da tela é sempre a mesma, exista ou não conta com aquele e-mail.
- O link expira em 1 hora e só pode ser usado uma vez.
- Ao redefinir a senha, todas as sessões antigas da conta são invalidadas.
- Em ambiente de desenvolvimento e de apresentação, o e-mail é simulado: o conteúdo é registrado em uma tabela de notificações e exibido na tela de administração.

### Perfil e endereços
- A pessoa pode editar nome, telefone e CPF e trocar a senha informando a senha atual.
- O CPF, quando informado, é validado pelos dígitos verificadores.
- É possível cadastrar, editar e remover endereços. Ao informar o CEP, rua, bairro, cidade e UF são preenchidos por consulta interna a uma tabela de CEPs de exemplo.
- Existe sempre no máximo um endereço padrão por usuário. Remover o padrão promove o endereço mais recente.

### Papéis e autorização
- Rotas de administração retornam erro de acesso negado (403) para quem não é `ADMIN`.
- Um usuário só consegue ver e alterar os próprios endereços e pedidos. Tentar acessar dados de outro usuário retorna 404.
- Existe um comando de seed que cria uma conta `ADMIN` de demonstração, com senha lida de variável de ambiente.

### Segurança
- Todas as ações que alteram dados exigem proteção contra CSRF.
- Entradas são validadas no servidor, independentemente da validação do navegador.
- Mensagens de erro nunca expõem detalhes internos (stack trace, consultas SQL).

## Testes esperados

- Unidade: validação de e-mail, senha e CPF; hash e verificação de senha; limite de tentativas.
- Integração: fluxo completo de cadastro, login, logout e redefinição de senha com banco de teste; autorização por papel.
- Ponta a ponta: cadastrar, sair, entrar de novo e editar um endereço.

## Fora do escopo

- Login social (Google, Facebook)
- Autenticação em dois fatores
- Verificação de e-mail por confirmação
- Envio real de e-mails
