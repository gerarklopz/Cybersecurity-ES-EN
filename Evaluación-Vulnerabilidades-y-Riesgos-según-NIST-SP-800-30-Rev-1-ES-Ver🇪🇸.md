
## Informe Técnico: Evaluación de Vulnerabilidades y Riesgos en Entorno Híbrido (Cloud + On-premise) según NIST SP 800-30 Rev. 1 ES-Ver🇪🇸

- **Escenario:** Sistema híbrido en empresa global de comercio electrónico.
La empresa almacena información en un **servidor de base de datos**, ya que muchos empleados trabajan de forma remota desde ubicaciones en todo el mundo. Los empleados consultan o solicitan datos del servidor regularmente para encontrar clientes potenciales. **La base de datos ha estado abierta al público desde el lanzamiento de la empresa hace tres años**.
- **Objetivo:** Evaluar amenazas, vulnerabilidades y riesgos asociados a activos críticos expuestos públicamente.
- **Normativa:** NIST SP 800-30 Rev. 1, ISO 27001, ENS (España), RGPD

## Información adicional:

| Marco | ¿Qué es? | ¿Por qué es importante? | ¿Cómo afecta a nuestra empresa? |
|-------|----------|-------------------------|---------------------------------|
| **NIST SP 800-30 Rev. 1** | Una guía oficial de EE. UU. para evaluar riesgos en sistemas informáticos. | Proporciona un método estructurado para identificar, valorar y reducir riesgos de ciberseguridad. | Permite priorizar inversiones en seguridad con base en datos, y documentar decisiones frente a auditorías o incidentes. |
| **ISO 27001 & ENS (España)** | Normas internacionales y nacionales para gestionar la seguridad de la información. | Son exigidas o valoradas en concursos públicos, certificaciones y auditorías. | Demostrar cumplimiento genera confianza, reduce sanciones y abre puertas a contratos con administración pública. |
| **RGPD (Reglamento General de Protección de Datos)** | Ley europea que protege los datos personales de clientes y empleados. | Impone fuertes sanciones si se exponen o mal gestionan datos personales. | Nos obliga a proteger la información de clientes con medidas técnicas (cifrado, control de accesos) y organizativas (políticas, formación). |

---

## Arquitectura Evaluada: Sistema Híbrido

| Componente                  | Ubicación               | Justificación Técnica                                                   |
|----------------------------|-------------------------|-------------------------------------------------------------------------|
| Base de datos de clientes  | On-premise (Hierro)     | Protección de datos sensibles, cumplimiento RGPD                        |
| Frontend web (HTML, CSS)   | Cloud (CDN + S3)        | Baja latencia, entrega global de contenido                              |
| API Backend (REST)         | Híbrido (activo/pasivo) | Tolerancia a fallos, rendimiento local y escalabilidad cloud            |
| Logging y SIEM             | Nube privada            | Retención segura, análisis en tiempo real                               |
| Seguridad perimetral       | WAF + Firewall físico   | Defensa en profundidad                                                  |
| Acceso remoto              | VPN, IAM Cloud          | Acceso seguro y controlado                                              |


---

## Información adicional:

- **API Backend (REST)**:  
  Canal que conecta el frontend con la base de datos. Controla el acceso a la información y permite escalar servicios.

- **Logging y SIEM**:  
  Registro centralizado de actividad. Clave para detectar ataques, auditar accesos y cumplir normativas.

- **WAF + Firewall físico**:  
  Doble barrera de seguridad: el WAF protege apps web y el firewall la red interna. Bloquean amenazas y accesos no deseados.

- **VPN, IAM Cloud**:  
  Accesos cifrados y controlados para empleados remotos. Solo accede quien debe, cuando debe y con permisos justos.



---

## Identificación de Activos, Amenazas y Usuarios

### Activos Críticos

- **Servidor con 3 años en producción**
  - Contiene PII (Información Personal Identificable)
  - Tarjetas de crédito
  - Historial de pedidos

- **Base de datos relacional**
  - Accesible vía API y Web

## Información adicional:

- **PII (Información Personal Identificable)**:
Datos como nombre, DNI, dirección, email o tarjeta de crédito que permiten identificar a una persona.

- **España / UE (RGPD)**: Protección obligatoria. Exposición o mal uso puede conllevar multas millonarias.

- **Worldwide (EE.UU., LATAM, etc.)**: Legislaciones como CCPA (California) o LGPD (Brasil) también exigen medidas de seguridad.

- **Negocio**: Gestionar PII implica confianza, reputación y responsabilidad legal. No protegerla supone un riesgo crítico.

---

### Usuarios Implicados

- Empleados internos (administración, soporte técnico)
- Clientes y usuarios finales
- Proveedores externos (pasarelas de pago, CRM, integradores, etc.)

---

### Amenazas Posibles (Modelo STRIDE / NIST)

| Tipo        | Ejemplos                                                       |
|-------------|----------------------------------------------------------------|
| **Externas**| Fuerza bruta, SQL Injection, escaneo de puertos, explotación de CVEs |
| **Internas**| Abuso de privilegios, exfiltración de datos, errores humanos   |
| **Accidente**| Borrado involuntario, mala configuración (open S3, puertos abiertos) |
| **Naturaleza**| Corte eléctrico, fallo de hardware local                     |

---

## Información adicional:

- **Proveedores externos (pasarelas de pago, CRM, integradores, etc.)**:  
  Terceros que se conectan a nuestros sistemas para funciones clave (cobros, gestión de clientes, sincronización de datos).  
  - **Riesgo**: Ampliación de la superficie de ataque (supply chain).
  - **Ejemplo**: Una pasarela de pago comprometida puede filtrar datos de tarjetas.  
  - **Mitigación**: Validar seguridad de terceros, contratos con cláusulas específicas, controles de acceso segmentados.

- **Fuerza bruta**:  
  Técnica de ataque automatizada para adivinar contraseñas o claves mediante intentos repetitivos.  
  - **Ejemplo**: Acceso no autorizado vía SSH.  
  - **Mitigación**: MFA, bloqueo de IPs tras intentos fallidos, límites de login, CAPTCHA.

- **SQL Injection (SQLi)**:  
  Ataque que inserta código SQL malicioso a través de formularios o URLs para acceder o manipular bases de datos.  
  - **Ejemplo**: `" OR '1'='1"` para saltarse la autenticación.  
  - **Impacto**: Robo de datos, borrado de tablas, control total.  
  - **Mitigación**: Validación de entradas, consultas preparadas (prepared statements), WAF.

- **Escaneo de puertos**:  
  Técnica de reconocimiento para identificar servicios activos en un servidor (HTTP, SSH, MySQL, etc.).  
  - **Ejemplo**: Uso de herramientas como Nmap.  
  - **Riesgo**: El atacante detecta servicios vulnerables.  
  - **Mitigación**: Firewall, cierre de puertos no utilizados, IDS.

- **Explotación de CVEs (Common Vulnerabilities and Exposures)**:  
  Aprovechamiento de fallos de seguridad conocidos en software/hardware para obtener acceso o privilegios.  
  - **Ejemplo**: CVE-2021-44228 (Log4Shell).  
  - **Mitigación**: Escaneos regulares, parches actualizados, monitorización de fuentes oficiales (NIST, MITRE).

- **Open S3 (bucket público de AWS S3)**:  
  Configuración incorrecta que deja accesible contenido almacenado en la nube (archivos, backups, logs...).  
  - **Impacto**: Fuga de datos, acceso no autorizado, exposición mediática.  
  - **Mitigación**: Revisar permisos ACLs, usar cifrado, políticas IAM restrictivas.

---

## Análisis de Vulnerabilidades

### Herramientas Utilizadas

| Herramienta     | Funcionalidad Principal                                               |
|------------------|----------------------------------------------------------------------|
| **Nessus / OpenVAS** | Escaneo de red, detección de CVEs, auditoría de configuraciones inseguras |
| **Nikto**         | Análisis HTTP/HTTPS, identificación de encabezados y servidores vulnerables |
| **Nmap**          | Detección de puertos abiertos y servicios activos                   |
| **Burp Suite**    | Pruebas de seguridad web: fuzzing, detección de XSS, CSRF y otras vulnerabilidades OWASP |
| **Shodan**        | Verificación de exposición pública en motores de búsqueda de dispositivos |

---

### Documentación Consultadas

- **CVE / MITRE**: Referencias oficiales de vulnerabilidades públicas.
- **NVD (National Vulnerability Database)**: Clasificación por **CVSS (Common Vulnerability Scoring System)**.
- **OWASP Top 10**: Riesgos más críticos en aplicaciones web a nivel global.

---

### Resultado Destacado

> 🔴 **Servidor expuesto en Shodan** sin protección perimetral activa  
> (🛑 Sin WAF ni firewall habilitado en el momento del escaneo)

**Riesgo crítico** detectado debido a una configuración expuesta públicamente, sin medidas mínimas de defensa en profundidad activas. Este hallazgo implica una superficie de ataque altamente vulnerable y prioritaria para remediación inmediata.

---

## Evaluación de Riesgos

<table>
  <thead>
    <tr>
      <th>Riesgo</th>
      <th>Probabilidad</th>
      <th>Impacto</th>
      <th>Nivel</th>
      <th>CVSS (Técnico)</th>
      <th>Impacto Negocio</th>
      <th>Riesgo Final (NIST)</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>SQL Injection en API REST</td>
      <td style="background-color:#f8d7da;">Alta</td>
      <td style="background-color:#f5c6cb;">Crítico</td>
      <td style="background-color:#dc3545; color:white;">Crítico</td>
      <td style="background-color:#dc3545; color:white;">9.8</td>
      <td>Afecta datos sensibles y servicio público</td>
      <td style="background-color:#dc3545; color:white;"><strong>Crítico</strong></td>
    </tr>
    <tr>
      <td>Acceso no autorizado vía SSH</td>
      <td style="background-color:#fff3cd;">Media</td>
      <td style="background-color:#ffeeba;">Alto</td>
      <td style="background-color:#fd7e14; color:white;">Alto</td>
      <td style="background-color:#fd7e14; color:white;">8.2</td>
      <td>Compromete infraestructura. Riesgo lateral</td>
      <td style="background-color:#fd7e14; color:white;"><strong>Alto</strong></td>
    </tr>
    <tr>
      <td>Data leak por bucket S3 abierto</td>
      <td style="background-color:#f8d7da;">Alta</td>
      <td style="background-color:#f5c6cb;">Crítico</td>
      <td style="background-color:#dc3545; color:white;">Crítico</td>
      <td style="background-color:#dc3545; color:white;">9.5</td>
      <td>Exposición pública de datos personales (PII)</td>
      <td style="background-color:#dc3545; color:white;"><strong>Crítico</strong></td>
    </tr>
    <tr>
      <td>Error de configuración (LDAP)</td>
      <td style="background-color:#d4edda;">Baja</td>
      <td style="background-color:#d1ecf1;">Medio</td>
      <td style="background-color:#17a2b8; color:white;">Moderado</td>
      <td style="background-color:#17a2b8; color:white;">6.1</td>
      <td>Puede afectar autenticación interna</td>
      <td style="background-color:#17a2b8; color:white;"><strong>Moderado</strong></td>
    </tr>
  </tbody>
</table>



> Recomendación NIST:  
Utilizar el marco **CVSS** (Common Vulnerability Scoring System) para cuantificar el **riesgo técnico (base score)**, complementado con un análisis cualitativo del impacto en el negocio.

---

## Remediación y Mitigación

### Plan de Acción y KPIs

| Acción                         | Plazo     | Responsable | Indicador de Éxito (KPI)                      |
|--------------------------------|-----------|-------------|-----------------------------------------------|
| Aplicar parches críticos       | 24–48h    | SysAdmin    | Tiempo medio de remediación < 48h            |
| Configurar WAF + IDS           | 1 semana  | SecOps      | Reducción de intentos maliciosos detectados  |
| Segmentar red con VLANs        | 15 días   | IT Infra    | Aislamiento validado por escaneo de red      |
| Revisar política IAM           | 7 días    | CISO        | MFA y RBAC activos en el 100% de los roles   |

---
## Monitoreo y Control Continuo

- **SIEM Activo**:  
  - Elastic Cloud  
  - Splunk  
  - Azure Sentinel

- **UEBA (User and Entity Behavior Analytics)**:  
  - Detección avanzada de comportamientos anómalos, insiders y ataques sofisticados.

- **Auditoría de Accesos**:  
  - Syslog centralizado  
  - Cron jobs para revisión periódica  
  - Alertas automatizadas (correo, SIEM, etc.)

> ✅ **Buenas prácticas**:  
Cada medida correctiva debe ser validada mediante test de regresión y **revisiones periódicas planificadas**.

---
## Información adicional:

- **SIEM activo**  
  Plataformas como **Elastic Cloud**, **Splunk** o **Azure Sentinel** centralizan y analizan eventos de seguridad en tiempo real.  
  → Detectan incidentes, correlacionan datos, generan alertas.

- **UEBA (User & Entity Behavior Analytics)**  
  Detecta **comportamientos anómalos** de usuarios y sistemas (e.g. acceso inusual fuera de horario o localización).  
  → Clave contra amenazas internas y ataques persistentes.

- **Auditoría de accesos**  
  - **Syslog central**: Registra toda la actividad crítica.  
  - **Cron jobs**: Revisión periódica automatizada.  
  - **Alertas proactivas**: Notificaciones vía email, SIEM o apps.  
  → Garantiza trazabilidad y respuesta rápida.

---

## Marco AAA: Autenticación, Autorización y Auditoría

| Componente     | Buenas Prácticas Aplicadas                                                         |
|----------------|-------------------------------------------------------------------------------------|
| **Autenticación** | MFA (doble factor), SSO, contraseñas robustas, tokens temporales de acceso         |
| **Autorización**  | RBAC (control por rol), principio de “least privilege”, entornos separados         |
| **Auditoría**     | Logs firmados digitalmente, revisión semanal, alertas proactivas en tiempo real  |

---

## 🛡️ Defensa Activa y Hardening

| Defensa        | Herramienta                      | Descripción Técnica                                                  |
|----------------|----------------------------------|----------------------------------------------------------------------|
| **WAF**        | ModSecurity, AWS WAF             | Protección contra ataques OWASP Top 10 mediante reglas automatizadas |
| **Firewall**   | pfSense, Azure NSG               | Reglas por IP, geolocalización, filtrado de puertos y protocolos     |
| **IPS/IDS**    | Snort, Suricata                  | Análisis de tráfico, detección y prevención de intrusiones           |
| **Hardening**  | CIS Benchmarks                   | Aplicación de checklist automatizado de refuerzo del sistema         |
| **Parcheo**    | WSUS, Ansible, Landscape         | Gestión centralizada de actualizaciones, con validación previa       |

---

## Preguntas Clave y Reflexión
**¿Qué significa estar expuesto públicamente?**
Cualquier IP puede escanear el sistema

Posibilidad de acceso directo a servicios no protegidos

Implica necesidad de VPN, DMZ o túneles cifrados

**¿Y si el ataque viene desde dentro?**
Zero Trust: nunca confiar por defecto

Segmentación de red, control de accesos internos, alertas por comportamiento

---

## Conclusión Estratégica
Un entorno híbrido bien gestionado aporta seguridad, eficiencia y resiliencia, pero requiere:

Diseño compartimentado de arquitectura

Monitorización continua

Gobernanza de accesos

Políticas de parches y hardening

Evaluación de riesgos según NIST de forma periódica

---

## Lessons Learned (Lecciones Aprendidas)

| Lección                                      | Descripción                                                                                                                                              |
|---------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Visibilidad ≠ Seguridad**                 | Detectar un servidor expuesto en Shodan fue un recordatorio claro de que estar en producción no significa estar protegido.                             |
| **El riesgo interno es tan relevante como el externo** | La evaluación demostró que los errores humanos y abusos de privilegio tienen un potencial destructivo igual o mayor que los ataques externos.          |
| **Los controles deben ser dinámicos**       | Las configuraciones, accesos y reglas deben adaptarse al entorno cambiante. La seguridad no es un estado, sino un proceso continuo.                     |
| **La norma NIST no solo es guía, sino método** | Aplicar la NIST SP 800-30 permitió estructurar el análisis con rigor técnico y generar documentación defendible ante auditorías o comités.             |
| **Comunicar riesgo es tan importante como detectarlo** | La tabla de riesgos coloreada y el uso de lenguaje claro en cada sección facilitaron la comprensión por parte de responsables no técnicos.              |

**Elaborado por:** Gerardo Arca López.
**Fecha:** 17/04/2023

