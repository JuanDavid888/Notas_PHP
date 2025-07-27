# **Estructuras Repetitivas**
<br>

# **Estructuras de Control**

## **Estructuras de Control de Repetición (Bucles)**

Permiten repetir una acción varias veces, de forma controlada. Muy útiles cuando se necesita hacer lo mismo muchas veces sin escribir el código una y otra vez.

### ***Ejemplo:***

`for` — Repite un número conocido de veces

```php
for ($i = 1; $i <= 5; $i++) {
    echo "Repetición número $i <br>";
}
```

`while` — Repite mientras se cumpla una condición

```php
$contador = 1;

while ($contador <= 5) {
    echo "Contador: $contador <br>";
    $contador++;
}
```

`do...while` — Ejecuta al menos una vez

```php
$numero = 1;

do {
    echo "Número actual: $numero <br>";
    $numero++;
} while ($numero <= 5);
```

## **Estructuras de Control de Excepciones**

Permiten capturar y manejar errores o situaciones inesperadas en tiempo de ejecución, como intentar dividir entre cero o acceder a un archivo inexistente.

### ***Ejemplo:***

```php
try {
$divisor = 0;
if ($divisor == 0) {
throw new Exception("No se puede dividir por cero");
}
echo 10 / $divisor;
} catch (Exception $e) {
echo "Error: " . $e->getMessage();
}
```

**Explicación:**

- **`try`:** Intenta ejecutar un bloque de código.
- **`throw`:** Lanza una excepción si ocurre un error.
- **`catch`:** Captura y maneja esa excepción.
<br>

# **For y ForEach**

## **¿Qué son?**

Son estructuras de control de flujo que permiten repetir un bloque de código múltiples veces.

Se usan para recorrer datos o ejecutar acciones repetitivas.

## **for**

### **¿Qué es?**

El ciclo `for` se utiliza cuando sabes exactamente cuántas veces quieres repetir un bloque de código.

Ideal para contar o recorrer arrays indexados por posición.

### ***Estructura***

```php
for (inicialización; condición; incremento) {
    // Código a ejecutar en cada iteración
}
```

### ***Ejemplo***

```php
for ($i = 0; $i < 5; $i++) {
    echo "Número: $i<br>";
}
```

- **Inicialización:** `$i = 0` se ejecuta una vez al inicio.
- **Condición:** `$i < 5` se evalúa en cada vuelta; si es `true`, se ejecuta el bloque.
- **Incremento:** `$i++` se ejecuta después de cada iteración.

### **Ventajas**

- Control total del inicio, condición y paso.
- Perfecto para iterar rangos numéricos.

### **Desventajas**

- Puede ser más propenso a errores (por olvidar actualizar el contador).
- Menos intuitivo para recorrer arrays asociativos o colecciones no secuenciales.
<br>

## **foreach**

### **¿Qué es?**

`foreach` se usa para recorrer arrays o colecciones, sin preocuparte de índices ni contadores.

Itera automáticamente sobre cada elemento.

### ***Estructura:*** Solo valores

```php
foreach ($array as $valor) {
    // Código que usa $valor
}
```

### ***Estructura:* Clave y valor**

```php
foreach ($array as $clave => $valor) {
    // Código que usa $clave y $valor
}
```

### ***Ejemplo***

```php
$frutas = array("Manzana", "Banana", "Naranja");

foreach ($frutas as $fruta) {
    echo "Fruta: $fruta<br>";
}

$persona = array("nombre" => "Ana", "edad" => 25);

foreach ($persona as $clave => $valor) {
    echo "$clave: $valor<br>";
}

```

### **Ventajas**

- Muy legible y claro para recorrer arrays y objetos.
- Sin necesidad de preocuparse por índices.

### **Desventajas**

- No sirve para ciclos numéricos arbitrarios (como de 1 a 100 sin array).
- No tienes control del índice salvo que lo pidas explícitamente.