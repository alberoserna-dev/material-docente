# 1. Seguridad informática: concepto y necesidad
Actualmente, existe una realidad inmutable en la sociedad en general y en el sector empresarial en particular: el hardware es reemplazable, pero la información no. Si el centro de datos de una organización se incendia, las máquinas se pueden volver a comprar; sin embargo, si se pierde la base de datos de clientes, las configuraciones críticas o los historiales financieros, la empresa simplemente desaparece.

La seguridad informática nace exactamente para proteger esa supervivencia. Asegurar un sistema no consiste únicamente en frenar a un ciberataque puntual. Un plan de seguridad debe blindarnos también contra el empleado que borra una carpeta sin querer, contra un corte de luz repentino o la pérdida repentina de un disco duro.

La premisa fundamental de esta profesión es que **la seguridad absoluta es una utopía**. Nuestro trabajo no es crear sistemas mágicos imposibles de atacar, sino diseñar arquitecturas donde los riesgos estén calculados, las brechas se detecten de inmediato y exista capacidad de recuperación.

## 1.1. Las cuatro reglas fundamentales
La solidez de cualquier infraestructura tecnológica se fundamenta en el cumplimiento de cuatro atributos esenciales:

- **Confidencialidad:** Significa levantar muros para que nadie vea lo que no debe. Si un intruso o un empleado sin los permisos adecuados logra leer un documento reservado, este principio se ha roto.
- **Integridad:** Consiste en asegurar que los datos no sufren ninguna alteración por el camino. Desde que un archivo se crea, se envía o se guarda, debe mantenerse exacto. Modificar el IBAN de una factura en tránsito es un ataque directo a la integridad.
- **Disponibilidad:** De nada sirve un dato hiperseguro si no podemos acceder a él cuando hace falta. Este principio exige que los servicios respondan a los usuarios legítimos. Si tumban nuestra web corporativa con un ataque de denegación de servicio, hemos perdido la disponibilidad.
- **Fiabilidad:** Es la resistencia al desgaste. Mide cuánto tiempo es capaz de aguantar el sistema haciendo su trabajo de forma correcta antes de sufrir la siguiente avería.

Estos principios mantienen una estrecha interrelación. A modo de ejemplo, el despliegue de mecanismos de control de acceso contribuye simultáneamente a preservar la confidencialidad y la integridad, mientras que el diseño de infraestructuras redundantes o el uso de copias de seguridad inciden de manera directa en la disponibilidad.

### Alta disponibilidad y categorización del servicio

El concepto de Alta Disponibilidad hace referencia a la capacidad de una arquitectura tecnológica para permanecer operativa de manera ininterrumpida, minimizando al máximo los tiempos de inactividad frente a posibles fallos. La disponibilidad de un servicio se clasifica de manera porcentual en distintas categorías:

- **Baja:** Inferior al 90 %.
- **Media:** Entre el 90 % y el 95 %.
- **Alta:** Entre el 95 % y el 99 %.
- **Muy alta:** Entre el 99 % y el 99,9 %.
- **Crítica:** Superior al 99,9 %.

### Métricas para la evaluación de la disponibilidad
Para evaluar el rendimiento de los sistemas y planificar su mantenimiento, se emplean diversas métricas estandarizadas:

- **MTBF (Tiempo Medio Entre Fallos):** Aplicable a sistemas reparables, indica el promedio de tiempo que el equipo funciona correctamente entre una incidencia y la siguiente. Cuanto mayor es su valor, mayor es la fiabilidad del activo.
	- Si un servidor opera de manera estable durante 1000 horas antes de averiarse; tras su reparación, funciona otras 800 horas hasta sufrir una segunda incidencia y, finalmente, alcanza 1200 horas operativas antes de un tercer fallo. El cálculo se establecería promediando dichos periodos: 
		`MTBF = (1000 + 800 + 1200) / 3 = 1000 horas`. 
	- Esto determina que el sistema ofrece una media de 1000 horas de servicio ininterrumpido entre fallos.

- **MTTR (Tiempo Medio de Reparación):** Representa el tiempo promedio necesario para restablecer el servicio tras la ocurrencia de una avería. La optimización del mantenimiento busca reducir esta métrica al mínimo posible.
	- Tomando como referencia las tres averías del caso anterior, si los tiempos de intervención técnica requeridos para subsanarlas han sido de 2, 4 y 3 horas respectivamente, la métrica se obtiene de la siguiente manera: 
		`MTTR = (2 + 4 + 3) / 3 = 3 horas`. 
	- Esto indica que las tareas de recuperación exigen una media de 3 horas por incidente.

- **MTTF (Tiempo Medio Hasta el Fallo):** Se emplea en componentes considerados no reparables (como baterías o unidades de almacenamiento), estimando el tiempo medio de funcionamiento ininterrumpido hasta su fallo definitivo.
	- Si se somete a prueba un lote de cuatro unidades de almacenamiento y presentan fallos irreversibles a las 4000, 5000, 6000 y 5000 horas de uso, su ciclo de vida promedio se determina así: 
		`MTTF = (4000 + 5000 + 6000 + 5000) / 4 = 5000 horas`. 
	- Este valor establece la vida útil esperada del hardware antes de su degradación total.

A partir de estos indicadores, es posible cuantificar la disponibilidad real mediante las siguientes expresiones matemáticas:


- **Fórmula genérica:** 
	`Disponibilidad (%) = ((Tiempo de funcionamiento total - Tiempo de inactividad) / Tiempo de funcionamiento total) * 100`
    
- **Para sistemas reparables:** 
	`Disponibilidad = MTBF / (MTBF + MTTR)`
    
- **Para componentes no reparables:** 
	`Disponibilidad = MTTF / (MTTF + MTTR)`

## 1.2. La paradoja de la seguridad y el compromiso operativo

Incrementar la seguridad de un sistema informático suele requerir la introducción de nuevos controles, pasos adicionales y restricciones. Sin embargo, si estas medidas se aplican de forma desproporcionada o sin criterio, el sistema puede resultar incómodo de manejar o incluso perder parte de su funcionalidad original. Por este motivo, una buena administración de sistemas consiste en encontrar el equilibrio adecuado entre la seguridad, la funcionalidad y la facilidad de uso.

Un ejemplo claro de esta situación ocurre con las políticas de contraseñas. Si el sistema obliga a los usuarios a crear claves excesivamente largas y a cambiarlas con mucha frecuencia, el resultado suele ser contraproducente. Para evitar olvidar las contraseñas, las personas terminan anotándolas en papeles visibles o utilizando patrones fáciles de adivinar. De este modo, aunque la medida técnica exista, el comportamiento que provoca acaba reduciendo la protección real de la empresa.

# 2. Vulnerabilidades, amenazas, ataques y riesgos

Para gestionar correctamente la seguridad de una infraestructura, es imprescindible comprender las diferencias exactas entre los factores que la ponen en peligro.
## 2.1. Vulnerabilidad

Una vulnerabilidad es cualquier debilidad, fallo o deficiencia presente en un sistema que podría facilitar la aparición de un incidente de seguridad. La simple existencia de un punto débil no implica que el sistema esté siendo atacado en ese momento, sino que existe una puerta abierta que alguien o algo podría aprovechar. Estas brechas pueden aparecer en diferentes niveles:

- **Vulnerabilidades de hardware:** Incluyen el desgaste natural de los componentes, la falta de elementos redundantes (como discos o fuentes de alimentación de respaldo), la exposición física de los equipos a personal no autorizado o la ausencia de condiciones ambientales adecuadas (control de temperatura, humedad o suministro eléctrico).

- **Vulnerabilidades de software:** Se derivan de fallos de programación, sistemas operativos sin actualizar, servicios activos que no son necesarios, asignación excesiva de privilegios a los usuarios o el uso de credenciales por defecto.

- **Vulnerabilidades de los datos:** Aparecen cuando la información confidencial se almacena sin cifrar, cuando las copias de seguridad son inexistentes o nunca se comprueba si funcionan, y cuando los permisos de lectura o escritura no están correctamente limitados.

- **Vulnerabilidades de red:** Surgen por configuraciones deficientes, como mantener puertos abiertos sin justificación, emplear protocolos de transmisión de datos sin cifrar (como HTTP o Telnet), utilizar redes inalámbricas con protección débil o carecer de una correcta segmentación que separe los equipos críticos del resto de la red.

### Análisis y detección de vulnerabilidades

El análisis de vulnerabilidades es el procedimiento utilizado para localizar y clasificar estas debilidades antes de que ocasionen un problema. Este trabajo se apoya en revisiones manuales y en el uso de herramientas de software especializadas que automatizan la búsqueda de fallos:

|**Herramienta**|**Enfoque principal**|**Función destacada**|
|---|---|---|
|**Nmap**|Análisis de red y puertos|Orientado al descubrimiento de equipos conectados y servicios activos, sirviendo como base para posteriores análisis.|
|**Nessus**|Escaneo de servicios accesibles|Detección automatizada de vulnerabilidades conocidas y generación de informes de gravedad.|
|**OpenVAS / Greenbone**|Escaneo de servicios accesibles|Alternativa de código abierto para la identificación y gestión de vulnerabilidades en red.|
|**Lynis**|Auditoría interna del sistema|Análisis profundo en entornos Linux/Unix para proponer mejoras de fortificación (_hardening_) en la configuración del equipo.|
|**OpenSCAP**|Auditoría de normativas|Comprobación automatizada para verificar si un sistema cumple con políticas o estándares de seguridad específicos.|

## 2.2. Conceptos de amenaza, ataque y riesgo

Aunque a menudo se confunden, es necesario distinguir tres términos fundamentales en la gestión de incidentes:

- **Amenaza:** Es cualquier evento, agente o circunstancia que tiene el potencial de causar daño a un sistema. Puede tener un origen natural, accidental o intencionado.
    
- **Ataque:** Es la materialización intencionada de una amenaza. Consiste en una acción deliberada para comprometer la seguridad. Por ello, todo ataque es una amenaza, pero no todas las amenazas son ataques (un incendio es una amenaza, pero no un ataque).
    
- **Riesgo:** Representa la probabilidad real de que una amenaza concreta consiga explotar una vulnerabilidad existente, calculando además el impacto negativo que esto generaría.

La evolución habitual de un incidente sigue esta secuencia lógica: **Activo** (Lo que protegemos) → **Vulnerabilidad** (El punto débil) → **Amenaza** (El factor de riesgo) → **Riesgo** (La probabilidad de daño) → **Incidente** (El suceso consumado).

|**Situación del sistema (Vulnerabilidad)**|**Evento desencadenante (Amenaza)**|
|---|---|
|Uso de una contraseña sencilla como "123456".|Un programa automatizado realizando un ataque de fuerza bruta.|
|Un servidor web con el sistema operativo sin actualizar.|Un software malicioso diseñado para explotar ese fallo concreto.|
|Un equipo conectado directamente a la red sin protección eléctrica.|Un corte o pico de tensión en el suministro eléctrico.|
|Ausencia de cerraduras o control de acceso en la sala de servidores.|El acceso físico de un intruso para robar o dañar el equipo.|

## 2.3. Tipología de amenazas: físicas y lógicas

Las amenazas se dividen en dos grandes grupos según la naturaleza del daño que provocan:

- **Amenazas físicas:** Son aquellas que actúan de manera directa sobre el hardware, las instalaciones o los soportes de almacenamiento. Ejemplos comunes son los incendios, las inundaciones, las averías eléctricas, el robo de material o el exceso de temperatura en el centro de datos.
    
- **Amenazas lógicas:** Actúan a nivel de software e información. Comprenden el _malware_, el secuestro de datos (_ransomware_), las intrusiones no autorizadas, la explotación de código y los ataques orientados a colapsar un servicio.

### Ingeniería social y fraude informático

La ingeniería social es un conjunto de técnicas que no ataca a la tecnología, sino al factor humano. Se basa en la manipulación psicológica para lograr que un usuario entregue sus credenciales, instale software malicioso o revele información confidencial de forma voluntaria.

Las técnicas de manipulación más habituales incluyen:

- **Phishing:** Envío de mensajes fraudulentos que suplantan la identidad de una entidad legítima para engañar al usuario y conseguir que facilite datos sensibles.
    
- **Spear phishing:** Variación del _phishing_ diseñada a medida para una víctima específica, utilizando información previa para ganar su confianza.
    
- **Smishing y Vishing:** Uso de mensajes de texto (SMS) o llamadas de voz telefónicas, respectivamente, para llevar a cabo el engaño.
    
- **Pretexting:** Invención de un escenario ficticio muy elaborado para justificar la solicitud de información confidencial.
    
- **Baiting:** Utilización de un cebo (físico o digital) para despertar la curiosidad de la víctima y empujarla a realizar una acción perjudicial.
    
- **Sniffing:** Captura y lectura silenciosa del tráfico de red para interceptar información sin cifrar.
    
- **Spoofing:** Falsificación técnica de una identidad (como falsear una dirección IP, una dirección MAC o un remitente de correo electrónico) para simular ser una fuente de confianza.

La defensa contra estas prácticas requiere medidas preventivas enfocadas en el personal y en los procesos, tales como la formación continua, la verificación de operaciones críticas a través de vías alternativas, la adopción del múltiple factor de autenticación (MFA) y la creación de un entorno seguro para que los empleados reporten sospechas sin temor a ser penalizados.

# 3. Seguridad física y del entorno

Es un error frecuente suponer que una infraestructura tecnológica está a salvo simplemente por contar con barreras de software, como cortafuegos o programas antivirus. Si una persona sin autorización logra acceder al recinto donde se alojan los servidores y desconecta un cable o extrae un disco duro, todas las defensas lógicas quedan anuladas al instante. Por ello, la seguridad física se encarga de proteger el hardware y los soportes de almacenamiento frente a daños materiales, factores climáticos adversos y accesos no permitidos.

## 3.1. Control de accesos y ubicación de los equipos

Para garantizar la integridad física de los sistemas críticos, estos deben situarse obligatoriamente en áreas de acceso restringido. Las medidas fundamentales incluyen:

- **Restricción de entrada:** El paso a las salas técnicas debe limitarse exclusivamente al personal autorizado, empleando métodos de autenticación como lectores biométricos, tarjetas inteligentes con chip o códigos PIN.
    
- **Protección del hardware:** Los armarios de comunicaciones y _racks_ donde se atornillan los servidores deben permanecer cerrados con llave.
    
- **Monitorización continua:** Es imprescindible instalar sistemas de videovigilancia que registren la actividad tanto en los puntos de entrada como en el interior de los centros de datos.
    
- **Separación de respaldos:** Como medida de precaución básica, los soportes que contienen las copias de seguridad deben almacenarse en una ubicación física distinta a la del sistema original que protegen.

## 3.2. Respaldo eléctrico: Sistemas de Alimentación Ininterrumpida (SAI)

Las anomalías en el suministro eléctrico representan una amenaza constante. Un Sistema de Alimentación Ininterrumpida (SAI, o UPS por sus siglas en inglés) es un equipo diseñado para proporcionar energía de forma temporal cuando se produce un corte en la red eléctrica, además de estabilizar posibles alteraciones del flujo.

Su objetivo principal no es mantener la infraestructura funcionando de forma indefinida, sino ofrecer el tiempo suficiente para que los sistemas de respaldo (como generadores diésel) arranquen, o bien permitir un apagado automático y ordenado de los servidores para evitar la pérdida o corrupción de datos.

A nivel interno, un SAI se compone de tres piezas clave:

1. **Batería:** Es el núcleo del sistema, responsable de almacenar la energía que se liberará durante el corte de luz.
    
2. **Rectificador:** Se encarga de transformar la corriente alterna que llega de la red eléctrica en corriente continua, permitiendo así cargar la batería.
    
3. **Inversor:** Realiza el proceso inverso, convirtiendo la corriente continua almacenada en la batería en corriente alterna para poder alimentar los equipos informáticos conectados.

Al implementar un SAI en un entorno profesional, el administrador debe evaluar características técnicas como la potencia total que puede soportar, la autonomía de las baterías, la topología del equipo (existen modelos _Offline_, _Online_ e _Interactivos_, que varían en su forma de filtrar picos de corriente) y su capacidad para comunicarse por red con los servidores.

## 3.3. Acondicionamiento ambiental

Las salas técnicas y los centros de proceso de datos (CPD) exigen un control exhaustivo de su entorno físico para prevenir el deterioro de los componentes. Los equipos deben ubicarse en zonas libres de polvo, impactos, humedad excesiva o altas temperaturas.

Para lograrlo, las instalaciones deben contar con sistemas avanzados de climatización y ventilación. Además, es crítico desplegar sensores de inundación y mecanismos de protección contra el fuego, lo que incluye detectores de humo de muy alta sensibilidad combinados con sistemas de extinción automática (como rociadores de gas especializado) y extintores manuales adecuados para material electrónico.

# 4. Seguridad Lógica

La seguridad lógica engloba todos los mecanismos técnicos orientados a controlar y restringir el acceso a la información, las redes y las aplicaciones. Esta rama de la seguridad hace uso de herramientas como los procesos de autenticación, la asignación de permisos, el cifrado de datos, el uso de cortafuegos y la gestión de copias de seguridad.

## 4.1. El principio de mínimo privilegio

Una regla fundamental en la administración de sistemas es que cada usuario, proceso o servicio debe contar única y exclusivamente con los permisos estrictamente necesarios para desempeñar su función. Al reducir los privilegios al nivel más bajo posible, se acota de forma considerable el daño que podría ocasionar un error humano, la vulneración de una cuenta o la ejecución de un software malicioso.

## 4.2. Criptografía y protección de la información

La criptografía recurre a algoritmos matemáticos para transformar los datos y protegerlos, garantizando propiedades clave como la confidencialidad y la integridad tanto durante su almacenamiento como en su transmisión. Para asegurar información crítica, como documentos o credenciales, se emplean principalmente dos mecanismos diferenciados: las funciones hash y los algoritmos de cifrado.

### Funciones Hash y verificación de integridad

Una función hash es un proceso matemático que toma un archivo o conjunto de datos de cualquier tamaño y devuelve una cadena de texto de longitud fija, conocida como resumen o huella digital.

Para que un algoritmo hash sea apto en ciberseguridad, debe cumplir ciertas propiedades:

- **Unidireccionalidad:** Es matemáticamente inviable reconstruir el archivo original a partir de su huella. No existe el concepto de "descifrar un hash".
    
- **Longitud fija:** Independientemente de si el archivo original ocupa unos pocos kilobytes o varios gigabytes, el resultado siempre mantendrá el mismo tamaño (por ejemplo, 256 bits).
    
- **Alta sensibilidad:** Modificar un único carácter en el archivo original genera un hash completamente distinto.
    
- **Resistencia a colisiones:** Debe ser extremadamente difícil encontrar dos archivos diferentes que generen el mismo resumen.

Históricamente se utilizaron algoritmos como MD5 o SHA-1, pero en la actualidad se consideran obsoletos y vulnerables. Los estándares recomendados son SHA-256 y SHA-512.

**Aplicación práctica en sistemas Linux:** El comando `sha256sum archivo.txt` permite obtener la huella digital de un fichero. Si tiempo después se vuelve a calcular y el resultado es idéntico, queda garantizado que el documento no ha sufrido alteraciones. Cabe destacar que el hash certifica la integridad de los datos, pero no oculta su contenido; por tanto, no aporta confidencialidad.

### Cifrado de datos

El cifrado transforma información legible (texto en claro) en un formato incomprensible (texto cifrado) mediante el uso de un algoritmo y una clave. A diferencia de las funciones hash, el cifrado es un proceso reversible: el destinatario puede recuperar la información original si dispone de la clave adecuada para el descifrado. Su propósito principal es asegurar la confidencialidad.

Existen dos modalidades principales:

- **Cifrado simétrico:** Utiliza exactamente la misma clave secreta para cifrar y para descifrar. Destaca por su velocidad al procesar grandes volúmenes de datos, aunque presenta el reto de compartir dicha clave con el destinatario de forma segura.
    
- **Cifrado asimétrico:** Emplea un par de claves vinculadas matemáticamente: una clave pública (que se puede distribuir libremente) y una clave privada (que debe mantenerse en secreto absoluto).

**Aplicación práctica en sistemas Linux:** La herramienta GPG (GNU Privacy Guard) permite realizar operaciones criptográficas de forma nativa. Para cifrar un archivo de forma simétrica, se utiliza el comando `gpg -c archivo.txt`, lo que generará un nuevo fichero ilegible terminado en `.gpg` protegido por una contraseña. Para revertir el proceso y leer los datos, se emplea el comando `gpg -d archivo.txt.gpg`.

**Comparativa general:**

|**Característica**|**Función Hash**|**Cifrado**|
|---|---|---|
|**Objetivo principal**|Comprobar la integridad de los datos|Proteger la confidencialidad|
|**Uso de claves**|No requiere clave|Sí requiere clave|
|**Reversibilidad**|No es reversible|Es reversible con la clave correcta|
|**Ejemplo de tecnología**|SHA-256|GPG|

### Sistemas híbridos y estados de la información

En escenarios reales, es habitual combinar ambas técnicas de cifrado. Protocolos fundamentales como TLS (utilizado para asegurar el tráfico web) emplean criptografía asimétrica para intercambiar las contraseñas de forma segura y, posteriormente, pasan a usar criptografía simétrica para transmitir toda la información de la sesión con mayor fluidez.

La estrategia de protección siempre debe adaptarse al estado en el que se encuentre la información:

- **Datos en reposo:** Información almacenada de forma persistente en discos, que requiere protección mediante el cifrado de volúmenes o bases de datos.
    
- **Datos en tránsito:** Información que viaja por la red y que debe canalizarse a través de túneles seguros utilizando protocolos como SSH, TLS o configuraciones de VPN.

## 4.3. Listas de Control de Acceso (ACL)

El sistema de permisos tradicional de muchos sistemas operativos (que divide el acceso entre propietario, grupo y el resto de usuarios) puede resultar rígido en entornos corporativos complejos. Las Listas de Control de Acceso (ACL) resuelven esta limitación permitiendo asignar privilegios granulares y específicos a diferentes usuarios sobre un mismo recurso.

**Aplicación práctica en sistemas Linux:** Si se desea otorgar permisos de lectura y escritura al usuario "ana" sobre el directorio `/proyecto` sin alterar los permisos del propietario original, se emplea el comando `setfacl -m u:ana:rw /proyecto`. Para auditar las reglas aplicadas sobre ese recurso, se utiliza el comando de lectura `getfacl /proyecto`.

## 4.4. Políticas de contraseñas y métodos de autenticación

Las contraseñas representan el mecanismo de autenticación más extendido para restringir el acceso a los sistemas informáticos. Su eficacia se basa en verificar la identidad del usuario mediante una información secreta que solo él debería conocer. Sin embargo, si las claves son demasiado breves, previsibles o se reutilizan en múltiples plataformas, el nivel de riesgo aumenta de forma drástica.

Para evitar estas vulnerabilidades, se establece una **política de contraseñas**: un conjunto de normas técnicas diseñadas para asegurar que las credenciales mantengan una robustez adecuada a lo largo de su ciclo de vida.

Entre los parámetros que suele definir una política corporativa destacan:

- Longitud mínima exigida y grado de complejidad (uso de distintos tipos de caracteres).
    
- Bloqueo de términos comunes o patrones predecibles.
    
- Historial de contraseñas, para impedir que el usuario alterne repetidamente entre las mismas claves.
    
- Tiempos de validez: un plazo máximo antes de forzar su caducidad y un plazo mínimo para evitar cambios compulsivos en un mismo día.
    
- Restricción de intentos fallidos para mitigar ataques de fuerza bruta.

> [!NOTE] **Aclaración sobre tendencias actuales:** Aunque tradicionalmente se exigía renovar las contraseñas cada cierto tiempo (por ejemplo, cada 90 días), las directrices de seguridad modernas priorizan la creación de claves largas y únicas, y desaconsejan forzar su cambio periódico a menos que exista una sospecha real de que han sido comprometidas.

### Gestión de contraseñas en entornos Linux

Por motivos de seguridad, los sistemas Linux nunca almacenan las contraseñas en texto claro. En su lugar, el sistema guarda un "hash" derivado de la clave. Cuando el usuario intenta iniciar sesión, el sistema calcula el hash de la palabra introducida y lo compara con el almacenado, permitiendo validar la identidad sin necesidad de conocer la contraseña original.

A nivel de archivos del sistema, el registro de usuarios se localiza en `/etc/passwd`, pero una marca con la letra `x` indica que la información real de las credenciales está resguardada en otro fichero mucho más restrictivo: `/etc/shadow`. Este último contiene el hash de la contraseña junto a los datos numéricos que controlan su antigüedad, caducidad y periodos de aviso.

Para administrar el ciclo de vida de las contraseñas de cuentas ya existentes, los administradores emplean el comando `chage`:

- Visualizar el estado de un usuario: `sudo chage -l [usuario]`.
    
- Modificar su política: `sudo chage -M 90 -m 1 -W 7 [usuario]` (estableciendo un máximo de 90 días, un mínimo de 1 día entre cambios y un aviso previo de 7 días).

Por otro lado, los valores predeterminados que se aplicarán a las **nuevas** cuentas creadas en el sistema se definen en el archivo `/etc/login.defs`, mediante variables como `PASS_MAX_DAYS`, `PASS_MIN_DAYS` y `PASS_WARN_AGE`.

### Arquitectura PAM y calidad de las contraseñas

En Linux, la validación de contraseñas se gestiona mediante una arquitectura modular denominada **PAM (Pluggable Authentication Modules)**. Su gran ventaja es que centraliza la lógica de autenticación, permitiendo que servicios como el inicio de sesión local (`login`), la elevación de privilegios (`sudo`) o el acceso remoto (`ssh`) apliquen las mismas reglas de seguridad sin tener que programarlas de forma individual.

Para auditar y garantizar la fortaleza de las contraseñas antes de que el sistema las acepte, se utiliza el módulo `pam_pwquality`. Su archivo de configuración, ubicado en `/etc/security/pwquality.conf`, permite imponer restricciones matemáticas, como una longitud mínima (`minlen = 12`) o la exigencia de mezclar distintos tipos de caracteres (`minclass = 3`).

### Autenticación Multifactor (MFA)

Para robustecer el acceso, el modelo MFA combina diferentes dimensiones de autenticación:

1. **Algo que el usuario sabe:** Una contraseña o un código PIN.
    
2. **Algo que el usuario posee:** Un dispositivo móvil, una tarjeta inteligente o un _token_ generador de códigos.
    
3. **Algo que el usuario es:** Un rasgo físico validado mediante biometría.

## 4.5. Sistemas de identificación biométrica

La biometría autentica a los individuos basándose en características físicas o de comportamiento únicas, como la huella dactilar, el reconocimiento facial o el escáner de iris.

- **Ventajas:** Ofrece un acceso rápido, no requiere memorizar claves y es una tecnología idónea para integrar en sistemas MFA porque los rasgos son sumamente difíciles de replicar.
    
- **Inconvenientes:** Presenta desafíos relacionados con la privacidad de los usuarios y falsos positivos/negativos en la detección. Además, si un patrón biométrico se ve comprometido, el usuario no puede "cambiar" su huella dactilar como si fuera una contraseña ordinaria, lo que hace que estas bases de datos sean extremadamente críticas.
    

En el mercado actual, tecnologías como Windows Hello o Apple Touch ID han estandarizado este tipo de accesos en entornos corporativos y personales.

## 4.6. Políticas de almacenamiento

Diseñar un modelo de almacenamiento seguro implica tomar decisiones que van mucho más allá de seleccionar el modelo de disco duro. Una política integral debe documentar dónde se aloja la información confidencial, qué usuarios tienen permisos de acceso, qué mecanismos de cifrado se aplican, cómo se estructuran las redundancias técnicas y cuáles son los procedimientos exactos de restauración en caso de desastre.

## 4.7. Respaldo de información: Copias e imágenes

Las copias de seguridad (_backups_) son el último salvavidas frente a ataques de _ransomware_, borrados accidentales o catástrofes físicas. Existen tres metodologías principales para realizarlas:

- **Completa:** Copia la totalidad de los datos seleccionados. Su recuperación es directa y sencilla, pero consume gran cantidad de tiempo y espacio de almacenamiento.
    
- **Incremental:** Solo respalda los archivos que hayan sufrido modificaciones desde la copia inmediatamente anterior (ya fuera completa o incremental). Es muy rápida, pero su restauración requiere aplicar ordenadamente todas las piezas de la cadena.
    
- **Diferencial:** Únicamente guarda los cambios producidos desde la última copia completa. Ocupa más espacio que la incremental, pero simplifica el proceso de recuperación.

En sistemas Linux, las tareas de respaldo se suelen automatizar mediante cron y herramientas como `rsync`. Por ejemplo, el comando `rsync -av --delete /origen /destino` sincroniza directorios de forma exacta, eliminando en el destino aquellos ficheros que ya no existan en el origen.

Por otro lado, cuando se necesita asegurar el sistema operativo al completo (incluyendo particiones, configuraciones y aplicaciones), se generan **imágenes de respaldo** con herramientas como Clonezilla. Esto permite revivir un servidor inoperativo en cuestión de minutos sin tener que reinstalar el software desde cero.

## 4.8. Tolerancia a fallos: Sistemas RAID

Es vital comprender que **un sistema RAID no es una copia de seguridad**. Su función es agrupar varios discos físicos para aumentar el rendimiento o asegurar que la máquina siga operando si un disco se rompe, pero no protege frente a borrados accidentales ni frente a virus, ya que el daño se replicaría instantáneamente en todos los discos del conjunto.

Los niveles más habituales son:

- **RAID 0 (Striping):** Reparte los datos entre varios discos para multiplicar la velocidad de lectura y escritura. No ofrece tolerancia a fallos; si un disco muere, se pierde toda la información.
    
- **RAID 1 (Mirroring):** Crea un "espejo" exacto de los datos en dos discos. Si uno falla, el sistema sigue funcionando ininterrumpidamente con el otro. En Linux, se configura mediante el comando `mdadm`.
    
- **RAID 5:** Distribuye los datos y añade información de paridad matemática entre un mínimo de tres discos. Permite sobrevivir a la pérdida de un disco optimizando mejor el espacio que un RAID 1.

## 4.9. Medios físicos de almacenamiento

La elección del soporte físico es una decisión crítica que impacta directamente en la capacidad, la velocidad, el coste, la portabilidad, la vida útil y la seguridad de los datos. No existe una solución única que sea idónea para todos los escenarios:

- **Cinta magnética:** Destaca por ofrecer una enorme capacidad de almacenamiento con un coste por terabyte muy reducido. Su sistema de lectura y escritura es secuencial, lo que la hace lenta para el uso diario, y requiere de hardware especializado. No obstante, sigue siendo una pieza clave en entornos empresariales para la conservación de copias a muy largo plazo.
    
- **Discos mecánicos (HDD):** Proporcionan alta capacidad a un precio asequible. Sin embargo, al estar compuestos por partes mecánicas móviles, presentan una mayor sensibilidad al desgaste y a los impactos físicos en comparación con los discos de estado sólido.
    
- **Unidades de estado sólido (SSD):** Basadas en memoria flash, carecen por completo de piezas mecánicas. Su principal ventaja es la altísima velocidad de lectura y escritura, siendo la opción obligatoria cuando el sistema requiere un alto rendimiento y una latencia mínima.
    
- **Memorias USB:** Son dispositivos extremadamente portátiles y cómodos para realizar transferencias rápidas. El mayor riesgo que presentan es la facilidad con la que pueden extraviarse o ser robados, por lo que resulta indispensable cifrar su contenido siempre que transporten información sensible o confidencial.
    
- **Soportes ópticos (CD / DVD / Blu-ray):** Aunque han perdido gran parte de su protagonismo comercial en favor de las descargas y el almacenamiento flash, mantienen cierta utilidad en escenarios específicos, como la distribución física inalterable de software o el archivado cerrado.
    
- **Sistemas NAS (Network Attached Storage):** Consisten en servidores de almacenamiento centralizados y conectados a la red local. Facilitan que múltiples usuarios accedan simultáneamente a los recursos, permiten aplicar controles de permisos avanzados y suelen incorporar de forma nativa la gestión de copias y configuraciones RAID.
    
- **Almacenamiento en la nube (Cloud):** Representa el alojamiento remoto de datos en la infraestructura de un proveedor externo. Aporta gran escalabilidad y permite el acceso desde cualquier lugar. Su uso exige evaluar cuidadosamente los niveles de disponibilidad garantizados, el cumplimiento de las normativas de privacidad, el cifrado de la información, la ubicación geográfica real de los servidores y la dependencia tecnológica respecto al proveedor.
    
- **Almacenamiento en cristal:** Es una tecnología emergente basada en el uso de materiales ópticos y fotónicos. Su desarrollo está orientado a la conservación de datos durante lapsos de tiempo extremadamente largos, aunque en la actualidad no se considera una solución de uso general en la administración de sistemas rutinaria.

En la práctica, un diseño empresarial sólido suele combinar varias de estas tecnologías. Una estrategia habitual consiste en utilizar un sistema NAS interno equipado con discos SSD rápidos en configuración redundante, complementado con un sistema de copias automáticas dirigidas a otra ubicación física o a un entorno en la nube. La redundancia local garantiza la disponibilidad inmediata, mientras que la copia externa es el escudo definitivo frente a desastres que puedan destruir la sede principal de la organización.

## 4.10. Protocolos de recuperación de datos

Cuando la información se daña o se elimina por error, las opciones de recuperación varían según el sistema operativo y el grado de sobrescritura de los sectores del disco. La norma principal es evitar realizar nuevas escrituras en el dispositivo afectado.

En Windows existen utilidades dedicadas como Recuva, mientras que en Linux destacan herramientas como TestDisk (enfocada en recuperar particiones dañadas) y PhotoRec (especializada en recuperar ficheros individuales rastreando firmas de datos, técnica conocida como _carving_).

## 4.11. Auditorías de seguridad

Una auditoría de seguridad consiste en una revisión sistemática y exhaustiva cuyo propósito es identificar configuraciones defectuosas, debilidades técnicas, posibles riesgos y desviaciones respecto a las políticas establecidas por la organización. La simple implementación de controles iniciales (como cortafuegos o permisos) resulta insuficiente si no se verifica de manera periódica que continúan siendo efectivos y que se ajustan a la realidad del entorno. Estas evaluaciones pueden llevarse a cabo de forma manual, mediante software automatizado, o combinando ambos enfoques.

### Auditoría en entornos Windows

En los sistemas operativos de Microsoft, la herramienta central para la monitorización nativa es el **Visor de eventos**. Esta utilidad recopila y clasifica de forma estructurada los registros (_logs_) correspondientes al propio sistema, a las aplicaciones en ejecución y, de forma crítica, a la seguridad.

El análisis detallado de estos registros permite a los administradores investigar intentos de inicio de sesión (tanto exitosos como fallidos), alteraciones en los privilegios de los usuarios, errores críticos y cualquier otra actividad sospechosa. Para garantizar una auditoría eficaz y adaptada a las amenazas modernas, es imperativo utilizar y dominar las herramientas de seguridad y administración vigentes en las versiones actuales del sistema operativo.

### Auditoría en entornos Linux

La evaluación de la seguridad en sistemas basados en Linux se apoya frecuentemente en herramientas de análisis profundo. Entre ellas destaca **Lynis**, una utilidad diseñada para auditar el sistema operativo, revisar el software instalado y evaluar las configuraciones activas en busca de posibles vulnerabilidades. Su ejecución mediante el comando `lynis audit system` proporciona un informe detallado con advertencias y recomendaciones de fortificación.

Paralelamente, la suite **OpenSCAP** permite automatizar la comprobación del sistema para verificar si su configuración cumple con políticas o estándares de seguridad concretos y oficiales. Cabe destacar que el resultado de estas herramientas automatizadas sirve como guía, pero no sustituye el análisis y la interpretación final que debe realizar el administrador del sistema.

### Auditoría de red

Las deficiencias en la configuración de la infraestructura de red pueden dejar expuestos servicios internos que no deberían ser accesibles desde el exterior. Para auditar el estado del perímetro y de las redes internas sobre las que se tiene autorización, se emplean herramientas de reconocimiento como **Nmap**. Esta utilidad permite identificar los equipos conectados (_hosts_), escanear los puertos activos y descubrir qué servicios se están ejecutando en ellos.

A modo de ejemplo, la ejecución del comando `nmap -sS -p 1-65535 192.168.1.1` instruye a la herramienta para realizar un sondeo rápido (tipo SYN) abarcando la totalidad de los puertos posibles de esa dirección IP. Una vez descubierto el mapa de red, para dar un paso más y detectar vulnerabilidades previamente documentadas en esos servicios, se recurre a escáneres especializados como **OpenVAS / Greenbone**.

# 5. Protección perimetral

La conexión de cualquier infraestructura informática a internet u otras redes públicas exige el diseño de una estrategia de seguridad perimetral. El propósito de este perímetro es establecer fronteras claras entre áreas que poseen diferentes niveles de confianza, regulando de forma estricta el flujo de información que transita entre ellas.

Es fundamental interiorizar el concepto de "defensa en profundidad": la seguridad de una red jamás debe sostenerse exclusivamente en un único cortafuegos. Para que la arquitectura sea resistente, requiere la superposición de múltiples capas de protección:

- **Cortafuegos (Firewalls):** Sistemas encargados de inspeccionar y filtrar el tráfico de red entrante y saliente según un conjunto de reglas.
    
- **Segmentación de red:** División de la infraestructura en subredes aisladas para limitar la capacidad de movimiento de un atacante que haya logrado penetrar en el sistema.
    
- **Zonas Desmilitarizadas (DMZ):** Subredes aisladas donde se ubican los equipos que forzosamente deben ser accesibles desde el exterior (como servidores web o de correo), creando una barrera extra que protege la red interna corporativa.
    
- **Redes Privadas Virtuales (VPN):** Túneles de comunicación cifrados para garantizar que las conexiones de usuarios remotos o la interconexión entre sedes se realice de forma segura.
    
- **Monitorización e IDS/IPS:** Implementación de Sistemas de Detección y Prevención de Intrusos para identificar patrones anómalos o comportamientos maliciosos en tiempo real.
    
- **Fortificación de equipos:** Aplicación de actualizaciones constantes y políticas de endurecimiento (_hardening_) sobre cualquier servidor expuesto al exterior.
    
- **Gestión de incidentes:** Mantenimiento de registros de actividad (_logs_) centralizados y diseño de procedimientos claros de actuación frente a posibles brechas de seguridad.

# 6. Introducción a la informática forense digital

El análisis forense digital es la disciplina encargada de investigar infraestructuras, dispositivos móviles y medios de almacenamiento con el fin de recolectar, proteger y examinar pruebas vinculadas a un incidente de seguridad o a un delito informático. Su propósito es triple: reconstruir la secuencia exacta de los hechos, determinar las técnicas utilizadas durante el incidente y garantizar que las pruebas obtenidas posean una validez técnica y jurídica irrefutable.

## 6.1. Objetivo y cadena de custodia

La obtención de pruebas digitales exige un rigor metodológico absoluto, ya que cualquier alteración no controlada de una evidencia puede invalidarla. Para prevenir este problema, se establece la denominada **cadena de custodia**. Este procedimiento consiste en documentar de forma ininterrumpida todo el ciclo de vida de la prueba: qué especialista la ha extraído, quién la ha manipulado, en qué momento, dónde ha permanecido almacenada y qué operaciones técnicas precisas se han aplicado sobre ella.

## 6.2. Fases de adquisición y análisis de evidencias

El protocolo estándar de actuación forense se divide en cuatro grandes etapas consecutivas:

1. **Aseguramiento de la escena:** El primer paso consiste en tomar el control del entorno para evitar que la situación continúe deteriorándose. Según la naturaleza del incidente, puede ser necesario aislar la máquina de la red corporativa para evitar la propagación de un ataque. Es crucial comprender que desconectar el equipo de la corriente no siempre es la decisión correcta; un apagado repentino destruye de forma irremediable la información almacenada en la memoria volátil, la cual podría ser vital para la investigación.
    
2. **Generación de la imagen forense:** Se realiza un clonado exacto, bit a bit, del medio de almacenamiento utilizando hardware o software especializado. A diferencia de una simple copia de archivos, una imagen forense captura también el espacio libre, las estructuras del sistema de archivos y las áreas ocultas, que son los lugares donde habitualmente residen los datos borrados.
    
3. **Verificación de la integridad:** Para certificar que la imagen forense es una réplica exacta del original y no ha sufrido manipulaciones, se calculan las funciones _hash_ de ambas unidades. Si los valores resultantes son idénticos, existe una evidencia técnica irrefutable de la fidelidad de la copia, y dichos valores _hash_ deben quedar documentados formalmente.
    
4. **Ejecución del análisis:** Por norma general, la investigación nunca se realiza sobre el dispositivo original. Todo el trabajo forense (búsqueda de elementos eliminados, extracción de metadatos, auditoría de actividad de usuarios y revisión de conexiones) se lleva a cabo exclusivamente sobre las copias de trabajo generadas.

# 7. Herramientas aplicadas al análisis forense

La recolección y evaluación de evidencias digitales requiere el uso de utilidades especializadas, las cuales varían según el entorno de trabajo afectado:

## 7.1. Entornos Windows

- **FTK Imager:** Software de referencia utilizado para la adquisición de imágenes forenses y la previsualización del contenido de distintos soportes.
    
- **EnCase:** Plataforma comercial de investigación forense de uso muy extendido en entornos corporativos.
    
- **Volatility:** Entorno de trabajo (_framework_) especializado en el análisis de volcados de memoria RAM, indispensable para extraer información sobre procesos en ejecución y conexiones de red en el momento del incidente.
    

## 7.2. Entornos Linux

- **The Sleuth Kit (TSK):** Una potente colección de herramientas en línea de comandos destinadas a investigar estructuras de sistemas de archivos y recuperar evidencias.
    
- **Autopsy:** Interfaz gráfica que facilita el uso estructurado de diferentes capacidades forenses y el análisis visual de las imágenes.
    
- **dd:** Comando tradicional empleado para realizar copias de bajo nivel. Su uso en un escenario de adquisición forense real exige una precaución extrema, ya que un error al designar los parámetros de origen y destino puede sobrescribir y destruir la prueba original de manera irreversible.