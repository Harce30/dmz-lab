
# Informe de configuración de DMZ con Cisco Packet Tracer


### 1. Objetivo del laboratorio

> Explica brevemente qué se buscaba lograr con este laboratorio.

El objetivo principal de este laboratorio fue diseñar e implementar una arquitectura de red segura con una zona desmilitarizada que permitiera utilizar un servidor web en internet sin comprometer la seguridad de la red interna.

**Ejemplo:**  
Configurar una DMZ segura usando un router Cisco ISR, aplicando NAT y ACLs para controlar el tráfico entre LAN, DMZ y red externa.

---

### 2. Topología implementada

> Describe la red. Puedes incluir una imagen si el software lo permite (captura de Packet Tracer).

- Cantidad de redes: 3 Redes.
- Dispositivos usados: 
  - 1 Router Cisco ISR.
  - 3 Switches Cisco 2960.
  - 1 PC en la LAN Interna.
  - 1 Servidor web en la DMZ.
  - 1 PC en la red externa.
- Breve descripción de la función de cada zona (LAN, DMZ, Externa).
  - Red Interna: Es la red privada de la organización donde se encuentran los usuarios y equipos internos.
  - Zona Desmilitarizada(DMZ): Es una red intermedia destinada a alojar servicios públicos, como el servidor web. Permite que los equipos internos accedan a este servicio sin ser expuestos.
  - Red Externa: Es la red pública desde donde los usuarios externos acceden a los servicios publicados.



### 3. Plan de direccionamiento IP

Completa la tabla con las IPs asignadas (puedes copiarla del enunciado si no cambió).

| Dispositivo             | IP              | Máscara           | Gateway           |
|-------------------------|------------------|-------------------|-------------------|
| PC_Internal             |192.168.1.10      |255.255.255.0                   |192.168.1.1        |
| Server_DMZ              |192.168.2.10      |255.255.255.0                   |192.168.2.1        |
| PC_External             |192.168.3.10      |255.255.255.0                   |192.168.3.1        |
| Router_FW Gi0/0 (LAN)   |192.168.1.1       |255.255.255.0                   |                   |
| Router_FW Gi0/1 (DMZ)   |192.168.2.1       |255.255.255.0                   |                   |
| Router_FW Gi0/2 (Ext)   |192.168.3.1       |255.255.255.0                   |                   |


### 4. Configuración aplicada (resumen)

> Resume los comandos o pasos más relevantes que ejecutaste. Usa texto + fragmentos de código cuando sea necesario.

- Interfaces configuradas con `ip address`
Se asignaron direcciones IP a cada interfaz del router para interconectar las tres redes:

  - PC_Interno:
    interface GigabitEthernet0/0
    ip address 192.168.1.1 255.255.255.0
  
  - Servidor_DMZ:
    interface GigabitEthernet0/1
    ip address 192.168.2.1 255.255.255.0

  - PC_Externo:
    interface GigabitEthernet0/2
    ip address 192.168.3.1 255.255.255.0

- NAT:
```bash
ip nat inside source static 192.168.2.10 192.168.3.1
```
Esto permite que los clientes exterrnos accedan al servidor web utilizando una IP pública.



- ACLs:
```bash
access-list 101 permit tcp any host 192.168.3.1 eq 80
access-list 100 deny ip 192.168.2.0 0.0.0.255 192.168.1.0 0.0.0.255
```

access-list 101 permite únicamente el HTTP desde internet hacia el servidor web

access-list 100 bloquea el tráfico desde la DMZ hacia la LAN interna (PC_Interno)


### 5. Verificaciones realizadas

> Describe las pruebas y su resultado. Incluye capturas o salidas de comandos si se puede.

- `ping` desde PC_Internal al router: ✅
El ping exitoso confirma que la conexión entre la LAN y el router es satisfactoria.

- Acceso web desde PC_External: ✅
La página web del servidor DMZ cargó correctamente desde el PC_External.

- Bloqueo de acceso desde DMZ a LAN: ✅
El bloqueo se realizó correctamente, ya que la comunicación desde la DMZ hacia la LAN fue denegada.


### 6. Conclusiones y recomendaciones

> ¿Qué aprendiste con este ejercicio? ¿Qué mejorarías?

El ejercicio me permitió comprender cómo segmentar una red mediante una arquitectura DMZ.

**Ejemplo:**
Aprendí a aplicar NAT y ACLs en un entorno simulado. Recomiendo verificar conectividad básica antes de aplicar reglas de firewall, ya que un error en la IP puede bloquear todo.


### 7. Capturas de evidencia

> Adjunta aquí (o en un PDF anexo) las capturas solicitadas: pings, navegador, comandos `show`, etc.
