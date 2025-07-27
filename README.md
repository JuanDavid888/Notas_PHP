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