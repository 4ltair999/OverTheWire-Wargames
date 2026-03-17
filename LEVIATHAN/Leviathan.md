_________________
##  Índice de Leviathan

### GitHub

- [Level 0](https://www.google.com/search?q=%23level-0)
- [Level 1](https://www.google.com/search?q=%23level-1)
- [Level 2](https://www.google.com/search?q=%23level-2)
- [Level 3](https://www.google.com/search?q=%23level-3)
- [Level 4](https://www.google.com/search?q=%23level-4)
- [Level 5](https://www.google.com/search?q=%23level-5)
- [Level 6](https://www.google.com/search?q=%23level-6)

# Level 0 

- En este ejercicio tenemos que revisar en un uno de los archivos mas próximos a nosotros 

![[Pasted image 20260124012610.png]]

### password:

```
3QJ3TgzHDq
```

________________


# level 1

- En este ejercicio trabajamos con un binario tipo **ELF** .

    -(El formato ELF (Executable and Linkable Format) es un **estándar de archivos binarios** utilizado principalmente en sistemas operativos Unix y Linux)

```
./check: setuid ELF 32-bit LSB executable, Intel 80386, version 1 (SYSV), dynamically linked, interpreter /lib/ld-linux.so.2, BuildID[sha1]=990fa9b7d511205601669835610d587780d0195e, for GNU/Linux 3.2.0, not stripped
```

     Cuando lo revisamos con ls -l vemos que tiene el SUID activo

```
-r-sr-x---   1 leviathan2 leviathan1 15084 Oct 14 09:27 check
```

     Esto ya nos abre un puerta para ejecutar este binario como el usuario propietario y obtener la contrasena  

- Vamos a usar la herramiento **ltrace** para 

![[Pasted image 20260124012610.png]]

### password:

```
NsN1HwFoyN
```

#### Por que usar ltrace?

    ltrace se enfoca en rastrear las llamadas que un programa hace a librerías dinámicas durante su ejecución, es muy usado ya que da muchos detalles relacionados a los errores cometidos en estas interacciones, por ejemplo en este ejercicio analizamos el flujo de ejecucion poniendo una contrasena incorrecta para que ltrace haga su trabajo y nos brinde toda la informacion dobre esta interacion donde se encuentra la contrasena real 

_______________________________________

# level 2

## Version 1 (escape method)


- Para este ejercicio nos aprovechamos de una **Inyección de comandos** viendo que nos enfrentamos a un binario **ELF** como el anterior ejercicio pero en este caso este corre un **cat** que lee los archivos que le pasemos.

![[Captura de pantalla 2026-01-25 044822.png]]

     Con esto entendido tenemos que disenar un metodo para evadir ese cat y ejecutar un exploit adecuado.

- Para esto es necesario hacer uso de **mktemp -d** para crear un directorio en el cual vamos a a crear un archivo con lo siguiente:

```
'file;bash'
```

- Luego de eso vamos a ejecutar el binario con el siguiente orden:

```
./printfile /tmp/tmp.KzHoUcHNKA/file\;bash
```

     Primero estamos llamando a el binario, luego ingresamos la ruta con nuestro archivo creado temporalmente pero con una exception y es que nuestro archivo file;bash va asi file\;bash. usamos el backlash como 'escape' lo cual va a ayudar a que se cierre el primer comando a la vez que iniciamos otro (todo gracias a el ;) para que se ejecute 'bash' y con una bash obtenida como leviathan3 sacar la contrasena.
   

![[Captura de pantalla 2026-01-25 040049.png]]


### password:

```
f0n8h2iWLP
```

## version 2 (symlink method)

- Para este ejercicio hacemos uso de la  vulnerabilidad llamada **Argument Splitting** (División de Argumentos) o, de forma más técnica, **Word Splitting** (División de palabras) y aparte hacer uso de un **symlink**

      Como en el ejercicio anterior tratamos con un binario tipo ELF el cual imprime un el contenido de un archivo con 'cat', con esto ententido vamos sobrepasar ese 'cat'

- Para empezar tenemos en entender dos cosas muy importantes:

1-  **Argument Splitting**/**Word Splitting:** Cuando dejamos un espacio en una **SHELL** esto se puede interpretar como el final de una instrucción. 

`Para la shell, un espacio no es solo un carácter, es la señal de: "Aquí termina una instrucción/archivo y comienza la siguiente"`

2- **symlink  o enlace simbolico:** Es un archivo especial que contiene una referencia a otro archivo o directorio.

- Con lo anterior entendido vamos a plantear la ruta de ataque:


![[Captura de pantalla 2026-01-25 213302.png]]

- Primero en el  **/tmp/** tenemos que establecer (en nuestra propia carperta) el **symlink** el cual nos llevara  a la ruta donde esta la contraseña

```
ln -s /etc/leviathan_pass/leviathan3 /tmp/gabs/test
```

-  Con esto establecido tenemos que crear el otro archivo que funcionara como carnada con el **Argument Splitting** veamos que hace:

     Cuando pones en la terminal algo como **"test 2".txt** si no esta sanitizado propiamente la **SHELL** va a tomar 2 instrucciones **test 2.txt** y luego solo **test**, con esto logramos que en **test** se referencia a la contrasena que necesitamos de esa forma le extraeremos engañando la **SHELL**

```
system("/bin/cat /tmp/holaa/test 2.text"/bin/cat: /tmp/holaa/test: No such file or directory
```

### password

```
f0n8h2iWLP
```

______________________________

# Level 3 

- Este ejercicio nos plantea un binario que nos pide una contraseña para avanzar, vamos a verlo con **ltrace** como se compone.

![[Captura de pantalla 2026-01-26 031949.png]]

      En este notamos algo muy importante y es que se usa strcmp().

- ***strcmp():***

    La función **`strcmp`** compara dos cadenas lexicográficamente. Si `strcmp(hola, snlprint)` devuelve **-1**, significa que la primera cadena es **menor** que la segunda cadena. Esto se debe a que la función compara los valores ASCII de cada carácter de las cadenas, y si el primer carácter de las cadenas no coincide, se continúa comparando los caracteres subsecuentes hasta encontrar una diferencia.

- Debido a lo anterior se puede deducir gracias a **ltrace** que hubo un primero intento para que **strcmp()** comparara la llave principal con un input que no funciono (y ser ve por el -1) pero luego vemos mas abajo un segungo **strcmp()** que falla por que nos da la oportunidad de poner el valor que ya estaba ahi para que se compare contra si mismo y que se rompa el binario 

![[Captura de pantalla 2026-01-26 031350.png]]

- Y si coincide aparte de que nos da una **SHELL**

     Con esta **SHELL** solo nos queda aprovechar el privilegio **SUID** que obtuve descifrando el binario. vamos a ejecutar el binario una vez mas pero esta vez le vamos a agregar un **cat**

```
cat /etc/leviathan_pass/leviathan4
```

![[Captura de pantalla 2026-01-26 030816.png]]

- Y funciona! nos da la contraseña 

```
WG1egElCvO
```

________________

# Level 4

- El desafío consistió en realizar una **conversión de datos crudos (binario) a texto plano (ASCII)** para obtener la credencial.

![[Captura de pantalla 2026-01-26 051413.png]]

     Vamos a usar :

[[https://cyberchef.org/]]

![[Captura de pantalla 2026-01-26 051617.png]]

## Password:

```
0dyxT7F4QD
```

__________________________

# Level 5

- En este ejercicio se nos plantea un binario que tiene un error para correr una archivo **.log** 

```
leviathan5@leviathan:~$ ./leviathan5 
Cannot find /tmp/file.log
```

     Vamos a aprovechar esto como vector de ataque para vincular este /tmp/file.log con un symlink que nos redirige a la contrasena

![[Captura de pantalla 2026-01-27 014344.png]]

```
ln -s /etc/leviathan_pass/leviathan6 /tmp/file.log
```

- Con esto hecho vamos a probar a cargar el binario a ver si funciona:

```
leviathan5@leviathan:~$ ./leviathan5
szo7HDB88w
```

     Ahi tenemos la contrasena

```
szo7HDB88w
```

### Aplicacion en la vida real:

- En este ejercicio tratamos una vulnerabilidad muy importante que se llama **symlink race condition** la cual  trabaja creando un vinculo desde una **ruta predecible** en un directorio con **permisos de escritura global** a uno donde halla información critica como contraseñas 


______________________

# Level 6

- En este ejercicio se nos plantea un binario que nos pide 4 digitos.

     Inicialmente empecé analizando con **strace** y **ltrace** para estudiar como funciona el binario pero al no ver nada en especial como un filtro opte por analizar las respuestas antes diferentes tipos de solicitudes, vamos a verlas

![[Captura de pantalla 2026-01-27 164821.png]]

     Si prestamos atencion vemos que si en nuestro input hay letras da =0 pero si solo usamos numeros estos se veran reflejados =1111 lo que nos da la idea de que interpreta solo numeros 

- Con lo anterior deducido y con el hecho de que no hay  **filtros** o hay **mecanismos de anti-bruteforce** o **rate limiting** en el binario que interfieran con nuestro input optaremos por un ataque de **bruteforce**, vamos a usar un script de **bash** para esto.

```
for i in {0000..9999}; do echo -ne "\rProbando: $i"; ./leviathan6 $i 2>&1 | grep -v "Wrong" && echo -e "\n¡GANASTE! Código: $i" && break; done
Probando: 7123^Z
```

![[Captura de pantalla 2026-01-27 163310.png]]

- Como vemos teniendo los 4 dígitos correctos nos hace subir a leviathan7 y estando ya ahí solo tenemos que buscar la contraseña 

```
qEs5Io5yM8
```

________________________

## Level 7

![[Captura de pantalla 2026-01-27 172219.png]]

______________


























