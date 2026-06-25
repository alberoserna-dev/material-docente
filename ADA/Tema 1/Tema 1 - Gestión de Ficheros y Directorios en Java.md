--- title: "Gestión de Ficheros y Directorios en Java" ---
### Acceso al sistema de archivos desde Java
#### Clase File en Java
La clase File de Java forma parte del paquete java.io y se utiliza para representar rutas de archivos y directorios en el sistema de archivos. No proporciona métodos para leer o escribir directamente en los archivos, sino que sirve como un objeto abstracto que describe la ruta y permite realizar operaciones relacionadas con la gestión de archivos.
Características principales

##### Algunas de las características más importantes de la clase File son:
- Representa rutas de archivos y directorios, tanto relativos como absolutos.
- Permite crear, eliminar y renombrar archivos o directorios.
- Proporciona métodos para comprobar permisos de lectura, escritura y ejecución.
- Permite obtener información como tamaño, fecha de modificación y ruta absoluta.
- Puede listar los archivos contenidos en un directorio.

##### Ejemplo de uso
A continuación se muestra un ejemplo sencillo de uso de la clase File: 

```Java
import java.io.File;

public class EjemploFile {
	public static void main(String[] args) { 
		File archivo = new File("ejemplo.txt"); 
		if (archivo.exists()) { 
			System.out.println("El archivo existe."); 
			System.out.println("Ruta absoluta: " + archivo.getAbsolutePath());
			System.out.println("Tamaño: " + archivo.length() + " bytes");
		} else { 
			System.out.println("El archivo no existe. Creando..."); 
			try { 
				if (archivo.createNewFile()) { 
					System.out.println("Archivo creado correctamente."); 
				} 
			} catch (Exception e) {
				e.printStackTrace(); 
			} 
		} 
	} 
}
```

> [!Ejercicio 1]
Crea un programa Java que reciba como parámetro el nombre de un Directorio y muestre por consola la lista de archivos que contiene.

> [!Ejercicio 2]
> Amplia el programa anterior para que muestre recursivamente la lista de archivos y directorios que contiene el Directorio recibido por parámetro.

#### Clases de Java para leer ficheros de texto
En Java existen diferentes clases y enfoques para leer ficheros de texto. La elección depende de si se busca simplicidad, eficiencia o un control más avanzado sobre la codificación. A continuación, se explican las principales clases con ejemplos prácticos.

##### FileReader
La clase FileReader permite leer caracteres directamente desde un archivo. Es adecuada para leer texto básico, pero normalmente se combina con BufferedReader para mejorar el rendimiento. 

Este método lee un solo carácter y te devuelve un **`int`** (el valor numérico de ese carácter en la tabla Unicode, por ejemplo, 65 para la 'A'). Es por esto que es necesario el casteo a `char` que se muestra en el ejemplo:

```Java
public class EjemploFileReader { 
	public static void main(String[] args) { 
		try (FileReader fr = new FileReader("archivo.txt")) { 
			int c; 
			while ((c = fr.read()) != -1) { 
				System.out.print((char)c); 
			} 
		} catch (IOException e) { 
			e.printStackTrace(); 
		} 
	} 
}
```
##### BufferedReader
BufferedReader permite leer texto de manera eficiente, especialmente línea por línea. Se suele utilizar junto con FileReader o InputStreamReader. Importante advertir que en este caso ya no es necesario el casteo.

Este método internamente, lee esos carácteres numéricos que ofrecía FileReader, los junta en su array de memoria (Buffer), y cuando detecta un salto de línea (`\n`), empaqueta todos esos caracteres y fabrica el `String`. El casteo de `int` a `char` ya está hecho dentro del tratamiento realizado por la clase `BufferedReader`.

```Java
public class EjemploBufferedReader {
	public static void main(String[] args) { 
		try (BufferedReader br = new BufferedReader(new                                         FileReader("archivo.txt"))) { 
			String linea; 
			while ((linea = br.readLine()) != null) { 
				System.out.println(linea); 
			} 
		} catch (IOException e) { 
			e.printStackTrace();
		} 
	} 
}
```

##### InputStreamReader
InputStreamReader convierte un flujo de bytes en caracteres, permitiendo especificar una codificación como UTF-8. Se combina habitualmente con BufferedReader.

Dado que InputStreamReader trabaja con flujo de Bytes, no es posible usarlo con la clase FileReader que usábamos hasta ahora, la cual leía caracteres a través de su codificación (`int`). Es por ello que, para usar InputStreamReader, accedemos al fichero a través de la clase FileInputStream, que nos ofrece el flujo de bytes correspondiente al contenido del fichero.

```Java
public class EjemploInputStreamReader { 
	public static void main(String[] args) { 
		try (BufferedReader br = new BufferedReader( new InputStreamReader(new FileInputStream("archivo.txt"), "UTF-8"))) { 
			String linea; 
				while ((linea = br.readLine()) != null) { 
					System.out.println(linea); 
				} 
		} catch (IOException e) { 
			e.printStackTrace(); 
		}
	} 
}
```

este apilamiento de triple `new` (`new BufferedReader(new InputStreamReader(new FileInputStream(...)))`) se le llama en ingeniería de software el **Patrón Decorador** (Decorator Pattern). Y resulta fácil de entender si lo imaginamos como la  **línea de ensamblaje en una fábrica**:

1. **Primer nivel de la línea (El Minero - `FileInputStream`):** Su único trabajo es ir a la mina (el disco duro) y sacar piedras en bruto (Bytes). Hace una sola cosa, pero la hace muy bien.
    
2. **Línea intermedia (El Fundidor - `InputStreamReader`):** Recoge las piedras en bruto del minero y las funde usando un molde específico ("UTF-8") para convertirlas en piezas reconocibles (Caracteres).
    
3. **Línea final (El Empaquetador - `BufferedReader`):** Recoge las piezas del fundidor, las mete en su almacén temporal (buffer) y te entrega cajas completas y ordenadas (Líneas de texto / `String`).

##### Files.readAllLines
El método Files.readAllLines lee todas las líneas de un archivo y las devuelve en una lista de Strings. Es una forma sencilla de obtener todo el contenido de un archivo pequeño o mediano. Es importante tener en cuenta que, dado que lee todas las líneas del fichero, si el archivo es grande puede suponer problemas de memoria de la aplicación.

```Java
public class EjemploFilesReadAllLines { 
	public static void main(String[] args) { 
		try { 
			List<String> lineas = Files.readAllLines( Paths.get("archivo.txt"), StandardCharsets.UTF_8); 
			lineas.forEach(System.out::println); 
		} catch (IOException e) { 
			e.printStackTrace(); 
		}
	} 
}
```

##### Files.lines
Files.lines devuelve un Stream de las líneas de un archivo, lo que permite procesarlas con las funcionalidades de Streams de Java 8 o superior.

```Java
public class EjemploFilesLines { 
	public static void main(String[] args) { 
		try (Stream<String> stream = Files.lines( Paths.get("archivo.txt"), StandardCharsets.UTF_8)) { 
			stream.forEach(System.out::println); 
		} catch (IOException e) { 
			e.printStackTrace(); 
		} 
	} 
}
```

##### Scanner
La clase Scanner de java.util se puede utilizar para leer archivos de texto de manera sencilla, línea a línea o token por token. Permite definir la codificación del archivo.

```Java
public class EjemploScanner { 
	public static void main(String[] args) { 
		try (Scanner sc = new Scanner(new File("archivo.txt"), "UTF-8")) { 
			while (sc.hasNextLine()) { 
				System.out.println(sc.nextLine()); 
			} 
		} catch (IOException e) { 
			e.printStackTrace(); 
		} 
	} 
}
```

>[!Ejercicio 3]
>Crea un programa que lea si existe un fichero llamado ”log.txt” mostrando por consola una línea por cada línea del fichero.

>[!Ejercicio 4]
>Crea un programa que cuente cuántas líneas tiene un archivo de texto.

>[!Ejercicio 5]
>Crea un programa que pida al usuario el nombre de un fichero de texto y muestre en consola la cantidad (total) de vocales que contiene.

>[!Ejercicio 6]
>Crea un programa Java que reciba como parámetro el nombre de un archivo de texto y muestre por pantalla todas las palabras que contenga que empiecen por A sin distinguir mayúsculas de minúsculas

#### Clases Java para escribir ficheros de texto

En Java, existen varias clases dentro del paquete java.io que permiten escribir ficheros de texto. Entre las más utilizadas se encuentran FileWriter, BufferedWriter y PrintWriter. A continuación, se explican sus usos con ejemplos prácticos y algunos ejercicios propuestos.

##### FileWriter
La clase FileWriter se utiliza para escribir caracteres en un archivo. Permite escribir directamente en el fichero, aunque no es la más eficiente al trabajar con grandes volúmenes de datos.

```Java
public class EjemploFileWriter { 
	public static void main(String[] args) { 
		try { 
			FileWriter writer = new FileWriter("archivo.txt"); 
			writer.write("Hola, mundo!"); 
			writer.close(); 
		} catch (IOException e) { 
			e.printStackTrace(); 
		}
	}
}
```

##### BufferedWriter
BufferedWriter se utiliza junto con FileWriter para mejorar la eficiencia, ya que almacena los datos en un buffer antes de escribirlos en el archivo.

```Java
public class EjemploBufferedWriter { 
	public static void main(String[] args) {
		try { 
			FileWriter fw = new FileWriter("archivo.txt"); 
			BufferedWriter bw = new BufferedWriter(fw); 
			bw.write("Línea con buffer"); 
			bw.newLine(); 
			bw.write("Otra línea"); 
			bw.close(); 
		} catch (IOException e) { 
			e.printStackTrace(); 
		}
	}
}
```

##### PrintWriter
PrintWriter permite escribir texto en un archivo de manera más sencilla, similar a cómo funciona System.out.println.

```Java
public class EjemploPrintWriter { 
	public static void main(String[] args) { 
		try { 
			PrintWriter pw = new PrintWriter("archivo.txt"); 
			pw.println("Primera línea"); 
			pw.println("Segunda línea"); 
			pw.close(); 
		} catch (FileNotFoundException e) { 
			e.printStackTrace(); 
		}
	}
}
```

>[!Ejercicio 7]
>Crea un programa que añada a un fichero ”log.txt” una nueva línea en cada ejecución, que contendrá la fecha (en formato 19-09-2019), la hora (17:02:09).

>[!Ejercicio 8]
>Crea un programa que pida al usuario el nombre de un fichero de texto y vuelque su contenido a otro fichero, pero invirtiendo el orden de las líneas (la última pasará a ser la primera, la penúltima será la segunda y así sucesivamente).

>[!Ejercicio 9]
>Escribe un programa que solicite al usuario ingresar 5 frases por teclado y las guarde en un archivo de texto, cada una en una línea diferente.

>[!Ejercicio 10]
>Crea un programa que copie el contenido de un archivo de texto en otro utilizando FileReader y BufferedWriter.

>[!Ejercicio 11]
>Crea un programa que escriba en un fichero ”triangle.txt” un triángulo creciente de asteriscos, del ancho y alto escogido por el usuario.

#### Clases Java para leer ficheros binarios

##### Clase FileInputStream en Java
La clase FileInputStream en Java forma parte del paquete java.io y se utiliza para leer datos en bruto desde un archivo. Opera a nivel de bytes, por lo que es adecuada para leer datos binarios como imágenes, audio, vídeo o cualquier tipo de archivo que no sea de texto plano.

###### Características principales de FileInputStream:
- Permite leer archivos byte por byte.
- Es más apropiada para datos binarios que para texto.
- Se puede combinar con otras clases como InputStreamReader o BufferedInputStream.
- Lanza excepciones de tipo IOException si ocurre un error en la lectura.

###### Constructores:
1. `FileInputStream(String nombreArchivo)`
		Crea un objeto FileInputStream para leer desde el archivo cuyo nombre se pasa por parámetro.
2. `FileInputStream(File archivo)`
		Crea un objeto FileInputStream usando un objeto File existente
3. `FileInputStream(FileDescriptor fd)`
		Crea un objeto FileInputStream usando un descriptor de archivo (nivel más bajo).

###### Métodos más comunes:
- `int read()` - Lee un solo byte del archivo (devuelve -1 si es fin de archivo).
- `int read(byte[] b)` - Lee bytes y los almacena en el array proporcionado.
- `int read(byte[] b, int off, int len)` - Lee hasta len bytes y los coloca en b desde la posición off.
- `void close()` - Cierra el flujo de entrada y libera recursos asociados.
- `int available()` - Devuelve el número estimado de bytes que pueden leerse sin bloquear.

###### Ejemplo de uso
```Java
public class EjemploFileInputStream {
	public static void main(String[] args) { 
		try (FileInputStream fis = new FileInputStream("archivo.txt")) { 
			int byteLeido; 
			while ((byteLeido = fis.read()) != -1) { 
				System.out.print((char) byteLeido); 
			} 
		} catch (IOException e) {
			e.printStackTrace();
		} 
	}
}
```

##### Clase BufferedInputStream en Java

`BufferedInputStream` mejora el rendimiento de `FileInputStream` al utilizar un buffer intermedio. Es útil cuando se trabaja con archivos grandes.

###### Ejemplo de uso
```Java
public class EjemploBufferedInputStream { 
	public static void main(String[] args) {
		try { 
			FileInputStream fis = new FileInputStream("archivo.bin");
			BufferedInputStream bis = new BufferedInputStream(fis); 
			int byteData; 
			while ((byteData = bis.read()) != -1) { 
				System.out.print(byteData + " "); 
			} 
			bis.close(); 
			} 
		catch (IOException e) { 
			e.printStackTrace(); 
		}
	}
}
```

##### Clase DataInputStream en Java
`DataInputStream` permite leer datos primitivos de manera más cómoda (`int`, `double`, `char`, etc.) a partir de un InputStream.

###### Ejemplo de uso
```Java
public class EjemploDataInputStream { 
	public static void main(String[] args) {
		try { 
			FileInputStream fis = new FileInputStream("datos.bin");
			DataInputStream dis = new DataInputStream(fis); 
			int numero = dis.readInt(); 
			double decimal = dis.readDouble(); 
			System.out.println("Número: " + numero); 
			System.out.println("Decimal: " + decimal); 
			dis.close();
		} catch (IOException e) { 
			e.printStackTrace(); 
		}
	}
}
```

>[!Ejercicio 11]
> Crea un programa Java capaz de copiar todo el contenido de un fichero en otro fichero. Debes leer todo el fichero inicial y guardarlo en un único bloque que tendrá el mismo tamaño que el fichero. Los nombres de ambos ficheros pueden estar prefijados o ser pedidos al usuario, como prefieras.

>[!Ejercicio 12]
> Crea un programa que pida al usuario el nombre de un fichero y diga si parece un BMP válido (sus dos primeros bytes son B y M) y, en ese caso, su ancho y su alto. Debes leer por bloques (la cabecera es un bloque de 54 bytes).
> https://es.wikipedia.org/wiki/Windows_bitmap

>[!Ejercicio 13]
>Crea un programa que pida al usuario el nombre de un fichero BMP y diga si está comprimido (deberás buscar información sobre cómo es la cabecera de un BMP, que deberás leer como bloque).

>[!Ejercicio 14]
>Desarrolla un programa que copie un archivo binario en otro utilizando BufferedInputStream y BufferedOutputStream.

##### Clase RandomAccessFile en Java
La clase RandomAccessFile de Java forma parte del paquete java.io y se utiliza para leer y escribir en archivos de manera aleatoria, es decir, se puede acceder a cualquier posición dentro del archivo sin necesidad de leerlo secuencialmente desde el inicio.

###### Características principales de RandomAccessFile en java:
Algunas de las características más importantes de la clase RandomAccessFile son:
- Permite tanto la lectura como la escritura en un archivo.
- Se puede mover el puntero de archivo a cualquier posición mediante el método seek().
- Puede abrirse en diferentes modos de acceso: solo lectura ("r") o lectura/escritura ("rw").
- Proporciona métodos para leer y escribir datos primitivos (int, long, char, etc.).
- Es útil para aplicaciones que necesitan acceso directo a partes específicas de un archivo, como bases de datos o índices.

###### Ejemplo de uso
```Java
public class EjemploRandomAccessFile { 
	public static void main(String[] args) { 
		try { 
			// Abrir el archivo en modo lectura/escritura
			RandomAccessFile raf = new RandomAccessFile("datos.txt", "rw");
			
			// Escribir datos en el archivo 
			raf.writeUTF("Hola mundo");
			raf.writeInt(123); 
			
			// Mover el puntero al inicio 
			raf.seek(0); 
			// Leer los datos escritos 
			String texto = raf.readUTF(); 
			int numero = raf.readInt(); 
			
			System.out.println("Texto leído: " + texto); 
			System.out.println("Número leído: " + numero); 
			raf.close(); 
		} catch (Exception e) { 
			e.printStackTrace(); 
		} 
	} 
}
```

##### Cabecera ID3 v1 en archivos mp3
La cabecera ID3 v1 es un estándar utilizado en archivos MP3 para almacenar metadatos relacionados con la pista de audio. Fue introducido en 1996 y permite incluir información básica sobre la canción, como el título, el artista, el álbum y el género, entre otros.

###### Estructura de ID3 v1
La cabecera ID3 v1 ocupa los últimos 128 bytes de un archivo MP3 y está formada por diferentes campos:
- 3 bytes: Identificador 'TAG' que marca el inicio de la cabecera.
- 30 bytes: Título de la canción.
- 30 bytes: Nombre del artista.
- 30 bytes: Nombre del álbum.
- 4 bytes: Año de publicación.
- 30 bytes: Comentarios (en ID3 v1.1, los últimos 2 bytes se usan para número de pista).
- 1 byte: Género (representado por un número que corresponde a una lista predefinida).

###### Limitaciones
- Solo permite texto en codificación ISO-8859-1 (ASCII extendido).
- Los campos tienen longitud fija y limitada (ejemplo: 30 caracteres para el título).
- No soporta caracteres especiales o textos largos.
- Fue reemplazado progresivamente por ID3 v2, que ofrece mucha más flexibilidad.

###### Ejemplo de cabecera ID3 v1
Un esquema de cómo se verían los últimos 128 bytes de un archivo MP3 con cabecera ID3 v1:

| **offset (bytes)** | **Campos**  |
| ------------------ | ----------- |
| 0-2                | "TAG"       |
| 3-32               | Título      |
| 33-62              | Artista     |
| 63-92              | Álbum       |
| 93-96              | Año         |
| 97-126             | Comentarios |
| 127                | Género      |

###### Ejemplo de lectura de cabecera ID3 en Java con FileInputStream:

```Java
public class LectorID3v1 { 
	public static void main(String[] args) { 
		if (args.length != 1) { 
			System.out.println("Uso: java LectorID3v1 <archivo.mp3>");
			return; 
		} 
		String fileName = args[0]; 
		File file = new File(fileName); 
		
		if (!file.exists()) { 
			System.out.println("El archivo no existe."); 
			return; 
		} 
		
		try (FileInputStream fis = new FileInputStream(file)) { 
		// La cabecera ID3v1 ocupa 128 bytes al final 
		if (file.length() < 128) { 
			System.out.println("El archivo es demasiado pequeño para contener una cabecera ID3v1."); 
			return; 
		} 
		
		fis.skip(file.length() - 128); 
		byte[] buffer = new byte[128]; 
		fis.read(buffer); 
		
		String tag = new String(buffer, 0, 3, StandardCharsets.ISO_8859_1);
		if (!"TAG".equals(tag)) { 
			System.out.println("No se encontró cabecera ID3v1 en este archivo."); 
			return; 
		} 
		
		String titulo = new String(buffer, 3, 30, StandardCharsets.ISO_8859_1).trim();
		String artista = new String(buffer, 33, 30, StandardCharsets.ISO_8859_1).trim(); 
		String album = new String(buffer, 63, 30, StandardCharsets.ISO_8859_1).trim(); 
		String anio = new String(buffer, 93, 4, StandardCharsets.ISO_8859_1).trim(); 
		String comentario = new String(buffer, 97, 30, StandardCharsets.ISO_8859_1).trim(); 
		int genero = buffer[127] & 0xFF; // unsigned byte
		
		System.out.println("== Cabecera ID3v1 =="); 
		System.out.println("Título: " + titulo); 
		System.out.println("Artista: " + artista); 
		System.out.println("Álbum: " + album); 
		System.out.println("Año: " + anio); 
		System.out.println("Comentario: " + comentario); 
		System.out.println("Género (código): " + genero); 
		
		} catch (IOException e) { 
			e.printStackTrace(); 
		} 
	} 
}
```

###### Ejemplo de lectura de cabecera ID3 en Java con RandomAccessFile:

```Java
public class LectorID3v1RAF { 
	public static void main(String[] args) { 
		if (args.length != 1) { 
			System.out.println("Uso: java LectorID3v1RAF <archivo.mp3>");
			return; 
		} 
		
		String fileName = args[0]; 
		File file = new File(fileName); 
		
		if (!file.exists()) { 
			System.out.println("El archivo no existe."); 
			return; 
		} 
		
		try (RandomAccessFile raf = new RandomAccessFile(file, "r")) { 
			if (raf.length() < 128) { 
				System.out.println("El archivo es demasiado pequeño para contener una cabecera ID3v1."); 
				return; 
			} 
			
			// Ir a los últimos 128 bytes 
			raf.seek(raf.length() - 128); 
			byte[] buffer = new byte[128]; 
			raf.readFully(buffer);
			String tag = new String(buffer, 0, 3, StandardCharsets.ISO_8859_1); 
			
			if (!"TAG".equals(tag)) { 
				System.out.println("No se encontró cabecera ID3v1 en este archivo."); 
				return; 
			} 
			
			String titulo = new String(buffer, 3, 30, StandardCharsets.ISO_8859_1).trim(); 
			String artista = new String(buffer, 33, 30, StandardCharsets.ISO_8859_1).trim(); 
			String album = new String(buffer, 63, 30, StandardCharsets.ISO_8859_1).trim(); 
			String anio = new String(buffer, 93, 4, StandardCharsets.ISO_8859_1).trim(); 
			String comentario = new String(buffer, 97, 30, StandardCharsets.ISO_8859_1).trim(); 
			int genero = buffer[127] & 0xFF; 
			
			System.out.println("== Cabecera ID3v1 (RandomAccessFile) ==");
			System.out.println("Título: " + titulo); 
			System.out.println("Artista: " + artista); 
			System.out.println("Álbum: " + album); 
			System.out.println("Año: " + anio); 
			System.out.println("Comentario: " + comentario); 
			System.out.println("Género (código): " + genero); 	
		} catch (IOException e) { 
			e.printStackTrace(); 
		} 
	} 
}
			
```

> [!Ejercicio 15]
>  Crea un pequeño programa en Java que reciba como parámetro el nombre de un archivo MP3 y muestre por pantalla los siguientes datos de la cabecera ID3V1: Título, Artista, Álbum, Año, Comentario y Género.

>[!Ejercicio 16]
> Modifica el programa anterior de forma que reciba por parámetro un Directorio y muestre por pantalla todos los archivos mp3 existentes con sus cabeceras ID3V1 correspondientes.

>[!Ejercicio 17]
>Modifica el programa anterior de forma que recorra recursivamente todos los directorios existentes en el Directorio inicial y muestre los archivos MP3 existentes en cada uno.

##### Serialización y deserialización de objetos en Java
La serialización en Java permite convertir un objeto en una secuencia de bytes para almacenarlo en un fichero binario o transmitirlo por red. La deserialización realiza el proceso inverso, reconstruyendo el objeto a partir de los bytes. Esto se implementa principalmente con las clases ObjectOutputStream y ObjectInputStream del paquete java.io.

###### Requisitos básicos
- La clase debe implementar la interfaz java.io.Serializable (marcador, sin métodos).
- Se recomienda declarar un campo estático final long serialVersionUID para el control de versiones.
- Los campos marcados como transient no se serializan.
- Las referencias a otros objetos también deben ser serializables (o marcarlas transient).

###### Ejemplo básico: guardar y leer un objeto
Clase de dominio: 
```Java
class Persona implements Serializable { 
	private static final long serialVersionUID = 1L; 
	String nombre; 
	int edad; 
	transient String password; // no se serializa 
	
	List<String> aficiones; // también deben ser serializables 
	
	Persona(String nombre, int edad, String password, List<String> aficiones) { 
		this.nombre = nombre; 
		this.edad = edad; 
		this.password = password; 
		this.aficiones = aficiones; 
	} 
}
```

Serializar a un fichero binario (try-with-resources): 
```Java
try (FileOutputStream fos = new FileOutputStream("personas.bin");
ObjectOutputStream oos = new ObjectOutputStream(fos)) { 
	Persona p = new Persona( "Ana", 30, "secreta", List.of("leer", "senderismo")); 
	oos.writeObject(p); 
	} catch (IOException e) { 
		e.printStackTrace(); 
	}
```

Deserializar desde un fichero binario: 
```Java
try (FileInputStream fis = new FileInputStream("personas.bin");
ObjectInputStream ois = new ObjectInputStream(fis)) { 
	Persona pLeida = (Persona) ois.readObject(); 
	System.out.println(pLeida.nombre + " - " + pLeida.edad); 
} catch (IOException | ClassNotFoundException e) { 
	e.printStackTrace(); 
}
```

###### Serializar colecciones de objetos
Cualquier colección estándar de Java (ArrayList, HashMap, etc.) que sea Serializable puede guardarse con writeObject. Ejemplo: escribir una lista de Personas.

```Java
List<Persona> lista = new ArrayList<>(); 
lista.add(new Persona("Ana", 30, "x", List.of("leer"))); 
lista.add(new Persona("Luis", 28, "y", List.of("música"))); 

try (ObjectOutputStream oos = new ObjectOutputStream( new FileOutputStream("lista.bin"))) { 
	oos.writeObject(lista); 
}
```

Y leerla: 
```Java
try (ObjectInputStream ois = new ObjectInputStream( new FileInputStream("lista.bin"))) { 
	@SuppressWarnings("unchecked") List<Persona> listaLeida = (List<Persona>) ois.readObject(); 
	listaLeida.forEach(p -> System.out.println(p.nombre)); 
}
```

###### Control de versiones con serialVersionUID
