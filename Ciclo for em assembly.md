### Trabalhos em Assembly

## Basicamente é um ciclo for mas em assembly
### Objetivo:
- for (int i = 5; i >= 0; i--){ 
      printf("%d\n",i);
    }
    
````asm
;for (int i = 5; i >= 0; i--)
;    printf("%d\n",i);
org 100h

escrita equ 40h ;40h=função de escrita
ecran equ 1 ;1=ecran
leitura equ 07h ;leitura
retorno equ 4ch ;retorno ao SO
numcaracteres equ 1
executarcodigo equ 21h

;--------------------------------------------
mov [cont], 5 ;ciclo vai executar 5 vezes
ciclo_for:
 add [cont], 48   ; vamos buscar o cod ASCII do valor que está em cont
 mov ah, escrita
 mov bx, ecran
 mov cx, numcaracteres
 mov dx, cont ;o registo dx contém o endereço da variável com a mensagem, que neste caso é a variável 
 ;de controlo do ciclo;
 ; REGRA: o registo dx deve apontar sempre para o endereço da variável e não para o seu conteúdo, 
 ;logo não são usados os parenteses retos;
 int executarcodigo
 sub [cont], 48 ; repor o valor da variavel de controlo
 dec [cont]
jnz ciclo_for
;--------------------------------------------
mov ah, leitura
int executarcodigo
;--------------------------------------------
mov ah, retorno
int executarcodigo
;--------------------------------------------
cont rb 1
;--------------------------------------------
````