# **Principios SOLID**
<br>

# **Principios de Diseño (SOLID)**

## **¿Qué son?**

SOLID es un acrónimo que representa cinco principios fundamentales de la programación orientada a objetos, propuestos por Robert C. Martin (“Uncle Bob”). Estos principios son:

- **S** – Single Responsibility Principle (SRP)
- **O** – Open/Closed Principle (OCP)
- **L** – Liskov Substitution Principle (LSP)
- **I** – Interface Segregation Principle (ISP)
- **D** – Dependency Inversion Principle (DIP)

## **Single Responsibility Principle (SRP)**

### **¿Qué es?**

Una clase debe tener una única razón para cambiar, es decir, debe tener una sola responsabilidad.

### **Ventajas:**

- Código más limpio y enfocado.
- Fácil mantenimiento y pruebas.
- Menor acoplamiento.

### **Desventajas:**

- Puede generar muchas clases pequeñas.
- Más esfuerzo inicial de diseño.

### **Ejemplo en PHP:**

```php
// ❌ Violación de SRP: esta clase hace demasiado.
class Report {
    public function generateReport() {
        // Generar el reporte
    }

    public function saveToFile() {
        // Guardar en archivo
    }

    public function sendEmail() {
        // Enviar por correo
    }
}

// ✅ Aplicando SRP
class ReportGenerator {
    public function generate() {
        // Generar el reporte
    }
}

class FileSaver {
    public function save($report) {
        // Guardar en archivo
    }
}

class EmailSender {
    public function send($report) {
        // Enviar por correo
    }
}
```

## **Open/Closed Principle (OCP)**

### **¿Qué es?**

El software debe estar abierto a la extensión pero cerrado a la modificación.

### **Ventajas:**

- Facilita agregar nuevas funcionalidades sin modificar código existente.
- Menor riesgo de introducir errores.

### **Desventajas:**

- Requiere una buena comprensión de abstracción e interfaces.

### **Ejemplo en PHP:**

```php
// ❌ Violación de OCP: cada vez que agrego un tipo, debo modificar esta clase.
class PaymentProcessor {
    public function pay($type) {
        if ($type === 'paypal') {
            // lógica de PayPal
        } elseif ($type === 'stripe') {
            // lógica de Stripe
        }
    }
}

// ✅ Aplicando OCP
interface PaymentMethod {
    public function pay();
}

class PayPal implements PaymentMethod {
    public function pay() {
        // lógica de PayPal
    }
}

class Stripe implements PaymentMethod {
    public function pay() {
        // lógica de Stripe
    }
}

class PaymentProcessor {
    public function pay(PaymentMethod $method) {
        $method->pay();
    }
}
```

## **Liskov Substitution Principle (LSP)**

### **¿Qué es?**

Los objetos de una clase derivada deben poder sustituir a los de su clase base sin alterar el comportamiento del programa.

### **Ventajas:**

- Favorece herencia correcta.
- Previene errores de tipo.

### **Desventajas:**

- Difícil detectar violaciones sutiles.
- Mal uso puede romper el diseño.

### **Ejemplo en PHP:**

```php
// ❌ Violación de LSP: la clase hija no respeta el comportamiento de la base
class Bird {
    public function fly() {
        // Todos los pájaros vuelan
    }
}

class Penguin extends Bird {
    public function fly() {
        throw new Exception("Los pingüinos no pueden volar");
    }
}

// ✅ Solución: rediseñar jerarquía
interface Bird {
    public function eat();
}

interface FlyingBird extends Bird {
    public function fly();
}

class Sparrow implements FlyingBird {
    public function eat() {}
    public function fly() {}
}

class Penguin implements Bird {
    public function eat() {}
}
```

## **Interface Segregation Principle (ISP)**

### **¿Qué es?**

Una clase no debe verse forzada a implementar interfaces que no usa.

### **Ventajas:**

- Interfaces específicas.
- Menor acoplamiento.

### **Desventajas:**

- Puede generar muchas interfaces.
- Mayor esfuerzo de diseño.

### **Ejemplo en PHP:**

```php
// ❌ Violación de ISP
interface Worker {
    public function work();
    public function eat();
}

class Robot implements Worker {
    public function work() {}
    public function eat() {
        // Los robots no comen
        throw new Exception("No puedo comer");
    }
}

// ✅ Solución: dividir la interfaz
interface Workable {
    public function work();
}

interface Eatable {
    public function eat();
}

class Human implements Workable, Eatable {
    public function work() {}
    public function eat() {}
}

class Robot implements Workable {
    public function work() {}
}
```

## **Dependency Inversion Principle (DIP)**

### **¿Qué es?**

Los módulos de alto nivel no deben depender de módulos de bajo nivel. Ambos deben depender de abstracciones.

### **Ventajas:**

- Mayor flexibilidad y reutilización.
- Facilita pruebas y mantenimiento.

### **Desventajas:**

- Mayor complejidad inicial.
- Puede ser excesivo en proyectos pequeños.

### **Ejemplo en PHP:**

```php
// ❌ Violación de DIP: el controlador depende directamente de MySQL
class MySQLConnection {
    public function connect() {
        // conectar a MySQL
    }
}

class UserController {
    private $db;

    public function __construct() {
        $this->db = new MySQLConnection();
    }
}

// ✅ Aplicando DIP
interface DBConnectionInterface {
    public function connect();
}

class MySQLConnection implements DBConnectionInterface {
    public function connect() {}
}

class UserController {
    private $db;

    public function __construct(DBConnectionInterface $db) {
        $this->db = $db;
    }
}
```

## **Resumen**

| Principio | Qué busca                       | Beneficio clave                   |
| --------- | ------------------------------- | --------------------------------- |
| SRP       | Una clase → Una responsabilidad | Código limpio y modular           |
| OCP       | Extensible sin modificar        | Evita romper funcionalidad previa |
| LSP       | Sustitución segura de clases    | Uso correcto de herencia          |
| ISP       | Interfaces pequeñas y enfocadas | Evita implementaciones forzadas   |
| DIP       | Depender de abstracciones       | Flexibilidad y testabilidad       |
