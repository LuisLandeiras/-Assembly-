### Trabalhos em Assembly

## Hello World feito com ciclos
````asm
org 100h

escrita equ 40h
ecran equ 1 

;--------------------------------
;executa o ciclo
mov [cont], 5
ciclo1:
        call escrever ;chama a rotina 'escrever'
        dec [cont]
jnz ciclo1
;--------------------------------
mov [cont], 3
ciclo2:
        call escrever ;chama a rotina 'escrever'
        dec [cont]
jnz ciclo2
;--------------------------------
;aguarda que se carregue numa tecla
call teclado
;retorna ao sistema operativo
call sair
;--------------------------------

;NOTA: o local ideal para inserir a subrotina no programa é depois das 
; instruções de retorno ao sistema operativo, para garantir que a rotina 
; nunca é executada a não ser após ter sido chamada (call).
;subrotina para escrever ‘UBI’
;-----------------------------------------
escrever:
        mov ah, escrita
        mov bx, ecran
        mov cx, 4
        mov dx, msg
        int 21h
ret ; NÃO ESQUECER ISTO!; Se esquecerem.... Azar!!!!
;-----------------------------------------
teclado:
        mov ah, 07h
        int 21h
ret
;-----------------------------------------
sair:
       mov ah, 4ch
       int 21h
ret
;-----------------------------------------
;-----------------------------------------
msg db "UBI", 10 
cont rb 1
````