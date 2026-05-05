Nombre y apellidos: 

Curso:

### **INSTRUCCIONES GENERALES DEL EXAMEN**

Por favor, lea atentamente las siguientes normas antes de comenzar:
- **Duración:** Disponéis de un tiempo máximo de **2 horas** para la realización de la prueba.
- **Formato:** El examen consta de **40 preguntas tipo test**. Cada pregunta presenta 4 posibles opciones de respuesta, de las cuales **SOLO UNA es correcta**.
- **Sistema de Puntuación:**
	- Cada **3 respuestas incorrectas restan 1 correcta** (cada error penaliza un 33% del valor de un acierto).
	- Las preguntas no contestadas (en blanco) no suman ni restan puntuación.

- **Registro de Respuestas (IMPORTANTE):** Las respuestas definitivas deben marcarse obligatoriamente en la **tabla de respuestas** situada al comienzo del examen. **Cualquier respuesta que no se encuentre trasladada a dicha tabla no será considerada para la calificación**, independientemente de lo marcado en el cuerpo de las preguntas.

![[Pasted image 20260128095039.png]]


**1. ¿Qué resultado se obtiene al acceder al recurso `/public/index.html` después de ejecutar el siguiente código en Express?**
```JavaScript
const express = require('express');
let app = express();
app.use('/public', express.static(__dirname + '/public'));
app.listen(8080);
```
- a) Se redirige automáticamente a la página de inicio sin mostrar contenido estático.
- b) Error 404 porque no se encuentra el recurso.
- c) Se sirve el contenido estático ubicado en la carpeta `/public` del proyecto.
- d) Se muestra el código fuente de `index.html` como texto plano.


**2. Si tenemos la configuración `app.use('/assets', express.static('public'));` y dentro de la carpeta `public` existe una imagen llamada `foto.jpg`, ¿cuál es la etiqueta HTML correcta para mostrarla?**

- a) `<img src="/public/foto.jpg">`
- b) `<img src="/foto.jpg">`
- c) `<img src="/assets/foto.jpg">`
- d) `<img src="/static/foto.jpg">`


**3. ¿Por qué es considerada una buena práctica utilizar el módulo `path` y su método `path.join` al configurar carpetas estáticas en lugar de concatenar cadenas?**

- a) Porque `path.join` convierte automáticamente los archivos HTML a plantillas EJS.
- b) Porque asegura la compatibilidad de las rutas entre diferentes sistemas operativos (Windows utiliza `\` y Linux `/`).
- c) Porque permite que Express cargue los archivos más rápido en memoria.
- d) Porque es obligatorio en las nuevas versiones de Express 5.0.


**4. ¿Cuál es el error en este fragmento al intentar incluir Bootstrap en las páginas HTML después de haberlo instalado vía `npm`?**

```JavaScript
app.use(express.static(__dirname + '/node_modules/bootstrap/dist'));
```

```HTML
<link rel="stylesheet" href="css/bootstrap.min.css">
```

- a) No hay ningún error en el fragmento de código.
- b) Se debe incluir el prefijo `/public` en la ruta del elemento `<link>`.
- c) La ruta debería comenzar con `/node_modules/bootstrap/dist/css/` en el elemento `<link>`.
- d) En el elemento `<link>` la ruta debe comenzar con `/` para indicar la ruta raíz.


**5. ¿Qué se suele hacer cuando se accede a la raíz ("/") de una aplicación Express para llevar al usuario a la página de inicio estática?**

- a) Cargar una página de inicio para indicarle al usuario que vaya a otra página.
- b) Mostrar un mensaje de error.
- c) Redirige a otra URL (por ejemplo, mediante `res.redirect`).
- d) Ninguna opción es correcta.


**6. Si necesitamos enviar un único archivo estático específico en una ruta concreta (no toda una carpeta), ¿qué método del objeto `res` debemos usar?**

- a) `res.sendStatic()`
- b) `res.sendFile()`
- c) `res.render()`
- d) `res.json()`


**7. ¿Qué hace el siguiente fragmento de código en una aplicación Express?**
```JavaScript
app.get('/', (req, res) => {
    res.redirect('/public/index.html');
});
```

- a) Establece `/public/index.html` como la ruta por defecto para el contenido estático.
- b) Redirige al usuario a `/public/index.html` cuando accede a la raíz del sitio.
- c) Redirige todas las peticiones a la carpeta `/public`.
- d) Muestra el contenido de `index.html` en caso de que se produzca un error.


**8. Técnicamente, ¿qué devuelve la función `express.static()`?**

- a) Un objeto JSON con la configuración del servidor.
- b) Un valor booleano indicando si la carpeta existe.
- c) Una función middleware que maneja la petición y respuesta (req, res, next).
- d) Una cadena de texto con la ruta absoluta de la carpeta.


**9. ¿Qué tipo de contenido se puede considerar como estático en una aplicación web?**

- a) Datos de la base de datos.
- b) Datos de sesiones de usuario.
- c) Imágenes, archivos CSS y JavaScript.
- d) Datos de usuarios.


**10. ¿Qué ocurre si utilizamos una ruta relativa como `express.static('public')` en lugar de una absoluta con `__dirname`?**

- a) Express lanzará un error y detendrá la ejecución inmediatamente.
- b) Funcionará siempre correctamente sin importar desde dónde se lance el proceso.
- c) La ruta dependerá del directorio de trabajo actual (CWD) desde donde se ejecuta el comando `node`, lo que puede causar errores si se lanza desde otra carpeta.
- d) Express ignorará la carpeta `public` y buscará en la raíz del disco duro.


**11. ¿Cuál es la extensión de archivo estándar recomendada para las plantillas en Nunjucks?**

- a) `.html`
- b) `.nunjucks`
- c) `.tpl`
- d) `.njk`


**12. Si utilizamos herencia de plantillas con `{% extends 'base.njk' %}`, ¿dónde debe colocarse esta etiqueta dentro del archivo hijo?**

- a) En cualquier parte del archivo.
- b) Justo antes de cerrar el `</body>`.
- c) Debe ser obligatoriamente la primera línea del archivo.
- d) Dentro de un bloque `{% block init %}`.


**13. ¿Qué hace la siguiente configuración en Nunjucks?**

```JavaScript
nunjucks.configure('views', { autoescape: true, express: app });
```

- a) Activa el modo de depuración.
- b) Configura el motor y lo asocia a Express, activando el escape automático de HTML por seguridad.
- c) Permite que Express sirva archivos estáticos desde 'views'.
- d) Desactiva la caché de plantillas.


**14. ¿Cuál es la sintaxis correcta para realizar una operación matemática básica (suma) directamente en la salida de una plantilla?**

- a) `{% 5 + 3 %}`
- b) `{{ 5 + 3 }}`
- c) `{{ math(5 + 3) }}`
- d) No se pueden hacer operaciones matemáticas en la vista.


**15. ¿Qué propiedad de `loop` usarías para saber si estás en la última iteración de un bucle?**

- a) `loop.end`
- b) `loop.finish`
- c) `loop.last`
- d) `loop.final`


**16. ¿Qué filtro utilizarías para convertir un array como `['A', 'B', 'C']` en una cadena de texto "A-B-C"?**


- a) `{{ array | join('-') }}`
- b) `{{ array | stringify }}`
- c) `{{ array | concat('-') }}`
- d) `{{ array | split('-') }}`


**17. ¿Cuál es el error en este fragmento al configurar Nunjucks?**

```JavaScript
const nunjucks = require('nunjucks');
const app = express();
nunjucks.configure('views', { express: app });
```

_(Supón que falta el require de express antes)_

- a) No se puede pasar `app` directamente.
- b) La carpeta views no existe.
- c) Falta requerir el módulo `express` antes de inicializar `app`.
- d) `nunjucks.configure` no acepta parámetros.


**18. ¿Cómo se accede al primer elemento de un array llamado `frutas` dentro de una plantilla?**

- a) `{{ frutas.first }}`
- b) `{{ frutas[0] }}`
- c) `{{ frutas(0) }}`
- d) `{{ frutas.0 }}`


**19. ¿Qué se utiliza para generar contenido dinámico (imprimir variables) en una plantilla Nunjucks?**

- a) `<% %>`
- b) `{{ }}`
- c) `{% %}`
- d) `#{ }`


**20. ¿Cuál es la diferencia principal entre `{% include %}` y `{% extends %}`?**

- a) `include` copia el contenido de otro archivo en el punto actual, mientras que `extends` define una estructura base que la plantilla actual rellena.
- b) `extends` es para archivos CSS y `include` para HTML.
- c) Son sinónimos, hacen exactamente lo mismo.
- d) `include` solo funciona con archivos `.html` y `extends` con `.njk`.


**21. ¿Qué indica el parámetro `extended: true` en la configuración de `express.urlencoded`?**

- a) Que se permite cualquier tipo de dato en la petición.
- b) Que se permite procesar datos complejos como objetos y arrays anidados (usando la librería qs).
- c) Que solo se aceptan cadenas y matrices en la URL.
- d) Que se restringe el tamaño de la petición.


**22. En una vista de edición con Nunjucks, ¿cómo pre-rellenamos un `<input>` con el valor actual del nombre del contacto?**

- a) `<input type="text" name="nombre" text="{{ contacto.nombre }}">`
- b) `<input type="text" name="nombre">{{ contacto.nombre }}</input>`
- c) `<input type="text" name="nombre" value="{{ contacto.nombre }}">`
- d) `<input type="text" name="nombre" placeholder="{{ contacto.nombre }}">`


**23. ¿Cómo se accede al nombre del archivo subido (generado por Multer) dentro del router POST?**

- a) `req.body.filename`
- b) `req.file.filename`
- c) `req.upload.filename`
- d) `req.files.name`


**24. ¿Qué middleware debemos usar para que Express procese correctamente el campo oculto `_method` y transforme la petición POST en PUT o DELETE?**

- a) `body-parser`
- b) `method-override`
- c) `express-validator`
- d) `cookie-parser`


**25. En Nunjucks, ¿cómo se genera dinámicamente el `action` de un formulario de borrado para cada contacto dentro de un bucle `for` (asumiendo que la ruta es `/contactos/:id`)?**

- a) `action="/contactos/{{ contacto.id }}"`
- b) `action="contactos" + {{ contacto.id }}`
- c) `action="{{ '/contactos' + contacto.id }}"`
- d) `action="'/contactos' + contacto.id"`


**26. Si al validar un formulario detectamos errores y queremos volver a mostrar el formulario con los datos que el usuario ya había escrito, ¿qué debemos hacer en el `res.render`?**

- a) Solo pasar los errores: `res.render('vista', { errores })`.
- b) Pasar los errores y el objeto con los datos recibidos: `res.render('vista', { errores, contacto: req.body })`.
- c) Redirigir de nuevo al formulario: `res.redirect('/formulario')`.
- d) No se puede hacer, el usuario debe escribir todo de nuevo.


**27. ¿Cuál es el código Nunjucks correcto para imprimir un valor de una variable llamada `nombre` en un campo de texto?**

```HTML
<input type="text" name="nombre" value="_____">
```

- a) `{{ value=nombre }}`
- b) `{{ nombre }}`
- c) `{{ input nombre }}`
- d) `{{ text(nombre) }}`


**28. ¿Qué ocurrirá si intentamos subir un archivo mediante un formulario que tiene `enctype="multipart/form-data"` pero NO hemos configurado ningún middleware como Multer en la ruta de Express?**

- a) El archivo se sube correctamente y está disponible en `req.body`.
- b) `req.body` estará vacío o incompleto y no podremos acceder al archivo.
- c) Express lanzará un error 500 automáticamente.
- d) El archivo se guarda en la carpeta temporal del sistema operativo.
  

**29. En la validación de esquemas con Mongoose, ¿cómo se define un mensaje personalizado para un campo requerido?**

- a) `required: [true, 'El nombre del contacto es obligatorio']`
- b) `required: "El nombre del contacto es obligatorio"`
- c) `required: [false, 'El nombre del contacto es obligatorio']`
- d) `validate: 'El nombre del contacto es obligatorio'`


**30. Para proteger nuestra aplicación de ataques CSRF en formularios, aunque no es obligatorio en este nivel básico, ¿qué elemento se suele incluir en los formularios POST?**

- a) Un campo oculto con un token CSRF.
- b) Un campo `password` adicional.
- c) Un atributo `secure="true"` en el form.
- d) Nada, Express lo gestiona solo.


**31. ¿Qué se utiliza para saber qué usuarios tienen los permisos adecuados para acceder a determinados recursos en Express mediante sesiones?**

- a) Tokens
- b) Roles
- c) Contraseñas
- d) Ninguna opción es correcta


**32. Si un usuario intenta acceder a una ruta protegida `/admin` y su rol en sesión es `normal`, ¿qué código HTTP es el más adecuado para denegar el acceso?**

- a) 404 Not Found
- b) 200 OK (con mensaje de error)
- c) 403 Forbidden
- d) 500 Internal Server Error


**33. Encuentra el error en el siguiente código que valida usuarios:**

```JavaScript
let usuarioValido = usuarios.find(usuario => 
    usuario.usuario = login && usuario.password = password);
```

- a) Falta definir la variable `usuarios`.
- b) Uso incorrecto del operador de asignación (=) en lugar de comparación (=== ).
- c) Uso incorrecto de `res.redirect`.
- d) La lógica del `find` está invertida.


**34. ¿Qué parámetro de configuración de `express-session` determina si se debe guardar una sesión nueva aunque no se haya modificado nada en ella (útil para cumplir leyes de cookies)?**

- a) `saveUninitialized`
- b) `resave`
- c) `secure`
- d) `httpOnly`


**35. ¿Cuál es el error en este fragmento de código que intenta proteger una ruta para usuarios autenticados mediante un middleware?**

```JavaScript
let autenticacion = (req, res) => {
    if (req.session && req.session.usuario) next();
    else res.redirect('/login');
};
```

- a) La función `autenticacion` no acepta (no tiene definido) el argumento `next`.
- b) La ruta no requiere autenticación.
- c) Uso incorrecto de `res.redirect`.
- d) Falta el `return` antes del redirect.


**36. ¿Dónde se almacenan por defecto los datos de la sesión en `express-session` si no configuramos ningún "store" adicional?**

- a) En una base de datos Redis.
- b) En la memoria RAM del servidor (MemoryStore).
- c) En un archivo de texto plano.
- d) En el navegador del cliente.


**37. ¿Qué hace el siguiente fragmento de código en una aplicación Express?**

```JavaScript
app.use(session({
    secret: '1234',
    resave: true,
    saveUninitialized: false
}));
```

- a) Inicia un servidor HTTPS.
- b) Configura el middleware de sesión con una clave secreta y comportamientos de guardado específicos.
- c) Conecta con la base de datos de usuarios.
- d) Crea una cookie llamada "1234".


**38. ¿Qué riesgo de seguridad tiene usar la configuración por defecto de almacenamiento de sesiones (MemoryStore) en un entorno de producción?**

- a) Las sesiones se borran si el servidor se reinicia y puede haber fugas de memoria.
- b) Las contraseñas se guardan en texto plano.
- c) Es demasiado lento.
- d) No permite guardar arrays.


**39. ¿Qué valor se imprime al ejecutar el siguiente código si el usuario 'nacho' intenta iniciar sesión con la contraseña correcta '12345'?** _(Ver código en pregunta 1 del fichero original)_

- a) undefined
- b) 'Acceso denegado'
- c) 'Acceso concedido'
- d) Error


**40. Para evitar que un script malicioso en el cliente (XSS) pueda leer la cookie de sesión, ¿qué opción de configuración de la cookie es importante mantener en `true` (que lo es por defecto)?**

- a) `secure`
- b) `httpOnly`
- c) `sameSite`
- d) `domain`
