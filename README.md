# **Notas PHP**

## **¿Qué encontrarás aquí?**

Esta es una colección de notas y ejemplos de código para que puedas aprender a programar en PHP. Cada clase está separada en un archivo independiente, así que puedes leer y copiar los ejemplos que necesites.

## **¿Cómo puedo contribuir?**

Si encuentras algún error o quieres agregar algo nuevo, puedes hacerlo [aquí](https://github.com/JuanDavid888/Notas_PHP).

## **Clases**

A continuación encontrarás una lista de clases que he creado para esta colección de notas. Puedes darle click directamenta a cada una de ellas para ver el contenido completo. Igualmente aún hay más temas que no he mencionado, pero debajo de estos links encontrarás más material de estudio útil.

**1.** [Introducción a PHP](clases/Introduccion_PHP.md)

**2.** [Variables, Operadores y Estructuras de Decisión](clases/variables_operadores_estructuras.md)

**3.** [Estructuras Repetitivas](clases/estructuras_repetitivas.md)

**4.** [Listas y Arreglos](clases/listas_arreglos.md)

**5.** [Funciones Definidas](clases/funciones_definidas.md)

**6.** [Introducción a la Programación Orientada a Objetos](clases/introduccion_POO.md)

**7.** [Herencia y Polimorfismo](clases/herencia_polimorfismo.md)

**8.** [Principios SOLID](clases/principios_SOLID.md)

**9.** [Patrones de Diseño](clases/patrones_diseno.md)

**10.** [Persistencia de Datos](clases/persistencia_datos.md)

**11.** [Introducción a PDO](clases/Introduccion_PDO.md)

<br>

# **Conexión DB con PHP**

# **Conectar PHP con MySQL**

## **Configuración del devcontainer**

Usaremos VS Code con la extensión Remote - Containers (o Codespaces). Esto permite trabajar en un entorno Docker con PHP + MySQL ya configurados.

### **Estructura de archivos**

```
/myapp
├── .devcontainer
│   ├── devcontainer.json
│   └── docker-compose.yml
├── src
│   ├── index.php
│   └── api.php
└── README.md
```

## **Estructura Dev Container**

### **Normal - Solo PHP**

*devcontainer.json*

```json
{
	"name": "PHP",
	"image": "mcr.microsoft.com/devcontainers/php:1-8.2-bullseye",
	"customizations": {
		"vscode": {
			"settings": {
				"terminal.integrated.shell.linux": "/bin/bash",
				"php.validate.executablePath": "/usr/local/bin/php",
				"php.suggest.basic": false,
				"editor.formatOnSave": true
			},
			"extensions": [
				"felixfbecker.php-debug",
				"bmewburn.vscode-intelephense-client",
				"esbenp.prettier-vscode",
				"vivaxy.vscode-conventional-commits"
			]
		}
	},
	"forwardPorts": [
		8080
	],
	"features": {
		"ghcr.io/devcontainers/features/github-cli:1": {},
		"ghcr.io/devcontainers-extra/features/composer:1": {}
	}
}
```

### **PHP & MySQL**

*devcontainer.json*

```json
{
    "name": "PHP & MySQL",
    "dockerComposeFile": "docker-compose.yml",
    "service": "app",
    "workspaceFolder": "/workspaces/${localWorkspaceFolderBasename}",
    "customizations": {
        "vscode": {
            "settings": {
                "terminal.integrated.shell.linux": "/bin/bash",
                "php.validate.executablePath": "/usr/local/bin/php",
                "php.suggest.basic": false,
                "editor.formatOnSave": true
            },
            "extensions": [
                "felixfbecker.php-debug",
                "bmewburn.vscode-intelephense-client",
                "esbenp.prettier-vscode",
                "vivaxy.vscode-conventional-commits",
                "cweijan.vscode-mysql-client2"
            ]
        }
    },
	// Use 'forwardPorts' to make a list of ports inside the container available locally.
    "forwardPorts": [
        8081,
        3307
    ],
    "features": {
        "ghcr.io/devcontainers/features/github-cli:1": {},
        "ghcr.io/devcontainers-extra/features/composer:1": {}
    }
}
```

*Dockerfile*

```dockerfile
FROM mcr.microsoft.com/devcontainers/php:1-8.2-bullseye

RUN docker-php-ext-install mysqli pdo pdo_mysql
```

*docker-compose.yml*

```yml
services: 
  app:
    build:
      context: .
      dockerfile: Dockerfile
    volumes:
      - ../..:/workspaces:cached
    command: sleep infinity
    network_mode: service:db
  db:
    image: mysql:8.4.3
    restart: unless-stopped
    volumes:
      - mysql-pdo-data:/var/lib/mysql
    environment:
      MYSQL_ROOT_PASSWORD: admin
      MYSQL_DATABASE: php_pdo
volumes:
  mysql-pdo-data:
```

## **Crear la base de datos**

El contenedor `db` levanta MySQL con la base `mydb` ya creada.

**Puedes conectarte con un cliente como DBeaver, MySQL Workbench o con la terminal:**

```bash
mysql -h 127.0.0.1 -P 3306 -u user -p
# password: pass
```

**Y crear una tabla por ejemplo:**

```sql
CREATE DATABASE  IF NOT EXISTS 
Conexión PHP-MySQLmydb;

USE mydb;

CREATE TABLE users (
  id INT AUTO_INCREMENT PRIMARY KEY,
  name VARCHAR(50),
  email VARCHAR(100)
);
```

## **Conexión PHP & MySQL**

*db.php*

```php
<?php
// Ojo con el host='db': es el nombre del servicio en docker-compose.

$host = 'db';
$db   = 'mydb';
$user = 'user';
$pass = 'pass';
$charset = 'utf8mb4';

$dsn = "mysql:host=$host;dbname=$db;charset=$charset";
$options = [
    PDO::ATTR_ERRMODE            => PDO::ERRMODE_EXCEPTION,
    PDO::ATTR_DEFAULT_FETCH_MODE => PDO::FETCH_ASSOC,
];

try {
    $pdo = new PDO($dsn, $user, $pass, $options);
} catch (\PDOException $e) {
    throw new \PDOException($e->getMessage(), (int)$e->getCode());
}
?>
```

## **CRUD PHP**

*api.php*

```php
<?php
header("Content-Type: application/json");
require 'db.php';

$method = $_SERVER['REQUEST_METHOD'];

switch ($method) {
    case 'GET':
        $stmt = $pdo->query("SELECT * FROM users");
        echo json_encode($stmt->fetchAll());
        break;

    case 'POST':
        $data = json_decode(file_get_contents('php://input'), true);
        $stmt = $pdo->prepare("INSERT INTO users (name, email) VALUES (?, ?)");
        $stmt->execute([$data['name'], $data['email']]);
        echo json_encode(['status' => 'User created']);
        break;

    case 'PUT':
        $data = json_decode(file_get_contents('php://input'), true);
        $stmt = $pdo->prepare("UPDATE users SET name=?, email=? WHERE id=?");
        $stmt->execute([$data['name'], $data['email'], $data['id']]);
        echo json_encode(['status' => 'User updated']);
        break;

    case 'DELETE':
        parse_str(file_get_contents("php://input"), $data);
        $stmt = $pdo->prepare("DELETE FROM users WHERE id=?");
        $stmt->execute([$data['id']]);
        echo json_encode(['status' => 'User deleted']);
        break;

    default:
        http_response_code(405);
        echo json_encode(['error' => 'Method not allowed']);
}
?>
```

<br>

# **Composer**

## **¿Qué es?**

Composer es un gestor de paquetes para PHP. Funciona leyendo un archivo llamado `composer.json`, donde defines las dependencias (bibliotecas, frameworks, herramientas) que tu aplicación necesita. Luego Composer descarga e instala esas dependencias en el directorio de tu proyecto.

## **¿Para qué sirve Composer?**

- Instalar automáticamente librerías de terceros (como Symfony, Laravel, Guzzle, etc.).
- Mantener las versiones correctas de dependencias.
- Cargar automáticamente clases (autoloader).
- Compartir proyectos con otros desarrolladores sin necesidad de enviar las bibliotecas manualmente.

## **Ventajas de Composer**

1. Automatiza la gestión de dependencias: No necesitas descargar ni actualizar manualmente las librerías.
2. Control de versiones: Puedes fijar versiones específicas o rangos compatibles de cada paquete.**
3. Autoloading: Composer genera automáticamente un cargador de clases para tus bibliotecas (`vendor/autoload.php`).
4. Facilita el trabajo en equipo: Solo necesitas compartir tu código fuente y el `composer.json`; otros pueden instalar todo con `composer install`.
5. Amplio ecosistema: Compatible con [Packagist.org](https://packagist.org/), el repositorio más grande de paquetes PHP.

## **Desventajas de Composer**

1. **Dependencia del archivo `composer.lock`:** Si no se gestiona bien en proyectos colaborativos, puede causar diferencias en versiones instaladas.
2. **Requiere conexión a internet:** Para instalar o actualizar paquetes.
3. **Curva de aprendizaje inicial:** Para quienes no están familiarizados con la gestión de dependencias.
4. **Dependencia de terceros:** Si un paquete desaparece de Packagist o GitHub, podría afectar tu proyecto si no tienes copia local.

## **Situaciones para usarlo**

| Situación                                       | Cómo Composer ayuda                                                                 |
|------------------------------------------------|--------------------------------------------------------------------------------------|
| Crear un proyecto con Laravel o Symfony        | `composer create-project` descarga todo el framework y dependencias                |
| Usar una librería HTTP como Guzzle             | `composer require guzzlehttp/guzzle`                                               |
| Compartir tu propio paquete                    | Puedes publicarlo en Packagist                                                     |
| Proyectos con múltiples desarrolladores        | Todos instalan las mismas dependencias con `composer install`                      |
| Automatizar autoload de clases                 | Composer genera un autoloader con PSR-4 o PSR-0                                    |


## **Ejemplo básico de `composer.json`**

```json
{
  "require": {
    "monolog/monolog": "^2.0"
  }
}
```

**Y luego con:**

```bash
composer install
```

Composer descarga la librería `monolog` en el directorio `/vendor`.

## **Comandos**

### ***Inicializar Composer* (Forma interactiva)**

```bash
composer init 
```

### ***Inicializar Composer* (Forma no interactiva)**

```bash
composer init --name=adds/api-basic --no-interaction
```

### ***Instalar phpunit***

```bash
composer require --dev phpunit/phpunit ^10.5.48
```

### ***Instala de nuevo el vendor***

```bash
composer install
```

### ***Crear archivo autoload en vendor***

```bash
composer dump-autoload
```

### ***Instala versión de Slim***

```bash
composer require slim/slim"^4.0”
```

### ***Actualizar Slim***

```bash
composer update slim
```

### ***Instala psr7* (para slim)**

```bash
composer require slim/psr7
```

### ***Instalar Eloquent ORM***

```bash
composer require illuminate/database
```

### ***Instalar lucas***

```bash
composer require vlucas/phpdotenv
```

### ***Instalar librería php-di***

```bash
composer require php-di/php-di
```

### ***Visualizar formato de arbol***

```bash
tree -l 5
```

```bash
tree -la 3
```

```bash
tree -L 3
```
<br>

# **Inyección de dependencias**

## **¿Qué es?**

La inyección de dependencias (Dependency Injection o DI) es un patrón de diseño que permite desacoplar componentes en un sistema, facilitando la gestión de dependencias entre objetos.

En lugar de que una clase cree directamente sus dependencias, estas se inyectan desde el exterior, lo que mejora la modularidad, testabilidad y mantenimiento del código.

## **¿Para qué se usa?**

**Se usa principalmente para:**

- Reducir el acoplamiento entre componentes.
- Facilitar las pruebas unitarias (mocking).
- Mejorar la escalabilidad y mantenimiento de aplicaciones grandes.
- Aplicar principios SOLID, especialmente la Inversión de Dependencias (D).

## **Casos de uso**

- Frameworks web como Spring (Java), ASP.NET Core (C#), Angular (TypeScript).
- Aplicaciones móviles (como Android con Dagger/Hilt o Koin).
- Arquitecturas limpias (Clean Architecture, DDD, Hexagonal).
- Sistemas donde se requiere facilidad para testing (pruebas unitarias con mocks/stubs).*

## **Ventajas**

- Bajo acoplamiento entre componentes.
- Mayor facilidad para testear (mockear dependencias).
- Reutilización de código más sencilla.
- Facilita la implementación de principios SOLID.
- Cambiar una implementación por otra sin modificar el código cliente.

## **Desventajas**

- Mayor complejidad inicial, especialmente para principiantes.
- Puede generar configuraciones difíciles de rastrear, sobre todo en grandes aplicaciones.
- Sobrecarga de configuración si se usa incorrectamente o en exceso.
- Uso excesivo puede llevar a overengineering (*práctica de diseñar un producto o solución que es más complejo o robusto de lo necesario*)

## **Tipos de inyección**

- Por constructor (la más común y recomendada).
- Por setter (menos segura, puede haber estados inválidos).
- Por interfaz (menos común).

## **Ejemplo simple de inyección de dependencias en PHP**

Supongamos que tienes una clase que depende de otra, por ejemplo, un `Logger` y un `UserService` que usa ese logger:

### **Sin inyección (acoplamiento fuerte)**

```php
class Logger {
    public function log($mensaje) {
        echo "[LOG]: $mensaje\n";
    }
}

class UserService {
    private $logger;

    public function __construct() {
        $this->logger = new Logger(); // ACOPLAMIENTO FUERTE
    }

    public function crearUsuario($nombre) {
        $this->logger->log("Usuario '$nombre' creado.");
    }
}

$servicio = new UserService();
$servicio->crearUsuario("Carlos");
```

Aquí, `UserService` crea su propia dependencia, lo que viola la inversión de dependencias y dificulta testear o cambiar `Logger`.

### **Con inyección de dependencias (acoplamiento débil)**

```php
class Logger {
    public function log($mensaje) {
        echo "[LOG]: $mensaje\n";
    }
}

class UserService {
    private $logger;

    // Inyección por constructor
    public function __construct(Logger $logger) {
        $this->logger = $logger;
    }

    public function crearUsuario($nombre) {
        $this->logger->log("Usuario '$nombre' creado.");
    }
}

// Inyectamos la dependencia desde fuera
$logger = new Logger();
$servicio = new UserService($logger);
$servicio->crearUsuario("Carlos");
```

Ahora `UserService` no sabe cómo se construye `Logger`, solo que necesita uno. Esto hace el código más modular, fácil de testear y de extender.

<br>

# **Archivos PDF con PHP**

Para generar archivos PDF con PHP, lo más común es usar bibliotecas externas que simplifican el proceso. Las dos más populares son:

## **TCPDF**

Una de las bibliotecas más completas y sin necesidad de dependencias externas.

### **Link**

https://tcpdf.org/

### **Ventajas:**

- No requiere extensiones especiales de PHP.
- Totalmente en PHP.
- Soporta HTML, CSS, imágenes, tablas, UTF-8, etc.

### **Instalación con composer**

```bash
composer require tecnickcom/tcpdf
```

### **Ejemplo: Con composer**

```php
require_once('vendor/autoload.php');

$pdf = new TCPDF();
$pdf->AddPage();
$pdf->SetFont('helvetica', '', 12);
$pdf->Write(0, '¡Hola, mundo en PDF con TCPDF!');
$pdf->Output('ejemplo_tcpdf.pdf', 'I'); // 'I' = inline en navegador, 'D' = descargar
```

***Estructura:***

```mathematica
/mi-proyecto
│
├── index.php
├── composer.json
└── vendor/
		├── autoload.php
		└── tecnickcom/
				└── tcpdf/
```

### **Ejemplo: Sin composer**

```php
require_once('tcpdf/tcpdf.php');

$pdf = new TCPDF();
$pdf->AddPage();
$pdf->SetFont('helvetica', '', 12);
$pdf->Write(0, '¡Hola, mundo desde TCPDF!');
$pdf->Output('ejemplo.pdf', 'I'); // 'I' para mostrar en el navegador
```

***Estructura:***

```mathematica
/mi-proyecto
│
├── index.php
└── tcpdf/
		├── tcpdf.php
		├── config/
		└── fonts/
```

## **FPDF**

Una librería más sencilla, muy ligera, ideal para casos simples.

### **Link**

http://www.fpdf.org/

### **Ventajas:**

- Fácil de aprender.
- Muy liviana.
- Ideal para PDFs simples y control absoluto del diseño.
- Permite agregar texto, imágenes, tablas simples

### **Insalación con composer**

```bash
composer require setasign/fpdf
```

### **Alternativas de composer**

| Paquete              | Comando Composer                    | Comentario                              |
| -------------------- | ----------------------------------- | --------------------------------------- |
| **setasign/fpdf**    | `composer require setasign/fpdf`    | Oficial y mantenido por Setasign        |
| **anouar/fpdf**      | `composer require anouar/fpdf`      | Alternativa con namespace `Anouar\Fpdf` |
| **clegginabox/fpdf** | `composer require clegginabox/fpdf` | Otra variante básica                    |

### **Ejemplo: Con composer**

```php
require 'vendor/autoload.php';

use setasign\Fpdi\Fpdf\Fpdf; // Esto funciona para el paquete de FPDI que incluye FPDF
// O directamente:
use Fpdf\Fpdf; // Algunos paquetes lo exponen así, depende del paquete exacto

$pdf = new Fpdf();
$pdf->AddPage();
$pdf->SetFont('Arial','B',16);
$pdf->Cell(40,10,'¡Hola mundo desde FPDF con Composer!');
$pdf->Output();
```

***Estructura***

```mathematica
mi-proyecto/
├── index.php
├── composer.json
└── vendor/
		├── autoload.php
		└── setasign/
			└── fpdf/
			└── ...
```

### **Nota sobre FPDI**

Si además necesitas importar o modificar PDFs existentes, puedes usar FPDI, que depende de FPDF y extiende sus funciones:

```bash
composer require setasign/fpdi
```

### **Ejemplo: Sin composer**

```php
require('fpdf.php');

$pdf = new FPDF();
$pdf->AddPage();
$pdf->SetFont('Arial','B',16);
$pdf->Cell(40,10,'¡Hola, mundo con FPDF!');
$pdf->Output();
```

Para usarlo, descarga `fpdf.php` desde el sitio oficial y colócalo en tu proyecto.

***Estructura***

```mathematica
mi-proyecto/
├── index.php
└── fpdf/
		├── fpdf.php
		└── font/
```
## **DomPDF**

Convierte HTML y CSS a PDF. Muy útil si ya tienes tu contenido en HTML.

### **Link**

https://github.com/dompdf/dompdf/releases

https://github.com/dompdf/dompdf

Descarga el archivo `.zip` más reciente (por ejemplo: `dompdf_x.x.x.zip`)

### **Ventajas**

- Soporte para imágenes, fuentes y estilos externos.
- Fácil de usar.
- Ideal si ya tienes contenido en HTML.
- Control de formato de página, puedes definir tamaño y orientación del papel (`A4`, `letter`, `landscape`, etc.).

### **Instalación con composer**

```bash
composer require dompdf/dompdf
```

### **Ejemplo: Con composer**

```php
use Dompdf\Dompdf;

require 'vendor/autoload.php';

$dompdf = new Dompdf();
$dompdf->loadHtml('<h1>Hola mundo en PDF con DomPDF</h1>');
$dompdf->setPaper('A4', 'portrait');
$dompdf->render();
$dompdf->stream("documento.pdf", ["Attachment" => false]); // true para forzar descarga
```

***Estructura***

```mathematica
mi-proyecto/
├── composer.json                ← Archivo de configuración de Composer
├── composer.lock                ← Bloquea las versiones instaladas
├── vendor/                      ← Carpeta de dependencias (incluye DomPDF)
│   └── autoload.php             ← Autoload universal de Composer
│   └── dompdf/
│       └── dompdf/              ← Código fuente de DomPDF
│           └── ...
├── index.php                    ← Archivo principal PHP que genera el PDF
└── templates/                   ← Carpeta para las plantillas HTML
└── factura.html             ← Ejemplo de plantilla HTML para el PDF
```

### **Ejemplo: Sin composer**

```php
require_once 'dompdf/autoload.inc.php';

use Dompdf\Dompdf;

// Crear instancia
$dompdf = new Dompdf();
$dompdf->loadHtml('<h1>Hola desde DomPDF sin Composer</h1>');

// Configurar tamaño y orientación
$dompdf->setPaper('A4', 'portrait');

// Renderizar PDF
$dompdf->render();

// Mostrar en el navegador
$dompdf->stream("documento.pdf", ["Attachment" => false]);
```

***Estructura***

```mathematica
mi-proyecto/
├── index.php
└── dompdf/
		├── autoload.inc.php
		├── dompdf_config.inc.php
		└── src/
```
## **Conversión manual**

Sí, es posible generar archivos PDF en PHP sin usar bibliotecas externas, pero no es recomendable para la mayoría de los casos, porque:

- El formato PDF es complejo (estructura binaria con encabezados específicos, objetos, compresión, fuentes, etc.).
- Hacerlo manualmente requiere escribir el contenido del PDF byte por byte siguiendo la especificación del formato PDF (PDF 1.4, por ejemplo).
- Es mucho más propenso a errores y no soporta fácilmente fuentes, imágenes, UTF-8, ni estilos complejos.

```php
header('Content-type: application/pdf');
header('Content-Disposition: inline; filename="simple.pdf"');

// PDF mínimo (PDF 1.1)
echo "%PDF-1.1\n";
echo "1 0 obj\n<< /Type /Catalog /Pages 2 0 R >>\nendobj\n";
echo "2 0 obj\n<< /Type /Pages /Kids [3 0 R] /Count 1 >>\nendobj\n";
echo "3 0 obj\n<< /Type /Page /Parent 2 0 R /MediaBox [0 0 300 144] /Contents 4 0 R /Resources << >> >>\nendobj\n";
echo "4 0 obj\n<< /Length 44 >>\nstream\nBT /F1 24 Tf 100 100 Td (Hola PDF) Tj ET\nendstream\nendobj\n";
echo "xref\n0 5\n0000000000 65535 f \n";
echo "0000000010 00000 n \n0000000063 00000 n \n0000000114 00000 n \n0000000211 00000 n \n";
echo "trailer\n<< /Root 1 0 R /Size 5 >>\nstartxref\n289\n%%EOF";
```

Esto genera un archivo PDF muy rudimentario con el texto "Hola PDF" en una posición fija.

### **Conclusión**

- Sí, se puede crear un PDF sin bibliotecas, pero solo en casos educativos o ultra simples.
- Para cualquier cosa práctica (fuentes, estilos, tablas, imágenes, UTF-8, etc.), usar una librería como FPDF, TCPDF o DomPDF es muchísimo más eficiente y seguro.