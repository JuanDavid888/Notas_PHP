# **Herencia y Polimorfismo**
<br>

# **Herencia**

## **¿Qué es?**

La herencia permite que una clase (llamada subclase o clase hija) herede atributos y métodos de otra clase (la superclase o clase padre).

### **Ventajas:**

- Reutilizar código común.
- Evitar duplicación.
- Permitir especialización.

### **Ejemplo:**

```php
class Animal {
    public $nombre;

    public function __construct($nombre) {
        $this->nombre = $nombre;
    }

    public function hablar() {
        echo "Hace un sonido.";
    }
}

class Perro extends Animal {
    public function hablar() {
        echo $this->nombre . " dice: Guau!";
    }
}

$miPerro = new Perro("Firulais");
$miPerro->hablar(); // Firulais dice: Guau!
```

Se observa que la clase `Perro` **hereda** de `Animal` y **sobrescribe (override)** el método `hablar`.

### **`$this`**

**`$this` es una referencia al objeto actual.**

Sirve para acceder a los atributos y métodos del propio objeto dentro de la clase.

> Es como decir: "*yo mismo, este objeto*".
> 

***Ejemplo***

```php
class Persona {
    public $nombre;

    public function __construct($nombre) {
        $this->nombre = $nombre; // asigna a SU propio atributo
    }

    public function saludar() {
        echo "Hola, soy " . $this->nombre;
    }
}

$p = new Persona("Juan");
$p->saludar(); // Hola, soy Juan

```

- Aquí `$this->nombre` accede al atributo `nombre` del objeto actual.
- `$this->saludar()` llamaría a otro método del mismo objeto.

### **`parent`**

**`parent` es una palabra especial que hace referencia a la clase padre (superclase).**

Se usa para:

- llamar métodos o constructores heredados desde la clase padre.
- extender o sobrescribir comportamiento, pero invocando la lógica base.

> Es como decir: "*usa lo que está definido en la clase padre*".
> 

***Ejemplo***

```php
class Animal {
    public function __construct() {
        echo "Animal creado\n";
    }

    public function hablar() {
        echo "Hace un sonido genérico\n";
    }
}

class Perro extends Animal {
    public function __construct() {
        parent::__construct(); // llama al constructor del padre
        echo "Perro creado\n";
    }

    public function hablar() {
        parent::hablar(); // opcional: llama al método del padre
        echo "Guau guau\n";
    }
}

$p = new Perro();
// Salida:
// Animal creado
// Perro creado

$p->hablar();
// Hace un sonido genérico
// Guau guau
```

### Ejemplo 2:

```php
<?php
class Usuario
{
    protected $nombre;
    protected $documento;

    public function __construct($nombre, $documento)
    {
        $this->nombre = $nombre;
        $this->documento = $documento;
    }

    public function getNombre()
    {
        return $this->nombre;
    }

    public function setNombre($nombre)
    {
        $this->nombre = $nombre;
    }

    public function getDocumento()
    {
        return $this->documento;
    }

    public function setDocumento($documento)
    {
        $this->documento = $documento;
    }
}
?>
```

En primer lugar, está la clase padre que es `Usuario`, donde se definen todos los atributos y métodos (constructor, getters y setters) definidos en el diagrama. En este ejemplo aparece la palabra `$this` en el método constructor y en los métodos setters. En este caso `$this` se utiliza para hacer referencia al objeto actual en el que se está ejecutando un método o constructor y diferenciar el atributo del argumento pasado en el método.

```php
<?php
class Estudiante extends Usuario
{
    private $codigoEstudiante;

    public function __construct($codigoEstudiante, $nombre, $documento)
    {
        parent::__construct($nombre, $documento);
        $this->codigoEstudiante = $codigoEstudiante;
    }

    public function getCodigoEstudiante()
    {
        return $this->codigoEstudiante;
    }

    public function setCodigoEstudiante($codigoEstudiante)
    {
        $this->codigoEstudiante = $codigoEstudiante;
    }
}
?>
```

En la clase `Estudiante` lo primero que se hace es decirle que hereda de `Usuario` con la palabra `extends`, pues permite usar entonces los atributos y métodos (incluido el constructor) de la clase que hereda. Como se puede ver en el constructor de `Estudiante`, se piden los atributos declarados en `Usuario` y al asignarlos se usa `parent::__construct()` para invocar el constructor de la clase padre. La palabra clave `parent` sirve para hacer referencia a la superclase desde una subclase.

```php
<?php
$estudiante1 = new Estudiante(
    "123456",
    "Pepito Perez",
    "1098000000"
);

echo $estudiante1->getNombre();
?>
```

Para terminar este análisis de un caso sencillo de herencia, se puede crear un objeto de tipo `Estudiante`, se pasan los atributos que hereda de `Usuario` y los propios de la clase `Estudiante`; luego se puede ver que es posible acceder a un método de la clase `Usuario` como propio de él, pues la herencia implica que todos los métodos y atributos que tiene la clase padre son reconocidos como propios en las clases hijas.

## **Polimorfismo**

### **¿Qué es?**

El polimorfismo significa que un mismo método puede comportarse de distintas formas según el objeto que lo invoque.

**En PHP se logra mediante:**

- Herencia + sobrescritura de métodos.
- Interfaces.

### **Ejemplo simple con herencia:**

```php
$animales = [
    new Perro("Boby"),
    new Gato("Michi")
];

foreach ($animales as $animal) {
    $animal->hablar(); // Cada objeto responde de manera diferente
}
```

Así, aunque todos tienen el método `hablar()`, cada clase define su propio comportamiento.

## **Interfaces**

### **¿Qué son?**

Una interfaz define un contrato: declara qué métodos debe implementar una clase, pero sin su implementación.

### **Ventajas:**

- Permite definir reglas que varias clases distintas deben cumplir.
- Facilita el polimorfismo.

### Ejemplo:

```php
Interface Figura {
    public function calcularArea();
}

class Circulo implements Figura {
    private $radio;

    public function __construct($radio) {
        $this->radio = $radio;
    }

    public function calcularArea() {
        return pi() * pow($this->radio, 2);
    }
}

class Cuadrado implements Figura {
    private $lado;

    public function __construct($lado) {
        $this->lado = $lado;
    }

    public function calcularArea() {
        return pow($this->lado, 2);
    }
}

$figuras = [new Circulo(3), new Cuadrado(4)];

foreach ($figuras as $figura) {
    echo $figura->calcularArea() . "\n";
}
```

Todas las clases que implementan `Figura` están obligadas a implementar `calcularArea()`.

## **Getter y Setters**

### **¿Qué son?**

Los getters y setters son métodos usados para leer y modificar atributos privados o protegidos, garantizando el encapsulamiento.

### **Ejemplo:**

```php
class Persona {
    private $nombre;

    public function setNombre($nombre) {
        $this->nombre = $nombre;
    }

    public function getNombre() {
        return $this->nombre;
    }
}

$p = new Persona();
$p->setNombre("Juan");
echo $p->getNombre(); // Juan
```

Esto controla el acceso a los datos, permitiendo validación o lógica adicional.

## **Resumen rápido**

| Concepto         | ¿Qué es?                                               |
|------------------|--------------------------------------------------------|
| Herencia         | Clase hija hereda métodos/atributos.                  |
| Polimorfismo     | Mismo método → comportamientos distintos.             |
| Interfaces       | Contratos que obligan a implementar métodos.          |
| Getter/Setter    | Métodos para acceder o modificar atributos.           |
| Modificadores    | Controlan visibilidad, uso y herencia.                |
