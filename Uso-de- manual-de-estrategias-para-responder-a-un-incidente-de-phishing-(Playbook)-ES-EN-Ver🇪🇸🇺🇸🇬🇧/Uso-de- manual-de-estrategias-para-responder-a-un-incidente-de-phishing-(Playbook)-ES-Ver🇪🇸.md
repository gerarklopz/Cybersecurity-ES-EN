# Uso de un manual de estrategias para responder a un incidente de phishing (Playbook)ES-Ver🇪🇸

## Descripción General

En este informe, debemos responder a un incidente de phishing relacionado con un hash de archivo malicioso (SHA256).

## Contexto

Como ya sabemos, los playbooks detallan los pasos necesarios para gestionar eficazmente incidentes de seguridad. Su objetivo es coordinar acciones rápidas y efectivas, minimizar el impacto del incidente y reducir el tiempo de respuesta. Como analista de seguridad, estas guías nos ayudan a responder de forma sistemática y alineada con las políticas internas de la organización.

## Escenario de la Actividad/Incidente

Como futuros analistas de nivel uno en el centro de operaciones de seguridad (SOC) de una organización, con anterioridad se ha recibido una alerta de phishing relacionada con un archivo sospechoso descargado en el equipo de un empleado.

Tras analizar el hash del archivo adjunto al correo, se confirmó que el archivo es malicioso.

La tarea a realizar es seguir el procedimiento definido en la organización para completar la investigación y resolver la alerta.

## Resolución del Ticket de Alerta

### Detalles del Ticket de Alerta de Phishing

| Campo             | Detalle                                                                                                                                   |
| ----------------- | ----------------------------------------------------------------------------------------------------------------------------------------- |
| Ticket ID         | A-2703                                                                                                                                    |
| Mensaje de alerta | SERVER-MAIL Phishing attempt – possible download of malware                                                                               |
| Gravedad          | Media                                                                                                                                     |
| Estado del ticket | Abierto                                                                                                                                   |
| Hash malicioso    | `54e6ea47eb04634d3e87fd7787e2136ccfbcc80ade34f246a12cf93bab527f6b`                                                                        |
| Correo sospechoso | De: Def Communications `<76tguyhh6tgftrt7tg.su>`<br>IP origen: `114.114.114.114`<br>Para: `hr@inergy.com`<br>IP destino: `176.157.125.93` |
| Asunto            | "Re: Infrastructure Egnieer role"                                                                                                         |
| Fecha del correo  | 20 de julio de 2022 – 09:30 AM                                                                                                            |
| Archivo adjunto   | `bfsvc.exe` (protegido con contraseña: `paradise10789`)                                                                                   |

### Evaluación según Phishing Playbook

### Paso 1: Evaluar la alerta

Análisis del contenido del correo sospechoso:

| Elemento         | Observaciones                                                                                                 |
| ---------------- | ------------------------------------------------------------------------------------------------------------- |
| Gravedad         | Media → Posible escalamiento según el playbook                                                                |
| Remitente        | Dominio sospechoso + IP genérica (`114.114.114.114`) → posible suplantación                                   |
| Mensaje          | Contiene errores gramaticales → indicio común de phishing (e.g., asunto: "Re: Infrastructure 'Egnieer' role") |
| Adjunto          | `.exe` protegido con contraseña → técnica usada para evadir antivirus                                         |
| Hash del archivo | `54e6ea47eb04634d3e87fd7787e2136ccfbcc80ade34f246a12cf93bab527f6b` → malicioso                                |

### Paso 2: Identificación básica del incidente

### 5W : Who, What, When, Where, Why?

| Pregunta  | Respuesta                                                                                |
| --------- | ---------------------------------------------------------------------------------------- |
| ¿Quién?   | Clyde West (supuesto candidato) desde `<76tguyhh6tgftrt7tg.su>`                          |
| ¿Qué?     | Envío de correo con archivo `.exe` malicioso confirmado por hash                         |
| ¿Cuándo?  | Miércoles 20 de julio de 2022, a las 09:30 AM                                            |
| ¿Dónde?   | Dispositivo de Recursos Humanos de Inergy (IP destino: `176.157.125.93`)                 |
| ¿Por qué? | Intento de ejecución de código malicioso a través de un archivo adjunto disfrazado de CV |

### Paso 3: ¿Contiene adjuntos? ¿Son maliciosos?

* Sí, contiene un archivo .exe adjunto.
* Sí, el hash está confirmado como malicioso por herramientas como VirusTotal.
* Se justifica el escalamiento del ticket.

Comentarios para el Ticket

Descripción breve del incidente:
Se recibió un correo sospechoso dirigido al equipo de RRHH desde una dirección desconocida, con un archivo adjunto ejecutable (.exe) protegido por contraseña.

Motivos para escalar el incidente:

1. El archivo adjunto (`bfsvc.exe`) posee un hash previamente verificado como malicioso: `54e6ea47eb04634d3e87fd7787e2136ccfbcc80ade34f246a12cf93bab527f6b`.
2. El remitente utiliza un dominio e IP no asociados a ninguna entidad legítima conocida.
3. El contenido del mensaje presenta errores ortográficos y gramaticales, además de seguir patrones típicos de phishing (archivo ejecutable disfrazado de CV).

Acción tomada:
El ticket ha sido escalado para revisión por parte de un analista SOC de nivel 2.

Estado del Ticket:
Cambiar de "Open" a Escalated

## Diagrama de Flujo – Propuesta Visual

```
+------------------------------+
| Paso 1: Recibir ticket de    |
|         alerta de phishing   |
+--------------+---------------+
               |
               v
+--------------+---------------+
| Paso 2: Evaluar la alerta    |
| - Gravedad                  |
| - Remitente/Destinatario    |
| - Asunto/Cuerpo del mensaje |
| - Enlaces o adjuntos        |
+--------------+---------------+
               |
               v
+--------------+---------------+
| Paso 3.0: ¿Hay enlaces o     |
|           archivos adjuntos?|
+-------+---------+------------+
        |         |
        | No      | Sí
        v         v
  [Paso 4]   +----------------------------+
             | Paso 3.1: ¿Son maliciosos?|
             +-------------+-------------+
                           |
          +-----------------------------+
          | Sí                          |
          v                             v
[Paso 3.2: Escalar]              [Paso 4: Cerrar]
```

---

## Reflexión Crítica / Observaciones Finales

Como analista de nivel 1, tras revisar este caso y simular una respuesta siguiendo el playbook, surgen algunas dudas razonables sobre el flujo y la estructura operativa ante este tipo de incidentes:

### 1. Falta de filtrado previo antes de llegar al SOC

* Observación: Resulta sorprendente que un correo sospechoso llegue directamente a la vista de un analista SOC nivel 1 sin haber pasado por filtros previos más eficientes (como gateways de correo seguros, DLP o sandboxing automatizado).
* Comentario técnico: En entornos bien estructurados, el correo debería haber sido interceptado por soluciones como Proofpoint, Mimecast o Microsoft Defender for Office 365, incluso antes de generar un ticket. Si esto no ocurre, hay una brecha clara en los sistemas de protección perimetral.

### 2. ¿Por qué no confirmar primero con RRHH la legitimidad del remitente?

* Observación: Si el remitente se presenta como un candidato, ¿por qué no se valida inmediatamente con el departamento de Recursos Humanos si estaban esperando tal comunicación?
* Buena práctica recomendada: Antes de invertir tiempo en analizar el correo como amenaza técnica, podría ser eficiente validar si “Clyde West” está en algún proceso de selección o si fue contactado previamente.

### 3. Confianza ciega en VirusTotal

* Crítica: Si bien VirusTotal es una herramienta útil, no debería ser la única fuente de validación. Un hash marcado como malicioso requiere más contexto:

  * ¿Qué motores lo marcaron como malicioso?
  * ¿Cuál fue la fecha del análisis?
  * ¿Coincide con campañas conocidas?
* Mejor enfoque: Correlacionar con otras fuentes como Hybrid Analysis, Any.run o incluso Threat Intel feeds propios de la organización.

### 4. ¿Quién es Clyde West y cómo obtuvo los correos corporativos?

* Cuestión clave: ¿Es una suplantación de identidad o hay fuga de datos previa? ¿Cómo obtuvo este supuesto candidato la dirección directa de RRHH? ¿Es una lista filtrada?
* Riesgo latente: Esto podría formar parte de una campaña más amplia de spear phishing, donde se utilizan datos reales de empleados o departamentos.

### Conclusión General Personal

Aunque el playbook ayuda a estructurar una respuesta técnica ordenada, no puede sustituir el juicio crítico, la conciencia situacional y la validación entre departamentos. La ciberseguridad no se trata solo de hashes o archivos maliciosos, sino también de contexto humano y organizacional. En este caso, muchas preguntas quedan abiertas: ¿fallaron los filtros?, ¿hay un error de procedimiento?, ¿cómo fue posible este contacto externo? Por eso, incluso en niveles operativos, debemos cuestionar cada paso y no asumir nada como “normal”.

---

Realizado por: Gerardo Arca López

Fecha: 16 de Mayo de 2024

