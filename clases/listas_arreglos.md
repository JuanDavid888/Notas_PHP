# **Listas y Arreglos**
<br>

# **Arrays**

## **¿Qué son?**

Un array en PHP es una estructura de datos que permite almacenar múltiples valores en una sola variable. Es como una lista o un mapa que puede tener índices numéricos o asociativos (claves de texto).

## **Ventajas**

- Almacenar muchos datos en una sola variable.
- Acceso fácil mediante índices o claves.
- Muy útiles para iterar con `foreach`.
- Flexiblesa al poder mezclar tipos de datos.

## ***Ejemplo:***

```php
$productos = array(
    "P001" => "Laptop",
    "P002" => "Tablet",
    "P003" => "Smartphone"
);

foreach ($productos as $codigo => $nombre) {
    echo "$codigo : $nombre <br>";
}
```

## **Tipos de array**

PHP tiene arrays muy flexibles:

### **🐝 Indexados**

Índices numéricos automáticos (0,1,2,…).

### ***Estructura***

```php
$frutas = array("Manzana", "Banana", "Naranja");
echo $frutas[1]; // Banana
```

### **🐇 Asociativos**

Con claves nombradas (ej: `"nombre" => "Juan"`).

### ***Estructura***

```php
$persona = array(
    "nombre" => "Juan",
    "edad" => 30,
    "ciudad" => "Bogotá"
);
echo $persona["nombre"]; // Juan
```

### **🐺 Multidimensionales**

Arrays dentro de arrays.

### ***Estructura***

```php
$personas = array(
    array("Juan", 30, "Bogotá"),
    array("Ana", 25, "Medellín")
);
echo $personas[1][0]; // Ana

```
<br>

# **Manipulación de cadenas de texto**

La **manipulación de cadenas de texto** es el conjunto de técnicas y funciones que permiten **modificar, analizar, transformar o trabajar con textos (strings)** dentro de un programa.

## ***Declaración de una Cadena de Texto***

```php
$nombreCliente = "Campers Campuslands";
```

Aquí, se define la variable `$nombreCliente` con la cadena `"Campers Campuslands"`.

## ***Conocer la Longitud de una Cadena con `strlen()`***

```php
echo strlen($nombreCliente);
```

***Salida esperada:***

```php
18
```

**Explicación:** `strlen()` devuelve la cantidad de caracteres en la cadena, incluyendo espacios en blanco.

## ***Mostrar Tipo y Valor con `var_dump()`***

```php
var_dump($nombreCliente);
```

***Salida esperada:***

```php
string(18) "Campers Campuslands"
```

**Explicación:** `var_dump()` muestra el tipo de dato (`string`), su longitud (`18 caracteres`) y su contenido (`"Campers Campuslands"`).

## ***Eliminar Espacios en Blanco con `trim()`***

```php
$texto = trim($nombreCliente);
echo strlen($texto);
```

***Salida esperada:***

```php
18
```

**Explicación:** `trim()` elimina espacios en blanco al inicio y al final de una cadena. En este caso, como `"Campers Campuslands"` no tiene espacios extras, la longitud sigue siendo `18`.

## ***Convertir a Mayúsculas con `strtoupper()`***

```php
echo strtoupper($nombreCliente);
```

***Salida esperada:***

```php
CAMPERS CAMPUSLANDS
```

**Explicación:** `strtoupper()` convierte la cadena a mayúsculas.

## ***Convertir a Minúsculas con `strtolower()`***

```php
echo strtolower($nombreCliente);
```

***Salida esperada:***

```php
campers campuslands
```

**Explicación:** `strtolower()` convierte la cadena a minúsculas.

## ***Comparar Correos Ignorando Mayúsculas***

```php
$mail1 = "correo@correo.com";
$mail2 = "Correo@correo.com";
var_dump(strtolower($mail1) === strtolower($mail2));
```

***Salida esperada:***

```php
bool(true)
```

**Explicación:**

- Se convierten ambos correos a minúsculas con `strtolower()`.
- Se comparan con `===`, que verifica si son idénticos en valor y tipo.
- Como `"correo@correo.com" === "correo@correo.com"`, devuelve `true`.

## ***Reemplazar Texto con `str_replace()`***

```php
echo str_replace('Juan', 'J', $nombreCliente);
```

***Salida esperada:***

```php
Campers Campuslands
```

**Explicación:** `str_replace('Juan', 'J', $nombreCliente);` busca `"Juan"` en `$nombreCliente`, pero como `"Juan"` no está presente, la cadena queda igual.

## ***Buscar una Subcadena con `strpos()`***

```php
echo strpos($nombreCliente, 'Pedro');
```

***Salida esperada:***

```php
(no imprime nada)
```

**Explicación:**

- `strpos($nombreCliente, 'Pedro')` busca la palabra `"Pedro"` en `"Campers Campuslands"`.
- Como `"Pedro"` no está presente, devuelve `false`, y `echo` no imprime nada.

## ***Concatenación de Strings***

```php
$tipoCliente = "Premium - Empresarial";
echo "<br>";
echo "El Cliente " . $nombreCliente . " es " . $tipoCliente;
```

***Salida esperada:***

```php
El Cliente Campers Campuslands es Premium - Empresarial
```

**Explicación:**

- Se usa el operador `.` para concatenar cadenas.
- Se concatena `"El Cliente "`, el valor de `$nombreCliente`, `" es "` y el valor de `$tipoCliente`.

## ***Uso de Comillas Dobles `{}` en Cadenas***

```php
echo "El Cliente {$nombreCliente} es {$tipoCliente} ";
```

***Salida esperada:***

```php
El Cliente Campers Campuslands es Premium - Empresarial
```

**Explicación:** Usando `{$variable}` dentro de una cadena con comillas dobles `"..."`, PHP interpreta la variable sin necesidad de concatenar.