# Informe de Incidente de Seguridad - Ataque DDoS 🇪🇸

## 1. Resumen Ejecutivo

Este informe documenta un incidente de seguridad ocurrido en la red interna de una empresa del campo de la multimedia, que ofrece servicios de diseño web, diseño gráfico y soluciones de marketing en redes sociales para pequeñas empresas. Dicha empresa sufrió un ataque de **Denegación de Servicio Distribuido (DDoS)**, que comprometió la disponibilidad de los servicios durante dos horas. El ataque consistió en una inundación de paquetes **ICMP** (Protocolo de Mensajes de Control de Internet), lo que saturó la red e impidió el acceso a los recursos críticos.

El incidente fue causado por una configuración deficiente del **firewall**, que permitió a un actor malicioso enviar una gran cantidad de solicitudes ICMP, sobrecargando la red. Como respuesta, el equipo de gestión de incidentes bloqueó el tráfico ICMP, restauró los servicios críticos e implementó medidas adicionales para prevenir futuros ataques.

Se recomienda la implementación de **reglas de firewall más estrictas**, la verificación de direcciones IP, y la adopción de herramientas de monitoreo y detección de intrusiones para fortalecer la seguridad de la red, siguiendo el marco **NIST CSF** (Marco de Ciberseguridad del Instituto Nacional de Estándares y Tecnología).

---

## 2. Descripción del Incidente

**Fecha del incidente:** 2025-03-10  
**Empresa afectada:** Empresa multimedia de diseño web, gráfico y soluciones de marketing.  
**Área impactada:** Red interna, servidores web, servidores de correo y bases de datos.

### 2.1. Detalle del Ataque

- Un atacante explotó una vulnerabilidad en el **firewall**, enviando una inundación de paquetes ICMP a la red interna.
- El tráfico malicioso saturó la red, impidiendo que los empleados accedieran a los recursos críticos, como servidores web, correo electrónico y bases de datos.
- El ataque duró aproximadamente dos horas, durante las cuales los servicios estuvieron parcial o totalmente inaccesibles.

### 2.2. Respuesta Inmediata

El equipo de seguridad respondió de la siguiente manera:

1. **Contención**: Se bloquearon los paquetes ICMP entrantes utilizando una regla de firewall específica y se desconectaron los servicios no críticos para reducir la carga en la red.
2. **Restauración**: Se priorizó la recuperación de los servicios críticos, como el correo electrónico y los servidores web, utilizando copias de seguridad recientes.
3. **Investigación**: Se identificó que el firewall no estaba correctamente configurado, lo que permitió el ataque. Se revisaron los logs de red para rastrear el origen del tráfico malicioso.

---

## 3. Análisis del Incidente

### 3.1. Identificación del Ataque

- **Tipo de ataque**: Denegación de Servicio Distribuido (DDoS) mediante inundación de paquetes ICMP.
- **Sistemas afectados**: Servidores web, servidores de correo y bases de datos.
  - **Consecuencias**: 
    - **Servidores web**: Los sitios web de los clientes estuvieron inaccesibles, lo que podría haber afectado negativamente a su reputación y pérdida de ingresos.
    - **Servidores de correo**: La interrupción del servicio de correo electrónico impidió la comunicación interna y con clientes.
    - **Bases de datos**: Se detectaron intentos de acceso no autorizado a datos sensibles, aunque no se confirmó una filtración.
- **Vulnerabilidad explotada**: Firewall mal configurado que permitió el tráfico ICMP no filtrado.

### 3.2. Impacto del Incidente

- **Disponibilidad**: Los servicios internos estuvieron inaccesibles durante dos horas, afectando las operaciones diarias.
- **Reputación**: La interrupción del servicio podría afectar la confianza de los clientes, especialmente si se repite en el futuro.
- **Costos**: Pérdida de productividad y posibles costos asociados con la recuperación y mejora de la infraestructura.

---

## 4. Plan de Mejoras y Recomendaciones

### 4.1. Protección (NIST CSF: **Protect**)

- **Implementar reglas de firewall más estrictas**: Limitar el tráfico ICMP y verificar las direcciones IP de origen para prevenir ataques de suplantación (spoofing).
- **Actualizar políticas de seguridad**: Realizar auditorías regulares para identificar y corregir vulnerabilidades en la red.
- **Capacitación del personal**: Ofrecer formación a los empleados sobre cómo reconocer y responder a posibles amenazas de seguridad.

### 4.2. Detección (NIST CSF: **Detect**)

- **Monitoreo continuo de la red**: Implementar herramientas de monitoreo para detectar patrones de tráfico anómalos.
- **Sistema de Detección de Intrusiones (IDS)**: Utilizar un IDS para identificar y alertar sobre actividades sospechosas en la red.
- **Revisión de logs**: Analizar regularmente los registros de red para detectar intentos de acceso no autorizados.

### 4.3. Respuesta (NIST CSF: **Respond**)

- **Plan de respuesta a incidentes**: Desarrollar un protocolo claro para contener y neutralizar futuros ataques.
- **Comunicación con stakeholders**: Establecer un proceso para informar a los clientes y empleados sobre incidentes de seguridad.
- **Análisis post-incidente**: Realizar una revisión detallada después de cada incidente para identificar áreas de mejora.

### 4.4. Recuperación (NIST CSF: **Recover**)

- **Restauración de sistemas**: Asegurar que los sistemas afectados se restauren rápidamente a partir de copias de seguridad.
- **Verificación de integridad**: Confirmar que los datos y sistemas restaurados no han sido comprometidos.
- **Mejora de procesos de recuperación**: Implementar un plan de recuperación ante desastres (DRP) para minimizar el tiempo de inactividad en futuros incidentes.

---

## 5. Conclusión

Este incidente de seguridad subraya la importancia de contar con una infraestructura de red bien configurada y protegida. Aunque el ataque fue contenido y los servicios restaurados, es crucial implementar medidas adicionales para prevenir futuros incidentes. La adopción de un enfoque proactivo, basado en el marco **NIST CSF**, permitirá fortalecer la postura de seguridad y garantizar la continuidad del negocio.

Se recomienda proceder con las acciones correctivas de inmediato y establecer un plan de seguridad a largo plazo para evitar incidentes similares.

**Firma:**  
Gerardo Arca López  
**Cargo:** Analista de Ciberseguridad (estudiante en prácticas)  
**Fecha:** 2025-03-10
