# **Introducción a PHP**

## **¿Qué es PHP?**

PHP (acrónimo recursivo de *PHP: Hypertext Preprocessor*) es un lenguaje de programación de código abierto ampliamente utilizado, especialmente diseñado para el desarrollo web. Se ejecuta del lado del servidor, lo que significa que el código PHP se procesa en el servidor antes de enviarse al navegador del usuario.

**Se integra fácilmente con HTML, bases de datos (como *MySQL*), y es una de las tecnologías fundamentales del desarrollo web dinámico.**

## **Estructura de un archivo**

Al igual que en HTML, en el lenguaje de servidor PHP es necesario definir etiquetas que indiquen el inicio y el final de un script.

```php
<?php
    // código PHP
?>
```

## **Características**

PHP es ampliamente utilizado en desarrollo web y aplicaciones debido a su flexibilidad y facilidad de integración, donde sus principales usos incluyen lo siguiente:

**1.** ***Desarrollo de Páginas Web Dinámicas***

- PHP permite generar contenido HTML dinámico basado en variables y lógica de negocio.
- **Ejemplo:** Mostrar los últimos artículos publicados en un blog o generar contenido personalizado para un usuario.

**2.** ***Manejo de Formularios y Datos de Usuario***

- Recibir y procesar datos de formularios mediante `$_POST` y `$_GET`.
- **Ejemplo:** Validar y sanitizar datos de entrada para evitar ataques como SQL Injection o XSS.

**3.** ***Conexión y Gestión de Bases de Datos***

- PHP se conecta a bases de datos como MySQL, PostgreSQL, SQLite, MongoDB, entre otras.
- **Ejemplo:** Permite realizar operaciones CRUD (`Crear`, `Leer`, `Actualizar`, `Eliminar`) en bases de datos.

**4. ***Gestión de Sesiones y Cookies***

- PHP permite crear sesiones para identificar a los usuarios (`$_SESSION`).
- **Ejemplo:** Usar cookies para recordar preferencias de usuario (`setcookie`).

**5.** ***Generación de Contenido Dinámico***

- Creación de archivos dinámicos en formato HTML, JSON, XML, PDF, imágenes, etc..
- **Ejemplo:** Se usa en la creación de API REST para aplicaciones móviles y web.

**6.** ***Autenticación de Usuarios y Seguridad***

- PHP permite implementar autenticación con hashing de contraseñas (`password_hash()`).
- **Ejemplo:** Protección contra ataques como SQL Injection y Cross-Site Scripting (XSS).

**7.** ***Creación de Aplicaciones Web Complejas***

- Se usa en la construcción de CMS (Content Management Systems) como WordPress, Joomla, Drupal.
- **Ejemplo:** Contrucción de base de grandes plataformas como Facebook, Wikipedia y Laravel.

## **SQL Injection (Inyección de SQL)**

### **¿Qué es?**

`SQL Injection` es una vulnerabilidad de seguridad que permite a un atacante interferir con las consultas que una aplicación web realiza a su base de datos. Esto se logra insertando código SQL malicioso en campos de entrada de usuario.

### **Características:**

- Ocurre cuando las entradas del usuario no son correctamente validadas o filtradas.
- Permite acceder, modificar o eliminar datos de la base de datos sin autorización.
- Puede permitir el bypass de autenticación, acceso a datos sensibles, e incluso ejecución remota de comandos si el sistema lo permite.
- Suele afectar a aplicaciones que usan consultas SQL concatenadas de forma insegura.

### **Ejemplo**

Supón que tienes esta consulta en tu código:

```sql
SELECT * FROM usuarios WHERE usuario = '$usuario' AND contraseña = '$contraseña';
```

Si el atacante introduce:

```sql
usuario: admin
contraseña: ' OR '1'='1
```

La consulta resultante sería:

```sql
SELECT * FROM usuarios WHERE usuario = 'admin' AND contraseña = '' OR '1'='1';
```

## **XSS (Cross-Site Scripting)**

### **¿Qué es?**

Es una vulnerabilidad que permite a un atacante inyectar scripts maliciosos (generalmente JavaScript) en páginas web vistas por otros usuarios. Se usa para robar cookies, redirigir usuarios, o realizar acciones en nombre de otros.

### **Características:**

- Afecta a los usuarios del sitio web, no directamente a la base de datos.
- Se aprovecha de entradas que no están correctamente sanitizadas o escapadas.
- Puede causar robo de datos de sesión o realizar ataques de phishing.
- **Se clasifica en:**
    - ***Stored XSS:*** el script se guarda en la base de datos y se ejecuta cada vez que un usuario accede.
    - ***Reflected XSS:*** el script se devuelve inmediatamente en la respuesta (por ejemplo, en un mensaje de error).
    - ***DOM-based XSS:*** el script se ejecuta como resultado de cambios en el DOM del navegador.
    

### **Ejemplo**

Un atacante puede enviar un comentario como este:

```html
<script>alert('XSS');</script>
```

Si el sitio muestra ese comentario sin sanitizar, el script se ejecutará en el navegador de cualquier usuario que lo vea.