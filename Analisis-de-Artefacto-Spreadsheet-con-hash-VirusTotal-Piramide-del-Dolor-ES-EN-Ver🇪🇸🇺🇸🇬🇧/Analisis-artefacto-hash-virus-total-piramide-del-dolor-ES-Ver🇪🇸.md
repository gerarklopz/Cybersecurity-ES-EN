## 🧾 Análisis de Artefacto (Spreadsheet con hash) + VirusTotal y Pirámide del Dolor (Pyramid of Pain)

**Objetivo de la Actividad**:
Analizar un archivo sospechoso (malware) usando VirusTotal y clasificar indicadores de compromiso (IoCs) encontrados según su dificultad de bloqueo para el atacante, empleando el modelo de la Pirámide del Dolor.

**Herramientas empleadas**:

*VirusTotal*: Plataforma que analiza archivos, dominios, IPs y URLs para identificar amenazas.

*SHA256 Hash*: Firma única del archivo sospechoso.

**Escenario realista**:
Un analista junior (nivel 1) en un SOC (Centro de Operaciones de Seguridad) de una empresa financiera.llega una alerta:

"Un empleado recibió un email con una hoja de cálculo protegida con contraseña.

Al abrirla, se ejecuta un **payload malicioso**.

El sistema IDS (*Intrusion Detection System*) genera una alerta tras detectar múltiples ejecutables."

**Hash del archivo sospechoso**:
```powershell
SHA256: 54e6ea47eb04634d3e87fd7787e2136ccfbcc80ade34f246a12cf93bab527f6b
```

**Línea temporal**:

Hora y eventos:

* 13:11	El empleado recibe un email con archivo adjunto
* 13:13	Descarga y abre el archivo usando la contraseña
* 13:15	Se crean múltiples ejecutables no autorizados
* 13:20	El IDS detecta actividad sospechosa y lanza una alerta

**Pasos del análisis con VirusTotal**:

* Buscar el hash para ver el análisis del archivo

**Explorar los siguientes apartados**:

**Secciones y qué buscar**

*  Detection	¿Cuántos antivirus lo marcan como malicioso? ¿Qué nombre de malware aparece?
* Details	Otras versiones del hash: MD5, SHA1
* Relations	¿A qué IPs/dominios se conecta? ¿Son maliciosos?
* Behavior	Acciones observadas en entorno aislado (sandbox): procesos, tráfico, técnicas MITRE ATT&CK

##  CIBERSEGURIDAD: EXPLICACIONES Y RESPUESTAS DETALLADAS

| Concepto                             | Explicación Didáctica                                                                                                                               | ¿Viene instalado por defecto?                                                                                                              | Comentarios útiles                                                                                                                         |
| ------------------------------------ | --------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------ |
| **IDS (Intrusion Detection System)** | Sistema que **detecta intrusiones o comportamientos anómalos** en una red o equipo, sin bloquearlos. Se limita a generar alertas.                   | ❌ No viene instalado por defecto en Windows ni Linux. Se instala por separado (ej: **Snort**, **Suricata**, **OSSEC**)                     | No confundir con **IPS**, que sí bloquea. IDS es como un "alarma silenciosa". Ideal para SOCs.                                             |
| **Payload malicioso**                | Es la **parte activa** de un malware que ejecuta una acción dañina: robar datos, abrir backdoors, cifrar archivos, etc.                             | -                                                                                                                                          | Todo exploit tiene **un vector de entrada + un payload**. Es lo que **realiza** el daño tras la explotación.                               |
| **Spreadsheet con contraseña**       | Aunque tenga contraseña, puede ocultar **macros maliciosas**, **scripts embebidos**, **vulnerabilidades**.                                          | ⚠️ No debes confiar solo en la contraseña.                                                                                                 | Las contraseñas en archivos Office no siempre usan cifrado fuerte. Puede romperse con herramientas como **John the Ripper** o **hashcat**. |
| **SHA-256**                          | Algoritmo de **hash criptográfico seguro**, de la familia SHA-2. Produce un hash de 256 bits. Se usa para verificar integridad de datos y archivos. | ✅ Muy utilizado en ciberseguridad (firmas, integridad, contraseñas).                                                                       | Mucho más seguro que MD5.                                                                                                                  |
| **MD5**                              | Antiguo algoritmo hash, ahora considerado **inseguro**. Vulnerable a **colisiones** (dos entradas distintas pueden tener mismo hash).               | -                                                                                                                                          | Todavía lo ves en **checksums antiguos**. No recomendado para nuevas implementaciones.                                                     |
| **Motores AV (Antivirus Engines)**   | Son los "cerebros" de los antivirus: usan firmas, heurísticas, sandboxing y machine learning para detectar amenazas.                                | ✅ Windows: **Windows Defender** viene por defecto. Linux: generalmente no trae AV, pero puedes instalar (ej: **ClamAV**, **Sophos**, etc.) | Acompañados por servicios en la nube para análisis profundo.                                                                               |
| **Mimikatz**                         | Herramienta de post-explotación usada para **extraer credenciales** de Windows (hashes, contraseñas, tickets Kerberos).                             | ❌ No viene instalada. Debe ser descargada por el atacante.                                                                                 | Muy poderosa. Detectada por la mayoría de AVs. Herramienta clásica del pentester.                                                          |
| **PsExec**                           | Herramienta de Sysinternals para ejecutar procesos en sistemas remotos. **Usada legítimamente**, pero también por atacantes.                        | ❌ No viene por defecto, pero es de Microsoft.                                                                                              | Es un clásico del **Living off the Land** (LOLBins). Cuidado si la ves en entornos donde no debería estar.                                 |
| **Netcat (nc)**                      | Herramienta de red para leer/escribir datos por TCP/UDP. Usada para escaneos, shells reversas, etc.                                                 | ✅ Linux: sí suele venir. Windows: no. Debe instalarse.                                                                                     | Apodado el "cuchillo suizo de redes". Si se usa con propósito malicioso, puede ser una gran amenaza.                                       |


## 📊 Evaluación de Maliciosidad
Indicadores clave para decidir si el archivo es malicioso:

🔺 Vendors' ratio alto (muchos motores AV lo detectan)

👎 Community score negativo (opinión de analistas)

⚠️ Detección del malware por nombre y comportamiento típico

✅ Si cumple con estos indicadores, el archivo debe considerarse malicioso.

## Pirámide del Dolor – Clasificación de Indicadores de Compromiso (IoCs)

Un concepto de David Bianco sobre la eficacia y dificultad de defenderse contra ataques cibernéticos según el tipo de indicador usado.

| Nivel | Tipo de Indicador                       | Dificultad de Bloqueo | Dolor para el Atacante | Ejemplo                        |
|-------|------------------------------------------|------------------------|-------------------------|--------------------------------|
| 🔺 6  | TTPs (Tácticas, Técnicas, Procedimientos) | Muy difícil            | Muy alto                | Powershell remoto, Lateral Movement |
| 🔻 5  | Herramientas                              | Difícil                | Alto                    | Mimikatz, PsExec, Netcat      |
| 🔻 4  | Infraestructura (C2: IPs/Dominios)        | Medio-alto             | Medio                   | `bad-domain[.]com`, `192.168.2.5` |
| 🔻 3  | Hashes de archivos                        | Medio                  | Bajo                    | `SHA256: 54e6ea...`            |
| 🔻 2  | IPs específicas                           | Fácil                  | Muy bajo                | `185.72.25.7`                  |
| 🔻 1  | Nombres de archivos                       | Muy fácil              | Nulo                    | `invoice_protected.xlsm`       |
 
---                 

> Cuanto más arriba estés en la pirámide, más difícil es para una organización defenderse, **pero más daño causamos al atacante si implementamos bien**.


Este documento muestra cómo identificar Indicadores de Compromiso (IoCs) en función de su nivel de dificultad para ser reemplazados, siguiendo la **Pirámide del Dolor**. Incluye comandos prácticos para sistemas **Windows** y **Linux**.



## 🔴 Muy difícil de sustituir
**Tipo de IoC**: TTPs (Tácticas, Técnicas, Procedimientos)  
**Descripción**: Comportamientos del atacante como persistencia, escalada de privilegios, ejecución remota, etc.

### 🪟 Windows
```powershell
Get-WinEvent -LogName Security | Where-Object { $_.Message -match "PowerShell" }
```
### Linux
```powershell
ausearch -k exec
journalctl | grep -i powershell
auditctl -l
```
## 🟠 Difícil de sustituir
Tipo de IoC: Herramientas (Mimikatz, PsExec, netcat, etc.)
Descripción: Herramientas usadas para movimiento lateral o exfiltración de datos.

### 🪟 Windows
```powershell
tasklist /v | findstr /i "mimikatz"

Get-Process | Where-Object { $_.Path -like "*netcat*" }
```
### 🐧 Linux
```powershell
ps aux | grep -Ei "mimikatz|nc|nmap|psexec"
lsof -nP | grep '/tmp'
```
### 🟡 Moderadamente difícil de sustituir

**Tipo de IoC**: Artefactos de red o del sistema
**Descripción**: Archivos, claves de registro, servicios o procesos sospechosos creados por malware.

### 🪟 Windows
```powershell
reg query HKCU\Software\Microsoft\Windows\CurrentVersion\Run
Get-ItemProperty -Path 'HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion\Run'
```

### 🐧 Linux
```powershell
find /etc/systemd/ -type f -name "*.service"
ls -alh /tmp /var/tmp
crontab -l
```
### 🟦 Fácil de sustituir
**Tipo de IoC**: Nombre de dominio malicioso
**Descripción**: Dominio al que se conecta el malware.

### 🪟 Windows
```powerrshell
netstat -nob
Resolve-DnsName example.com
Get-DnsClientCache
```

### 🐧 Linux
```powershell
netstat -pant
dig example.com
tcpdump -i eth0 -nnn host bad-domain.com
```

### 🟦 Fácil de sustituir
**Tipo de IoC**: Dirección IP maliciosa
**Descripción**: IP usada como C2 (Command and Control) o para exfiltración.

### 🪟 Windows
```powershell
netstat -ano | findstr :443
Get-NetTCPConnection
```
### 🐧 Linux
```powershell
ss -tupn
netstat -antp
tcpdump -nn dst host 192.168.1.100
```

### 🟦 Muy fácil de sustituir
**Tipo de IoC**: Hash (SHA256, MD5, etc.)
**Descripción**: Firma única del archivo malicioso.

### 🪟 Windows
```powershell
Get-FileHash C:\ruta\archivo.exe -Algorithm SHA256
```
### 🐧 Linux
```powershell
sha256sum archivo.exe
md5sum archivo.exe
sha1sum archivo.exe
```

---


## 🧠 Notas y buenas prácticas
🔐 TTPs y herramientas están en la cima de la pirámide porque cambian menos y definen el estilo de ataque del actor.

🔁 Hashes, IPs y dominios cambian fácilmente; son útiles para alertas, pero menos efectivos para prevenir persistencia.

🎯 Como analista, lo ideal es elevar tu detección hacia los niveles más altos (TTPs, herramientas), donde los atacantes tienen que rediseñar sus campañas.

**🧰 Extras útiles**:

Ver procesos sospechosos recientes

**Windows (PowerShell)**:

```powershell
Get-WinEvent -LogName Security | Where-Object { $_.Id -eq 4688 } | Select-Object -First 10
```
**Linux**:
```powershell
grep -i "execve" /var/log/audit/audit.log
```
**Listar conexiones activas por programa**:

**Windows**:
```powershell 
netstat -ano | findstr ESTABLISHED
```
**Linux**:
```powershell
ss -pant
```

## Qué debemos incluir en la entrega del informe: 

* ✅ Declaración clara de si el archivo es malicioso o no.

* ✅ Tres indicadores de compromiso distintos:

*E.g: otro hash, una IP maliciosa, un dominio, un artefacto del sistema, una herramienta usada o un TTP*.

* ✅ Registrar esos IoCs en la plantilla Pyramid of Pain (una diapositiva por cada tipo).

## Consideraciones importantes

* VirusTotal comparte los archivos públicamente: nunca subas información sensible o personal.

* No te fiarnos de un único análisis o herramienta: VirusTotal es parte de un conjunto de herramientas de un buen analista.

* Usa la información como apoyo para mejorar tu capacidad de detección, análisis y respuesta ante incidentes.

## Reflexiones personales:

# 🧠 Preguntas Críticas sobre el Análisis con VirusTotal y la Seguridad Operacional

| Pregunta / Duda                                                                       | Análisis Crítico y Profesional                                                                                                                                                                                                                                                                                     |
|---------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **¿No hay que ser muy ingenuos para abrir un spreadsheet sospechoso?**               | **Depende del contexto.** No siempre se trata de ingenuidad; muchas veces se debe a falta de formación específica o a una cultura corporativa que no prioriza la ciberseguridad. Los usuarios finales rara vez actúan con mentalidad defensiva. En este sentido, el fallo es tanto humano como sistémico.          |
| **¿No es culpa del operario?**                                                        | **Parcialmente.** En ciberseguridad se aplica el principio de **responsabilidad compartida**. El usuario debe mantener cierto nivel de criterio, pero es responsabilidad de la organización establecer controles, educación continua y entornos seguros para prevenir este tipo de incidentes.                        |
| **¿Confiar en software de terceros para subir archivos sensibles? ¿No es peligroso?** | **Efectivamente.** Esta es una observación muy pertinente. VirusTotal es una herramienta pública: subir artefactos directamente podría comprometer la privacidad y revelar comportamientos a los atacantes. En entornos críticos, deberían utilizarse soluciones internas como **sandbox on-premise**.              |
| **¿Los atacantes pueden usar los datos que subimos a VirusTotal?**                    | **Sí, sin duda.** Grupos de amenazas avanzadas (APT) monitorizan VirusTotal para detectar si sus muestras han sido subidas y así adaptar sus TTPs. Esta actividad se conoce como **VT hunting**. Por ello, **nunca se deben subir muestras sensibles sin considerar las consecuencias técnicas y estratégicas.**   |
| **¿Somos objetivos de ataque por solo usar herramientas de terceros?**                | **Puede ocurrir, aunque no es automático.** Subir un hash no suele ser problemático, pero si se suben archivos con **metadatos, rutas internas o macros activas**, el riesgo aumenta. Además, si se analiza malware sin anonimizar la conexión (por ejemplo, sin VPN o Tor), se expone la IP a los atacantes.       |

---

## Recomendaciones finales:
* Utilizar entornos controlados para análisis de malware (sandbox privada, entorno virtual desconectado, etc.).

* Nunca subir archivos que contengan datos internos, personales o confidenciales.

* Formar al personal de forma continua en cultura de ciberseguridad.

* Usar VPN o entornos anónimos al realizar análisis en línea, especialmente con herramientas públicas como VirusTotal.

*Autor del informe*: Gerardo Arca López

*Fecha*: 15 de mayo de 2025
