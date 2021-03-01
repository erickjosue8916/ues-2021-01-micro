# Tarea 2  / 02-03-2021

## Indicar los tipos de direccionamiento
```
INCLUDE MIP115.inc
.data
str1 BYTE "EJEMPLO, En color",0dh,0ah,0


.code
main PROC
  mov ax,yellow + (blue * 16)
  call SetTextColor ; (1) direccionamiento _____?
  
  mov edx,OFFSET str1
  call WriteString (2) direccionamiento _____?
  
  call GetTextColor
  call DumpRegs
  
  exit
  
main ENDP
END main

```
1. Utiliza direccionamiento 
2. Utiliza Direccionamiento indirecto ya que se asume que el contenido de que se imprime en pantalla debe estar en el registro edx

## Ejercicios 4.4.5
1.   (Verdadero/Falso): cualquier registro de propósito general de 16 bits puede usarse como operando indirecto.  
2.  (Verdadero/Falso): cualquier registro de propósito general de 32 bits puede usarse como operando indirecto.  
3.   (Verdadero/Falso): por lo general, el registro BX se reserva para direccionar la pila.  
4.   (Verdadero/Falso): un error de protección general ocurre en modo de direccionamiento real, cuando el subín-dice de un arreglo está fuera de rango.  
5.   (Verdadero/Falso): la siguiente instrucción es inválida: inc [esi]  
6.   (Verdadero/Falso): el siguiente es un operando indexado: arreglo[esi]
---
#### Use las siguientes definiciones de datos para el resto de las preguntas en esta sección:

- misBytes     BYTE  10h,20h,30h,40h
- misPalabras  WORD  8Ah,3Bh,72h,44h,66h
- misDobles      DWORD 1,2,3,4,5
- miApuntador    DWORD misDobles

7.   Llene los valores de los registros requeridos en el lado derecho de la siguiente secuencia de instrucciones:
```
mov  esi,OFFSET misBytes
mov  al,[esi]                      ; a. AL = 
mov  al,[esi+3]                    ; b. AL =
mov  esi,OFFSET misPalabras + 2
mov  ax,[esi]                      ; c. AX = 
mov  edi,8mov  edx,[misDobles + edi]         ; d. EDX =
mov  edx,misDobles[edi]            ; e. EDX =
mov  ebx,miApuntadormov  eax,[ebx + 4]                 ; f. EAX =  
```

8.   Llene los valores de los registros requeridos en el lado derecho de la siguiente secuencia de instrucciones:
```
mov  esi,OFFSET misBytes
mov  ax,WORD PTR [esi]             ; a. AX =
mov  eax,DWORD PTR misPalabras     ; b. EAX =
mov  esi,miApuntadormov  ax,WORD PTR [esi+2]           ; c. AX =
mov  ax,WORD PTR [esi+6]           ; d. AX =
mov  ax,WORD PTR [esi-4]           ; e. AX =
```

## Ejercicos 4.5.5
1. (Verdadero/Falso): una instrucción JMP sólo puede saltar a una etiqueta dentro del procedimiento actual, a menos que esa etiqueta se haya designado como global.  
2.   (Verdadero/Falso): JMP es una instrucción de transferencia condicional.  
3.  Si ECX se inicializa con cero antes de empezar un ciclo, ¿cuántas veces se repetirá la instrucción LOOP? (Suponga que no hay instrucciones que modifi quen a ECX dentro del ciclo).  
4.   (Verdadero/Falso):  la  instrucción  LOOP  primero  verifi  ca  si  ECX  es  mayor  que  cero;  después  decrementa  ECX y salta a la etiqueta de destino.  
5.   (Verdadero/Falso): la instrucción LOOP hace lo siguiente: decrementa ECX; después, si ECX es mayor que cero, la instrucción salta a la etiqueta de destino.  
6.   En el modo de direccionamiento real, ¿qué registro utiliza la instrucción LOOP como contador?  
7.   En el modo de direccionamiento real, ¿qué registro utiliza la instrucción LOOPD como contador?  
8.   (Verdadero/Falso): el destino de una instrucción LOOP debe estar a no más de 256 bytes de distancia de la ubicación actual.  
9.  Reto: ¿cuál será el valor fi nal de EAX en este ejemplo?     
```
    mov   eax,0     
    mov   ecx,10                  ; contador del ciclo exterior
  L1:     
    mov   eax,3     
    mov   ecx,5                   ; contador de ciclo interior
  L2:
    add  eax,5     
    loop L2                       ; repite el ciclo interior     
    loop L1                       ; repite el ciclo exterior
```
10.   Revise el código de la pregunta anterior, para que el contador del ciclo exterior no se borre cuando empiece el ciclo interior.
