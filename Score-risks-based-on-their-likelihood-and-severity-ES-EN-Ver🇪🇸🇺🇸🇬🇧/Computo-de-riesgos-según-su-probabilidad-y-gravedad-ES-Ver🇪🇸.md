# **Evaluación de Riesgos: Análisis de la Administración del Arzobispado ES-Ver🇪🇸**  
*Kangodrila Solutions - Informe de Evaluación de Riesgos*  

## **1. Descripción del Entorno Operacional**  
El Arzobispado de X Ciudad enfrenta importantes desafíos en la gestión de sus datos financieros, registros eclesiásticos y comunicaciones con la Santa Sede. Actualmente, la institución atraviesa dificultades administrativas que han generado errores críticos con un impacto potencial en la transparencia, seguridad y eficiencia operativa.  

Entre los factores identificados, destaca la falta de formación en tecnologías de la información (TI) del personal religioso encargado de estas gestiones, quienes se encuentran especializados en Derecho Canónico y Teología. En paralelo, diversos laicos y colaboradores con experiencia en TI han manifestado preocupaciones sobre la situación actual. La Santa Sede ha tomado conocimiento de estas dificultades y se prevé la llegada de delegados para su evaluación, en paralelo con el seguimiento de la Conferencia Episcopal Española.  

## **2. Implementación del Framework NIST/CSF**  
El **NIST Cybersecurity Framework (CSF)** es un marco de referencia ampliamente utilizado para mejorar la ciberseguridad organizacional. Su implementación permitirá al Arzobispado estructurar mejor sus procesos de seguridad digital y avanzar hacia un modelo de protección más maduro y eficiente. El marco se compone de tres elementos clave:  

- **Core**: Define cinco funciones esenciales para la gestión del riesgo cibernético: **Identificar, Proteger, Detectar, Responder y Recuperar**. Estas funciones ayudarán a comprender y abordar las amenazas de manera estructurada.  
- **Tiers**: Evalúa el nivel actual de madurez en ciberseguridad, clasificándolo en uno de cuatro niveles: **Tier 1 (Parcial), Tier 2 (Riesgo Gestionado), Tier 3 (Riesgo Mitigado) y Tier 4 (Adaptable)**. Esto permitirá conocer la preparación del Arzobispado en seguridad digital.  
- **Profiles**: Determina el perfil de seguridad más adecuado en función de las necesidades y riesgos específicos de la institución, permitiendo una personalización del marco a la realidad del Arzobispado.  

### **Desglose de las funciones clave del NIST CSF**

1. **Identificar**:  
Es fundamental conocer los activos digitales del Arzobispado, evaluar los riesgos existentes y entender las vulnerabilidades presentes. Esto incluye la elaboración de un inventario de sistemas y datos críticos, así como la definición de responsabilidades dentro de la organización. Un ejemplo específico sería identificar las bases de datos de los registros sacramentales y clasificarlos según su nivel de importancia.

2. **Proteger**:  
Se deben establecer medidas para resguardar la información y los sistemas de posibles ataques o fallos. Algunas estrategias clave incluyen la autenticación multifactor, la encriptación de datos y la formación del personal en buenas prácticas de seguridad. Por ejemplo, la implementación de políticas de acceso restringido y la utilización de contraseñas seguras en el sistema de gestión financiera.

3. **Detectar**:  
Implementar mecanismos que permitan la identificación rápida de incidentes de seguridad. Esto abarca el monitoreo continuo de redes, el uso de herramientas de detección de intrusos y la generación de alertas ante comportamientos sospechosos. En el caso del Arzobispado, se podrían utilizar herramientas de monitoreo para detectar intentos de acceso no autorizado a bases de datos sensibles.

4. **Responder**:  
Contar con planes de acción detallados para mitigar el impacto de un incidente de seguridad. Esto implica definir protocolos de comunicación, asignar roles en caso de crisis y coordinar respuestas efectivas ante ataques o fallos del sistema. El Arzobispado debería contar con un protocolo de comunicación claro con la Santa Sede en caso de incidentes.

5. **Recuperar**:  
Desarrollar estrategias para restaurar los sistemas afectados y minimizar las interrupciones en las operaciones del Arzobispado. Entre las medidas recomendadas se incluyen la implementación de backups regulares y la creación de un plan de continuidad del negocio. En este caso, la restauración de registros eclesiásticos a partir de backups en la nube debería ser una prioridad.

## **3. Identificación de Riesgos y Evaluación**  
A continuación, se detallan los principales riesgos detectados, con sus respectivos valores de **Probabilidad (1-3)** y **Severidad (1-3)**, conforme al modelo de evaluación de riesgos. La probabilidad refleja la frecuencia estimada de ocurrencia del riesgo, mientras que la severidad mide el impacto que este podría generar en la operatividad de la institución. La combinación de ambos valores permite determinar la prioridad de intervención.  

| **Riesgo** | **Descripción** | **Probabilidad (1-3)** | **Severidad (1-3)** | **Prioridad (P x S)** |
|------------|------------------|--------------------|-----------------|------------------|
| **1. Desorden en la Gestión Financiera** | Errores en el registro de donaciones durante un mes, afectando la transparencia y auditabilidad. | 3 | 3 | **9 (Crítico)** |
| **2. Pérdida de la Base de Datos de Bautizados (1999)** | Eliminación accidental de registros sacramentales esenciales. La situación se agrava por el uso de licencias de Oracle caducadas y la ausencia de soporte técnico. | 3 | 3 | **9 (Crítico)** |
| **3. Envío Erróneo de Documentación a la Santa Sede** | Información incorrecta sobre un sacerdote enviada al Vaticano. Se identificó que el correo utilizado es una cuenta Gmail sin protocolos de seguridad ni estandarización. | 2 | 2 | **4 (Moderado)** |
| **4. Falta de Backups y Seguridad en Datos** | Ausencia de copias de seguridad actualizadas en registros sensibles, comprometiendo la integridad de la información. | 3 | 3 | **9 (Crítico)** |
| **5. Ausencia de Procedimientos de Validación** | No existen procesos de doble verificación en la administración de finanzas y documentación oficial. | 2 | 3 | **6 (Alto)** |

## **4. Plan de Mitigación de Riesgos**  
Para abordar los riesgos identificados, Kangodrila Solutions recomienda la implementación de las siguientes medidas correctivas:  

### **🔹 Para la Gestión Financiera:**  
✅ Implementación de un software de contabilidad con auditoría automática.  
✅ Establecimiento de un protocolo de doble verificación previo al cierre contable mensual.  
✅ Capacitación del personal en gestión financiera y normativas de auditoría.  

### **🔹 Para la Pérdida de Datos en Registros:**  
✅ Recuperación con herramientas especializadas en recuperación forense de datos.  
✅ Implementación de backups automáticos en la nube con redundancia geográfica.  
✅ Creación de copias de seguridad offline para documentación crítica.  
✅ Regularización de licencias de software y contratación de soporte técnico especializado.  

### **🔹 Para la Gestión de Comunicaciones con la Santa Sede:**  
✅ Validación obligatoria por el Vicario General antes de enviar información oficial.  
✅ Implementación de un sistema de firma digital y encriptación de documentos.  
✅ Creación de un correo institucional con dominio oficial y aplicación de políticas de gestión segura de correos electrónicos.  

### **🔹 Para la Seguridad de Datos y Backups:**  
✅ Desarrollo e implementación de un Sistema de Gestión de Seguridad de la Información (SGSI).  
✅ Uso de cifrado avanzado en bases de datos y establecimiento de acceso restringido con autenticación multifactor.  
✅ Auditoría interna trimestral para validar el cumplimiento de protocolos de seguridad.  

### **🔹 Para la Validación de Documentación:**  
✅ Establecimiento de un protocolo de revisión en tres niveles antes de aprobar registros financieros y documentales.  
✅ Creación de un Comité de Control de Calidad documental para garantizar la exactitud de la información.  
✅ Implementación de un sistema de tickets para el seguimiento y trazabilidad de gestiones administrativas.  

## **5. Conclusión**  
La falta de estandarización de procesos y la carencia de herramientas tecnológicas adecuadas están generando riesgos significativos en la administración del Arzobispado. La aplicación de medidas correctivas inmediatas permitirá mejorar la transparencia, eficiencia y seguridad en la gestión documental y financiera.  

**Kangodrila Solutions** se compromete a brindar un plan de acción integral para la modernización de los procesos administrativos del Arzobispado, garantizando un entorno seguro y eficaz acorde con los estándares internacionales.

