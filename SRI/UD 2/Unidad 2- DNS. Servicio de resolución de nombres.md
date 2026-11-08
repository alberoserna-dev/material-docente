
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
