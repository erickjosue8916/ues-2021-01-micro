## Trabajando con la pila

```bash
.386
.model flat,stdcall
.stack 4096
	ExitProcess proto,dwExitCode:dword
.data
	arreglo byte "MIP115",0
	tamanio = ($ -arreglo) -1
.code
main proc
	mov ecx,tamanio
	mov esi,0
	L1:
		movzx eax,arreglo[esi]
		push eax
		inc esi
		loop L1
		mov ecx,tamanio
		mov esi,0
	L2:
		pop  eax
		mov arreglo[esi],al
		inc esi
		loop L2
	Invoke ExitProcess,0
main endp
end main
```

## Procedimientos

```
INCLUDE MIP115.inc
.data
arrayD     DWORD 1000h,2000h,3000h
prompt1    BYTE "INGRESE UN ENTERO DE 32 BITS CON SIGNO: ",0
valor   DWORD ?
.code
main PROC
	mov	eax,yellow + (blue * 16)
	call	SetTextColor
	call	Clrscr
	mov	esi,OFFSET arrayD
	mov	ecx,LENGTHOF arrayD
	mov	ebx,TYPE arrayD
	call	DumpMem
	call	Crlf				;NUEVA LINEA


; PREGUNTA EL NUMERO.
	mov	edx,OFFSET prompt1
	call	WriteString
	call	ReadInt			; LEE EL ENTEROI
	mov	valor,eax		; GUARDA EN LA VARIABLE

	call	Crlf				;NUEVA LINEA
	; Muestra en diversas bases
	call	Crlf
	call	WriteInt
	call	Crlf
	call	WriteHex
	call	Crlf
	call	WriteBin
	call	Crlf
	call	WaitMsg


; retorna el color en consola
	mov	eax,lightGray + (black * 16)
	call	SetTextColor
	call	Clrscr
	exit
main ENDP
END main
```

## EJERCICIO 3: ENCRIPTAR

```
INCLUDE MIP115.inc
LETRA = 239     		; LETRAS PERIMITIDAD 1-255
EsMemoMAX = 128     	; TAMAÑO DEL BUFFER

.data
sCursor  BYTE  "INGRESE UN TEXTO: ",0
sTextEnctipt BYTE  "TEXTO CRIFRADO:          ",0
sTextDecrypt BYTE  "DESENCRIPTADO:            ",0
EsMemo   BYTE   EsMemoMAX+1 DUP(0)
EsMemoTam  DWORD  ?

.code
main PROC
	call	EntradadeString
	call	TrasladaBuffer
	mov	edx,OFFSET sTextEnctipt	;
	call	MostrarMensaje
	call	TrasladaBuffer

	exit
main ENDP

;-----------------------------------------------------
EntradadeString PROC
; Recibe: nothing
; Retorna: nothing
;-----------------------------------------------------
	pushad
	mov	edx,OFFSET sCursor	; muestra el promt
	call	WriteString
	mov	ecx,EsMemoMAX		;maximo de caracteres
	mov	edx,OFFSET EsMemo   ; coloca en la direccion del buffer
	call	ReadString         	; lee el string
	mov	EsMemoTam,eax        	; guarda el tamanio
	call	Crlf
	popad
	ret
EntradadeString ENDP

;-----------------------------------------------------
MostrarMensaje PROC
;
; Recibe: EDX direccion del mensaje
; Retorna:  Nada
;-----------------------------------------------------
	pushad
	call	WriteString
	mov	edx,OFFSET EsMemo
	call	WriteString
	call	Crlf
	call	Crlf
	popad
	ret
MostrarMensaje ENDP

;-----------------------------------------------------
TrasladaBuffer PROC
;
; Recibe: nothing
; Retorna: nothing
;-----------------------------------------------------
	pushad
	mov	ecx,EsMemoTam		; contador
	mov	esi,0			; indice a  0 en el buffer
L1:
	xor	EsMemo[esi],LETRA	; traslado de bytes
	inc	esi				; incrementa a siguiente byte
	loop	L1

	popad
	ret
TrasladaBuffer ENDP
END main

```

# Resolucion

```
INCLUDE MIP115.inc
LETRA = 239     		; LETRAS PERIMITIDAD 1-255
EsMemoMAX = 128     	; TAMAÑO DEL BUFFER

.data
sCursor  BYTE  "INGRESE UN TEXTO: ",0
sTextEnctipt BYTE  "TEXTO CRIFRADO:          ",0
sTextDecrypt BYTE  "DESENCRIPTADO:            ",0
menu db "Seleccione el tipo de encriptado",0dh,0ah,
		 "1 - AND",0dh,0ah,
		"1+ - OR",0dh,0ah,0
EsMemo   BYTE   EsMemoMAX+1 DUP(0)
EsMemoTam  DWORD  ?
EncryptSelected BYTE ?

.code
main PROC
	mov edx, OFFSET menu
	call MostrarMensaje
	call readInt
	mov EncryptSelected, al
	call	EntradadeString

	mov al,1
	cmp al,EncryptSelected
	je andEncrypt
	jmp orEncrypt
	andEncrypt:
		call EncryptWithAnd
		jmp showEncrypted
	orEncrypt:
		call EncryptWithOr
		jmp showEncrypted
	showEncrypted:
		mov	edx,OFFSET sTextEnctipt	;
		call	MostrarMensaje
		exit
main ENDP

;-----------------------------------------------------
EntradadeString PROC
; Recibe: nothing
; Retorna: nothing
;-----------------------------------------------------
	pushad
	mov	edx,OFFSET sCursor	; muestra el promt
	call	WriteString
	mov	ecx,EsMemoMAX		;maximo de caracteres
	mov	edx,OFFSET EsMemo   ; coloca en la direccion del buffer
	call	ReadString         	; lee el string
	mov	EsMemoTam,eax        	; guarda el tamanio
	call	Crlf
	popad
	ret
EntradadeString ENDP

;-----------------------------------------------------
MostrarMenu PROC
; Recibe: nothing
; Retorna: nothing
;-----------------------------------------------------
	pushad
	mov	edx,OFFSET sCursor	; muestra el promt
	call	WriteString
	mov	ecx,EsMemoMAX		;maximo de caracteres
	mov	edx,OFFSET EsMemo   ; coloca en la direccion del buffer
	call	ReadString         	; lee el string
	mov	EsMemoTam,eax        	; guarda el tamanio
	call	Crlf
	popad
	ret
MostrarMenu ENDP

;-----------------------------------------------------
MostrarMensaje PROC
;
; Recibe: EDX direccion del mensaje
; Retorna:  Nada
;-----------------------------------------------------
	pushad
	call	WriteString
	mov	edx,OFFSET EsMemo
	call	WriteString
	call	Crlf
	call	Crlf
	popad
	ret
MostrarMensaje ENDP


EncryptWithAnd PROC
;
; Recibe: nothing
; Retorna: nothing
;-----------------------------------------------------
		pushad
	mov	ecx,EsMemoTam		; contador
	mov	esi,0			; indice a  0 en el buffer
L1:

	and	EsMemo[esi],LETRA	; traslado de bytes
	inc	esi				; incrementa a siguiente byte
	loop	L1

	popad
	ret
EncryptWithAnd ENDP

EncryptWithOr PROC
;
; Recibe: nothing
; Retorna: nothing
;-----------------------------------------------------
		pushad
	mov	ecx,EsMemoTam		; contador
	mov	esi,0			; indice a  0 en el buffer
L1:
	or	EsMemo[esi],LETRA	; traslado de bytes
	inc	esi				; incrementa a siguiente byte
	loop	L1

	popad
	ret
EncryptWithOr ENDP
END main
```
