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

