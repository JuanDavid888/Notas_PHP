# **Funciones Definidas**
<br>

# **Funciones**

## **¿Qué son?**

Las funciones son bloques de código reutilizablesque realizan una tarea específica. Permiten organizar, reutilizar y mantener el código más limpio y modular

## **Ventajas**

- Evitan repetir el mismo código varias veces.
- Permiten dividir el programa en pequeñas partes lógicas.
- Facilitan mantenimiento y pruebas.

## **Estructura básica**

```php
<?php
function nombreFuncion($parametro1, $parametro2 = valorPorDefecto) {
// Bloque de código
return $resultado;
}
?>
```

***Explicación:***

- `function` → palabra reservada para declarar funciones.
- `nombreFuncion` → identificador de la función.
- `$parametro1, $parametro2` → parámetros (opcionales o con valores por defecto).
- `return` → devuelve un valor (opcional).

### ***Ejemplo 1: función sencilla sin parámetros***

```php
<?php
function saludar() {
echo "¡Hola desde una función!";
}

saludar(); // Llamar a la función
?>
```

### ***Ejemplo 2: función con parámetros***

```php
<?php
function sumar($a, $b) {
return $a + $b;
}

echo sumar(5, 3); // Imprime 8
?>
```

### ***Ejemplo 3: función con valor por defecto***

```php
<?php
function saludarPersona($nombre = "Amigo") {
    echo "¡Hola, $nombre!";
}

saludarPersona("Carlos"); // ¡Hola, Carlos!
saludarPersona();         // ¡Hola, Amigo!
?>
```

### ***Ejemplo 4: devolver múltiples valores (array)***

```php
<?php
function datosUsuario() {
    return array("nombre" => "Ana", "edad" => 28);
}

$usuario = datosUsuario();
echo $usuario["nombre"]; // Ana
?>
```
<br>

# **Tipado en PHP**

PHP es un lenguaje débilmente tipado, pero desde PHP 7 puedes hacer tipado fuerte en:

- **Parámetros de entrada**
    
    ```php
    function saludo(string $nombre) { ... }
    ```
    

- **Tipo de retorno**
    
    ```php
    function getEdad(): int { ... }
    ```
    

## ***Ejemplo 1: Estructura***

```php
function sumar(int $a, int $b): int {
return $a + $b;
}
```

### ***Explicación:***

- `function sumar` → define una función llamada `sumar`.
- `(int $a, int $b)` → indica que espera dos parámetros, ambos deben ser enteros (`int`).
    
    Si pasas otro tipo, PHP lo convertirá o dará error si el `declare(strict_types=1);` está activado.
    
- `: int` → indica que la función devolverá un entero. Es el tipo de retorno.
- `return $a + $b;` → Suma los dos enteros y devuelve el resultado, que debe coincidir con el tipo `int`.

## ***Ejemplo 2: Calcula el total de un producto con nombre y precio.***

```php
// Calcula el total de un producto con nombre y precio.

function calcularTotalProducto(string $nombre, float $precio, int $cantidad): string {
    $total = $precio * $cantidad;
    return "Compraste $cantidad $nombre(s) por un total de $" . number_format($total, 2);
}

echo calcularTotalProducto("camisa", 19.99, 3);
// Compraste 3 camisa(s) por un total de $59.97
?>
```

## ***Ejemplo 3: Calcula el total de un carrito***

```php
// Calcula el total de un carrito.

function calcularTotalCarrito(array $productos): float {
    $total = 0.0;
    foreach ($productos as $producto) {
        $subtotal = $producto['precio'] * $producto['cantidad'];
        $total += $subtotal;
    }
    return $total;
}

$carrito = [
    ["nombre" => "Zapatos", "precio" => 50.0, "cantidad" => 2],
    ["nombre" => "Gorra", "precio" => 15.5, "cantidad" => 1],
    ["nombre" => "Camisa", "precio" => 20.0, "cantidad" => 3]
];

echo "Total a pagar: $" . calcularTotalCarrito($carrito); // Total a pagar: $175.5
?>
```

## **Recomendaciones**

- Usa tipado (`string`, `int`, `float`, `bool`, `array`) para que tu función sea más clara y evites errores.
- Documenta bien con `@param` y `@return`.
- Pon valores por defecto en parámetros opcionales.
- Usa `number_format()` para mostrar decimales bonitos en precios.
- Combina tipos para funciones potentes, por ejemplo, `string` para nombres, `int` para cantidades, `float` para precios, `bool` para opciones.

## **Tips**

- **Usa nombres descriptivos:**
    
    ```php
    function calcularTotalConImpuestos($subtotal) { ... }
    ```
    

- **Documenta tus funciones con comentarios:**
    
    ```php
    /**
     * Calcula el área de un rectángulo
     * @param float $base
     * @param float $altura
     * @return float
     */
    function areaRectangulo($base, $altura) {
        return $base * $altura;
    }
    ```
    

- **Puedes usar tipado escalar (desde PHP 7) para mayor seguridad:**
    
    ```php
    function sumar(int $a, int $b): int {
    return $a + $b;
    }
    ```
    

- **Siempre organiza tus funciones antes del código principal o en archivos separados e inclúyelos con `include` o `require`.**

<br>

# **Funciones de PHP**

Existen varias funciones en PHP que permiten realizar ciertos tipos de operaciones como manipular, estructurar, ordenar, añadir, eliminar, etc.

## **`*count()` - Permite contar los elementos de un arreglo:***

```php
<?php
$frutas = ["Manzana", "Banana", "Cereza"];
echo count($frutas); // Salida: 3
?>
```

## **`*sort()` - Permite ordenar un arreglo de manera ascendente:***

```php
<?php
$números = [5, 3, 8, 1, 2];
sort($números);
print_r($números); // Salida: Array ( [0] => 1 [1] => 2 [2] => 3 [3] => 5 [4] => 8 )
?>
```

## **`*rsort`  - Permite ordenar un arreglo de manera descendente:***

```php
<?php
$números = [5, 3, 8, 1, 2];
rsort($números);
print_r($números); // Salida: Array ( [0] => 8 [1] => 5 [2] => 3 [3] => 2 [4] => 1 )
?>
```

## **`array_push()` - Permite agregar un elemento al final del arreglo:**

```php
<?php
$colores = ["Rojo", "Azul"];
array_push($colores, "Verde", "Amarillo");
print_r($colores); // Salida: Array ( [0] => Rojo [1] => Azul [2] => Verde [3] => Amarillo )
?>
```

## **`array_pop()` - Permite eliminar y retornar el último elemento del arreglo:**

```php
<?php
$animales = ["Perro", "Gato", "Tigre"];
$ultimo = array_pop($animales);
echo $ultimo; // Salida: Tigre
print_r($animales); // Salida: Array ( [0] => Perro [1] => Gato )
?>
```

## **`array_merge()` - Permite combinar dos o más arreglos:**

```php
<?php
$frutas1 = ["Manzana", "Banana"];
$frutas2 = ["Pera", "Uva"];
$resultado = array_merge($frutas1, $frutas2);
print_r($resultado); // Salida: Array ( [0] => Manzana [1] => Banana [2] => Pera [3] => Uva )
?>
```

## **`array_key_exists()` - Permite verificar si una clave existe en un arreglo asociativo:**

```php
<?php
$persona = ["nombre" => "Carlos", "edad" => 25];
if (array_key_exists("edad", $persona)) {
    echo "La clave 'edad' existe."; // Salida: La clave 'edad' existe.
} else {
    echo "La clave 'edad' no existe.";
}
?>
```

## **`*isset()` - Permite verificar si una variable o el elemento de array existe y tiene un valor***

Verifica si una variable está definida y no es `null`.

```php
$miVariable = "Hola";
if (isset($miVariable)) {
    echo "La variable está definida y tiene un valor";
} else {
    echo "La variable no está definida o no tiene valor";
}
```

## `empty()` ***- Permite verificar si una variable o el elemento de array no existe o no tiene un valor***

**Verifica si una variable está vacía. Una variable se considera vacía si:**

- No está definida.
- Tiene el valor `""` (cadena vacía).
- Tiene el valor `0` (entero o cadena).
- Tiene el valor `"0"`.
- Tiene el valor `null`.
- Tiene el valor `false`.
- Es un arreglo vacío `[]`.

```php
$miVariable = "";
if (empty($miVariable)){
echo "La variable está vacia o no está definida";
} else {
echo "La variable tiene un valor";
}
```
<br>

# **Funciones de salida**

En lenguajes de programación, las funciones de salida son aquellos que permiten mostrar información al usuario cuando finaliza la ejecución de un proceso determinado o cuando finaliza el proceso una petición realizada por el usuario final.

## ***echo***

Esta es la función más común para imprimir en PHP. Se utiliza para mostrar una o varias cadenas de texto en la salida del servidor web.

**Ejemplo:**

```php
echo "Hola mundo";
```

## ***print***

Similar a `echo`, pero devuelve siempre 1, por lo que puede usarse en expresiones.

**Ejemplo:**

```php
print "Hola mundo";
```

## **Diferencias clave entre echo y print**

| Característica     | `echo`                   | `print`                  |
| ------------------ | ------------------------ | ------------------------ |
| Devuelve valor     | ❌ No                     | ✅ Sí (siempre `1`)       |
| Parámetros         | ✅ Múltiples              | ❌ Solo uno               |
| Velocidad          | ⚡ Ligeramente más rápida | 🐢 Ligeramente más lenta |
| Uso en expresiones | ❌ No                     | ✅ Sí                     |

## ***print_r()***

Muestra información legible de una variable (útil para arrays).

**Ejemplo:**

```php
$arr = array("a", "b", "c");
print_r($arr);
```

## ***var_dump()***

Muestra información detallada de una variable (tipo, tamaño, valor). Muy útil para depurar.

**Ejemplo:**

```php
$num = 10;
var_dump($num);
```

## ***var_export()***

Similar a `var_dump()`, pero devuelve código PHP válido que representa el valor.

**Ejemplo:**

```php
$arr = array("a", "b");
var_export($arr);
```

## ***printf()***

Imprime texto formateado (como en C).

**Ejemplo:**

```php
printf("El número es: %d", 7);
```

## ***sprintf()***

Similar a `printf()`, pero devuelve el string en lugar de imprimirlo.

**Ejemplo:**

```php
$texto = sprintf("El número es: %d", 7);
echo $texto;
```