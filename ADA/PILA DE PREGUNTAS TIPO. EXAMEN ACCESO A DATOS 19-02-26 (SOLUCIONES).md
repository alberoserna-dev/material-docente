

**1. Analice el siguiente fragmento de código para cargar el driver de PostgreSQL. ¿Cuál es la opción correcta para sustituir a `________` y evitar errores de compilación y ejecución?**


```Java
public static void main(String[] args) {
    try {
        ________("org.postgresql.Driver");
    } catch (ClassNotFoundException e) {
        e.printStackTrace();
    }
}
```

A) `Driver.load` 
B) `Class.forName` 
C) `DriverManager.registerDriver` 
D) `System.loadLibrary`

**Solución:** B

**2. Según el temario, ¿cuál es la principal desventaja técnica de usar ODBC ("Open DataBase Connectivity") directamente desde una aplicación Java en comparación con JDBC?** 

A) ODBC no permite conectar con bases de datos relacionales modernas como PostgreSQL. 
B) ODBC requiere el uso de punteros en lenguaje C, lo que introduce riesgos de seguridad y falta de robustez en la conversión a Java. 
C) ODBC es una tecnología propietaria de Microsoft y no funciona en sistemas Linux. 
D) ODBC no soporta el uso de sentencias SQL estándar.

**Solución:** B

**3. Observe la siguiente línea de conexión. ¿Qué elemento falta o es incorrecto en la estructura de la URL JDBC para PostgreSQL según los ejemplos del tema?** `String url = "jdbc:postgresql:5432/datos1";` 

A) Falta el nombre del host (ej: `//localhost`) antes del puerto. 
B) El protocolo debería ser `jdbc:odbc:postgresql`. 
C) El puerto 5432 es incorrecto para PostgreSQL, debería ser 3306. 
D) Falta especificar el usuario y contraseña dentro de la propia cadena URL obligatoriamente.

**Solución:** A

**4. ¿Cuál de las siguientes afirmaciones describe correctamente la diferencia de rendimiento entre `Statement` y `PreparedStatement`?** 

A) `Statement` es más rápido porque no requiere precompilación en la base de datos. 
B) `PreparedStatement` es más lento en la primera ejecución pero más rápido en las siguientes porque la ruta de acceso se almacena en caché. 
C) `PreparedStatement` está precompilado, lo que permite reutilizar la misma instrucción SQL varias veces con mayor rendimiento. 
D) No existe diferencia de rendimiento, la única diferencia es la seguridad contra inyección SQL.

**Solución:** C

**5. Analice el siguiente bloque de cierre de recursos. Si se lanzase una excepción al cerrar el `ResultSet` (rs), ¿qué ocurriría con el cierre de la `Connection` (con)?**

```Java
try {
    // ... código ...
} finally {
    rs.close();
    statement.close();
    con.close();
}
```

A) La conexión se cerrará igualmente porque está en un bloque `finally`. 
B) La conexión no se cerrará porque la excepción en `rs.close()` interrumpirá el bloque `finally`.
C) Java cerrará automáticamente la conexión al detectar el error en el ResultSet. 
D) Depende de la implementación del driver JDBC.

**Solución:** B

**6. Para solucionar el problema de seguridad conocido como "Inyección SQL", ¿qué mecanismo ofrece JDBC según el texto?** 

A) El uso de métodos `setString` que eliminan automáticamente las comillas simples de las cadenas. 
B) El uso de la interfaz `PreparedStatement` que permite parametrizar las consultas. 
C) La validación automática de datos en el `DriverManager`. 
D) El uso de procedimientos almacenados obligatorios.

**Solución:** B

**7. Indique qué línea completa correctamente la siguiente sentencia preparada para insertar un nombre en la base de datos:**

```Java
String sql = "INSERT INTO clientes (nombre) VALUES (?)";
PreparedStatement ps = con.prepareStatement(sql);
________;
ps.executeUpdate();
```

A) `ps.setParameter(1, "Juan")` 
B) `ps.setString(0, "Juan")` 
C) `ps.setString(1, "Juan")` 
D) `ps.setString("nombre", "Juan")`

**Solución:** C

**8. Si deseamos ejecutar una sentencia SQL de tipo `UPDATE`, `DELETE` o `INSERT`, ¿qué método debemos llamar sobre el objeto `Statement` y qué devuelve?** 

A) `executeQuery()`, que devuelve un objeto `ResultSet`. 
B) `execute()`, que devuelve un `boolean`. 
C) `executeUpdate()`, que devuelve un `int` indicando el número de filas afectadas. 
D) `executeUpdate()`, que devuelve un `void` ya que no hay resultados.

**Solución:** C

**9. En la iteración de un `ResultSet`, ¿qué función cumple exactamente el método `rs.next()`?** 

A) Devuelve el siguiente registro y retorna `null` si no hay más. 
B) Mueve el cursor a la siguiente fila y devuelve `true` si dicha fila existe y es válida. 
C) Mueve el cursor a la siguiente fila y devuelve el número de fila actual. 
D) Verifica si hay una siguiente fila sin mover el cursor.

**Solución:** B

**10. Al recuperar un dato numérico de un `ResultSet`, el temario indica una optimización respecto a leerlo como cadena. ¿Cuál es?** 

A) Usar `rs.getObject(1)` y hacer un cast a `int`. 
B) Usar `rs.getString(1)` y luego `Integer.parseInt()`. 
C) Usar directamente `rs.getInt(1)` para evitar la conversión de cadena a número. 
D) JDBC convierte automáticamente todos los números a `double` para mayor precisión.

**Solución:** C

**11. ¿Cuál es el índice de la primera columna en un `ResultSet` cuando utilizamos los métodos `get...` (ej: `getString(int index)`)?** 

A) 0 
B) 1 
C) -1 
D) Depende de la configuración de la base de datos.

**Solución:** B

**12. Analice la sintaxis del "try-with-resources" mostrada en el tema. ¿Cuál es la principal ventaja de utilizar esta estructura para la conexión y los statements?**

```Java
try (Connection conn = DriverManager.getConnection(url, user, pass);
     Statement stmt = conn.createStatement()) { ... }
```

A) Mejora el rendimiento de la consulta SQL. 
B) Permite capturar excepciones de tipo `ClassNotFoundException` automáticamente. 
C) Garantiza el cierre automático de los recursos `conn` y `stmt` al finalizar el bloque, incluso si ocurren excepciones. 
D) Permite abrir múltiples conexiones simultáneas al mismo servidor.

**Solución:** C

**13. Si utilizamos el método `rs.getString("NOMBRE")`, ¿qué característica destaca el texto sobre el nombre de la columna?** 

A) Debe estar escrito estrictamente en mayúsculas. 
B) Debe coincidir exactamente con el nombre en la base de datos (case-sensitive). 
C) Es indiferente si se escribe en mayúsculas o minúsculas. 
D) No se recomienda usar nombres de columnas, solo índices numéricos.

**Solución:** C

**14. ¿Qué excepción obligatoria (Checked Exception) debemos gestionar o propagar al utilizar los métodos de conexión y consulta de JDBC?** 

A) `DatabaseException` 
B) `SQLRuntimeException` 
C) `IOException` 
D) `SQLException`

**Solución:** D

**15. ¿Qué interfaz de JDBC se define en el texto como "un portador entre un programa Java y la base de datos" que no está precompilado?** 

A) `DriverManager` 
B) `PreparedStatement` 
C) `Statement` 
D) `Connection`

**Solución:** C

**16. En el código proporcionado en el tema, se utiliza `Class.forName("org.postgresql.Driver")`. ¿Qué función cumple esta línea?** 

A) Establece la conexión física con el servidor. 
B) Carga dinámicamente la clase del driver JDBC en la memoria de la JVM. 
C) Verifica que la base de datos PostgreSQL esté instalada en el sistema. 
D) Importa el paquete `java.sql` automáticamente.

**Solución:** B

**17. Observe el siguiente código. ¿Qué ocurrirá si la consulta `SELECT` no devuelve ningún resultado?**

```Java
ResultSet rs = statement.executeQuery("SELECT * FROM vacia");
while (rs.next()) {
    System.out.println(rs.getString(1));
}
```

A) Se lanzará una `SQLException` indicando "No data found". 
B) El programa entrará en un bucle infinito. 
C) El método `rs.next()` devolverá `false` inmediatamente y no se imprimirá nada. 
D) Se imprimirá `null` por consola.

**Solución:** C

**18. ¿Cuál es el orden correcto y seguro para cerrar los objetos JDBC manualmente en un bloque `finally` (asumiendo que se han abierto en orden: Connection -> Statement -> ResultSet)?** 

A) Connection -> Statement -> ResultSet 
B) ResultSet -> Statement -> Connection 
C) Statement -> ResultSet -> Connection 
D) El orden es indiferente siempre que se cierren todos.

**Solución:** B

**19. Según el texto, ¿qué desventaja tienen las "Bases de datos incrustadas" frente al uso de una API genérica como JDBC con un gestor independiente?** 

A) Son más lentas en la comunicación. 
B) Requieren cambios profundos en el programa si se necesita conectar a una base de datos distinta. 
C) No soportan SQL. 
D) Solo funcionan en entornos Windows.

**Solución:** B

**20. ¿Qué método de la clase `DriverManager` se utiliza para obtener una instancia de `Connection`?** 

A) `DriverManager.createConnection(url)` 
B) `DriverManager.getDriver(url).connect()` 
C) `DriverManager.getConnection(url, usuario, password)` 
D) `new Connection(url, usuario, password)`

**Solución:** C

## TEMA 4: MAPEO OBJETO-RELACIONAL (HIBERNATE)

**1. En la configuración de Hibernate mediante XML (`hibernate.cfg.xml`), ¿qué propiedad es fundamental para que el framework sepa cómo traducir el HQL a las sentencias SQL específicas de la base de datos que estamos usando (por ejemplo, MySQL o PostgreSQL)?** A) `connection.driver_class` B) `hibernate.connection.url` C) `hibernate.dialect` D) `show_sql`

**Solución:** C

**2. Analice el siguiente fragmento de un fichero `hibernate.cfg.xml`. ¿Qué línea falta donde aparece `________` para registrar el fichero de mapeo de la clase `Libro`?**

XML

```
<session-factory>
    ...
    <mapping class="com.ejemplo.model.Autor"/>
    ________
</session-factory>
```

A) `<file path="com/ejemplo/model/Libro.hbm.xml"/>` B) `<mapping resource="com/ejemplo/model/Libro.hbm.xml"/>` C) `<class name="com.ejemplo.model.Libro"/>` D) `<property name="mapping">com/ejemplo/model/Libro.hbm.xml</property>`

**Solución:** B

**3. En un proyecto Hibernate configurado mediante anotaciones, ¿qué dos anotaciones son estrictamente obligatorias en una clase Java para que sea considerada una entidad persistente mínima?** A) `@Entity` y `@Table` B) `@Entity` y `@Id` C) `@Persistence` y `@Id` D) `@Table` y `@Column`

**Solución:** B

**4. Observe la siguiente relación Uno-a-Muchos en la clase `Autor`. ¿Qué atributo falta en la anotación `@OneToMany` para indicar que la relación es bidireccional y que la clave foránea está gestionada por el otro lado (la clase `Libro`)?**

Java

```
@Entity
@Table(name="autores")
public class Autor {
    @Id
    private int id;

    @OneToMany(________ = "autor", cascade = CascadeType.ALL)
    private List<Libro> libros;
}
```

A) `targetEntity` B) `mappedBy` C) `joinColumn` D) `fetch`

**Solución:** B

**5. Si estamos utilizando Hibernate con ficheros de mapeo XML (`.hbm.xml`), ¿qué etiqueta se utiliza dentro de `<class>` para mapear un atributo simple de tipo `String` o `int` a una columna de la base de datos?** A) `<column>` B) `<field>` C) `<attribute>` D) `<property>`

**Solución:** D

**6. Analice el siguiente código de inserción de un objeto. ¿Qué línea fundamental falta al final para que los cambios se hagan efectivos en la base de datos física?**

Java

```
Session session = factory.openSession();
Transaction tx = session.beginTransaction();

Autor autor = new Autor();
autor.setNombre("Cervantes");
session.save(autor);

________;
session.close();
```

A) `session.flush()` B) `session.persist(autor)` C) `tx.commit()` D) `factory.close()`

**Solución:** C

**7. En la estrategia de generación de claves primarias con anotaciones, ¿qué opción debemos usar en `@GeneratedValue(strategy = ...)` si queremos que la base de datos utilice una columna autoincremental (tipo `AUTO_INCREMENT` en MySQL o `SERIAL` en PostgreSQL)?** A) `GenerationType.SEQUENCE` B) `GenerationType.AUTO` C) `GenerationType.TABLE` D) `GenerationType.IDENTITY`

**Solución:** D

**8. Según el documento de "Hibernate con anotaciones", si tenemos una clase `Libro` con un atributo `titulo`, pero en la base de datos la columna se llama `titulo_libro` y es obligatoria (NOT NULL), ¿cómo debemos anotarlo?** A) `@Column(name = "titulo_libro", nullable = false)` B) `@Basic(optional = false, column = "titulo_libro")` C) `@Field(value = "titulo_libro", required = true)` D) `@TableColumn(name = "titulo_libro", notNull = true)`

**Solución:** A

**9. ¿Qué objeto de Hibernate se crea UNA sola vez durante la inicialización de la aplicación y es el encargado de fabricar las sesiones?** A) `Session` B) `Transaction` C) `Configuration` D) `SessionFactory`

**Solución:** D

**10. En el mapeo XML, si la clase `Libro` tiene una relación Muchos-a-Uno con `Autor`, ¿qué etiqueta debemos usar en `Libro.hbm.xml` para representar esta clave foránea?** A) `<one-to-many class="Autor"/>` B) `<many-to-one name="autor" class="com.ejemplo.Autor" column="codautor"/>` C) `<join column="codautor" table="autores"/>` D) `<foreign-key column="codautor"/>`

**Solución:** B

**11. ¿Qué método de la interfaz `Session` se utiliza para recuperar un objeto de la base de datos dado su identificador (Clave Primaria)?** A) `session.load(Clase.class, id)` o `session.get(Clase.class, id)` B) `session.find(id)` C) `session.select(Clase.class, id)` D) `session.fetch(id)`

**Solución:** A

**12. En una relación bidireccional Uno-a-Muchos con anotaciones, ¿dónde debemos colocar la anotación `@JoinColumn` para especificar la columna de clave foránea real en la base de datos?**

A) En el lado `@OneToMany` (el "Uno", la lista). 
B) En el lado `@ManyToOne` (el "Muchos", el hijo). 
C) En ambos lados obligatoriamente. 
D) En ninguno, Hibernate la deduce por el nombre de la clase.

**Solución:** B

**13. Si deseamos borrar un objeto persistente de la base de datos usando Hibernate, ¿cuál es la secuencia correcta de comandos dentro de una transacción?** 

A) `session.remove(objeto);` 
B) `session.delete(objeto);` 
C) `session.erase(objeto);` 
D) `session.drop(objeto);`

**Solución:** B

**14. Observe el siguiente código para configurar Hibernate 5+ mediante código (sin `hibernate.cfg.xml` directo). ¿Qué clase constructora se utiliza típicamente para cargar la configuración y construir la `SessionFactory`?**

```Java
Configuration config = new Configuration();
config.configure();
ServiceRegistry serviceRegistry = new _________
    .applySettings(config.getProperties()).build();
sessionFactory = config.buildSessionFactory(serviceRegistry);
```

A) `HibernateRegistryBuilder` 
B) `StandardServiceRegistryBuilder` 
C) `SessionBuilder` 
D) `TransactionFactoryBuilder`

**Solución:** B

**15. ¿Qué significa el concepto de "Ingeniería Inversa" mencionado en el temario de Hibernate con XML?** 

A) Generar la base de datos a partir de las clases Java. 
B) Generar las clases Java (POJOs) y los ficheros de mapeo (`.hbm.xml`) automáticamente a partir de las tablas de la base de datos existente. 
C) Convertir código XML a anotaciones automáticamente. 
D) Recuperar una transacción fallida.

**Solución:** B

**16. En una consulta HQL (Hibernate Query Language), si queremos obtener todos los libros, ¿cuál es la sintaxis correcta?** 

A) `SELECT * FROM libros` (Nombre de la tabla) 
B) `FROM Libro` (Nombre de la clase) 
C) `GET ALL Libro` 
D) `SELECT l FROM table_libros l`

**Solución:** B

**17. Si definimos una propiedad en el POJO como `int` (primitivo) pero en la base de datos el campo permite nulos, ¿qué problema podríamos tener según las buenas prácticas de mapeo?** 

A) Hibernate lanzará una excepción al iniciar. 
B) Si la base de datos devuelve `null`, Hibernate no podrá asignarlo al `int` primitivo y lanzará un error; se debería usar la clase envoltorio `Integer`. 
C) Hibernate convertirá el `null` a `-1` automáticamente. 
D) No hay ningún problema, `int` soporta nulos en Java.

**Solución:** B

**18. En el fichero `hibernate.cfg.xml`, ¿para qué sirve la propiedad `hbm2ddl.auto` con el valor `update`?** 

A) Para borrar la base de datos cada vez que se inicia la aplicación. 
B) Para validar que el esquema coincida, pero sin tocar nada. 
C) Para actualizar el esquema de la base de datos (crear tablas o columnas nuevas) si hay cambios en el modelo de objetos, manteniendo los datos existentes. 
D) Para formatear las consultas SQL en la consola.

**Solución:** C

**19. ¿Qué anotación se utiliza para definir que un atributo no debe ser persistido en la base de datos (es decir, que Hibernate debe ignorarlo)?** 

A) `@Ignored` 
B) `@Transient` 
C) `@NotMapped` 
D) `@Temporary`

**Solución:** B

**20. Al usar anotaciones, si queremos especificar que una relación es de tipo "Muchos a Muchos", ¿qué anotación utilizamos?** 

A) `@ManyToMany` 
B) `@OneToMany` repetido dos veces. 
C) `@JoinTable` solamente. 
D) `@Relationship(type = "N:M")`

**Solución:** A



**1. En la definición de la interfaz de un repositorio con Spring Data JPA, ¿qué línea completa correctamente el siguiente código para gestionar la entidad `Evento` cuya clave primaria es de tipo `Integer`?**

Java

```
@Repository
public interface EventoRepository extends __________________ {
}
```

A) `CrudRepository<Evento>` B) `JpaRepository<Integer, Evento>` C) `JpaRepository<Evento, Integer>` D) `Repository<Evento>`

**Solución:** C

**2. Analice la relación definida en la clase `Usuario`. ¿Qué atributo falta en la anotación `@OneToMany` para indicar que la relación es bidireccional y que la clave foránea está gestionada por el campo `usuario` de la clase `Evento`?**

Java

```
@Entity
public class Usuario {
    @Id
    private Integer id;
    
    @OneToMany(________ = "usuario", cascade = CascadeType.ALL)
    private List<Evento> eventos;
}
```

A) `targetEntity` B) `mappedBy` C) `joinColumn` D) `fetch`

**Solución:** B

**3. En la arquitectura de tres capas implementada en el tema, ¿cuál es la responsabilidad principal de la clase anotada con `@Service`?** A) Gestionar las peticiones HTTP y devolver códigos de estado (200, 404). B) Definir la conexión directa con la base de datos y las credenciales. C) Encapsular la lógica de negocio y orquestar las llamadas al repositorio. D) Mapear los JSON a objetos Java automáticamente.

**Solución:** C

**4. ¿Qué anotación se debe colocar sobre la clase `UsuarioController` para indicar que es un componente que gestiona peticiones HTTP y que la respuesta de sus métodos se serializará automáticamente (por ejemplo, a JSON) en el cuerpo de la respuesta?** A) `@Controller` B) `@WebComponent` C) `@Service` D) `@RestController`

**Solución:** D

**5. Observe el siguiente método en el Controlador. ¿Qué anotación falta para que Spring inyecte el valor de `{id}` de la URL en el parámetro del método?**

Java

```
@GetMapping("/{id}")
public ResponseEntity<Usuario> getUsuarioById(@________ Integer id) {
    // ...
}
```

A) `RequestParam` B) `PathVariable` C) `PathParam` D) `BodyParam`

**Solución:** B

**6. En el fichero `application.properties`, ¿qué efecto tiene la propiedad `spring.jpa.hibernate.ddl-auto=update`?** A) Borra la base de datos cada vez que se reinicia la aplicación. B) Genera un script `.sql` en la carpeta resources pero no lo ejecuta. C) Actualiza el esquema de la base de datos (crea tablas o columnas nuevas) si hay cambios en el modelo, manteniendo los datos existentes. D) Valida que el esquema coincida, lanzando una excepción si no es idéntico.

**Solución:** C

**7. Si queremos implementar un método en `EventoRepository` que busque eventos cuyo precio sea mayor que cierto valor, utilizando la funcionalidad de "Derived Query Methods" de Spring Data, ¿cuál sería el nombre correcto del método?** A) `findEventsWherePriceIsBig(double precio)` B) `getAllByPrecio(double precio)` C) `findByPrecioGreaterThan(double precio)` D) `queryPrecioMayorQue(double precio)`

**Solución:** C

**8. En la clase `Usuario`, ¿qué anotación utilizamos para indicar que el campo `id` es la clave primaria y que su valor debe ser generado automáticamente por la base de datos (autoincremental)?**

Java

```
@Id
@________(strategy = GenerationType.IDENTITY)
private Integer id;
```

A) `GeneratedValue` B) `AutoIncrement` C) `SequenceGenerator` D) `Identity`

**Solución:** A

**9. ¿Qué función cumple la anotación `@Autowired` cuando se coloca sobre el atributo `private UsuarioRepository usuarioRepository;` dentro de una clase Service o Controller?** A) Indica que ese atributo es una clave primaria foránea. B) Habilita la inyección de dependencias, permitiendo que Spring instancie y asigne automáticamente el repositorio. C) Crea una nueva tabla en la base de datos llamada `usuario_repository`. D) Obliga a que el repositorio sea serializable.

**Solución:** B

**10. En el método `createUsuario` del controlador, ¿qué anotación es imprescindible para que Spring deserialice el JSON enviado en el cuerpo de la petición POST y lo convierta en un objeto `Usuario`?**

Java

```
@PostMapping
public ResponseEntity<Usuario> createUsuario(@________ Usuario usuario) {
    // ...
}
```

A) `RequestAttribute` B) `PathVariable` C) `ModelAttribute` D) `RequestBody`

**Solución:** D

**11. Analice el siguiente código del `UsuarioService` para actualizar un usuario. ¿Qué lógica fundamental falta en el espacio marcado?**

```Java
public Usuario actualizar(Integer id, Usuario usuarioDetalles) {
    Usuario usuarioExistente = usuarioRepository.findById(id).orElse(null);
    if (usuarioExistente != null) {
        usuarioExistente.setNombre(usuarioDetalles.getNombre());
        usuarioExistente.setEmail(usuarioDetalles.getEmail());
        ________________________; // ¿Qué falta aquí?
    }
    return null;
}
```

A) `return usuarioRepository.save(usuarioExistente)` 
B) `return usuarioRepository.update(usuarioExistente)` 
C) `entityManager.refresh(usuarioExistente)` 
D) `return new Usuario()`

**Solución:** A

**12. En la clase `Evento`, que tiene una relación Muchos-a-Uno con `Usuario`, ¿qué anotación se utiliza para definir la columna de clave foránea que unirá ambas tablas?**


```Java
@ManyToOne
@________(name = "usuario_id")
private Usuario usuario;
```

A) `ForeignKey` 
B) `JoinColumn` 
C) `MappedBy` 
D) `ColumnLink`

**Solución:** B

**13. ¿Qué devuelve el método `usuarioRepository.findById(id)` por defecto (método estándar de `JpaRepository`)?** 

A) Un objeto `Usuario` directamente (o `null` si no existe). 
B) Un `Optional<Usuario>`, obligando a gestionar la posible ausencia del valor. 
C) Una `List<Usuario>` con un solo elemento. 
D) Un `ResponseEntity` con el usuario.

**Solución:** B

**14. En el método `deleteUsuario` del controlador, si el servicio confirma que el usuario ha sido borrado correctamente, ¿qué respuesta HTTP es la estándar y correcta según el temario?** 

A) `ResponseEntity.ok().build()` (200 OK) con el usuario borrado en el cuerpo. 
B) `ResponseEntity.noContent().build()` (204 No Content). 
C) `ResponseEntity.accepted().build()` (202 Accepted). 
D) `ResponseEntity.status(HttpStatus.CREATED).build()`.

**Solución:** B

**15. ¿Qué dependencia de Maven es la encargada de incluir todas las bibliotecas necesarias para trabajar con JPA, Hibernate y la conexión a base de datos en Spring Boot?** 

A) `spring-boot-starter-web` 
B) `spring-boot-starter-security` 
C) `spring-boot-starter-data-jpa` 
D) `spring-boot-starter-thymeleaf`

**Solución:** C

**16. ¿Cuál es la ruta base para todos los endpoints del `UsuarioController` si la clase está anotada de la siguiente manera?**

```Java
@RestController
@RequestMapping("/api/usuarios")
public class UsuarioController { ... }
```

A) `http://localhost:8080/usuarios` 
B) `http://localhost:8080/api/usuarios` 
C) `http://localhost:8080/api` 
D) `http://localhost:8080/`

**Solución:** B

**17. Si intentamos guardar un objeto usando `usuarioRepository.save(usuario)`, ¿cómo sabe Hibernate si debe ejecutar un `INSERT` o un `UPDATE`?** 

A) Por el nombre del método (save vs update). 
B) Hibernate siempre hace INSERT y si falla hace UPDATE. 
C) Comprobando si el campo `@Id` del objeto es nulo (o no existe en BBDD) para insertar, o si tiene un valor existente para actualizar. 
D) Se debe especificar manualmente con un parámetro booleano.

**Solución:** C

**18. En el método `getUsuarioById` del controlador, ¿qué línea completa correctamente la lógica para devolver un 404 si el usuario es `null`?**

```Java
Usuario usuario = usuarioService.obtenerPorId(id);
if (usuario != null) {
    return ResponseEntity.ok(usuario);
} else {
    return ________________________;
}
```

A) `ResponseEntity.error().build()` 
B) `ResponseEntity.notFound().build()` 
C) `ResponseEntity.status(400).build()` 
D) `null`

**Solución:** B

**19. ¿Qué es Spring Initializr (start.spring.io) según se describe en el tema?** 

A) Un IDE específico para programar en Spring. 
B) Una herramienta web para generar la estructura inicial de un proyecto Spring Boot con las dependencias seleccionadas. 
C) El gestor de base de datos embebido de Spring. 
D) Una librería para testear la API.

**Solución:** B

**20. ¿Qué ventaja principal ofrece el uso de la interfaz `JpaRepository` frente a implementar los métodos de acceso a datos manualmente?** 

A) Permite escribir SQL nativo más rápido. 
B) Proporciona automáticamente métodos CRUD estándar (`save`, `findAll`, `deleteById`...) sin necesidad de escribir su implementación. 
C) Elimina la necesidad de tener una base de datos. 
D) Convierte automáticamente la base de datos de MySQL a PostgreSQL.

**Solución:** B