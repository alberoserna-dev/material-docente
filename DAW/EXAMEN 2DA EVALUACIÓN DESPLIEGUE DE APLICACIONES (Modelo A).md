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


| **1**  | **2**  | **3**  | **4**  | **5**  | **6**  | **7**  | **8**  | **9**  | **10** |
| :----: | :----: | :----: | :----: | :----: | :----: | :----: | :----: | :----: | :----: |
|        |        |        |        |        |        |        |        |        |        |
| **11** | **12** | **13** | **14** | **15** | **16** | **17** | **18** | **19** | **20** |
|        |        |        |        |        |        |        |        |        |        |
| **21** | **22** | **23** | **24** | **25** | **26** | **27** | **28** | **29** | **30** |
|        |        |        |        |        |        |        |        |        |        |
| **31** | **32** | **33** | **34** | **35** | **36** | **37** | **38** | **39** | **40** |
|        |        |        |        |        |        |        |        |        |        |
![[Pasted image 20260128094920.png]]


**1. ¿Cuál es el error en el siguiente fragmento de código al intentar servir contenido estático?**
```JavaScript
const express = require('express');
let app = express();
app.use('/public', express.static('/public'));
app.listen(3000);
```
- a) La ruta especificada en `express.static` es incorrecta.
- b) El puerto especificado en `app.listen` no es válido.
- c) Se debería usar `__dirname` para construir la ruta absoluta hacia la carpeta `public`.
- d) No hay ningún error en el fragmento de código.


**2. En el contexto de Node.js y Express, ¿qué valor contiene exactamente la variable global `__dirname`?**

- a) El nombre del archivo principal de la aplicación (ej: index.js).
- b) La ruta absoluta del directorio donde reside el script que se está ejecutando actualmente.
- c) La ruta relativa desde la raíz del sistema operativo hasta la carpeta `node_modules`.
- d) El directorio temporal donde Express almacena la caché de archivos estáticos.


**3. Si definimos dos directorios de contenido estático en Express de la siguiente manera, ¿qué ocurre si existe un archivo llamado `style.css` en ambas carpetas?**
```JavaScript
app.use(express.static(__dirname + '/public'));
app.use(express.static(__dirname + '/files'));
```
- a) Express devuelve un error por conflicto de archivos duplicados.
- b) Express sirve el archivo ubicado en la carpeta `/files` por ser la última definida.
- c) Express sirve el archivo ubicado en la carpeta `/public` por ser la primera definida.
- d) Express fusiona ambos archivos en una sola respuesta HTTP.


**4. ¿Cuál es la sintaxis correcta para configurar el middleware y servir contenido desde la carpeta "public" bajo la ruta virtual "/public"?**
- a) `app.content('/public', express.static(__dirname + '/public'));`
- b) `app.static('/public', express.use(__dirname + '/public'));`
- c) `app.use(express.static(__dirname + '/public'));`
- d) `app.use('/public', express.static(__dirname + '/public'));`


**5. ¿Qué sucede si se intenta acceder a `/css/estilos.css` teniendo la siguiente configuración para Bootstrap y una carpeta pública personalizada?**
```JavaScript
app.use(express.static(__dirname + '/node_modules/bootstrap/dist'));
app.use(express.static(__dirname + '/public'));
```
- a) Se sirven los estilos CSS personalizados desde la carpeta `public`.
- b) Se produce un conflicto entre las rutas de Bootstrap y `public`.
- c) Se accede a los estilos CSS de Bootstrap (si existen en la ruta solicitada).
- d) No se puede acceder a los estilos debido a un error de ruta.


**6. Por defecto, ¿Qué archivo busca `express.static` si un usuario solicita la URL raíz de un directorio estático (por ejemplo, `http://localhost:3000/`)?**

- a) `main.js`
- b) `home.html`
- c) `default.html`
- d) `index.html`


**7. ¿Qué middleware se utiliza en Express para servir contenido estático?**

- a) express-files
- b) express-static
- c) express-public
- d) express-content


**8. Si configuramos `app.use('/static', express.static(__dirname + '/public'));` y tenemos una imagen en `/public/img/logo.png`, ¿cuál es la URL correcta para acceder a ella desde el navegador?**

- a) `http://localhost:3000/public/img/logo.png`
- b) `http://localhost:3000/img/logo.png`
- c) `http://localhost:3000/static/img/logo.png`
- d) `http://localhost:3000/static/public/img/logo.png`


**9. ¿Cuál es el método recomendado para incluir el framework de diseño web Bootstrap en una aplicación Express profesional?**

- a) Descargando e instalando Bootstrap como un módulo de Node.js (npm).
- b) Copiando los archivos de Bootstrap manualmente en la carpeta "public".
- c) Agregando un CDN de Bootstrap en el archivo HTML.
- d) Importando Bootstrap directamente en el código JavaScript de la aplicación.


**10. ¿Qué hace Express si un archivo solicitado NO se encuentra en ninguno de los directorios estáticos configurados con `express.static`?**

- a) Detiene el servidor automáticamente para prevenir errores.
- b) Llama automáticamente a `next()` para pasar el control al siguiente middleware configurado.
- c) Devuelve inmediatamente un error 404 sin consultar otros middlewares.
- d) Crea un archivo vacío con ese nombre en la carpeta pública.


**11. ¿Qué instrucción se utiliza para incluir el contenido de una plantilla dentro de otra en Nunjucks (por ejemplo, un menú)?**

- a) `{% import %}`
- b) `{% insert %}`
- c) `{% include %}`
- d) `{% extend %}`


**12. ¿Cuál es la sintaxis correcta para escribir un comentario en una plantilla Nunjucks que no se renderice en el HTML final?**

- a) ` `` `
- b) `{{-- Comentario --}}`
- c) `{# Comentario #}`
- d) `{% comment %} Comentario {% endcomment %}`


**13. ¿Qué propiedad se utiliza para obtener la posición actual en un bucle, empezando a contar desde 1?**

- a) `loop.index0`
- b) `loop.counter`
- c) `loop.index`
- d) `loop.count`


**14. Si queremos renderizar una variable que contiene código HTML (como `<p>Hola</p>`) y queremos que el navegador lo interprete como HTML en lugar de mostrar las etiquetas como texto, ¿qué filtro debemos usar?**

- a) `{{ variable | html }}`
- b) `{{ variable | safe }}`
- c) `{{ variable | raw }}`
- d) `{{ variable | unescape }}`


**15. ¿Cómo se pasa el listado de contactos a una vista de Nunjucks desde un controlador de Express correctamente?**

```JavaScript
res.render('contactos_listado', { _____ });
```

- a) `resultado: contactos`
- b) `contactos: resultado`
- c) `req, res, resultado`
- d) `resultado, req, res`

**16. ¿Qué resultado mostrará el siguiente código en la plantilla: `{{ "hola" | upper | replace("A", "O") }}`?**

- a) `hola`
- b) `HOLA`
- c) `HOLO`
- d) Da error, no se pueden encadenar filtros.


**17. ¿Qué código se utiliza en Nunjucks para mostrar un mensaje alternativo si un bucle `for` no tiene elementos sobre los que iterar?**

- a) `{% empty %}`
- b) `{% else %}`
- c) `{% if empty %}`
- d) `{% default %}`


**18. ¿Cómo se declara una variable nueva dentro de una plantilla Nunjucks para usarla posteriormente en ese mismo archivo?**

- a) `{% var color = "rojo" %}`
- b) `{{ color = "rojo" }}`
- c) `{% set color = "rojo" %}`
- d) `{% let color = "rojo" %}`


**19. ¿Cómo se define un bloque en una plantilla base para que pueda ser sobrescrito por las plantillas hijas?**

- a) `{{ block nombre }}`
- b) `{% block nombre %} ... {% endblock %}`
- c) `{% define nombre %} ... {% enddefine %}`
- d) `<block name="nombre"> ... </block>`


**20. ¿Qué ocurrirá si intentamos acceder a `{{ usuario.nombre }}` en una plantilla, pero la variable `usuario` no ha sido pasada desde el controlador?**

- a) Se detiene la ejecución del servidor Node.js.
- b) Se muestra el texto `undefined` en el HTML.
- c) Se muestra un error 500 en el navegador.
- d) No se muestra nada (espacio en blanco) y no genera error.


**21. ¿Qué método HTTP se utiliza comúnmente para enviar los datos en un formulario de inserción de un nuevo registro?**

- a) GET
- b) PUT
- c) POST
- d) DELETE


**22. Si queremos simular una petición `PUT` desde un formulario HTML estándar (que solo soporta GET y POST), ¿qué debemos hacer para que el middleware `method-override` lo detecte?**

- a) Añadir `method="PUT"` en la etiqueta `<form>`.
- b) Añadir un campo oculto `<input type="hidden" name="_method" value="put">`.
- c) Enviar los datos como JSON en lugar de usar un formulario.
- d) Usar JavaScript para interceptar el envío y cambiar el método.


**23. ¿Cómo se habilita en Express el procesamiento de los datos enviados desde un formulario (URL-encoded)?**

- a) `app.use(express.json());`
- b) `app.use(express.urlencoded({ extended: true }));`
- c) `app.use(express.parser());`
- d) `app.use(express.urlencoded({ extended: false }));`


**24. ¿Qué librería de Node.js se utiliza habitualmente para gestionar la subida de ficheros (multipart/form-data) en Express?**

- a) formidable
- b) express-upload
- c) multer
- d) body-parser


**25. ¿Cómo se recogen los datos de un formulario en una ruta POST para realizar una inserción en una base de datos?**

- a) Usando `req.query`
- b) Usando `req.params`
- c) Usando `req.body`
- d) Usando `req.data`


**26. En Mongoose, si queremos definir un mensaje de error personalizado para la validación `minlength` de un campo String, ¿cómo debemos definir el esquema?**

- a) `minlength: 5, message: "Error personalizado"`
- b) `minlength: [5, "Error personalizado"]`
- c) `validate: { min: 5, msg: "Error personalizado" }`
- d) `min: [5, "Error personalizado"]`


**27. ¿Cuál es el error en este fragmento al definir un formulario para borrar un contacto?**

```HTML
<form action="/contactos/borrar" method="post">
    <input type="hidden" name="id" value="{{ contacto.id }}">
    <button type="submit">Borrar</button>
</form>
```

- a) Falta incluir un campo oculto `_method` para simular un método "DELETE".
- b) La acción del formulario debería apuntar a "/contactos/{{ contacto.id }}".
- c) Debe usarse el método "DELETE" en lugar de "POST".
- d) No hay ningún error en el fragmento de código.


**28. Al configurar el almacenamiento en disco con Multer (`diskStorage`), ¿qué dos propiedades principales debemos definir?**

- a) `folder` y `name`
- b) `path` y `url`
- c) `destination` y `filename`
- d) `source` y `target`


**29. ¿Qué problema presenta este código al intentar subir un archivo en un formulario?**

```HTML
<form action="/contactos" method="post">
    <input type="file" name="imagen">
    <button type="submit">Enviar</button>
</form>
```

- a) El tipo de input debe ser "text" en lugar de "file".
- b) No se puede subir archivos usando el método "POST".
- c) Falta el atributo `enctype="multipart/form-data"` en la etiqueta `<form>`.
- d) No hay ningún error en el fragmento de código.


**30. Si tenemos un input `<input type="text" name="edad">` y queremos validar en el servidor que el valor recibido es numérico usando la librería `validator.js`, ¿qué función usaríamos?**

- a) `validator.isNumber(req.body.edad)`
- b) `validator.isNumeric(req.body.edad)`
- c) `req.body.edad.isNumber()`
- d) `check(req.body.edad).isNumeric()`


**31. ¿Cuál es el comando para instalar el módulo `express-session` necesario para manejar sesiones en una aplicación Express?**

- a) `npm start express-session`
- b) `npm install express`
- c) `node install express-session`
- d) `npm install express-session`


**32. ¿Qué propiedad del objeto `req` utilizamos para almacenar y recuperar datos específicos de la sesión de un usuario (como su nombre o rol)?**

- a) `req.cookie`
- b) `req.session`
- c) `req.body`
- d) `req.data`


**33. ¿Qué parámetro se utiliza para establecer una clave de cifrado (secreto) para firmar la cookie de sesión en Express?**

- a) `sessionKey`
- b) `secret`
- c) `token`
- d) `key`


**34. ¿Qué valor imprime el siguiente código si intentamos acceder a `req.session.contador` sin haberlo inicializado previamente?**

```JavaScript
app.get('/', (req, res) => {
    console.log(req.session.contador);
    res.send('Hola');
});
```

- a) 0
- b) null
- c) undefined
- d) Error: session not found


**35. ¿Para qué se utiliza el parámetro "resave" en la configuración de sesiones en Express?**

- a) Para guardar la sesión de nuevo en el almacén, incluso si no ha habido cambios durante la petición.
- b) Para cifrar los datos de la sesión.
- c) Para definir el tiempo de vida de la sesión.
- d) Para guardar sesiones nuevas aunque no se hayan inicializado.


**36. Si queremos destruir explícitamente una sesión (por ejemplo, en un Logout), ¿qué método debemos llamar?**

- a) `req.session.delete()`
- b) `req.session.destroy()`
- c) `req.session.remove()`
- d) `req.logout()`


**37. ¿Cuál es el error en este fragmento al intentar verificar si un usuario está autenticado en Express?**

```JavaScript
app.get('/dashboard', (req, res) => {
    if (!req.session.usuario) {
        res.redirect('/login');
    } else {
        res.render('dashboard.njk');
    }
});
```

- a) Falta pasar datos a la plantilla `dashboard.njk`.
- b) `req.session.usuario` no es la manera correcta de verificar la autenticación.
- c) Se debe utilizar `req.isAuthenticated()` para verificar la autenticación.
- d) No hay ningún error en el fragmento de código.


**38. ¿Cuál es el flujo correcto cuando un usuario hace Login con éxito en una aplicación con sesiones?**

- a) El servidor envía un token JWT al cliente.
- b) El servidor guarda los datos del usuario en `req.session` y el cliente recibe una cookie con el ID de sesión.
- c) El cliente envía sus datos en cada petición posterior.
- d) El servidor redirige a `/logout`.


**39. Identifica el error en el siguiente fragmento de código que intenta configurar una sesión en Express:**

```JavaScript
app.use(session({
    secret: '1234',
    resave: true,
    saveUninitialized: false;
}));
```

- a) Uso incorrecto de comillas en el valor de `secret`.
- b) Uso incorrecto del punto y coma (;) después de `saveUninitialized: false`.
- c) No se importa el módulo `express-session` correctamente.
- d) `resave` debe ser `false` obligatoriamente.


**40. ¿Qué propiedad del objeto `req.session.cookie` sirve para establecer la fecha de caducidad absoluta de la cookie de sesión?**

- a) `timeout`
- b) `maxAge` (o `expires`)
- c) `limit`
- d) `duration`
