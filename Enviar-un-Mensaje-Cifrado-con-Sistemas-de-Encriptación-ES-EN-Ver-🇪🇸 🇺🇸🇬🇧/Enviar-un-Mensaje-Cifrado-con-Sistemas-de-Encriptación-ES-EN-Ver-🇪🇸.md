
# Enviar un Mensaje Cifrado con Sistemas de Encriptación ES-Ver-🇪🇸

## Introducción

En esta práctica, aprenderemos a cifrar y descifrar mensajes utilizando diferentes sistemas de encriptación en Linux y Windows. Vamos a usar la codificación Base64 para la transformación básica de mensajes, y el cifrado OpenSSL AES-256-CBC para un enfoque más avanzado y seguro.

## Objetivo

El objetivo es cifrar un mensaje y enviarlo a una persona, proporcionándole instrucciones claras sobre cómo descifrarlo en distintos sistemas operativos. Además, veremos cómo las conversiones de texto a binario y otros formatos intermedios se realizan en el proceso de cifrado.

---

### 1. Enviar un mensaje cifrado en Linux

#### 1.1 Cifrar con Base64

El primer paso es cifrar el mensaje utilizando la codificación Base64. Ejecutamos el siguiente comando para convertir el mensaje a Base64:

```bash
echo "Hola Laura, soy Gerard y has descifrado el mensaje secreto: 'eres muy buena profesora' ;)" | base64 > mensaje.b64
```

#### 1.0. Transformaciones: de Texto a Binario, HEX, etc.

Cada carácter del mensaje se convierte en su equivalente en binario (código ASCII en 8 bits). A continuación, se muestra cómo se convierte el texto en binario:

```makefile
# Mensaje Cifrado en Binario
H   = 01001000  
o   = 01101111  
l   = 01101100  
a   = 01100001  
(space) = 00100000  
L   = 01001100  
a   = 01100001  
u   = 01110101  
r   = 01110010  
a   = 01100001  
,   = 00101100  
(space) = 00100000  
s   = 01110011  
o   = 01101111  
y   = 01111001  
(space) = 00100000  
G   = 01000111  
e   = 01100101  
r   = 01110010  
a   = 01100001  
r   = 01110010  
d   = 01100100  
(space) = 00100000  
y   = 01111001  
(space) = 00100000  
h   = 01101000  
a   = 01100001  
s   = 01110011  
(space) = 00100000  
d   = 01100100  
e   = 01100101  
s   = 01110011  
c   = 01100011  
i   = 01101001  
f   = 01100110  
r   = 01110010  
a   = 01100001  
d   = 01100100  
o   = 01101111  
(space) = 00100000  
e   = 01100101  
l   = 01101100  
(space) = 00100000  
m   = 01101101  
e   = 01100101  
n   = 01101110  
s   = 01110011  
a   = 01100001  
j   = 01101010  
e   = 01100101  
(space) = 00100000  
s   = 01110011  
e   = 01100101  
c   = 01100011  
r   = 01110010  
e   = 01100101  
t   = 01110100  
o   = 01101111  
:   = 00111010  
(space) = 00100000  
'   = 00100111  
e   = 01100101  
r   = 01110010  
e   = 01100101  
s   = 01110011  
(space) = 00100000  
m   = 01101101  
u   = 01110101  
y   = 01111001  
(space) = 00100000  
b   = 01100010  
u   = 01110101  
e   = 01100101  
n   = 01101110  
a   = 01100001  
(space) = 00100000  
p   = 01110000  
r   = 01110010  
o   = 01101111  
f   = 01100110  
e   = 01100101  
s   = 01110011  
o   = 01101111  
r   = 01110010  
a   = 01100001  
'   = 00100111  
(space) = 00100000  
;)  = 00111011
```

### Proceso de conversión:

Cada letra se convierte en su valor binario utilizando la tabla ASCII.

La cadena binaria resultante es lo que luego se agrupa en bloques para su codificación en Base64.

#### Agrupación en bloques de 24 bits (Base64)

Base64 trabaja en bloques de 3 bytes (24 bits), por lo que agrupamos los bits en bloques de 24 bits (3 caracteres). Luego, cada bloque de 24 bits se divide en 4 bloques de 6 bits, y cada uno de estos bloques se convierte en un símbolo Base64.

**Ejemplo con "Hola":**

```makefile
H   = 01001000  
o   = 01101111  
l   = 01101100  
a   = 01100001  
```

**Bloque de 24 bits:**

```bash
01001000 01101111 01101100
```

**Dividimos el bloque de 24 bits en 4 bloques de 6 bits:**
```bash
010010 → S  
011011 → G  
111101 → 9  
101100 → s
```

#### Relleno con = si es necesario

Si el número total de bits no es múltiplo de 24, se añaden ceros extra al final para completar el bloque y se usa el carácter = como relleno. Este relleno asegura que la longitud del mensaje codificado sea siempre un múltiplo de 4 caracteres Base64.

**Ejemplo con "Hola":**

```makefile
a   = 01100001  
(space) = 00100000  
```

**Bloque incompleto:**

```yaml
01100001 00100000  
```

Completamos con ceros:

```yaml
011000 010010 0000 000000  
```

Convertimos a Base64:

```bash
011000 → Y  
010010 → S  
000000 → A  
000000 → =  
```

Así que "Hola" en Base64 es "SG9sYQ==".


#### Resultado final

Una vez aplicado el algoritmo a todo el mensaje, obtenemos su representación final en Base64:

```cpp
SG9sYSBMYXVyYSwgc29 ... (continúa con la salida final en Base64)
```

Este es el mensaje original **"Hola Laura, soy Gerard..."** transformado en Base64, listo para ser transmitido o almacenado de manera segura.

🔹 **Explicación del comando Base64 en Linux**:

`echo` imprime el mensaje en la terminal.

`| base64` convierte el mensaje a Base64.

`> mensaje.b64` guarda la salida en un archivo llamado mensaje.b64.

Para verificar el contenido, podemos usar:

```bash
cat mensaje.b64
```

#### 1.2 Enviar el archivo

Ahora, el archivo mensaje.b64 se puede enviar a la persona deseada, ya sea por correo, USB, o incluso a través de un buzón virtual.

---

### 2. Desencriptar en Windows (PowerShell)

#### 2.1 Instrucciones para el receptor

El receptor del mensaje debe guardar el archivo mensaje.b64 en su escritorio y ejecutar el siguiente comando en PowerShell:

```powershell
Get-Content "$env:USERPROFILE\Desktop\mensaje.b64" | ForEach-Object { [System.Text.Encoding]::UTF8.GetString([System.Convert]::FromBase64String($_)) }
```

🔹 **Explicación**:

`Get-Content` obtiene el contenido del archivo.

`FromBase64String` convierte el texto Base64 de nuevo a texto normal.

`UTF8.GetString` lo muestra correctamente en pantalla.

---

#### 2.2 Desencriptar en Windows (CMD con OpenSSL)

Si el receptor tiene OpenSSL instalado en Windows, puede usar el siguiente comando en CMD para desencriptar el mensaje:

```cmd
openssl aes-256-cbc -pbkdf2 -a -d -in mensaje.enc -k claveSuperSecreta
```

#### 2.3 Cifrar en Linux

Para cifrar un mensaje en Linux con OpenSSL:

```bash
echo "Hola Laura, este mensaje está cifrado con AES-256" | openssl aes-256-cbc -pbkdf2 -a -e -out mensaje.enc -k claveSuperSecreta
```

🔹 **Explicación del comando OpenSSL**:

`openssl aes-256-cbc` utiliza el algoritmo AES con una clave de 256 bits en modo CBC.

`-pbkdf2` refuerza la seguridad de la clave.

`-a` convierte la salida a formato Base64.

`-e` indica que estamos cifrando.

`-out mensaje.enc` guarda el mensaje cifrado en el archivo mensaje.enc.

`-k claveSuperSecreta` define la clave de cifrado.

#### 2.4 Desencriptar en Linux

Para desencriptar el mensaje en Linux:

```bash
openssl aes-256-cbc -pbkdf2 -a -d -in mensaje.enc -k claveSuperSecreta
```

🔹 **Diferencia**: En este caso, usamos el parámetro `-d` para indicar que estamos descifrando el mensaje.

---

### 3. Enviar un mensaje cifrado con OpenSSL (AES-256-CBC)

Cuando necesitamos una **mayor seguridad** para enviar mensajes, el **cifrado** es la mejor opción. Si bien técnicas como Base64 convierten el mensaje en un formato más fácil de manejar y transmitir, **no lo cifran**. **AES-256-CBC** es una técnica de cifrado, lo que significa que **transforma el contenido de manera irreversible** para que solo la persona correcta pueda leerlo.

### ¿Qué es AES-256-CBC?

- **AES**: "Advanced Encryption Standard" (Estándar de Cifrado Avanzado) es uno de los algoritmos de cifrado más utilizados en todo el mundo debido a su **robustez y eficiencia**.
- **256**: Se refiere al tamaño de la **clave** utilizada para el cifrado. Cuanto mayor es la longitud de la clave, **más seguro** es el cifrado, porque se vuelve más difícil de romper.
- **CBC**: "Cipher Block Chaining" (Encadenamiento de Bloques de Cifrado) es un **modo de operación** de AES que mejora la seguridad. Básicamente, cada bloque de texto cifrado depende del anterior, lo que hace que un ataque sea más complicado, ya que cada bloque de texto cifrado no se puede manipular de manera independiente.

---

### 4. ¿Por qué cifrar con OpenSSL AES-256-CBC?

#### 1. Cifrado simétrico de 256 bits
El algoritmo AES-256-CBC utiliza una **clave secreta de 256 bits**, lo que significa que solo una persona que tenga la **clave correcta** podrá descifrar el mensaje. Esto es **mucho más seguro** que una clave de menor longitud, ya que las combinaciones posibles aumentan exponencialmente con la longitud de la clave.

- **¿Por qué 256 bits?**
  Los **256 bits** permiten tener **2^256 combinaciones posibles**, lo que es una cantidad **astronómica** de posibles claves. Con esto, un atacante tendría que intentar **miles de millones de combinaciones** (o más) para romper el cifrado, algo **virtualmente imposible** con la tecnología actual.

#### 2. Cifrado simétrico
AES-256-CBC es un **cifrado simétrico**, lo que significa que la **misma clave** se usa tanto para cifrar como para descifrar el mensaje. Esto lo hace eficiente, pero también implica que la clave debe ser **protegida** adecuadamente.

- **¿Qué significa "simétrico"?**
  Si alguien conoce la clave, puede cifrar y descifrar el mensaje. Es importante asegurarse de que la clave no se revele a personas no autorizadas.

#### 3. La seguridad de CBC (Cipher Block Chaining)
El modo CBC añade una capa adicional de seguridad. En este modo, el **primer bloque** del mensaje se cifra normalmente, pero **los bloques siguientes** se cifran de manera que cada bloque depende del anterior. Esto hace que, aunque un atacante tenga acceso a algunos bloques cifrados, no pueda **manipularlos** ni **alterar el mensaje** sin afectar el resto.

- **¿Por qué CBC es más seguro que otros modos?**
  Si no se utilizara CBC y se cifraran los bloques de forma independiente, un atacante podría modificar un bloque sin que el resto del mensaje se vea afectado. Con CBC, esto no es posible, ya que cada bloque de texto cifrado depende de los anteriores.

---

## Conclusiones

Hemos aprendido a cifrar y descifrar mensajes utilizando Base64 para una codificación sencilla y AES-256-CBC para una encriptación robusta y segura. Estos métodos son útiles tanto en Linux como en Windows, lo que garantiza que la privacidad y seguridad de la información se mantenga intacta durante su transmisión.

¡Ahora puedes enviar y recibir mensajes cifrados con mayor seguridad! 🚀


