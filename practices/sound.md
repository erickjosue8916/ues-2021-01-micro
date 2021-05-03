- is required a sound file in a same directory with "wav" format

```asm
includelib winmm.lib
include MIP115.INC

PlaySound PROTO,
	pszSound:PTR BYTE,
	hmod:DWORD,
	fdwSound:DWORD

.data
	sonido_conectar BYTE "DeviceConnect",0
	SND_ALIAS DWORD 003000H
	SND_RESOURCE DWORD 00040000H
	SND_FILENAME DWORD 00010000H
	archivo BYTE "sound.wav",0

.code
main PROC
	INVOKE PlaySound, OFFSET sonido_conectar, NULL, SND_ALIAS
	INVOKE PlaySound, OFFSET archivo, NULL, SND_FILENAME
	EXIT
main ENDP
END	main

```
