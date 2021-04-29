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
