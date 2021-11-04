### Trabalhos de Assembly

## Hello World com \n
````asm
org 100h
;---------------------------------------------------

escrita equ 40h ;40h=função de escrita
ecran equ 1 ;1=ecran
leitura equ 07h ;leitura
retorno equ 4ch ;retorno ao SO
numcaracteres equ 4
executarcodigo equ 21h
;---------------------------------------------------
mov [cont], 15 ;inicializar a variável de controlo do ciclo – atenção aos parentesis retos
 ;os parentesis retos significam que é o conteúdo da variável que é modificado

ciclo: ;label para marcar o início do ciclo
 mov ah, escrita
 mov bx, ecran
 mov cx, numcaracteres
 mov dx, msg ;o registo dx contém o endereço da variável com a mensagem (e não a própria mensagem), 
 ;ou seja, é um apontador para a mensagem
 int executarcodigo
 dec [cont] ;em cada execução do ciclo decrementa de uma unidade o conteúdo da variável ‘cont’
jnz ciclo ; continua enquanto não atingir o fim
mov ah, leitura ;aguarda que se carregue numa tecla
int executarcodigo
mov ah, retorno ;retornar ao sistema operativo
int executarcodigo
;---------------------------------------------------

msg db "UBI", 10 ;definição da mensagem – o caráter 10 (LF=Line Feed, ver tabela ASCII) vai provocar 
 ;a mudança de linha

cont rb 1 ; reserva espaço (1 byte) para a variável ‘cont’, permitindo que a variável posso tomar valores 
 ;desde 0 até 255

;---------------------------------------------------
````