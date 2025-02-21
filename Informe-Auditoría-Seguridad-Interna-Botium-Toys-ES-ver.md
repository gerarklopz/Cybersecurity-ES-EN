# ES ver.🇪🇸
# Informe de Auditoría Interna de Seguridad para Botium Toys

## Contexto y Escenario

Botium Toys es una pequeña empresa de EE. UU. especializada en el desarrollo y la venta de juguetes. Con la expansión de su presencia en línea, la empresa ha experimentado un rápido crecimiento, atrayendo clientes tanto a nivel nacional como internacional. Como resultado, el departamento de Tecnología de la Información (TI) enfrenta una creciente presión para apoyar este mercado en línea ampliado.

La gerente del departamento de TI decidió llevar a cabo una auditoría interna de TI para mantener el cumplimiento de las normativas y asegurar la continuidad del negocio a medida que la empresa crece. La auditoría se centra en identificar y mitigar riesgos, amenazas o vulnerabilidades de activos críticos. Una preocupación particular es asegurar el cumplimiento con la normativa interna de procesamiento de pagos y las regulaciones internacionales como el Reglamento General de Protección de Datos (GDPR) en la Unión Europea (UE), en caso de manejar datos de ciudadanos de la UE.

La auditoría se está llevando a cabo utilizando el **Marco de Ciberseguridad del Instituto Nacional de Estándares y Tecnología (NIST CSF)**. Este marco ayuda a identificar los riesgos existentes y las brechas en la postura de seguridad de la empresa. La auditoría también tiene como objetivo evaluar si los controles necesarios están en su lugar y determinar el impacto de no cumplir con las normativas pertinentes.

## Objetivo de la Auditoría

- **Revisar el alcance, los objetivos y el informe de evaluación de riesgos** generado por la gerente de TI y el equipo auditor.
- **Realizar una auditoría interna** completando la lista de controles y cumplimiento.
- **Evaluar la postura de seguridad** de Botium Toys y determinar las áreas donde se deben implementar controles para mejorar la seguridad y el cumplimiento.

---

# Lista de Controles y Cumplimiento

## Controles de Evaluación

| Check | Control |
|---|---|
| ✅ | Menor privilegio |
| ❌ | Planes de recuperación ante desastres |
| ❌ | Políticas de contraseña |
| ✅ | Separación de funciones |
| ✅ | Firewall |
| ❌ | Sistema de detección de intrusos (IDS) |
| ❌ | Copias de seguridad |
| ✅ | Software antivirus |
| ❌ | Monitoreo, mantenimiento e intervención manual para sistemas heredados |
| ❌ | Cifrado de datos sensibles |
| ❌ | Sistema de gestión de contraseñas |
| ✅ | Cerraduras (oficinas, tiendas, almacén) |
| ✅ | Videovigilancia CCTV |
| ✅ | Detección y prevención de incendios (alarma contra incendios, sistema de rociadores, etc.) |

### **Estándar de Seguridad de Datos para la Industria de Tarjetas de Pago (PCI DSS)**

| Check | Mejor práctica |
|---|---|
| ✅ | Solo los usuarios autorizados tienen acceso a la información de la tarjeta de crédito de los clientes. |
| ❌ | La información de la tarjeta de crédito se almacena, acepta, procesa y transmite internamente en un entorno seguro. |
| ❌ | Se implementan procedimientos de cifrado de datos para mejorar la seguridad de las transacciones y datos de tarjetas de crédito. |
| ❌ | Se aplican políticas seguras de gestión de contraseñas. |

### **Respuestas a las Preguntas de la Tarea**

### **Explicación de Controles SOC 1 y SOC 2**

Estos controles garantizan la seguridad, integridad y disponibilidad de los datos. Son esenciales para demostrar el compromiso de Botium Toys con la seguridad en sus operaciones y proteger la información crítica de clientes y empleados.

| Check | Mejor práctica |
|---|---|
| ✅ | Se establecen políticas de acceso de usuarios. |
| ✅ | Los datos sensibles (PII/SPII) son confidenciales/privados. |
| ✅ | La integridad de los datos asegura que estos sean consistentes, completos, precisos y validados. |
| ✅ | Los datos están disponibles para las personas autorizadas a acceder a ellos. |

---

## Conclusiones Finales y Recomendaciones

1. **Implementar un sistema de control de accesos basado en el principio de menor privilegio**.
2. **Fortalecer los firewalls y los sistemas antivirus** con configuraciones personalizadas y actualizaciones periódicas.
3. **Cifrar los datos sensibles**, como información de tarjetas de crédito y PII, utilizando algoritmos estándar de la industria como AES-256.
4. **Mejorar la seguridad física** con controles de acceso biométricos o con tarjetas de identificación.
5. **Desarrollar e implementar un plan de recuperación ante desastres (DRP)**, incluyendo la creación de copias de seguridad completas, incrementales y diferenciales.
6. **Fortalecer la política de contraseñas**, exigiendo contraseñas complejas y aplicando autenticación multifactor (MFA).

Con la implementación de estos controles y mejores prácticas, Botium Toys podrá mejorar significativamente su postura de seguridad y cumplir con las regulaciones relevantes, protegiendo tanto sus activos como los datos de sus clientes.

--










