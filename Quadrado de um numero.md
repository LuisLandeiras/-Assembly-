### Trabalhos em Assembly

## Calcular o quadrado de um número dado por o utilizador
````asm
org 100h
escrita equ 40h ;escrita
ecran equ 01 ;ecran
leitura equ 3Fh ;leitura
teclado equ 00 ;teclado
CR equ 0Dh ;Carriage Return
LF equ 0Ah ;Line Feed
terminar equ 4Ch ;terminar
;mensagem a pedir um número
mov ah,escrita
mov bx,ecran
mov cx,8
mov dx,pergunta
int 21h
;ler o valor do teclado (um só algarismo ou seja um só byte, 0..9)
mov ah,leitura ;ler
mov bx,teclado ;do teclado
mov cx,01 ;1 byte a ler
mov dx,valor ;aponta para a variável que guarda os dados lidos
int 21h
;consumir o <CR> + <LF> inseridos pelo teclado - nada vai ser feito com eles ; o que acontece se esta 
; leitura não for feita?
mov ah,leitura
mov bx,teclado
mov cx,2 ;2 bytes a ler, <CR> + <LF>
mov dx,crlf
int 21h

; Imaginar que deram o nº: 7----> 55
; NREAL = 55 - 48 ---> 7
;obter o algarismo lido a partir do seu código ASCII ? para obter o algarismo há que subtrair 48, 
; correspondente aos códigos na tabela ASCII
sub [valor],48 ;obtém o algarismo a partir do seu código ASCII

; Neste momento temos o valor que é visto no ecran
;mov al,[valor]
;mov bl,[valor]
;mul bl ; ax=al*al=valor*valor - MELHOR OPÇÃO

mov al,[valor]
mul al

mov [quadrado],ax ;guarda o quadrado do valor introduzido
; Em Quadrado temos o valor 49
; Que temos de escrever no ecran! 49 |_ 10----> AL=4  ; AH=9
;a partir daqui trata-se de escrever no ecran um número com dois algarismos, o que já foi feito nos
;exemplos anteriores
;separa as dezenas das unidades (dividir por 10 e obter quociente/dezenas=AL e resto/unidades=AH)



fim:
mov ah, terminar
int 21h
pergunta db 'Valor ? '
resultado db 'Resultado = '
mudalinha db CR,LF

valor rb 1 ;valor a calcular o quadrado
crlf rb 2 ;para ler o CR+LF do teclado
quadrado rw 1 ;vai conter o quadrado de 'valor' (rw - reserve word, neste caso reserva 1 word)
dezenas rb 1 ;algarismo das dezenas
unidades rb 1 ;algarismo das unidades

;NOTA:
;Multiplicar 2 Valores;
;1º coloco em al o 1º valor;
;2º colocar em bl o 2ª valor;
;3º Efectuar a multiplicação mul bl; // mult al*bl e o resultado fica em ax
;mov al,[valor_1]
;mov bl,[valor_2]
;mul bl ;
;---------------------------
;Dividir 2 Valores;

;---------------------------
;Soma de Valores
;add .....
;---------------------------
;Diferença Valores
;sub .....
;---------------------------
````