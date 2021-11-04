### Trabalhos em Assembly

##  TP
````asm
org 100h

abrir equ 3Dh
escrita equ 40h ;escrita
leitura equ 3Fh ;leitura
teclado equ 00 ;teclado
fechar equ 3EH
ecra equ 1
terminar equ 4ch


mov ah,abrir ; abre ficheiro
mov al,0 ; 0=leitura 1=escrita 2=leitura/escrita
mov dx,path ; path para ficheiro a abrir
int 21h
jc erro
mov [handle],ax
mov [max],0
mov [min],0
add byte[max], 48
add byte[min], 48
;--------------------------------
ler:
 mov dx, ''
 mov ah,leitura ; ler de ficheiro ou device ? AX=número de bytes lidos (ou código de erro)
 mov bx,[handle] ; handle do ficheiro
 mov cx,1 ; ler caráter a caráter
 mov dx,msg ; variável para guardar a leitura
 int 21h 
 cmp ax,0 ; compara ax com 0: se ax=0 ? não leu nada, logo está no fim de ficheiro
 ; (EOF-End Of File) e
 je sair ;nesse caso termina a leitura
 ;escrever no ecran o carácter lido do ficheiro
 cmp dx, 'Z'
 jle maius
 cmp dx, 'z'
 jle minus
 jmp ler

;------------------------------------------
sair:
 mov dx, msg2
 mov ah,escrita ; escrita
 mov bx,ecra ; no ecran
 mov cx,12 ;caráter a escrever
 int 21h
;---------------------
 mov dx, max
 mov ah,escrita ; escrita
 mov bx,ecra
 mov cx,1 ;caráter a escrever
 int 21h
;--------------------
 mov dx, msg3
 mov ah,escrita ; escrita
 mov bx,ecra
 mov cx,13;caráter a escrever
 int 21h
;-----------
 mov dx, min
 mov ah,escrita ; escrita
 mov bx,ecra
 mov cx,1 ;caráter a escrever
 int 21h

 mov ah,fechar
 mov bx,[handle] ; ficheiro a fechar
 int 21h ;
 jmp fim ;salta por cima da escrita da mensagem de erro
 ;escrever mensagem de erro
;---------------------------------------------------
erro: mov ah,escrita ; escrever mensagem de erro
 mov bx,ecra ; no ecran
 mov cx,20 ; nº de caracteres a escrever
 mov dx,msgerro ; mensagem de erro
 int 21h
;-----------------------------------
;Terminar programa
fim:
mov ah, terminar
int 21h

maius:
 cmp dx, 'A'
 jge soma1
 jmp ler

minus:
 cmp dx, 'a'
 jge soma2
 jmp ler

soma1:
 add byte[max],1
 jmp ler

soma2:
 add byte[min],1
 jmp ler

path db 'a.txt',0 ;formato AXCIIZ
msgerro db 'Ficheiro Inexistente'
handle rw 1 ;handle do ficheiro
msg rb 1 ;variável para armazenar o caráter lido do ficheiro
max rb 1
min rb 1
msg2 db 'maiusculas='
msg3 db ' minusculas= '
````