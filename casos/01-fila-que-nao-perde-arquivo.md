# Uma fila de envio que não perde arquivo

## O problema
A promessa do produto é "mandou, chegou". Na prática, o arquivo sai de um celular com sinal ruim, de um computador que desliga ou de um navegador que alguém fecha no meio do envio. Uma fila comum, só na memória, perde tudo nesses casos.

## O que fiz
1. **Gravar antes de tudo.** O arquivo entra na fila no instante em que é solto, antes da verificação e do envio.
2. **Três registros no aparelho.** IndexedDB (gravação conferida lendo de volta), uma cópia do conteúdo no armazenamento privado do navegador (OPFS) e um índice no localStorage. Ao reabrir, os três são cruzados e o que faltar num é recuperado do outro.
3. **Impressão digital.** Um SHA-256 é calculado na entrada e conferido antes do envio. Se a cópia estiver corrompida, entra a de segurança; se as duas falharem, o arquivo não é enviado errado e a pessoa é avisada.
4. **Envio retomável.** Upload pelo protocolo TUS, em pedaços de 6 MB. Depois de uma queda, o envio continua do último pedaço, não do zero.
5. **Registro no servidor.** Cada arquivo esperado fica registrado com o aparelho de origem. Um painel mostra, de qualquer aparelho, o que ainda não chegou. O banco marca como concluído quando o arquivo aparece no Storage.
6. **Uma aba por vez.** Com Web Locks, só uma aba envia, e nada sobe em dobro.
7. **Pedido de armazenamento persistente**, para o navegador não apagar a fila por falta de espaço.

## Limites que deixei escritos
- A fila só anda com o PDrop aberto em algum lugar do aparelho.
- No iPhone, apps web não enviam em segundo plano.
- Arquivos acima de 200 MB não recebem impressão digital, para não estourar a memória do celular.

## O que eu levo
"Funciona offline" é fácil de prometer. O trabalho está em decidir o que acontece em cada falha e em mostrar para a pessoa, sem jargão, o que ficou pendente.
