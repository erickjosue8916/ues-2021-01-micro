# Leer string y guardarlo en archivo existente

![system options file](./systemFunctions.png)

```asm
INCLUDE MIP115.inc

TAMANIO_BUFFER =501
.data
	buffer BYTE TAMANIO_BUFFER DUP(?)
	newTextToFile BYTE "My text",0DH,0AH
	tamanioBuffer DWORD ($-buffer)

	nombreArchivo BYTE "output2.txt",0
	manejadorArchivo HANDLE ?
	tamanioCadena DWORD ?
	bytesEscribir DWORD ?
	cantOpenFile BYTE "No se puede crear el archivo",0DH,0AH,0
	susccessMessage BYTE "Bytes escritos exitosamente en archivo output.txt",0DH,0AH,0
	writeMessage BYTE "Ingrese un maximo de 500 caracteres y presione enter"
				 BYTE "[ENTER]",0DH,0AH,0


.code
main PROC
	INVOKE CreateFile,
	ADDR nombreArchivo,GENERIC_WRITE,DO_NOT_SHARE,NULL,OPEN_EXISTING,FILE_ATTRIBUTE_NORMAL,0 ; deja manejador de evento en eax

	mov manejadorArchivo, eax

	.IF eax==INVALID_HANDLE_VALUE
		mov edx, OFFSET cantOpenFile
		call WriteString
		jmp salir
	.ENDIF

	INVOKE SetFilePointer, manejadorArchivo,0,0,FILE_END
	INVOKE WriteFile, manejadorArchivo,ADDR newTextToFile, tamanioBuffer, ADDR bytesEscribir,0

	INVOKE CloseHandle, manejadorArchivo

	salir:
		exit
main ENDP

END MAIN
```
