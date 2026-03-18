
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






