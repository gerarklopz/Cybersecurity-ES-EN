# Security Risk Assessment Report ES-Ver🇪🇸

## 📅 Fecha: 2025-10-03

## 👨‍💻 Elaborado por: Equipo de Ciberseguridad (Actualmente cursando:Google Cybersecurity Analyst Certificate)

## 🌐 Empresa: Escenario sobre una organización de redes sociales (pequeña/mediana).

---

## 1. 💡 **Resumen Ejecutivo**

Tras un reciente incidente de seguridad, la empresa ha experimentado una brecha de datos que ha comprometido información sensible de sus clientes. Con el fin de fortalecer la seguridad de la red y prevenir futuros ataques, se ha realizado una evaluación de riesgos para identificar vulnerabilidades y definir un plan de mejora en la seguridad de la infraestructura TI.

Este informe tiene como objetivo proporcionar medidas correctivas y preventivas de alto impacto, que incluyen el uso de herramientas avanzadas de Google, un enfoque de seguridad basado en **Zero Trust**, y una estrategia de capacitación continua para todo el personal, incluyendo tanto el equipo técnico como la administración y la gerencia.

---

## 2. 🛡️ **Vulnerabilidades Identificadas**

Se han detectado las siguientes vulnerabilidades críticas en la infraestructura de red:

1. **Uso compartido de contraseñas entre empleados.**
2. **Contraseña de administrador de la base de datos en su configuración predeterminada.**
3. **Falta de reglas en los firewalls para filtrar el tráfico de entrada y salida.**
4. **Ausencia de autenticación multifactor (MFA) en accesos críticos.**
5. **Uso de software propietario con vulnerabilidades y altos costos de licencia.**

Si estas vulnerabilidades no son mitigadas, la organización continuará en riesgo de sufrir ataques que comprometan su seguridad y reputación.

---

## 3. 🔒 **Medidas de Endurecimiento de la Red**

### 3.1 🔐 Políticas de Contraseñas

- Implementación de **contraseñas seguras** con un mínimo de 16 caracteres, incluyendo mayúsculas, minúsculas, números y caracteres especiales.
- Uso de **gestores de contraseñas corporativos** (ej. Bitwarden, KeePassXC).
- Aplicación de hashing con **bcrypt o Argon2** en el almacenamiento de contraseñas.
- Implementación de **cambios periódicos de contraseña**, con alertas de caducidad.
- Restricción del uso de contraseñas compartidas mediante **políticas de acceso granular**.

### 3.2 🔧 Configuración Segura de la Base de Datos

- **Cambio inmediato** de la contraseña de administrador de la base de datos.
- **Cifrado de datos en reposo y en tránsito** con AES-256 y TLS 1.3.
- **Restricción de accesos** a la base de datos sólo a los sistemas autorizados.
- Implementación de **copias de seguridad regulares** según la estrategia 3-2-1 (3 copias en 2 medios diferentes, 1 fuera del sitio).

### 3.3 🛡️ Firewall y Filtrado de Puertos

- **Habilitación de firewall UFW e iptables** para controlar tráfico de red.
- **Cierre de puertos innecesarios** y aplicación de reglas estrictas:
  - **Permitidos:** 22 (SSH restringido), 80/443 (web), 3306 (MySQL interno).
  - **Denegados:** Accesos externos no autorizados.
- **Segmentación de la red** con VLANs para limitar accesos internos.

### 3.4 🛡️ Autenticación Multifactor (MFA)

- **Activación de 2FA** en todos los accesos críticos.
- Uso de **TOTP (Google Authenticator, FreeOTP) en lugar de SMS**.
- Implementación de **llaves de seguridad FIDO2/Yubikey** para accesos administrativos.

### 3.5 🔢 Implementación de Software Libre

- **Migración de sistemas operativos a Linux (Ubuntu LTS)** para mejorar la seguridad y reducir costos de licencia.
- Uso de **software libre y de código abierto** en servidores y estaciones de trabajo.
- Reducción de vulnerabilidades presentes en sistemas propietarios como Windows.
- Mayor control sobre actualizaciones y configuraciones sin dependencia de fabricantes.
- Uso de **LibreOffice, GIMP** como alternativas seguras a software propietario.

### 3.6 🛡️ **Seguridad en Gmail y Herramientas de Google**

- **Activación de la verificación en dos pasos (2FA)** para todas las cuentas de Gmail de los empleados, utilizando **Google Authenticator** o **llaves de seguridad FIDO2**.
- Implementación de **filtrado de phishing** en Gmail para prevenir ataques de ingeniería social y garantizar la autenticidad de las comunicaciones internas.
- **Google Safe Browsing** para proteger a los empleados contra enlaces maliciosos en los correos electrónicos.

### 3.7 🔢 Monitoreo y Análisis de Registros

- Implementación de **Graylog o ELK Stack** para gestión centralizada de logs.
- Uso de **Fail2Ban** para bloquear intentos de acceso no autorizados.

### 3.8 🛠️ Pruebas de Penetración y Auditorías de Seguridad

- **Realización de pentests trimestrales** con Kali Linux y Metasploit.
- **Auditorías periódicas** con Lynis, OSSEC y Wazuh.
- Simulaciones de ataques para evaluar la efectividad de las medidas implementadas.

---

## 4. 🔄 **Plan de Implementación**

| Medida | Responsable | Frecuencia |
|--------|------------|------------|
| Políticas de contraseñas | IT Security | Permanente |
| Configuración de base de datos | DBA | Inmediato |
| Reglas de firewall | IT Networking | Mensual |
| Implementación de MFA | IT Security | Inmediato |
| Migración a software libre | IT Management | Progresiva |
| Análisis de logs | SOC Team | Diario |
| Pentesting | Equipo Red Team | Trimestral |

---

## 5. 💼 **Conclusiones y Recomendaciones**

Se recomienda la **implementación inmediata** de estas medidas para mitigar los riesgos identificados y garantizar la seguridad de la información de la empresa. Además, se sugiere:

- **Capacitación continua** del personal en buenas prácticas de ciberseguridad. Esto incluye formación específica para el **equipo técnico** sobre nuevas herramientas y **el personal administrativo y de gerencia** sobre los fundamentos de seguridad informática, la importancia de los **ataques de phishing**, y la gestión adecuada de información sensible.
- **Transición gradual a software libre** para mejorar la seguridad y reducir costos.
- **Revisión periódica de las políticas de seguridad** y ajustes según nuevas amenazas.
- **Contratación de expertos en ciberseguridad** para auditorías externas anuales.

Estas acciones permitirán fortalecer la postura de seguridad de la organización y minimizar el impacto de posibles ataques.

---

**👨‍💼 Firmado:** Gerardo Arca López  
**🏢 Departamento:** Ciberseguridad (Actualmente cursando:Google Cybersecurity Analyst Certificate)  
**📅 Fecha:** 2025-10-03

