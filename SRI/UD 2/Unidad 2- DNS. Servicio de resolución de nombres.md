## Introducción y Objetivos de Aprendizaje

El Sistema de Nombres de Dominio (DNS) es uno de los pilares fundamentales que sostienen la arquitectura de las redes modernas y de Internet. En esta unidad, abordaremos tanto los fundamentos teóricos del protocolo como su configuración práctica, persiguiendo los siguientes 

**objetivos de aprendizaje:**

- **Sistemas de nombres:** Conocer la diferencia entre un sistema de nombres plano y uno de estructura jerárquica.
- **Dominios de primer nivel:** Distinguir entre los distintos tipos de dominios de primer nivel (TLD) que estructuran Internet.
- **Delegación de dominios:** Describir y resolver correctamente los retos que plantea la delegación de la autoridad de los dominios.
- **Servidores primarios y secundarios:** Comprender las diferencias técnicas y operativas entre un servidor de nombres primario y uno secundario.
- **Bases de datos DNS:** Reconocer los distintos elementos y registros que conforman las bases de datos en DNS y ser capaz de implementarlos.
- **Herramientas de diagnóstico:** Identificar y manejar las herramientas de red disponibles para el diagnóstico y resolución de problemas DNS.
- **Despliegue de servicios:** Ser capaz de instalar y configurar correctamente un servidor de nombres funcional.

## 2.1. El servicio DNS

El protocolo DNS (_Domain Name System_) surge históricamente para solventar un problema práctico: la dificultad que supone para los seres humanos identificar, memorizar e interactuar con los equipos de una red utilizando exclusivamente el direccionamiento de la capa de red. A nivel de máquina, las comunicaciones exigen el uso de direcciones IP, pero para un usuario o administrador resulta inviable recordar direcciones IPv4 como `83.144.201.39` o extensas direcciones IPv6 como `2001:0db8:85a3:0000:00000:8a2e:0370:7334`.

Para establecer un puente entre la memoria humana y el enrutamiento de red, el servicio DNS realiza principalmente tareas de **resolución directa**; es decir, se encarga de traducir un nombre inteligible (como una dirección web) a su número IP correspondiente. Sin embargo, en determinadas circunstancias operativas, el proceso inverso también es estrictamente necesario. A este proceso se le conoce como **DNS inverso** (o resolución inversa) y se encarga de averiguar qué nombre de dominio está asociado a una dirección IP concreta.

### El DNS como recurso crítico de red

Si nuestro servidor de nombres experimenta una caída o deja de estar disponible, el equipo cliente será incapaz de acceder a los recursos de la red a menos que el usuario conozca de antemano la dirección IP exacta del destino. A efectos prácticos, para la inmensa mayoría de los usuarios, la caída del DNS equivale a quedarse sin acceso a Internet.

Para mitigar este riesgo y dotar a la red de tolerancia a fallos, es imprescindible diseñar la infraestructura con redundancia, disponiendo siempre de más de un servidor de nombres.

Históricamente, la configuración de redundancia establecía una jerarquía estricta de **servidor primario y secundario** (pudiendo existir terciarios o superiores). En este modelo clásico, el sistema operativo del cliente envía todas las consultas inicialmente al servidor primario. Si este servidor falla, el cliente debe agotar un tiempo de espera predeterminado antes de repetir la misma consulta al servidor secundario. El gran inconveniente de este enfoque secuencial es que, ante la caída del primario, la suma de los tiempos de espera de cada consulta degrada severamente el rendimiento general de la navegación, haciéndolo casi inviable si el cliente debe escalar hasta un servidor terciario.

En la actualidad, para solucionar esta latencia, gran parte de los sistemas operativos modernos gestionan toda la lista de servidores DNS configurados de forma simultánea, distribuyendo la carga de consultas entre todos ellos y consiguiendo así una notable disminución en los tiempos de respuesta.

### Los tres retos del crecimiento de las redes

A medida que las redes empresariales y la propia Internet crecen, el volumen de equipos y de consultas DNS se multiplica exponencialmente. Este crecimiento conduce a tres grandes problemas que cualquier software que implemente el protocolo DNS moderno debe ser capaz de resolver:

1. **Organización:** A medida que las bases de datos de nombres crecen en tamaño, el proceso de buscar y encontrar una entrada específica se vuelve más lento.
    
2. **Escalabilidad:** Si todos los _hosts_ de Internet intentaran acceder a un único grupo centralizado de servidores de nombres, la carga de procesamiento y tráfico sería inasumible.
    
3. **Administración:** Mantener un registro centralizado de todos los nombres de una red gigante hace que la administración técnica sea un cuello de botella. Es fundamental diseñar un mecanismo que agilice estas tareas y evite conflictos graves.

Estos retos se afrontan en la actualidad mediante distintas arquitecturas, estrategias o modelos que veremos a lo largo de la unidad.

## 2.2. ¿Cómo funciona el Sistema de Nombres de Dominio de Internet?

El Sistema de Nombres de Dominio de Internet es una implementación específica del concepto de servidor de nombres, optimizada para soportar la magnitud de la red global. Esta implementación fue diseñada de origen para satisfacer tres requisitos fundamentales: la necesidad de establecer una jerarquía de nombres, permitir un balanceo de carga eficiente entre los servidores y dotar al sistema de la capacidad de delegar su administración.

Para lograrlo, el DNS se estructura lógicamente como un árbol invertido. En la cúspide se encuentra el **nodo raíz**, seguido inmediatamente por los dominios de primer nivel (TLD), los dominios de segundo nivel (SLD) y, por último, un número indefinido de nodos de niveles inferiores. Al escribir un dominio, todos estos niveles jerárquicos se separan visualmente mediante un punto.

### Tipos de Dominios de Primer Nivel (TLD)

Los dominios de primer nivel (Top Level Domains) se dividen principalmente en dos grandes categorías:

- **gTLD (Dominios Genéricos):** Son dominios de carácter general e internacional, como `.com`, `.net` u `.org`. Son administrados de forma autoritativa y centralizada por la ICANN (Corporación de Internet para la Asignación de Nombres y Números).

- **ccTLD (Dominios Geográficos):** Son dominios asociados administrativamente a países y siempre están formados por exactamente dos caracteres, como `.es` (España), `.fr` (Francia) o `.uk` (Reino Unido). La ICANN delega la administración de estos dominios a cada país, otorgándoles el derecho de definir sus propias reglas internas de segmentación. Por ejemplo, Estados Unidos segmenta su dominio geográfico territorialmente por estados (asignando `.ny.us` a Nueva York), mientras que España optó por no aplicar este tipo de segmentación geográfica en su dominio `.es`.

![[Pasted image 20261030145641.png]]

### Estructura de un Dominio y el concepto FQDN

El nombre de dominio que utilizan los usuarios habitualmente (por ejemplo, `asir.es`) se construye combinando un SLD y un TLD. Se lee y escribe de izquierda a derecha, colocando siempre el nivel más bajo de la jerarquía a la izquierda y el nivel más alto a la derecha.

Para comprender su anatomía de forma completa, analicemos el nombre `www.informatica.mi-empresa.es`:

- `mi-empresa.es`: Es el nombre del dominio principal de la organización.
- `informatica`: Es un subdominio, creado por la empresa para organizar lógicamente su red interna (por ejemplo, separando departamentos).
- `www`: Es el nombre del _host_ o equipo informático final donde reside un servicio concreto (en este caso, el servidor de la página web).

A la unión completa de todos estos elementos se le denomina **FQDN** (_Full Qualified Domain Name_ o Nombre de Dominio Totalmente Cualificado). A nivel puramente técnico, un FQDN debería terminar siempre en un punto (ej. `[www.mi-empresa.com](https://www.mi-empresa.com).`) para hacer referencia explícita al nodo raíz del que cuelga toda la estructura, aunque en el uso cotidiano por parte del usuario final este punto se omita.

A nivel de diseño, el estándar establece que un FQDN puede tener una longitud máxima de 255 caracteres, con una restricción estricta de 63 bytes por etiqueta (el texto contenido entre dos puntos). El protocolo tradicional solo permite el uso de letras de la A a la Z (sin distinguir mayúsculas de minúsculas), dígitos numéricos y el guion medio (`-`). 

>[!WARNING] Nota
>Como excepción particular, desde octubre de 2007, la autoridad del dominio `.es` permite la inclusión de caracteres especiales propios del idioma español, como la `ñ`, la `ç` y las vocales acentuadas.

>[!NOTE] Ejercicio 1
>**Identifica las distintas partes de los siguientes dominios:**
>	portal-empleado.rrhh.europa.corp-global.com.
>	db-cluster.backend.app-ventas.tecnologia-valencia.es.
>	vpn01.admin.sistemas.cloud-asir.net.
### El pilar del sistema: La Delegación

La autoridad es un concepto central en DNS: cada nodo de la jerarquía es asignado a una entidad o persona responsable de su gestión operativa. Esta autoridad tiene la potestad de **delegar** la organización y administración de los niveles inferiores de su nombre de dominio.

El propietario de un dominio puede delegar, aplicando las reglas internas que considere oportunas, cualquier cosa que decida escribir a la izquierda de su nombre de dominio. Este proceso de delegación permite que el administrador de un dominio superior (dominio padre) se vea liberado de la carga técnica y de mantenimiento de servidores que, de otra forma, le correspondería asumir. Es el mecanismo vital que permite que la administración de Internet escale de forma distribuida.

## 2.3. Zona DNS vs Dominio DNS

Derivado directamente del mecanismo de delegación, surge una de las confusiones conceptuales más comunes en la administración de redes: la diferencia entre dominio y zona. Es crucial delimitar ambos conceptos.

Como gran titular de dicha diferenciación, podríamos declarar que **cada vez que se delega un dominio, se está aislando una nueva parcela administrativa denominada zona**. La unidad o porción del árbol que ha sido delegada es lo que técnicamente se conoce como zona.

- **Dominio DNS:** Hace referencia a cualquier subárbol completo de la estructura DNS. Por ejemplo, el "dominio raíz" o el "dominio `.es`" engloban teóricamente a absolutamente todos los ordenadores y subdominios que existan físicamente bajo esa terminación.
    
- **Zona DNS:** Es la fracción administrativa exacta que ha sido delegada y que cuenta con sus propios servidores DNS. Cuando a una empresa se le delega la zona `mi-empresa.es`, sus administradores de red son los únicos responsables de configurar un conjunto de servidores DNS para gestionar exclusivamente esa parcela.

![[Pasted image 20261030150723.png]]

## 2.4. Bases de datos DNS: Estructura y tipos de registros

En el protocolo DNS, la base de datos de una zona no es más que un archivo de texto plano estructurado mediante los denominados **Registros de Recursos** (_Resource Records_ o RR). Independientemente de su función, todos los registros comparten una estructura lógica basada en los siguientes campos:

- **Nombre de dominio:** El objetivo al que aplica el registro (ej. `www`, `mail`, o `@` para la raíz).
- **Clase:** Indica la clase de red. En el 99% de los casos será `IN`, que significa Internet.
- **Tipo:** Define la función del registro.
- **Valor TTL (_Time To Live_):** El tiempo, medido en segundos, durante el cual la respuesta puede ser almacenada en la memoria caché de otros servidores antes de solicitarse de nuevo.
- **Datos:** La respuesta en sí misma (una IP, un nombre de dominio, prioridades, etc.).

A continuación, se describen en detalle los principales tipos de registros, su sintaxis y su implementación técnica en los archivos de zona.

### Registros SOA (Start of Authority)

Ningún archivo de zona DNS es válido si no comienza obligatoriamente con un registro SOA. Este registro marca el inicio de la autoridad sobre la zona y es fundamental para los clústeres de servidores, ya que regula cómo y cuándo se deben sincronizar los servidores esclavos (secundarios) con los datos del servidor maestro (primario) mediante las "transferencias de zona".

**Sintaxis y ejemplo de configuración:**

```Plaintext
$ORIGIN example.org.
$TTL 1750
@   IN  SOA master.example.org. admin.master.example.org. (
            2019040502 ; serial
            86400      ; refresh
            7200       ; retry
            3600000    ; expire
            1750       ; minimum
)
```

**Análisis de los parámetros clave:**

- `$ORIGIN` y `@`: El símbolo `@` funciona como un atajo que equivale al nombre del dominio base definido en el ORIGIN.
- `master.example.org.`: Es el FQDN del servidor DNS primario que posee los datos originales.
- `admin.master.example.org.`: Es la dirección de correo del administrador. Como el símbolo `@` está reservado por el sistema DNS para referirse al origen de la zona, la arroba del correo electrónico se sustituye obligatoriamente por un punto (equivaldría a `admin@master.example.org`).
- `serial`: Número de serie (habitualmente formado por la fecha y una revisión). Debe incrementarse manualmente en cada modificación del archivo para que los esclavos detecten el cambio y se sincronicen.
- `refresh`: Tiempo (en segundos) que espera un esclavo antes de preguntar al maestro si hay actualizaciones.
- `retry`: Tiempo de espera para volver a intentar la conexión si el maestro falló.
- `expire`: Tiempo tras el cual, si el esclavo no logra contactar con el maestro, dejará de responder consultas al considerar sus datos obsoletos.

### Registros NS (Name Server)

Se encargan de declarar públicamente qué máquinas actúan como servidores de nombres oficiales para la zona. Debe existir al menos un registro NS por cada servidor (primario y secundarios) que gestione el dominio.

**Sintaxis:**

```Plaintext
@   IN  NS  ns1.exampleserver.com.
```

 >[!WARNING] Nota
 >Aunque este registro indica el nombre del servidor, los clientes informáticos necesitan IPs para comunicarse. Por ello, si el servidor NS pertenece al mismo dominio de la zona, es estrictamente obligatorio crear debajo un "Registro A" que asocie ese nombre `ns1` a su dirección IP real.

### Registros A (Address) y AAAA

Es el registro más común y fundamental del sistema. Su función es enlazar directamente un nombre de dominio (un FQDN) con una dirección IP física. El tipo `A` se utiliza para direcciones IPv4, mientras que el tipo `AAAA` cumple la misma función para el protocolo IPv6.

**Sintaxis:**

```Plaintext
www.example.com. 12879 IN A 93.184.216.34
```

En este ejemplo, `12879` representa el TTL específico de este registro (ignorando el global), `IN` la clase de red, `A` el tipo, y `93.184.216.34` la IP de destino.

### Registros CNAME (Canonical Name)

El registro CNAME actúa como un alias. Permite enlazar un nombre de dominio secundario hacia otro nombre de dominio principal (el nombre "canónico") que sí posee un registro A asociado. Su gran ventaja administrativa es que, si un mismo servidor aloja múltiples servicios (web, ftp, nube) y su IP física cambia, el administrador solo necesita modificar un único registro A, y todos los registros CNAME que apunten a él se redirigirán automáticamente a la nueva dirección.

**Sintaxis:**

```Plaintext
www.example.net. IN CNAME www.example.com.
```

>[!WARNING] Nota
>Aunque NS y CNAME puedan parecer similares (ambos apuntan un nombre hacia otro nombre), su alcance es diametralmente opuesto. El **CNAME** es un apodo exclusivo para una máquina o servicio individual (como redireccionar de FTP a WWW). El registro **NS**, por el contrario, afecta a la totalidad de los equipos del dominio, delegando la autoridad de toda la zona a otro gestor.

### 2.5.5. Registros MX (Mail Exchange)

Especifican qué servidor (o servidores) están autorizados para recibir los correos electrónicos dirigidos al nombre de dominio. Sin este registro, los mensajes dirigidos a `usuario@example.com` no sabrían a qué equipo deben enrutarse.

Posee dos particularidades estrictas:

1. **Nunca apunta a una IP:** El destino de un MX debe ser siempre un FQDN (un nombre), el cual deberá contar con su propio registro A en la zona para poder resolverse.
2. **Sistema de prioridad inversa:** Incorpora un valor numérico para establecer jerarquías entre varios servidores de correo. El número más bajo indica la prioridad más alta (el servidor principal).

**Sintaxis:**

```Plaintext
example.com. IN MX 10 mail.example.com.
example.com. IN MX 20 backup.example.com.
```

En este escenario, el correo siempre intentará entregarse primero al servidor con prioridad `10`. Solo si este se encuentra inactivo, el sistema derivará el tráfico al servidor de respaldo con prioridad `20`.

### 2.5.6. Registros PTR (Pointer)

Se utilizan de manera exclusiva para la resolución inversa de DNS, es decir, traducir una dirección IP conocida para averiguar su nombre FQDN asociado.

Sus particularidades son altamente específicas:

- No se configuran en la zona normal del dominio, sino en una zona de resolución inversa global llamada `in-addr.arpa`.
- Siguiendo la lectura jerárquica del árbol DNS (de derecha a izquierda), los octetos de la dirección IP deben escribirse en orden inverso. Por ejemplo, la red `192.168.1.X` se define como la zona `1.168.192.in-addr.arpa`.
- Son una pieza vital como medida antispam: los servidores de correo receptores realizan consultas PTR para verificar que la IP pública desde la que llega un mensaje se corresponde realmente con el nombre de dominio que el remitente afirma ser.

**Sintaxis:**

```Plaintext
10.1.168.192.in-addr.arpa. IN PTR mail.example.com.
```

En este ejemplo, se afirma públicamente que la dirección IP `192.168.1.10` pertenece y responde al equipo `mail.example.com.`

## 3. Tipos de Servidores DNS: Resolución, Reenvío y Autoritativo.

### El Resolver (Resolutor DNS)

Es el mecanismo encargado de iniciar y gestionar todo el proceso de resolución DNS en nombre del cliente que formula la pregunta. Existen dos vertientes de este rol:

- **Stub Resolver (Resolutor de código auxiliar):** Reside directamente en el ordenador o dispositivo del usuario. Es una pequeña parte del sistema operativo que se encarga exclusivamente de enviar consultas recursivas al servidor DNS local o del proveedor de Internet, sin poseer capacidad para realizar búsquedas por sí mismo.
- **Recursive Resolver (Resolutor Recursivo):** Es el servidor que asume el esfuerzo real de búsqueda. Si carece de la respuesta en su memoria caché, realiza de forma transparente una serie de consultas iterativas: pregunta primero a los servidores raíz de Internet, desciende hasta los servidores de los dominios TLD, e interroga finalmente a los servidores autoritativos pertinentes, devolviendo al cliente una única respuesta completa y final. 

Cuando un servidor de este tipo no posee autoridad sobre ninguna zona y se emplea con el único fin de realizar consultas pertinentes y almacenar las respuestas obtenidas para acelerar peticiones futuras de los clientes de una red, se le denomina a nivel conceptual **Servidor Caché**.

### El Forwarder (Reenviador)

Un servidor _forwarder_ es un intermediario que no efectúa el proceso de resolución iterativa por sí mismo, sino que delega la carga reenviando las consultas que recibe directamente a otro servidor.

- **Casos de uso:** Se despliegan habitualmente en redes locales de empresas e institutos para centralizar la salida a Internet y ahorrar consumo de ancho de banda.
- **Flujo de trabajo:** El _forwarder_ recibe una petición, la remite intacta a un _resolver_ recursivo potente en la nube, y cuando este le devuelve el dato ya resuelto, el _forwarder_ se lo entrega al cliente local y lo conserva en su propia caché.

### El Autoritativo

Un servidor autoritativo (_authoritative server_) es aquel que ostenta la autoridad delegada sobre una zona DNS específica, lo que significa que contiene y gestiona los registros oficiales de dicha zona (como los registros A, NS, MX o SOA). Sus propiedades técnicas fundamentales son:

- **Ausencia de recursión:** Nunca buscan respuestas en otros servidores. Se limitan estrictamente a responder con los datos locales de su propia zona o a indicar oficialmente que el registro solicitado no existe.
- **Certificación de respuesta:** Etiquetan sus contestaciones utilizando la bandera AA (_Authoritative Answer_) para certificar ante el cliente que el dato es de origen oficial.
- **Almacenamiento local:** Albergan físicamente los archivos de la zona en su sistema de ficheros (por ejemplo, en el directorio `/etc/bind/db.ejemplo.local` bajo sistemas Linux).

Para garantizar la disponibilidad y tolerar fallos, cada zona debe contar forzosamente con un conjunto de servidores autoritativos coordinados entre sí:

1. **Servidor Primario (Maestro / _Master_):** Es el servidor principal de la zona. Es el único en el que el administrador de sistemas efectúa directamente las operaciones de alta, baja o modificación de los registros DNS. Posee, por tanto, los datos originales y editables.
2. **Servidor Secundario (Esclavo / _Slave_):** Son los servidores de respaldo de la zona. Contienen una copia exacta de los registros, obtenida del servidor primario mediante un proceso de sincronización llamado "transferencia de zona". A nivel de usuario, las respuestas que emite un secundario son técnica y operativamente igual de válidas que las del primario; su denominación se debe únicamente a que son réplicas de los datos originales, no a una supuesta falta de validez.

### Trazabilidad de una consulta DNS
Para comprender la interacción global de estos roles, analicemos el recorrido técnico que se desata cuando un equipo local ejecuta el comando `ping [www.google.com](https://www.google.com)`:

1. **Petición inicial:** El PC del usuario (_Stub Resolver_) remite una consulta recursiva al servidor DNS local de su red.
    
2. **Delegación:** El servidor DNS local, actuando como _Forwarder_, reenvía la consulta directamente al servidor público `8.8.8.8`.
    
3. **Proceso iterativo:** El servidor de Google (`8.8.8.8`), operando como _Recursive Resolver_, ejecuta las consultas ocultas y escalonadas: contacta con los servidores raíz de Internet, interroga a los responsables del dominio `.com`, y finalmente alcanza a los servidores autoritativos del dominio `google.com`.
    
4. **Respuesta oficial:** Los servidores _Autoritativos_ de Google responden afirmativamente: "La IP asociada a [www.google.com](https://www.google.com/search?q=https%3A%2F%2Fwww.google.com) es 142.250.200.4".
    
5. **Entrega y memorización:** El _resolver_ devuelve la IP obtenida a lo largo de toda la cadena de retorno hasta el PC del usuario, quedando el dato cacheado en los servidores intermedios para acelerar accesos posteriores.

>[!NOTE] Ejercicio 3
>Abre la terminal de comandos y consulta los servidores externos que gestionan los millones de correos electrónicos de Gmail. Le preguntaremos al servidor recursivo de Google (8.8.8.8) para evitar bloqueos de nuestra red local: `nslookup -type=mx gmail.com 8.8.8.8`
>
>¿Cuál de todos ellos es el servidor principal que intentará recibir el correo primero? ¿Cómo se ordenan los de respaldo si el principal se cae?

## 4. DNS Dinámico (DDNS)

El DNS Dinámico (DDNS, por sus siglas en inglés) es una extensión operativa del sistema DNS tradicional diseñada para permitir la actualización en tiempo real de la dirección IP asociada a un nombre de dominio. Esta tecnología surge para dar respuesta a un problema muy común en entornos residenciales y de pequeñas empresas: las conexiones a Internet donde el Proveedor de Servicios (ISP) asigna direcciones IP públicas dinámicas que cambian periódicamente.

El DNS tradicional asume que las direcciones IP de los servidores son estáticas, pero el DDNS flexibiliza esta arquitectura apoyándose en las siguientes características técnicas:

- **Agente automatizado:** El sistema depende de un software cliente que funciona en segundo plano (frecuentemente integrado de fábrica en _routers_, servidores NAS o en los propios sistemas operativos). Este agente monitoriza de forma constante la IP pública de la red local y, si detecta cualquier variación, envía una notificación inmediata al servidor DNS autoritativo.
    
- **Actualizaciones en caliente (RFC 2136):** Utiliza un protocolo específico de actualización dinámica que permite modificar los registros de los archivos de zona "al vuelo". Esto elimina la necesidad de que un administrador edite manualmente los ficheros de texto o reinicie el servicio BIND9 para aplicar los cambios.
    
- **Valores TTL sumamente bajos:** Para evitar que los usuarios sean dirigidos a una dirección IP obsoleta tras un cambio repentino, los registros generados mediante DDNS se configuran con un _Time To Live_ muy corto (por ejemplo, 60 o 120 segundos). Esto fuerza a todos los servidores intermedios y cachés de Internet a descartar la información rápidamente y volver a preguntar.
    
- **Autenticación obligatoria:** Dado que permitir modificaciones automáticas en una zona es un riesgo grave de seguridad, las peticiones de actualización dinámica exigen autenticación criptográfica. Solo los clientes que posean la clave secreta o _token_ autorizado podrán alterar a dónde apunta el dominio.

### Ejemplo de implementación práctica (Caso de uso VPN)

Para ilustrar la utilidad operativa del DNS Dinámico, analicemos el despliegue de un servidor VPN en una oficina que carece de una conexión a Internet con IP estática:

1. **Registro de la zona:** El administrador da de alta un subdominio específico (por ejemplo, `oficina.miempresa.com`) en un servidor DNS público que soporte actualizaciones dinámicas.
    
2. **Configuración del cliente:** En el _router_ de la oficina (o en el propio servidor VPN), se configura el cliente DDNS introduciendo el nombre del subdominio y el _token_ de seguridad criptográfica proporcionado por el servidor.
    
3. **El salto de IP:** Transcurridos unos días, el ISP renueva la concesión de red y la IP pública de la oficina cambia de forma imprevisible a, por ejemplo, `95.10.12.8`.
    
4. **Notificación automática:** En cuestión de segundos, el agente cliente detecta el cambio en la interfaz de red y envía un mensaje seguro al servidor DNS autoritativo: _"Actualiza el registro A de oficina.miempresa.com a la nueva IP 95.10.12.8"_.
    
5. **Resolución ininterrumpida:** El servidor DNS modifica el registro A en sus bases de datos en milisegundos. Cuando un empleado desde su casa intente conectar su cliente VPN hacia `oficina.miempresa.com`, la consulta DNS le entregará la nueva IP correcta, logrando que el salto de red haya sido completamente transparente para el usuario final.

**Implementación en BIND9**
Para que el servidor primario acepte estas actualizaciones dinámicas en caliente, se debe configurar una clave criptográfica (TSIG) y habilitar la directiva `allow-update` en la declaración de la zona dentro de `named.conf.local`:

```PlainText
zone "oficina.miempresa.com" {
    type master;
    file "/var/lib/bind/db.oficina.miempresa.com";
    allow-update { key "ddns-key"; };
};
```

>[!NOTE] Ejercicio 4
>Busca dos proveedores **del mundo de los videojuegos** que ofrezcan servicios de DNS Dinámico gratuitos para usuarios domésticos. ¿Qué limitaciones suelen tener las cuentas gratuitas en estos servicios respecto a la caducidad del dominio?

## 5. Seguridad en el servicio DNS
El protocolo DNS original fue diseñado en los albores de Internet sin mecanismos de seguridad integrados; transmitía las consultas en texto plano y asumía la buena fe y fiabilidad de todos los emisores. En la actualidad, para garantizar la disponibilidad e integridad de los servicios exigida en la administración de redes, es fundamental mitigar los riesgos inherentes al protocolo mediante configuraciones estrictas en el servidor. 

### 5.1. Vulnerabilidades principales
La falta de control sobre quién pregunta y qué se responde expone a los servidores de nombres a dos grandes amenazas: 
* **Envenenamiento de caché (Cache Poisoning / Spoofing):** Un atacante aprovecha el momento en que un servidor resolutor hace una consulta externa para inyectar una respuesta DNS falsificada antes de que llegue la respuesta legítima. Esto provoca que el servidor guarde en su memoria caché una dirección IP fraudulenta, redirigiendo todo el tráfico legítimo de los usuarios de esa red hacia un sitio web malicioso (phishing). 
* **Ataques de amplificación (DDoS):** Consiste en la explotación de servidores DNS "abiertos" (aquellos mal configurados que responden a consultas recursivas de cualquier origen en Internet). El atacante falsifica su IP de origen haciéndose pasar por la víctima y lanza miles de peticiones al servidor DNS. El servidor, al responder, envía paquetes de datos mucho más pesados que la petición original hacia la víctima, multiplicando el volumen de tráfico hasta colapsar su red. 
### 5.2. Mecanismos de protección en BIND9
Para proteger un servidor DNS, el administrador de sistemas debe implementar el principio de mínimo privilegio mediante directivas específicas en los archivos de configuración (`named.conf.options` y `named.conf.local`). 

**A. Ocultación de la versión del software** 
Por defecto, BIND9 responde si se le consulta qué versión exacta está ejecutando. Los atacantes utilizan esta información para buscar vulnerabilidades específicas (exploits) documentadas para esa versión. Es una buena práctica ocultarla. 
```Plaintext
// Archivo: /etc/bind/named.conf.options 
options {
	version "No disponible"; 
};
```

**B. Listas de Control de Acceso (ACLs) y Recursión**
Un servidor autoritativo expuesto a Internet solo debe responder consultas sobre sus propias zonas. La recursión (la capacidad de buscar respuestas en otros servidores de Internet) debe estar deshabilitada para clientes externos y permitida únicamente para la red interna de confianza. Esto previene los ataques de amplificación.

```PlainText
// Archivo: /etc/bind/named.conf.options
acl "red_interna" {
    192.168.50.0/24;
    localhost;
};

options {
    allow-query { any; }; // El mundo entero puede consultar nuestras zonas
    allow-recursion { "red_interna"; }; // Solo nuestra red puede usarlo para navegar
};
```

**C. Restricción de transferencias de zona (AXFR)**
Una transferencia de zona (AXFR) es el mecanismo legítimo mediante el cual un servidor secundario copia la base de datos del primario. Sin embargo, si no se restringe, cualquier atacante podría solicitar una transferencia y descargar el mapa completo de la red interna de la empresa (servidores, equipos, subdominios). Esta operación debe bloquearse por defecto y permitirse exclusivamente hacia la dirección IP del servidor secundario autorizado.

```PlainText
// Archivo: /etc/bind/named.conf.local
zone "miempresa.com" {
    type master;
    file "/etc/bind/db.miempresa.com";
    allow-transfer { 192.168.50.100; }; // IP estricta del servidor secundario (Esclavo)
};
```

**D. DNSSEC (Domain Name System Security Extensions)**
A un nivel superior, DNSSEC es una extensión del protocolo que añade firmas criptográficas asimétricas a los registros DNS. No cifra las consultas, pero permite a los clientes verificar matemáticamente que la respuesta proviene del administrador legítimo y que el registro no ha sido alterado en tránsito, erradicando por completo el riesgo de _spoofing_ o envenenamiento.

Para garantizar la correcta operativa de una infraestructura de red, no basta con configurar los servicios; el administrador debe ser capaz de auditar su comportamiento en tiempo real. La auditoría y el diagnóstico del tráfico DNS permiten verificar si las resoluciones se están ejecutando correctamente, identificar cuellos de botella y detectar anomalías de seguridad, como el tráfico originado por _malware_ o configuraciones erróneas que exponen la red.

>[!NOTE] Ejercicio 5
>Partiendo de la siguiente configuración de un servidor DNS que estás auditando:
>
>zone "granempresa.com" {
 > 	  type master;
  > 	file "/etc/bind/db.granempresa.com";
 > 	  allow-transfer { any; };
>};
>
>¿Qué error de seguridad ha cometido el administrador en este bloque de código?
>
Si fueras un atacante utilizando herramientas como `dig` o `host`, ¿qué tipo de solicitud intentarías realizar contra este servidor?
>
¿Qué información crítica podrías llegar a descargar en texto plano sin necesidad de romper ninguna contraseña?


>[!NOTE] Ejercicio 6
>Visualiza el siguiente video y debate acerca de las cuestiones que se plantean:
>https://www.youtube.com/watch?v=QHKBFxhnz4s
>
>1. Se supone que Internet es una red descentralizada para resistir ataques nucleares. Si es así, ¿por qué la caída de un solo proveedor (Dyn) logró desconectar a la mitad de los gigantes tecnológicos del mundo? ¿Debería obligarse a estas empresas a tener servidores DNS primarios y secundarios en distintos proveedores?- 
>
>2. La cámara de vigilancia barata del salón de un usuario cualquiera fue el arma para tumbar a Twitter. ¿Debería haber una legislación que prohíba vender hardware de red con credenciales por defecto (admin/admin)?

https://www.youtube.com/watch?v=NmcmLaCvxGw
## 6. Alta Disponibilidad: Servidores Secundarios y Transferencia de Zonas

Dado que el servicio DNS actúa como un recurso crítico para la infraestructura, la indisponibilidad del servidor de nombres implica que los equipos cliente perderán, a efectos prácticos, el acceso a los recursos de la red y a Internet. Para evitar este punto único de fallo, la arquitectura DNS exige el despliegue de alta disponibilidad mediante la implementación de, al menos, un servidor secundario de respaldo que asuma las peticiones si el principal cae.

### 6.1. Diferencias operativas entre Maestro y Esclavo

A nivel de protocolo, tanto el servidor primario como el secundario son **servidores autoritativos**; esto significa que las respuestas que emiten ambos sobre su zona son oficiales y técnicamente igual de válidas para los clientes. La diferencia real radica en cómo obtienen y administran los datos:

- **Servidor Primario (Maestro):** Es el equipo que contiene los archivos de zona originales y editables. Es el único punto de la red donde el administrador del sistema efectúa de forma manual las operaciones de alta, baja o modificación de los registros (A, CNAME, MX, etc.).
    
- **Servidor Secundario (Esclavo):** No posee archivos originales, sino que alberga una copia exacta de la zona en modo de solo lectura. Obtiene esta información conectándose periódicamente al maestro mediante un proceso denominado **transferencia de zona**.

**El rol vital del registro SOA en la sincronización:** Para que esta arquitectura funcione, el servidor esclavo necesita saber cuándo debe actualizar su copia. Esta mecánica está gobernada enteramente por el **registro SOA** de la zona:

1. Cuando el administrador hace un cambio en el maestro, debe incrementar obligatoriamente el **número de serie (`serial`)** en el registro SOA.
2. El esclavo, según el tiempo marcado en el parámetro `refresh` del SOA, interroga al maestro. Si detecta que el `serial` del maestro es mayor que el suyo, solicita inmediatamente una transferencia de zona para descargar los nuevos cambios.

### 6.2. Configuración de transferencias de zona seguras (AXFR) limitadas por IP

El mecanismo estándar que utiliza el esclavo para solicitar una copia completa de la base de datos se denomina **AXFR** (Transferencia de Zona Completa).

A nivel de seguridad, una transferencia AXFR sin restricciones es extremadamente peligrosa. Si un atacante ejecuta una petición AXFR contra nuestro servidor, podrá descargar el archivo de zona íntegro, obteniendo un mapa completo de la red interna (nombres de máquinas, subdominios ocultos, IPs de servidores de correo y bases de datos). Por ello, las transferencias de zona deben estar estrictamente bloqueadas por defecto y permitirse únicamente hacia las direcciones IP de los servidores esclavos autorizados.

**Configuración en el Servidor Maestro (`named.conf.local`)** En el servidor primario, se declara la zona como `master` y se utiliza la directiva `allow-transfer` para blindar el proceso, especificando la IP exacta del esclavo.

```Plaintext
zone "miempresa.com" {
    type master;
    file "/etc/bind/db.miempresa.com";
    allow-transfer { 192.168.50.30; }; // IP estricta del servidor Esclavo
};
```

**Configuración en el Servidor Esclavo (`named.conf.local`)** En el servidor secundario, se declara la zona como `slave` y se especifica la IP del maestro mediante la directiva `masters`.

```Plaintext
zone "miempresa.com" {
    type slave;
    file "/var/cache/bind/db.miempresa.com"; 
    masters { 192.168.50.20; }; // IP del servidor Maestro
};
```

_(Nota: A diferencia del maestro, que guarda sus archivos en `/etc/bind/` (directorio de solo lectura), el esclavo debe guardar la zona descargada en un directorio donde el usuario del sistema `bind` tenga permisos de escritura, como suele ser `/var/cache/bind/` o `/var/lib/bind/`, dependiendo de la distribución)._
  

> [!NOTE] Ejercicio 6
> 
> Un alumno ha configurado correctamente las directivas `allow-transfer` y `masters` en ambos servidores BIND9, y ha reiniciado ambos servicios. Sin embargo, al crear un nuevo registro tipo A (`intranet IN A 192.168.50.100`) en el servidor Maestro, comprueba con el comando `dig` que el servidor Esclavo sigue sin conocer la existencia de ese subdominio tras 30 minutos.
> Sabiendo que no hay problemas de conectividad ni cortafuegos entre ellos:
> 
> 1. ¿Qué parámetro exacto del archivo de zona olvidó modificar el alumno antes de reiniciar el Maestro?
> 
> 2. ¿Qué herramienta de comando (`named-check...`) le habría alertado de este problema o al menos verificado la integridad antes del reinicio?

## 7. Optimización de red: Servidores Reenviadores (Forwarders)
En infraestructuras de red corporativas o centros educativos, no siempre es eficiente que el servidor DNS local asuma la carga de realizar todo el proceso iterativo de resolución (preguntar a los servidores raíz, luego a los TLD, etc.) cada vez que un usuario solicita acceder a una web externa. Para optimizar el tráfico, se recurre a la figura del servidor reenviador.

### 7.1. Concepto de reenvío de consultas externas
Un servidor reenviador (_forwarder_) es un servidor DNS intermedio que no resuelve las consultas por sí mismo, sino que delega la resolución recursiva reenviando la petición a otro servidor externo de mayor capacidad.

El flujo de trabajo es el siguiente:

1. El servidor DNS local recibe una consulta de un equipo de la red para un dominio externo (ej. `[www.github.com](https://www.github.com)`).
    
2. Al comprobar que no tiene autoridad sobre esa zona ni tiene la respuesta en caché, no inicia la búsqueda jerárquica. En su lugar, empaqueta la consulta y se la reenvía íntegramente a un servidor resolutor público potente (como `8.8.8.8` de Google o `1.1.1.1` de Cloudflare).
    
3. El resolutor público realiza todo el trabajo pesado, obtiene la IP final y se la devuelve al servidor local.
    
4. El servidor local entrega la respuesta al cliente y la almacena en su memoria caché para futuras peticiones.
    

**Beneficios arquitectónicos:** El uso de _forwarders_ es fundamental en redes de empresas porque centraliza toda la salida de tráfico DNS a Internet y ahorra un ancho de banda considerable, ya que delega el consumo de las peticiones iterativas en la infraestructura de terceros.

### 7.2. Configuración de forwarders en BIND9 para delegar la recursión
En sistemas basados en Linux con BIND9, el reenvío de consultas se configura a nivel global, ya que afecta al comportamiento general del servidor frente a zonas desconocidas. Las directivas se aplican dentro del bloque `options` en el archivo principal de opciones.

**Sintaxis y configuración (`named.conf.options`)** 
Para habilitar el reenvío, se utiliza la directiva `forwarders` seguida de una lista de direcciones IP separadas por punto y coma. Adicionalmente, a nivel de ASIR, es importante conocer la directiva `forward` para determinar el comportamiento en caso de fallo.

```Plaintext
// Archivo: /etc/bind/named.conf.options

options {
    directory "/var/cache/bind";

    // Habilitar la recursión para nuestra red interna (vital por seguridad)
    allow-recursion { 192.168.50.0/24; localhost; };

    // Definir los servidores a los que reenviar las consultas externas
    forwarders {
        8.8.8.8; // DNS Primario (Google)
        1.1.1.1; // DNS Secundario (Cloudflare)
    };

    // 'forward first;' (por defecto): Si los forwarders fallan, el servidor intentará resolverlo él mismo.
    // 'forward only;': El servidor SOLO usará los forwarders. Si estos fallan, devolverá un error al cliente.
    forward only; 
};
```

Con esta configuración, le estamos diciendo a nuestro BIND9 local: _"Si no sabes la respuesta oficial porque no es tu zona, delega la pregunta recursiva a estos servidores externos, y si ellos no te contestan, no intentes buscarla por tu cuenta"_.

  

> [!NOTE] Ejercicio 7
> 
> Un administrador ha configurado los `forwarders` en el archivo `/etc/bind/named.conf.options` de su servidor BIND9 local (cuya IP es `192.168.50.20`) para que envíe todas las peticiones externas al servidor `9.9.9.9` (Quad9). Tras reiniciar el servicio, quiere comprobar si realmente su servidor está delegando el trabajo.
> 
> **Responde a las siguientes cuestiones técnicas:**
> 
> 1. Si el administrador abre la terminal en su propio servidor, ¿qué comando exacto de `dig` debería escribir para preguntarle a **su propio servicio BIND9 local** por la dirección IP de `www.gva.es`?
>     
> 2. Si quisiéramos demostrar de forma visual que nuestro BIND9 se está comunicando con el `9.9.9.9` para obtener la respuesta, ¿qué filtro de captura utilizaríamos en `tcpdump` o Wireshark en la interfaz del servidor local? _(Pista: Piensa en el puerto de destino y la IP externa)_.

## 8. Servidores DNS Caché
En arquitecturas de red donde la velocidad de respuesta y la optimización de los recursos son prioritarias, la configuración de la caché DNS juega un papel fundamental. A diferencia de los servidores primarios o secundarios, un **servidor caché** es un tipo de servidor DNS que no es autoritativo de ninguna zona en especial. Su única misión es realizar las consultas pertinentes en nombre de los clientes y almacenar temporalmente los resultados obtenidos.

### 8.1. Almacenamiento y distribución local de respuestas externas para ahorro de ancho de banda
El principio de funcionamiento de un servidor caché se basa en la reutilización de la información. Cuando el primer equipo de la red corporativa solicita la dirección IP de una web externa (por ejemplo, `www.gva.es`), el servidor DNS local invierte tiempo y ancho de banda en resolver esa petición (ya sea de forma recursiva o a través de un _forwarder_). Una vez obtenida la respuesta, se la entrega al cliente y guarda una copia en su memoria caché (RAM).
  
Si un segundo equipo de la misma red solicita el mismo dominio instantes después, el servidor no sale a Internet a preguntar; lee la respuesta directamente de su memoria y la entrega en milisegundos. Esta mecánica persigue dos grandes objetivos:

1. **Ahorro de ancho de banda:** Se minimiza drásticamente el tráfico DNS hacia el exterior, motivo por el cual los Proveedores de Servicios de Internet (ISP) despliegan grandes servidores caché que solo atienden las peticiones de sus propios clientes.
2. **Reducción de latencia:** La navegación de los usuarios se percibe mucho más fluida al no tener que esperar las resoluciones iterativas.

**El papel del TTL (Time To Live) en la caché** El almacenamiento en caché no es permanente ni arbitrario. El tiempo exacto que un servidor caché local mantiene guardada una respuesta externa viene determinado estrictamente por el **valor TTL** (medido en segundos) que el administrador del dominio original configuró en su registro. Una vez transcurrido ese tiempo, el dato caduca, se purga de la memoria y el servidor caché se verá obligado a solicitar la información de nuevo la próxima vez que un cliente pregunte.

### 8.2. Gestión de la caché en BIND9
En BIND9, cualquier servidor que tenga habilitada la recursión (`allow-recursion`) actúa de forma nativa como servidor caché para las consultas externas. A nivel de administración de sistemas, es crucial saber cómo intervenir manualmente en esta memoria caché, especialmente durante procesos de _troubleshooting_ (resolución de problemas).

La herramienta administrativa por excelencia para interactuar con el demonio de BIND9 en tiempo real es **`rndc`** (Remote Name Daemon Control).

**Comandos útiles para el administrador:**

- **Vaciar la caché completa (Flush):** Si sospechamos que nuestro servidor está entregando direcciones IP obsoletas porque un dominio externo acaba de cambiar de servidor, podemos forzar el borrado inmediato de toda la caché temporal antes de que expire su TTL.

    ```PlainText
    sudo rndc flush
    ```
   
- **Volcar la caché a un archivo para su auditoría:** Si queremos comprobar exactamente qué dominios y qué IPs tiene memorizadas nuestro servidor en un momento dado, podemos ordenar que escriba el contenido de la memoria RAM en un archivo de texto plano (habitualmente en `/var/cache/bind/named_dump.db`).

    ```PlainText
    sudo rndc dumpdb -cache
    ```


> [!NOTE] Ejercicio 8
> Una academia online con cientos de alumnos ha decidido migrar su plataforma web a un servidor más potente. El administrador del dominio ha cambiado el **Registro A** en el servidor maestro (autoritativo) para que apunte a la IP del nuevo servidor, pero los alumnos informan de un problema: algunos estudiantes ya ven la web nueva, mientras que otros siguen viendo la web antigua e inactiva.
> 
> Sabiendo cómo funciona el sistema de caché distribuido por los ISPs en Internet:
> 
> 1. Explica técnicamente por qué está ocurriendo esta "desincronización" entre los distintos alumnos.
>    
> 2. Si el registro A original de la academia tenía un TTL de 86400, ¿cuánto tiempo máximo tendrán que esperar los alumnos afectados para poder ver la web nueva de forma automática?
>    
> 3. ¿Qué acción preventiva debería haber tomado el administrador del dominio 48 horas antes de hacer la migración para evitar que este problema durase tanto tiempo?

## 9. Auditoría y Diagnóstico de Tráfico DNS
Para garantizar la correcta operativa de una infraestructura de red, no basta con configurar los servicios; el administrador debe ser capaz de auditar su comportamiento en tiempo real. La auditoría y el diagnóstico del tráfico DNS permiten verificar si las resoluciones se están ejecutando correctamente, identificar cuellos de botella y detectar anomalías de seguridad, como el tráfico originado por _malware_ o configuraciones erróneas que exponen la red.
### 9.1. Herramientas de diagnóstico en terminal (CLI)
En los sistemas basados en Linux, los administradores disponen de herramientas nativas (incluidas en el paquete `dnsutils` o `bind9-dnsutils`) para interrogar directamente a los servidores de nombres sin depender de la caché del sistema operativo o del navegador.

**A. El comando `dig` (Domain Information Groper)**
Es la herramienta estándar y más potente en la industria para el diagnóstico de DNS. A diferencia de `nslookup`, `dig` proporciona una salida mucho más detallada, mostrando las secciones exactas de la respuesta del protocolo DNS (Pregunta, Respuesta, Autoridad y Adicional).

```PlainText
//Ejemplo de uso básico para consultar el registro MX de un dominio forzando la consulta contra el servidor de Google (8.8.8.8):

dig @8.8.8.8 miempresa.com MX
```

**B. El comando `tcpdump` (Captura de paquetes en terminal)** 
En servidores sin entorno gráfico, `tcpdump` permite interceptar y registrar el tráfico de red en tiempo real. Es vital para comprobar si las peticiones DNS están llegando físicamente a la tarjeta de red del servidor.

```PlainText
//Ejemplo para escuchar exclusivamente el tráfico DNS (puerto 53) en la interfaz `eth0`:

sudo tcpdump -i eth0 -n port 53
```

### 9.2. Captura y análisis visual de peticiones (Wireshark)
Para un análisis en profundidad, las herramientas de inspección visual de paquetes como **Wireshark** permiten diseccionar la estructura interna del protocolo.

Al analizar tráfico DNS en Wireshark, el administrador debe fijarse en los siguientes identificadores estructurales:

- **Protocolo y Puerto:** Las consultas estándar viajan siempre encapsuladas en protocolo **UDP sobre el puerto 53**. Si la respuesta supera los 512 bytes (común en respuestas con DNSSEC) o se trata de una transferencia de zona (AXFR), el tráfico cambiará a protocolo **TCP sobre el puerto 53**.
    
- **Transaction ID (ID de transacción):** Es un número hexadecimal único de 16 bits que asocia una "Pregunta" con su correspondiente "Respuesta". Evita que el cliente confunda las respuestas si ha lanzado varias consultas simultáneas.
    
- **Flags (Banderas):** Indican el estado de la comunicación. Por ejemplo, si el bit de _Recursion Desired_ (RD) está activado en la petición, o si la respuesta contiene el bit _Authoritative Answer_ (AA), confirmando que el dato proviene del servidor oficial de la zona.
    
- **Texto Plano:** Por defecto, las peticiones DNS tradicionales no van cifradas, por lo que el nombre del dominio consultado (ej. `[www.google.com](https://www.google.com)`) y las IPs devueltas son perfectamente legibles en la captura de red, lo que subraya la necesidad de implementar medidas de seguridad en el servicio.

>[!NOTE] Ejercicio 9 
> Mostrando el tráfico de la red interceptado por Wireshark, al lanzar desde la terminal la siguiente consulta de diagnóstico:
> `nslookup www.gva.es 8.8.8.8`
> 
> Observa atentamente la disección de los dos paquetes capturados en pantalla (la *Standard query* enviada y la *Standard query response* recibida) y responde a las siguientes cuestiones:
> 
> 1. **Capa de Transporte:** Analizando las cabeceras, ¿qué protocolo (TCP o UDP) y qué puerto destino utiliza nuestro equipo para hacerle la petición al servidor 8.8.8.8?
> 2. **Transaction ID:** Compara el código hexadecimal del identificador de transacción de la petición con el de la respuesta. ¿Coinciden? ¿Por qué es arquitectónicamente vital que este número sea exactamente el mismo?
> 3. **Banderas (Flags):** En el panel desplegable del paquete de respuesta, busca la bandera *Recursion available* (RA). ¿Se encuentra activada? ¿Qué nos confirma este estado sobre el rol del servidor 8.8.8.8?
> 4. **Resolución (Answers):** Desplegando la sección de respuestas del segundo paquete. ¿La resolución ha devuelto directamente una única IP de tipo A, o el servidor ha tenido que resolver previamente algún registro de tipo alias (CNAME)?



