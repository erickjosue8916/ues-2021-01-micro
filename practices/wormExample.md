# Resources

- im.ico
- figura.bmp
- recurso.rc
- idiota.wam

# compilacion

```
rc /v recurso.rc
ml /c /coff prueba_1.asm
link /SUBSYSTEM:CONSOLE prueba_1.asm recurso.res
```
