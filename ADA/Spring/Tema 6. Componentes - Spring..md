--- title: "Spring" ---
## 1. Spring

Spring es un framework para el desarrollo de aplicaciones y contenedor de inversión de control, de código abierto para la plataforma Java.

Si bien las características fundamentales de Spring Framework pueden ser usadas en cualquier aplicación desarrollada en Java, existen variadas extensiones para la construcción de aplicaciones web sobre la plataforma Java EE. A pesar de que no impone ningún modelo de programación en particular, este framework se ha vuelto popular en la comunidad al ser considerado una alternativa, sustituto, e incluso un complemento al modelo EJB (Enterprise JavaBean).

#### Módulos
Spring Framework comprende diversos módulos que proveen un rango de servicios:

- Contenedor de inversión de control: permite la configuración de los componentes de aplicación y la administración del ciclo de vida de los objetos Java, se lleva a cabo principalmente a través de la inyección de dependencias.
- Programación orientada a aspectos: habilita la implementación de rutinas transversales.
- Acceso a datos: se trabaja con RDBMS en la plataforma java, usando Java Database Connectivity y herramientas de Mapeo objeto relacional con bases de datos NoSQL.
- Gestión de transacciones: unifica distintas APIs de gestión y coordina las transacciones para los objetos Java.
- Modelo vista controlador: Un framework basado en HTTP y servlets, que provee herramientas para la extensión y personalización de aplicaciones web y servicios web REST.
- Framework de acceso remoto: Permite la importación y exportación estilo RPC, de objetos Java a través de redes que soporten RMI, CORBA y protocolos basados en HTTP incluyendo servicios web (SOAP).
- Convención sobre Configuración: el módulo Spring Roo ofrece una solución rápida para el desarrollo de aplicaciones basadas en Spring Framework, privilegiando la simplicidad sin perder flexibilidad.
- Procesamiento por lotes: un framework para procesamiento de mucho volumen que como características incluye funciones de registro/trazado, manejo de transacciones, estadísticas de procesamiento de tareas, reinicio de tareas, y manejo de recursos.
- Autenticación y Autorización: procesos de seguridad configurables que soportan un rango de estándares, protocolos, herramientas y prácticas a través del subproyecto Spring Security (antiguamente Acegi).
- Administración Remota: Configuración de visibilidad y gestión de objetos Java para la configuración local o remota vía JMX.
- Mensajes: Registro configurable de objetos receptores de mensajes, para el consumo transparente a través de JMS, una mejora del envío de mensajes sobre las API JMS estándar.
- Testing: Soporte de clases para desarrollo de unidades de prueba e integración.

#### Contenedores de inversión de control

El corazón de Spring Framework es su contenedor de inversión de control (IoC). Su trabajo es instanciar, inicializar y conectar objetos de la aplicación, además de proveer una serie de características adicionales disponibles en Spring a través del tiempo de vida de los objetos.

Los objetos creados y gestionados por el contenedor se denominan objetos gestionados o beans. Estos objetos son del tipo POJO. Para realizar su tarea el contenedor necesita información indicando cómo instanciar y conectar entre sí los beans. A esta información se la llama metadatos de configuración. Hay distintas formas de proporcionar esta información: basándose en XML, basándose en anotaciones o basándose en objetos Java (desde Spring 3.0). El contenedor es independiente del formato de los metadatos de configuración. El usuario puede usar el formato que desee e incluso mezclarlos en la misma aplicación.

Los objetos pueden ser obtenidos por búsqueda de dependencias o por inyección de dependencias. Búsqueda de dependencias es un modelo donde se pide al objeto contenedor un objeto con un nombre específico o de un tipo específico. Inyección de dependencias es un modelo en el que el contenedor pasa objetos por nombre a otros objetos, ya sea a través de métodos constructores, propiedades, o métodos de la fábrica.

En muchos casos cuando se utilizan otras partes del Spring Framework no necesita utilizar el Contenedor, aunque probablemente su uso le permita hacer una aplicación más fácil de configurar y personalizar. El Contenedor de Spring le proporciona un mecanismo consistente para configurar las aplicaciones, y se integra con casi todos los entornos Java, desde aplicaciones de pequeñas a grandes aplicaciones empresariales.

El contenedor se puede convertir en un contenedor EJB 3.0 parcialmente por medio del proyecto Pitchfork. Algunos critican al Spring Framework por no cumplir los estándares. Sin embargo, SpringSource no ve el cumplimiento EJB 3 como un objetivo importante, y afirma que el Spring Framework y el contenedor permiten modelos de programación más potentes. No creas un objeto, sino que describes la forma en que deben crearse, definiéndolo en el archivo de configuración de Spring. No llamas a los servicios y componentes, sino que dices qué servicios y componentes deben ser llamados, definiéndolos en los archivos de configuración de Spring. Esto hace el código fácil de mantener y más fácil de probar mediante la Inyección de Dependencia (IoC).

#### Spring Boot

**Spring Boot** es una de las tecnologías dentro del mundo de Spring de las que más se está hablando últimamente. ¿Qué es y cómo funciona Spring Boot? Para entender el concepto primero debemos reflexionar sobre cómo construimos aplicaciones con Spring Framework.
![[Pasted image 20260130121820.png]]

**Fundamentalmente existen tres pasos a realizar**. El primero es crear un proyecto Maven y descargar las dependencias necesarias. En segundo lugar desarrollamos la aplicación y en tercer lugar la desplegamos en un servidor. Si nos ponemos a pensar un poco a detalle en el tema, **únicamente el paso dos es una tarea de desarrollo**. Los otros pasos están más orientados a infraestructura.

SpringBoot nace con la intención de simplificar los pasos 1 y 3 y que nos podamos centrar en el desarrollo de nuestra aplicación.

## 2. Creación de nuestro primer proyecto Spring

Aunque existe un IDE específico basado en eclipse que facilita la creación de proyectos de este framework (Spring Tool Suite)... En lugar de depender de asistentes específicos de un IDE, utilizaremos la herramienta oficial **Spring Initializr**. Esto garantiza que el proyecto estructura correctamente las carpetas y dependencias compatibles, independientemente del IDE utilizado.

##### Generar el esqueleto

Accede a la web **[https://start.spring.io/](https://start.spring.io/)** y configura el proyecto con las siguientes opciones:

- **Project:** Maven
- **Language:** Java
- **Spring Boot:** La última versión estable disponible (que **NO** ponga SNAPSHOT ni M1).
- **Project Metadata:**
    - Group: `com.accesoadatos`
    - Artifact: `biblioteca-api` (o el nombre del proyecto)
    - Packaging: Jar
    - Java: **17** (o 21, según lo que tengáis instalado).

##### Las Dependencias

A la derecha, en el botón **"ADD DEPENDENCIES"**, debéis buscar y añadir estas 4 fundamentales para nuestro curso:

1. **Spring Web:** Incluye todo lo necesario para crear la API REST y el servidor Tomcat embebido.
2. **Spring Data JPA:** Incluye Hibernate y la capa de repositorios automáticos.
3. **PostgreSQL Driver**: El conector para la base de datos.
4. **Lombok** (Opcional pero recomendado): Para ahorrarnos Getters/Setters/Constructores con anotaciones.

##### Importar en IntelliJ IDEA

1. Pulsa el botón **GENERATE** (abajo). Se descargará un archivo `.zip`.
2. Descomprime el ZIP en tu carpeta de proyectos.
3. Abre tu IntelliJ (o tu IDE de confianza).
4. Selecciona **File > Open** (o "Open" en la pantalla de bienvenida).
5. Navega hasta la carpeta descomprimida y selecciona el archivo **`pom.xml`** (o la carpeta raíz).
6. Pulsa "Open" y luego **"Open as Project"**.
7. Espera a que Maven descargue todas las dependencias (puede tardar un poco la primera vez).


Lo primero si vemos la estructura del proyecto, veremos que hay ya un archivo java que nos ha creado el asistente por defecto y que será nuestra entrada a la aplicación, además esta clase ya indica con la etiqueta que precede a la clase que es de tipo SpringBoot para que permita configuraciones automáticas.

```Java
@SpringBootApplication
public class SpringApiApplication {
	public static void main(String[] args) {
		SpringApplication.run(SpringApiApplication.class, args);
	}
}
```

  
También tendrá especial importancia el archivo application.properties que será donde configuraremos aspectos relativos con nuestra aplicación, tales como las conexiones a base de datos o el puerto por donde acceder a nuestra aplicación por ejemplo. También podemos ver todas las dependencias que se han añadido gracias al archivo pom.xml.

Por el momento, como hemos creado nuestro nuevo proyecto indicando que usamos las dependencias de JPA y de Postgress, deberemos indicar la conexión a nuestra base de datos para el proyecto, antes de ser capaces de arrancar siquiera.

Crearemos una nueva base de datos en PgAdmin llamada Spring-Database:

![[Pasted image 20260205190214.png]]

Y añadimos las correspondientes lineas para la conexión a esta nueva base de datos en el application.properties:

```txt
spring.application.name=spring-api
spring.datasource.driver-class-name=org.postgresql.Driver
spring.datasource.url=jdbc:postgresql://localhost:5432/Spring-Database
spring.datasource.username=postgres
spring.datasource.password=1234
spring.jpa.database-platform=org.hibernate.dialect.PostgreSQLDialect
server.port=8080
```

## 3. Creando el controlador y la vista

Primero crearemos un package dentro de la carpeta de fuentes para almacenar los controladores y dentro de ella una clase cuyo nombre acabe en Controller (esto es obligatorio).

![[Pasted image 20260205115107.png]]


> [!NOTE] **IMPORTANTE: Dónde crear las clases.** La clase principal (`@SpringBootApplication`) busca automáticamente nuestros componentes, pero solo **hacia abajo**.
> 
> **Correcto**:
> Main: com.ejemplo.biblioteca.App
> Controller: com.ejemplo.biblioteca.controladores.SaludoController
> 
> **Incorrecto** (No funcionará):
> Main: com.ejemplo.biblioteca.App
> Controller: com.ejemplo.otros.SaludoController (Spring no mirará aquí).


Un controlador nos va a servir para manejar las peticiones de los usuarios. Es donde tendremos los manejadores, por ejemplo para manejar un formulario, para cargar datos, consultas a la base de datos, guardar, eliminar… Esos datos, posteriormente se mostrarán en una vista.

Para ello, indicaremos que es un controlador con la etiqueta @RestController y añadiremos la librería requerida:


>[!NOTE] Nota:
> La notación @Controller se usaba anteriormente en Spring MVC clásico para formalizar las vistas en HTML. Sin embargo, para este curso usaremos la notación más moderna @RestController, que define un controlador tipo REST autónomo y devuelve DATOS directamente.

Añadimos una función para que mapee una ruta (por defecto la aplicación arranca en el puerto 8080, aunque lo cambiamos en application properties, como ya sabéis), por ejemplo:

```Java
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestMethod;

//@Controller ANTIGUO: ESPERA DEVOLVER UN ARCHIVO HTML
@RestController //ACTUAL: DEVUELVE DATOS
public class IndexController {

//	Las Siguientes lineas anotadas son equivalentes, usando @RequestMapping o @GetMapping respectivamente
//	@RequestMapping(value = "/index", method = RequestMethod.GET)
//	@RequestMapping(value = "/index", method = RequestMethod.GET)
//	@GetMapping(value = "/index")
	@GetMapping("/index")
	public String index() {
		return "index";
	}
}
```

Ahora que ya tenemos el controlador, si ejecutamos la aplicación con el botón RUN podremos ver en la consola que la aplicación está arrancada en el puerto 8080 (por defecto):

```txt
Tomcat started on port 8080 (http) with context path '/'
Started SpringApiApplication in 4.452 seconds
```

A continuación, si visitáis a través de cualquier navegador la dirección: `http://localhost:8080/index`. podréis ver el texto pintado en pantalla.


### 4. RequestMapping
En ocasiones lo que nos conviene es a todo el controlador asignarle una ruta base, eso lo podemos hacer con esta etiqueta, de manera que si la ponemos en la cabecera del controlador:
```Java
@RestController
@RequestMapping("/app")
public class IndexController {
...
...
```

Ahora se accederá a través de la ruta `http://localhost:8080/app/index`, no obstante, Voy a aprovechar para que la ruta raíz del get también me funcione sin “/” y eso se hace añadiendo una ruta más con la cadena vacía:
```Java
	@GetMapping({"/index", ""})
	public String index() {
		return "index";
	}
```

De este modo Ahora también la página es devuelta en [http://localhost:8080/app](http://localhost:8080/app)

Ahora que ya sabemos devolver texto simple, ahora vamos a devolver **Objetos complejos** convertidos a **JSON** automáticamente, y vamos a leerlos desde la Base de Datos sin escribir ni una sola línea de SQL.

## 5. API Rest
La transferencia de estado representacional (en inglés representational state transfer) o REST es un estilo de arquitectura software para sistemas hipermedia distribuidos como la World Wide Web. El término se originó en el año 2000, en una tesis doctoral sobre la web escrita por Roy Fielding, uno de los principales autores de la especificación del protocolo HTTP y ha pasado a ser ampliamente utilizado por la comunidad de desarrollo.

Si bien el término REST se refería originalmente a un conjunto de principios de arquitectura —descritos más abajo—, en la actualidad se usa en el sentido más amplio para describir cualquier interfaz entre sistemas que utilice directamente HTTP para obtener datos o indicar la ejecución de operaciones sobre los datos, en cualquier formato (XML, JSON, etc) sin las abstracciones adicionales de los protocolos basados en patrones de intercambio de mensajes, como por ejemplo SOAP. Es posible diseñar sistemas de servicios web de acuerdo con el estilo arquitectural REST de Fielding y también es posible diseñar interfaces XMLHTTP de acuerdo con el estilo de llamada a procedimiento remoto (RPC), pero sin usar SOAP. Estos dos usos diferentes del término REST causan cierta confusión en las discusiones técnicas, aunque RPC no es un ejemplo de REST.

REST afirma que la web ha disfrutado de escalabilidad como resultado de una serie de diseños fundamentales clave:

- Un protocolo cliente/servidor sin estado: cada mensaje HTTP contiene toda la información necesaria para comprender la petición. Como resultado, ni el cliente ni el servidor necesitan recordar ningún estado de las comunicaciones entre mensajes. Sin embargo, en la práctica, muchas aplicaciones basadas en HTTP utilizan cookies y otros mecanismos para mantener el estado de la sesión (algunas de estas prácticas, como la reescritura de URLs, no son permitidas por REST).
- Un conjunto de operaciones bien definidas que se aplican a todos los recursos de información: HTTP en sí define un conjunto pequeño de operaciones, las más importantes son POST, GET, PUT y DELETE. Con frecuencia estas operaciones se equiparan a las operaciones CRUD en bases de datos (CLAB en castellano: crear, leer, actualizar, borrar) que se requieren para la persistencia de datos, aunque POST no encaja exactamente en este esquema.
- Una sintaxis universal para identificar los recursos. En un sistema REST, cada recurso es direccionable únicamente a través de su URI.
- El uso de hipermedios, tanto para la información de la aplicación como para las transiciones de estado de la aplicación: la representación de este estado en un sistema REST son típicamente HTML o XML. Como resultado de esto, es posible navegar de un recurso REST a muchos otros, simplemente siguiendo enlaces sin requerir el uso de registros u otra infraestructura adicional.

Un concepto importante en REST es la existencia de recursos (elementos de información),  que  pueden  ser  accedidos  utilizando  un  identificador  global (un Identificador  Uniforme  de  Recurso).  Para  manipular  estos  recursos, los componentes de la red (clientes y servidores) se comunican a través de una interfaz estándar (HTTP) e intercambian representaciones de estos recursos (los ficheros que se descargan y se envían) - es cuestión de debate, no obstante, si la distinción entre recursos y sus representaciones es demasiado platónica para su uso práctico en la red, aunque es popular en la comunidad RDF.

La petición puede ser transmitida por cualquier número de conectores (por ejemplo clientes, servidores, cachés, túneles, etc.) pero cada uno lo hace sin "ver más allá" de su propia petición (lo que se conoce como stateless (sin estado), otra restricción de REST, que es un principio común con muchas otras partes de la arquitectura de redes y de la información). Así, una aplicación puede interactuar con un recurso conociendo el identificador del recurso y la acción requerida, no necesitando conocer si existen cachés, proxys, cortafuegos, túneles o cualquier otra cosa entre ella y el servidor que guarda la información. La aplicación, sin embargo, debe comprender el formato de la información devuelta (la representación), que es por lo general un documento HTML o XML, aunque también puede ser una imagen o cualquier otro contenido.

## 5.1 Crear las clases del modelo

En primer lugar, crearemos en nuestra nueva Base de Datos "Spring-Database" las tablas que usaremos para nuestro nuevo proyecto Spring:

```SQL
CREATE TABLE usuarios (
	id SERIAL PRIMARY KEY, 
	nombre VARCHAR(100) NOT NULL, 
	email VARCHAR(100) NOT NULL UNIQUE, 
	password VARCHAR(100) NOT NULL 
);
```

```SQL
CREATE TABLE eventos (
	id SERIAL PRIMARY KEY, 
	nombre VARCHAR(150) NOT NULL, 
	descripcion VARCHAR(2000), 
	precio NUMERIC(8, 2), 
	fecha DATE,
	usuario_id INTEGER, 
	
	CONSTRAINT fk_evento_usuario FOREIGN KEY (usuario_id)
		REFERENCES usuarios (id)
		ON DELETE SET NULL 
);
```

Una vez creadas las tablas que usaremos en este proyecto, crearemos las entidades (@Entity) que servirán de modelo para nuestro proyecto de Spring:

```Java
@Entity
@Table(name = "usuarios")
@AllArgsConstructor
@NoArgsConstructor
@Getter
@Setter
public class Usuario {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Integer id;

    @Column(nullable = false, length = 100)
    private String nombre;

    @Column(nullable = false, length = 100, unique = true)
    private String email;
    
    @Column(nullable = false)
    private String password;

    // RELACIÓN 1:N
    // Un usuario tiene una lista de eventos.
    // Recordemos:"mappedBy" indica que la clave foránea no está aquí, sino en el      // atributo 'creador' de la clase Evento.
    @OneToMany(mappedBy = "creador", cascade = CascadeType.ALL)
    @JsonIgnore // <--- ¡IMPORTANTE!
    private List<Evento> eventos = new ArrayList<>();

}
```

```Java
@Entity
@Table(name = "eventos")
public class Evento {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Integer id;

    @Column(nullable = false)
    private String nombre;

    private String descripcion;
    private BigDecimal precio;
    private LocalDate fecha;

    @ManyToOne 
    @JoinColumn(name = "usuario_id")
    private Usuario creador;

}
```
Además de sus Constructores (importante el consturctor sin parámetros), Getter y Setter correspondientes -> más adelante usaremos lombok para ahorrarnos este paso. De momento lo mantenemos.

Algunas consideraciones sobre estas entidades:
- La notación @JsonIgnore que hemos incluido en el atributo OneToMany se debe a que, al convertir las entidades a objetos JSON, al pedir un Usuario, Spring convertirá a JSON sus Eventos, que a su vez contienen al Usuario, que contiene Eventos... -> _StackOverflowError_. @JsonIgnore corta el bucle para que el JSON se pueda generar.
- Hemos usado el tipo de dato java.time.LocalDate en lugar del obsoleto (y problemático) java.util.Date. Spring Boot 3 lo mapea automáticamente a la base de datos sin anotaciones extra para el formato.

## 5.2 La Capa de Datos: Repositorios

En lugar de crear una clase `UsuarioDAO` y escribir métodos para `save`, `update`, `delete` y `list`, como hacíamos hasta ahora, en Spring Boot utilizamos **Interfaces** que llevan a cabo todo el trabajo por sí solas.

Spring Data JPA implementa el patrón **Repository**. Solo tenemos que definir una interfaz que extienda de `JpaRepository`, indicándole dos cosas:

1. Qué **Entidad** va a gestionar.
2. De qué **tipo de dato** es su Clave Primaria (`@Id`).

Spring se encargará automáticamente de inyectar el código necesario en tiempo de ejecución.

Por tanto, crearemos un nuevo Package llamado `.repositories` y crearemos las distintas interfaces:

```Java
// Anotamos con @Repository (aunque JpaRepository ya lo hace implícitamente, es buena práctica)
@Repository
public interface UsuarioRepository extends JpaRepository<Usuario, Integer> {
    Usuario findByEmail(String email);
}

```

Al extender de JpaRepository, ya tenemos disponibles automáticamente:
.findAll()      -> SELECT * FROM usuarios
.findById(id)   -> SELECT * FROM usuarios WHERE id = ?
.save(usuario)  -> INSERT o UPDATE
.deleteById(id) -> DELETE

**Consultas Derivadas:** Si queremos buscar por algo que no sea el ID, solo nombramos el método correctamente: `findByEmail` -> SELECT * FROM usuarios WHERE email = ?


```Java
@Repository
public interface EventoRepository extends JpaRepository<Evento, Integer> {
    List<Evento> findByPrecioGreaterThan(double precio);
}
```

## 5.3 Consumir estos datos desde el Controlador.
Insertaremos algunos datos de prueba en nuestra base de datos, para consumir posteriormente desde Java:

```SQL
INSERT INTO usuarios (nombre, email, password) VALUES 
('Profesor Java', 'profe@clase.com', '123456'),
('Zipi Estudiante', 'zipi@clase.com', 'secret123'),
('Zape Oyente', 'zape@clase.com', 'pass');

INSERT INTO eventos (nombre, descripcion, precio, fecha, usuario_id) VALUES 
('Clase de Spring Boot', 'Aprenderemos a hacer APIs REST', 0.00, '2026-02-10', 1),
('Examen Final', 'El examen del módulo de Acceso a Datos', 0.00, '2026-02-20', 1);

INSERT INTO eventos (nombre, descripcion, precio, fecha, usuario_id) VALUES 
('Fiesta de Fin de Curso', 'Barbacoa en el patio', 15.50, '2026-06-25', 2),
('Grupo de Estudio', 'Repaso para el examen', 0.00, '2026-02-18', 2);
```

Ahora, crearemos un UsuariosController tal y como habíamos hecho previamente con el Index, para consumir el resultado de estos repositorios:

```Java
@RestController
public class UsuarioController {

    @Autowired
    private UsuarioRepository usuarioRepository;

    @GetMapping("/usuarios")
    public List<Usuario> verUsuarios() {
        return usuarioRepository.findAll();
    }
}
```

Una vez ejecutado el SQL podemos:

1. Arrancar la aplicación Spring Boot.
2. ir al navegador: `http://localhost:8080/usuarios`.

## Inyección de Dependencias
En el apartado anterior, para poder probar el repositorio inicial, hemos usado la anotación @Autowired dentro del controlador. 

Esto nos ha permitido usar un método definido en UsuarioRepository (findAll), sin llevar a cabo un `new UsuarioRepository()`. Esto ha sido posible gracias a uno de los pilares de Spring. la **Inyección de Dependencias**.
### ¿Qué es la Inyección de Dependencias?

En la programación tradicional, si una Clase A necesita usar una Clase B, la Clase A crea una instancia de B (`new ClaseB()`). Esto genera un **fuerte acoplamiento**: A depende directamente de cómo se crea B.

Tal y como vimos en la introducción del tema, Spring utiliza el principio de **Inversión de Control (IoC)**. En lugar de que tu clase controle sus dependencias, es el **Contenedor de Spring** (el "cerebro" del framework) quien:

1. Escanea tu proyecto al arrancar.
2. Crea las instancias de las clases necesarias (llamadas **Beans**).
3. Las "inyecta" automáticamente donde se necesiten.

Por tanto, con esta porción de código que hemos usado en nuestro controlador:
```Java
@Autowired
private UsuarioRepository usuarioRepository;
```
Le estamos indicando a Spring que necesitamos que busque una instancia válida de UsuarioRepository y la *conecte* directamente con la variable usuarioRepository.

| **Beneficios**                                                                                                                                                         | **Costes**                                                                                                                                                                                                                                                                                                                                                                            |
| ---------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Desacoplamiento:** El controlador no necesita saber cómo se crea o se conecta a la base de datos el repositorio. Solo necesita usarlo.                               | **Peligro de dependencias cruzadas:** ocurren cuando dos o más componentes (beans) dependen entre sí directa o indirectamente, formando un ciclo que impide que Spring los inicialice correctamente (por ejemplo, A necesita a B, y B necesita a A). <br>Debemos vigilar estos casos ya que provoca que la aplicación no inicie, lanzando un error `BeanCurrentlyInCreationException` |
| **Singleton por defecto:** Spring crea una única instancia del repositorio y la reutiliza en toda la aplicación, ahorrando memoria.                                    | **Errores en tiempo de ejecución:** Si Spring no encuentra un Bean que coincida (por ejemplo, olvidaste la anotación `@Repository`), la aplicación fallará al arrancar.                                                                                                                                                                                                               |
| **Testabilidad:** Facilita enormemente las pruebas unitarias, ya que podemos inyectar repositorios "falsos" (mocks) para probar el controlador sin base de datos real. |                                                                                                                                                                                                                                                                                                                                                                                       |

Llegados a este punto, debo detenerme para hablar un poco de como tenemos estructurado nuestro proyecto. En el apartado anterior definimos los repositorios y los inyectamos directamente en el controlador que *expone* el método al navegador para hacer una prueba rápida de obtención de datos de la base de datos y Modelo Vista Controlador (MVC). Sin embargo, inyectar el Repositorio en el Controlador es una mala práctica (anti-patrón) en aplicaciones reales, ya que mezcla la lógica de exposición (HTTP) con la lógica de base de datos, impidiendo la reutilización y el testing adecuado.

En una aplicación profesional Spring Boot, no conectamos la base de datos directamente a la web. Seguimos una **Arquitectura en 3 Capas**:

- **Capa de Presentación (Controller):** Recibe la petición HTTP del usuario, valida los datos de entrada y llama al Servicio. **No tiene lógica de negocio**.

- **Capa de Servicio (Service):** Aquí reside la **lógica de negocio**. Es quien decide _qué_ hacer. Llama al Repositorio. Comúnmente usa Interfaces e Implementaciones.

- **Capa de Acceso a Datos (Repository):** Solo se encarga de hablar con la base de datos.

## 5.4 Creación de la Capa intermedia de Servicios...

Para crear la capa intermedia, utilizamos la anotación **`@Service`**. Funciona igual que `@Repository` o `@RestController`: Spring interpreta esta anotación como que este es un componente de lógica de negocio, crea una instancia del mismo (Bean) y la tiene lista para inyectar.

Tal y como hemos mencionado, usaremos Interfaces e Implementaciones para definir la capa de Servicios. Será la interfaz la que contenga la notación @Service y la que posteriormente inyectaremos donde sea necesario mediante la notación @Autowired.

En primer lugar crearemos la paquetería correspondiente para la capa de servicios (Interfaces e implementaciones):
![[Pasted image 20260211110011.png]]

Definimos el método de obtención de usuarios  que teníamos hasta ahora, esta vez consumiendo el *repositorio* desde el *servicio*: 

```Java
@Service
public interface UsuarioService {

	public List<Usuario> obtenerTodos();
	
}
```

```Java
public class UsuarioServiceImpl implements UsuarioService{
	
	@Autowired
    private UsuarioRepository usuarioRepository;

	@Override
	public List<Usuario> obtenerTodos() {
		return usuarioRepository.findAll();
	}

}
```

Y modificamos el Controller anterior para que use esta capa de Servicio, en lugar de consumir directamente el repositorio:

```Java
@RestController
public class UsuarioController {

    @Autowired
    private UsuarioService usuarioService;

    @GetMapping("/usuarios")
    public List<Usuario> verUsuarios() {
        return usuarioService.obtenerTodos();
    }
}
```

De este modo queda definida la estructura definitiva y profesional del proyecto, que conecta las capas en cascada:

**Controller** --usa--> **Service** --usa--> **Repository**

### Siguiendo con nuestra API REST...

#### Obtener un único usuario

Ahora que tenemos nuestro `UsuarioController` conectado al repositorio mediante una capa intermedia de Servicio, vamos a continuar añadiendo funcionalidades para exponer nuestros datos a Internet.

A menudo necesitaremos el detalle de un solo registro. El método getXById que ofrece JpaRepository devuelve un Optional. Puede haberse encontrado el usuario, o no. Habrá que validarlo antes de devolver el usuario final.

**CONTROLLER**
```Java
@GetMapping("/usuarios/{id}")
public ResponseEntity<Usuario> getUsuarioById(@PathVariable Integer id) {
	
	Usuario usuario = usuarioService.obtenerPorId(id);
	
	if (usuario != null) {
		return ResponseEntity.ok(usuario); // Devuelve 200 OK y el usuario
	} else {
		return ResponseEntity.notFound().build(); // Devuelve 404 Not Found
	}
}
```
**`@PathVariable`**: Le dice a Spring que coja el valor `{id}` de la URL y lo meta en la variable `Integer id`.
**`ResponseEntity`**: Es un objeto contenedor que nos permite controlar no solo los datos, sino también el **código de estado HTTP** (200 si todo va bien, 404 si no existe).

**SERVICE**
```Java
public Usuario obtenerPorId(Integer id);
```

**SERVICE IMPL**
```Java
@Override
public Usuario obtenerPorId(Integer id) {
	return usuarioRepository.findById(id).orElse(null);
}
```

#### Crear un nuevo Usuario (Post)

Para enviar datos a la base de datos, utilizamos el verbo HTTP **POST**.

**CONTROLLER**
```Java
@PostMapping
public ResponseEntity<Usuario> createUsuario(@RequestBody Usuario usuario) {
	Usuario nuevoUsuario = usuarioService.guardar(usuario);
	// Devolvemos código 201 (Created)
	return ResponseEntity.status(HttpStatus.CREATED).body(nuevoUsuario);
}
```

**SERVICE**
```Java
public Usuario guardar(Usuario usuario);
```

**SERVICE IMPL**
```Java
@Override
public Usuario guardar(Usuario usuario) {
	// Aquí podríamos poner lógica extra: validar email, encriptar contraseña, etc.
	return usuarioRepository.save(usuario);
}
```

El método .save() devuelve el objeto guardado (con el ID autogenerado ya relleno).
- **`@PostMapping`**: Mapea peticiones POST.
- **`@RequestBody`**: Indica a Spring que busque los datos del usuario dentro del **cuerpo (body)** de la petición HTTP y trate de convertir ese JSON recibido en un objeto Java `Usuario`.

#### Actualizar un usuario existente (Post)
Para modificar un registro, utilizamos el verbo HTTP **PUT**. La lógica es: buscar si existe el ID; si existe, actualizamos sus campos y guardamos; si no, devolvemos un error 404.

**CONTROLLER**
```Java
@PutMapping("/{id}") 
public ResponseEntity<Usuario> updateUsuario(@PathVariable Integer id, @RequestBody Usuario usuario) {

	 // Llamamos al servicio para intentar actualizar 
	 Usuario actualizado = usuarioService.actualizar(id, usuario);
	 if (actualizado != null) {
		 return ResponseEntity.ok(actualizado); 
	 } else {
		 return ResponseEntity.notFound().build(); // No existía el ID
	 }
	 
 }
```
**`@PutMapping`**: Mapea la petición de actualización.

**SERVICE**
```Java
public Usuario actualizar(Integer id, Usuario usuarioDetalles);
```

**SERVICE IMPL**
```Java
@Override
	public Usuario actualizar(Integer id, Usuario usuarioDetalles) {
		// Buscamos el usuario en la BBDD
        Usuario usuarioExistente = usuarioRepository.findById(id).orElse(null);

        // Si existe, actualizamos sus campos
        if (usuarioExistente != null) {
            usuarioExistente.setNombre(usuarioDetalles.getNombre());
            usuarioExistente.setEmail(usuarioDetalles.getEmail());
            // No tocamos el ID ni la contraseña en este ejemplo
            
            // Guardamos los cambios
            return usuarioRepository.save(usuarioExistente);
        }

        // Si no existía, devolvemos null
        return null;
	}
```

#### Borrar un Usuario existente (Delete)

Finalmente, para eliminar registros usamos el verbo **DELETE**.

**CONTROLLER**
```Java
@DeleteMapping("/{id}")
    public ResponseEntity<Void> deleteUsuario(@PathVariable Integer id) {
        boolean borrado = usuarioService.borrar(id);

        if (borrado) {
            return ResponseEntity.noContent().build();
        } else {
            return ResponseEntity.notFound().build(); // 404 No existía
        }
    }
```

- **`@DeleteMapping`**: Mapea la petición de borrado.

**SERVICE**
```Java
public boolean borrar(Integer id);
```

**SERVICE IMPL**
```Java
@Override
public boolean borrar(Integer id) {
	boolean response = false;
	if (usuarioRepository.existsById(id)) {
		usuarioRepository.deleteById(id);
		response = true;
	}
	return response;
}
```

Con esto tendríamos completado nuestra API REST con Spring, con todos los métodos CRUD básicos. Observa que no hemos modificado en ningún momento el Repository, ya que por el momento hemos usado únicamente métodos ofrecidos por defecto por la extensión JpaRepository.

## 6. Consumiendo los recursos de nuestra API Rest (Postman)