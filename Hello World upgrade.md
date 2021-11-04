### Trabalhos em Assembly

## Não sei, mas sei que é um Hello World melhor
````asm
org 100h ; é necessário colocar sempre isto!

 ;bloco de instruções para escrever no ecran (carregar ah, bx, cx e dx com os valores apropriados)
mov ah, 40h ;função de escrita
mov bx, 1 ;bx = 1 (1=handle do ecran) 
mov cx, 8 ;cx = 8 (número de caracteres a escrever )
mov dx, msg ;dx = endereço da variável "msg" (dx aponta para os dados a escrever)
int 21h ;provoca a execução da acção (escrita)

mov ah, 40h ;função de escrita
mov bx, 1 ;bx = 1 (1=handle do ecran) 
mov cx, 8 ;cx = 8 (número de caracteres a escrever )
mov dx, msg ;dx = endereço da variável "msg" (dx aponta para os dados a escrever)
int 21h ;provoca a execução da acção (escrita)

 ;pára o programa até que o utilizador carregue numa tecla qualquer para dar tempo que se possa ver a 
 ;mensagem anterior (neste exemplo a tecla lida é ignorada)
mov ah, 3fh ;ah = 3fh (função de leitura, teclado)
int 21h ;provoca a execução da função(leitura)

;terminar o programa corretamente com retorno ao Sistema Operativo 
mov ah, 4ch ;ah = 4ch (função de retorno ao Sistema Operativo)
int 21h ;provoca a execução da função (fechar)

;definição de variáveis 
msg db 'Ola UBI',10 ;define a variável "msg" (db – define byte)
````