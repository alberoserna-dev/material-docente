### Contexto del Proyecto

El año es 2056. Ha ocurrido lo impensable: el apocalipsis zombi ha colapsado las comunicaciones y los gobiernos mundiales. Como uno de los pocos ingenieros de software que quedan con acceso a servidores operativos y electricidad, tu misión es vital para la humanidad.

Debes programar la **Z-Tracker API**, un sistema de radiobalizas basado en arquitectura REST para gestionar los **Refugios** que quedan en pie y registrar a los **Supervivientes** que habitan en ellos. De tu código depende que mantengamos un registro seguro de quién está a salvo en los refugios... y de quién ha sido infectado. No hay margen para el error: si el código falla, el refugio cae.

### Objetivos de Aprendizaje

Con la realización de este proyecto asentarás los conceptos vistos en el Tema 6 de Componentes con Spring:

- Configurar un proyecto Spring Boot desde cero utilizando dependencias clave (Web, Data JPA, PostgreSQL).
    
- Mapear entidades relacionales (`1:N`) utilizando anotaciones JPA (`@Entity`, `@OneToMany`, `@ManyToOne`).
    
- Implementar correctamente el patrón de **Arquitectura en 3 Capas** (`@RestController`, `@Service`, `@Repository`).
    
- Exponer endpoints REST (GET, POST, PUT, DELETE) controlando las respuestas HTTP de forma profesional mediante `ResponseEntity`.
    
- Aplicar lógica de negocio condicional en la capa de servicios para controlar eventos dinámicos.
    

---

### Detalles sobre la Base de Datos

Para este proyecto utilizaremos **PostgreSQL**. No necesitarás escribir el script SQL de creación de tablas; delegaremos esta tarea en el propio framework de Spring.

Deberás crear una base de datos vacía llamada `z_tracker_db` en tu gestor local y configurar el archivo `application.properties` con la propiedad `spring.jpa.hibernate.ddl-auto=update` para que Spring construya la estructura.

El modelo de datos constará de dos entidades principales con una relación bidireccional:

**1. Entidad `Refugio`**

- `id`: Identificador único (Clave primaria, autoincremental).
    
- `nombre`: Cadena de texto con el nombre clave del refugio (ej. "Estación Alfa").
    
- `nivelSeguridad`: Número entero del 1 al 10.
    
- `supervivientes`: Lista de supervivientes albergados en este refugio (Lado "Uno" de la relación).
    

**2. Entidad `Superviviente`**

- `id`: Identificador único (Clave primaria, autoincremental).
    
- `nombre`: Cadena de texto con el nombre del superviviente.
    
- `estado`: Cadena de texto. Solo admitirá los valores _"SANO"_, _"INFECTADO"_, o _"ZOMBI"_.
    
- `refugio`: El refugio al que pertenece (Lado "Muchos" de la relación, clave foránea).
    

---

### Relación de Pasos a Realizar

Te propongo este orden  de desarrollo para asegurar el éxito del proyecto:

**Paso 1: Inicialización de la Base de Operaciones**

- Acude a _Spring Initializr_ y genera un proyecto Spring Boot con las dependencias: Spring Web, Spring Data JPA y PostgreSQL Driver.
    
- Añade las credenciales de tu base de datos en `application.properties`.
    

**Paso 2: Modelado de Datos (Entities)**

- Crea el paquete `models` y programa las clases `Refugio` y `Superviviente`.
    
- Añade las anotaciones de JPA correspondientes (`@Entity`, `@Id`, `@GeneratedValue`...).
    
- Establece la relación usando entre la clase Refugio y la clase Superviviente, usando anotaciones.
    

**Paso 3: Capa de Acceso a Datos (Repositories)**

- Crea el paquete `repositories` y define las interfaces `RefugioRepository` y `SupervivienteRepository` extendiendo de `JpaRepository`.
    
- Añade un método derivado (_Derived Query Method_) en el repositorio de Superviviente para poder buscar a todos los supervivientes según su `estado`.
    

**Paso 4: Reglas de Supervivencia (Services)**

- Crea el paquete `services` con las clases `RefugioService` y `SupervivienteService`. Recuerda que en esta capa debes inyectar los repositorios usando `@Autowired`.
    
- Implementa los métodos básicos para obtener, guardar y borrar registros.
    
- **Lógica de negocio 1:** En el método de guardar un superviviente, añade una comprobación: si el `estado` del superviviente es _"ZOMBI"_, no se le puede asignar ningún refugio (debes devolver `null` o guardarlo sin refugio asignado).
    
- **Lógica de negocio 2 (El Evento de Horda):** En `RefugioService`, crea un método llamado `simularAtaqueZombi(Integer idRefugio)`. Este método buscará el refugio; si lo encuentra, restará 1 a su `nivelSeguridad`. Si tras la resta el nivel de seguridad es menor a 5, el primer superviviente de la lista de ese refugio cambiará automáticamente su estado a _"INFECTADO"_. Guarda los cambios en la base de datos.
    

**Paso 5: Comunicaciones por Radio (Controllers)**

- Crea el paquete `controllers` e implementa `RefugioController` y `SupervivienteController` (mapeados en `/api/refugios` y `/api/supervivientes`).
    
- Inyecta los servicios correspondientes y expón los métodos CRUD básicos asegurándote de devolver los códigos correctos (`200 OK`, `201 CREATED`, `204 NO CONTENT`, `404 NOT FOUND`).
    
- Crea un endpoint especial mapeado por POST a la ruta `/api/refugios/{id}/ataque` que invoque la lógica de la horda programada en el paso anterior.
    

**Paso 6: Pruebas de Transmisión (Postman)**

- Inicia el servidor de Spring Boot.
    
- Abre Postman y verifica que puedes registrar Refugios y Supervivientes (POST).
    
- Verifica que puedes leerlos (GET).
    
- Lanza el ataque zombie a uno de tus refugios usando el endpoint de ataque y comprueba cómo cambian los datos de seguridad e infección en tu base de datos. 

¡Buena suerte!