# Sopa de letras

required files:

- instrucciones
- nivel1
- nivel2
- nivel3

```asm
INCLUDE  MIP115.INC
INCLUDE MACROS.INC

TAMANIO_BUFFER=1000

.data
	cadena1 BYTE "INGRESE UNA PALABRA",0
	entrada BYTE 10 DUP(?)
	puntaje byte 0
	vidas BYTE 3
	verificar BYTE 1
	lista_palabra BYTE "CPU","ASM","ALU","AX","FETCH",0
	arreglo_lista1 BYTE 5 DUP(1)
	archivo_lista1 BYTE "nivel1.txt",0
	archivo_lista4 BYTE "instrucciones.txt",0
	caracter BYTE 4 DUP("0")
	nombrearchivo BYTE "puntaje.txt",0
	manejadorArchivo HANDLE ?
	buffer BYTE TAMANIO_BUFFER DUP(0)
	stringLength DWORD ?
.code

main proc
	Nuevamente:
		call clrscr
		mWrite<"1-Jugar",0dh,0ah,"2-Instrucciones",0dh,0ah,"3-Cambiar Color",0dh,0ah>
		mWrite<"4-Puntaje",0dh,0ah,"5-Salir",0dh,0ah,0dh,0ah>
		mWrite <"Ingrese una opcion:">
		mov eax,0
		call readdec

		cmp al,1
		jne siguiente
		call jugar
		jmp salir

	siguiente:

		cmp al,2
		jne siguiente1
		call instrucciones
		jmp salir

	siguiente1:

		cmp al,3
		jne siguiente2
		call configurar
		jmp salir

	siguiente2:
		cmp al,4
		jne siguiente3
		mWrite<"puntaje:",0>
		mov edx,offset nombrearchivo
		 call leer_archivo
		 call crlf
		jmp salir

	 siguiente3:
		cmp al,5
		jne siguiente4
		 mov verificar,0
		 jmp Salir1

	siguiente4:
		mWrite<"ingrese un digito valido",0dh,0ah>
		mov eax,500
		call delay
		jmp Nuevamente

	Salir:
		call readdec
		cmp verificar,0
		jne  Nuevamente
	Salir1:
		exit
main endp


Jugar PROC
call clrscr
call nivel1

ret
Jugar endp
;---------------------------------------------------------------------
;--------------------------------------------------------------------
nivel1 PROC

	mientrasCiclo:
		cmp vidas,0
		je Salir
		mWrite<"vidas:",0>
		movzx eax,vidas
		call writedec
		mWrite<"     Puntaje:",0>
		movzx  eax,puntaje
		call WriteDec
		call crlf
		call crlf

		mov edx,offset archivo_lista1
		call leer_archivo
		call crlf

		mov edx,offset cadena1
		call writestring

		mov  edx,OFFSET entrada
		mov  ecx,5
		call ReadString

		mov al,arreglo_lista1[0]
		cmp al,1
		jne sino1

		cld
		mov esi,offset entrada
		mov edi,offset lista_palabra[0]
		mov ecx,3
		repe cmpsb
		jnZ sino1
		mWrite <"Se encontro la palabra ingresada",0dh,0ah>
		inc puntaje
		mov arreglo_lista1[0],0
		jmp siguiente
	sino1:
		mov al,arreglo_lista1[1]
		cmp al,1
		jne sino2
		cld
		mov esi,offset entrada
		mov edi,offset lista_palabra[3]
		mov ecx,3
		repe cmpsb
		jnz sino2
		mWrite <"Se encontro la palabra ingresada",0dh,0ah>
		inc puntaje
		mov arreglo_lista1[1],0
		jmp siguiente
	sino2:
		mov al,arreglo_lista1[2]
		cmp al,1
		jne sino3
		cld
		mov esi,offset entrada
		mov edi,offset lista_palabra[6]
		mov ecx,3
		repe cmpsb
		jnz sino3
		mWrite <"Se encontro la palabra ingresada",0dh,0ah>
		inc puntaje
		mov arreglo_lista1[2],0
		jmp siguiente
	sino3:
		mov al,arreglo_lista1[3]
		cmp al,1
		jne sino4
		cld
		mov esi,offset entrada
		mov edi,offset lista_palabra[9]
		mov ecx,2
		repe cmpsb
		jnz sino4
		 mWrite <"Se encontro la palabra ingresada",0dh,0ah>
		inc puntaje
		mov arreglo_lista1[3],0
		jmp siguiente
	sino4:
		mov al,arreglo_lista1[4]
		cmp al,1
		jne sino5
		cld
		mov esi,offset entrada
		mov edi,offset lista_palabra[11]
		mov ecx,5
		repe cmpsb
		jnz sino5
		mWrite <"Se encontro la palabra ingresada",0dh,0ah>
		inc puntaje
		mov arreglo_lista1[4],0
		jmp siguiente
	sino5:
		mWrite<"No se encontro la palabra ingresada!",0dh,0ah>
		dec vidas
	siguiente:
		MOV EAX,500
		CALL delay
		call clrscr
		MOV AL,puntaje
		cmp al,5
		jl mientrasCiclo

	Salir:
		ret
nivel1 endp
;---------------------------------------------------------------------
;--------------------------------------------------------------------
configurar PROC

	mWrite<" 1-Cambie color",0dh,0ah>
	Nuevamente:
		mWrite<"Ingrese una opción:",0>

		mov eax,0
		call readdec

		cmp al,1
		jne siguiente
		call cambiarcolor
		jmp siguiente1
	siguiente:
		mWrite<"Ingreso un número invalido",0dh,0ah>
		mov eax,500
		call delay
		jmp Nuevamente

	siguiente1:
		ret
configurar endp



;---------------------------------------------------------------------
;--------------------------------------------------------------------

cambiarcolor PROC

	call clrscr
	mWrite<" 1-Azul",0dh,0ah," 2-Blanco",0dh,0ah," 3-Verde",0dh,0ah>
	mWrite<" 4-Rojo",0dh,0ah," 5-Magenta",0dh,0ah," 6-Amarillo",0dh,0ah>
	mWrite<" 7-Cyan",0dh,0ah," 8-Cafe",0dh,0ah>
	mWrite<"Ingrese Color:",0>
	mov eax,0
	call readdec

	cmp al,1
	jne siguiente
	mov eax,blue
	call settextcolor
	jmp Salir

	siguiente:
		cmp al,2
		jne siguiente1
		mov eax,white
		call settextcolor
		jmp Salir

	siguiente1:
		cmp al,3
		jne siguiente2
		mov eax,green
		call settextcolor
		jmp Salir

	siguiente2:
		cmp al,4
		jne siguiente3
		mov eax,red
		call settextcolor
		jmp Salir

	siguiente3:
		cmp al,5
		jne siguiente4
		mov eax,magenta
		call settextcolor
		jmp Salir

	siguiente4:
		cmp al,6
		jne siguiente5
		mov eax,yellow
		call settextcolor
		jmp Salir

	siguiente5:
		cmp al,7
		jne siguiente6
		mov eax,cyan
		call settextcolor
		jmp Salir

	siguiente6:
		cmp al,8
		jne siguiente7
		mov eax,brown
		call settextcolor
		jmp Salir
		siguiente7:
		mWrite <"Ingreso un número invalido",0dh,0ah>


	Salir:
		ret

cambiarcolor endp


Instrucciones PROC
      mov edx,offset archivo_lista4
	  call leer_archivo
	  call crlf
ret
Instrucciones endp


leer_archivo proc

	call OpenInputFile
	mov manejadorArchivo,eax

	; Verifica errores.
	cmp eax,INVALID_HANDLE_VALUE ; hay error al abrir el archivo?
	jne archivo_correcto ; no: salir
	  mWrite <"No se puede abrir el archivo",0dh,0ah>
	  jmp Salir ; and Salir

	archivo_correcto:
		; Lee el archivo all buffer.
		mov edx,OFFSET buffer
		mov ecx,TAMANIO_BUFFER
		call ReadFromFile

		jnc verifica_tamanio_buffer ; error al leer el archivo?
		mWrite "Error de lectura del archivo. " ; si: mostrar error
		call WriteWindowsMsg
		jmp cerrar_archivo

	verifica_tamanio_buffer:
		cmp eax,TAMANIO_BUFFER ; el tamaño del buffer es sufiente?
		jb buffer_tamanio_correcto ; si
		mWrite <"Error: Buffer demasiado pequeño para el archivo",0dh,0ah>
		jmp Salir ; y salir

	buffer_tamanio_correcto:
		mov buffer[eax],0 ; insertal null al final del buffer

	mov edx,OFFSET buffer ; muestra buffer
	call WriteString
	call Crlf

	cerrar_archivo:
		mov eax,manejadorArchivo
		call CloseFile
	Salir:
		ret
leer_archivo endp
end main
```
