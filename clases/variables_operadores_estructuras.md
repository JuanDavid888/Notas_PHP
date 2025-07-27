# **Variables Básicas, Operadores y Estructuras de Decisión**
<br>

# **Variables**

## **¿Qué son?**

- Son espacios en memoria donde puedes almacenar datos que pueden cambiar durante la ejecución.
- todas las variables empiezan con el signo `$` .
- No necesitas declarar el tipo de dato (PHP es débilmente tipado).
- Sensibles a mayúsculas/minúsculas (`$Name` y `$name` son diferentes).

## ***Ejemplo:***

```php
<?php
$name = "Juan";
$age = 25;
$height = 1.75;
?>
```
<br>

# **Constantes**

## **¿Qué son?**

- Son valores que no cambian durante la ejecución.
- Se definen con `define` o con `const`.

## **Diferencia de define y const**

- `define()` se usa fuera de clases.
- `const` puede usarse también dentro de clases.

## **Características**

**1.** ***Son inmutables:*** Una vez asignado un valor, no puede cambiarse.

**2.** ***No llevan el símbolo `$`:*** Se diferencian de las variables por no usar `$`.

**3.** Deben tener nombres en mayúsculas por convención.

**4.** Tienen un ámbito global, lo que significa que pueden ser accedidas desde cualquier parte del código.

## ***Ejemplo:***

```php
<?php
define("PI", 3.1416);
const APP_NAME = "Mi Aplicación";
?>
```
<br>

# **Tipos de datos**

En PHP, existen varios tipos de datos que se pueden utilizar para almacenar diferentes tipos de información. Los tipos de datos más comunes son los siguientes:

- ***Enteros (int):*** Se utilizan para almacenar números enteros sin decimales.
- ***Punto flotante (float):*** Se utilizan para almacenar números con decimales.
- ***Cadenas de texto (string):*** Se utilizan para almacenar texto y caracteres.
- ***Booleanos (bool):*** Se utilizan para almacenar valores de verdad o falsedad, que se representan por true o false.
- ***Arreglos (array):*** Se utilizan para almacenar una colección de datos, que pueden ser de diferentes tipos.
- ***Objetos (object):*** Se utilizan para almacenar instancias de clases, que son definiciones de objetos.
- ***Recursos (resource):*** Se utilizan para almacenar referencias a recursos externos, como conexiones a bases de datos o archivos abiertos.
- ***Nulos (null):*** Se utilizan para representar una variable sin valor o sin definir.

## ***Ejemplo:***

```php
<?php
$texto = "Hola";
$numero = 5;
$decimal = 3.14;
$activo = true;
$colores = array("rojo", "verde", "azul");
?>
```
<br>

# **Operadores**

PHP ofrece una amplia gama de operadores matemáticos, lógicos y de comparación que permiten realizar cálculos, evaluar condiciones y controlar el flujo del programa.

## **Operadores matemáticos**

Son similares a los usados en matemáticas y otros lenguajes (como `+`, `-`, `*`, `/`, `%`), y permiten realizar operaciones aritméticas básicas.

### ***Ejemplo:***

```php
<?php
$a = 10;
$b = 3;

echo $a + $b;  // Suma: 13
echo $a - $b;  // Resta: 7
echo $a * $b;  // Multiplicación: 30
echo $a / $b;  // División: 3.333...
echo $a % $b;  // Módulo: 1 (el resto de 10 / 3)
?>
```

## **Operadores lógicos**

Se utilizan para combinar o invertir condiciones lógicas (`&&`, `||`, `!`, `and`, `or`, etc.), esenciales en estructuras de control como `if`, `while`, o `for`.

### ***Ejemplo:***

```php
<?php
$a = true;
$b = false;

var_dump($a && $b);  // false (ambos deben ser verdaderos)
var_dump($a || $b);  // true (uno de los dos es verdadero)
var_dump(!$a);       // false (invierte el valor de $a)
?>
```

## **Operadores de comparación**

Comparan valores y devuelven resultados booleanos (`==`, `!=`, `>`, `<`, `>=`, `<=`, `===`, `!==`), fundamentales para tomar decisiones en el código.

### ***Ejemplo:***

```php
<?php
$x = 5;
$y = "5";

var_dump($x == $y);   // true (compara solo el valor)
var_dump($x === $y);  // false (compara valor y tipo)
var_dump($x != $y);   // false
var_dump($x !== $y);  // true
var_dump($x > 3);     // true
var_dump($x <= 5);    // true
?>
```

## **Operador de nave espacial**

El operador `<=>` compara dos valores y devuelve:

| Comparación             | Resultado | Significado                  |
|-------------------------|-----------|------------------------------|
| Izquierda < Derecha     | -1        | El primer valor es menor     |
| Izquierda = Derecha     | 0         | Ambos valores son iguales    |
| Izquierda > Derecha     | 1         | El primer valor es mayor     |

<br>

Este operador se utiliza mucho para ordenar datos, ya que devuelve directamente el resultado necesario para saber si un valor debe ir antes, igual o después que otro.

### ***Ejemplo:***

```php
<?php
$numero1 = 20;
$numero2 = 30;
$numero3 = 30;

var_dump($numero1 <=> $numero2); // 20 <=> 30
// Resultado: int(-1)
// Explicación: 20 es menor que 30, devuelve -1

var_dump($numero2 <=> $numero3); // 30 <=> 30
// Resultado: int(0)
// Explicación: 30 es igual a 30, devuelve 0

var_dump($numero2 <=> $numero1); // 30 <=> 20
// Resultado: int(1)
// Explicación: 30 es mayor que 20, devuelve 1
?>
```
<br>

# **Estructuras de control**

## **¿Qué son?**

**Las estructuras de control son bloques de código que permiten controlar el flujo de ejecución de un programa, es decir, decidir qué instrucciones se ejecutan, cuándo y cuántas veces.**

**Sin estas estructuras, el código se ejecutaría línea por línea de forma estrictamente secuencial, sin posibilidad de tomar decisiones ni repetir tareas.**

## ***Estructuras de Control Condicionales***

Permiten ejecutar una parte del código solo si se cumple una condición. Son como decir:

***“Si ocurre esto, haz esto otro”.***

### ***Ejemplo:***

`if`, `else`, `elseif`:

```php
$edad = 20;

if ($edad >= 18) {
echo "Eres mayor de edad";
} else {
echo "Eres menor de edad";
}
```

`switch`:

```php
$dia = "lunes";

switch ($dia) {
	case "lunes":
		echo "Inicio de semana";
		break;
	case "viernes":
		echo "¡Por fin viernes!";
		break;
	default:
	echo "Día normal";
}
```