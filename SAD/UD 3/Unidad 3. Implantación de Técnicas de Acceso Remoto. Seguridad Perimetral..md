
En la unidad anterior centramos nuestros esfuerzos en el interior de la infraestructura: aprendimos a fortificar el sistema operativo (_hardening_), analizar software malicioso y desplegar sensores de detección y prevención de intrusiones (IDS/IPS) capaces de interceptar amenazas que ya habían cruzado nuestras defensas. Sin embargo, una organización moderna no es una isla desconectada. Las empresas necesitan publicar servicios web hacia clientes externos, interconectar delegaciones geográficas y permitir que empleados o administradores accedan de forma remota a los recursos corporativos desde ubicaciones no confiables. Cuando abrimos puertas hacia redes públicas como Internet, la defensa individual de cada servidor (_endpoint_) resulta insuficiente: necesitamos diseñar una frontera lógica y física que gobierne quién entra, quién sale y hasta dónde puede llegar. Este es el dominio de la **seguridad perimetral** y el **acceso remoto seguro**.

## 1. Fundamentos de Seguridad Perimetral y Zonificación de Red

La **seguridad perimetral** es el conjunto de mecanismos, arquitecturas de red y políticas de control diseñados para proteger la frontera entre una red privada corporativa y las redes externas no confiables (principalmente Internet).

Durante décadas, la seguridad perimetral se basó en el modelo clásico del **"castillo y el foso"** (_castle-and-moat_): se asumía que todo lo que estaba fuera del perímetro era hostil y debía bloquearse, mientras que todo lo que estaba dentro de la red local (LAN) era confiable por defecto. En la administración de sistemas actual (2.º ASIR), este modelo ha evolucionado hacia arquitecturas segmentadas y principios de **Confianza Cero (_Zero Trust_)**, donde el perímetro ya no es un único muro exterior, sino una sucesión de fronteras internas que contienen posibles brechas y limitan el movimiento lateral de un atacante.

### 1.1. Escenarios de exposición a redes públicas y necesidad de defensa perimetral

Cuando una infraestructura empresarial se conecta a redes públicas o expone servicios al exterior, abandona el aislamiento físico y pasa a estar sometida a un sondeo automatizado constante (bots, escaneos de puertos masivos, intentos de fuerza bruta y explotación de vulnerabilidades de día cero). En el ámbito profesional encontramos cuatro escenarios típicos que exigen un diseño perimetral riguroso:

- **Publicación de servicios corporativos hacia Internet:** Servidores web públicos, portales de comercio electrónico, pasarelas de correo electrónico (SMTP) o servidores DNS autoritativos que deben ser alcanzables por cualquier usuario externo las 24 horas del día, pero que a su vez necesitan consultar bases de datos o directorios internos (como Active Directory o LDAP) para funcionar.

- **Teletrabajo y acceso remoto de usuarios (_Roadwarriors_):** Empleados que se conectan desde redes domésticas, hoteles o cafeterías (redes públicas no controladas) y requieren acceder a carpetas compartidas, aplicaciones ERP o escritorios de la red corporativa con las mismas garantías de confidencialidad e integridad que si estuvieran en la oficina.

- **Administración remota de infraestructura:** Técnicos de sistemas que necesitan gestionar servidores, hipervisores o equipos de red fuera del horario laboral o desde centros remotos sin exponer interfaces administrativas (SSH, RDP, paneles web de gestión) directamente a Internet.

- **Interconexión de sedes y entornos híbridos (_Site-to-Site_ y _Cloud_):** Empresas con múltiples sucursales o que despliegan parte de su infraestructura en proveedores de nube pública (AWS, Azure, Google Cloud) y necesitan unir ambas redes como si formaran una única red local privada.

#### Riesgos de una conexión directa sin defensa perimetral (Red Plana)

Si conectamos directamente los servidores internos a un router con salida a Internet mediante simples redirecciones de puertos (_Port Forwarding / DNAT_) sin segmentación previa:

1. **Compromiso total ante una única vulnerabilidad:** Si un atacante compromete el servidor web público mediante una inyección SQL o una ejecución remota de código (RCE), obtiene inmediatamente visibilidad directa a nivel de capa 2 y capa 3 sobre los servidores de nóminas, bases de datos y controladores de dominio de la empresa.

2. **Exfiltración de datos sin control:** Un malware instalado en un equipo interno puede establecer conexiones salientes hacia su servidor de Mando y Control (C2) o extraer gigabytes de información confidencial sin que ningún elemento intermedio inspeccione el flujo de salida.

3. **Ausencia de visibilidad y trazabilidad:** Sin puntos de estrangulamiento (_chokepoints_) por donde pase obligatoriamente el tráfico entre zonas, resulta imposible aplicar políticas granulares o auditar qué equipo externo accedió a qué recurso interno.
    
![[Gemini_Generated_Image_sutw43sutw43sutw.jpeg]]

### 1.2. Clasificación de zonas de riesgo según criterios de seguridad perimetral

Para eliminar el peligro de las redes planas, la seguridad perimetral divide la infraestructura en **zonas de confianza o zonas de riesgo**, asignando a cada una un nivel de seguridad específico y definiendo reglas estrictas sobre qué flujos de tráfico están permitidos entre ellas.


```
+-------------------+          +-------------------+          +-------------------+
|   RED EXTERNA     |          |        DMZ        |          |    RED INTERNA    |
| (WAN / Untrusted) |  =====>  | (Semi-confiable)  |  ----->  |  (LAN / Trusted)  |
|   Nivel Riesgo:   | Permitido|   Nivel Riesgo:   | Restring.|   Nivel Riesgo:   |
|     MÁXIMO        | (80/443) |     MEDIO/ALTO    | (Solo BD)|      MÍNIMO       |
+-------------------+          +-------------------+          +-------------------+
         ^                              |                              |
         |                              X (Bloqueado por defecto)      |
         +------------------------------+------------------------------+
```

#### 1. Zona Externa (WAN / _Untrusted Zone_)

- **Definición:** Comprende todas las redes que escapan al control administrativo de la organización (Internet, redes de proveedores externos, redes Wi-Fi públicas o enlaces de invitados).
    
- **Nivel de confianza:** **Nulo (0 %)**. Se asume por defecto que todo tráfico procedente de esta zona es potencialmente hostil.
    
- **Política de acceso:** Todo tráfico entrante desde la zona externa hacia la red interna debe estar **bloqueado por defecto**. Únicamente se permite el acceso controlado hacia los servicios específicos alojados en la DMZ o hacia los puntos de terminación de túneles VPN autenticados.

#### 2. Zona Desmilitarizada (DMZ / _Perimeter Network_)

- **Definición:** Es una subred física o lógica aislada que actúa como "tierra de nadie" o zona colchón (_buffer zone_) entre la red externa y la red interna. Su propósito es alojar todos aquellos servidores y servicios que deben ser accesibles desde Internet (servidores web, servidores de correo, DNS externos, proxies inversos o pasarelas VPN).
    
- **Nivel de confianza:** **Medio / Bajo (Semi-confiable)**. Dado que los equipos de la DMZ reciben conexiones directas desde Internet, se asume que tienen una probabilidad elevada de ser atacados y eventualmente comprometidos.
    
- **Reglas de oro de una DMZ:**
    
    - **De Externo a DMZ:** **Permitido**, pero estrictamente filtrado a nivel de puerto y protocolo (por ejemplo, solo TCP 80 y 443 hacia el servidor web).
        
    - **De Interno a DMZ:** **Permitido** de forma controlada para tareas de administración (SSH/RDP desde estaciones de gestión) y publicación de contenidos.
        
    - **De DMZ a Interno:** **Bloqueado por defecto**. Este es el principio más crítico: si un atacante toma el control de un servidor en la DMZ, **no puede iniciar conexiones hacia la red interna**. Si el servidor web de la DMZ necesita consultar una base de datos situada en la red interna, se abrirá única y exclusivamente el puerto concreto (ej. TCP 3306 o 5432) desde la IP exacta del servidor web hacia la IP exacta del servidor de base de datos, preferiblemente a través de un proxy o API intermedia.
        
    - **De DMZ a Externo:** **Bloqueado o altamente restringido**. Un servidor web de la DMZ no debería tener acceso libre para navegar por Internet; restringir el tráfico saliente impide que un malware descargue cargas útiles adicionales o establezca túneles reversos hacia servidores C2.

#### 3. Zona Interna (LAN / _Trusted Zone_)

- **Definición:** Es el núcleo de la infraestructura corporativa donde residen los activos de mayor valor: servidores de bases de datos, controladores de dominio, servidores de ficheros, sistemas ERP/CRM, copias de seguridad y los puestos de trabajo de los empleados.
    
- **Nivel de confianza:** **Alto** (aunque sujeto a segmentación interna mediante VLANs por departamentos: Administración, Desarrollo, Dirección, IoT/Impresoras).
    
- **Política de acceso:** Invisible y completamente inaccesible de forma directa desde la red externa. Los usuarios internos acceden a Internet a través de sistemas intermediarios controlados (NAT con estado, cortafuegos con inspección de salida o servidores proxy).

#### 4. Zona de Gestión (_Management Network_ / OOB - _Out-of-Band_)

- **Definición:** En infraestructuras críticas y centros de datos (CPD), se define una cuarta zona completamente aislada dedicada exclusivamente al tráfico administrativo: acceso SSH a servidores Linux, RDP a servidores Windows, interfaces de gestión de switches/firewalls y controladores hardware de servidor (iLO, iDRAC, IPMI).
    
- **Política de acceso:** Solo es accesible desde las estaciones de trabajo de los administradores de sistemas o tras autenticarse mediante un servidor bastión (_Jump Host_) con autenticación multifactor.

#### Matriz resumen de control de tráfico entre zonas

|**Origen \ Destino**|**Zona Externa (WAN)**|**DMZ**|**Zona Interna (LAN)**|**Zona Gestión**|
|---|---|---|---|---|
|**Zona Externa (WAN)**|—|**Permitido** _(Solo servicios públicos)_|**PROHIBIDO** _(Salvo túnel VPN)_|**PROHIBIDO**|
|**DMZ**|**Restringido** _(Solo actualizaciones/DNS)_|**Restringido** _(Aislamiento entre hosts)_|**PROHIBIDO** _(Salvo puerto puntual a BD)_|**PROHIBIDO**|
|**Zona Interna (LAN)**|**Permitido** _(Vía Proxy / NAT filtrado)_|**Permitido** _(Consulta/gestión controlada)_|**Segmentado** _(Por VLANs)_|**PROHIBIDO** _(Salvo equipo Admin)_|
|**Zona Gestión**|**PROHIBIDO**|**Permitido** _(Administración SSH/RDP)_|**Permitido** _(Administración SSH/RDP)_|—|

### 1.3. Topologías de subred protegida: Arquitectura débil frente a arquitectura fuerte

Una vez definidas las zonas de riesgo, el administrador de sistemas debe decidir cómo interconectar físicamente los equipos de red (routers y cortafuegos) para materializar esa separación. En el diseño de seguridad perimetral existen cuatro arquitecturas clásicas, evolucionando desde modelos básicos hasta la topología estándar empresarial (_Screened Subnet_).

#### A. Host de Doble Conexión (_Dual-Homed Host_) — Sin DMZ real

Consiste en un único servidor o cortafuegos equipado con dos tarjetas de red (una conectada a la red externa y otra a la red interna) en el que se desactiva el reenvío automático de paquetes (_IP Forwarding_) a nivel de sistema operativo. Todo el tráfico debe pasar obligatoriamente por servicios proxy o aplicaciones instaladas en dicho equipo.

- **Limitación:** Si el sistema operativo del _Dual-Homed Host_ sufre una vulnerabilidad o se habilita por error el enrutamiento, la red interna queda expuesta sin barreras adicionales.

#### B. Cortafuegos de Subred Protegida con Un Solo Cortafuegos (Arquitectura Débil)

En esta topología se emplea un **único cortafuegos** con al menos **tres interfaces de red físicas o lógicas** (conocido habitualmente como cortafuegos de "tres patas" o _Three-Legged Firewall_):

- **Interfaz 1 (WAN / Red):** Conectada al router de borde hacia Internet.
    
- **Interfaz 2 (DMZ / Orange):** Conectada al switch donde se alojan los servidores públicos.
    
- **Interfaz 3 (LAN / Green):** Conectada a la red interna corporativa.

```
[ INTERNET ]
      |
[ Router Borde ]
      |
      | (Interfaz WAN)
+-----+-------------------+
|  CORTAFUEGOS ÚNICO      +--- (Interfaz DMZ) ---> [ Servidores Públicos
												(Web/Mail) ]
+-----+-------------------+
      | (Interfaz LAN)
      v
[ RED INTERNA (LAN) ]
```

- **Ventajas:** Es económica, sencilla de administrar y centraliza todas las políticas de filtrado y logs en una sola consola. Es muy habitual en pymes o mediante soluciones unificadas (pfSense, OPNsense, FortiGate).
    
- **¿Por qué se denomina "Arquitectura Débil"?**
    
    1. **Punto Único de Fallo de Seguridad:** Si un atacante descubre una vulnerabilidad en el firmware o sistema operativo del cortafuegos y logra comprometerlo, obtiene el control simultáneo de las tres interfaces y acceso directo a la LAN.
        
    2. **Riesgo por error humano:** Una regla mal configurada en el cortafuegos (por ejemplo, una regla `ANY -> ANY` temporal para pruebas que olvida borrarse) deja expuesta al instante toda la red interna hacia Internet.
        
    3. **Tráfico no aislado físicamente:** Tanto el tráfico externo como el interno comparten la misma CPU y memoria del dispositivo; un ataque de Denegación de Servicio (DDoS) contra la DMZ puede saturar el cortafuegos y dejar sin salida a Internet ni comunicaciones internas a toda la empresa.

#### C. Subred Protegida con Doble Cortafuegos (_Screened Subnet_ — Arquitectura Fuerte)

Es el estándar de referencia en entornos corporativos medianos y grandes que requieren alta seguridad. En este diseño, la DMZ queda **físicamente encapsulada entre dos cortafuegos independientes** (preferiblemente de distinto fabricante para evitar que una misma vulnerabilidad afecte a ambos):

```
[ INTERNET ]
      |
      v
+------------------------+
| CORTAFUEGOS EXTERIOR   |  <-- Filtra tráfico desde Internet (bloquea ataques masivos,
| (Exterior / Perimetral)|      escaneos y tráfico no dirigido a servicios de la DMZ).
+------------------------+
      |
      +----------------------> [ DMZ: Web, Correo, Pasarela VPN, Bastión SSH ]
      |
+------------------------+
| CORTAFUEGOS INTERIOR   |  <-- Protege la LAN. Bloquea cualquier conexión iniciada
| (Interior / Core)      |      desde la DMZ hacia la red interna.
+------------------------+
      |
      v
[ RED INTERNA (LAN) ]
```

1. **Cortafuegos Exterior (_Exterior Firewall_):** Su misión es proteger tanto a la DMZ como al cortafuegos interior. Filtra el ruido de Internet, bloquea paquetes malformados, realiza tareas de _anti-spoofing_ y únicamente deja pasar hacia la DMZ el tráfico destinado a los puertos de los servicios públicos (80, 443, 25, 1194/UDP, etc.).
    
2. **Zona Desmilitarizada (DMZ):** Situada en el segmento de red intermedio entre ambos cortafuegos.
    
3. **Cortafuegos Interior (_Interior Firewall_):** Su única misión es proteger la red interna frente a la DMZ y frente a cualquier fallo del cortafuegos exterior. Sus reglas son mucho más restrictivas: bloquea por defecto todo tráfico procedente de la DMZ (incluso aunque un servidor de la DMZ haya sido hackeado) y solo permite peticiones muy específicas (como consultas SQL hacia un servidor de base de datos concreto) y tráfico saliente de los usuarios internos hacia el exterior.
    

- **Ventajas de la Arquitectura Fuerte:**
    
    - **Defensa en Profundidad real:** Para alcanzar la red interna, un atacante debe vulnerar dos cortafuegos distintos y un servidor intermedio en la DMZ.
        
    - **Contención de brechas:** Si un servidor de la DMZ es comprometido, el atacante sigue atrapado frente al cortafuegos interior, que no conoce ni permite rutas hacia la red privada.
        
    - **Rendimiento y visibilidad:** El cortafuegos exterior absorbe la carga bruta de Internet, mientras que el interior opera con baja latencia y registra cualquier intento anómalo de conexión desde la DMZ hacia la LAN.
        
![[Gemini_Generated_Image_gqchsvgqchsvgqch.jpeg]]

### 1.4. Comprobación práctica en Linux: Simulación de aislamiento y enrutamiento entre zonas

En sistemas Linux que actúan como routers o cortafuegos perimetrales entre zonas, el aislamiento entre interfaces se controla a nivel de kernel mediante el parámetro `net.ipv4.ip_forward` y las cadenas de filtrado de Netfilter (`FORWARD` en `iptables` o `nftables`).

Cuando configuramos una máquina Linux como pasarela perimetral entre una red externa (`eth0`), una DMZ (`eth1`) y una red interna (`eth2`), el flujo de paquetes que atraviesa el equipo **no pasa por las cadenas `INPUT` ni `OUTPUT`**, sino exclusivamente por la cadena **`FORWARD`**:


```Bash
# 1. Comprobar si el reenvío de paquetes entre interfaces está activo en el kernel
sysctl net.ipv4.ip_forward
# (0 = equipo aislado / sin enrutamiento; 1 = actúa como router/pasarela entre zonas)

# 2. Ejemplo de política perimetral en cadena FORWARD (Arquitectura de 3 zonas):
# Política por defecto: bloquear todo el tráfico que intente cruzar entre zonas
sudo iptables -P FORWARD DROP

# Permitir tráfico de respuesta de conexiones ya establecidas (Cortafuegos con estado)
sudo iptables -A FORWARD -m conntrack --ctstate ESTABLISHED,RELATED -j ACCEPT

# Permitir acceso desde Internet (eth0) hacia el servidor Web de la DMZ (172.16.10.10:443)
sudo iptables -A FORWARD -i eth0 -o eth1 -d 172.16.10.10 -p tcp --dport 443 -j ACCEPT

# Permitir salida a Internet desde la LAN (eth2) hacia WAN (eth0)
sudo iptables -A FORWARD -i eth2 -o eth0 -j ACCEPT

# PROHIBIR explícitamente cualquier inicio de conexión desde la DMZ (eth1) hacia la LAN (eth2)
sudo iptables -A FORWARD -i eth1 -o eth2 -j DROP
```

Este ejemplo de `FORWARD` nos sirve para relacionar el uso de `iptables` visto en la Unidad anterior con la configuración de pasarelas VPN que implementaremos en la futura práctica 3.1.

### Actividad de Aula / Reflexión Guiada (Para trabajar en las sesiones 1-2)

>[!NOTE] Ejercicio 1. 
>Una empresa de comercio electrónico dispone de:
>1. Un servidor web público (`TiendaOnline`) donde los clientes compran productos.
>    
>2. Un servidor de base de datos (`BD_Clientes`) donde se almacenan los datos personales, pedidos y tarjetas tokenizadas.
>    
>3. Un servidor de correo corporativo (`MailServer`).
>    
>4. 40 puestos de trabajo de empleados en oficinas.
>    
>5. Un servidor de copias de seguridad (`BackupNAS`).
>    
>**Cuestiones para resolver:**
>
>
>1. Asigna cada uno de los 5 sistemas a su zona de riesgo correspondiente (DMZ o Red Interna) justificando técnicamente la decisión.
   > 
>2. Si `TiendaOnline` necesita leer y escribir registros en `BD_Clientes` (puerto TCP 5432), ¿dónde ubicarías `BD_Clientes` y qué regla exacta debería existir en el cortafuegos interior?
>    
>3. Si un atacante logra subir una _web shell_ a `TiendaOnline` aprovechando un plugin desactualizado, ¿qué le impedirá lanzar un escaneo Nmap o un ataque de ransomware contra el `BackupNAS` y los 40 puestos de trabajo?

