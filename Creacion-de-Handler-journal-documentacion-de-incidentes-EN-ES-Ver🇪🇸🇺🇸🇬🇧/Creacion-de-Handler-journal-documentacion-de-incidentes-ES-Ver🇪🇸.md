# Creación de Handler Journal: Documentación de Incidentes (Versión ES 🇪🇸)

## Introducción
Este documento tiene como objetivo presentar un ejemplo profesional de documentación en el marco de la respuesta a incidentes de ciberseguridad. Se presenta bajo el contexto de un caso práctico y se estructura en base al marco internacional NIST Cybersecurity Framework (CSF), así como normativas relevantes en el ámbito europeo y español. Este contenido está orientado a gerentes, profesionales de recursos humanos, recruiters y responsables de TI, proporcionando una visión estructurada y comprensible del ciclo de vida de un incidente y su correcta documentación.

---

## Marco de referencia: NIST CSF y otras normativas

### Core Functions del NIST-CSF:
Las funciones principales del marco NIST-CSF describen capacidades esenciales:

- **Identificar (Identify)**: Comprensión del contexto organizacional y los riesgos de ciberseguridad.
- **Proteger (Protect)**: Salvaguardas para asegurar la entrega de servicios críticos.
- **Detectar (Detect)**: Identificación oportuna de incidentes de ciberseguridad.
- **Responder (Respond)**: Acciones en caso de un incidente detectado.
- **Recuperar (Recover)**: Restauración de capacidades o servicios que se vieron afectados.

---

### Marcos de trabajo complementarios

| Marco | Descripción | Aplicación práctica |
|-------|-------------|---------------------|
| **ENS (Esquema Nacional de Seguridad - España)** | Marco legal español que establece requisitos mínimos de seguridad para sistemas de la administración pública y entidades que prestan servicios al sector público. | Clasifica los sistemas por criticidad (Bajo, Medio, Alto) y exige medidas según nivel. Ej: política de accesos, trazabilidad, cifrado. Muy usado en licitaciones y proyectos gubernamentales. |
| **ISO/IEC 27001** | Norma internacional para gestionar la seguridad de la información a través de un SGSI (Sistema de Gestión de Seguridad de la Información). | Requiere análisis de riesgos, políticas, controles técnicos y de procedimiento. En empresas privadas mejora confianza y cumplimiento. |
| **ISO/IEC 27035** | Complementa a 27001 centrando el enfoque en la gestión de incidentes de seguridad: detección, reporte, evaluación, respuesta, lecciones aprendidas. | Ofrece un marco que puedes adaptar con procedimientos paso a paso para cada fase del incidente. Ideal para equipos SOC (Security Operations Center) en ciberseguridad es un equipo especializado que se encarga de monitorear, detectar, analizar y responder a incidentes de ciberseguridad dentro de una organización) o pequeños departamentos TI. |
| **GDPR / LOPDGDD** | Reglamento General de Protección de Datos (UE) + su adaptación española. Regulan cómo se recopilan, tratan y protegen los datos personales. | Implica notificar incidentes a la AEPD (Agencia Española de Protección de Datos es la autoridad pública encargada de garantizar el cumplimiento de la normativa de protección de datos en España. ) en menos de 72h si afectan datos personales. Multas importantes. La documentación del Journal puede ser una prueba de diligencia. |
| **CCN-STIC** | Serie de guías técnicas del Centro Criptológico Nacional (España), vinculadas al cumplimiento del ENS y la ciberdefensa nacional. | Recomendaciones técnicas precisas: bastionado de sistemas, análisis de logs, respuesta a incidentes. Muy útiles para definir políticas robustas en entornos públicos o híbridos. |


---

## Ciclo de vida de respuesta a incidentes 

```mermaid
   Preparación --> Detección_y_Análisis
    Detección_y_Análisis --> Contención
    Contención --> Erradicación_y_Recuperación
    Erradicación_y_Recuperación --> Post_Incidente
    Post_Incidente --> Actualización_de_controles
```

---

## Desarrollo de fases

### 1. Preparación
- Políticas, procedimientos, formación, herramientas y acuerdos.
- Principio de mínimo privilegio y autenticación multifactor.

### 2. Detección y análisis
- Recopilación de logs, alertas SIEM, comportamiento anómalo.
- Análisis de eventos para confirmar incidentes.

> ⚠️ *Todos los incidentes son eventos, pero no todos los eventos son incidentes.*

- **Evento**: Ocurrencia observable en un sistema, red o dispositivo.

### 3. Contención
- Aislamiento de máquinas comprometidas.
- Segmentación de red y desactivación de accesos.

### 4. Erradicación y recuperación
- Eliminación de malware.
- Restauración desde backups validados.
- Revisión de accesos y contraseñas comprometidas.

### 5. Actividad post-incidente
- Lecciones aprendidas.
- Mejoras en políticas y controles.
- Generación del informe final.

---

## Caso práctico: Ataque interno premeditado

### Descripción:
Un empleado con privilegios administrativos y conocimientos técnicos, antes de ser despedido, inserta ejecutables maliciosos en los sistemas administrativos. Estos scripts se ejecutan semanas después, generando sabotaje sin dejar trazas evidentes que lo incriminen directamente.

---

### ¿Cómo lo hizo el saboteador?

El atacante interno (el exempleado con conocimientos avanzados de TI) actuó de forma planificada y sigilosa. Aquí desglosamos sus técnicas con detalle técnico y contextual:

| Técnica utilizada | Explicación y riesgo |
|-------------------|----------------------|
| **Tareas programadas con retardo** | Usó el *Task Scheduler* de Windows o *cron* en sistemas UNIX para programar ejecución diferida de scripts maliciosos (días o semanas después). Difícil de vincular directamente con su salida. |
| **Living off the land (LotL)** | Técnica que aprovecha herramientas legítimas del sistema operativo para actividades maliciosas. Ej: `PowerShell`, `WMI`, `certutil`, sin levantar sospechas. Evita AVs y EDRs ( AV : antivirus son software de seguridad que se centra en detectar y eliminar malware conocido, mientras que los EDR (Endpoint Detection and Response) son soluciones más avanzadas que analizan el comportamiento de los sistemas para detectar amenazas, incluso las desconocidas, y responder a ellas. ) mal configurados. |
| **Nombres camuflados** | Renombró ejecutables maliciosos como `svchosts.exe`, `winupdater.bat` o `system32check.vbs` para pasar desapercibido. También modificó atributos para ocultarlos. |
| **Modificación de permisos** | Alteró ACLs (Listas de Control de Acceso) para impedir la eliminación o análisis de sus archivos. También inhabilitó registros o inhibió notificaciones del sistema. |
| **Desactivación silenciosa de antivirus** | Ejecutó comandos que modifican políticas locales o claves del registro para inhabilitar temporalmente defensas como Windows Defender. Ej: `Set-MpPreference`. |

> Consejo para detección: Monitorizar tareas programadas nuevas, cambios en ACLs (dispositivos de red como routers, firewalls y switches. Su función principal es filtrar el tráfico de datos entrante y saliente, permitiendo o denegando el acceso a la red en función de diversos criterios.) , scripts no firmados y uso anómalo de PowerShell.

---

## Investigación del incidente – Método de las 5 W

- **¿Quién?**: Ex-empleado con acceso privilegiado.
- **¿Qué?**: Instalación de ejecutables que alteran cifras, borran archivos o redirigen documentación.
- **¿Cuándo?**: Semanas después de su salida, usando temporizadores y persistencia.
- **¿Dónde?**: Sistemas de gestión administrativa y servidores compartidos.
- **¿Por qué?**: Venganza y sabotaje oculto hacia el empleador.

> Llevar un registro meticuloso de esta información es esencial, no solo durante una investigación, sino también durante la fase de cierre del incidente.

---

## Casos adicionales de amenazas internas

- **Administradores que crean backdoors ( es una secuencia especial o un término trasero dentro del código de programación, mediante la cual se pueden evitar los sistemas de seguridad del algoritmo (autenticación) para acceder al sistema.).**
- **Empleados que filtran datos a competidores.**
- **Errores no maliciosos pero graves (borra archivos por accidente).**

---

## Anexos técnicos

- Scripts detectados (ejemplo de payload ofuscado).
- Listado de procesos y tareas programadas.
- Configuración del SIEM y alertas generadas.
- Logs críticos de acceso y ejecución.

---

## Medidas preventivas

- Implementar EDR y alertas sobre comportamiento anómalo.
- Revisión y control de tareas programadas.
- Auditoría post-empleo (eliminación inmediata de accesos).
- Control de versiones, integridad de archivos y hashing.

---

## Checklist de respuesta a incidentes

- [x] Confirmación del incidente
- [x] Aislamiento del sistema
- [x] Notificación a responsables
- [x] Análisis forense
- [x] Erradicación
- [x] Restauración de datos
- [x] Informe final y lecciones aprendidas

---

## Roles y responsabilidades

| Rol | Responsabilidad |
|-----|-----------------|
| CISO | Supervisión general del incidente |
| Analista SOC | Identificación y análisis inicial |
| Forense digital | Recopilación de evidencias |
| Administrador de sistemas | Contención y recuperación |
| Legal / RRHH | Gestión de consecuencias internas |

---

## Handler's Journal: Herramienta documental

- **Fecha del incidente**: 2025-04-21
- **Número de entrada**: 1
- **Descripción del evento**: Sabotaje interno planificado con ejecución retardada.
- **Herramientas utilizadas**: Sysinternals, ELK, Wireshark, Velociraptor.

    | Herramienta | ¿Qué hace? | Aplicación en este caso |
    |------------|------------|-------------------------|
    | **Sysinternals Suite** (Microsoft) | Conjunto de utilidades para análisis profundo de sistemas Windows. Ej: `Autoruns`, `Process Explorer`, `PsExec`. | Se usó para detectar procesos sospechosos, analizar arranques automáticos y rastrear actividades ocultas. Útil para encontrar persistencia. |
    | **ELK Stack (Elastic, Logstash, Kibana)** | Plataforma de análisis de logs y eventos. Permite centralizar, visualizar y correlacionar actividad. | Ideal para detectar patrones de comportamiento anómalos: accesos fuera de horario, eventos de error, escritura de ficheros sensibles. |
    | **Wireshark** | Analizador de tráfico de red. Captura paquetes y permite estudiar comunicaciones en profundidad. | Usado para detectar conexiones salientes inusuales o comunicación con servidores C2 (command & control). Muy útil si el ejecutable intentaba exfiltrar datos. |
    | **Velociraptor** | Herramienta forense y de hunting en vivo. Recoge artefactos de endpoints (procesos, logs, eventos) y permite consultas remotas. | Muy eficaz en análisis post-mortem. Se empleó para auditar cambios del sistema, examinar tareas programadas, y detectar indicadores de compromiso. |

- **Desarrollo de las 5 W**: Ver sección anterior.
- **Observaciones adicionales**: Se recomienda revisar procedimientos de baja laboral.

---

## Conclusión

Este documento ejemplifica cómo una documentación adecuada y metódica puede servir tanto como registro técnico como recurso profesional y pedagógico. El uso del Handler’s Journal junto con marcos como NIST CSF y ENS garantiza una respuesta eficaz, minimizando riesgos y fortaleciendo la resiliencia organizacional.

---

_Realizado por: Gerardo Arca López_

_Fecha:22 de Abril de 2025_


