Nombre y apellidos: 

Curso:

### **INSTRUCCIONES GENERALES DEL EXAMEN**

Por favor, lea atentamente las siguientes normas antes de comenzar:
- **Duración:** Disponéis de un tiempo máximo de **1 hora** para la realización de la prueba.
- **Formato:** El examen consta de **30 preguntas tipo test**. Cada pregunta presenta 4 posibles opciones de respuesta, de las cuales **SOLO UNA es correcta**.
- **Sistema de Puntuación:**
	- Cada **3 respuestas incorrectas restan 1 correcta** (cada error penaliza un 33% del valor de un acierto).
	- Las preguntas no contestadas (en blanco) no suman ni restan puntuación.

- **Registro de Respuestas (IMPORTANTE):** Las respuestas definitivas deben marcarse obligatoriamente en la **tabla de respuestas** situada al comienzo del examen. **Cualquier respuesta que no se encuentre trasladada a dicha tabla no será considerada para la calificación**, independientemente de lo marcado en el cuerpo de las preguntas.

![[Pasted image 20260218231156.png]]
## TEMA 2: ACCESO A BASES DE DATOS (JDBC)

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

**2. ¿Cuál es el índice de la primera columna en un `ResultSet` cuando utilizamos los métodos `get...` (ej: `getString(int index)`)?** 

A) 0 
B) 1 
C) -1 
D) Depende de la configuración de la base de datos.

**3. Observe la siguiente línea de conexión. ¿Qué elemento falta o es incorrecto en la estructura de la URL JDBC para PostgreSQL según los ejemplos del tema?** `String url = "jdbc:postgresql:5432/datos1";` 

A) Falta el nombre del host (ej: `//localhost`) antes del puerto. 
B) El protocolo debería ser `jdbc:odbc:postgresql`. 
C) El puerto 5432 es incorrecto para PostgreSQL, debería ser 3306. 
D) Falta especificar el usuario y contraseña dentro de la propia cadena URL obligatoriamente.

**4. Si utilizamos el método `rs.getString("NOMBRE")`, ¿qué característica destaca el texto sobre el nombre de la columna?** 

A) Debe estar escrito estrictamente en mayúsculas. 
B) Debe coincidir exactamente con el nombre en la base de datos (case-sensitive). 
C) Es indiferente si se escribe en mayúsculas o minúsculas. 
D) No se recomienda usar nombres de columnas, solo índices numéricos.


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

**6. ¿Qué interfaz de JDBC se define en el texto como "un portador entre un programa Java y la base de datos" que no está precompilado?** 

A) `DriverManager` 
B) `PreparedStatement` 
C) `Statement` 
D) `Connection`

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

**8. Observe el siguiente código. ¿Qué ocurrirá si la consulta `SELECT` no devuelve ningún resultado?**

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


**9. En la iteración de un `ResultSet`, ¿qué función cumple exactamente el método `rs.next()`?** 

A) Devuelve el siguiente registro y retorna `null` si no hay más. 
B) Mueve el cursor a la siguiente fila y devuelve `true` si dicha fila existe y es válida. 
C) Mueve el cursor a la siguiente fila y devuelve el número de fila actual. 
D) Verifica si hay una siguiente fila sin mover el cursor.

**10. Según el texto, ¿qué desventaja tienen las "Bases de datos incrustadas" frente al uso de una API genérica como JDBC con un gestor independiente?** 

A) Son más lentas en la comunicación. 
B) Requieren cambios profundos en el programa si se necesita conectar a una base de datos distinta. 
C) No soportan SQL. 
D) Solo funcionan en entornos Windows.

## TEMA 4: MAPEO OBJETO-RELACIONAL (HIBERNATE)

**11. En la configuración de Hibernate mediante XML (`hibernate.cfg.xml`), ¿qué propiedad es fundamental para que el framework sepa cómo traducir el HQL a las sentencias SQL específicas de la base de datos que estamos usando (por ejemplo, MySQL o PostgreSQL)?** 

A) `connection.driver_class` 
B) `hibernate.connection.url` 
C) `hibernate.dialect` 
D) `show_sql`

**12. ¿Qué método de la interfaz `Session` se utiliza para recuperar un objeto de la base de datos dado su identificador (Clave Primaria)?** 

A) `session.load(Clase.class, id)` o `session.get(Clase.class, id)` 
B) `session.find(id)` 
C) `session.select(Clase.class, id)` 
D) `session.fetch(id)`

**13. Observe la siguiente relación Uno-a-Muchos en la clase `Autor`. ¿Qué atributo falta en la anotación `@OneToMany` para indicar que la relación es bidireccional y que la clave foránea está gestionada por el otro lado (la clase `Libro`)?**

```Java
@Entity
@Table(name="autores")
public class Autor {
    @Id
    private int id;

    @OneToMany(________ = "autor", cascade = CascadeType.ALL)
    private List<Libro> libros;
}
```

A) `targetEntity` 
B) `mappedBy` 
C) `joinColumn` 
D) `fetch`

**14. Si deseamos borrar un objeto persistente de la base de datos usando Hibernate, ¿cuál es la secuencia correcta de comandos dentro de una transacción?** 

A) `session.remove(objeto);` 
B) `session.delete(objeto);` 
C) `session.erase(objeto);` 
D) `session.drop(objeto);`


**15. Si estamos utilizando Hibernate con ficheros de mapeo XML (`.hbm.xml`), ¿qué etiqueta se utiliza dentro de `<class>` para mapear un atributo simple de tipo `String` o `int` a una columna de la base de datos?** 

A) `<column>` 
B) `<field>` 
C) `<attribute>` 
D) `<property>`

**16. ¿Qué significa el concepto de "Ingeniería Inversa" mencionado en el temario de Hibernate con XML?** 

A) Generar la base de datos a partir de las clases Java. 
B) Generar las clases Java (POJOs) y los ficheros de mapeo (`.hbm.xml`) automáticamente a partir de las tablas de la base de datos existente. 
C) Convertir código XML a anotaciones automáticamente. 
D) Recuperar una transacción fallida.

**17. En la estrategia de generación de claves primarias con anotaciones, ¿qué opción debemos usar en `@GeneratedValue(strategy = ...)` si queremos que la base de datos utilice una columna autoincremental (tipo `AUTO_INCREMENT` en MySQL o `SERIAL` en PostgreSQL)?** 

A) `GenerationType.SEQUENCE` 
B) `GenerationType.AUTO` 
C) `GenerationType.TABLE` 
D) `GenerationType.IDENTITY`

**18. Si definimos una propiedad en el POJO como `int` (primitivo) pero en la base de datos el campo permite nulos, ¿qué problema podríamos tener según las buenas prácticas de mapeo?** 

A) Hibernate lanzará una excepción al iniciar. 
B) Si la base de datos devuelve `null`, Hibernate no podrá asignarlo al `int` primitivo y lanzará un error; se debería usar la clase envoltorio `Integer`. 
C) Hibernate convertirá el `null` a `-1` automáticamente. 
D) No hay ningún problema, `int` soporta nulos en Java.

**19. ¿Qué objeto de Hibernate se crea UNA sola vez durante la inicialización de la aplicación y es el encargado de fabricar las sesiones?** 

A) `Session` 
B) `Transaction` 
C) `Configuration` 
D) `SessionFactory`

**20. ¿Qué anotación se utiliza para definir que un atributo no debe ser persistido en la base de datos (es decir, que Hibernate debe ignorarlo)?** 

A) `@Ignored` 
B) `@Transient` 
C) `@NotMapped` 
D) `@Temporary`

## TEMA 6: COMPONENTES (SPRING)

**21. En la definición de la interfaz de un repositorio con Spring Data JPA, ¿qué línea completa correctamente el siguiente código para gestionar la entidad `Evento` cuya clave primaria es de tipo `Integer`?**

```Java
@Repository
public interface EventoRepository extends __________________ {
}
```

A) `CrudRepository<Evento>` 
B) `JpaRepository<Integer, Evento>` 
C) `JpaRepository<Evento, Integer>` 
D) `Repository<Evento>`

**22. Analice el siguiente código del `UsuarioService` para actualizar un usuario. ¿Qué lógica fundamental falta en el espacio marcado?**

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

**23. En la arquitectura de tres capas implementada en el tema, ¿cuál es la responsabilidad principal de la clase anotada con `@Service`?** 

A) Gestionar las peticiones HTTP y devolver códigos de estado (200, 404). 
B) Definir la conexión directa con la base de datos y las credenciales. 
C) Encapsular la lógica de negocio y orquestar las llamadas al repositorio. 
D) Mapear los JSON a objetos Java automáticamente.

**24. ¿Qué devuelve el método `usuarioRepository.findById(id)` por defecto (método estándar de `JpaRepository`)?** 

A) Un objeto `Usuario` directamente (o `null` si no existe). 
B) Un `Optional<Usuario>`, obligando a gestionar la posible ausencia del valor. 
C) Una `List<Usuario>` con un solo elemento. 
D) Un `ResponseEntity` con el usuario.

**25. Observe el siguiente método en el Controlador. ¿Qué anotación falta para que Spring inyecte el valor de `{id}` de la URL en el parámetro del método?**

```Java
@GetMapping("/{id}")
public ResponseEntity<Usuario> getUsuarioById(@________ Integer id) {
    // ...
}
```

A) `RequestParam` 
B) `PathVariable` 
C) `PathParam` 
D) `BodyParam`

**26. ¿Qué dependencia de Maven es la encargada de incluir todas las bibliotecas necesarias para trabajar con JPA, Hibernate y la conexión a base de datos en Spring Boot?** 

A) `spring-boot-starter-web` 
B) `spring-boot-starter-security` 
C) `spring-boot-starter-data-jpa` 
D) `spring-boot-starter-thymeleaf`

**27. Si queremos implementar un método en `EventoRepository` que busque eventos cuyo precio sea mayor que cierto valor, utilizando la funcionalidad de "Derived Query Methods" de Spring Data, ¿cuál sería el nombre correcto del método?** 

A) `findEventsWherePriceIsBig(double precio)` 
B) `getAllByPrecio(double precio)` 
C) `findByPrecioGreaterThan(double precio)` 
D) `queryPrecioMayorQue(double precio)`

**28. Si intentamos guardar un objeto usando `usuarioRepository.save(usuario)`, ¿cómo sabe Hibernate si debe ejecutar un `INSERT` o un `UPDATE`?** 

A) Por el nombre del método (save vs update). 
B) Hibernate siempre hace INSERT y si falla hace UPDATE. 
C) Comprobando si el campo `@Id` del objeto es nulo (o no existe en BBDD) para insertar, o si tiene un valor existente para actualizar. 
D) Se debe especificar manualmente con un parámetro booleano.

**29. ¿Qué función cumple la anotación `@Autowired` cuando se coloca sobre el atributo `private UsuarioRepository usuarioRepository;` dentro de una clase Service o Controller?** 

A) Indica que ese atributo es una clave primaria foránea. 
B) Habilita la inyección de dependencias, permitiendo que Spring instancie y asigne automáticamente el repositorio. 
C) Crea una nueva tabla en la base de datos llamada `usuario_repository`. 
D) Obliga a que el repositorio sea serializable.

**30. ¿Qué es Spring Initializr (start.spring.io) según se describe en el tema?** 

A) Un IDE específico para programar en Spring. 
B) Una herramienta web para generar la estructura inicial de un proyecto Spring Boot con las dependencias seleccionadas. 
C) El gestor de base de datos embebido de Spring. 
D) Una librería para testear la API.