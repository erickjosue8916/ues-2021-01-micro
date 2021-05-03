# WORM

Son programas que realizan copias de sí mismos, alojándolas en diferentes ubicaciones del ordenador.El objetivo de este malware suele ser colapsar los ordenadores y las redes informáticas, impidiendo así el trabajo a los usuarios. A diferencia de los virus, los gusanos no infectan archivos.

# LIBRERIAS

Se debe descargar e instalar de https://www.masm32.com/download.htm

- include C:\\masm32\\include\\windows.inc
- include C:\\masm32\\include\\user32.inc
- include C:\\masm32\\include\\kernel32.inc
- include C:\\masm32\\include\\gdi32.inc
- include C:\\masm32\\include\\winmm.inc

## Libreria Windows

Es una libreria del MASM que permite trabajar con la libreria WIN32 de Microsoft.

Las versiones modernas de Windows utilizan la API de 32 bits llamada Win32. Está compuesta por funciones en C almacenadas en bibliotecas de enlace dinámico (DLL), especialmente en las del núcleo:

- kernel32.dll
- user32.dll
- gdi32.dll

## Libreia User32

Libreria dinamica de que implementa la biblioteca de clientes de API de usuario de Windows

User32.dll es un archivo que controla los cuadros de alerta de mensajes y los paneles de ventanas cuando se utiliza el equipo. Cada programa de software en su sistema o cualquier componente de Windows tiene que usar este archivo DLL para ayudar a que su ordenador funcione lo mejor posible.

Se usa para:

- Trabajar con icono
- Manejar el cursor
- Crear y mostrar ventanas
- Actualizar ventanas
- Cargar imagenes
- Dibujar en ventana
- Manejar las coordenadas de las ventanas
- Procesar mensajes o eventos en ventanas

## Libreria Kernel32

Es uno de los archivos que se carga cuando el sistema operativo se inicia. Su propósito es hacer que las funciones del sistema estén disponibles para los programas.

En esta clase se usará para:

- Manejador de procedimientos
- Linea de comandos

## Libreria WINMM

Permite trabajar,reproducir y registrar archivos de audio.

Es muy utilizada para crear reproductores de audio para el sistema operativo windows.

## Libreria GDI32

Contiene las funciones para Windows GDI (interfaz de dispositivo gráfico) que asiste a ventanas en crear objetos de 2 dimensiones simples.

Se implementará para:

- Crear espacios en memoria
- Transferir bits de un dispositivo a otro
- Elimina los espacios en memoria

## Libreria Win32

Crear módulos cuando se llama a un proceso

- Buscar archivos
- cargar archivos
- bloquear archivos
- trabajo con hilos

### GetModuleHandle

Esta función devuelve un identificador de módulo para el módulo especificado

### GetCommandLine

Es parte de Kernel32

Retorna un puntero a la linea de comandos del proceso actual

### LoadIcon

Permite cargar un icono, este procedimiento se encuentra en la libreria User32, junto a las propiedades para la creación de la ventana.

### LoadCursor

Carga el cursor en un recurso especificado. Pertenece a la libreria User32

### RegisterClassEx

Registrar la estructura que será usada para la construcción de una ventana. Pertenece a User32

### CreateWindowEx

```asm
Crea una nueva ventana y es parte de las funciones de Windows32
HWND hwnd = CreateWindowEx(
    0,                              // Optional window styles.
    CLASS_NAME,                     // Window class
    L"Learn to Program Windows",    // Window text
    WS_OVERLAPPEDWINDOW,            // Window style
    // Size and position
    CW_USEDEFAULT, CW_USEDEFAULT, CW_USEDEFAULT, CW_USEDEFAULT,
    NULL,       // Parent window
    NULL,       // Menu
    hInstance,  // Instance handle
    NULL        // Additional application data
);
```

### ShowWindow

Funcion de la librería User32 que controla la venta que se muestra.

```asm
BOOL ShowWindow(
  HWND hWnd,
  int  nCmdShow
);
```

### UpdateWindow(User32)

La función UpdateWindow actualiza el área del cliente de la ventana especificada enviando un mensaje WM_PAINT a la ventana si la región de actualización de la ventana no está vacía. La función envía un mensaje WM_PAINT directamente al procedimiento de ventana de la ventana especificada, sin pasar por la cola de la aplicación. Si la región de actualización está vacía, no se envía ningún mensaje.

```
BOOL UpdateWindow(
  HWND hWnd
);
```

### GetMessage(User32)

Recupera un mensaje de la cola de mensajes del hilo que llama. La función distribuye los mensajes enviados entrantes hasta que un mensaje publicado está disponible para su recuperación.

```
BOOL GetMessage(
  LPMSG lpMsg,
  HWND  hWnd,
  UINT  wMsgFilterMin,
  UINT  wMsgFilterMax
);
```

### TranslateMessage(User32)

Traduce mensajes de teclas virtuales en mensajes de caracteres. Los mensajes de caracteres se envían a la cola de mensajes del hilo que realiza la llamada, para ser leídos la próxima vez que el hilo llame a la función GetMessage

### DispatchMessage(User32)

REnvía un mensaje a un procedimiento de ventana. Por lo general, se usa para enviar un mensaje recuperado por la función GetMessage.

### LoadBitmap(User32)

```
HBITMAP LoadBitmapA(
  HINSTANCE hInstance,
  LPCSTR    lpBitmapName
);
```

### FindResource(win32)

```
HRSRC FindResourceA(
  HMODULE hModule,
  LPCSTR  lpName,
  LPCSTR  lpType
);
```

### LoadResource(win32)

```
HGLOBAL LoadResource(
  HMODULE hModule,
  HRSRC   hResInfo
);
```

### LockResource(win32)

```
LPVOID LockResource(
  HGLOBAL hResData
);
```

### BeginPaint(User32)

```
HDC BeginPaint(
  HWND          hWnd,
  LPPAINTSTRUCT lpPaint
);
```

### CreateCompatibleDC(gdi32)

La función CreateCompatibleDC crea un contexto de dispositivo de memoria (DC) compatible con el dispositivo especificado.

```
HDC CreateCompatibleDC(
  HDC hdc
);
```

### SelectObject(gdi32)

La función SelectObject selecciona un objeto en el contexto de dispositivo especificado (DC). El nuevo objeto reemplaza al objeto anterior del mismo tipo.

```
HGDIOBJ SelectObject(
  HDC     hdc,
  HGDIOBJ h
);
```

### GetClientRect(user32)

Recupera las coordenadas del área de cliente de una ventana. Las coordenadas del cliente especifican las esquinas superior izquierda e inferior derecha del área del cliente. Debido a que las coordenadas del cliente son relativas a la esquina superior izquierda del área del cliente de una ventana, las coordenadas de la esquina superior izquierda son (0,0).

```
BOOL GetClientRect(
  HWND   hWnd,
  LPRECT lpRect
);
```

### BitBlt (GDI32)

La función BitBlt realiza una transferencia de bloques de bits de los datos de color correspondientes a un rectángulo de píxeles desde el contexto de dispositivo de origen especificado a un contexto de dispositivo de destino.

```
BOOL BitBlt(
  HDC   hdc,
  int   x,
  int   y,
  int   cx,
  int   cy,
  HDC   hdcSrc,
  int   x1,
  int   y1,
  DWORD rop
);
```

### DeleteDC(gdi32)

La función DeleteDC elimina el contexto de dispositivo especificado (DC).

```
BOOL DeleteDC(
  HDC hdc
);
```

### EndPaint (USer32)

La función EndPaint marca el final de la pintura en la ventana especificada. Esta función es necesaria para cada llamada a la función BeginPaint, pero solo después de completar la pintura.

```
BOOL EndPaint (
    HWND hwnd,
    const PAINTSTRUCT *lpPaint
)
```

### GetModuleHandle

Recupera un identificador de módulo para el módulo especificado. El módulo debe haber sido cargado por el proceso de llamada.

> El manejador queda en el registro eax, la variable debe ser tipo HINSTANCE

```
HMODULE GetModuleHandle(
LPCSTR lpModuleName
)
```

### DefWindowProc(user32)

Esta función llama al procedimiento de ventana predeterminado para proporcionar un procesamiento para cualquier mensaje de ventana que una aplicación no procese.

```
LRESULT DefWindowProc(
    HWND hWnd,
    UINT Msg,
    WPARAM wParam,
    LPARAM lParam
)
```

### CreateThread(win32)

Crea un hilo para ejecutar dentro del espacio de direcciones virtuales del proceso de llamada.

# Hilos

En sistemas operativos, un hilo o hebra (del inglés thread), proceso ligero o subproceso es una secuencia de tareas encadenadas muy pequeña que puede ser ejecutada por un sistema operativo.

La destrucción de los hilos antiguos por los nuevos es una característica que no permite a una aplicación realizar varias tareas a la vez (concurrentemente). Los distintos hilos de ejecución comparten una serie de recursos tales como el espacio de memoria, los archivos abiertos, la situación de autenticación, etc. Esta técnica permite simplificar el diseño de una aplicación que debe llevar a cabo distintas funciones simultáneamente.

Un hilo es simplemente una tarea que puede ser ejecutada al mismo tiempo que otra tarea.

Los hilos de ejecución que comparten los mismos recursos, sumados a estos recursos, son en conjunto conocidos como un proceso. El hecho de que los hilos de ejecución de un mismo proceso compartan los recursos hace que cualquiera de estos hilos pueda modificar estos recursos. Cuando un hilo modifica un dato en la memoria, los otros hilos acceden a ese dato modificado inmediatamente.
Lo que es propio de cada hilo es el contador de programa, la pila de ejecución y el estado de la CPU (incluyendo el valor de los registros).

El proceso sigue en ejecución mientras al menos uno de sus hilos de ejecución siga activo. Cuando el proceso finaliza, todos sus hilos de ejecución también han terminado. Asimismo en el momento en el que todos los hilos de ejecución finalizan, el proceso no existe más y todos sus recursos son liberados.

# Recomendado

- https://www.youtube.com/watch?v=xwdYr6YjR1I
- https://www.youtube.com/watch?v=EjEY6IjdkYw
- https://www.youtube.com/watch?v=y_vcMc_UZkg
- https://www.youtube.com/watch?v=zeAkHabyGDo&t=110s
- https://www.youtube.com/watch?v=e6-uvGJhzM4&t=7s
