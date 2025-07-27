# **Introducción a PDO**
<br>

# **PDO**

## **¿Qué es?**

`PDO` significa PHP Data Objects. Es una interfaz orientada a objetos para conectarse a bases de datos. Permite usar el mismo código para conectarte a diferentes tipos de bases de datos (MySQL, PostgreSQL, SQLite, etc.).

## **Ventajas**

- Es más **seguro** (con soporte para prepared statements).
- Es **más limpio y moderno** que `mysqli`.
- **Facilita** el manejo de errores y configuraciones.

## **setAttribute**

### **¿Qué es `setAttribute()`?**

Es un método de la clase `PDO`.

**Sirve para establecer ciertos atributos (parámetros internos) del objeto de conexión.**

```php
$pdo->setAttribute(A, B);
```

***Significa:***

> “*Establecer el atributo A con el valor B en este objeto de conexión*”.
> 

### **Ejemplo:**

```php
$pdo->setAttribute(PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION);
```

***Explicación:***

`PDO::` → Esto accede a constantes definidas dentro de la clase `PDO`.

- `PDO` es la clase.
- `::` es el operador de resolución de ámbito (para acceder a métodos o constantes estáticas).
- `PDO::CONSTANTE` significa que estás accediendo a una constante propia de la clase PDO.

---

`PDO::ATTR_ERRMODE` → Es un atributo interno de PDO. **Significa** “*modo de manejo de errores*”.

**Opciones posibles:**

- **`PDO::ERRMODE_SILENT`** → silencioso (no lanza errores, solo los registra internamente).
- **`PDO::ERRMODE_WARNING`** → lanza `warnings` de PHP.
- **`PDO::ERRMODE_EXCEPTION`** → lanza excepciones (`PDOException`) cuando hay errores (la opción más segura y recomendable).

---

`PDO::ERRMODE_EXCEPTION` → Es el valor que le estás asignando al atributo `ATTR_ERRMODE`.

**Le estás diciendo a PDO:**

> “*Cuando ocurra un error en la base de datos, lanza una excepción, no un warning ni un fallo silencioso.*”
> 

**Esto te permite manejar errores con `try-catch`, lo cual es mucho más limpio y seguro.**

### **Resumen:**

```php
$pdo->setAttribute(PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION);
```

**Le dice al objeto de conexión `$pdo`:**

> “*Si hay un error, lanza una excepción para que lo pueda atrapar con catch()*”.
> 

**¿Por qué es importante?**

- Sin esto, errores como "tabla no encontrada" o "columna inválida" no lanzan errores visibles (muy difícil de depurar).
- Con esto activado, cualquier fallo se puede capturar, loguear y mostrar controladamente, por ejemplo, en formato JSON en una API.