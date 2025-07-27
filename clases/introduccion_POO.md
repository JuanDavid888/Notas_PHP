# POO **(Programación Orientada a Objetos)**
<br>

# **Programación Orientada a Objetos**

## **¿Qué es?**

La Programación Orientada a Objetos (POO) es un paradigma de programación basado en clases y objetos, que permite estructurar programas en componentes reutilizables. A diferencia de paradigmas más antiguos como la programación secuencial o procedural, la POO se centra en modelar el software a través de objetos que interactúan entre sí. En este modelo, el programador organiza datos y comportamientos relacionados en clases, a partir de las cuales se crean objetos que colaboran entre sí para ejecutar el programa.

## **Clases**

El desarrollo de programas orientados a objetos se basa en la creación de clases, que actúan como plantillas para definir datos y comportamientos comunes, y en la instanciación de objetos a partir de ellas. Por ejemplo, una clase "`Animal`" podría incluir atributos como "`edad`" y métodos como "`envejecer`". Así, la clase define una estructura general, mientras que los objetos son instancias concretas que utilizan esa estructura para representar entidades específicas dentro del programa.

### **Modificadores de acceso**

Los modificadores de acceso controlan la visibilidad de atributos y métodos en una clase. En PHP, existen tres:

- ***public:*** acceso desde cualquier lugar.
- ***private:*** acceso solo desde la misma clase.
- ***protected:*** acceso desde la clase y sus subclases.

Estos modificadores ayudan a proteger los datos internos y aplicar el encapsulamiento, promoviendo un diseño más seguro y estructurado.

### **Estructura**

```php
<?php
//Se establece el nombre "MiClase" a la clase
class MiClase
{
    //Atributos de la clase
    public $atributo1;
    public $atributo2;
    public $atributo3;

    //Constructor con el mismo nombre de la clase
    public function __construct(){}

    //Métodos de la clase
    public function metodo1()
    {
        //Método vacío
    }

    public function metodo2()
    {
        return "metodo2";
    }
}
?>
```

***Explicación:***

La estructura básica de una clase en PHP se compone de los siguientes elementos:

- ***Declaración de la clase:*** Se utiliza la palabra clave `class` seguida del nombre de la clase, en este caso, "`MiClase`".
- ***Atributos de la clase:*** Son variables que representan características o propiedades de la clase.
- ***Constructor:*** Es un método especial que se utiliza para crear e inicializar objetos de la clase.
- ***Métodos de la clase:*** Son funciones que definen el comportamiento de la clase.

### **Métodos**

En PHP, las funciones se definen como métodos dentro de una clase, pues permiten encapsular un conjunto de instrucciones que realizan una tarea específica.

**Un método en PHP se declara dentro de una clase utilizando la siguiente sintaxis:**

```php
public function nombreDelMetodo($parametros)
{
    //cuerpo del método
}
```

**A continuación se explican las partes de la construcción de esta función (método):**

- ***Modificadores:*** Son opcionales y pueden especificar el acceso y otros atributos del método (por ejemplo, `public`, `private`, `static`, etc.).
- ***Tipo de retorno:*** En PHP no se especifica de forma obligatoria, pero se pueden definir tipos de retorno con `: tipo`.
- ***Nombre del método:*** Es el identificador del método que se utiliza para invocarlo posteriormente.
- ***Parámetros:*** Son valores de entrada que el método puede recibir. Pueden ser de cualquier tipo de dato y se separan por comas si hay más de uno.
- ***Cuerpo del método:*** El bloque de código que contiene las instrucciones que se ejecutan cuando se invoca el método. Puede contener declaraciones de variables, estructuras de control como bucles y condicionales, y cualquier otra instrucción válida en PHP. Dentro del método también se pueden utilizar los parámetros pasados y devolver con `return` el valor correspondiente.

### **Invocar método**

Para llamar a un método y ejecutar su código, se utiliza la flecha `->` con el nombre del método seguido de paréntesis, donde si el método tiene parámetros, se pasan los valores correspondientes dentro de los paréntesis.

```php
$resultado = $objeto->nombreDelMetodo($argumentos);
```

**Un método tiene los siguientes elementos:**

- ***Tipo de retorno:*** Si el método devuelve un valor, se puede asignar a una variable del tipo correspondiente.
- ***Nombre del método:*** Es el identificador del método que se desea invocar.
- ***Argumentos:*** Son los valores que se pasan al método, en el mismo orden que se declararon los parámetros.

### ***Ejemplo:***

```php
<?php
class Animal
{
  public $tipo;
  public $edad;

  public function __construct($nuevoTipo)
  {
    $this->tipo = $nuevoTipo; //Se le da un tipo al animal
  }

  //Método para obtener la edad del animal
  public function getEdad()
  {
    return $this->edad;
  }

  //Método para establecer la edad del animal
  public function setEdad($nuevaEdad)
  {
    $this->edad = $nuevaEdad;
  }

  //Método para obtener el tipo del animal
  public function getTipo()
  {
    return $this->tipo;
  }
}
?>
```

**Este ejemplo de clase en PHP representa la definición de una clase llamada "Animal", donde el significado de cada elemento es el siguiente:**

- ***Atributos de la clase:*** La clase "Animal" tiene dos atributos: "tipo" y "edad".
- ***Constructor:*** El constructor recibe un parámetro para inicializar "tipo".
- ***Métodos getEdad(), setEdad(), getTipo():*** Permiten acceder y modificar los atributos.

### ***Ejemplo: Usar arrays***

```php
<?php
class Persona {
    public $nombre;
    public $edad;

    public function __construct($nombre, $edad) {
        $this->nombre = $nombre;
        $this->edad = $edad;
    }

    public function saludar() {
        echo "Hola, soy {$this->nombre} y tengo {$this->edad} años.\n";
    }
}

// Array de objetos Persona
$personas = [
    new Persona("Ana", 25),
    new Persona("Luis", 30),
    new Persona("Marta", 22)
];

// Saludar a todos
foreach ($personas as $persona) {
    $persona->saludar();
}

echo "\n--- Seleccionando por índice ---\n";

// Seleccionar el segundo de la lista (índice 1)
$seleccionado = $personas[1];
$seleccionado->saludar(); // Luis
?>

```