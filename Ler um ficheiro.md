### Trabalhos de Assembly

## Ler um ficheiro
````asm
org 100h
 abrir equ 3Dh
 criar equ 3Ch
 fechar equ 3Eh
 escrever equ 40h
 ecra equ 1
 leitura equ 3Fh
 teclado equ 0
 CR equ 0Dh
 LF equ 0Ah
;-------------
 mov ah,abrir ; abre ficheiro
 mov al,0 ; 0=leitura 1=escrita 2=leitura/escrita
 mov dx,path ; path para ficheiro a abrir
 int 21h 
 jc erro ; jc=”jump if carry” ? se c=1 (ou seja, c?0) então houve erro na abertura, vai 
 ;escrever uma mensagem de erro e terminar o programa (não é preciso fechar o 
 ;ficheiro pois ele não chegou a ser aberto devido ao erro)

mov [handle],ax ; se chegou aqui não houve erro, logo guarda o handle válido do ficheiro


 mov ah,criar
 mov cx,0 ; 0=ficheiro normal (podia ser escondido, sistema, etc)
 mov dx,path2 ; caminho para o ficheiro a criar (formato ASCIIZ)
 int 21h ; cria o ficheiro, atribuindo-lhe um handle que fica no registo ax
 mov [handle2],ax ; guarda o handle para posterior uso

;-------------
ler: mov ah,leitura ; ler de ficheiro ou device ? AX=número de bytes lidos (ou código de erro)
 mov bx,[handle] ; handle do ficheiro
 mov cx,1 ; ler caráter a caráter
 mov dx,msg ; variável para guardar a leitura
 int 21h 
 cmp ax,0 ; compara ax com 0: se ax=0 ? não leu nada, logo está no fim de ficheiro
 ; (EOF-End Of File) e
 je sair ;nesse caso termina a leitura


 ;escrever no ecran o carácter lido do ficheiro
 mov ah,escrever ; escrita
 mov bx,ecra ; no ecran
 mov cx,1 ; 1 caráter a escrever
 int 21h
 mov dx,msg ; caráter que foi lido do ficheiro
 mov ah,escrever
 mov bx,[handle2]
 int 21h
 jmp ler ;salto incondicional para “ler”, ou seja, vai tentar ler mais um caráter


 ;quando chega aqui é porque todo o ficheiro foi lido
;-------------
sair:
 mov ah,leitura
 int 21h
 mov cx,66 ; nº de carateres a escrever
 mov dx,msg ; mensagem a escrever
 int 21h ; escreve no ficheiro
 mov ah, fechar ; fecha ficheiro
 mov bx,[handle] ; indica qual o ficheiro a fechar
 int 21h
 mov ah,fechar
 mov bx,[handle2] ; ficheiro a fechar
 int 21h ;
 jmp fim ;salta por cima da escrita da mensagem de erro
 ;escrever mensagem de erro

;-------------
erro: mov ah,escrever ; escrever mensagem de erro
 mov bx,ecra ; no ecran
 mov cx,20 ; nº de caracteres a escrever
 mov dx,msgerro ; mensagem de erro
 int 21h

;-------------
fim:
 mov ah,leitura
 int 21h
 mov ah,leitura
 int 21h
 mov ah,4ch ; termina o programa
 int 21h

;-----DADOS-----------
path db 'teste.txt',0 ;formato AXCIIZ
path2 db 'teste2.txt',0 ;formato AXCIIZ
msgerro db 'Ficheiro Inexistente'
handle rw 1 ;handle do ficheiro
handle2 rw 1 ;handle do ficheiro
msg rb 1 ;variável para armazenar o caráter lido do ficheiro
````