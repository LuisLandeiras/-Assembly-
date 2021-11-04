### Trabalhos em Assembly

## Programa para desenhar no ecrã uma linha reta, de um certo tamanho e numa certa cor
````asm
org 100h
ESC equ 1Bh ;código ASCII do ESCape (1Bh=27)
terminar equ 4ch
;------
;seleccionar modo de vídeo
mov ah, 4FH
mov al, 02
mov bx, 13h ;modo gráfico 13h, 320 colunas * 200 linhas
int 10h
;------
;parâmetros da reta
mov [cor], 5 ;cor
mov [col], 50 ;coluna inicial
mov [lin], 50 ;linha inicial
mov [tam], 100 ;tamanho da reta
;------
;sentido = 'h' - horizontal 'v' - vertical 
mov ah, 07h ;ler tecla do teclado (código ASCII) correspondente ao sentido
int 21h ; a função de leitura 07h coloca em AL o código da tecla lida
mov [sentido], al

cmp [sentido], ESC ;compara o código da tecla com código do ESC
je fim ;se é igual, o programa termina de imediato
;------
call reta ;desenha a reta
;parar o ecran
mov ah, 07h
int 21h
;------
;terminar
fim:
        mov ah, terminar
        int 21h

;****************************** SUBROTINAS *****************************

;---------------------------------------
;rotina para escrever a mensagem de texto
;---------------------------------------
msgerro:
        mov ah,40h
        mov bx,1
        mov cx,6
        mov dx, msge
        int 21h
ret
;---------------------------------------
;rotina que ativa um pixel no ecran
;---------------------------------------
pixel:
        mov ah,0ch ;pixel
        mov bh,0
        int 10h
ret

;-------------------------------------
;rotina que desanha o reta horizontal
;-------------------------------------

reta_h:
        ciclo:
         call pixel
         inc cx
         dec byte[tam]
        jnz ciclo
ret
;-------------------------------------
;rotina que desanha o reta vertical
;-------------------------------------

reta_v:
        ciclo2:
         call pixel
         inc dx
         dec byte[tam]
        jnz ciclo2
ret
;-------------------------------------
;rotina que desanha o reta obliqua
;-------------------------------------

reta_o:
        ciclo3:
         call pixel
         inc cx
         inc dx
         dec byte[tam]
        jnz ciclo3
ret
;-------------------------------------
;rotina que desanha o reta quadrado
;-------------------------------------

des_quadrado:
 mov[tam2], 50
 ciclo4:
   mov [tam], 50
   mov cx, valor
   ciclo5:
        mov ah, 0CH
        mov al,5 ;cor=magenta
        mov bh,0
        int 10h
        inc cx;chamada ao vídeo
        dec [tam]
   jnz ciclo5
   inc dx
   dec[tam2]
 jnz ciclo4
 mov dx, 80
 mov [valor], 130
ret

;---------------------------------------
;rotina que desenha uma reta numa certa cor, coluna, linha, tamanho e sentido
;---------------------------------------
reta:
        mov al, [cor] ;cor
        mov cx, [col] ;coluna
        mov dx, [lin] ;linha

        cmp [sentido], 'h' ;será reta horizontal?
        je horizontal
        cmp [sentido], 'v' ;será reta vertical?
        je vertical
        cmp [sentido], 'o' ;será reta obliqua?
        je obliqua
        cmp [sentido], 'q' ;será quadrado?
        je quadrado
        call msgerro ;sentido inválido (nenhum dos anteriores)
        jmp sair_funcao ;termina a subrotina pois o sentido é inválido

                horizontal: ;reta horizontal
                          call reta_h
                jmp  sair_funcao

                obliqua: ;reta obliqua
                       call reta_o
                jmp  sair_funcao

                quadrado:
                       call des_quadrado; chama desenha quadrado
                jmp  sair_funcao

                vertical: ;reta vertical
                          call reta_v ;passa à linha seguinte

        sair_funcao:
ret ;termina a rotina 'reta'

;---------------------------------------
;variáveis
;---------------------------------------
tam rb 1 ;tamanho da reta
cor rb 1 ;cor
lin rw 1 ;linha inicial
col rw 1 ;coluna inicial
sentido rb 1 ;sentido : 'v'-vertical 'h'-horizontal
msge db 'ERRO!', 10
tam2 rb 1
valor rb 1
````
