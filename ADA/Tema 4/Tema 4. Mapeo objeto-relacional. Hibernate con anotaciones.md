--- title: "Mapeo objeto-relacional. Hibernate con anotaciones" ---
## 1.  Anotaciones en Hibernate.

El formato XML tiene muchos detractores, que lo consideran “demasiado farragoso” y poco legible en documentos de un cierto tamaño. En ciertos entornos se prefieren notaciones más compactas, como JSON. En Java, en ciertos casos se pueden emplear “anotaciones”, etiquetas especiales precedidas por el símbolo de la arroba (@). Hibernate permite también usar anotaciones en vez de ficheros XML, de hecho es lo más común cuando se trabaja con Hibernate-Spring.

Para ver un ejemplo, vamos a crear una nueva base de datos BdAnotacionesHibernate:
```SQL
CREATE TABLE autores (
	cod VARCHAR(5) PRIMARY KEY,
	nombre VARCHAR(60)
);
```

Y una tabla “libros”, que contendrá el título, una clave primaria numérica, y el código del autor como forma de enlazar con la otra tabla para representar esa relación “uno a muchos”.

```SQL
CREATE TABLE libros (
	id INTEGER PRIMARY KEY,
	titulo VARCHAR(60),
	codautor VARCHAR(5) REFERENCES autores(cod)
);
```


Y añadimos un par de datos de ejemplo en cada tabla:

```SQL
INSERT INTO autores VALUES ('WSHAK', 'William Shakespeare'), ('FROJA', 'Fernando de Rojas');

INSERT INTO libros VALUES (1, 'Macbeth', 'WSHAK'),
(2, 'La Celestina (Tragicomedia de Calisto y Melibea)', 'FROJA');
```

Ahora, deberemos crear un nuevo proyecto (HibernateAnotaciones1, por ejemplo), recuerda adecuar la versión de Java a tu entorno:


A continuación se detalla el procedimiento estándar para la creación y configuración de un proyecto Java utilizando el framework **Hibernate ORM** bajo el enfoque de **anotaciones (JPA)**. Este procedimiento es agnóstico del Entorno de Desarrollo Integrado (IDE) utilizado, basándose en la gestión de dependencias mediante Maven.

## a. Inicialización del Proyecto

Se debe generar un nuevo proyecto **Maven** estándar (arquetipo quickstart o proyecto vacío). En esta guía evitaremos el uso de asistentes de configuración específicos de un IDE (como JBoss Tools en Eclipse) para garantizar la portabilidad del código y la comprensión de la arquitectura subyacente.

La estructura de directorios resultante debe respetar el estándar Maven:

- src/main/java: Para el código fuente Java.
- src/main/resources: Para los ficheros de configuración (xml, properties).

## b. Gestión de Dependencias (pom.xml)

Para utilizar las versiones modernas de Hibernate (6.x o superior), es necesario definir las dependencias correspondientes en el archivo pom.xml. Estas versiones implementan la especificación **Jakarta Persistence** (sustituta de la antigua javax.persistence).

A continuación, se detalla la configuración requerida:

```XML
(...)
<dependencies>
        <dependency>
            <groupId>org.hibernate.orm</groupId>
            <artifactId>hibernate-core</artifactId>
            <version>6.4.4.Final</version>
        </dependency>
        <dependency>
            <groupId>org.postgresql</groupId>
            <artifactId>postgresql</artifactId>
            <version>42.7.2</version>
        </dependency>
...)
```

## c. Fichero de Configuración (hibernate.cfg.xml)

La configuración de la conexión a la base de datos y el dialecto SQL se centraliza en el archivo hibernate.cfg.xml. Este archivo debe crearse manualmente y ubicarse obligatoriamente en la ruta src/main/resources.

```XML
<!DOCTYPE hibernate-configuration PUBLIC
        "-//Hibernate/Hibernate Configuration DTD 3.0//EN"
        "http://www.hibernate.org/dtd/hibernate-configuration-3.0.dtd">

<hibernate-configuration>
    <session-factory>
        <property name="connection.driver_class">org.postgresql.Driver</property>
        <property name="connection.url">jdbc:postgresql://localhost:5432/BdAnotacionesHibernate</property>
        <property name="connection.username">postgres</property>
        <property name="connection.password">1234</property>
        <property name="dialect">org.hibernate.dialect.PostgreSQLDialect</property>
        <property name="hibernate.default_schema">public</property>
    </session-factory>
</hibernate-configuration>
```

## 2.  Definición de Entidades con Anotaciones

En el modelo basado en anotaciones, la metainformación de mapeo se incluye directamente en la clase Java, eliminando la necesidad de archivos .hbm.xml auxiliares.

**Nota Importante**: Para Hibernate 6+, las importaciones deben provenir del paquete **jakarta.persistence.***. El uso del antiguo paquete javax.persistence provocará errores de compilación o ejecución.

Crearemos por tanto en este punto a mano las clases Java que corresponden a los Modelos de nuestra base de datos, implementando ya las anotaciones que se explicarán a continuación:

```java
package com.AccesoADatos.Ev2.HibernateAnotaciones1.modelos;

import java.util.HashSet;
import java.util.Set;

import jakarta.persistence.Column;
import jakarta.persistence.Entity;
import jakarta.persistence.FetchType;
import jakarta.persistence.Id;
import jakarta.persistence.OneToMany;
import jakarta.persistence.Table;

@Entity
@Table(name = "autores")
public class Autores implements java.io.Serializable {
	
	private static final long serialVersionUID = 1L;
	private String cod;
	private String nombre;
	private Set<Libros> libroses = new HashSet<Libros>(0);
	
	public Autores() {
	}
	
	public Autores(String cod) {
		this.cod = cod;
	}
	
	public Autores(String cod, String nombre, Set<Libros> libroses) {
		this.cod = cod;
		this.nombre = nombre;
		this.libroses = libroses;
	}
	
	@Id
	@Column(name = "cod", unique = true, nullable = false, length = 5 )
	public String getCod() {
		return this.cod;
	}
	
	public void setCod(String cod) {
		this.cod = cod;
	}
	
	@Column(name = "nombre", length = 60)
	public String getNombre() {
		return this.nombre;
	}
	
	public void setNombre(String nombre) {
		this.nombre = nombre;
	}
	
	@OneToMany(fetch = FetchType.LAZY, mappedBy = "autores")
	public Set<Libros> getLibroses() {
		return this.libroses;
	}
	
	public void setLibroses(Set<Libros> libroses) {
		this.libroses = libroses;
	}

}

```  


Analizaremos las diferentes etiquetas para que las entendamos:

```Java
@Entity
@Table(name = "autores")
public class Autores implements java.io.Serializable{
//...
}
```
  
Significa que la clase “Autores” en Java estará enlazada con la tabla “autores” de PostgreSql, la etiqueta @Entity indica que se corresponde a una entidad de la base de datos y con la etiqueta @Table le indico el nombre de la tabla (que podría llamarse diferente a la clase de Java). Si no ponemos la etiqueta @Table, Hibernate buscará una tabla en la base de datos que se llame igual que la clase en minúsculas por lo que en este caso como existe no es imprescindible esa etiqueta ya que hubiera buscado ese nombre por defecto.

>[!NOTE] Recordatorio:
Importante llamar a las **tablas en postgres en minúsculas**.


```Java
@Id
@Column(name="cod", unique = true, nullable = false, length = 5)
public String getCod(){
	return this.cod;
}
```

Observamos que delante de los get y set de cada atributo de la clase aparece @Column para indicar que es una columna de la base de datos y las características que tiene son las mismas que la columna de la base de datos. Además, en el atributo que representa la clave primaria también aparece @Id indicándolo.

Pero lo que realmente le da una potencia enorme a los ORM como Hibernate es la siguiente etiqueta:

```Java
@OneToMeny(fetch = FetchType.LAZY, mappedBy = "autores")
public Set<Libros> getLibroses() {
	return this.libroses;
}
```

Esto nos indica que Autores tiene una relación 1:M con Libros (ya que el Set es de la clase Libros), que está mapeado por el atributo “autores” de la clase Libros, es decir que la clase Java Libros tiene un campo que es la clave ajena que se llamará “autores” y que desde esta clase Autores puedo acceder directamente a todos los libros de un determinado autor. Ahí también tiene importancia el tipo de fetch que, por defecto, es de tipo “Lazy” que indica que la obtención de los libros de un determinado autor se producirá cuando hagamos uso de ellos, no antes. Esto último mejora la eficiencia a la hora de coger datos ya que no lo hace hasta que no los necesita pero pierde velocidad al no tenerlos disponibles a priori.

Si quieres aprender más sobre los tipos de fetch puedes consultar el siguiente link:

[http://www.davidmarco.es/articulo/lazy-fetch-en-jpa](http://www.davidmarco.es/articulo/lazy-fetch-en-jpa)

Como concepto, quédate que en esta relación 1:M entre autor y libros puedes acceder directamente desde un autor a todos sus libros, cosa que haremos en los ejemplos.

Veamos cómo quedaría la clase Libros:

```java

package com.AccesoADatos.Ev2.HibernateAnotaciones1.modelos;

import jakarta.persistence.Column;
import jakarta.persistence.Entity;
import jakarta.persistence.FetchType;
import jakarta.persistence.Id;
import jakarta.persistence.JoinColumn;
import jakarta.persistence.ManyToOne;
import jakarta.persistence.Table;

@Entity
@Table(name = "libros")
public class Libros implements java.io.Serializable {
	
	private static final long serialVersionUID = 1L;
	
	@Id
	@Column(name = "id", unique = true, nullable = false)
	private int id;
	
	@ManyToOne(fetch = FetchType.LAZY)
	@JoinColumn(name = "codautor")
    private Autores autores;
	
	@Column(name = "titulo", length = 60)
    private String titulo;

	public Libros() {
    }
	
	public Libros(int id) {
		this.id = id;
	}
	
    public Libros(int id, Autores autores, String titulo) {
        this.id = id;
        this.autores = autores;
        this.titulo = titulo;
    }

    // Getters y Setters
    public int getId() { return id; }
    public void setId(int id) { this.id = id; }
    public String getTitulo() { return titulo; }
    public void setTitulo(String titulo) { this.titulo = titulo; }
    public Autores getAutores() { return autores; }
    public void setAutores(Autores autores) { this.autores = autores; }
    
}
```

Como cambio significativo podemos agrupar todas las etiquetas en la parte de declaración de los atributos de la clase.

_(En el caso de generar las clases automáticamente mediante ingeniería inversa…)_ El sistema automático nos pone las etiquetas delante de los getters y setters, pero a mí personalmente (y a mucha gente) me gusta más organizarlas en la parte de creación de los atributos, así sabes que todo está al principio de la clase y es más fácil detectar los errores.

Más consideraciones sobre estas etiquetas, que cada vez controlaremos más con el paso de los ejercicios ya que en Spring solo usaremos el sistema de anotaciones, podrían ser que el campo name en las columnas no es obligatorio si el nombre del campo en la tabla de la base de datos se llama igual.

Y como nota importante, aquí encontramos la parte del Muchos de la relación 1:M donde indicamos que el atributo autores se corresponde con el campo codautor de la base de datos y recuerda que era el atributo que referenciábamos desde la clase Autores.

Por último que no se nos olvide añadir los “mapping” del fichero de configuración básica de Hibernate, “hibernate.cfg.xml”, para hacer referencia a esta clase, en vez de a los (inexistentes) ficheros hbm.xml:

```xml
<!DOCTYPE hibernate-configuration PUBLIC
        "-//Hibernate/Hibernate Configuration DTD 3.0//EN"
        "http://www.hibernate.org/dtd/hibernate-configuration-3.0.dtd">

<hibernate-configuration>
    <session-factory>
        <property name="connection.driver_class">org.postgresql.Driver</property>
        <property name="connection.url">jdbc:postgresql://localhost:5432/BdAnotacionesHibernate</property>
        <property name="connection.username">postgres</property>
        <property name="connection.password">1234</property>
        <property name="dialect">org.hibernate.dialect.PostgreSQLDialect</property>
        <property name="hibernate.default_schema">public</property>  
        <mapping class="com.AccesoADatos.Ev2.HibernateAnotaciones1.modelos.Libros"/>
        <mapping class="com.AccesoADatos.Ev2.HibernateAnotaciones1.modelos.Autores"/>
    </session-factory>
</hibernate-configuration>
```

>[!WARNING] Recuerda 
>En Hibernate con XML esta instrucción mapping se hacía usando <mapping resource…> y separábamos con '/', sin embargo ahora es con <mapping class…> y separamos con '.'

Ya podremos crear el main para probarlo y todo debería funcionar igual que como lo hacía con ficheros Xml. Implementamos, por el momento, la funcionalidad de mostrar todos los libros. Usando el patron DAO:

**App.java**
```java
public class App {
	
    static Scanner teclado = new Scanner(System.in);
    
    static LibrosDAO librosDAO = new LibrosDAOImpl();


	public static void main(String[] args) {
        boolean terminado = false;
        
        do {
            System.out.println("\nEscoja una opción:");
            System.out.println("1. Ver todos los Libros");
            System.out.println("0. Salir");
            
            String opcion = teclado.nextLine();
            
            if (opcion.equals("1")) {
                mostrarLibros();
            } else if (opcion.equals("0")) {
                terminado = true;
            }
            
        } while (!terminado);
        
        System.out.println("Fin del programa.");
        teclado.close();
    }
	
	public static void mostrarLibros() {
        List<Libros> resultados = librosDAO.obtenerLibros();
        
        if (resultados != null && !resultados.isEmpty()) {
            for (Libros libro : resultados) {
                System.out.println(libro.getId() + ": " + libro.getTitulo() +
                 ", de " + 
                (libro.getAutores()!=null ? libro.getAutores().getNombre() : "Anónimo"));
            }
        } else {
            System.out.println("No hay libros registrados o hubo un error.");
        }
    }

}
```

**LibrosDAO**
```JAVA
public interface LibrosDAO {
    List<Libros> obtenerLibros();
}
```

**LibrosDAOImpl**
```JAVA
public class LibrosDAOImpl implements LibrosDAO {

    @Override
    public List<Libros> obtenerLibros() {
        try (Session session = HibernateUtil.getSessionFactory().openSession()) {
            // HQL: Usamos el nombre de la Clase, no de la tabla
            Query<Libros> consulta = session.createQuery("from Libros", Libros.class);
            return consulta.list();
        } catch (Exception e) {
            System.err.println("Error al listar libros: " + e.getMessage());
            return null;
        }
    }
}
```


**NOTA: CARGA LAZY VS EAGER** 
Si usamos la carga LAZY para el atributo Autores de la clase Libros, ejecutamos el ejemplo anterior y tratamos de obtener todos los libros, saltará una *LazyInitializationException*:
	- Tu DAO abre la sesión, pide los libros (`SELECT * FROM libros`) y **cierra la sesión**.
	- Hibernate te devuelve los libros, pero **NO se trae los datos del Autor**. En su lugar, pone un "Proxy" (un marcador de posición o fantasma) en el campo `autor`, los datos del autor son accesibles durante la sesión SI SE ACCEDE A ELLOS.
	- En tu `main` (método `mostrarLibros`), intentas hacer `libro.getAutor().getNombre()`.
	- Hibernate intenta acceder a esos datos que **NO** fueron estrictamente cargados.
	- **¡ERROR!** La sesión (la conexión) ya se cerró en el paso 1. Hibernate no tiene cómo ir a buscar el dato.
- Soluciones:
Opción 1:
```JAVA 
Query<Libro> q = session.createQuery("SELECT l FROM Libro l JOIN FETCH l.autores", Libro.class);
```

Opción 2:
```JAVA
@ManyToOne(fetch = FetchType.EAGER) // Ansioso
```


> [!NOTE] Ejercicio 1.
> Implementa, usando el Patrón DAO, la funcionalidad de mostrar todos los Autores


Si dejas activado el log verás mesajes de este estilo:

![[Pasted image 20260123165756.png]]

Esto es así porque Hibernate a bajo nivel utiliza jdbc para hacer las conexiones, de ahí la importancia de conocer cómo trabaja jdbc directamente y el haber visto este tema previamente.

## 3.  Relaciones: Uno a Muchos

En el modelo relacional, la relación **1:N** es la más común (ej. Un Artista tiene muchas Canciones, un Pedido tiene muchas Líneas...).

En Hibernate, esta relación se gestiona mediante dos anotaciones principales:

1. **`@ManyToOne`**: Se coloca en la entidad que tiene la Clave Foránea (La tabla "hija").
2. **`@OneToMany`**: Se coloca en la entidad "padre", sobre una lista o colección.

En nuestro ejemplo, la tabla `libros` es la que tiene la columna `codautor`. Por tanto, es la que "manda" estructuralmente -> Usamos `@ManyToOne`.
```java
@ManyToOne
@JoinColumn(name = "codautor")
private Autores autores;
```

Por otro lado, la entidad `Autores` no tiene ninguna columna en su tabla que apunte a los libros. Sin embargo, en Java queremos tener un `Set<Libros>`. -> Usamos `@OneToMany`.
```java
@OneToMany(mappedBy = "autores")
public Set<Libros> getLibroses() {
	return this.libroses;
}
```

**Concepto Clave (`mappedBy`):** Debemos decirle a Hibernate: _"Yo no tengo la clave foránea. La clave la tiene el atributo 'Autores' de la clase Libros"_. mappedBy Es obligatorio en relaciones bidireccionales

## 4.  Relaciones: Muchos a Muchos

Si se diese la circunstancia de que VARIOS autores, escriben VARIOS libros (Ej: Documentos científicos con coautores), habría que hacer unos ligeros cambios en nuestro ejemplo para propiciar esta relación N:M. Veamos, de manera **teórica** cómo afectaría esto al código, tal y como lo teníamos hasta ahora:
- En primer lugar, Como una relación Muchos a Muchos física es imposible de representar directamente en el modelo relacional estándar, la rompemos en dos relaciones Uno a Muchos. Para ello es necesario **crear una tabla auxiliar** cuya única función es **almacenar parejas de IDs**. Esta tabla podría ser de la siguiente manera:
```SQL
CREATE TABLE libro_autor (
    id_libro INT NOT NULL,
    id_autor INT NOT NULL,
    
    -- Definimos la Clave Primaria Compuesta
    -- Esto evita duplicados: no podemos tener dos veces el par (Libro A, Autor B)
    PRIMARY KEY (id_libro, id_autor),
    
    -- Definimos las Claves Foráneas (Relaciones 1:N)
    CONSTRAINT fk_libro FOREIGN KEY (id_libro) 
        REFERENCES libros (id) 
        ON DELETE CASCADE,  -- Si borro el libro, se borra la relación
        
    CONSTRAINT fk_autor FOREIGN KEY (id_autor) 
        REFERENCES autores (id) 
        ON DELETE CASCADE   -- Si borro al autor, se borra la relación
);
```

- En segundo lugar, sería necesario modificar la clase Autor. Para esta nueva situación, debemos poner una lista `autores`. Usaremos `@ManyToMany` y definiremos la **tabla intermedia** con `@JoinTable`.

```Java
package com.AccesoADatosEv2.HibernateAnotaciones1.modelos;

import jakarta.persistence.*;
import java.util.ArrayList;
import java.util.List;

@Entity
@Table(name = "libros")
public class Libros {

    @Id
	@Column(name = "id", unique = true, nullable = false)
    private int id;

	@Column(name = "titulo", length = 60)
    private String titulo;

    // CAMBIO IMPORTANTE
    @ManyToMany(cascade = {CascadeType.PERSIST, CascadeType.MERGE}, fetch = FetchType.EAGER)
    @JoinTable(
        name = "libro_autor",                  // Nombre de la tabla intermedia
        joinColumns = @JoinColumn(name = "id_libro"),       // FK libros
        inverseJoinColumns = @JoinColumn(name = "id_autor") // FK autores
    )
    private List<Autor> autores = new ArrayList<>();

    // Constructor vacío
    public Libro() {}

    public Libro(String titulo) {
        this.titulo = titulo;
    }

    // Getters y Setters actualizados
    public List<Autores> getAutores() {
        return autores;
    }

    public void setAutores(List<Autores> autores) {
        this.autores = autores;
    }

    // Método helper para añadir autores fácilmente
    public void addAutor(Autores autor) {
        this.autores.add(autor);
        autor.getLibros().add(this); // Mantenemos la coherencia en memoria
    }
    
    // ... resto de getters/setters (id, titulo) ...
}
```

- Después, las modificaciones de la clase Autores.java. Aquí ya teníamos una lista, pero la anotación era `@OneToMany`. Ahora será `@ManyToMany` y debemos indicar que la configuración la lleva la otra clase (`Libros`) usando `mappedBy`.
```Java
package com.AccesoADatosEv2.HibernateAnotaciones1.modelos;

import java.util.HashSet;
import java.util.Set;

import jakarta.persistence.Column;
import jakarta.persistence.Entity;
import jakarta.persistence.FetchType;
import jakarta.persistence.Id;
import jakarta.persistence.ManyToMany; // Importante: Cambiamos OneToMany por ManyToMany
import jakarta.persistence.Table;

@Entity
@Table(name = "autores")
public class Autores implements java.io.Serializable {
	
	private static final long serialVersionUID = 1L;
	private String cod;
	private String nombre;
	private Set<Libros> libroses = new HashSet<Libros>(0);
	
	public Autores() {
	}
	
	public Autores(String cod) {
		this.cod = cod;
	}
	
	public Autores(String cod, String nombre, Set<Libros> libroses) {
		this.cod = cod;
		this.nombre = nombre;
		this.libroses = libroses;
	}
	
	@Id
	@Column(name = "cod", unique = true, nullable = false, length = 5 )
	public String getCod() {
		return this.cod;
	}
	
	public void setCod(String cod) {
		this.cod = cod;
	}
	
	@Column(name = "nombre", length = 60)
	public String getNombre() {
		return this.nombre;
	}
	
	public void setNombre(String nombre) {
		this.nombre = nombre;
	}
	
    // CAMBIO PRINCIPAL AQUÍ
    // Cambiamos @OneToMany por @ManyToMany
    // Mantenemos fetch = FetchType.EAGER (Carga ansiosa para evitar errores de sesión cerrada)
    // mappedBy = "autores": Indica que la clase 'Libros' es la que manda (la que tiene el @JoinTable)
    //    y que allí el atributo se llama 'autores'.
	@ManyToMany(fetch = FetchType.EAGER, mappedBy = "autores")
	public Set<Libros> getLibroses() {
		return this.libroses;
	}
	
	public void setLibroses(Set<Libros> libroses) {
		this.libroses = libroses;
	}

}
}
```

- Por último, en nuestro método principal Podríamos crear un escenario donde **dos autores escriben juntos un libro** (algo imposible en la versión anterior):

```Java
public class App {
    public static void main(String[] args) {
        // (Configuración de SessionFactory igual que siempre)
        Session session = sessionFactory.openSession();
        session.beginTransaction();

        // Creamos los objetos
        Autor autor1 = new Autor("Neil Gaiman");
        Autor autor2 = new Autor("Terry Pratchett");
        
        Libro libroColaborativo = new Libro("Buenos Presagios");
        Libro libroSoloGaiman = new Libro("Coraline");

        // Establecemos las relaciones (N:M)
        // Buenos Presagios lo escribieron los dos
        libroColaborativo.addAutor(autor1);
        libroColaborativo.addAutor(autor2);
        
        // Coraline solo Gaiman
        libroSoloGaiman.addAutor(autor1);

        // Persistimos (Gracias al CascadeType.PERSIST en Libro, al guardar libro se guardan autores)
        session.persist(libroColaborativo);
        session.persist(libroSoloGaiman);

        session.getTransaction().commit();
        
        System.out.println("Datos guardados. Revisa la base de datos.");

        session.close();
        sessionFactory.close();
    }
}
```

>[!WARNING] ERROR COMÚN
Hibernate **solo mira la lista del Lado Propietario** para saber qué insertar, borrar o actualizar en la tabla intermedia `libro_autor`.

### 1. El Escenario Correcto

Si haces esto en el `Main`:
```Java
// Creas los objetos
Libro libro = new Libro("Dune");
Autor autor = new Autor("Frank Herbert");

// AÑADES EN LA LISTA DEL PROPIETARIO (Libro)
libro.getAutores().add(autor); 

// Guardas el libro
session.persist(libro);
```

Hibernate detecta que la lista `autores` dentro de `libro` tiene un elemento nuevo. Entonces genera automáticamente **dos** SQLs:

1. `INSERT INTO libros ...` (Guarda el libro).
    
2. `INSERT INTO libro_autor (id_libro, id_autor) VALUES (..., ...)` (Guarda la relación).

### 2. El Escenario Erroneo

Si por el contrario, haces esto:
```Java
// Creas los objetos
Libro libro = new Libro("Dune");
Autor autor = new Autor("Frank Herbert");

// AÑADES SOLO EN LA LISTA DEL INVERSO (Autor)
autor.getLibroses().add(libro); // El lado 'mappedBy'

// Guardas el autor
session.persist(autor);
```

**¿Qué pasa en la base de datos?**

- Se guarda el Autor (`INSERT INTO autores...`).
- Se guarda el Libro (por la cascada).
- **¡NO se guarda nada en `libro_autor`!**

Porque Hibernate ignora los cambios hechos en la colección marcada con `mappedBy` a la hora de escribir en la base de datos. `mappedBy` significa literalmente: _"Yo no controlo esta relación, ve a mirar a la otra clase"_.

## 5.  Más sobre Hibernate. NativeQuery

A estas alturas del documento, ya te habrás dado cuenta que Hibernate es una herramienta tan poderosa como enorme. Podríamos dar un curso entero sobre esta herramienta, que es ampliamente utilizada en el entorno empresarial. Como ya he puesto durante el manual, la mejor información la puedes obtener en el manual oficial de Hibernate:

https://docs.jboss.org/hibernate/orm/current/userguide/html_single/Hibernate_User_Guide.html

Hasta ahora, hemos estado viendo conceptos del punto 17 que habla de HQL y JPQL, pero verás que el apartado 19 habla de Native SQL Query.

Esto es útil si deseas utilizar características específicas de la base de datos, como funciones de ventana, expresiones de tabla comunes (CTE) o la opción CONNECT BY en Oracle. También proporciona una ruta de migración limpia desde una aplicación directa basada en SQL / JDBC a Hibernate / JPA.

Hibernate también te permite especificar SQL escrito a mano (incluidos los procedimientos almacenados) para todas las operaciones de creación, actualización, eliminación y recuperación. 

No es objeto de este curso (por cuestiones horarias) ser megaexpertos en Hibernate, pero sí me gustaría ver una aproximación a lo que podría ser una función actual que use NativeQuery:

```Java
public static void nativeQuery() {
    try (Session session = HibernateUtil.getSessionFactory().openSession()) {
        
        // createNativeQuery pasando la clase como SEGUNDO argumento.
        List<Libro> libros = session.createNativeQuery(
            "SELECT * FROM Libros WHERE titulo LIKE :name", 
            Libros.class 
        )
        .setParameter("name", "M%")
        .getResultList(); // Usamos getResultList() en lugar de list()

        for (Libro libro : libros) {
            System.out.println(libro.getTitulo());
        }
    } catch (Exception e) {
        e.printStackTrace();
    }
}
```

## 6.  Named Query

Existe otra forma interesante de generar las consultas sobre las entidades de una forma organizada, que consiste en definir las querys que vayamos a utilizar en el propio archivo de la clase y luego instanciar dichas consultas. Un ejemplo podría ser lo siguiente:

```Java
@Entity @NamedQueries({
@NamedQuery(name = "Libros.findAll", query = "SELECT l FROM Libros l"), @NamedQuery(name = "Libros.findById", query = "SELECT l FROM Libros l WHERE l.id = :id"),
@NamedQuery(name = "Libros.findByTitulo", query = "SELECT l FROM Libros l WHERE l.titulo = :titulo")})
public class Libros implements java.io.Serializable {
...
```

Como se observa, justo encima de la definición de la clase colocamos la/s query/s que queremos tener almacenadas para poder llamar.

Para poder usar estas querys automáticas, basta con ver el siguiente ejemplo que usa la de Libros.findByTitulo:

```Java
public static void ejemploNamedQuerys(){
	SessionFactory sessionFactory = new Configuration()
		.configure().buildSessionFactory();
	Session session = sessionFactory.openSession();
	List<Libros> libros = session
		.createNamedQuery("Libros.findByTitulo", Libros.class)
		.setParameter("titulo", "Android")
		.getResultList();
	
	libros.forEach(libro->System.out.println(libro.getId() + "" + libro.getTitulo()));
	session.close();
}
```


Podemos generar tantas named querys como consideremos oportuno. Podemos obtener más información aquí:

https://docs.jboss.org/hibernate/orm/current/userguide/html_single/Hibernate_User_Guide.html#sql-named-queries

## 7.  Diferencias entre JPA y JDBC

Los que ya han tenido la oportunidad de trabajar con algún ORM como JPA o Hibernate sabrán las bondades que tiene ya que nos permite desarrollar de una forma mucho más rápida y con muchos menos errores en tiempo de ejecución además de modelar nuestras entidades como Clases de Java las cuales serán convertidas a las instrucciones Insert, Update o Select según sea la operación a realizar. Claro que todos estos beneficios tienen un costo y es que el rendimiento se degrada debido a todas las conversiones que se tienen que hacer para convertir las Entity en Querys y los ResultSet pasarlos a clases, además que cada registro representa un Objeto en memoria que tendrá que ser administrado por nuestro servidor.

Por otro lado, tenemos a JDBC que nos brinda total libertad de hacer lo que queramos sin ningún tipo de limitación explotando al máximo las características de la base de datos. JDBC nos permite realizar consultas nativas para cada base de datos, lo que ayuda mucho a la velocidad de respuesta, y los resultados son devueltos en un ResultSet de los cuales podemos extraer solamente los datos que requerimos y no toda la Entity como en el caso de JPA o Hibernate.

**JPA, Hibernate**

Ventajas:
1.     Nos permite desarrollar mucho más rápido.
2.     Permite trabajar con la base de datos por medio de entidades en vez de Querys.
3.     Nos ofrece un paradigma 100% orientado a objetos.
4.     Elimina errores en tiempo de ejecución.
5.     Mejora el mantenimiento del software.

Desventajas:
1.     No ofrece toda la funcionalidad que ofrecería tirar consultas nativas.
2.     El rendimiento es mucho más bajo que realizar las consultas por JBDC.
3.     Puede representar una curva de aprendizaje más grande.

**JDBC**

Ventajas:
1. Ofrece un rendimiento superior ya que es la forma más directa de mandar instrucciones a la base de datos.
2. Permite explotar al máximo las funcionalidades de la base de datos.

Desventajas:
1. El mantenimiento es mucho más costoso.
2. Introduce muchos errores en tiempo de ejecución.
3. El desarrollo es mucho más lento.

Probablemente pensarás que JPA o Hibernate son mejores ya que ofrecen mayores ventajas que desventajas, sin embargo las desventajas que tienen son muy serias y pueden ser cruciales a la hora de decidir qué tecnología utilizar ya que si tenemos una aplicación muy buena y fácil de mantener pero que tarda demasiado en consultar datos puede llegar a ser algo muy malo.

Como conclusión yo diría que si requieres una aplicación donde el rendimiento sea el factor crítico utilices JDBC, pero por otra parte si el rendimiento no es tan crucial y prefieres un mantenimiento y seguridad mayor deberías utilizar JPA o Hibernate. A día de hoy las empresas importantes usan ampliamente Hibernate en su pull de tecnologías.

## 8.  Para profundizar…

Conceptos básicos:
https://en.wikipedia.org/wiki/Object-relational_impedance_mismatch

https://en.wikipedia.org/wiki/Object-relational_mapping

https://en.wikibooks.org/wiki/Java_Persistence/OneToMany#Example_of_a_OneToMany_relationship_and_inverse_ManyToOne_XML

https://en.wikibooks.org/wiki/Java_Persistence/ManyToMany#Example_of_a_ManyToMany_relationship_database

Nociones básicas de JPA e Hibernate: https://en.wikipedia.org/wiki/Java_Persistence_API

Un ejemplo de cómo usar Hibernate desde NetBeans (en una aplicación Swing que conecta a MySQL): https://netbeans.org/kb/docs/java/hibernate-java-se.html

**La documentación oficial de Hibernate está en**:

https://docs.jboss.org/hibernate/orm/current/userguide/html_single/Hibernate_User_Guide.html