# Análisis de Seguridad: Uso Indebido de Tarjeta Corporativa (Framework AAA)ES-Ver🇪🇸

## 🔐 Contexto del Caso

**Escenario:**
Durante el confinamiento de la pandemia del COVID 19 (abril 2020), se registraron gastos recurrentes en servicios personales (Netflix, Spotify, compras domésticas) con la tarjeta corporativa del CEO.
Total registrado: 63 transacciones (≈€4,500) sin justificación empresarial.

### 📊 Tabla de Gastos Sospechosos (Resumen)

| Fecha       | Proveedor     | Importe (€) | Patrón Detectado                     |
|-------------|---------------|-------------|--------------------------------------|
| 2020-04-01  | Netflix       | 12.91       | 12 transacciones en 30 días          |
| 2020-04-03  | Amazon        | 207.50      | Compras fragmentadas (3/día)         |
| 2020-04-22  | Apple Store   | 313.24      | Suma mayor oculta en 3 pagos         |
| 2020-04-30  | Zara Home     | 136.91      | Último día del mes recurrente        |

### 🔍 Framework AAA Aplicado

El marco AAA, que significa **Autenticación, Autorización y Contabilidad**, es un marco de seguridad utilizado para controlar y rastrear el acceso dentro de una red informática, garantizando que solo los usuarios autorizados puedan acceder a recursos específicos y monitoreando sus actividades.

A continuación, lo desglosamos:
- **Autenticación**:
Este proceso verifica la identidad de un usuario o dispositivo que intenta acceder a un recurso, generalmente mediante credenciales como nombres de usuario y contraseñas, u otros métodos como datos biométricos.
- **Autorización**:
Una vez autenticado, la autorización determina a qué recursos y acciones puede acceder el usuario o dispositivo, según su rol o privilegios.
- **Contabilidad / auditoría**:
Este componente rastrea y registra las actividades del usuario, incluyendo los recursos a los que accedió, la hora de acceso y las acciones realizadas, lo que proporciona un registro de auditoría para la monitorización y el análisis de la seguridad.

#### 1️⃣ Autenticación

| Componente       | Hallazgos                           | Riesgo Identificado                    |
|------------------|-------------------------------------|---------------------------------------|
| **Login/IP**     | Misma IP privada del CEO            | No hay MFA para transacciones         |
| **Dispositivo**  | Uso exclusivo de su móvil corporativo| Sin registro de dispositivos autorizados |
 

**Recomendaciones**:  
✔ Implementar autenticación multifactor (MFA) para pagos online  
✔ Registrar IPs/dispositivos autorizados con alertas por anomalías  
✔ Política de contraseñas únicas (no reutilizar en múltiples servicios)
✔ Revocar accesos inmediatos al terminar relación laboral

**Registro de Dispositivos**
✔ Jamf/Intune para:
✔ Whitelist de IMEIs autorizados
✔ Borrado remoto en pérdida/robo

## Información relacionada:
**Intune**
Microsoft Intune es una solución de gestión de dispositivos y aplicaciones (Endpoint Management) basada en la nube que permite a las empresas gestionar y proteger dispositivos, aplicaciones y usuarios de manera centralizada y segura, integrándose con otros servicios de Microsoft como Azure Active Directory y Microsoft 365. 

**IMEI**
El IMEI (International Mobile Equipment Identity) es un código único de 15 dígitos que identifica de forma inequívoca cada teléfono móvil, similar a un número de serie, y que se transmite a la red cada vez que el dispositivo se conecta. 

#### 2️⃣ Autorización

| Componente       | Hallazgos                           | Riesgo Identificado                    |
|------------------|-------------------------------------|---------------------------------------|
| **Políticas**    | No existe documento firmado         | Gastos “discrecionales” ambiguos       |
| **Límites**      | Sin control de montos  | CEO podría argumentar “urgencia pandémica” |
| **Fin de colaboración**      | Sin control frecuencias | 	Ex-empleados con privilegios vigentes
 |

**Recomendaciones**:  
✔ Crear política de gastos con:  
   - Lista blanca de proveedores permitidos  
   - Límites mensuales por categoría  
   - Requisito de aprobación para compras >€100  

1. [X] **Revocación Automática**:  
   - Desactivar cuentas tras 30 días de inactividad  
   - Auditoría trimestral de usuarios activos  

2. [X] **Jerarquía de Accesos**:  
   
     A[CEO](Chief Executive Officer) -->|Aprobación dual| B(Compras >€200)
     B --> C[CFO] ( Chief Financial Officer)
     D[Ex-Empleados] -->|Acceso denegado| B

#### 3️⃣ Contabilidad / Auditoría

| Componente       | Hallazgos                           | Riesgo Identificado                    |
|------------------|-------------------------------------|---------------------------------------|
| **Traza digital**| Facturas sin descripción detallada  | Imposible distinguir uso laboral/personal |
| **Alertas**      | Ningún sistema detectó patrones    | 63 transacciones en 30 días pasaron desapercibidas |

**Recomendaciones**:  
✔ Implementar sistema de clasificación automática de gastos
✔ Alertas en tiempo real para:  
   - Compras en horario no laboral  
   - Proveedores de lista negra (ej. Netflix)  

# 🔍 **Análisis Forense Integrado: Uso Indebido de Tarjeta Corporativa por CEO**  
*(Confinamiento Abril 2020)*  

---

## 🎯 **Resumen Ejecutivo**  
**Hallazgo Central**:  
63 transacciones no justificadas (€4,497.64) en servicios personales (Netflix, Zara Home, Apple) usando tarjeta corporativa BBVA.  

**Evidencia Digital**:  
```python
# Patrón detectado (Python 3.12+ executable snippet)
if proveedor in ["Netflix", "Spotify"] and hora > "20:00":
    clasificar_como("USO PERSONAL")  
```

---

## 🔐 **Framework AAA Aplicado**  

### 1️⃣ **Autenticación Comprometida**  
| **Falla**               | **Prueba**                          | **Recomendación Técnica**          |
|--------------------------|-------------------------------------|------------------------------------|
| No MFA en transacciones  | IP 152.207.255.255 (VPN personal)   | Implementar **[Authy](https://authy.com/)** para pagos >€50 |
| Dispositivo no auditado  | Mismo IMEI móvil CEO (no corporativo)| Registrar dispositivos con **[Jamf](https://www.jamf.com/)** |

**Dato Crítico**:  
12 compras en Netflix (€180.62) desde IP residencial del CEO.  


1️⃣ Autenticación Comprometida
Falla	Prueba	Recomendación Técnica
No MFA en transacciones	IP 152.207.255.255 (VPN personal)	Implementar Authy para pagos >€50
Dispositivo no auditado	Mismo IMEI móvil CEO (no corporativo)	Registrar dispositivos con Jamf
Dato Crítico:
12 compras en Netflix (€180.62) desde IP residencial del CEO.

## Información relacionada:
**Authy**
¿Por qué usar podríamos usar la autentificación de dos factores?
Confiar únicamente en nombres de usuario y contraseñas para proteger nuestras cuentas en línea puede no ser seguro. Las filtraciones de datos ocurren a diario y los hackers siempre están buscando nuevas formas de acceder a nuestras cuentas. Protegernos activando la autenticación de dos factores (2FA) verificareá nuestra identidad a través de los dispositivos y bloqueará el acceso a posible robo de datos. Activando la 2FA desde ahora protegeremos nuestras cuentas en línea.

**Jamf**
Ayudan a las organizaciones a alcanzar el éxito con Apple.
Automatiza y escala los flujos de trabajo de TI y seguridad de Apple.
Comprar hardware de Apple es solo una parte de la ecuación tecnológica. La forma en que protege, gestiona y empodera a sus usuarios finales con la tecnología marca la diferencia.
¿Por qué? Jamf cuenta con casi 20 años de experiencia, una reputación inigualable de soporte técnico inmediato para el sistema operativo Apple y una plataforma completa de seguridad y gestión para ampliar la experiencia Apple.

---

### 2️⃣ **Autorización Ambigua**  
**Políticas Inexistentes**:  
- ❌ No hay límites para compras domésticas.  
- ❌ Justificación "urgente" usada para Uber Eats/Glovo.  

**Solución Propuesta**:  
```markdown
1. [X] Lista negra de proveedores:  
   - Netflix, Spotify, Zara Home  
2. [ ] Aprobación dual para compras >€200 (CEO + CFO)  
3. [ ] Geofencing: Bloquear compras fuera de la UE  
```
---

### 3️⃣ **Auditoría Deficiente**  
**Hallazgos Forenses**:  
| **Táctica Evasiva**       | **Ejemplo**                     | **Contramedida**                  |
|---------------------------|---------------------------------|-----------------------------------|
| Fragmentación de compras   | 3x Amazon en 1h (€78.97+€108.32)| Alertas en tiempo real con **[Splunk](https://www.splunk.com/)** |
| Horario no laboral        | 92% transacciones 20:00-23:59   | IA para detectar anomalías horarias |

Táctica Evasiva	Ejemplo	Contramedida
Fragmentación de compras	3x Amazon en 1h (€78.97+€108.32)	Alertas en tiempo real con Splunk
Horario no laboral	92% transacciones 20:00-23:59	IA para detectar anomalías horarias

## Información relacionada:
**Splunk**
Detecta, investiga y respondae más rápido con la Plataforma Unificada de Seguridad y Observabilidad de Splunk, cómo los equipos de **SecOps, ITOps** e ingeniería que pueden colaborar para garantizar la seguridad y fiabilidad de los sistemas digitales.

**Monitorización Proactiva**
✔ Reglas Splunk:
```
sql
Copy
source="BBVA" (vendor="Netflix" OR vendor="Spotify") 
| stats count by user, date_hour
| where count > 1  # Alertar si >1 transacción/día
```
**SecOps**
se refieren a la función o equipo que se ocupa de la seguridad de los sistemas y la red de una organización, enfocándose en la detección, respuesta y mitigación de incidentes de segurida

Ejemplos de tareas:
- Monitoreo de logs y alertas de seguridad.
- Análisis de tráfico de red.
- Investigación de incidentes de seguridad.
- Implementación de herramientas de seguridad.
- Desarrollo de políticas de seguridad.
- Capacitación de usuarios en seguridad. 

**ITOps**
consisten en los servicios y procesos que un departamento de IT ejecuta dentro de una organización o negocio. Es importante porque administra los activos y procesos digitales más críticos dentro de su organización, así como su seguridad.


---

## ⚖️ **Acciones Legales**  
**Base Jurídica**:  
- **Art. 295 Código Penal**: Apropiación indebida por funcionario.  
- **Art. 226 CCom**: Deber fiduciario incumplido.  

**Pasos Inmediatos**:  
1. [ ] Solicitar reintegro vía carta notarial.  
2. [ ] Auditoría forense con perito judicial.  

---

## 🛡️ **Plan de Mitigación**  
**Fase 1 (48h)**:  
- Congelar tarjeta física.  
- Activar MFA en todas las cuentas CEO.  

**Fase 2 (15d)**:  
- Implementar política de gastos firmada.  
- Capacitación en ética financiera para directivos.  

**Fase 3 (30d)**:  
- Integrar SIEM con bancos (APIs BBVA).  

## Información relacionada:
**SIEM**
Security Information and Event Management(Gestión de Información y Eventos de Seguridad), es una solución de ciberseguridad que **recopila, analiza y correlaciona datos** de seguridad de diversas fuentes para detectar y responder a amenazas de forma rápida y efectiva. 

**BBVA APIs**
Son interfaces de programación de aplicaciones (APIs) que permiten a empresas e individuos integrar funcionalidades y servicios financieros de BBVA en sus propios sistemas y aplicaciones, facilitando la creación de nuevos productos y servicios financieros, así como la automatización de procesos. 

---

**Elaborado por**: Gerardo Arca López.  
**Fecha**: 10/04/2023  



 




