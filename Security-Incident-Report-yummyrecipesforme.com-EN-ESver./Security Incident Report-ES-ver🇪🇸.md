# Security Incident Report - ES ver.🇪🇸

## 1. Resumen Ejecutivo

Este informe documenta una brecha de seguridad en el sitio web **yummyrecipesforme.com**, donde un atacante logró acceso no autorizado al panel de administración, modificó el código fuente e introdujo un archivo malicioso que comprometió la seguridad de los usuarios.

El ataque fue posible debido al uso de credenciales predeterminadas para la cuenta de administrador, facilitando un ataque de fuerza bruta. Como consecuencia, los visitantes del sitio fueron redirigidos a una web maliciosa y sus equipos resultaron afectados.

Se recomienda la implementación de **autenticación en dos pasos (2FA)**, revisión de logs, restauración de copias de seguridad y reforzamiento de la seguridad de acceso al servidor.

---

## 2. Descripción del Incidente

**Fecha del incidente:** 2025-03-08 
**Empresa afectada:** yummyrecipesforme.com 
**Área impactada:** Sitio web y clientes afectados 

### 2.1. Detalle del Ataque

- Un atacante obtuvo acceso al panel de administración mediante un ataque de **fuerza bruta**.
- Se detectó la modificación del código fuente de la web, incluyendo un **script en JavaScript** que solicitaba a los visitantes descargar un archivo malicioso.
- Al ejecutar el archivo, los usuarios eran **redirigidos a un sitio web falso** (`greatrecipesforme.com`) que comprometía sus dispositivos.
- La web legítima perdió credibilidad y generó múltiples quejas de clientes.

### 2.2. Pruebas del Incidente

A continuación, se presentan evidencias extraídas del análisis de tráfico con `tcpdump`:

| **Paso** | **Acción** | **Explicación** |
|----------|-----------|----------------|
| 1 | DNS Request a `yummyrecipesforme.com` | El navegador pide la IP del dominio legítimo. |
| 2 | DNS Response | Se devuelve la IP real del sitio web. |
| 3 | HTTP Request | Se establece una conexión HTTP (puerto 80). |
| 4 | Descarga de malware | Se descarga automáticamente un archivo malicioso. |
| 5 | DNS Request a `greatrecipesforme.com` | Se solicita la IP del sitio fraudulento. |
| 6 | DNS Response | Se recibe la IP del servidor malicioso. |
| 7 | HTTP Request a `greatrecipesforme.com` | Se establece conexión con la web del atacante. |

---

## 3. Recomendaciones para prevenir futuros ataques

### 3.1. Implementación de 2FA (Autenticación en Dos Pasos)

Para evitar futuros accesos no autorizados al panel de administración, se recomienda activar **autenticación en dos pasos (2FA)** en las cuentas de administrador y acceso al servidor.

**Implementación en tres entornos clave:**

1. **Para el acceso al servidor (SSH en Ubuntu):**
   - Instalar `libpam-google-authenticator`.
   - Configurar códigos 2FA en cada cuenta administrativa.
   - Habilitar la autenticación en el archivo de configuración SSH.

2. **Para el acceso a la Consola de AWS (si se usa para hosting):**
   - Habilitar MFA en las cuentas IAM.
   - Utilizar Google Authenticator o Authy.

3. **Para el acceso de usuarios en la web (con AWS Cognito):**
   - Configurar un **User Pool** en Amazon Cognito.
   - Habilitar 2FA con SMS o app.
   - Integrar Cognito con la API del sitio para proteger autenticaciones.
   

4. **Implementación de HTTPS en YummyRecipesForMe.com**

   - Para mejorar la seguridad del sitio web **yummyrecipesforme.com**, se recomienda la implementación de **HTTPS** y el uso de **VPNs** para la 
   - administración del sistema. Estas medidas ayudarán a proteger la integridad del sitio y la seguridad de sus administradores y clientes.

### **¿Por qué usar HTTPS?**
✅ **Cifra la comunicación** entre el usuario y el servidor, evitando ataques MITM (Man-in-the-Middle). 
✅ **Genera confianza** en los visitantes, ya que los navegadores advierten sobre sitios HTTP inseguros. 
✅ **Protege credenciales y datos sensibles** al evitar que sean interceptados en tránsito. 

### **Pasos para implementar HTTPS**
   **Obtener un certificado SSL/TLS:**
   - Se recomienda usar **Let’s Encrypt** (gratuito) o **Cloudflare SSL**.
   - Alternativamente, comprar un certificado de **DigiCert, GlobalSign o Sectigo**.

---

5. **Uso de VPNs para Administración Segura en YummyRecipesForMe.com**

### **¿Por qué usar una VPN?**
✅ **Oculta la IP real del administrador**, evitando ataques dirigidos. 
✅ **Cifra el tráfico de datos**, protegiéndolo de espionaje en redes públicas. 
✅ **Evita bloqueos geográficos y restricciones de acceso a la administración del sitio.** 

### **Recomendaciones para el equipo de IT**
1. **Usar una VPN confiable** como **NordVPN, ExpressVPN o ProtonVPN**.
2. **Configurar una VPN corporativa** en un servidor propio con **WireGuard o OpenVPN**.
3. **Restringir accesos administrativos** solo a conexiones desde direcciones IP autorizadas. 
4. **Habilitar autenticación en dos pasos (2FA)** para el acceso al panel de administración.

---

### 3.2. Medidas adicionales

✅ **Revisión de logs**: Monitorear intentos de acceso y detectar actividades sospechosas. 
✅ **Cambio de credenciales**: Implementar contraseñas seguras y evitar credenciales por defecto. 
✅ **Restauración de Backups**: Verificar que no se hayan comprometido datos antes de la restauración. 
✅ **Comunicado oficial a clientes**: Informar a los usuarios sobre el incidente y los pasos para protegerse. 

---

## 4. Conclusión

Este ataque pudo haberse prevenido con mejores prácticas de seguridad, como la autenticación en dos pasos y monitoreo de accesos. La implementación de estas medidas reducirá significativamente la posibilidad de ataques similares en el futuro.

Se recomienda proceder con las acciones correctivas de inmediato y establecer un plan de seguridad a largo plazo para evitar incidentes similares.

**Firma:** 
Gerardo Arca López
**Cargo:** Cybersecurity Analyst(internship student) 
**Fecha:** 2025-03-08 

---


