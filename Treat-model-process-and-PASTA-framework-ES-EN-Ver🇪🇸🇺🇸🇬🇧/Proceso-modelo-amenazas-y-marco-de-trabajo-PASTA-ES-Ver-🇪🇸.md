# Proceso de Modelado de Amenazas y Framework PASTA (versión ES 🇪🇸)

## Proyecto: What2SMS - Redirigir Mensajes de WhatsApp a SMS para Móviles Clásicos

---

## Escenario

Muchas personas aún dependen de móviles básicos ("almejitas") sin acceso a WhatsApp ni apps modernas. **What2SMS** busca redirigir mensajes recibidos por WhatsApp a través de **SMS plano**, permitiendo notificaciones en tiempo real sin necesidad de smartphone.

---

## Objetivo del Servicio

**Facilitar la recepción de mensajes de WhatsApp mediante SMS**, útil en:

- Personas mayores o con dificultades tecnológicas
- Áreas sin conectividad de datos
- Viajeros y trabajadores de campo
- Usuarios que desean reducir su dependencia digital ("Digital detox", teléfonos tipo LightPhone)

---

## Ventajas del Servicio

| Ventaja                | Descripción                                                  |
|------------------------|--------------------------------------------------------------|
| Inclusión digital      | Sin necesidad de smartphone o apps.                          |
| Bajo consumo           | Solo utiliza red GSM.                                        |
| Simplicidad operativa  | Texto plano sin capas ni configuraciones complejas.          |
| Privacidad controlada  | Menor exposición digital.                                    |

---

## ⚠️ Retos Técnicos y de Seguridad

| Reto                        | Riesgo Potencial                              |
|----------------------------|-----------------------------------------------|
| Sincronización WhatsApp    | Dificultad legal y técnica.                   |
| Limitaciones del SMS       | No cifrado, longitud reducida.               |
| Exposición de terceros     | Redirección involuntaria de mensajes.        |
| Automatización no autorizada | Violación de políticas de WhatsApp.         |
| Spoofing                   | Suplantación si no se valida origen.         |

---

## Introducción al Threat Modeling

El **Modelado de Amenazas** permite identificar y anticipar riesgos de seguridad **antes** de que ocurran, estructurando defensas desde la fase de diseño.

> Es pensar como un atacante para construir como un defensor.

---

## Threat Model Process – Etapas Clave

| Paso | Acción                     | Ejemplo en What2SMS                          |
|------|----------------------------|----------------------------------------------|
| 1    | Identificar activos        | Mensajes, usuarios, API, backend.            |
| 2    | Enumerar actores           | Hackers, scripts, insiders.                  |
| 3    | Analizar amenazas          | Spoofing, fuga de datos, acceso no autorizado.|
| 4    | Modelar ataques            | Phishing, intercepción, manipulación.        |
| 5    | Evaluar impacto/probabilidad | Riesgos críticos vs. aceptables.           |
| 6    | Aplicar controles           | Cifrado, validaciones, autenticación.        |

---

## Framework PASTA – Proceso de Seguridad por Fases

**PASTA** = **P**rocess for **A**ttack **S**imulation and **T**hreat **A**nalysis*

Un enfoque moderno y estructurado para evaluar riesgos alineando seguridad y objetivos del sistema.

| Fase | Enfoque                        | Aplicación en What2SMS                        |
|------|--------------------------------|-----------------------------------------------|
| I    | Objetivos del negocio          | Redirigir WhatsApp a SMS sin apps.            |
| II   | Alcance técnico                | WhatsApp Web, backend modular, gateway SMS, sockets. Tecnologías priorizadas por su compatibilidad con móviles clásicos, bajo requerimiento de recursos y facilidad de integración sin dependencias cerradas. <br><br>**Justificación:**<br>• *WhatsApp Web* permite capturar mensajes sin API oficial.<br>• *Sockets* permiten comunicación en tiempo real sin polling.<br>• *Backend modular* permite escalar, dividir funciones y aplicar seguridad por capas.<br>• *API SMS Gateway* garantiza entrega a dispositivos no conectados.<br>• *PKI y tokens* aseguran autenticación y protección contra spoofing.<br>• *AES/SHA-256* cifran datos sensibles en tránsito y reposo.<br>• *SQL* garantiza integridad, persistencia y auditoría. |
| III  | Modelado de flujo de datos     | WhatsApp → Backend → SMS → Usuario.           |
| IV   | Identificación de amenazas     | Acceso no autorizado, spoofing, exposición.   |
| V    | Análisis de vulnerabilidades   | APIs abiertas, validaciones débiles.          |
| VI   | Simulación de ataques          | Sniffing, automatización abusiva.             |
| VII  | Controles y mitigaciones       | Cifrado, autenticación, control de flujo.     |

---

## **What2SMS bajo la lupa de PASTA**

Este modelo permite anticipar riesgos reales y aplicar defensas específicas para mitigar las amenazas identificadas:

---

### 🔐 **Riesgos Principales:**

- **Interceptación de SMS**: Riesgo de que los mensajes sean leídos por actores no autorizados.
- **Redirecciones erróneas**: Posibilidad de desviar mensajes a destinatarios incorrectos.
- **Exposición de datos sensibles**: Acceso no autorizado a información personal o confidencial.
- **Violaciones de políticas externas**: Uso indebido de WhatsApp y otras plataformas sin permisos adecuados.

---

### 🛡️ **Defensas Aplicadas:**

#### ✔️ **Validación de Emisores**
**¿Qué es?**  
Confirmar que el origen de un mensaje es legítimo, evitando suplantación.

**¿Cómo se implementa?**  
- **Firmas digitales (PKI)**: Verificación mediante claves públicas y privadas.
- **Lista blanca de números/emisores**: Aceptación solo de fuentes confiables.
- **Validación de tokens JWT**: Asegura la autenticidad del emisor mediante autenticación segura.

---

#### 🔒 **Cifrado de Extremo a Extremo (E2EE)**
**¿Qué es?**  
Protege la confidencialidad de los mensajes a lo largo de todo el proceso, desde el emisor hasta el receptor.

**¿Cómo se implementa?**  
- **AES-256**: Cifrado robusto de mensajes en tránsito.
- **RSA/ECDH**: Intercambio seguro de claves entre el emisor y receptor.
- **Descifrado final**: El mensaje solo puede ser leído por el receptor, sin exposición durante el tránsito.

---

#### 📝 **Logs de Actividad y Alertas**
**¿Qué es?**  
Monitoreo constante de las acciones realizadas por el sistema para detectar incidentes y generar informes.

**¿Cómo se implementa?**  
- **Sistemas de Logs**: Ejemplos como ELK stack, Loki o Fluentd para almacenar registros detallados (IP, hora, acción, usuario).
- **Alertas automáticas**: Notificación por email o webhook ante actividades sospechosas o anómalas.

---

#### ⚙️ **Control de Automatismos**
**¿Qué es?**  
Evitar abusos del servicio por parte de bots o usuarios no autorizados.

**¿Cómo se implementa?**  
- **Rate limiting**: Restricciones sobre el número de mensajes por minuto (ej: 10 mensajes/minuto por usuario).
- **Captchas**: Validaciones de "humano vs. máquina" para evitar interacciones automatizadas.
- **Detección de comportamiento automatizado**: Uso de algoritmos de machine learning o reglas predefinidas para identificar y bloquear actividades sospechosas.

---

## ¿Cómo lo abordamos?

- Aplicamos **PASTA** como marco analítico.
- Usamos **DFDs** y **árboles de ataque** para visualizar flujos y amenazas.
- Diseñamos controles desde el inicio del desarrollo.

---

## Evaluación Final

Hemos construido un sistema resiliente y con foco en la seguridad, sabiendo que **la protección no es estática**. Continuaremos auditando y adaptando el modelo conforme evolucione el proyecto y sus riesgos.

---

##  Referencias

- [OWASP Threat Modeling Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Threat_Modeling_Cheat_Sheet.html)

---

##  Representaciones Visuales

### Data Flow Diagram (DFD - Nivel 1)

```plaintext
+-------------+       SMS/API        +------------+        SQL         +--------------+
|  Usuario    |  <---------------->  |  Backend   | <----------------> |  Base de Datos |
| (Almejita)  |                      | (Servidor) |                    | (Mensajes)    |
+-------------+                      +------------+                    +--------------+
       |                                  |
       |       WhatsApp Web               |
       +------------------------------->  |
            (Captura y Reenvío)
```
---
##  Attack Tree (Resumen Simplificado)
```
                         [Acceso a Mensajes]
                                 |
            +--------------------+----------------------+
            |                                           |
     [Interceptar SMS]                          [Comprometer Backend]
            |                                           |
   +--------+--------+                        +--------+--------+
   |                 |                        |                 |
[Spoofing SMS]   [Sniffing GSM]        [SQL Injection]   [API Token Leak]

```
---


## **Participación del Equipo de Seguridad**

El proceso de **Threat Modeling** se implementó en 6 fases clave:

---

1. **Identificación de Activos**  
   Activos críticos como **mensajes, claves y tokens**.

2. **Perfiles de Atacantes**  
   Consideración de amenazas como **spoofers, interceptores e insiders**.

3. **Vectores de Ataque**  
   Identificación de posibles vectores: **phishing, hijacking y DoS**.

4. **Diseño de Árboles de Ataque**  
   Aplicación de la **metodología PASTA** para estructurar posibles ataques.

5. **Evaluación de Riesgos**  
   Análisis de vulnerabilidades utilizando **referencias CWE, CVE y OWASP**.

6. **Controles y Mitigaciones**  
   Implementación de **PKI, doble token y alertas** para proteger los activos.

---

## Framework PASTA aplicado a What2SMS

| Etapa                                  | Acción                                                                 | Ejemplo aplicado                                                   |
|----------------------------------------|------------------------------------------------------------------------|--------------------------------------------------------------------|
| **I. Objetivos del negocio**           | Garantizar recepción de mensajes importantes por SMS.                 | Mensajes críticos desde WhatsApp entregados en móviles simples.    |
| **II. Alcance técnico**                | WhatsApp Web → Backend → Gateway SMS.                                 | Integración de sockets, APIs y bots de lectura.                    |
| **III. Flujo de datos**                | Captura → Validación → Conversión → Envío SMS.                        | Representado con DFD.                                              |
| **IV. Identificación de amenazas**     | Spoofing, sniffing, DoS al gateway.                                   | Basado en STRIDE.                                                  |
| **V. Análisis de vulnerabilidades**    | API abierta, backend expuesto, SMS sin cifrado.                       | Escaneo de vulnerabilidades técnicas y de diseño.                  |
| **VI. Simulación de ataques**          | Clonado de SIM, inyección SQL, token replay.                          | Se construyen árboles de ataque.                                  |
| **VII. Controles recomendados**        | PKI, monitoreo, autenticación fuerte.                                 | Defensa en profundidad.                                            |

---

## Lecciones Aprendidas

### 1. **Simplicidad tecnológica como ventaja**
> "Adaptar lo moderno a lo clásico también es innovar."

- Los móviles básicos aún son útiles.
- La inclusión debe considerar las limitaciones tecnológicas.
- Lo importante es el acceso, no la sofisticación.

### 2. **Integración responsable**
- WhatsApp no permite integraciones automatizadas.
- APIs no oficiales deben usarse con cautela.
- Riesgos legales, técnicos y éticos presentes.

### 3. **Threat Modeling desde el inicio**
- Anticipar amenazas desde el diseño evita sorpresas.
- PASTA ayuda a estructurar la seguridad desde la raíz.
- Árboles de ataque hacen tangible la superficie de exposición.

### 4. **El SMS no es seguro**
- Carece de cifrado extremo-a-extremo.
- Spoofing, SIM swap y sniffing son amenazas reales.
- Se necesita validación y cifrado desde el backend.

### 5. **Backend modular y robusto**
- El backend es el centro neurálgico del sistema.
- Separar funciones y responsabilidades mejora el control.
- Arquitectura por eventos permite escalabilidad y resiliencia.

### 6. **Equilibrio seguridad/usabilidad**
- No sobrecargar al usuario de móviles simples.
- Seguridad sin fricción debe ser una meta.
- Verificación automática sin interacción cuando sea posible.

### 7. **Colaboración multidisciplinar**
- Seguridad, desarrollo, diseño y usuario deben colaborar.
- Visiones cruzadas reducen errores.
- El Threat Modeling no es tarea de un solo rol.

---

## Conclusión

**What2SMS** es un proyecto que recupera el poder de la simplicidad, adaptando tecnología moderna a contextos clásicos. Pero al hacerlo, enfrenta desafíos de seguridad que deben tratarse desde el primer día.

El uso del **framework PASTA** junto al **Threat Model Process** asegura una arquitectura segura, comprensible y escalable. Este enfoque permite proteger sin complicar, innovar sin olvidar la accesibilidad, y avanzar sin dejar a nadie atrás.

---

> La seguridad no es un producto, sino un proceso continuo.
> La mejor defensa empieza en el diseño.

---
**Realizado por**: Gerardo Arca López 
**Fecha**: 21 de Abril de 2025

---
