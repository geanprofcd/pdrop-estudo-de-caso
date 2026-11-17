# Conferir o arquivo pelo conteúdo, não pelo nome

## O problema
O PDrop aceita qualquer formato, porque cada cliente manda um tipo de documento. Mas a extensão não prova nada: um `.exe` renomeado para `.pdf` passa por qualquer filtro que só olha o nome, e um arquivo corrompido só é descoberto quando alguém tenta abrir.

## O que fiz
Antes de enviar, o arquivo é lido pelos primeiros bytes, a "assinatura" de cada formato. O sistema recusa:
- arquivo vazio;
- arquivo acima do limite configurado;
- extensão bloqueada pela empresa;
- executável disfarçado de outro formato;
- conteúdo que não confere com a extensão (corrompido ou renomeado errado).

O formato detectado aparece numa coluna própria na lista de arquivos. Fotos HEIC e vídeos MOV do iPhone são reconhecidos normalmente.

## O que eu levo
Validar no momento do envio custa milissegundos. Descobrir o problema três meses depois, quando alguém precisa do documento, custa caro.
