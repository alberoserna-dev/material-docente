--- title: "ANEXO: Consultas en JPA" ---
### 1. Búsquedas de Texto (Strings)

Ideal para crear buscadores o filtrar por nombres.

| **Java**                             | **Lo que hace en SQL**                 | **Para qué sirve**                                                |
| ------------------------------------ | -------------------------------------- | ----------------------------------------------------------------- |
| `findByNombre(String n)`             | `... WHERE nombre = 'n'`               | Búsqueda exacta.                                                  |
| `findByNombreContaining(String n)`   | `... WHERE nombre LIKE '%n%'`          | **El más usado.** Busca si _contiene_ el texto (ej: "Concierto"). |
| `findByNombreStartingWith(String n)` | `... WHERE nombre LIKE 'n%'`           | Busca si _empieza_ por ese texto.                                 |
| `findByNombreIgnoreCase(String n)`   | `... WHERE UPPER(nombre) = UPPER('n')` | Ignora mayúsculas/minúsculas (ej: "pepe" encuentra "Pepe").       |

### 2. Números y Fechas (Rangos)

Fundamental para filtrar productos por precio o eventos por fecha.

| **Java**                                  | **Lo que hace en SQL**             | **Para qué sirve**                      |
| ----------------------------------------- | ---------------------------------- | --------------------------------------- |
| `findByPrecioGreaterThan(double p)`       | `... WHERE precio > p`             | Mayor que.                              |
| `findByPrecioLessThan(double p)`          | `... WHERE precio < p`             | Menor que.                              |
| `findByFechaAfter(LocalDate f)`           | `... WHERE fecha > f`              | Eventos futuros (después de tal fecha). |
| `findByFechaBefore(LocalDate f)`          | `... WHERE fecha < f`              | Eventos pasados.                        |
| `findByPrecioBetween(double a, double b)` | `... WHERE precio BETWEEN a AND b` | Rango de precios (ej: entre 10€ y 50€). |
### 3. Combinar Condiciones (AND / OR)

| **Java**                                        | **Lo que hace en SQL**                    |
| ----------------------------------------------- | ----------------------------------------- |
| `findByNombreAndPrecio(String n, double p)`     | `... WHERE nombre = n AND precio = p`     |
| `findByNombreOrDescripcion(String n, String d)` | `... WHERE nombre = n OR descripcion = d` |
### 4. Ordenar y Limitar (Top X)
| **Java**                 | **Lo que hace en SQL**                        |
| ------------------------ | --------------------------------------------- |
| `...OrderByFechaAsc()`   | `... ORDER BY fecha ASC` (De antiguo a nuevo) |
| `...OrderByPrecioDesc()` | `... ORDER BY precio DESC` (De caro a barato) |
| `findTop3By...`          | `... LIMIT 3` (Solo devuelve los 3 primeros)  |
### 5. Booleanos y Existencia (True/False)
| **Lo que escribes en Java**   | **Lo que hace en SQL**    | **Resultado**                              |
| ----------------------------- | ------------------------- | ------------------------------------------ |
| `existsByEmail(String email)` | `SELECT COUNT(*) > 0 ...` | Devuelve `true` o `false` (muy eficiente). |