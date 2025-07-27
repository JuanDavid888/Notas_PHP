# **Patrones de Diseño**
<br>

# **Patrones de diseño**

## **¿Qué son?**

Los patrones de diseño (*Design Patterns*) son soluciones reutilizables a problemas comunes en el diseño de software. No son código listo para usar, sino plantillas que indican cómo resolver un problema de manera estructurada y eficiente, promoviendo la reutilización, la escalabilidad y el mantenimiento del código.

## **Ventajas:**

- **Reutilización de soluciones probadas.**
- **Facilitan la comunicación entre desarrolladores** (un lenguaje común).
- **Mejoran la organización** del código y su mantenimiento.
- **Fomentan** buenas prácticas de programación orientada a objetos.

## **Desventajas:**

- Pueden hacer que el código sea **más complejo si se usan innecesariamente**.
- Algunos patrones requieren **más clases o abstracciones**, lo cual puede ser excesivo en proyectos pequeños.

## **Clasificación de Patrones de Diseño**

**Los patrones de diseño se dividen generalmente en 3 categorías:**

1. ***Creacionales:*** Cómo crear objetos (ej: Factory Method, Singleton).
2. ***Estructurales:*** Cómo organizar clases y objetos (ej: Adapter, Composite).
3. ***Comportamiento:*** Cómo se comunican los objetos (ej: Observer, Strategy).

## **Factory Method (Creacional)**

### **¿Qué es?**

Permite crear objetos sin especificar su clase exacta, delegando esa decisión a subclases o métodos.

### **¿Cuándo usarlo?**

Cuando el código necesita crear objetos sin depender de su tipo exacto.

### **Ejemplo simple:**

```php
interface Transporte {
    public function entregar(): string;
}

class Camion implements Transporte {
    public function entregar(): string {
        return "Entrega por camión";
    }
}

class Barco implements Transporte {
    public function entregar(): string {
        return "Entrega por barco";
    }
}

abstract class Logistica {
    abstract public function crearTransporte(): Transporte;
}

class LogisticaTerrestre extends Logistica {
    public function crearTransporte(): Transporte {
        return new Camion();
    }
}

class LogisticaMaritima extends Logistica {
    public function crearTransporte(): Transporte {
        return new Barco();
    }
}

// Uso
$logistica = new LogisticaTerrestre();
$transporte = $logistica->crearTransporte();
echo $transporte->entregar();  // Entrega por camión
```

### **Ventajas:**

- Aísla la lógica de creación de objetos.
- Facilita la extensión del sistema con nuevas clases de productos.

### **Desventajas:**

- Más clases y estructura.
- Puede complicar el diseño si no es necesario.

## **Adapter (Estructural)**

### **¿Qué es?**

ermite que dos clases incompatibles trabajen juntas, actuando como un puente (adaptador) entre ellas.

### **¿Cuándo usarlo?**

Cuando ya tienes una clase existente que no se puede cambiar, pero necesitas que funcione con una nueva interfaz.

### **Ejemplo simple:**

```php
// Clase existente (no se puede modificar)
class EnchufeEuropeo {
    public function conectar(): string {
        return "Conectado con enchufe europeo";
    }
}

// Interfaz que espera el sistema
interface EnchufeAmericano {
    public function conectar110(): string;
}

// Adaptador
class AdaptadorEuropeoAamericano implements EnchufeAmericano {
    private $enchufeEuropeo;

    public function __construct(EnchufeEuropeo $enchufe) {
        $this->enchufeEuropeo = $enchufe;
    }

    public function conectar110(): string {
        return $this->enchufeEuropeo->conectar();
    }
}

// Uso
$europeo = new EnchufeEuropeo();
$adaptador = new AdaptadorEuropeoAamericano($europeo);
echo $adaptador->conectar110();  // Conectado con enchufe europeo
```

### **Ventajas:**

- Permite usar clases que ya existen sin modificarlas.
- Favorece la reutilización.

### **Desventajas:**

- Añade una capa extra de abstracción.
- Puede hacer el sistema más complejo si se abusa.

## **Observer (Comportamiento)**

### **¿Qué es?**

Define una relación uno-a-muchos entre objetos, de forma que cuando un objeto cambia su estado, todos sus observadores son notificados automáticamente.

### **¿Cuándo usarlo?**

Cuando múltiples objetos deben reaccionar a los cambios de estado de otro objeto (como en interfaces gráficas, sistemas de eventos, etc).

### **Ejemplo simple:**

```php
// Observador
interface Observador {
    public function actualizar(string $estado): void;
}

// Sujeto
class Notificador {
    private $observadores = [];
    private $estado;

    public function agregarObservador(Observador $obs): void {
        $this->observadores[] = $obs;
    }

    public function cambiarEstado(string $estado): void {
        $this->estado = $estado;
        $this->notificar();
    }

    private function notificar(): void {
        foreach ($this->observadores as $obs) {
            $obs->actualizar($this->estado);
        }
    }
}

// Observadores concretos
class Usuario implements Observador {
    private $nombre;
    public function __construct(string $nombre) {
        $this->nombre = $nombre;
    }

    public function actualizar(string $estado): void {
        echo "Usuario {$this->nombre} recibió la notificación: {$estado}\n";
    }
}

// Uso
$notificador = new Notificador();
$notificador->agregarObservador(new Usuario("Ana"));
$notificador->agregarObservador(new Usuario("Luis"));
$notificador->cambiarEstado("Nuevo mensaje disponible");
// Output:
// Usuario Ana recibió la notificación: Nuevo mensaje disponible
// Usuario Luis recibió la notificación: Nuevo mensaje disponible
```

### **Ventajas:**

- Alta desacoplación entre objetos emisores y receptores.
- Muy útil para sistemas reactivos o eventos.

### **Desventajas:**

- Puede volverse difícil de depurar si hay muchos observadores.
- Potencial problema de rendimiento si hay muchas notificaciones.

## **Conclusión**


| Patrón          | Tipo            | ¿Para qué sirve?                                               |
|-----------------|------------------|-----------------------------------------------------------------|
| Factory Method  | Creacional       | Crear objetos sin acoplarse a clases concretas.                |
| Adapter         | Estructural      | Hacer compatibles clases con interfaces distintas.             |
| Observer        | Comportamiento   | Notificar a varios objetos cuando otro cambia su estado.       |

---

Cada patrón resuelve un problema común específico y, bien aplicado, puede hacer que tu código sea más limpio, flexible y fácil de mantener.