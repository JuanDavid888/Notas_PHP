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

## **Ejemplo archivo Composer**

```bash
{
    "name": "add/api-basic-slim",
    "description": "Una API basica de Slim",
    "type": "project",
    "license": "MIT",
    "autoload": {
        "psr-4": {
            "App\\": "src/"
        }
    },
    "authors": [
        {
            "name": "JuanDavid888",
            "email": "190215642+JuanDavid888@users.noreply.github.com"
        }
    ],
    "require": {
        "slim/slim": "^4.10",
        "slim/psr7": "^1.7",
        "illuminate/database": "^12.21",
        "vlucas/phpdotenv": "^5.6",
        "php-di/php-di": "^7.0",
        "respect/validation": "^2.4"
    }
}
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

### **Forma #1**

<br>

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

### **Forma #2**

## **Ejemplo de Modeo**

```php
<?php

namespace App\Domain\Models;

use Illuminate\Database\Eloquent\Model;

class Camper extends Model
{
    protected $table = 'campers'; // Nombre tabla
    protected $primaryKey = 'id'; // Llave primaria
    public $timestamps = true; // Habilita el uso de created_at y updated_at
    protected $fillable = ['nombre', 'edad', 'documento', 'tipo_documento', 'nivel_ingles', 'nivel_programacion']; // Columnas habilitadas para la insercion
}
```

## **Ejemplo de reportitories**

```php
<?php

namespace App\Domain\Repositories;

use App\Domain\Models\Camper;


interface CamperRepositoryInterface {
    public function getAll(): array;

    public function getById(int $doc): ?Camper;

    public function create(array $data): Camper;

    public function update(int $doc, array $data): bool;

    public function delete(int $doc): bool;
}
```

## **Ejemplo de rutas**

```php
<?php

use App\Controllers\CamperController;
use App\Middleware\RoleMiddleware;
use App\Middleware\AuthMiddleware;
use Slim\App;

return function(App $app) {
    $app->group('/campers', function($group) {
        $group->get('', [CamperController::class, 'index']);

        $group->get('/{documento}', [CamperController::class, 'show']);

        $group->post('', [CamperController::class, 'store']);

        $group->put('/{documento}', [CamperController::class, 'update']);

        $group->delete('/{documento}', [CamperController::class, 'destroy']);
    })->add(new RoleMiddleware('user'))
    ->add(new AuthMiddleware());
};
```

## **Ejemplo controllers**

```php
<?php

namespace App\Controllers;

use App\Domain\Repositories\CamperRepositoryInterface;
use App\UseCases\GetAllCampers;
use App\UseCases\GetCamperById;
use App\UseCases\CreateCamper;
use App\UseCases\UpdateCamper;
use Psr\Http\Message\ResponseInterface as Response;
use Psr\Http\Message\ServerRequestInterface as Request;

class CamperController
{
    public function __construct(private CamperRepositoryInterface $repo) {}

    public function index(Request $request, Response $response): Response
    {
        $useCase = new GetAllCampers($this->repo);
        $campers = $useCase->execute();
        $response->getBody()->write(json_encode($campers));
        return $response;
    }

    public function show(Request $request, Response $response, array $args): Response
    {
        $useCase = new GetCamperByID($this->repo);
        $camper = $useCase->execute((int)$args['documento']);
        if(!$camper) {
            $response->getBody()->write(json_encode(["error" => "Camper no registrado en la plataforma"]));
            return $response->withStatus(404);
        }

        $response->getBody()->write(json_encode($camper));
        return $response;
    }

    public function store(Request $request, Response $response): Response
    {
        $data = $request->getParsedBody();
        $useCase = new CreateCamper($this->repo);
        $camper = $useCase->execute($data);
        $response->getBody()->write(json_encode($camper));

        return $response;
    }

    public function update(Request $request, Response $response, array $args): Response
    {
        $doc = (int)$args['documento'];
        $data = $request->getParsedBody();
        $useCase = new UpdateCamper($this->repo);
        $success = $useCase->execute($doc, $data);
        if(!$success) {
            $response->getBody()->write(json_encode(["error" => "Camper no registrado en la plataforma"]));
            return $response->withStatus(404);
        }

        return $response->withStatus(204);
    }

    public function destroy(Request $request, Response $response): Response
    {
        return $response;
    }
}
```

## **Ejemplo del CRUD**

```php
<?php

namespace App\Infrastructure\Repositories;

use App\Domain\Models\Camper;
use App\Domain\Repositories\CamperRepositoryInterface;


class EloquentCamperRepository implements CamperRepositoryInterface
{
    public function getAll(): array
    {
        // SELECT * FROM campers;
        return Camper::all()->toArray();
    }

    public function getById(int $doc): ?Camper
    {
        // SELECT * FROM campers WHERE id = id;
        // return Camper::find($doc);

        // SELECT * FROM campers WHERE documento = $doc LIMIT 1,1;
        return Camper::where('documento', $doc)->first();
    }

    public function create(array $data): Camper
    {
        $exists = $this->getById($data['documento']);
        if($exists) {
            return $exists;
        }
        return Camper::create($data);
    }

    public function update(int $doc, array $data): bool
    {
        // SELECT * FROM campers WHERE id = $doc;
        // Elije de data el documento a cambiar, pero sin cambiar documento
        $camper = $this->getById($data['documento']);

        // SELECT * FROM campers WHERE documento = $doc;
        // Elije de la url el documento, siendo posible cambiar el documento
        // $camper = Camper::where('documento', $doc)->first();

        // UPDATE campers SET nombre $data[x] ... WHERE id = $doc;
        return $camper ? $camper->update($data) : false;
    }

    public function delete(int $doc): bool
    {
        // SELECT * FROM campers WHERE id = $doc;
        $camper = Camper::find($doc);
        // DELETE FROM campers WHERE id = $doc;
        return $camper ? $camper->delete() : false;
    }
}
```

## **Ejemplo useCase**

```php
<?php

namespace App\UseCases;

use App\Domain\Models\Camper;
use App\Domain\Repositories\CamperRepositoryInterface;

class CreateCamper {

    public function __construct(private CamperRepositoryInterface $repo){}

    public function execute(array $data): Camper {
        return $this->repo->create($data);
    }
}
```
## **Conexión a la db**

```php
<?php

namespace App\Infrastructure\Database;

use Exception;
use Illuminate\Database\Capsule\Manager as Capsule;

class Connection {
    public static function init(): string|bool {
        $capsule = new Capsule;
        $capsule->addConnection([
        // Algunos datos viene de un archivo .env para mayor seguridad
            'driver' => 'mysql',
            'host' => $_ENV['DB_HOST'],
            'database' => $_ENV['DB_NAME'],
            'username' => $_ENV['DB_USER'],
            'password' => $_ENV['DB_PASS'],
            'charset' => 'utf8',
            'collation' => 'utf8_unicode_ci',
            'prefix' => ''
        ]);

        $capsule->setAsGlobal();
        $capsule->bootEloquent();
        
        try {
            Capsule::connection()->getPdo();
            return true;

        } catch (Exception $ex) {
            return "No se puede conectar con la base de datos: ". $ex->getMessage();
        }
    }
}
```

## **Manera de traer data JSON**

```php
<?php

namespace App\Middleware;

use Psr\Http\Message\ResponseInterface as Response;
use Psr\Http\Message\ServerRequestInterface as Request;
use Psr\Http\Server\MiddlewareInterface;
use Psr\Http\Server\RequestHandlerInterface as Handler;

class JsonBodyParserMiddleware implements MiddlewareInterface {
    
    public function process(Request $request, Handler $handler): Response {

        $contentType = $request->getHeaderLine('Content-Type');
        if(strtr($contentType, ["application/json"])) {
            $data = json_decode(file_get_contents('php://input'), true);
            $request = $request->withParsedBody($data);
        }

        return $handler->handle($request);
    }
}
```

## **Manera de conectar middlewares**

```php
<?php

use Slim\App;
use App\Middleware\JsonBodyParserMiddleware;
use Psr\Http\Message\ResponseInterface as Response;
use Psr\Http\Message\ServerRequestInterface as Request;
use Psr\Http\Server\RequestHandlerInterface as Handler;

return function(App $app) {
    // Middlewares
    // Capa que actua entre la solicitud y la respuesta
    // Ayuda a modificar o intersectar (validar)

    // Global -> a todas las Request del Backend
    $app->add(function(Request $req, Handler $han): Response {
        $response = $han->handle($req);
        return $response->withHeader('Content-Type', 'application/json');
    });

    // Custom Global Middleware
    $app->add(new JsonBodyParserMiddleware());
};
```

## **Crear contenedor donde trabajar la db**

```php
<?php

use App\Domain\Repositories\CamperRepositoryInterface;
use App\Domain\Repositories\UserRepositoryInterface;
use App\Handler\CustomErrorHandler;
use App\Infrastructure\Repositories\EloquentCamperRepository;
use App\Infrastructure\Repositories\EloquentUserRepository;
use DI\Container;
use Psr\Http\Message\ResponseFactoryInterface;
use Slim\Interfaces\ErrorHandlerInterface;

// Clase a reemplazar y valor creado a recibir
$container = new Container();

// El parametro de CamperController es CamperRepositoryInterface, pero recibe las instrucciones de EloquentCamperRepository
$container->set(CamperRepositoryInterface::class, function() {
    return new EloquentCamperRepository();
});

$container->set(UserRepositoryInterface::class, function() {
    return new EloquentUserRepository();
});

// new CamperController(new EloquentCamperRespository())
// Manejador
$container->set(ErrorHandlerInterface::class, function() use ($container) {
    return new CustomErrorHandler(
        $container->get(ResponseFactoryInterface::class)
    );
});

return $container;
```

## **Manera de traer y redireccionar a una ruta**

```php
<?php

require_once "vendor/autoload.php";

use App\Infrastructure\Database\Connection;
use Slim\Factory\AppFactory;
use Dotenv\Dotenv;
use Psr\Http\Message\ResponseFactoryInterface;
use Slim\Interfaces\ErrorHandlerInterface;

// Variables de .env
$dotenv = Dotenv::createImmutable(__DIR__. '/');
$dotenv->load(); // $_ENV[...]

// Se carga el Container de PHP-DI
$container = require_once 'bootstrap/container.php';

// Se asigna a Slim el contenedor
AppFactory::setContainer($container);

// Iniciar la conexion con la db
Connection::init();

$app = AppFactory::create();

// Inyectamos ResponseFactory que necesita nuestro CustomErrorHandler
$container->set(ResponseFactoryInterface::class, $app->getResponseFactory());

// Definir quien va a manejar los errores...
$errorHanlder = $app->addErrorMiddleware(true, true, true);
$errorHanlder->setDefaultErrorHandler($container->get(ErrorHandlerInterface::class));

// Ejecutando los scripts de

// public/
(require_once 'public/index.php')($app);

// routes/
(require_once 'routes/users.php')($app);
(require_once 'routes/campers.php')($app);

$app->run();
``` 

## **Manera de implementar errores customizados**

```php
<?php

namespace App\Handler;

use InvalidArgumentException;
use Psr\Http\Message\ResponseFactoryInterface;
use Psr\Http\Message\ResponseInterface;
use Psr\Http\Message\ServerRequestInterface;
use Slim\Exception\HttpMethodNotAllowedException;
use Slim\Exception\HttpNotFoundException;
use Slim\Interfaces\ErrorHandlerInterface;
use Throwable;

class CustomErrorHandler implements ErrorHandlerInterface
{

    public function __construct(private ResponseFactoryInterface $response) {}

    public function __invoke(ServerRequestInterface $request, Throwable $exception, bool $displayErrorDetails, bool $logError, bool $logErrorDetails): ResponseInterface {
        $status = 500;
        $message = "Error interno en el servidor.";

        if($exception instanceof HttpNotFoundException) {
            $status = 404;
            $message = "Ruta no encontrada";
        } elseif ($exception instanceof InvalidArgumentException) {
            $status = 422;
            $message = $exception->getMessage();
        }  elseif ($exception instanceof HttpMethodNotAllowedException) {
            $status = 405;
            $message = "Metodo no permitido";
        }

        $response = $this->response->createResponse($status);
        $response->getBody()->write(json_encode(['error' => $message]));

        return $response->withHeader('Content-Type', 'application/json');
    }
}
```

# **Roles**

## **Definir rol**

```php
<?php

namespace App\Middleware;

use App\Domain\Models\User;
use Psr\Http\Message\ServerRequestInterface AS Request;
use Psr\Http\Message\ResponseInterface as Response;
use Psr\Http\Server\RequestHandlerInterface as Handler;
use Slim\Psr7\Response as SlimResponse;

class RoleMiddleware {

    public function __construct(private readonly string $role){}

    public function __invoke(Request $request, Handler $handler): Response
    {
        $user = $request->getAttribute('user');
        if(!$user || $user->rol != $this->role) {
            $response = new SlimResponse();
            $response->getBody()->write(json_encode(['error' => 'Acceso prohibido']));

            return $response->withStatus(403)->withHeader('Content-Type', 'application/json');
        }

        return $handler->handle($request);
    }
}
```

## **Controller de usuarios**

```php
<?php

namespace App\Controllers;

use App\Domain\Repositories\UserRepositoryInterface;

use Psr\Http\Message\ResponseInterface as Response;
use Psr\Http\Message\ServerRequestInterface as Request;
use App\DTOs\UserDTO;

class UserController {
    public function __construct(private UserRepositoryInterface $repo) {}

    public function createUser(Request $request, Response $response): Response {

        $data = $request->getParsedBody();

        $dto = new UserDTO(
            nombre: $data['nombre'] ?? '',
            email: $data['correo'] ?? '',
            password: $data['contrasena'] ?? '',
            rol: 'user'
        );

        $user = $this->repo->create($dto);

        $response->getBody()->write(json_encode($user));
        return $response->withStatus(201);
    }

    public function createAdmin(Request $request, Response $response): Response {

        $data = $request->getParsedBody();
        $data['rol'] = 'admin';

        // DTO
        $data['password'] = password_hash($data['password'], PASSWORD_DEFAULT); // Algoritmo que hace el hash

        $user = $this->repo->create($data);

        $response->getBody()->write(json_encode($user));
        return $response->withStatus(201);
    }
}
```

## **CRUD de usuarios**

```php
<?php

namespace App\Infrastructure\Repositories;

use App\Domain\Models\User;
use App\Domain\Repositories\UserRepositoryInterface;
use Exception;
use App\DTOs\UserDTO;

class EloquentUserRepository implements UserRepositoryInterface {

    public function create(UserDTO $dto): User
    {
        $data = $dto->toArray();
        $exists = User::where('email', $data['email'])->first();
        if($exists) {
            // Mostrar un error
            throw new Exception('Error, el usuario ya existe');
        }

        return User::create($data);
    }
}
```

## **Ejemplo de Modelo de usuarios**

```php
<?php

namespace App\Domain\Models;

use Illuminate\Database\Eloquent\Model;

class User extends Model
{
    protected $table = 'usuarios';
    protected $primaryKey = 'id';
    public $timestamps = false;
    protected $fillable = ['nombre', 'email', 'password', 'rol'];
    protected $hidden = ['password'];
}
```
## **Ejemplo de repositories**

```php
<?php

namespace App\Domain\Repositories;

use App\Domain\Models\User;
use App\DTOs\UserDTO;

interface UserRepositoryInterface {

    public function create(UserDTO $dto): User;

}
```

## **Ejemplo de DTO**

```php
<?php

namespace App\DTOs;

use Respect\Validation\Exceptions\NestedValidationException;
use Respect\Validation\Validator as v;

class UserDTO {

    private readonly string $password;

    public function __construct(
        public readonly string $nombre,
        public readonly string $email,
        string $password,
        public readonly string $rol = 'user'
        ) {
            $this->validate($nombre, $email, $password, $rol);
            $this->password =password_hash($password, PASSWORD_DEFAULT);
        }

    private function validate(string $nombre, string $email, string $password, string $rol): void {
        try {
            v::stringType()->length(min: 2, max: 50)->setName('nombre')->assert($nombre);

            v::email()->setName('correo')->assert($email);

            v::stringType()->length(min: 8, max: 100)
            ->regex('/[!@#$%^&*()\-_=+{};:,<.>]/')
            ->regex('/[0-9]/')
            ->setName('contrasena')->assert($password);

            v::in(['user', 'admin'])->assert($rol);

        } catch(NestedValidationException $e) { // Exception :c
            throw new \InvalidArgumentException($e->getFullMessage());
        }
    }

    public function toArray(): array {
        return [
            "nombre" => $this->nombre,
            "password" => $this->password, // Ya esta el Hash
            "rol" => $this->rol,
            "email" => $this->email
            
        ];
    }
}
```

<br>

#  **Archivo .gitignore**

```gitignore
# Composer
vendor/
composer.lock

# Entorno
.env
.env.local
.env.*.local

# Archivos temporales y cachés
*.log
*.cache
*.tmp
*.temp
*.bak

# IDEs y editores
.idea/
*.sublime-project
*.sublime-workspace
.vscode/
*.swp
*.swo

# Sistemas de archivos (macOS / Linux / Windows)
.DS_Store
Thumbs.db
desktop.ini

# PHPUnit / Testing
phpunit.xml
phpunit.result.cache
coverage/

# Logs
logs/
*.log

# Base de datos (si usas SQLite local)
*.sqlite
*.sqlite3
*.db

# Eloquent migration backups (si los generas manualmente)
*_backup.sql

# Slim built-in server router (si lo usas)
router.php
```

<br>

# **Archivo .htaccess**

```htaccess
RewriteEngine On
RewriteCond %{REQUEST_FILENAME} !-f
RewriteRule ^ index.php [QSA,L]
```