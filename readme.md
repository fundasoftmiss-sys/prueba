# Guía de instalación del entorno de desarrollo

> **Para quién es esta guía:** estudiantes del curso de Microservicios que empiezan desde cero.
> Al terminar tendrás instalado y verificado todo lo necesario para crear, ejecutar y probar un microservicio con Java y Spring Boot.

## Índice

1. [Java JDK 21](#1-java-jdk-21)
2. [Maven](#2-maven)
3. [Visual Studio Code](#3-visual-studio-code)
4. [Spring Tools](#4-spring-tools)
5. [Base de datos](#5-base-de-datos)
6. [Postman y variables de entorno](#6-postman-y-variables-de-entorno)
7. [Git](#7-git)
8. [Checklist final](#8-checklist-final)
9. [Problemas frecuentes](#9-problemas-frecuentes)

---

## 1. Java JDK 21

El JDK (Java Development Kit) es el compilador y la máquina virtual de Java. Usaremos la versión **21 (LTS)**.

### Descarga

- Recomendado: **Eclipse Temurin 21** → https://adoptium.net/temurin/releases/?version=21
- Alternativa: **Oracle JDK 21** → https://www.oracle.com/java/technologies/downloads/#java21

### Instalación en Windows

1. Descarga el instalador `.msi` para Windows x64.
2. Ejecútalo y, en la pantalla de componentes, activa:
   - **Add to PATH**
   - **Set JAVA_HOME variable**
3. Termina el asistente con las opciones por defecto.

### Instalación en macOS / Linux

```bash
# macOS (Homebrew)
brew install --cask temurin@21

# Ubuntu / Debian
sudo apt update && sudo apt install temurin-21-jdk
```

### Configurar JAVA_HOME a mano (solo si el instalador no lo hizo)

En Windows: `Panel de control → Sistema → Configuración avanzada → Variables de entorno`.

| Variable    | Valor                                          |
|-------------|------------------------------------------------|
| `JAVA_HOME` | `C:\Program Files\Eclipse Adoptium\jdk-21.x.x` |
| `Path`      | añadir `%JAVA_HOME%\bin`                       |

### Verificación

Abre una terminal **nueva** y ejecuta:

```bash
java -version
javac -version
echo %JAVA_HOME%      # Windows (CMD)
echo $JAVA_HOME       # macOS / Linux
```

Salida esperada (la versión menor puede variar):

```
openjdk version "21.0.4" 2024-07-16 LTS
```

---

## 2. Maven

Maven gestiona las dependencias y compila el proyecto. Spring Boot lo usa por defecto.

### Descarga

https://maven.apache.org/download.cgi → archivo **Binary zip archive** (`apache-maven-3.9.x-bin.zip`).

### Instalación en Windows

1. Descomprime el zip en `C:\Program Files\Apache\maven` (o una ruta sin espacios como `C:\tools\maven`).
2. Crea la variable de entorno `MAVEN_HOME` apuntando a esa carpeta.
3. Añade `%MAVEN_HOME%\bin` al `Path`.

### Instalación en macOS / Linux

```bash
# macOS
brew install maven

# Ubuntu / Debian
sudo apt install maven
```

### Verificación

```bash
mvn -version
```

Debe mostrar la versión de Maven **y** el JDK 21 que detecta. Si muestra otra versión de Java, revisa `JAVA_HOME`.

### Comandos que usarás a diario

| Comando                | Qué hace                                      |
|------------------------|-----------------------------------------------|
| `mvn clean install`    | Limpia, compila, ejecuta tests y empaqueta    |
| `mvn spring-boot:run`  | Arranca la aplicación Spring Boot             |
| `mvn test`             | Solo ejecuta los tests                        |
| `mvn dependency:tree`  | Muestra el árbol de dependencias              |

> **Nota:** los proyectos generados con Spring Initializr incluyen `mvnw` / `mvnw.cmd` (Maven Wrapper). Con el wrapper no necesitas Maven instalado, pero es buena práctica tenerlo igualmente.

---

## 3. Visual Studio Code

Editor ligero que, con las extensiones adecuadas, funciona como un IDE de Java completo.

### Instalación

1. Descarga desde https://code.visualstudio.com/
2. Durante la instalación en Windows marca:
   - **Agregar a PATH** (permite abrir carpetas con `code .`)
   - **Agregar acción "Abrir con Code" al menú contextual**

### Extensiones necesarias

Instálalas desde la vista de extensiones (`Ctrl + Shift + X`) o desde la terminal:

```bash
code --install-extension vscjava.vscode-java-pack
code --install-extension vmware.vscode-boot-dev-pack
code --install-extension eamodio.gitlens
code --install-extension humao.rest-client
```

| Extensión                          | Para qué sirve                                              |
|------------------------------------|-------------------------------------------------------------|
| **Extension Pack for Java**        | Compilación, depuración, tests, Maven, autocompletado       |
| **Spring Boot Extension Pack**     | Spring Initializr, dashboard, soporte de `application.yml`  |
| **GitLens**                        | Historial y blame de Git dentro del editor                  |
| **REST Client** (opcional)         | Lanzar peticiones HTTP desde archivos `.http`               |

### Verificación

1. Abre VS Code y pulsa `Ctrl + Shift + P`.
2. Escribe `Java: Configure Java Runtime`.
3. Comprueba que aparece el **JDK 21** como runtime detectado.

---

## 4. Spring Tools

Spring Tools (STS) ofrece soporte específico para Spring Boot. Tienes dos opciones; **elige una** según tu preferencia.

### Opción A: Spring Tools dentro de VS Code (recomendada para este curso)

Ya la instalaste en el paso anterior con el **Spring Boot Extension Pack**. Incluye:

- **Spring Initializr**: crea proyectos desde `Ctrl + Shift + P → Spring Initializr: Create a Maven Project`.
- **Spring Boot Dashboard**: panel lateral para arrancar y parar aplicaciones.
- Autocompletado en `application.properties` / `application.yml`.

### Opción B: Spring Tools 4 for Eclipse (IDE independiente)

1. Descarga desde https://spring.io/tools
2. Ejecuta el archivo `.jar` autoextraíble con doble clic o con `java -jar spring-tool-suite-4-x.x.x.jar`.
3. Abre `SpringToolSuite4.exe` y elige una carpeta de *workspace*.
4. En `Window → Preferences → Java → Installed JREs` confirma que aparece el JDK 21.

### Crear tu primer proyecto (para verificar)

1. `Ctrl + Shift + P → Spring Initializr: Create a Maven Project`
2. Selecciona:
   - Spring Boot: **3.x** (la última estable)
   - Lenguaje: **Java**
   - Group: `com.curso`
   - Artifact: `demo`
   - Packaging: **Jar**
   - Java: **21**
   - Dependencias: **Spring Web**, **Spring Boot DevTools**
3. Abre la carpeta generada y ejecuta:

```bash
./mvnw spring-boot:run        # macOS / Linux
mvnw.cmd spring-boot:run      # Windows
```

4. Si ves en consola `Started DemoApplication in X seconds`, todo funciona.

---

## 5. Base de datos

Para las prácticas iniciales **no necesitas instalar ninguna base de datos**. Empezaremos con datos *quemados* (hardcodeados) en el backend y más adelante pasaremos a una base en memoria o real.

### Opción A: Datos quemados en el backend (sin instalación)

Un repositorio en memoria con una lista de Java. Sirve para practicar controladores y servicios sin preocuparse por persistencia.

```java
package com.curso.demo.repository;

import com.curso.demo.model.Producto;
import org.springframework.stereotype.Repository;

import java.util.ArrayList;
import java.util.List;
import java.util.Optional;

@Repository
public class ProductoRepository {

    private final List<Producto> productos = new ArrayList<>(List.of(
        new Producto(1L, "Teclado", 25.90),
        new Producto(2L, "Ratón", 12.50),
        new Producto(3L, "Monitor", 189.00)
    ));

    public List<Producto> findAll() {
        return productos;
    }

    public Optional<Producto> findById(Long id) {
        return productos.stream().filter(p -> p.getId().equals(id)).findFirst();
    }

    public Producto save(Producto producto) {
        productos.add(producto);
        return producto;
    }
}
```

> Los datos se pierden al reiniciar la aplicación. Es el comportamiento esperado.

### Opción B: H2 en memoria (sin instalación, con SQL real)

Añade la dependencia en `pom.xml`:

```xml
<dependency>
    <groupId>com.h2database</groupId>
    <artifactId>h2</artifactId>
    <scope>runtime</scope>
</dependency>
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-jpa</artifactId>
</dependency>
```

Y en `src/main/resources/application.properties`:

```properties
spring.datasource.url=jdbc:h2:mem:cursodb
spring.datasource.username=sa
spring.datasource.password=
spring.jpa.hibernate.ddl-auto=update
spring.h2.console.enabled=true
```

Consola web: http://localhost:8080/h2-console (JDBC URL: `jdbc:h2:mem:cursodb`).

### Opción C: PostgreSQL o MySQL (instalación local)

Solo cuando el profesor lo indique.

| Motor      | Descarga                                     | Cliente gráfico                  |
|------------|----------------------------------------------|----------------------------------|
| PostgreSQL | https://www.postgresql.org/download/         | pgAdmin (viene con el instalador)|
| MySQL      | https://dev.mysql.com/downloads/installer/   | MySQL Workbench                  |

Alternativa con Docker (si ya lo tienes instalado):

```bash
docker run --name curso-postgres -e POSTGRES_PASSWORD=curso -p 5432:5432 -d postgres:16
```

---

## 6. Postman y variables de entorno

Postman sirve para lanzar peticiones HTTP a tus microservicios sin necesidad de un frontend.

### Instalación

1. Descarga desde https://www.postman.com/downloads/
2. Instala y crea una cuenta gratuita (o usa la opción *Lightweight API Client* sin cuenta).

### Por qué usar variables de entorno

Evitan repetir la URL, el puerto o el token en cada petición. Si cambias de `localhost` a un servidor remoto, solo modificas la variable.

### Crear un entorno

1. En la barra lateral pulsa **Environments → +**.
2. Nombra el entorno `Local`.
3. Añade estas variables:

| Variable   | Initial value              | Current value              |
|------------|----------------------------|----------------------------|
| `baseUrl`  | `http://localhost:8080`    | `http://localhost:8080`    |
| `apiPath`  | `/api/v1`                  | `/api/v1`                  |
| `token`    | *(vacío)*                  | *(vacío)*                  |

4. Guarda y selecciona `Local` en el desplegable de entornos (arriba a la derecha).

### Usar las variables en una petición

Escribe las variables entre dobles llaves:

```
GET {{baseUrl}}{{apiPath}}/productos
GET {{baseUrl}}{{apiPath}}/productos/1
POST {{baseUrl}}{{apiPath}}/productos
```

Para cabeceras:

```
Authorization: Bearer {{token}}
```

### Guardar un valor desde la respuesta (script de Tests)

En la pestaña **Scripts → Post-response** de una petición de login:

```javascript
const body = pm.response.json();
pm.environment.set("token", body.token);
```

### Organización recomendada

- Crea una **Collection** por microservicio.
- Guarda cada petición dentro de la colección con nombre descriptivo (`Listar productos`, `Crear producto`).
- Exporta la colección y el entorno (`... → Export`) y súbelos al repositorio en una carpeta `postman/`.

---

## 7. Git

Control de versiones. Todo el trabajo del curso se entrega mediante repositorios Git.

### Instalación

- Windows: https://git-scm.com/download/win (acepta las opciones por defecto; asegúrate de que **Git Bash** quede instalado).
- macOS: `brew install git` o `xcode-select --install`.
- Linux: `sudo apt install git`.

### Configuración inicial (obligatoria, una sola vez)

```bash
git config --global user.name "Tu Nombre"
git config --global user.email "tu.correo@ejemplo.com"
git config --global init.defaultBranch main
git config --global core.autocrlf true     # solo en Windows
```

### Verificación

```bash
git --version
git config --list
```

### Cuenta en GitHub

1. Crea una cuenta en https://github.com/ con el mismo correo que configuraste.
2. Genera una clave SSH para no escribir la contraseña en cada `push`:

```bash
ssh-keygen -t ed25519 -C "tu.correo@ejemplo.com"
cat ~/.ssh/id_ed25519.pub
```

3. Copia la clave pública en `GitHub → Settings → SSH and GPG keys → New SSH key`.
4. Comprueba la conexión:

```bash
ssh -T git@github.com
```

### Archivo `.gitignore` para proyectos Spring Boot

Crea este archivo en la raíz de cada proyecto:

```gitignore
target/
*.class
*.jar
*.war
.idea/
*.iml
.vscode/
.settings/
.classpath
.project
.DS_Store
application-local.properties
```

### Flujo básico

```bash
git init
git add .
git commit -m "Proyecto inicial"
git remote add origin git@github.com:usuario/repositorio.git
git push -u origin main
```

---

## 8. Checklist final

Marca cada línea cuando el comando devuelva lo esperado:

- [ ] `java -version` muestra **21**
- [ ] `mvn -version` muestra Maven 3.9+ y Java 21
- [ ] `code --version` muestra la versión de VS Code
- [ ] En VS Code aparecen instalados **Extension Pack for Java** y **Spring Boot Extension Pack**
- [ ] Un proyecto de Spring Initializr arranca con `mvnw spring-boot:run`
- [ ] `http://localhost:8080` responde (aunque sea con error 404 de Whitelabel)
- [ ] Postman tiene el entorno `Local` con `baseUrl` definida
- [ ] `git --version` responde y `git config user.name` muestra tu nombre
- [ ] `ssh -T git@github.com` saluda con tu usuario

---

## 9. Problemas frecuentes

| Síntoma                                                  | Causa probable                                     | Solución                                                                   |
|----------------------------------------------------------|----------------------------------------------------|----------------------------------------------------------------------------|
| `'java' no se reconoce como un comando`                  | `Path` no incluye el JDK                           | Revisa `JAVA_HOME` y `Path`, abre una terminal nueva                       |
| `mvn -version` muestra Java 17 u 8                       | `JAVA_HOME` apunta a otro JDK                      | Cambia `JAVA_HOME` al JDK 21                                               |
| `Port 8080 was already in use`                           | Otra app usa el puerto                             | Cierra la otra app o añade `server.port=8081` en `application.properties`  |
| VS Code no reconoce el proyecto como Java                | No abriste la carpeta que contiene `pom.xml`       | `File → Open Folder` sobre la raíz del proyecto                            |
| Postman devuelve `Could not send request`                | La app no está arrancada o `baseUrl` es incorrecta | Comprueba la consola de Spring y el entorno seleccionado                   |
| `Permission denied (publickey)` al hacer `push`          | Clave SSH no añadida a GitHub                      | Repite el paso de SSH en la sección de Git                                 |
| Caracteres raros (`Ã±`) en la consola de Windows         | Codificación de la terminal                        | Ejecuta `chcp 65001` o usa Git Bash                                        |

---

*Última revisión: septiembre 2026.*
