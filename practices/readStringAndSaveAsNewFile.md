# Procedimientos

- CreateOutputFile ---> dx:
  - path to file
- WriteString ----> dx
- WriteToFile ----> dx ---> cx
- ReadStrid ---> Coloca en eax el tamanio de la cadena
- CloseFile
- WriteDec
- Crlf

```asm
INCLUDE MIP115.inc

TAMANIO_BUFFER =501
.data
buffer BYTE TAMANIO_BUFFER DUP(?)
nombreArchivo BYTE "output2.txt",0
manejadorArchivo HANDLE ?
tamanioCadena DWORD ?
escribirBytes DWORD ?
dontCreateMessage BYTE "No se puede crear el archivo",0DH,0AH,0
susccessMessage BYTE "Bytes escritos exitosamente en archivo output.txt",0DH,0AH,0
writeMessage BYTE "Ingrese un maximo de 500 caracteres y presione enter"
			 BYTE "[ENTER]",0DH,0AH,0


.code
main PROC
	mov edx, OFFSET nombreArchivo
	call CreateOutputFile ; el manejador de archivos queda en el registro eax
	mov manejadorArchivo, eax

	cmp  eax, INVALID_HANDLE_VALUE
	jne archivo_listo
	mov edx, OFFSET dontCreateMessage
	call WriteString
	jmp salir

	archivo_listo:
		mov edx, OFFSET writeMessage
		call WriteString

		mov ecx, TAMANIO_BUFFER ; indicar el maximo tamanio de la cadea a leer
		mov edx, OFFSET buffer ; pasar el buffer antes de leer el string
		call ReadString
		mov tamanioCadena, eax ; leer tamanio de la cadena ingresada

		; required to write in file
		mov eax, manejadorArchivo
		mov edx, OFFSET buffer
		mov ecx, tamanioCadena
		call WriteToFile

		mov escribirBytes,eax ; almacenar bytes ocupados por el archivo
		call CloseFile
		mov edx, OFFSET susccessMessage
		call WriteString
		mov eax, escribirBytes ; obtener bytes registrados para imprimir
		call WriteDec
		call Crlf

	salir:
		exit
main ENDP

END MAIN
```
