
___________________

- Maquinas para practicar.

     Anonymous (try hackme)
     Simple CTF (try hackme)

______________

### Sintaxis
sshpass -p 3QJ3TgzHDq ssh leviathan1@leviathan.labs.overthewire.org -p 2223

________________

#### Pagina util
https://gtfobins.org/

_________________


**strcmp**
### 1. `ltrace` (Library Trace)

Se encarga de interceptar las llamadas a **bibliotecas dinámicas** (como la `libc` de C).

- **Qué mira:** Funciones de alto nivel que el programador escribe en el código, como `printf()`, `strcmp()`, `access()`, `system()`, o `malloc()`.
    
- **Para qué sirve en Leviathan:** Es ideal para ver si el programa está comparando tu contraseña con una real usando `strcmp()` o si está intentando abrir un archivo con `access()`.
    

### 2. `strace` (System Trace)

Se encarga de interceptar las **llamadas al sistema** (syscalls). Es el nivel más bajo antes de llegar al kernel (el corazón del sistema operativo).

- **Qué mira:** Operaciones crudas como `open()`, `read()`, `write()`, `close()`, `execve()` o `connect()`.
    
- **Para qué sirve:** Si un programa no usa bibliotecas estándar o si quieres ver exactamente qué archivo está intentando abrir el sistema operativo (independientemente de qué función de C se usó), `strace` te lo muestra todo.
    

---

### Comparativa rápida

|**Característica**|**ltrace**|**strace**|
|---|---|---|
|**Nivel**|Nivel de usuario / Aplicación.|Nivel de Kernel / Sistema.|
|**Foco**|Funciones de bibliotecas (`libc`).|Interacción con el hardware/OS.|
|**Ejemplo**|Ver si usa `strcmp(input, "pass")`.|Ver si intenta leer el archivo `/etc/shadow`.|
|**Uso en Wargames**|**Es el más útil** para empezar a analizar la lógica.|Útil cuando `ltrace` no muestra nada interesante.|






### ¿Qué es un binario?

Un **binario** (o **archivo binario**) es un archivo que contiene datos en formato binario, es decir, en ceros y unos, que la computadora puede ejecutar directamente. A diferencia de los archivos de texto (como .txt), los binarios no están diseñados para ser leídos por humanos, sino por máquinas.

**¿Es un archivo?** Sí, un binario es un tipo de archivo. Por ejemplo, los programas que instalas en tu computadora (como un navegador o un juego) suelen ser archivos binarios.


### ¿Qué puedes hacer con un binario?

- **Ejecutar programas:** Los binarios son los archivos que "corren" los programas en tu computadora.
- **Almacenar datos complejos:** Imágenes, videos, música, etc., también son archivos binarios, pero no son ejecutables.
- **Contener instrucciones:** Los binarios ejecutables contienen instrucciones que la CPU de tu computadora sigue para realizar tareas.

**¿Puede un binario "correr imágenes"?** No exactamente. Las imágenes son archivos binarios, pero no son ejecutables. Un binario ejecutable puede ser un programa que abre y muestra imágenes, pero la imagen en sí no se "corre".

_____________________
## TOCTOU