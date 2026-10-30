# 1. Introducción a la seguridad activa.
En la unidad anterior se establecieron los fundamentos de la seguridad informática, centrando la atención en la prevención y en la construcción de una infraestructura resistente. Todas estas estrategias conforman lo que se denomina **seguridad pasiva o preventiva**.

Sin embargo, la administración de sistemas moderna parte de una premisa ineludible en el ámbito corporativo: ninguna barrera es completamente infranqueable. Tarde o temprano, un atacante, un _software_ malicioso de nueva generación o un error humano lograrán sortear los controles preventivos. Es en este escenario cuando entra en juego la **seguridad activa**.

La seguridad activa se define como el conjunto de medidas, herramientas y procedimientos dinámicos diseñados para detectar, interceptar y neutralizar una amenaza que ya se encuentra en curso o que ha logrado vulnerar el perímetro de la red. Mientras que la seguridad preventiva es estática (espera a que el ataque choque contra ella), la seguridad activa requiere monitorización constante, capacidad de análisis y mecanismos de respuesta inmediata.

## 1.1. Diferencia entre medidas preventivas y medidas activas.
Para comprender plenamente este cambio de paradigma en la defensa de una infraestructura, es necesario contrastar ambos enfoques a través de sus características operativas:

- **Naturaleza de la defensa:** La seguridad pasiva actúa _antes_ del incidente (por ejemplo, al configurar un cortafuegos para bloquear por defecto todo el tráfico no autorizado). Por el contrario, la seguridad activa actúa _durante y después_ del incidente (por ejemplo, un sistema que detecta un escaneo de puertos masivo en tiempo real y reconfigura las reglas para bloquear automáticamente la dirección IP de origen).
    
- **Dependencia de la interacción:** Las medidas preventivas operan de forma desatendida y constante basándose en reglas predefinidas y arquitecturas fijas. Las medidas activas, en cambio, requieren frecuentemente la intervención directa de un administrador de sistemas para interpretar alertas, o la ejecución de protocolos automatizados de respuesta basados en el comportamiento del tráfico.
    
- **Herramientas implicadas:** La prevención utiliza Listas de Control de Acceso (ACL), algoritmos de cifrado de discos, Sistemas de Alimentación Ininterrumpida (SAI) y arquitecturas redundantes (RAID). La seguridad activa emplea _software_ antivirus y antimalware avanzado, Sistemas de Detección y Prevención de Intrusiones (IDS/IPS), analizadores de tráfico de red y entornos de ejecución controlados (_sandboxing_).

En un entorno empresarial real, ambos enfoques no son excluyentes, sino estrictamente complementarios. La seguridad pasiva reduce el volumen de ataques que llegan al interior de la red, permitiendo que las herramientas de seguridad activa se concentren únicamente en las amenazas más sofisticadas que logran infiltrarse.

## 1.2. El ciclo de la defensa activa.
La implantación de mecanismos de seguridad activa no consiste únicamente en instalar _software_ de detección, sino en articular un procedimiento metodológico y estructurado para hacer frente a las intrusiones. Cuando una amenaza sortea las defensas iniciales de la organización, el equipo de administración debe ejecutar un ciclo de respuesta ante incidentes.

Este ciclo de defensa técnica se divide en cuatro fases críticas y secuenciales:


| Fase                                        | Descripción                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| ------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| **1. Detección y Análisis**                 | Identificación. En esta etapa, los sistemas de seguridad activa monitorizan el tráfico, evalúan los registros del sistema (_logs_) y generan alertas. La labor del administrador es analizar estas anomalías para confirmar si un comportamiento inusual constituye un incidente real de seguridad o un falso positivo.                                                                                                                                                                                                                                                              |
| **2. Contención**                           | Una vez confirmado el ataque, la prioridad es frenar su propagación. La contención busca limitar el daño aislando los recursos comprometidos. Esto puede implicar desconectar temporalmente un servidor infectado de la red corporativa, inhabilitar cuentas de usuario vulneradas o bloquear rangos de red específicos.                                                                                                                                                                                                                                                             |
| **3. Erradicación**                         | Con la amenaza debidamente contenida, se procede a localizar y eliminar la causa raíz del incidente. Esta fase incluye la eliminación definitiva de cualquier código malicioso, la desinstalación de herramientas de acceso remoto (_backdoors_) dejadas por el atacante, el cierre de los servicios utilizados como vía de entrada y la aplicación de parches de actualización urgentes para corregir la vulnerabilidad explotada.                                                                                                                                                  |
| **4. Recuperación y Lecciones Aprendidas:** | La fase final consiste en devolver los sistemas afectados a su estado operativo normal y seguro, verificando mediante monitorización estricta que la amenaza ha desaparecido por completo. Esto puede requerir la restauración de bases de datos a partir de copias de seguridad íntegras o la reinstalación de máquinas desde imágenes de respaldo limpias. Tras la recuperación, se realiza una auditoría del incidente para identificar qué falló en la seguridad pasiva y activa, actualizando las políticas del sistema para evitar que un ataque similar vuelva a tener éxito. |

# 2. Anatomía del ataque informático y amenazas lógicas
Para que los mecanismos de seguridad activa resulten eficaces, el personal de administración de sistemas debe comprender en profundidad a qué se enfrenta. No es posible detectar, detener ni erradicar una amenaza si se desconoce su naturaleza o el comportamiento que sigue al infiltrarse en una red.

Las **amenazas lógicas** son aquellas que operan a nivel de _software_ e información. Su objetivo principal es alterar el funcionamiento del sistema operativo, robar datos confidenciales, destruir registros o tomar el control remoto de los equipos, explotando para ello vulnerabilidades técnicas o descuidos humanos.

## 2.1. Clasificación de las principales amenazas lógicas actuales.
El _software malicioso_ (conocido de forma general como _malware_) ha evolucionado enormemente. En un entorno corporativo, las tipologías más críticas que exigen monitorización activa son:

- **Ransomware (Secuestro de datos):** Es, en la actualidad, una de las amenazas lógicas más destructivas para las empresas. Este código malicioso se introduce en el sistema y cifra de forma masiva los archivos, bases de datos e incluso los discos duros completos, utilizando algoritmos criptográficos fuertes. Posteriormente, los atacantes exigen el pago de un rescate (habitualmente en criptomonedas) a cambio de entregar la clave de descifrado. Además del secuestro técnico, las variantes modernas suelen practicar la doble extorsión: amenazar con publicar información confidencial en internet si no se realiza el pago.

- **Gusanos (Worms):** A diferencia de los virus tradicionales, un gusano informático no necesita adherirse a un archivo legítimo ni requiere la intervención del usuario para propagarse. Su principal característica es la replicación autónoma a través de la red local. Una vez que infecta un equipo, escanea automáticamente otros servidores y dispositivos conectados en busca de vulnerabilidades para expandirse, lo que puede provocar el colapso de la infraestructura por saturación de tráfico.

- **Troyanos (Trojans):** Son programas maliciosos que se ocultan bajo la apariencia de _software_ legítimo y útil. Cuando el usuario ejecuta el programa engañado, el troyano abre una "puerta trasera" o _backdoor_ (Vía de acceso secreta que elude los mecanismos de autenticación habituales del sistema operativo). Esto permite a los atacantes acceder al equipo de forma remota, administrar el sistema operativo de forma silenciosa y ejecutar otras cargas maliciosas.
    
- **Fileless Malware (Malware sin archivo):** Representa un reto mayúsculo para los antivirus tradicionales. En lugar de instalar un archivo ejecutable malicioso en el disco duro (que sería fácilmente detectable por una firma estática), este _malware_ opera exclusivamente en la memoria RAM del equipo. Para lograr sus objetivos, "secuestra" herramientas administrativas nativas y legítimas del propio sistema operativo, como PowerShell en Windows, ejecutando comandos maliciosos sin dejar apenas rastro en el almacenamiento físico.
    
- **Spyware (Software espía):** Su finalidad es operar de forma invisible, recopilando información sobre las actividades del usuario, capturando pulsaciones de teclado (_keyloggers_), interceptando credenciales y enviando estos datos a servidores externos controlados por el atacante.

**Otras amenazas lógicas**

- **Bomba lógica (Logic bomb):** Código malicioso que se introduce en un sistema de forma oculta pero que permanece en estado latente e inactivo. La ejecución de su carga destructiva (como el borrado masivo de discos) solo se desencadena cuando se cumple una condición matemática o lógica predeterminada, como alcanzar una fecha concreta o que un usuario específico sea eliminado de la base de datos de empleados.

- **Adware (Software publicitario):** Programas cuyo objetivo es mostrar publicidad invasiva en el equipo. A menudo operan en los límites de la legalidad, recopilando hábitos de navegación para perfilar a los usuarios sin su consentimiento explícito y consumiendo recursos de red innecesarios.

- **Phishing:** Aunque se cataloga como una técnica de ingeniería social, actúa como el vector de entrada principal para muchas amenazas lógicas. Consiste en la falsificación de comunicaciones (correos electrónicos o sitios web) simulando ser una entidad legítima para inducir a la víctima a revelar credenciales críticas o descargar código malicioso.

- **Hoaxes (Bulos):** Mensajes con contenido falso y alarmista que instan al usuario a reenviarlos masivamente. Aunque no contienen código ejecutable malicioso, se consideran una amenaza organizativa porque generan alarmismo, desinformación y pueden llegar a saturar los canales de comunicación corporativos.

- **Jokes (Programas de broma):** _Software_ inofensivo desde el punto de vista de la integridad de los datos, diseñado para simular un comportamiento malicioso o generar molestias operativas (como abrir reiteradamente la bandeja del lector de discos o mostrar ventanas emergentes imposibles de cerrar), reduciendo la productividad del usuario afectado.

## 2.2. Las fases del ciberataque.
Un ataque informático exitoso contra una organización rara vez es un evento improvisado. Consiste en un proceso estructurado y secuencial. El modelo **Cyber Kill Chain** (Cadena de Muerte Cibernética), desarrollado originalmente por Lockheed Martin, describe las distintas fases que sigue un atacante.

Comprender esta secuencia es vital en la seguridad activa: si los sistemas de detección logran identificar e interceptar la amenaza en cualquiera de las fases tempranas, la cadena se rompe y el ataque fracasa antes de alcanzar su objetivo final.

1. **Reconocimiento (_Reconnaissance_):** Fase preparatoria. El atacante recopila información sobre el objetivo sin interactuar de forma agresiva. Se buscan direcciones IP públicas, nombres de empleados en redes sociales (para futuros ataques de _phishing_), proveedores tecnológicos y la estructura general de la red corporativa.
    
2. **Escaneo y Preparación (_Weaponization & Scanning_):** Utilizando herramientas de reconocimiento, se sondean los servidores de la empresa en busca de puertos abiertos, servicios activos y sistemas operativos desactualizados. Una vez identificada una vulnerabilidad concreta (por ejemplo, un servidor web sin parchear), se "fabrica" o prepara el código malicioso, diseñado específicamente para aprovecharla.
    
3. **Explotación y Entrega (_Exploitation & Delivery_):** Es el momento del impacto. El atacante lanza su código contra la vulnerabilidad encontrada o entrega el _malware_ engañando a un empleado mediante ingeniería social. Si la vulnerabilidad se explota con éxito, el código malicioso entra en ejecución dentro del sistema.
    
4. **Instalación y Mantenimiento del acceso (_Installation & Persistence_):** Una vez dentro, el atacante necesita asegurar su posición. Para evitar ser expulsado si el servidor se reinicia o el usuario cambia de contraseña, el _malware_ instala puertas traseras, crea cuentas de usuario con privilegios de administrador ocultas, o modifica tareas programadas del sistema operativo para que la amenaza se ejecute automáticamente de forma silenciosa.
    
5. **Acción sobre los objetivos y Borrado de huellas (_Actions on Objectives & Clearing Tracks_):** En la fase final, el intruso lleva a cabo su propósito real: robo masivo de bases de datos, cifrado de la infraestructura (_ransomware_) o destrucción de información. Para retrasar la detección y dificultar el trabajo del análisis forense, el atacante manipulará o eliminará los registros de eventos (_logs_) del sistema, borrando sus huellas.

>[!NOTE] Ejercicio 1
>Durante una fuga masiva de información se han reconstruido los siguientes eventos en orden NO cronológico:
>
>- El programa malicioso modificó el registro de Windows para asegurarse de que volvería a arrancar automáticamente cada vez que se encendiera el ordenador.
>- El código localizó la carpeta compartida con los datos de nóminas, comprimió los archivos, los envió a un servidor externo y procedió a eliminar el historial del Visor de eventos para ocultar la transferencia.
>- Un individuo anónimo rastreó los perfiles de LinkedIn de los empleados del departamento de Recursos Humanos de la organización.
>- Al intentar abrir el documento, el código oculto aprovechó una vulnerabilidad antigua no parcheada en el visor de PDF del equipo para ejecutarse en segundo plano.
>- El individuo envió un correo electrónico dirigido específicamente a la responsable de selección, suplantando la identidad de un candidato legítimo y adjuntando un falso currículum en formato ZIP.
>
>Relacionad cada suceso (A, B, C, D, E) con su fase correspondiente dentro de la _Cyber Kill Chain_
>  

# 3. Análisis de malware y entornos de ejecución controlados.
Una vez comprendida la anatomía de un ciberataque y las diferentes tipologías de _software_ malicioso, el siguiente paso en la estrategia de seguridad activa consiste en diseccionar estas amenazas. El análisis de _malware_ es el proceso de estudiar el comportamiento, el código y el impacto de un archivo sospechoso para descifrar su funcionamiento interno.

Esta práctica resulta fundamental en la administración de sistemas, ya que permite a los equipos de seguridad identificar cómo se comunica el programa malicioso, qué vulnerabilidades intenta explotar y qué tipo de daños provoca. Con esta información, es posible crear reglas de detección precisas y diseñar contramedidas efectivas antes de que el ataque logre consolidarse en la infraestructura corporativa. Sin embargo, este proceso conlleva riesgos operativos extremos si no se realiza bajo unas condiciones tecnológicas muy estrictas.

## 3.1. Peligros del análisis de _malware_ en equipos en producción.
En el ámbito empresarial, se denomina "equipo en producción" a cualquier ordenador, servidor, base de datos o dispositivo que forma parte activa del entorno de trabajo diario de la organización y que se encuentra conectado a la red corporativa.

Ejecutar, abrir o examinar _software_ malicioso directamente en este tipo de sistemas, incluso cuando se hace con fines de diagnóstico o estudio, constituye una negligencia crítica. Los peligros asociados a esta práctica imprudente incluyen:

- **Infección accidental y pérdida de datos local:** En el momento en el que el código malicioso entra en ejecución, iniciará inmediatamente sus rutinas ofensivas. Si se trata de un _ransomware_, comenzará a cifrar los documentos del usuario en milisegundos; si es un troyano, abrirá puertas traseras silenciosas. La destrucción de la información almacenada en ese equipo es el primer daño inminente.
    
- **Movimiento lateral y propagación masiva:** Las amenazas lógicas modernas (especialmente los gusanos informáticos) están diseñadas para no conformarse con su primera víctima. En cuanto se activan, rastrean de forma automatizada las direcciones IP de la subred para saltar del equipo inicial al resto de servidores de la empresa. Un análisis imprudente en un ordenador conectado puede desencadenar el colapso de toda la infraestructura organizativa en cuestión de minutos.
    
- **Exfiltración silenciosa de credenciales:** Durante la manipulación del archivo sospechoso, el _malware_ puede desplegar módulos espía encargados de capturar pulsaciones de teclado, rastrear contraseñas almacenadas en la memoria caché o monitorizar el tráfico de red. Esta información crítica se envía a los servidores externos del atacante (conocidos como servidores C2) comprometiendo las credenciales del propio administrador que está realizando el análisis.
    
- **Destrucción de evidencia digital:** En el contexto de una respuesta ante incidentes, interactuar libremente con el _malware_ en el equipo original donde fue detectado altera irremediablemente los registros del sistema (_logs_) y los archivos temporales. Esto compromete la cadena de custodia y destruye las pruebas originales, dificultando cualquier investigación forense posterior.
    

Por todas estas razones, el análisis de amenazas lógicas debe someterse a un protocolo de aislamiento absoluto. Ninguna prueba de seguridad activa debe llevarse a cabo si existe el más mínimo riesgo de que el código escape hacia la red principal de la organización.

## 3.2. Diseño de laboratorios seguros: _Sandboxing_ y aislamiento mediante máquinas virtuales.
Para solventar los riesgos inherentes al análisis de _software_ malicioso, la administración de sistemas recurre a la creación de entornos de ejecución controlados. El objetivo de estos entornos es permitir que el código destructivo se ejecute libremente para poder observar su comportamiento, pero garantizando matemáticamente que sus efectos no puedan escapar hacia el sistema físico (el _host_) ni hacia la red corporativa.

Para lograr este aislamiento, se emplean dos técnicas fundamentales:

**1. El concepto de Sandbox (Caja de arena)** Un _sandbox_ es un entorno de pruebas estrictamente restringido. Cuando un archivo sospechoso se ejecuta dentro de un _sandbox_, el sistema operativo le hace creer que tiene acceso total a los recursos del equipo, pero en realidad está operando dentro de una "burbuja" virtual. Todas las modificaciones que el _malware_ intenta realizar quedan confinadas en esa caja de arena. Al finalizar el análisis, la burbuja se destruye, eliminando cualquier rastro de la infección.

**2. Aislamiento mediante Máquinas Virtuales (VM)** Para el análisis manual y detallado, la herramienta por excelencia es la máquina virtual. Una VM permite emular un sistema operativo completo. Sin embargo, para que una máquina virtual sea considerada un entorno seguro de análisis de _malware_, debe configurarse aplicando medidas de contención sumamente estrictas:

- **Aislamiento de red:** La tarjeta de red virtual jamás debe configurarse en modo "Adaptador Puente" (_Bridged_), ya que esto colocaría a la máquina infectada en la misma red local que los equipos de producción. Se deben utilizar modos de red interna o adaptadores "Solo anfitrión" (_Host-Only_) para simular conectividad sin salida real al exterior.
    
- **Desactivación de integraciones:** Es imprescindible deshabilitar cualquier puente de comunicación entre la máquina física y la máquina virtual. Esto incluye, por ejemplo, desactivar las carpetas compartidas, el portapapeles bidireccional y la función de arrastrar y soltar archivos. Ya que ciertos tipos de _malware_ avanzado están diseñados para utilizar estas vías como ruta de escape.
    
- **Uso de Instantáneas (_Snapshots_):** Antes de introducir y ejecutar el código malicioso, el administrador debe tomar una "instantánea" del estado limpio de la máquina virtual. Una vez concluido el análisis y extraída la información necesaria, se restaura la instantánea, lo que devuelve el sistema a su estado inmaculado de forma inmediata, erradicando por completo la infección sin necesidad de reinstalar el sistema operativo.
## 3.3. Aplicaciones de detección de amenazas y creación de reglas de identificación.
Una vez que la amenaza ha sido ejecutada, contenida y analizada en el laboratorio seguro, el equipo de seguridad se enfrenta al siguiente reto: ¿cómo enseñamos al resto de la infraestructura de la empresa a detectar y bloquear este _software_ en el futuro?

Como se vio en la unidad anterior, calcular la huella digital (el _hash_ SHA-256) de un archivo malicioso es útil, pero tiene una limitación crítica: si el creador del virus modifica un solo bit del código, el archivo generará un _hash_ completamente distinto y los sistemas de seguridad dejarán de detectarlo. Para solucionar este problema, se utilizan herramientas de identificación basadas en patrones.

**El estándar de la industria: YARA** 
YARA es una herramienta de código abierto descrita frecuentemente como "la navaja suiza de los investigadores de _malware_". En lugar de depender de un _hash_ estático y exacto, YARA permite a los administradores crear reglas personalizadas que buscan patrones de texto o secuencias binarias específicas dentro de los archivos sospechosos.

Si durante el análisis en el laboratorio descubrimos que un _ransomware_ siempre utiliza una dirección IP concreta para comunicarse con su servidor de control, o que siempre inserta una cadena de texto específica exigiendo el pago en Bitcoin, podemos escribir una regla YARA que identifique esas pistas.

**Estructura de una regla YARA** Para resultar comprensibles y efectivas, estas reglas se redactan utilizando una sintaxis lógica muy clara, dividida habitualmente en tres secciones:

1. **Meta (Metadatos):** Contiene información descriptiva para el administrador. Aquí se incluye el autor de la regla, la fecha de creación y una breve descripción de la amenaza que intenta localizar.
    
2. **Strings (Cadenas):** Es el núcleo de la detección. En esta sección se definen las pistas que se van a buscar dentro de los archivos. Pueden ser cadenas de texto legibles (por ejemplo, el nombre del archivo de rescate `readme.txt`) o secuencias de bytes hexadecimales características de ese virus en particular.
    
3. **Condition (Condición):** Establece la regla lógica matemática que determina si el archivo analizado está infectado o no. Por ejemplo, se puede configurar una condición que dicte: _"Si el archivo contiene la cadena A y al menos una de las cadenas B o C, clasifícalo como este Ransomware específico"_.

Gracias al uso de estas reglas, los Sistemas de Detección de Intrusiones (IDS) y los motores antimalware corporativos pueden escudriñar todo el tráfico y los archivos de la empresa, identificando e interceptando la amenaza incluso si el atacante ha intentado camuflarla modificando su _hash_ original.

### 3.4. Prueba de concepto de un ataque lógico

**Fase 1: Preparación del escenario**

1. Se creará una carpeta llamada `Simulacion_Aula`.
2. Dentro de esta carpeta, se creará un archivo de texto llamado `nomina_direccion.txt` con un contenido legible (por ejemplo: _"Salario base: 0,5 bitcoins. Bonificaciones: 0.01 bitcoins"_).
3. Se abrirá dicho archivo para comprobar que el sistema operativo lo lee sin problemas mediante el bloc de notas.

**Fase 2: El bloqueo de la seguridad base (Execution Policy)**

1. Abrir una consola de PowerShell estándar (sin privilegios de administrador iniciales).
2. Introduciendo el comando `Get-ExecutionPolicy`, comprobar el modo de ejecución del sistema. Abrir PowerShell como Administrador permite utilizar el comando `Set-ExecutionPolicy [modo]` para modificar dicho modo de ejecución entre:
	1. **Restricted:** Permite ejecutar comandos individuales, pero bloquea por completo la ejecución de cualquier _script_ (`.ps1`).
	2. **RemoteSigned:** Permite ejecutar _scripts_ creados localmente en el equipo. Sin embargo, exige que los _scripts_ descargados de internet estén firmados digitalmente por un editor de confianza.
	3. **Unrestricted:** Elimina las restricciones principales, permitiendo la ejecución de cualquier _script_. Si el archivo procede de internet, el sistema mostrará una advertencia solicitando confirmación antes de ejecutarlo.
	4. **Bypass:** Ignora todas las advertencias y reglas de seguridad, ejecutando cualquier código sin solicitar confirmación al usuario. Es el nivel de mayor riesgo y el utilizado frecuentemente para evitar bloqueos durante los despliegues automatizados.


**Fase 3: Simulación del cifrado** 

1. Crear un _script_ `ataque.ps1` que simule el comportamiento de un _ransomware_, utilizando comandos nativos para convertir el texto legible en una cadena de caracteres ofuscada.:

```PowerShell
# Lee el contenido íntegro del archivo original y lo almacena temporalmente en la variable $contenido.
$contenido = Get-Content .\nomina_direccion.txt

# Convierte el texto legible de la variable a una matriz de bytes utilizando codificación UTF-8. 
$bytes = [System.Text.Encoding]::UTF8.GetBytes($contenido)

# Transforma la matriz de bytes en una cadena de texto ofuscada utilizando el formato Base64. 
$cifrado = [Convert]::ToBase64String($bytes)

# Crea un nuevo archivo en el sistema añadiendo la extensión '.locked' a su nombre, y escribe en su interior la cadena de texto ofuscada.
Set-Content .\nomina_direccion.txt.locked -Value $cifrado

# Elimina el archivo original de la carpeta.
Remove-Item .\nomina_direccion.txt

# Imprime un mensaje por la pantalla de la consola.
Write-Host "Tus archivos han sido secuestrados. Se requiere clave para revertir el proceso."
```

2. Al ejecutar de nuevo `./ataque.ps1`, el archivo original desaparecerá visualmente del escritorio y será sustituido por `nomina_direccion.txt.locked`.
3. Al intentar abrir el nuevo archivo con el bloc de notas, el alumnado observará que el texto original ha sido destruido y reemplazado por una cadena de caracteres incomprensible.

# 4. Fortificación del sistema (_Hardening_)

La fortificación consiste en el proceso sistemático de asegurar un sistema operativo, aplicación o infraestructura de red mediante la reducción de su exposición a posibles ciberataques. Se trata de una medida de seguridad activa, pero a la vez preventiva. Se asume que el sistema base, en su configuración por defecto, es vulnerable. El objetivo principal es establecer múltiples capas defensivas siguiendo el principio de **Defensa en Profundidad**, garantizando que si un atacante logra evadir un control de seguridad perimetral de los estudiados en la unidad 1, se enfrente inmediatamente a restricciones internas.

## 4.1. Defensa en Profundidad y Superficie de Ataque

Para comprender las técnicas de fortificación, es necesario definir previamente el concepto de **superficie de ataque**. Este término engloba la suma de todos los puntos de entrada, interfaces, servicios, protocolos y vulnerabilidades conocidas que un usuario malintencionado podría utilizar para introducir o extraer datos de un entorno corporativo.

A mayor cantidad de _software_ instalado, puertos abiertos y servicios ejecutándose en segundo plano, mayor es la superficie de ataque. Por tanto, el primer pilar del _hardening_ es el **Principio de Mínima Funcionalidad**. Este principio establece que un sistema corporativo debe configurarse para ejecutar única y exclusivamente las aplicaciones y servicios estrictamente necesarios para su propósito organizacional, deshabilitando cualquier elemento adicional.

### Auditoría y desactivación de servicios

Los sistemas operativos modernos, diseñados para abarcar desde entornos domésticos hasta servidores empresariales, inician por defecto multitud de servicios (como la compartición de recursos multimedia, servicios de geolocalización o colas de impresión remota) que resultan innecesarios en un equipo de trabajo asilado o en un servidor de bases de datos.

Desde la perspectiva de la administración de sistemas, la auditoría y paralización de estos servicios reduce drásticamente los vectores de ataque. Este procedimiento se puede automatizar mediante la consola de comandos.

```PowerShell
# 1. Auditoría: Obtener un listado de todos los servicios del sistema que se encuentran actualmente en ejecución.
Get-Service | Where-Object {$_.Status -eq "Running"}

# 2. Intervención: Detener forzosamente un servicio específico que representa un riesgo innecesario (Ejemplo: Administrador de autenticación de Xbox Live).
Stop-Service -Name XblAuthManager -Force

# 3. Fortificación: Modificar el tipo de inicio del servicio para garantizar que no vuelva a ejecutarse tras el reinicio del sistema operativo.
Set-Service -Name XblAuthManager -StartupType Disabled
```

## 4.2. Gestión de Parches y Vulnerabilidades

Reducir la superficie de ataque desinstalando el software innecesario es vital, pero las aplicaciones y servicios que sí son obligatorios para el funcionamiento de la empresa pueden contener inherentemente defectos de programación.

Los fabricantes de software publican actualizaciones periódicas para corregir estas brechas, pero en infraestructuras con cientos de equipos, la actualización manual no es viable. La administración de sistemas debe implementar un **ciclo de vida de gestión de parches** estructurado:

1. **Monitorización y Detección:** Seguimiento continuo de los boletines de seguridad emitidos por los fabricantes y de la base de datos CVE (_Common Vulnerabilities and Exposures_), que estandariza la nomenclatura de las vulnerabilidades descubiertas a nivel mundial.
    
2. **Evaluación de Impacto:** Análisis técnico para determinar si los sistemas de la infraestructura local utilizan el software afectado y si la vulnerabilidad es explotable en el entorno específico de la organización.
    
3. **Prueba en Entorno de Desarrollo (Sandbox):** Todo parche de seguridad, especialmente aquellos calificados como críticos o de Día Cero (_Zero-Day_), debe instalarse primero en un entorno de pruebas aislado. La instalación directa en producción puede provocar incompatibilidades con el software corporativo (como aplicaciones heredadas o bases de datos) y paralizar la actividad de la empresa.
    
4. **Despliegue Controlado:** Distribución automatizada del parche a los equipos de producción mediante herramientas de administración centralizada (como WSUS en entornos Windows) o mediante el uso de _scripts_ de automatización.

### Automatización mediante PowerShell

Para equipos individuales o entornos que no disponen de un servidor WSUS, los administradores utilizan módulos específicos de PowerShell que permiten auditar e instalar actualizaciones del sistema operativo de forma desatendida.


```PowerShell
# Instalación del módulo oficial de gestión de actualizaciones.
Install-Module -Name PSWindowsUpdate -Force

# Auditoría: Consulta a los servidores del fabricante para obtener un listado de todos los parches de seguridad pendientes de instalación en el equipo.
Get-WindowsUpdate

# Despliegue: Descarga, instalación de todas las actualizaciones críticas pendientes y reinicio automático del sistema operativo en caso de ser requerido por el parche.
Install-WindowsUpdate -AcceptAll -AutoReboot
```

>[!NOTE] Ejercicio 2.
>Al realizar una auditoría de seguridad preventiva, se documenta la siguiente configuración actual del servidor:
>
>-El Firewall de Windows se encuentra desactivado "para evitar problemas de conectividad con la base de datos".
>-El servicio de Cola de impresión se encuentra en ejecución.
>-El administrador ha instalado un lector de archivos PDF y un navegador web de terceros en el servidor para poder leer manuales de configuración localmente.
>-El puerto 3389 (Escritorio Remoto - RDP) está abierto y enrutado hacia Internet para que los técnicos puedan administrar el servidor desde sus domicilios sin necesidad de utilizar una VPN.
>
>A partir del escenario descrito, responded de forma técnica y justificada a las siguientes cuestiones:
>
>**1. Análisis de la Superficie de Ataque:** Identificad qué elementos de la configuración documentada violan el Principio de Mínima Funcionalidad. Explicad qué comando o acción técnica aplicaríais para remediarlo.
>**2. Diseño de Defensa en Profundidad:** Suponiendo que un atacante logra robar las credenciales de Escritorio Remoto de uno de los técnicos y consigue entrar al sistema, ¿qué otras barreras debería haber implementado el departamento de sistemas para impedir que el atacante logre extraer, modificar o cifrar la base de datos de Recursos Humanos?

## 4.3. Control de Acceso y Restricción de Ejecución

Existe un vector de entrada que resulta imposible de eliminar mediante actualizaciones de _software_: la interacción del usuario. Si un empleado ejecuta inadvertidamente un archivo malicioso, la amenaza operará bajo el contexto de seguridad y los permisos de dicho usuario.

Para mitigar este riesgo, la administración de sistemas aplica mecanismos de restricción de ejecución fundamentados en el **Principio de Mínimo Privilegio**. Este principio establece que un usuario o proceso debe poseer únicamente los permisos de acceso estrictamente necesarios para desempeñar sus funciones legítimas, y nada más.

### Control de Cuentas de Usuario (UAC) y Gestión de Privilegios

En los sistemas modernos, la separación de privilegios se gestiona estructuralmente mediante el Control de Cuentas de Usuario (UAC). Cuando un usuario con credenciales administrativas inicia sesión, el sistema le asigna dos _tokens_ de acceso: uno estándar y otro con privilegios elevados. Por defecto, todas las aplicaciones (incluido el _malware_) se ejecutan utilizando el _token_ estándar. Si un proceso requiere realizar cambios a nivel de sistema (como modificar el registro o instalar un servicio), UAC intercepta la solicitud y exige una confirmación explícita, bloqueando la elevación de privilegios silenciosa característica de los troyanos y _scripts_ maliciosos.

A nivel organizativo, la fortificación exige auditar rigurosamente la pertenencia a los grupos administrativos locales, garantizando que los usuarios de producción operen siempre con cuentas estándar.

```PowerShell
# Auditoría: Comprobar qué usuarios pertenecen actualmente al grupo de Administradores locales del equipo.
Get-LocalGroupMember -Group "Administradores"

# Fortificación: Retirar a un usuario comprometido o innecesario del grupo de Administradores.
Remove-LocalGroupMember -Group "Administradores" -Member "UsuarioEstandar"
```

### Restricción de Software mediante AppLocker

Las amenazas lógicas de rápida propagación, como el _ransomware_ moderno o el _fileless malware_, suelen descargarse y ejecutarse desde directorios sobre los que el usuario estándar tiene permisos de escritura.

Para evitar esto, se despliegan políticas de restricción de _software_, siendo **AppLocker** la herramienta nativa más potente en entornos corporativos Windows. AppLocker permite al administrador crear reglas de denegación o permiso (listas blancas y listas negras) basadas en tres condiciones:

1. **Ruta:** Permite o bloquea la ejecución de archivos ubicados en carpetas específicas (ej. bloquear cualquier ejecutable en `%AppData%`).
    
2. **Editor:** Permite la ejecución únicamente si el programa está firmado digitalmente por un fabricante de confianza (ej. permitir solo aplicaciones firmadas por Microsoft o Adobe).
    
3. **Hash de archivo:** Bloquea la ejecución de un archivo concreto basándose en su huella criptográfica SHA-256, independientemente de cómo se llame o dónde esté ubicado.

## 4.4. Auditoría de Sistemas y Trazabilidad

La implementación de medidas de prevención y fortificación reduce drásticamente la probabilidad de una brecha de seguridad, pero la administración de sistemas asume que la invulnerabilidad total no existe. Cuando un ataque logra materializarse, o incluso cuando fracasa en el intento, el equipo de respuesta ante incidentes necesita disponer de un registro inmutable e histórico de todo lo ocurrido en la infraestructura. Este proceso se denomina **trazabilidad**.

La auditoría de sistemas consiste en la configuración del sistema operativo para que registre de forma automática y silenciosa ciertos eventos críticos relacionados con la seguridad. En plataformas Windows, esta información se almacena y consulta a través del **Visor de Eventos** (_Event Viewer_), específicamente en el registro de _Seguridad_.

### Configuración de Directivas de Auditoría

Por defecto, los sistemas operativos no registran todas las acciones para evitar la saturación de los discos duros y el impacto en el rendimiento. En un proceso de _hardening_, el administrador debe habilitar mediante Directivas de Grupo (GPO) o políticas locales (`secpol.msc`) qué categorías de eventos deben ser auditadas. Las más críticas en seguridad activa son:

- **Auditoría de sucesos de inicio de sesión:** Registra tanto los intentos exitosos (que pueden indicar un acceso en horarios anómalos o desde IPs externas) como los intentos fallidos (que delatan ataques de fuerza bruta contra las credenciales).
    
- **Auditoría de acceso a objetos:** Si se configura previamente el control de acceso en una carpeta crítica (por ejemplo, el directorio de nóminas o bases de datos), el sistema registrará qué usuario, desde qué proceso y en qué momento exacto intentó leer, modificar o eliminar un archivo. Esta es la principal contramedida forense para rastrear la actividad de un _ransomware_.
    

### Extracción y Análisis de Registros

La revisión manual de miles de eventos en la interfaz gráfica del Visor de Eventos resulta ineficiente. En seguridad activa, se recurre a la consola de comandos para filtrar registros basándose en su ID de evento (EventID). Por ejemplo, en Windows, el **EventID 4625** representa un error de inicio de sesión.

```PowerShell
# Búsqueda Forense: Extraer los últimos 20 intentos fallidos de inicio de sesión en el equipo.
# Esto permite detectar si se está produciendo un ataque de fuerza bruta en progreso.
Get-WinEvent -FilterHashtable @{LogName='Security'; Id=4625} -MaxEvents 20 | Format-List TimeCreated, Message

# Análisis de acceso a objetos: Buscar eventos de archivos modificados o eliminados (requiere auditoría de objetos habilitada, EventID 4663).
Get-WinEvent -FilterHashtable @{LogName='Security'; Id=4663} -MaxEvents 10
```

# 5. Seguridad en redes y comunicaciones

Hasta este punto, las estrategias de fortificación se han centrado en el _endpoint_, es decir, en asegurar el propio sistema operativo y sus servicios internos. Sin embargo, en una infraestructura corporativa, los equipos no operan de forma aislada. Intercambian información, consultan bases de datos y se sincronizan a través de la red local (LAN) y redes externas (WAN/Internet).

## 5.1. Segmentación y Aislamiento de Redes

El error de diseño más común y peligroso en infraestructuras de red tradicionales es la **red plana**, donde todos los dispositivos (servidores, ordenadores de empleados, impresoras e incluso dispositivos IoT) comparten el mismo segmento de red y tienen visibilidad directa entre sí. En este escenario, la propagación de un gusano informático o un _ransomware_ es inmediata.

La seguridad activa requiere diseñar la red asumiendo la brecha. Se aplican técnicas de segmentación lógica mediante **VLANs** (Redes de Área Local Virtuales) para aislar los departamentos. De este modo, si la VLAN de "Administración" sufre una infección, el tráfico no puede saltar automáticamente a la VLAN de "Servidores de Producción" sin pasar por un enrutador o un cortafuegos interno que audite y bloquee esa anomalía.

Para los servicios que deben ser accesibles desde el exterior (como un servidor web o de correo), se implementa una **DMZ** (Zona Desmilitarizada). La DMZ actúa como una subred aislada expuesta a Internet, pero con restricciones severas de acceso hacia la red corporativa interna.

## 5.2. Criptografía y Protección de Datos en Tránsito

Incluso segmentando la red, el tráfico que fluye entre dos puntos legítimos puede ser interceptado por un atacante interno mediante técnicas de _sniffing_ o ataques _Man-in-the-Middle_ (MitM). Para proteger la información en tránsito, la administración de sistemas recurre a la criptografía.

El objetivo de la criptografía moderna no es solo ocultar la información, sino garantizar tres pilares: **Confidencialidad** (solo el receptor legítimo puede leerlo), **Integridad** (el mensaje no ha sido modificado) y **No repudio** (el emisor no puede negar haberlo enviado).

### Cifrado Simétrico y Asimétrico

- **Cifrado Simétrico:** Utiliza una única clave compartida tanto para cifrar como para descifrar la información (ej. AES). Es extremadamente rápido y eficiente para cifrar grandes volúmenes de datos (como un disco duro completo o un túnel VPN), pero presenta el problema de cómo distribuir esa clave secreta de forma segura a través de una red insegura.
    
- **Cifrado Asimétrico:** Utiliza un par de claves matemáticamente vinculadas: una **Clave Pública** (que se distribuye libremente a cualquiera) y una **Clave Privada** (que el propietario custodia en secreto y nunca comparte). Lo que se cifra con la clave pública de un usuario, **solo** puede descifrarse con su clave privada correspondiente. Este modelo (ej. RSA) soluciona el problema de distribución, pero requiere una alta capacidad de procesamiento computacional.

Los protocolos seguros modernos, como **TLS/SSL** (utilizados en HTTPS o FTPS), combinan ambos modelos: utilizan el cifrado asimétrico inicialmente para intercambiar de forma segura una clave temporal, y luego cambian al cifrado simétrico para transmitir el grueso de los datos a alta velocidad.

### Firmas y Certificados Digitales

Mientras que el cifrado protege la confidencialidad, la **firma digital** garantiza la autenticidad y la integridad. Funciona invirtiendo el uso de las claves asimétricas:

1. El emisor calcula el _hash_ (huella digital) del documento o mensaje.
    
2. El emisor cifra ese _hash_ utilizando su propia **Clave Privada**. Ese _hash_ cifrado es la firma digital.
    
3. El receptor descifra la firma utilizando la **Clave Pública** del emisor (confirmando así su identidad) y recalcula el _hash_ del documento. Si ambos _hashes_ coinciden, el archivo es íntegro y auténtico.

Para evitar que un atacante suplante una clave pública (haciéndose pasar por el servidor legítimo), intervienen las **Autoridades de Certificación (CA)** dentro de una infraestructura PKI (_Public Key Infrastructure_). La CA actúa como un notario digital, emitiendo un **Certificado Digital** que vincula criptográficamente una clave pública con la identidad verificada de una empresa o servidor.

### Gestión de Certificados en PowerShell

En la administración de servidores, es habitual tener que generar certificados autofirmados para asegurar comunicaciones internas de prueba antes de adquirir un certificado comercial emitido por una CA.

```PowerShell
# Crear un certificado digital autofirmado para proteger el tráfico de un servidor web interno.
# El certificado se almacena en el repositorio personal del equipo (Cert:\LocalMachine\My).
$cert = New-SelfSignedCertificate -DnsName "intranet.centroeducativo.local" -CertStoreLocation "Cert:\LocalMachine\My"

# Visualizar la huella digital (Thumbprint) y los detalles del certificado recién creado.
Get-ChildItem -Path "Cert:\LocalMachine\My" | Where-Object {$_.Subject -match "intranet"} | Format-List Subject, Thumbprint, NotAfter

# Exportar la Clave Pública (sin la clave privada) a un archivo .cer para distribuirla a los clientes de la red.
Export-Certificate -Cert $cert -FilePath "C:\Admin\Intranet_Publica.cer"
```

## 5.3. Monitorización y Control de Flujo (Firewalls Avanzados)

La última capa de la seguridad en redes es la inspección activa de los paquetes. Los cortafuegos tradicionales se limitaban a bloquear puertos estáticos (por ejemplo, cerrar el puerto 23 de Telnet). En el contexto de la seguridad activa, esto es insuficiente, ya que las amenazas modernas encapsulan su tráfico malicioso dentro de puertos legítimos y permitidos (como el puerto 443 de HTTPS).

Para combatir esto, se despliegan **Firewalls de Nueva Generación (NGFW)**, que implementan tecnologías de inspección profunda de paquetes (DPI) y análisis de estado (_Stateful Inspection_). Estos dispositivos no solo miran a qué puerto va dirigido el tráfico, sino que analizan la aplicación real que lo está generando, permitiendo a la administración de sistemas crear reglas de control mucho más granulares y dinámicas, preparando el terreno para la automatización y el uso de sistemas de detección de intrusos.

> [!NOTE] **Reflexión** 
> Si todo el tráfico web de nuestra organización viaja cifrado de extremo a extremo mediante HTTPS (gracias a los certificados digitales), ¿cómo puede el cortafuegos de la empresa inspeccionar ese tráfico en busca de _malware_ sin romper la confidencialidad?
