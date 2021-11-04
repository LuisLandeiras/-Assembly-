### Trabalhos em Assembly

## Hello World
````asm
org 100h

mov ah, 40h
mov bx, 1
mov cx, 9
mov dx, msg
int 21h

mov ah, 07h
int 21h

mov ah, 4ch
int 21h

msg db "Hello"~
````