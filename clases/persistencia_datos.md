# **Persistencia de Datos**
<br>

# **Persistencia de Datos**

## **¿Qué es?**

La persistencia de datos se refiere a la capacidad de almacenar datos de forma permanente para que puedan ser recuperados y utilizados después de que finalice la ejecución de un programa. Es crucial para aplicaciones que necesitan mantener información entre sesiones (por ejemplo, usuarios, configuraciones, historial).

## **Ejemplos comunes de persistencia:**

- Archivos planos (.txt, .csv, .json, .xml)
- Bases de datos (MySQL, SQLite, MongoDB)
- Archivos binarios

## **Ventajas:**

- Permite guardar el estado de una aplicación.
- Facilita el análisis posterior de datos.
- Se pueden usar archivos simples sin necesidad de una base de datos completa.

## **Desventajas:**

- Puede ser más lento o inseguro que una base de datos.
- Más propenso a errores de lectura/escritura si no se controla bien el formato.

## **Manejo de Archivos**

### **¿Qué es?**

El manejo de archivos permite crear, leer y modificar archivos desde un programa. En PHP, por ejemplo, se hace usando funciones como `fopen`, `fwrite`, `fread`, `fgets`, etc.

## **Manejo de Archivos - Creación de un Archivo**

### **¿Qué es?**

Consiste en generar un nuevo archivo en el sistema con un nombre y una extensión específica.

### **Ejemplo en PHP:**

```php
$archivo = fopen("datos.txt", "w"); // crea el archivo para escritura
fclose($archivo);
```

**Modo comunes de apertura (`fopen`)**

- **`"w"`:** Escritura (borra contenido previo)
- **`"a"`:** Escritura (agrega al final)
- **`"r"`:** Lectura

## **Manejo de Archivos - Lectura Línea por Línea**

### **¿Qué es?**

Leer un archivo línea por línea permite procesar grandes archivos sin cargar todo en memoria.

### **Ejemplo en PHP:**

```php
$archivo = fopen("datos.txt", "r");
while (($linea = fgets($archivo)) !== false) {
    echo $linea;
}
fclose($archivo);
```

### **Ventajas:**

- Eficiente en consumo de memoria.
- Útil para archivos grandes o procesamiento secuencial.

## **Manejo de Archivos - Escritura en un Archivo**

### **¿Qué es?**

Consiste en añadir contenido al archivo, ya sea sobrescribiendo o agregando al final.

### **Ejemplo en PHP:**

```php
$archivo = fopen("datos.txt", "a"); // modo append
fwrite($archivo, "Nueva línea de texto\n");
fclose($archivo);
```

### **Ventajas:**

- Guarda registros o resultados.
- Muy útil para logs o bitácoras.

### **Desventajas:**

- Si se usa incorrectamente el modo de apertura, se puede perder información.

## **Manejo de Archivos - Lectura Completa de un Archivo**

### **¿Qué es?**

Cargar el contenido completo de un archivo en una variable o estructura.

### **Ejemplo en PHP:**

```php
$contenido = file_get_contents("datos.txt");
echo $contenido;
```

### **Ventajas:**

- Fácil de manipular si el archivo es pequeño.

### **Desventajas:**

- No recomendado para archivos grandes.

<br>

# **Serialización de Objetos**

## **¿Qué es?**

La serialización convierte un objeto en una secuencia de bytes o cadena que puede almacenarse en un archivo o transmitirse, y luego reconstruirse (deserialización).

## **¿Para qué sirve?**

- Guardar objetos en archivos.
- Enviar objetos por red (APIs, sockets).
- Persistir estados complejos (clases, configuraciones).

## **Ejemplo en PHP:**

```php
class Usuario {
    public $nombre;
    public $correo;
}

$u = new Usuario();
$u->nombre = "Juan";
$u->correo = "juan@email.com";

// Serializar
$datosSerializados = serialize($u);
file_put_contents("usuario.txt", $datosSerializados);
```
<br>

# **Deserialización**

### **¿Qué es?**

Es el proceso inverso a la serialización: tomar los datos serializados y reconstruir el objeto original.

### **Ejemplo:**

```php
$datos = file_get_contents("usuario.txt");
$objeto = unserialize($datos);
echo $objeto->nombre; // Imprime: Juan
```

## **Ventajas de la Serialización:**

- Permite guardar estructuras complejas.
- Útil para cache o almacenamiento temporal.

## **Desventajas de la Serialización:**

- No es legible por humanos.
- Puede ser inseguro si se manipula de forma externa (posibles ataques de inyección).
- No todos los objetos son serializables (por ejemplo, recursos como conexiones abiertas).