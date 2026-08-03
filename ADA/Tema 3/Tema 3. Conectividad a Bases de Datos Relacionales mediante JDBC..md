## Arquitectura JDBC: Introducción y conceptos clave

Hasta ahora hemos persistido la información en ficheros físicos (texto, binarios, XML y JSON). Sin embargo, en el entorno empresarial, el almacenamiento y gestión de grandes volúmenes de información estructurada recae en los Sistemas Gestores de Bases de Datos Relacionales (SGBDR) como MySQL, PostgreSQL u Oracle.

Para que una aplicación Java pueda comunicarse con cualquiera de estas bases de datos, utiliza **JDBC** (_Java Database Connectivity_). JDBC es una API estándar de Java que actúa como un "puente" universal. Su gran ventaja es que permite escribir el código Java una sola vez; si el día de mañana la empresa decide cambiar de MySQL a PostgreSQL, el código fuente de nuestra aplicación (las sentencias de consulta y actualización) se mantendrá prácticamente inalterado.

**Componentes de la Arquitectura JDBC**
La arquitectura se divide en dos capas principales:

1. **API JDBC:** Proporcionada por el propio JDK de Java (paquete `java.sql`). Contiene las interfaces estándar (`Connection`, `Statement`, `ResultSet`).
2. **Driver JDBC:** Es el software específico (creado por el fabricante de la base de datos) que traduce las llamadas estándar de Java al protocolo propietario de la base de datos concreta.

### Carga de drivers y establecimiento de conexiones

Existen distintas formas de conectar a gestores de bases de datos, como por ejemplo:

- Bases de datos incrustadas (“**embedded**”), en las que la conexión entre el programa y la base de datos es directa. Puede permitir la mayor velocidad en la comunicación, pero supone hacer cambios (quizá profundos) en el programa en caso de necesitar conectar a una base de datos distinta, lo que no es deseable.
- Como alternativa, es preferible usar un API (interfaz de programación de aplicaciones) genérico, que permita conectar de la misma forma a distintos gestores de bases de datos, así como enviar peticiones y recibir las tuplas con las respuestas de una manera uniforme. Uno de los estándares más extendidos es **ODBC** (Open DataBase Connectivity), usado habitualmente en C, C++, C# y Visual Basic.
- ODBC es poco adecuado para ser usado desde Java, porque se basa en llamadas en lenguaje C (lo que implica uso de punteros, que no existen en Java), y la conversión puede suponer riesgos de seguridad y de falta robustez. Por eso, en Java se suele emplear una alternativa totalmente basada en objetos, llamada **JDBC** (Java Database Connectivity).

En la mayoría de casos, se deberá instalar un “**driver**” que permita acceder desde el lenguaje o herramienta que se está utilizando al gestor de bases de datos deseados. En el mundo Java es habitual llamar también **conector** a estos drivers. En aplicaciones modernas, esta conexión se hace añadiendo la dependencia exacta en el archivo `pom.xml` de **Maven** (por ejemplo, `mysql-connector-j` para MySQL o `postgresql` para PostgreSQL).

**La cadena de conexión (URL JDBC)**
Para indicar a Java dónde está la base de datos, utilizamos una cadena de texto estructurada llamada URL de conexión. Su sintaxis general es: `jdbc:<subprotocolo>://<servidor>:<puerto>/<nombre_bd>`

Ejemplo:
- **PostgreSQL:** `jdbc:postgresql://localhost:5432/empresa`

**Estableciendo la conexión**
La clase fundamental para este proceso es `DriverManager`. Esta clase se encarga de buscar entre los drivers que hayamos importado vía Maven cuál es el adecuado para la URL que le pasamos.

> **Nota histórica:** En versiones antiguas de Java (anteriores a JDBC 4.0), era obligatorio "cargar" el driver manualmente en memoria usando `Class.forName("com.mysql.cj.jdbc.Driver");`. Hoy en día, gracias al mecanismo SPI (_Service Provider Interface_), `DriverManager` detecta y carga el driver automáticamente con solo tener la dependencia en el proyecto.

A continuación, se muestra el código estándar y robusto para abrir una conexión. Es crucial utilizar la estructura `try-with-resources` para garantizar que la conexión se cierre automáticamente y no dejemos sesiones "huérfanas" bloqueando el servidor.

``` Java
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.SQLException;

public class EjemploConexionBasica {
    public static void main(String[] args) {
        // 1. Definir los credenciales y la URL
        String url = "jdbc:postgresql://localhost:5432/instituto";
        String usuario = "postgres";
        String password = "password123";

        // 2. Establecer la conexión usando try-with-resources
        try (Connection conexion = DriverManager.getConnection(url, usuario, password)) {
            
            if (conexion != null) {
                System.out.println("¡Conexión establecida correctamente con la Base de Datos!");
                // Aquí iría la lógica de nuestra aplicación (consultas, inserciones...)
            }
            
        } catch (SQLException e) {
            System.err.println("Error al intentar conectar con la base de datos.");
            System.err.println("Código de estado SQL: " + e.getSQLState());
            System.err.println("Mensaje: " + e.getMessage());
        }
    }
}
```

>[!NOTE] Ejercicio 1
Prepara tu entorno de trabajo: crea un nuevo proyecto Maven en tu IDE y añade la dependencia del conector JDBC para PostgreSQL. A continuación, arranca tu servidor de bases de datos local, crea una base de datos vacía llamada `instituto` y escribe un programa en Java que establezca la conexión. Muestra un mensaje de éxito por consola si la conexión se ha realizado.

>[!NOTE] Ejercicio 2
Modifica el programa anterior para simular un fallo en la conexión (por ejemplo, cambiando la contraseña por una incorrecta o cambiando el puerto en la URL a uno inexistente). Ejecuta el programa y analiza la excepción `SQLException` que captura el bloque `catch`. ¿Qué mensaje de error (`e.getMessage()`) te devuelve el driver en cada caso?

## Ejecución de sentencias SQL y Gestión de resultados

Una vez que hemos establecido un objeto `Connection` válido con nuestra base de datos, el siguiente paso lógico es enviarle órdenes (consultas, inserciones, actualizaciones o borrados). Para ello, la API de JDBC nos proporciona tres interfaces principales de ejecución, dependiendo de la naturaleza de la orden y de si contiene parámetros dinámicos o no.

Independientemente de la interfaz que utilicemos, JDBC clasifica las operaciones en dos grandes grupos, utilizando métodos distintos para cada uno:

1. **`executeQuery(String sql)`**: Se utiliza **exclusivamente** para sentencias `SELECT`. Devuelve siempre un objeto `ResultSet` que contiene los datos recuperados.
2. **`executeUpdate(String sql)`**: Se utiliza para sentencias que modifican los datos o la estructura (`INSERT`, `UPDATE`, `DELETE`, `CREATE TABLE`...). Devuelve un valor numérico entero (`int`) que indica el número de filas que se han visto afectadas por la operación.
    

### La interfaz genérica: Statement

La interfaz `Statement` es la forma más básica de enviar una instrucción SQL estática a la base de datos. Es útil para consultas simples donde no intervienen variables introducidas por el usuario, como la creación inicial de tablas o listados completos inmutables.

 **Gestión de resultados: La interfaz ResultSet**
Cuando ejecutamos un `SELECT` mediante un `Statement`, el motor de PostgreSQL nos devuelve un conjunto de registros. JDBC encapsula esa "tabla" de resultados en un objeto llamado `ResultSet`.

El `ResultSet` funciona internamente con un cursor que inicialmente apunta a una posición anterior a la primera fila. Para leer los datos, debemos invocar a su método `next()`, el cual avanza el cursor a la siguiente fila y devuelve `true` si hay datos, o `false` si hemos llegado al final.

**Ejemplo de uso de Statement y ResultSet:**

```Java
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.ResultSet;
import java.sql.Statement;

public class EjemploStatement {
    public static void main(String[] args) {
        String url = "jdbc:postgresql://localhost:5432/instituto";
        
        try (Connection conexion = DriverManager.getConnection(url, "postgres", "password123")) {
            
            // 1. Crear el objeto Statement a partir de la conexión
            Statement statement = conexion.createStatement();
            
            // 2. Ejecutar una consulta SELECT
            String sql = "SELECT id_alumno, nombre, curso FROM alumnos";
            ResultSet resultados = statement.executeQuery(sql);
            
            // 3. Iterar sobre el ResultSet
            while (resultados.next()) {
                // Recuperar los datos según su tipo y el nombre de la columna en PostgreSQL
                int id = resultados.getInt("id_alumno");
                String nombre = resultados.getString("nombre");
                String curso = resultados.getString("curso");
                
                System.out.println("ID: " + id + " | Nombre: " + nombre + " | Curso: " + curso);
            }
            
            // Es buena práctica cerrar el ResultSet y el Statement, aunque el try-with-resources
            // de la Connection principal suele cerrarlos en cascada.
            resultados.close();
            statement.close();
            
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```

### Consultas parametrizadas: PreparedStatement

Si utilizamos la interfaz básica `Statement` concatenando cadenas de texto para incluir variables del usuario (`"SELECT * FROM alumnos WHERE nombre = '" + nombreUsuario + "'"`), nuestra aplicación será vulnerable al ataque informático más común en bases de datos: la **Inyección SQL**.

**Statement** es una interfaz JDBC que se usa para el acceso de propósito general a la base de datos SQL, se utiliza para ejecutar sentencias SQL estándar como CREATE, RETRIEVE, UPDATE y DELETE (CRUD). Funciona como un portador entre un programa Java y la base de datos, pero no puede aceptar parámetros en tiempo de ejecución y no está precompilado, lo que significa que la misma instrucción SQL no se puede usar varias veces, lo que representa un rendimiento más lento que **PreparedStatement**. Además, es más propenso a la inyección de SQL.

Para evitar esto, el estándar profesional exige utilizar la interfaz `PreparedStatement`. Esta interfaz precompila la consulta en la base de datos y utiliza comodines representados por el símbolo de cierre de interrogación (`?`). Los parámetros se inyectan de forma segura _después_ de que la estructura SQL haya sido validada, lo que supone una mejora de rendimiento e imposibilita la inyección de código malicioso.

**Ejemplo de uso de PreparedStatement (Inserción de datos):**

```Java
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.PreparedStatement;

public class EjemploPreparedStatement {
    public static void main(String[] args) {
        String url = "jdbc:postgresql://localhost:5432/instituto";
        
        try (Connection conexion = DriverManager.getConnection(url, "postgres", "password123")) {
            
            // 1. Escribir la sentencia SQL con comodines (?)
            String sql = "INSERT INTO alumnos (nombre, curso, edad) VALUES (?, ?, ?)";
            
            // 2. Preparar la sentencia
            PreparedStatement pstmt = conexion.prepareStatement(sql);
            
            // 3. Asignar valores a los comodines (el índice empieza en 1, no en 0)
            pstmt.setString(1, "Laura Gómez");
            pstmt.setString(2, "2º DAM");
            pstmt.setInt(3, 20);
            
            // 4. Ejecutar la actualización
            int filasAfectadas = pstmt.executeUpdate();
            System.out.println("Se ha insertado " + filasAfectadas + " registro(s) correctamente.");
            
            pstmt.close();
            
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```

![[image1_StatementVsPrepared.jpg]]

Por último, alguno de los métodos más importantes de PreparedStatement son los siguientes:

| **Método**                                          | **Descripción**                                                                          |
| --------------------------------------------------- | ---------------------------------------------------------------------------------------- |
| public void setInt(int paramIndex, int value)       | Establece el valor entero en el índice del parámetro dado.                               |
| public void setString(int paramIndex, String value) | Establece el valor de la cadena al índice del parámetro dado.                            |
| public void setFloat(int paramIndex, float value)   | Establece el valor `float` en el índice del parámetro dado.                              |
| public void setDouble(int paramIndex, double value) | Establece el valor double al índice del parámetro dado.                                  |
| public int executeUpdate()                          | Ejecuta la consulta. Se utiliza para crear, eliminar, insertar, actualizar, borrar, etc. |
| public ResultSet executeQuery()                     | Ejecuta la consulta select. Devuelve una instancia de ResultSet.                         |

### Ejecución de procedimientos: CallableStatement

PostgreSQL, al igual que Oracle y que (con más limitaciones) MySQL, permite definir funciones usando su lenguaje de programación propio, PL/pgSQL. El formato básico de una función será:

```PLSQL
CREATE [OR REPLACE] FUNCTION function_name (arguments) RETURNS return_datatype AS $variable_name$
	DECLARE
		declaration;
		[...]
	BEGIN
		< function_body > 
		[...]
		RETURN { variable_name | value } 
		END; 
LANGUAGE plpgsql;
```

A este tipo de funciones se les suele llamar también “procedimientos almacenados” (Stored Procedures) y se pueden llamar luego desde otros lenguajes en el equipo cliente. De hecho, es una de las formas de trabajar que se suelen recomendar para evitar ataques de “inyección de SQL”.

Como ejemplo de función que reciba un parámetro y que devuelva un valor, podemos crear una que devuelva la cantidad de datos que hay en la tabla “alumnos” cuyo nombre sigue un cierto patrón:

```PLSQL
CREATE OR REPLACE FUNCTION CantidadAlumnos (comodin VARCHAR) RETURNS INTEGER
AS
$$
	DECLARE
	    cantidad INTEGER;
	BEGIN
	    SELECT COUNT(*) INTO cantidad FROM alumnos WHERE nombre LIKE comodin; 
	    RETURN cantidad;
		END;
$$
LANGUAGE plpgsql;
```

Ahora es posible crear un programa en Java que llame a esa función, que le pase parámetros y que obtenga sus resultados. Para ello, emplearemos un “CallableStatement”. Esta tercera interfaz se utiliza exclusivamente cuando necesitamos invocar funciones o procedimientos almacenados que han sido programados previamente dentro del propio motor de la base de datos. Permite tanto enviar parámetros de entrada (`IN`) como recuperar parámetros de salida (`OUT`).

```Java
public class EjemploDeStoredProcedure {

	public static void main(String[] args) throws ClassNotFoundException, SQLException{
		Class.forName("org.postgresql.Driver");
		String url = "jdbc:postgresql://localhost:5432/instituto"; 
		String usuario = "postgres";
		String password = "password123";

		Connection con = DriverManager.getConnection(url, usuario, password);

		CallableStatement cStmt = con.prepareCall( "{call CantidadAlumnos(?)}");

		cStmt.registerOutParameter(1, Types.INTEGER); 
		cStmt.setString(1, "Lau%");

		cStmt.execute();
		int resultado = cStmt.getInt(1); 
		System.out.println("Resultado: " + resultado); 
		con.close();
	}
}
```

>[!NOTE] Ejercicio 3
Continuando con la base de datos `instituto` y la conexión que configuraste en el Ejercicio 1, crea una nueva clase Java que utilice un objeto `Statement` simple para ejecutar una sentencia `CREATE TABLE` que construya la tabla `alumnos`. La tabla debe contener los campos `id_alumno` (como clave primaria autoincremental de tipo `SERIAL` en PostgreSQL), `nombre` (texto), `apellidos` (texto) y `edad` (entero).

>[!NOTE] Ejercicio 4
Desarrolla un programa en Java que pida al usuario por teclado (utilizando la clase `Scanner`) los datos de un alumno (nombre, apellidos y edad). Utilizando `PreparedStatement`, inserta esos datos de forma segura en la tabla `alumnos` creada en el ejercicio anterior. Realiza varias ejecuciones del programa para insertar al menos tres alumnos.

>[!NOTE] Ejercicio 5
Crea una clase que se conecte a la base de datos y utilice un `Statement` para recuperar y mostrar por consola todos los registros de la tabla `alumnos`. Al final del listado, el programa debe imprimir una línea que indique el número total de alumnos que han sido recuperados (deberás usar un contador dentro del bucle de recorrido del `ResultSet`).

>[!NOTE] Ejercicio 6
>Abre tu cliente de base de datos PostgreSQL (como pgAdmin o DBeaver) y ejecuta el _script_ PL/pgSQL proporcionado arriba para compilar y guardar la función `CantidadAlumnos` en tu base de datos `instituto`. A continuación, implementa la clase `EjemploCallableStatement` en tu proyecto Java y pruébala buscando cuántos alumnos tienen un nombre que contenga la letra "a" en cualquier posición.

## Gestión de transacciones y control de concurrencia

Hasta el momento, todas las instrucciones SQL de inserción, actualización o borrado que hemos enviado a la base de datos se han ejecutado de forma inmediata e irreversible. Sin embargo, en aplicaciones reales, a menudo necesitamos que un bloque de varias sentencias se ejecute como un todo: o se ejecutan todas correctamente, o no se ejecuta ninguna.

Este concepto se conoce como **Transacción**. Una transacción garantiza la propiedad de atomicidad (la "A" de las famosas propiedades ACID de las bases de datos).

### El modo Auto-Commit

Por defecto, cuando abrimos una conexión en JDBC, esta se encuentra en modo _Auto-Commit_ (Autoconfirmación). Esto significa que cada sentencia SQL individual se trata como una transacción independiente y se guarda automáticamente en el disco en cuanto termina de ejecutarse.

Para agrupar varias sentencias en una única transacción, debemos desactivar este comportamiento predeterminado, ejecutar nuestras sentencias y, finalmente, indicar explícitamente a PostgreSQL que queremos confirmar los cambios.

#### Controlando la transacción: Commit y Rollback

Los tres métodos fundamentales de la interfaz `Connection` para gestionar transacciones son:

1. **`setAutoCommit(false)`:** Inicia la transacción manual. A partir de este momento, los cambios se realizan en memoria, pero no son definitivos en la base de datos.
2. **`commit()`:** Confirma la transacción. Toma todos los cambios pendientes y los consolida físicamente en la base de datos de forma permanente.
3. **`rollback()`:** Deshace la transacción. Si ocurre algún error (se lanza una excepción) durante el proceso, este método descarta todos los cambios que se habían intentado hacer desde el último commit, dejando la base de datos en su estado original.

Tras una modificación de este tipo, es buena práctica restaurar el estado original de la conexión mediante setAutoCommit(true).

**Ejemplo de uso: Transacción segura**

Vamos a imaginar un escenario en nuestro instituto: queremos dar de baja a un alumno de la tabla `alumnos` y, simultáneamente, registrarlo en una nueva tabla llamada `ex_alumnos`. Si el borrado funciona, pero la inserción en la otra tabla falla por algún motivo, perderíamos la información del alumno. La transacción evita este desastre.

```Java
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.PreparedStatement;
import java.sql.SQLException;

public class EjemploTransaccion {
    public static void main(String[] args) {
        String url = "jdbc:postgresql://localhost:5432/instituto";
        
        // Declaramos la conexión fuera del try-with-resources si necesitamos hacer rollback en el catch,
        // o la mantenemos dentro si manejamos el catch en su interior.
        try (Connection conexion = DriverManager.getConnection(url, "postgres", "password123")) {
            
            // 1. Desactivar el auto-commit para iniciar la transacción
            conexion.setAutoCommit(false);
            
            String sqlBaja = "DELETE FROM alumnos WHERE id_alumno = ?";
            String sqlHistorico = "INSERT INTO ex_alumnos (nombre, motivo_baja) VALUES (?, ?)";
            
            try (PreparedStatement pstmtBaja = conexion.prepareStatement(sqlBaja);
                 PreparedStatement pstmtHistorico = conexion.prepareStatement(sqlHistorico)) {
                
                // Operación 1: Borrar al alumno
                pstmtBaja.setInt(1, 5); // Supongamos el alumno con ID 5
                pstmtBaja.executeUpdate();
                
                // Simulamos un error inesperado (como un fallo de red o un error de sintaxis)
                // int divisionCero = 10 / 0; 
                
                // Operación 2: Insertarlo en el histórico
                pstmtHistorico.setString(1, "Carlos Martínez");
                pstmtHistorico.setString(2, "Traslado de expediente");
                pstmtHistorico.executeUpdate();
                
                // 2. Si ambas operaciones tienen éxito, confirmamos la transacción
                conexion.commit();
                System.out.println("Transacción completada: Alumno trasladado al histórico correctamente.");
                
            } catch (Exception ex) {
                // 3. Si ocurre CUALQUIER error en el bloque try, deshacemos todos los cambios
                System.err.println("Error durante la operación. Deshaciendo cambios (Rollback)...");
                conexion.rollback();
                ex.printStackTrace();
            } finally {
                // Es buena práctica restaurar el estado original de la conexión
                conexion.setAutoCommit(true);
            }
            
        } catch (SQLException e) {
            e.printStackTrace();
        }
    }
}
```

## Buenas prácticas y errores comunes en JDBC

**Buenas prácticas de programación y arquitectura**

- **Prioridad absoluta a `PreparedStatement`:** Salvo para operaciones de administración muy específicas (como crear tablas al inicio), se debe evitar el uso de `Statement`. Utilizar `PreparedStatement` por las ventajas estudiadas en la presente unidad.
- **Cierre estricto de recursos:** Todo objeto que interactúe con la base de datos (`Connection`, `Statement`, `PreparedStatement`, `ResultSet`...) consume recursos físicos de red y memoria. Deben cerrarse obligatoriamente utilizando sentencias close o bloques `try-with-resources`.
- **Externalización de credenciales:** Al contrario de lo que hemos visto en los ejemplos de la unidad, las contraseñas, usuarios y URLs de conexión nunca deben estar escritas a fuego (_hardcodeadas_) en las clases Java. Se deben extraer a ficheros de configuración externos o leerse desde variables de entorno.

**Errores comunes**

- **Índices basados en 1, no en 0:** Es el fallo de programación más habitual al empezar con JDBC. Mientras que los arrays y colecciones en Java empiezan en el índice 0, los comodines `?` de un `PreparedStatement` y las columnas numéricas de un `ResultSet` **empiezan siempre en 1**.
- **Olvidar avanzar el cursor del ResultSet:** Intentar extraer un dato ejecutando `resultados.getString("nombre")` inmediatamente después de la consulta lanzará una excepción indicando que el cursor no está posicionado. Siempre es obligatorio invocar `resultados.next()` (generalmente dentro de un `while` o un `if`) antes de leer la primera fila.
- **Fuga de conexiones (_Connection Leak_):** Ocurre cuando se abren conexiones dentro de un bucle o no se cierran adecuadamente en un bloque `finally` (o usando `try-with-resources`). Esto agota el límite máximo de conexiones simultáneas permitidas por PostgreSQL, provocando que la aplicación se quede congelada esperando una conexión libre que nunca llegará.
- **Concatenación letal:** Escribir código como `String sql = "SELECT * FROM alumnos WHERE nombre = '" + variable + "'";` en lugar de usar parámetros `?`. Esto permite a un usuario malintencionado introducir en el formulario web valores como `' OR '1'='1`, saltándose la validación y exponiendo toda la tabla.
- **Captura genérica de excepciones:** Tratar las caídas de base de datos capturando un genérico `Exception` en lugar de `SQLException`. La clase `SQLException` proporciona métodos vitales para la depuración, como `getSQLState()` (que devuelve el código de error estándar de PostgreSQL) y `getErrorCode()`, los cuales se pierden si no se captura la excepción correcta.