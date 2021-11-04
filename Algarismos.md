### Trabalhos em Assembly

## Indica as centenas, dezenas e unidades de um número
````asm
org 100h

escrita equ 40h ;40h=função de escrita
ecran equ 1 ;1=ecran
leitura equ 07h ;leitura
retorno equ 4ch ;retorno ao SO
mov [cont], 120

ciclo:
;separar a variável de controlo em dezenas/unidades
;mov ax,word[cont] ? se fosse usado iria provocar ‘divide overflow’ (*)

     mov ah,0 ;coloca a zero o registo ah (parte alta do registo ax)
     mov al,[cont] ;ax=dividendo
     mov dl,10 ;dl=divisor
     div dl ;al=quociente ah=resto
     mov [dezenas],al ;guarda o algarismo das dezenas
     mov [unidades],ah ;guarda o algarismo das unidades
     mov ah,0
     mov al,[cont] ;ax=dividendo
     mov dl,10 ;dl=divisor
     div dl ;al=quociente ah=resto
     mov [centenas],al


 ;escrever algarismo das centenas
     add byte[centenas],48 ;converte para ASCII
     mov ah, escrita
     mov bx, ecran
     mov cx, 1
     mov dx, centenas ;endereço do algarismo das dezenas
     int 21h
     sub byte[centenas],48 ;repõe valor ? será mesmo precisa? (**)

     ;escrever algarismo das dezenas
     add byte[dezenas],48 ;converte para ASCII
     mov ah, escrita
     mov bx, ecran
     mov cx, 1
     mov dx, dezenas ;endereço do algarismo das dezenas
     int 21h

     ;algarismo das unidades
     add byte[unidades],48 ;converte para ASCII
     mov ah, escrita
     mov bx, ecran
     mov cx, 1
     mov dx, unidades ;endereço do algarismo das unidades
     int 21h
     sub byte[unidades],48 ; repõe valor ? será mesmo precisa? (**)

     ;mudar de linha
     mov ah, escrita
     mov bx, ecran
     mov cx, 01
     mov dx,novalinha
     int 21h

     dec [cont]
jnz ciclo
;para o programa para
mov ah, leitura

int 21h
;terminar (retorno ao SO)
mov ah, retorno
int 21h

;dados
cont rb 1
centenas rb 1
dezenas rb 1
unidades rb 1
novalinha db 10 ;para mudar de linha~
````