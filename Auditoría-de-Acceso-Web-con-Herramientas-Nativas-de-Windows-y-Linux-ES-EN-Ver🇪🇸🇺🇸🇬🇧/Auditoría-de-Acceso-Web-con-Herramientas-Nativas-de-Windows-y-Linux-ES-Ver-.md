# 🛡️ Auditoría de Acceso Web con Herramientas Nativas de Windows y Linux

**Autores:** Gerard y equipo de investigación  
**Fecha:** 12 de mayo de 2025  
**Objetivo:** Investigar si un empleado ha accedido al dominio `www.example.com` usando únicamente herramientas nativas o de bajo perfil en Windows y Linux. El objetivo es crear un informe claro y detallado, llevarlo al escritorio como evidencia y proporcionar una forma de limpiar rastros sin comprometer la integridad del sistema.

---

## 🧭 Justificación

En entornos corporativos o profesionales donde no se permite el uso de software externo o donde es crucial evitar herramientas invasivas, hemos logrado realizar auditorías de acceso web de forma eficiente utilizando únicamente utilidades nativas de Windows y Linux. Este enfoque es especialmente útil cuando el software adicional no está permitido o cuando buscamos minimizar la huella de nuestras acciones durante una auditoría.

En este informe, detallamos el proceso que hemos seguido para detectar, registrar y reportar accesos a dominios específicos, como el caso de `www.example.com`, usando tanto herramientas nativas de **Windows** como de **Linux**. A través de estas herramientas, hemos sido capaces de generar un análisis forense sobre los posibles accesos a este dominio, comparar resultados entre plataformas y presentar los hallazgos de manera estructurada, asegurando que no alteremos ni comprometamos el sistema de forma inadvertida. Además, incluimos la posibilidad de limpiar rastros de nuestra investigación, siempre respetando el contexto ético y legal en que se realice.

---

## 🖥️ Herramientas y Enfoque Utilizado

### Herramientas en Windows:
En **Windows**, utilizamos herramientas de bajo perfil como `nslookup`, `curl`, `Get-WinEvent` y `Get-Process` para obtener información crítica de red, procesos y conexiones. Estas herramientas nos permitieron rastrear la actividad relacionada con el acceso a dominios web y generar reportes detallados sin necesidad de permisos de administrador elevados.

### Herramientas en Linux:
Por otro lado, en **Linux**, empleamos herramientas nativas como `getent`, `curl`, `ss`, `ps`, `journalctl`, `lsof` y `find` para realizar auditorías similares. Estas herramientas nos permitieron hacer la misma investigación de acceso web en un entorno distinto, comparando la efectividad y la facilidad de uso de las utilidades nativas entre ambos sistemas operativos.

### Comparativa: **tshark** y **tcpdump**
En cuanto a la captura y análisis de tráfico de red, tanto en **Windows** como en **Linux**, hemos utilizado herramientas como **`tshark`** y **`tcpdump`**. Ambas herramientas permiten realizar una captura detallada de paquetes en la red, pero tienen diferencias clave en cuanto a usabilidad y compatibilidad con los sistemas operativos:

| Herramienta | Sistema Operativo | Funcionalidad | ¿Por qué es relevante? |
|-------------|-------------------|---------------|-----------------------|
| **tshark**  | Windows / Linux    | Captura y análisis de paquetes de red | Permite un análisis detallado de tráfico HTTP/HTTPS con opciones avanzadas de filtrado. Requiere permisos de root en Linux, pero es poderosa para auditorías profundas. |
| **tcpdump** | Windows / Linux    | Captura de paquetes de red | Herramienta más simple y ampliamente disponible en Linux. Proporciona un análisis rápido y directo del tráfico sin la complejidad de `tshark`. |

Ambas herramientas son fundamentales cuando buscamos investigar accesos y conexiones a dominios en detalle, permitiéndonos comparar el tráfico observado y confirmar si los accesos a `www.example.com` fueron realizados en ambos sistemas operativos.

---

### **¿Por qué esta aproximación es relevante?**

Utilizar herramientas nativas ofrece varias ventajas clave:
- **Bajo perfil:** No generamos nuevas huellas que puedan delatar nuestras actividades o interferir en el funcionamiento del sistema.
- **Sin necesidad de permisos de administrador:** En muchos casos, hemos sido capaces de realizar auditorías detalladas sin necesidad de privilegios elevados, lo que facilita el trabajo en entornos con restricciones de acceso.
- **Eficiencia:** A pesar de no contar con herramientas externas, hemos conseguido acceder a información crítica sobre las conexiones de red, la resolución DNS y los procesos activos relacionados con la navegación web, tanto en Windows como en Linux.

De esta manera, ofrecemos una solución tanto para auditorías puntuales como para investigaciones más profundas, sin comprometer la integridad ni la seguridad del sistema, independientemente del sistema operativo.

---

### **Limpieza de Rastros:**

En cuanto a la limpieza de rastros, hemos incluido el proceso en Windows mediante el comando `Clear-EventLog`, advirtiendo que su uso solo debe realizarse en entornos controlados y con la debida autorización. En Linux, el enfoque fue más técnico, sin necesidad de comandos invasivos, sino más bien basándonos en la visualización y análisis de logs sin comprometer la integridad de los datos del sistema.

---

### Conclusión

Este procedimiento demuestra cómo, utilizando únicamente herramientas nativas y de bajo perfil en **Windows** y **Linux**, es posible:

- Verificar actividad DNS.
- Consultar respuestas HTTP.
- Rastrear rutas de red.
- Generar informes exportables.
- Limpiar rastros de forma segura.

El método es ideal para entornos con restricciones de software y puede ser ampliado a auditorías más completas integrando scripts, tareas programadas o alertas, adaptándose tanto a entornos Windows como Linux, con el beneficio adicional de no requerir permisos de administrador en muchos de los casos.
"""

---

## 🧰 Herramientas Utilizadas

| Herramienta | Tipo | Uso principal |
|------------|------|---------------|
| `nslookup` | CMD | Resolución de DNS |
| `Resolve-DnsName` | PowerShell | Consultas avanzadas de DNS |
| `curl` | CMD / PowerShell | Verificación de headers HTTP |
| `tracert` | CMD | Trazado de rutas |
| `Get-DnsClientCache` | PowerShell | Ver caché local DNS |
| `Get-WinEvent` / `Get-EventLog` | PowerShell | Revisión de eventos del sistema |
| `Findstr` | CMD | Filtrado de archivos de log |
| `Export-Csv` | PowerShell | Generación de reportes |

***

## 📡 Pasos Realizados

### 1. Verificar si `www.example.com` está presente en la caché DNS
```powershell
Get-DnsClientCache | Where-Object { $_.Name -match "example.com" }
```
## 📌 Si el sitio fue visitado recientemente, aparecerá aquí.

## 2. Obtener más información del dominio

```powershell
Resolve-DnsName -Name www.example.com -Type A
Resolve-DnsName -Name example.com -Type MX
curl -I https://www.example.com
```
***
## 3. Revisar historial de navegación (si se puede)

⚠️ En entornos profesionales no se debe acceder directamente a perfiles de usuario sin autorización.Pero si es necesario y se cuenta con permisos, se podría revisar:
```powershell
Get-ChildItem "C:\Users\*\AppData\Local\Microsoft\Windows\WebCache" -Recurse
```
### 4. Ver logs de eventos de red

```powershell
Get-WinEvent -LogName "Microsoft-Windows-DNS-Client/Operational" |
Where-Object { $_.Message -match "example.com" } |
Select TimeCreated, Message |
Export-Csv "$env:USERPROFILE\Desktop\informe_dns.csv" -Encoding UTF8
```
---
---

## 📊 Análisis Forense de Red y Sistema

Este bloque complementa la auditoría de DNS con datos de red y sistema extraídos durante la investigación. Proporciona evidencia adicional del posible acceso al dominio `www.example.com`.

---

### 🔌 Conexiones TCP activas

```plaintext
LocalAddress  LocalPort  RemoteAddress     RemotePort  State       OwningProcess
------------  ---------  ---------------   ----------  ----------  -------------
10.0.2.15     50370      3.233.158.26      443         Established 6412
10.0.2.15     50369      150.171.29.11     443         Established 6412
10.0.2.15     50367      2.16.54.142       80          Established 2332
10.0.2.15     50360      3.233.158.24      443         Established 6412
10.0.2.15     50176      3.233.158.24      443         Established 6412
10.0.2.15     50148      172.64.155.209    443         Established 6412
10.0.2.15     50146      172.64.155.209    443         Established 6412
10.0.2.15     49973      4.207.247.137     443         Established 2764
```
## 📝 Interpretación:

* El proceso 6412 está realizando múltiples conexiones HTTPS, probablemente navegando web (posiblemente Microsoft Edge, ver procesos).

* El proceso 2332 se comunica por el puerto 80 (HTTP).

* El destino 172.64.155.209 pertenece a Cloudflare, indicando uso de CDN/web.

## 📥 Caché DNS

```plaintext 207.red-81-46-38.customer.static.ccgg.telefonica.net
29.red-81-46-45.customer.static.ccgg.telefonica.net
169.red-81-46-44.customer.static.ccgg.telefonica.net
a.nel.cloudflare.com -> 35.190.80.1
1.red-80-58-78.staticip.rima-tde.net
a2-23-212-34.deploy.static.akamaitechnologies.com
```
## 📝 Interpretación:

* **a.nel.cloudflare.com**  y registros **akamaitechnologies.com** implican contacto con CDNs comunes en navegación web.

* Todos los PTR apuntan a proveedores residenciales en España, lo que confirma tráfico legítimo, no malicioso.

## 🧠 Procesos Activos Relevantes

```plaintext
PID   Nombre         CPU (%)    Memoria (MB)
----  -------------  --------   -------------
6412  msedge         23.81      39.5
7112  msedge        230.54     160.7
8104  msedge       1325.30     734.2
3252  powershell     25.59     106.8
1560  svchost         39.53     53.6
1000  dwm             67.59     53.1
2288  MsMpEng         56.87    162.5
```
## 🔗 Relación entre PID y Conexiones

Para confirmar qué procesos están realizando conexiones, se puede cruzar el PID con la columna **OwningProcess** en el resultado de `netstat`:
```plaintext
| PID (de procesos) | Coincidencia en netstat | Interpretación                        |
|------------------|--------------------------|--------------------------------------|
| `6412`           | Presente                 | Múltiples conexiones HTTPS activas   |
| `2332`           | Presente                 | Acceso por HTTP                      |
| `3252`           | Posible sesión PowerShell| Actividad manual del analista        |
```

🔍 Esto permite asociar procesos a dominios/contactos remotos observados durante la auditoría.


## 📝 Comentarios:

* Se detectan múltiples instancias de Microsoft Edge (msedge) con uso intensivo de CPU y memoria.

* El proceso powershell activo sugiere actividad manual en consola (posiblemente por el analista).

* MsMpEng (Microsoft Defender) también en ejecución normal.

## 📁 Exportación como Evidencia
Se recomienda exportar estos tres conjuntos como archivos individuales en el escritorio:
```plaintetxt
netstat -anob | Out-File "$env:USERPROFILE\Desktop\conexiones_tcp.txt"
Get-DnsClientCache | Out-File "$env:USERPROFILE\Desktop\cache_dns.txt"
Get-Process | Sort CPU -Descending | Out-File "$env:USERPROFILE\Desktop\procesos_activos.txt"
```

## 🧼 Limpieza: Cómo borrar rastros

 Borrar logs específicos (⚠️ solo si es éticamente permitido)
#Este comando puede borrar evidencia y debe usarse únicamente en laboratorios o entornos controlados.

⚠️ **ADVERTENCIA IMPORTANTE:**  
El uso de `Clear-EventLog` puede ser considerado como **alteración de evidencia** en entornos reales.  
Este paso **solo debe realizarse en entornos de prueba, con fines didácticos o con autorización explícita.**

🔐 **Nota adicional:**  
En auditorías reales o procesos judiciales, la preservación de evidencia es crítica. Cualquier acción de borrado debe estar documentada, autorizada, y enmarcada en protocolos aceptados por la organización o marco legal vigente.


```powershell
Clear-EventLog -LogName "System"

```
## 📁 Exportar Informe
```powershell
# Ya creado: informe_dns.csv en el escritorio
Start-Process "$env:USERPROFILE\Desktop\informe_dns.csv"
```
* También se puede generar un informe .txt o .md con observaciones personales:
```powershell
"Informe generado el $(Get-Date)" | Out-File "$env:USERPROFILE\Desktop\informe_web.md"
```
## Bonus: ✅ Comandos PowerShell Usados Sin Permisos de Administrador

| #️⃣ | Comando PowerShell                                                             | Descripción                                    | Relevancia                                                               |                                                                          |
| --- | ------------------------------------------------------------------------------ | ---------------------------------------------- | ------------------------------------------------------------------------ | ------------------------------------------------------------------------ |
| 1️⃣ | `Get-DnsClientCache`                                                           | Muestra la caché DNS local                     | Detecta si el dominio fue resuelto recientemente en el equipo.           |                                                                          |
| 2️⃣ | `Resolve-DnsName -Name www.example.com -Type A`                                | Consulta DNS de un dominio específico          | Permite conocer la IP de un dominio, confirmando conexión.               |                                                                          |
| 3️⃣ | `Get-WinEvent -LogName "Microsoft-Windows-DNS-Client/Operational"`             | Muestra logs de eventos del cliente DNS        | Revisa actividad de resoluciones DNS desde el sistema.                   |                                                                          |
| 4️⃣ | \`Get-Process                                                                  | Sort CPU -Descending\`                         | Muestra los procesos en orden de uso de CPU                              | Permite identificar procesos con alto uso de recursos, como navegadores. |
| 5️⃣ | `Export-Csv "$env:USERPROFILE\Desktop\informe_dns.csv"`                        | Exporta datos a CSV                            | Guarda los resultados de la auditoría para ser revisados luego.          |                                                                          |
| 6️⃣ | `Get-NetTCPConnection`                                                         | Muestra conexiones TCP activas                 | Detecta conexiones activas a dominios/puertos específicos.               |                                                                          |
| 7️⃣ | `Findstr` (para filtrar logs)                                                  | Filtra resultados de eventos o archivos de log | Ayuda a localizar eventos o conexiones relacionados con un dominio.      |                                                                          |
| 8️⃣ | `Get-EventLog`                                                                 | Revisa logs de eventos de sistema              | Detecta actividad de eventos y acceso a redes.                           |                                                                          |
| 9️⃣ | `Get-ChildItem "C:\Users\*\AppData\Local\Microsoft\Windows\WebCache" -Recurse` | Muestra archivos de caché web                  | Posible rastro de navegación web reciente.                               |                                                                          |
| 🔟  | `curl -I https://www.example.com`                                              | Verifica las cabeceras HTTP de un sitio web    | Confirma la disponibilidad de un sitio web con código de respuesta HTTP. |                                                                          |


## ✅ Conclusión
Este procedimiento demuestra cómo, sin herramientas externas, es posible:

* Verificar actividad DNS.

* Consultar respuestas HTTP.

* Rastrear rutas de red.

* Generar informes exportables.

* Limpiar rastros de forma segura.

El método es ideal para entornos con restricciones de software, y puede ser ampliado a auditorías más completas integrando scripts, tareas programadas o alertas.

---

# 🛡️ Auditoría de Acceso Web con Herramientas Nativas de Linux 

Objetivo: Detectar si un usuario accedió a www.example.com usando únicamente herramientas que vienen por defecto en distribuciones comunes de Linux (Debian, Ubuntu, Fedora, CentOS, etc.).

## 🧾 Auditoría Web Express (Solo con herramientas nativas Linux)
Objetivo: Confirmar si hubo acceso a www.example.com sin usar software externo

## 📊 Resumen de Comandos Efectivos en Auditoría Web Local 

| #️⃣ | Comando personalizado                                                        | ¿Qué se investigó?                                  | ¿Por qué es relevante?                                                           |                                                                                         |
| --- | ---------------------------------------------------------------------------- | --------------------------------------------------- | -------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------- |
| 1️⃣ | `getent hosts www.example.com`<br>`host www.example.com`                     | Resolución DNS previa (IPv4/IPv6)                   | ✔️ Si ya se resolvió, probablemente hubo intento de navegación                   |                                                                                         |
| 2️⃣ | `curl -I https://www.example.com`<br>`wget --spider https://www.example.com` | Respuesta HTTP del servidor                         | ✔️ Confirmamos que el servidor respondió (`200 OK`), evidenciando acceso exitoso |                                                                                         |
| 3️⃣ | \`ss -ntp                                                                    | grep ':443\|:80'\`                                  | Conexiones TCP activas a puertos web (HTTPS)                                     | ✔️ Vimos conexiones activas hacia IPs remotas, posiblemente de `example.com` o CDN      |
| 4️⃣ | \`ps aux                                                                     | grep -i chrome\`                                    | Procesos activos                                                                 | ✔️ `chrome` estaba activo; posible navegador usado para acceder a la web                |
| 5️⃣ | \`journalctl                                                                 | grep resolved`<br>`journalctl -u systemd-resolved\` | Logs del sistema sobre DNS                                                       | ⚠️ No muestra acceso directo, pero confirma actividad reciente de resolución de nombres |
| 6️⃣ | `find ~/.config/google-chrome -type f -iname '*Cache*'`                      | Rastro en cachés locales del navegador              | ✔️ Archivos de caché recientes de Chrome sugieren navegación por sitios web      |                                                                                         |
| 7️⃣ | \`lsof -i :443                                                               | grep chrome\`                                       | Procesos con sockets HTTPS abiertos                                              | ✔️ Chrome con conexiones cifradas — evidencia clara de navegación en tiempo real        |
| 8️⃣ | `mkdir -p ~/auditoria_web`                             | Crear un directorio dedicado para almacenar resultados            | ✔️ Organización de evidencias — buena práctica para auditorías reproducibles              |
| 9️⃣ | `ss -ntp > ~/auditoria_web/conexiones.txt`             | Guardar conexiones TCP activas junto con los procesos              | ✔️ Captura en tiempo real — útil para detectar conexiones sospechosas                    |
| 🔟 | `ps aux > ~/auditoria_web/procesos.txt`                 | Listar todos los procesos activos y volcarlos a archivo            | ✔️ Vista completa del sistema — base para correlación con puertos y procesos             |
| 1️⃣1️⃣ | `curl -I https://www.example.com > ~/auditoria_web/curl_response.txt` | Obtener y guardar cabeceras HTTP del sitio consultado              | ✔️ Verificación de servicio web — permite auditar estado y headers sin descargar contenido |
| 1️⃣2️⃣ | `cat ~/auditoria_web/*.txt`                          | Visualizar el contenido de los archivos generados                  | ✔️ Comprobación directa — confirma que los archivos se han generado correctamente        |
| 1️⃣3️⃣ | `ls -la ~/auditoria_web`                             | Listar con detalles todos los archivos generados                  | ✔️ Control de integridad — asegura presencia y permisos correctos de los logs            |



## ✅ CONCLUSIÓN
Hemos realizado una auditoría precisa y efectiva sin instalar ninguna herramienta externa, utilizando únicamente capacidades nativas del sistema operativo Linux. Esto es excelente para entornos restringidos o auditorías forenses rápidas.

## Bonus: 🧠 Tabla Comparativa: Auditoría Web Nativa vs tshark / tcpdump

| #️⃣ | Objetivo                           | Windows CLI/PowerShell                              | Linux nativo                                             | Equivalente `tshark`                                | Equivalente `tcpdump`                                         |                       |
| --- | ---------------------------------- | --------------------------------------------------- | -------------------------------------------------------- | --------------------------------------------------- | ------------------------------------------------------------- | --------------------- |
| 1️⃣ | Ver resolución DNS                 | `Resolve-DnsName www.example.com`                   | `host www.example.com`<br>`getent hosts www.example.com` | `tshark -Y "dns.qry.name == \"www.example.com\""`   | `tcpdump -n port 53 and udp and host www.example.com`         |                       |
| 2️⃣ | Ver respuesta HTTP (cabeceras)     | `curl -I https://www.example.com`                   | `curl -I https://www.example.com`<br>`wget --spider`     | `tshark -Y "http.request and ip.dst==example.com"`  | `tcpdump -A -s 512 'tcp port 80 or 443 and host example.com'` |                       |
| 3️⃣ | Ver conexiones TCP activas         | `netstat -ano`                                      | `ss -ntp`                                                | `tshark -Y "tcp.flags.syn==1 and tcp.flags.ack==1"` | `tcpdump -n tcp`                                              |                       |
| 4️⃣ | Ver procesos navegando             | `Get-Process`<br>`tasklist /fi "PID eq 6412"`       | \`ps aux                                                 | grep chrome\`                                       | — (revisar PID externo a `tshark`)                            | — (no cubre procesos) |
| 5️⃣ | Ver logs DNS en sistema            | `Get-WinEvent -LogName DNS-Client/Operational`      | `journalctl -u systemd-resolved`                         | `tshark -f "port 53"`                               | `tcpdump -vvv port 53`                                        |                       |
| 6️⃣ | Detectar rastro en caché navegador | Buscar en `WebCache` con `Get-ChildItem`            | `find ~/.config/google-chrome -iname "*Cache*"`          | — (solo tráfico, no archivos locales)               | —                                                             |                       |
| 7️⃣ | Identificar sockets HTTPS abiertos | `netstat -anob` (con PID)<br>`Get-NetTCPConnection` | `lsof -i :443`<br>\`ss -ntp                              | grep :443\`                                         | `tshark -Y "tcp.port == 443"`                                 | `tcpdump -n port 443` |
| 8️⃣ | Exportar evidencia                 | `Out-File`, `Export-Csv`                            | `> archivo.txt`                                          | `tshark -w evidencia.pcap`                          | `tcpdump -w evidencia.pcap`                                   |                       |
| 9️⃣ | Rastrear rutas a IP destino        | `tracert www.example.com`                           | `traceroute www.example.com`                             | `tshark -Y "icmp"`                                  | `tcpdump -n icmp`                                             |                       |
| 🔟  | Ver tráfico web en tiempo real     | (No nativo fácilmente)                              | (Tampoco sin herramientas externas)                      | `tshark -i eth0 -Y "http"`                          | `tcpdump -i eth0 -A -s 512 'tcp port 80 or 443'`              |                       |


