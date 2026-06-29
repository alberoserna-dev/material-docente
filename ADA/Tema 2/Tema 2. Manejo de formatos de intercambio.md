## Formatos de intercambio de información: XML

A diferencia de los ficheros de texto plano o los ficheros binarios, en la actualidad la información entre aplicaciones (independientemente del lenguaje en el que estén programadas) se transmite utilizando formatos estructurados y autodescriptivos. Uno de los estándares más consolidados en el entorno empresarial es **XML** (eXtensible Markup Language).

En Java, el tratamiento de documentos XML no se realiza leyendo el fichero línea a línea como hacíamos con la clase `BufferedReader`, sino utilizando "parsers" (analizadores sintácticos) que interpretan las etiquetas. La primera aproximación para leer estos ficheros es la API **DOM** (Document Object Model).

### Procesamiento de XML con la API DOM

La API DOM, incluida en el paquete `javax.xml.parsers` nativo de Java, lee el archivo XML completo y construye en la memoria RAM una estructura de árbol con todos sus elementos (nodos).

**Características principales:**

- Carga todo el documento en memoria antes de procesarlo.
- Permite navegar por el documento en cualquier dirección (acceso aleatorio) y modificar su contenido o estructura estructuralmente.
- Es ideal para documentos XML de tamaño pequeño o mediano.
- Puede generar problemas de rendimiento (desbordamiento de memoria) si el archivo XML es de varios Gigabytes.

#### Ejemplo de uso: Lectura básica con DOM

Supongamos que tenemos un archivo llamado `empleados.xml` con la siguiente estructura:

```XML
<empresa>
    <empleado id="1">
        <nombre>Stuart Little</nombre>
        <departamento>Informática</departamento>
    </empleado>
</empresa>
```

A continuación se muestra cómo cargar y leer este archivo utilizando DOM:

```Java
import javax.xml.parsers.DocumentBuilder;
import javax.xml.parsers.DocumentBuilderFactory;
import org.w3c.dom.Document;
import org.w3c.dom.Element;
import org.w3c.dom.Node;
import org.w3c.dom.NodeList;
import java.io.File;

public class EjemploLecturaDOM {
    public static void main(String[] args) {
        try {
            File archivoXML = new File("empleados.xml");
            
            // 1. Instanciar la fábrica y el constructor de documentos
            DocumentBuilderFactory dbFactory = DocumentBuilderFactory.newInstance();
            DocumentBuilder dBuilder = dbFactory.newDocumentBuilder();
            
            // 2. Parsear el archivo y construir el árbol en memoria
            Document doc = dBuilder.parse(archivoXML);
            
            // Opcional: Normalizar el documento (elimina nodos de texto vacíos)
            doc.getDocumentElement().normalize();
            
            System.out.println("Elemento raíz: " + doc.getDocumentElement().getNodeName());
            
            // 3. Obtener la lista de todos los elementos <empleado>
            NodeList listaEmpleados = doc.getElementsByTagName("empleado");
            
            for (int i = 0; i < listaEmpleados.getLength(); i++) {
                Node nodo = listaEmpleados.item(i);
                
                if (nodo.getNodeType() == Node.ELEMENT_NODE) {
                    Element elemento = (Element) nodo;
                    
                    String id = elemento.getAttribute("id");
                    String nombre = elemento.getElementsByTagName("nombre").item(0).getTextContent();
                    String departamento = elemento.getElementsByTagName("departamento").item(0).getTextContent();
                    
                    System.out.println("Empleado ID: " + id + " - Nombre: " + nombre + " (" + departamento + ")");
                }
            }
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```

>[!NODE] Ejercicio 1
Crea un archivo XML llamado `concesionario.xml` que contenga una lista de al menos 3 vehículos (con etiquetas para marca, modelo y precio). A continuación, escribe un programa en Java utilizando DOM que lea el archivo y muestre por consola el catálogo completo de vehículos.

>[!NODE] Ejercicio 2
Amplía el programa anterior para que calcule y muestre al final de la ejecución el precio medio de todos los vehículos almacenados en el documento XML.

### Procesamiento basado en eventos: La API SAX y la eficiencia en memoria

Como hemos visto, la API DOM es muy cómoda porque nos permite navegar por el XML como si fuera un árbol, pero tiene un coste altísimo: carga el archivo completo en la memoria RAM. Si intentamos leer un log de eventos XML de 5 Gigabytes con DOM, nuestro programa en Java sufrirá un _OutOfMemoryError_ y colapsará.

Para solucionar esto, Java incorpora la **API SAX** (_Simple API for XML_). SAX utiliza un enfoque completamente distinto: el **procesamiento basado en eventos**.

**Características principales de SAX:**

- **Lectura secuencial:** Lee el archivo de principio a fin, línea a línea, sin cargar el documento completo en memoria.
- **Solo lectura:** A diferencia de DOM, con SAX no podemos modificar el archivo XML ni añadir nuevos nodos, solo leer información.
- **Eficiencia máxima:** El consumo de memoria es mínimo y constante, independientemente de si el archivo pesa 2 Kilobytes o 10 Gigabytes.

**¿Cómo funcionan los Eventos en SAX?**

En lugar de buscar nosotros los datos, el _Parser_ (analizador) de SAX va leyendo el archivo y nos "avisa" cada vez que encuentra algo interesante. Para interceptar estos avisos, debemos crear una clase que herede de `DefaultHandler` y sobrescribir los métodos que nos interesen:

1. `startDocument()`: Se dispara al abrir el archivo.
2. `startElement()`: Se dispara al encontrar una etiqueta de apertura (ej. `<empleado id="1">`).
3. `characters()`: Se dispara al leer el texto plano dentro de una etiqueta (ej. `Juan Pérez`).
4. `endElement()`: Se dispara al encontrar una etiqueta de cierre (ej. `</empleado>`).
5. `endDocument()`: Se dispara al terminar de leer el archivo.

**Ejemplo de uso: Lectura eficiente con SAX**

Vamos a leer el mismo archivo `empleados.xml` del apartado anterior, pero esta vez utilizando SAX. Para ello, necesitamos nuestra clase principal y una clase _Manejador_ (`Handler`).

```Java
import javax.xml.parsers.SAXParser;
import javax.xml.parsers.SAXParserFactory;
import org.xml.sax.Attributes;
import org.xml.sax.SAXException;
import org.xml.sax.helpers.DefaultHandler;
import java.io.File;

public class EjemploLecturaSAX {
    public static void main(String[] args) {
        try {
            File archivoXML = new File("empleados.xml");
            
            // 1. Instanciar la fábrica y el parser SAX
            SAXParserFactory factory = SAXParserFactory.newInstance();
            SAXParser saxParser = factory.newSAXParser();
            
            // 2. Instanciar nuestro manejador de eventos personalizado
            ManejadorEmpleados handler = new ManejadorEmpleados();
            
            // 3. Iniciar el parseo (esto bloqueará el hilo hasta que termine de leer todo el archivo)
            saxParser.parse(archivoXML, handler);
            
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}

// Clase interna o externa que define qué hacer en cada evento
class ManejadorEmpleados extends DefaultHandler {
    
    private boolean bNombre = false;
    private boolean bDepartamento = false;
    
    @Override
    public void startElement(String uri, String localName, String qName, Attributes attributes) throws SAXException {
        if (qName.equalsIgnoreCase("empleado")) {
            String id = attributes.getValue("id");
            System.out.println("\nEmpleado ID: " + id);
        } else if (qName.equalsIgnoreCase("nombre")) {
            bNombre = true;
        } else if (qName.equalsIgnoreCase("departamento")) {
            bDepartamento = true;
        }
    }

    @Override
    public void characters(char[] ch, int start, int length) throws SAXException {
        if (bNombre) {
            System.out.println("Nombre: " + new String(ch, start, length));
            bNombre = false;
        } else if (bDepartamento) {
            System.out.println("Departamento: " + new String(ch, start, length));
            bDepartamento = false;
        }
    }
}
```

>[!NOTE] Ejercicio 3
Basándote en el archivo `concesionario.xml` que creaste en el Ejercicio 1, desarrolla un programa utilizando la API SAX que recorra el archivo e imprima **exclusivamente** los modelos de los vehículos, ignorando las marcas y los precios.

>[!NOTE] Ejercicio 4
El ayuntamiento ha publicado un archivo XML gigantesco con todas las multas de tráfico del año, y abrirlo con DOM colapsa el servidor. Utilizando SAX, crea un _Handler_ que no imprima nada por pantalla durante el parseo, sino que simplemente lleve un contador interno y, al dispararse el evento `endDocument()`, imprima por consola el número total de vehículos que superaron un precio de 30.000€ en tu archivo `concesionario.xml`.

## El concepto de Data Binding: Introducción a JAXB y anotaciones

Hasta ahora hemos visto cómo manipular archivos XML trabajando directamente con "nodos" (DOM) o interceptando "eventos" de texto (SAX). Ambas soluciones son de bajo nivel: nos obligan a escribir mucho código manual para extraer un simple texto y convertirlo en las propiedades de nuestros objetos Java.

Para solucionar esto de una forma mucho más profesional y orientada a objetos surge el concepto de **Data Binding** (vinculación de datos). El Data Binding es el proceso de mapear automáticamente un documento XML a una jerarquía de clases en Java (y viceversa) sin tener que recorrer manualmente el árbol de etiquetas.

La tecnología estándar de Java para realizar este proceso es **JAXB** (_Java Architecture for XML Binding_).

> **Nota técnica fundamental:** A partir de la versión 9 de Java, JAXB fue extraído del núcleo estándar del JDK (Java SE). Para poder utilizarlo en la actualidad, es obligatorio importar la librería mediante nuestro gestor de dependencias (ej: Maven), añadiendo la especificación de **Jakarta XML Binding** (`jakarta.xml.bind-api`) y su implementación en tiempo de ejecución.

### Procesos clave en JAXB: Marshalling y Unmarshalling

JAXB automatiza la conversión en ambas direcciones mediante dos procesos principales:

1. **Marshalling (Serialización):** Convierte un objeto Java (y todos los objetos que contenga) en un archivo o flujo XML.
2. **Unmarshalling (Deserialización):** Lee un archivo XML y crea automáticamente los objetos Java correspondientes, rellenando sus atributos con los datos del XML.

**Anotaciones principales de JAXB**
Para que JAXB sepa exactamente cómo traducir nuestra clase Java a etiquetas XML, debemos "decorar" nuestra clase con anotaciones (POJO anotado):

- `@XmlRootElement(name="etiqueta")`: Se coloca encima de la definición de la clase para indicar que será la etiqueta raíz del documento XML.
- `@XmlElement(name="etiqueta")`: Se coloca sobre un atributo (o su método _getter_) para indicar que ese dato será una etiqueta hija.
- `@XmlAttribute(name="nombreAtributo")`: Se indica para que el valor se guarde como un atributo dentro de la etiqueta XML en lugar de como un elemento separado.
- `@XmlTransient`: Se utiliza para que JAXB ignore ese campo y no lo incluya en el XML (similar a la palabra reservada `transient` en la serialización nativa de Java).
    

**Ejemplo de uso: Marshalling y Unmarshalling con JAXB**

Para este ejemplo, primero necesitamos preparar nuestro objeto o clase modelo (`Vehiculo`):

```Java
import jakarta.xml.bind.annotation.XmlAttribute;
import jakarta.xml.bind.annotation.XmlElement;
import jakarta.xml.bind.annotation.XmlRootElement;

// 1. Anotamos la clase como elemento raíz
@XmlRootElement(name = "vehiculo")
public class Vehiculo {
    
    private String matricula;
    private String marca;
    private int precio;

    // JAXB exige siempre un constructor vacío
    public Vehiculo() {}

    public Vehiculo(String matricula, String marca, int precio) {
        this.matricula = matricula;
        this.marca = marca;
        this.precio = precio;
    }

    // 2. Anotamos los getters para definir la estructura XML
    @XmlAttribute(name = "id_matricula")
    public String getMatricula() {
        return matricula;
    }

    public void setMatricula(String matricula) {
        this.matricula = matricula;
    }

    @XmlElement(name = "fabricante")
    public String getMarca() {
        return marca;
    }

    public void setMarca(String marca) {
        this.marca = marca;
    }

    @XmlElement
    public int getPrecio() {
        return precio;
    }

    public void setPrecio(int precio) {
        this.precio = precio;
    }
}
```

A continuación, creamos el programa principal que realizará los procesos de ida y vuelta:

```Java
import jakarta.xml.bind.JAXBContext;
import jakarta.xml.bind.JAXBException;
import jakarta.xml.bind.Marshaller;
import jakarta.xml.bind.Unmarshaller;
import java.io.File;

public class EjemploJAXB {
    public static void main(String[] args) {
        Vehiculo coche = new Vehiculo("1234ABC", "Toyota", 25000);
        File archivoXML = new File("coche_jaxb.xml");

        try {
            // Inicializar el contexto de JAXB asociado a nuestra clase
            JAXBContext contexto = JAXBContext.newInstance(Vehiculo.class);

            // ==========================================
            // MARSHALLING: De Objeto Java a archivo XML
            // ==========================================
            Marshaller marshaller = contexto.createMarshaller();
            // Configurar para que el XML salga formateado (con saltos de línea y tabulaciones)
            marshaller.setProperty(Marshaller.JAXB_FORMATTED_OUTPUT, Boolean.TRUE);
            
            // Realizar la conversión y volcar al fichero
            marshaller.marshal(coche, archivoXML);
            System.out.println("Archivo XML generado correctamente mediante Marshalling.");

            // ==========================================
            // UNMARSHALLING: De archivo XML a Objeto Java
            // ==========================================
            Unmarshaller unmarshaller = contexto.createUnmarshaller();
            
            // Leer el archivo y castear directamente al objeto
            Vehiculo cocheLeido = (Vehiculo) unmarshaller.unmarshal(archivoXML);
            
            System.out.println("\nDatos recuperados mediante Unmarshalling:");
            System.out.println("Marca: " + cocheLeido.getMarca());
            System.out.println("Precio: " + cocheLeido.getPrecio());

        } catch (JAXBException e) {
            e.printStackTrace();
        }
    }
}
```

>[!NOTE] Ejercicio 5
Crea un proyecto en tu IDE y añade las dependencias necesarias de Jakarta XML Binding (`jakarta.xml.bind-api` y su implementación) en el archivo `pom.xml`. Copia la clase `Vehiculo` mostrada en la teoría y crea una nueva clase `Concesionario` que contenga una lista (colección) de vehículos. Anota adecuadamente la clase `Concesionario`.

>[!NOTE] Ejercicio 6
Desarrolla una clase ejecutable que instancie un objeto `Concesionario`, le añada tres objetos `Vehiculo` e invócalo a un proceso de _Marshalling_ para generar un archivo físico llamado `concesionario_jaxb.xml`. Abre el archivo generado y comprueba que la estructura refleja tanto las etiquetas de los vehículos como la de la lista contenedora.

## Formato JSON: Librerías para Java (Gson/Jackson) y comparación con XML

Aunque XML sigue siendo un estándar robusto en entornos empresariales y sistemas heredados (_legacy_), **JSON** (_JavaScript Object Notation_) se ha convertido en el rey indiscutible para el intercambio de datos en la web moderna y el desarrollo de APIs RESTful.

### Comparación entre XML y JSON

Ambos son formatos estructurados y autodescriptivos, pero presentan diferencias arquitectónicas clave:

- **Sintaxis:** XML utiliza un modelo de árbol basado en etiquetas de apertura y cierre (muy similar a HTML). JSON utiliza una estructura mucho más ligera basada en pares de **clave-valor** y listas (arrays).
- **Verbosidad:** JSON es mucho menos verboso que XML, ya que elimina la redundancia de las etiquetas de cierre, lo que reduce el peso del archivo y ahorra ancho de banda en las comunicaciones de red.
- **Tipos de datos:** En XML todo el contenido es, por defecto, texto plano (salvo que se aplique un esquema XSD complejo). JSON soporta tipos de datos nativos de forma directa: cadenas de texto (`"texto"`), números (`42`), booleanos (`true`/`false`), nulos (`null`), arrays (`[...]`) y objetos (`{...}`).

### Librerías habituales en Java: Gson y Jackson

A diferencia de XML, el JDK estándar de Java no incluye de forma nativa una librería para parsear JSON. En el entorno profesional es obligatorio recurrir a librerías de terceros (importadas habitualmente vía Maven):

1. **Gson:** Desarrollada por Google. Es extremadamente sencilla de utilizar y perfecta para proyectos de tamaño pequeño y medio. Convierte objetos Java a JSON y viceversa con un par de líneas de código.
2. **Jackson:** Es la librería estándar de facto en grandes aplicaciones empresariales y viene integrada por defecto en _frameworks_ de backend tan potentes como Spring Boot. Es muy rápida y ofrece un control granular exhaustivo sobre el proceso de mapeo mediante anotaciones.

**Ejemplo de uso: Serialización y Deserialización con Gson**

Para utilizar Gson, primero debes añadir su dependencia en el archivo `pom.xml` de Maven. Una vez añadida, el proceso es tan directo que no requiere configurar contextos complejos como hacíamos en JAXB.

Reutilizando nuestra clase `Vehiculo` del apartado anterior (no son necesarias las anotaciones de JAXB para que Gson funcione, aunque se pueden configurar mapeos personalizados si los nombres de los atributos difieren):

```Java
import com.google.gson.Gson;
import com.google.gson.GsonBuilder;
import java.io.FileReader;
import java.io.FileWriter;
import java.io.IOException;

public class EjemploJSON {
    public static void main(String[] args) {
        Vehiculo coche = new Vehiculo("9876XYZ", "Ford", 21000);
        
        // Instanciar Gson. Usamos el Builder para activar el "Pretty Printing" (formato legible)
        Gson gson = new GsonBuilder().setPrettyPrinting().create();
        
        // ==========================================
        // SERIALIZACIÓN: De Objeto Java a archivo JSON
        // ==========================================
        try (FileWriter writer = new FileWriter("coche.json")) {
            // Convierte el objeto a JSON y lo escribe en el flujo de salida
            gson.toJson(coche, writer);
            System.out.println("Archivo JSON generado correctamente.");
        } catch (IOException e) {
            e.printStackTrace();
        }

        // ==========================================
        // DESERIALIZACIÓN: De archivo JSON a Objeto Java
        // ==========================================
        try (FileReader reader = new FileReader("coche.json")) {
            // Lee el archivo JSON y lo mapea directamente a la clase Vehiculo
            Vehiculo cocheLeido = gson.fromJson(reader, Vehiculo.class);
            
            System.out.println("\nDatos recuperados desde JSON:");
            System.out.println("Matrícula: " + cocheLeido.getMatricula());
            System.out.println("Marca: " + cocheLeido.getMarca());
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

>[!NOTE] Ejercicio 7
Añade la dependencia de la librería Gson (o Jackson, si lo prefieres) a tu proyecto Maven. Aprovechando la clase `Concesionario` creada en los ejercicios anteriores (que contiene una lista de vehículos), desarrolla una clase ejecutable que serialice dicho catálogo y lo exporte a un archivo físico llamado `catalogo_exportado.json`. Abre el archivo en el IDE y examina cómo ha formateado la lista (utilizando corchetes `[]` para la colección de objetos).

>[!NOTE] Ejercicio 8
Crea un archivo manualmente en tu entorno de trabajo llamado `configuracion.json` que contenga un objeto JSON con los datos ficticios de configuración de una aplicación (nombre del servidor, puerto, base de datos y un valor booleano indicando si está en modo desarrollo). Diseña la clase POJO correspondiente en Java y escribe un programa que deserialice el archivo y muestre los parámetros de configuración por consola.

## Buenas prácticas y Errores comunes en el manejo de formatos de intercambio

Como cierre de esta unidad, es fundamental interiorizar una serie de directrices profesionales y conocer los fallos más habituales a la hora de trabajar con flujos de datos estructurados. Adoptar estas prácticas garantizará el desarrollo de aplicaciones robustas, legibles y eficientes, facilitando la futura integración de nuestras soluciones.

**Buenas prácticas de programación y diseño**
- **Separación de responsabilidades:** Nunca se debe mezclar la lógica de lectura/escritura (parseo de XML o JSON) con la lógica de negocio o la interfaz de usuario de la aplicación. Centraliza el tratamiento de formatos en clases de utilidad específicas o implementa el patrón DAO (_Data Access Object_) para abstraer el origen de los datos.
- **Elección inteligente de la herramienta de parseo:** Evalúa siempre el tamaño del archivo y el objetivo de la aplicación antes de programar. Utiliza la API DOM únicamente para archivos pequeños o cuando necesites modificar la jerarquía. Para archivos masivos o telemetrías de solo lectura, emplea SAX para evitar cuellos de botella en la memoria RAM.
- **Gestión explícita de la codificación:** Al leer o escribir archivos de intercambio, no confíes en la codificación por defecto del sistema operativo anfitrión. Especifica siempre UTF-8 en los flujos de entrada/salida para asegurar la correcta transmisión de caracteres especiales en sistemas heterogéneos.
- **Validación estructural preventiva:** En entornos críticos, antes de procesar un documento XML, resulta altamente recomendable validarlo contra su esquema (XSD). Esto permite capturar estructuras mal formadas antes de que generen excepciones costosas durante el parseo.
- **Gestión limpia de dependencias:** Mantén el archivo `pom.xml` de Maven organizado. Declara explícitamente las versiones estables de librerías como Gson, Jackson o las implementaciones _runtime_ de JAXB para evitar conflictos de compatibilidad o advertencias de métodos deprecados.

 **Errores comunes**
- **Desbordamiento de memoria (_OutOfMemoryError_):** Ocurre sistemáticamente al intentar cargar un archivo XML de varios Gigabytes utilizando la API DOM, ya que agota el espacio del _Heap_ de Java. La solución pasa por refactorizar el código para utilizar el procesamiento basado en eventos de SAX.
- **Ausencia del constructor vacío en JAXB:** Es el error más frecuente al realizar _Data Binding_. La especificación JAXB exige obligatoriamente que todas las clases POJO anotadas posean un constructor público sin argumentos para poder instanciarlas dinámicamente durante el _Unmarshalling_.
- **Inconsistencia en el mapeo de nombres (JSON):** Al usar librerías como Gson o Jackson, si el nombre de la clave en el archivo JSON (por ejemplo, `"id_vehiculo"`) no coincide exactamente con el nombre del atributo en la clase Java (`idVehiculo`), el valor se inicializará por defecto a `null` o `0`. Se debe solucionar utilizando anotaciones específicas de la librería (como `@SerializedName` en Gson) para vincularlos.
- **Mala gestión de Excepciones de Entrada/Salida:** No capturar adecuadamente excepciones específicas de parseo (como `SAXParseException` o `JAXBException`) y atraparlas en un bloque genérico `Exception` dificulta enormemente la depuración. Utiliza bloques `try-catch` detallados para informar del problema real sin interrumpir la ejecución global.
- **Fuga de recursos (_Resource Leak_):** Se produce al abrir flujos de lectura de archivos y olvidar cerrarlos si ocurre un error a mitad de proceso. Para el manejo de archivos físicos subyacentes, utiliza siempre la estructura `try-with-resources` para garantizar el cierre automático y seguro de los flujos.