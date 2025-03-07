# Informe de Seguridad: Análisis del Ataque SYN Flood ES ver. 🇪🇸

---

## Sección 1: Identificación del tipo de ataque

Como analistas de seguridad, hemos identificado que el tipo de ataque que ha causado la interrupción del servicio en el sitio web de la agencia de viajes es un **ataque de inundación SYN (SYN Flood)**. Este tipo de ataque se caracteriza por enviar una gran cantidad de solicitudes SYN al servidor, lo que consume sus recursos y le impide responder a las solicitudes legítimas.

### Evidencia en los registros de Wireshark

# Análisis de Tráfico TCP/HTTP

## Tráfico Legítimo (Verde)
| No.  | Time (seg) | Source          | Destination   | Protocolo | Info                          | Tipo       |
|------|------------|-----------------|---------------|-----------|-------------------------------|------------|
| <span style="color:green">47</span> | 3.144521 | 198.51.100.23 | 192.0.2.1 | TCP       | SYN (inicio handshake)        | Legítimo   |
| <span style="color:green">48</span> | 3.195755 | 192.0.2.1     | 198.51.100.23 | TCP       | SYN-ACK (respuesta servidor)  | Legítimo   |
| <span style="color:green">50</span> | 3.298223 | 198.51.100.23 | 192.0.2.1 | HTTP      | GET /sales.html               | Legítimo   |
| <span style="color:green">51</span> | 3.349457 | 192.0.2.1     | 198.51.100.23 | HTTP      | HTTP 200 OK                   | Legítimo   |

## Ataques (Rojo)
| No.  | Time (seg) | Source          | Destination   | Protocolo | Info                          | Tipo     |
|------|------------|-----------------|---------------|-----------|-------------------------------|----------|
| <span style="color:red">52</span> | 3.390692 | 203.0.113.0   | 192.0.2.1 | TCP       | SYN (sin ACK posterior)       | Ataque   |
| <span style="color:red">57</span> | 3.664863 | 203.0.113.0   | 192.0.2.1 | TCP       | SYN repetido (patrón flood)  | Ataque   |
| <span style="color:red">59</span> | 3.795332 | 203.0.113.0   | 192.0.2.1 | TCP       | SYN con payload anómalo       | Ataque   |

## Conexiones Fallidas (Amarillo)
| No.  | Time (seg) | Source          | Destination   | Protocolo | Info                          | Tipo      |
|------|------------|-----------------|---------------|-----------|-------------------------------|-----------|
| <span style="color:yellow">73</span> | 6.230548 | 192.0.2.1     | 198.51.100.16 | TCP       | RST, ACK (conexión interrumpida) | Fallida |
| <span style="color:yellow">77</span> | 7.330577 | 192.0.2.1     | 198.51.100.5 | HTTP      | 504 Gateway Timeout           | Fallida   |

---

### Patrones Detectados 🔍
1. **Tráfico Legítimo**:
   - Handshake TCP completo (`SYN → SYN-ACK → ACK`).
   - Peticiones HTTP normales con respuestas `200 OK`.

2. **Ataques**:
   - **SYN Flood**: IP `203.0.113.0` envía múltiples SYN sin completar el handshake.
   - **Payloads anómalos**: SYN con `Len=120` (no común en tráfico normal).

3. **Conexiones Fallidas**:
   - Respuestas `RST, ACK` del servidor (posible saturación).
   - Timeout HTTP `504` (bloqueo por ataque).



- Hemos observado un gran número de solicitudes SYN provenientes de una dirección IP desconocida (**203.0.113.0**). Esta IP pertenece al rango **203.0.113.0/24**, que está reservado para documentación y pruebas. Sin embargo, en un escenario real, esta IP podría ser falsa o estar enmascarada mediante técnicas como el uso de VPNs o suplantación de IP (*IP spoofing*).
- Estas solicitudes no completan el handshake de tres pasos (no envían el ACK final), lo que hace que el servidor reserve recursos para cada solicitud SYN sin liberarlos.
- Como resultado, el servidor se sobrecarga y no puede responder a las solicitudes legítimas de los empleados que intentan acceder al sitio web.

### Conclusión

El ataque SYN Flood ha provocado que el servidor se vea abrumado por el volumen de tráfico malicioso, lo que ha resultado en un error de **"tiempo de conexión agotado"** para los usuarios legítimos. Además, este tipo de ataque puede generar otros errores asociados, como:

- **Errores HTTP 504 (Gateway Timeout)**: El servidor no responde a tiempo debido a la sobrecarga.
- **Paquetes RST (Reset)**: El servidor cierra conexiones inesperadamente debido a la falta de recursos.

---

## Sección 2: Explicación del impacto del ataque

El ataque SYN Flood afecta directamente al funcionamiento del sitio web y a la experiencia de los usuarios. A continuación, explicamos cómo ocurre este ataque y sus efectos.

### Handshake de tres pasos de TCP

1. **SYN**: El cliente envía una solicitud SYN al servidor para iniciar la conexión.
2. **SYN-ACK**: El servidor responde con un SYN-ACK, indicando que está listo para establecer la conexión.
3. **ACK**: El cliente envía un ACK para confirmar la conexión.

En un ataque SYN Flood, el atacante envía muchas solicitudes SYN pero no completa el handshake, dejando al servidor esperando respuestas que nunca llegan.

### Sobrecarga del servidor

- Cada solicitud SYN hace que el servidor reserve recursos (memoria y CPU) para la conexión.
- Al no recibir el ACK final, estos recursos no se liberan, lo que eventualmente agota la capacidad del servidor.
- Como resultado, el servidor no puede responder a las solicitudes legítimas, lo que provoca el error de **"tiempo de conexión agotado"**.

### Impacto en la organización

- **Interrupción del servicio**: Los empleados no pueden acceder al sitio web para buscar paquetes de vacaciones, lo que afecta negativamente a la productividad y al negocio.
- **Pérdida de reputación**: Si los clientes no pueden acceder al sitio web, podrían perder confianza en la empresa.
- **Costos económicos**: La interrupción del servicio podría generar pérdidas económicas, especialmente si el sitio web es una fuente importante de ingresos.

### Medidas inmediatas

1. **Bloquear la IP atacante**: Configurar el firewall para bloquear la dirección IP sospechosa (203.0.113.0).
2. **Habilitar SYN Cookies**: Activar la opción de SYN Cookies en el servidor para evitar la reserva innecesaria de recursos.
3. **Notificar a los usuarios**: Informar a los empleados y clientes sobre la interrupción del servicio y las medidas que se están tomando para resolverla.

---

## Sección 3: Medidas de mitigación y prevención

Para proteger el servidor de futuros ataques SYN Flood, recomendamos implementar las siguientes medidas:

### 1. SYN Cookies

- El servidor no reservará recursos para cada solicitud SYN. En su lugar, enviará un SYN-ACK con un "cookie" (un valor calculado a partir de la información de la solicitud).
- Solo si el cliente responde con un ACK válido, el servidor reservará recursos para la conexión.

### 2. Firewalls y filtros de tráfico

- **Configurar reglas en el firewall**: Bloquear direcciones IP sospechosas o que envíen un número anormal de solicitudes SYN.
- **Usar iptables en Linux**: Esta herramienta permite limitar el número de conexiones SYN por segundo desde una misma IP. Por ejemplo:
    
    iptables -A INPUT -p tcp --syn -m limit --limit 1/s -j ACCEPT
 
 - **Este comando limita a 1 solicitud SYN por segundo desde una misma IP.**
  

### 3. Balanceadores de carga

Distribuir el tráfico entre múltiples servidores para evitar que uno solo se sobrecargue. Esto también ayuda a mitigar ataques DDoS.

### 4. Sistemas de detección de intrusiones (IDS)

- Monitorear el tráfico en busca de patrones sospechosos, como un gran número de solicitudes SYN desde una misma IP.
- Bloquear automáticamente el tráfico malicioso.

### 5. Rate Limiting

- Limitar el número de solicitudes SYN que el servidor acepta por segundo desde una misma IP.
- Esto evita que un atacante inunde el servidor con solicitudes.

---

## Sección 4: Consecuencias del ataque

El ataque SYN Flood ha tenido las siguientes consecuencias negativas para la organización:

- **Interrupción del servicio**: Los empleados no pueden acceder al sitio web, lo que afecta a la productividad y al negocio.
- **Pérdida de reputación**: Si los clientes no pueden acceder al sitio web, podrían perder confianza en la empresa.
- **Costos económicos**: La interrupción del servicio podría generar pérdidas económicas, especialmente si el sitio web es una fuente importante de ingresos.

### Cómo informar a los usuarios

- **Comunicado oficial**: Enviar un correo electrónico o publicar un mensaje en la página web informando sobre la interrupción y las medidas que se están tomando.
- **Transparencia**: Explicar que el problema se debe a un ataque cibernético y que se está trabajando para resolverlo lo antes posible.

---

## Sección 5: Recomendaciones adicionales

### Capacitación del personal

- Asegurarse de que el equipo de TI esté capacitado para identificar y responder a este tipo de ataques.
- Un Entry Level Cybersecurity Analyst podría encargarse de monitorear el tráfico y aplicar medidas básicas de seguridad.

### Monitoreo continuo

- Implementar herramientas de monitoreo en tiempo real, como Nagios, Zabbix o Wireshark, para detectar y mitigar ataques de manera proactiva.
- Establecer alertas automáticas para notificar sobre actividades sospechosas.

### Plan de respuesta a incidentes

- Desarrollar un plan claro para responder a futuros incidentes de seguridad.

