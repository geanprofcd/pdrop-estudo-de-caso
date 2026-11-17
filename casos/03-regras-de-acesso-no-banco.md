# Quem vê o quê: regras de acesso no banco

## O problema
Num portal de arquivos de clientes, o pior erro possível é uma pessoa ver o arquivo de outra. Regra de acesso só na tela não basta: basta uma chamada direta à API para contorná-la.

## O que fiz
As regras ficam no PostgreSQL, com Row Level Security, e valem para qualquer caminho de acesso:

| | Administrador | Gerente | Comum |
|---|---|---|---|
| Arquivos que vê | todos | todos, menos os do administrador | os próprios e os recebidos no chat |
| Pessoas que vê | todas | todas, menos o administrador | gerentes e colegas de grupo |
| Convida pessoas | sim | como Comum | não |
| Muda papéis, senhas e exclui contas | sim | não | não |

Outras escolhas:
- Cada arquivo fica numa pasta própria de quem enviou no Storage.
- Quem recebe um anexo no chat pode abrir aquele arquivo, e só ele.
- O administrador fica invisível para os demais, a não ser para quem ele mesmo escrever.
- Não existe cadastro público: só convite com link que vale 7 dias.
- Funções auxiliares tiveram o acesso anônimo revogado numa migration própria.
- Verificação em duas etapas (TOTP), com aprovação do primeiro cadastro e pedido de desbloqueio.

## O que eu levo
Quando a regra mora no banco, a interface pode errar sem vazar dado.
