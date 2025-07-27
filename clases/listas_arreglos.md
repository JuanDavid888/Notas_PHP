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