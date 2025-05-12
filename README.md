# Setting Up the Management Network for OpenWISP

Este documento describe las estrategias recomendadas para establecer una red de gestión eficaz al utilizar OpenWISP para administrar dispositivos distribuidos. Se consideran dos escenarios principales: despliegue en red pública y despliegue en red privada.

---

## 🌐 Despliegue en Red Pública

En este escenario, el servidor OpenWISP está desplegado en un data center o entorno en la nube y **expuesto a la red pública** mediante una IP pública (IPv4 o IPv6) y un certificado SSL válido.

### Características del Escenario:

- Los dispositivos de red están distribuidos en diferentes ubicaciones y usualmente **detrás de una gateway NAT**.
- OpenWISP **no puede alcanzarlos directamente** a menos que exista un túnel de administración.

### Solución:

Se requiere una **VPN de administración** que permita a OpenWISP alcanzar los dispositivos remotos para tareas como monitoreo, debugging y troubleshooting.

#### Opciones de VPN sugeridas:

- WireGuard
- ZeroTier
- OpenVPN

El servidor VPN debe ser accesible desde el servidor OpenWISP y permitir la conexión de todos los dispositivos gestionados.

### Alternativa sin VPN:

Si la infraestructura de red ya permite conectividad directa (por ejemplo, usando **MPLS**, **SD-WAN** o soluciones de intranet existentes), **no es necesario un servidor VPN**, siempre y cuando los dispositivos:

- Tengan una interfaz dedicada con una IP alcanzable desde el servidor OpenWISP.
- Estén correctamente configurados para utilizar esa interfaz para gestión.

### Configuración Requerida en los Dispositivos:

- Los dispositivos deben ser configurados para **conectarse automáticamente al túnel de administración**.
- Esto se logra mediante plantillas preconfiguradas en el firmware o archivos de configuración personalizados.
- El agente de configuración de OpenWISP en los dispositivos debe especificar el **nombre de la interfaz de administración**, correspondiente a la creada por el túnel VPN.

---

## 🏠 Despliegue en Red Privada

Este escenario aplica cuando el servidor OpenWISP está directamente conectado a la **misma red privada** donde se encuentran los dispositivos.

### Características del Escenario:

- Todos los dispositivos y el servidor OpenWISP comparten el mismo segmento de red o red local.
- No se requiere VPN ni infraestructura externa.

### Configuración Requerida:

- OpenWISP debe configurarse para **aceptar conexiones mediante IPs privadas**.
- Debe utilizar el campo `Last IP` de cada dispositivo para enviar comandos o configuraciones.
- Es necesario establecer la variable:

```bash
OPENWISP_CONTROLLER_MANAGEMENT_IP_ONLY=false
````

---

## 📟 Configuración de Dispositivos con OpenWrt

Los dispositivos de red que se deseen administrar con OpenWISP deben **soportar OpenWrt**. Esto permite aprovechar el agente de configuración de OpenWISP y una integración completa con el sistema de control y automatización de la plataforma.

> ⚠️ Se recomienda generar imágenes de firmware personalizadas o usar plantillas de configuración para garantizar que cada dispositivo tenga las interfaces y parámetros necesarios para la administración remota.

---

## ✅ Requisitos Generales

* Certificado SSL válido (si se expone a Internet).
* Accesibilidad del servidor OpenWISP a las IPs de administración.
* Configuración automática del túnel de gestión (en redes públicas).
* Dispositivos con OpenWrt o compatibles.
* Variables clave correctamente definidas en el entorno de OpenWISP.

---

