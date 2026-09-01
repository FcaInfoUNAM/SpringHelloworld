# Crear un entorno de desarrollo con Dev Containers y Java

Este proyecto usa un contenedor de desarrollo definido en el archivo .devcontainer/devcontainer.json. La finalidad es que VS Code abra el proyecto dentro de un entorno con Java listo para usar, en lugar de depender de la instalación local del sistema operativo.

La configuración del repositorio incluye una imagen de Java de Microsoft, un feature para Maven y varias extensiones de VS Code para trabajar con Java y Spring Boot.

## Requisitos previos

De acuerdo con la documentación oficial de VS Code y Dev Containers, antes de abrir un proyecto en un contenedor necesitas:

1. Instalar Docker en tu equipo.
   - En Windows y macOS, normalmente se usa Docker Desktop.
   - En Linux, se requiere Docker Engine y, en algunos casos, Docker Compose.
2. Instalar Visual Studio Code.
3. Instalar la extensión Dev Containers en VS Code.

Estas herramientas son necesarias porque Dev Containers usa Docker para crear y administrar el entorno del proyecto, y VS Code para conectarse a ese contenedor como si fuera un entorno de trabajo normal.

## Paso a paso: crear un dev container con Java

### 1. Abre la carpeta del proyecto en VS Code

Abre esta carpeta desde VS Code y asegúrate de que el proyecto esté visible en el explorador.

### 2. Agrega la configuración del contenedor

En la paleta de comandos (Ctrl + Shift + P), ejecuta:

- Dev Containers: Add Dev Container Configuration Files...

Si prefieres crear la configuración manualmente, el archivo debe quedar en:

- .devcontainer/devcontainer.json

En este repositorio ya existe la configuración base, y su contenido es el siguiente:

```json
{
  "name": "Java",
  "image": "mcr.microsoft.com/devcontainers/java:3-25-trixie",
  "features": {
    "ghcr.io/devcontainers/features/java:1": {
      "version": "none",
      "installMaven": "true",
      "installGradle": "false"
    }
  },
  "customizations": {
    "vscode": {
      "extensions": [
        "vmware.vscode-spring-boot",
        "MermaidChart.vscode-mermaid-chart",
        "vscjava.vscode-maven",
        "vscjava.vscode-spring-initializr"
      ]
    }
  }
}
```

### 3. Reabre la carpeta dentro del contenedor

Después de guardar el archivo, usa la acción:

- Dev Containers: Reopen in Container

VS Code descargará la imagen base, construirá el contenedor y se conectará automáticamente a él. La documentación oficial de Dev Containers indica que esta es la forma recomendada para abrir un proyecto en un entorno aislado y reproducible.

### 4. Verifica que Java esté disponible en el contenedor

Abre una terminal dentro de VS Code y ejecuta:

```bash
java --version
mvn -version
```

Si todo está bien, podrás ver la versión de Java y la versión de Maven instalada por el feature del contenedor.

## ¿Qué se instala y por qué?

La configuración actual del devcontainer.json instala exactamente estos elementos:

### 1. Image base: Java de Microsoft

```json
"image": "mcr.microsoft.com/devcontainers/java:3-25-trixie"
```

Este valor usa la imagen oficial de desarrollo de Microsoft para Java. La documentación oficial de Dev Containers y de los devcontainer images de Microsoft explica que estas imágenes preconstruidas están pensadas para ofrecer un entorno de desarrollo listo para usar, con un sistema operativo base, compiladores, herramientas y configuraciones de VS Code listos para el trabajo.

Por qué se usa:

- Proporciona Java en el contenedor.
- Mantiene una base estándar y reproducible.
- Evita instalar manualmente el JDK en el sistema operativo local.
- El tag `3-25-trixie` indica la familia de imagen y la versión de Java 25 con Debian Trixie.

### 2. Feature de Java con Maven

```json
"features": {
  "ghcr.io/devcontainers/features/java:1": {
    "version": "none",
    "installMaven": "true",
    "installGradle": "false"
  }
}
```

Los Features de Dev Containers son unidades reutilizables de configuración e instalación. La documentación oficial de Dev Containers los describe como herramientas pequeñas y compartibles que se agregan a un contenedor para instalar dependencias o runtimes adicionales.

Qué hace cada propiedad:

- `ghcr.io/devcontainers/features/java:1`: agrega la funcionalidad base para Java dentro del contenedor.
- `version": "none"`: no fuerza una versión específica del feature, sino que deja usar la versión que trae la imagen base.
- `installMaven": "true"`: instala Maven para compilar y gestionar proyectos Java con build lifecycle.
- `installGradle": "false"`: no instala Gradle en este proyecto, porque la configuración actual está orientada a Maven.

Por qué es importante:

- Java y Maven son esenciales para proyectos Java y Spring Boot.
- Maven permite crear, compilar y ejecutar proyectos con una estructura estándar.
- Evita que el entorno de desarrollo dependa del equipo local del desarrollador.

### 3. Extensión de Spring Boot

```json
"vmware.vscode-spring-boot"
```

Se instala dentro de VS Code para ofrecer soporte específico para proyectos Spring Boot, como:

- generación de proyectos
- asistencia de código
- ejecución y depuración
- integración con el ecosistema de Spring

La documentación oficial de VS Code indica que las extensiones se pueden instalar automáticamente dentro del contenedor para que la experiencia del desarrollador quede preparada cuando se conecta al proyecto.

### 4. Extensión de Spring Initializr para Java

```json
"vscjava.vscode-spring-initializr"
```

Esta extensión agrega soporte para crear proyectos Spring Boot desde VS Code de forma guiada. Permite generar la estructura base del proyecto, seleccionar dependencias y preparar un arranque rápido de una aplicación Java con Spring.

Es especialmente útil para esta guía porque facilita la creación del proyecto Spring Boot para una API REST sin tener que configurar manualmente todas las carpetas y archivos iniciales.

### 5. Extensión de Maven para Java

```json
"vscjava.vscode-maven"
```

Esta extensión aporta integración con Maven en VS Code, especialmente útil para proyectos Java. Permite:

- ejecutar tareas de Maven desde la interfaz
- explorar ciclos de vida del proyecto
- trabajar con dependencias y configuraciones de build
- compatibilidad con Java en el editor

### 6. Extensión de Mermaid

```json
"MermaidChart.vscode-mermaid-chart"
```

Aunque no forma parte del núcleo de Java, esta extensión se incluye para apoyar diagramas, documentación visual y análisis de procesos dentro del editor. En este repositorio puede ser útil para documentar arquitectura o flujo del sistema.

## Qué significa esto en la práctica

El archivo devcontainer.json indica que, cuando alguien abra este proyecto en VS Code, se creará un entorno con:

- Java 25 en un contenedor
- Maven instalado
- soporte para Spring Boot en VS Code
- Spring Initializr para crear proyectos Java/Spring con rapidez
- soporte para Maven y diagramas

Esto asegura que todos los integrantes del equipo cuenten con un entorno equivalente, sin depender de la versión exacta del JDK instalada en su máquina local.

## Opción alternativa: usar GitHub Codespaces

Si no vas a trabajar desde una computadora local con Docker instalado, otra opción oficial es usar GitHub Codespaces. Esta alternativa es útil cuando quieres abrir el proyecto en un entorno remoto y no dependes de la configuración local del equipo.

### ¿Cuándo conviene usar Codespaces?

- No tienes Docker instalado en tu equipo.
- Quieres trabajar desde un entorno reproducible y preconfigurado.
- Necesitas una máquina con Java y herramientas listas para usar sin configurarlas manualmente.
- Estás usando GitHub para abrir un repositorio y quieres una experiencia similar a VS Code en la nube.

### ¿Qué hace Codespaces con este proyecto?

GitHub Codespaces puede abrir el repositorio en un contenedor de desarrollo basado en la misma configuración del archivo `.devcontainer/devcontainer.json`. En otras palabras, el entorno sigue siendo el mismo que en una máquina local, pero se ejecuta en un host remoto gestionado por GitHub.

### Pasos para usar Codespaces

1. Abre el repositorio en GitHub.
2. Haz clic en el botón "Code".
3. Selecciona la opción "Codespaces".
4. Crea un nuevo codespace a partir de la rama principal del proyecto.
5. Espera a que GitHub construya el entorno y abra VS Code en el navegador o en tu editor conectado.
6. Verifica que Java y Maven estén disponibles ejecutando:

```bash
java --version
mvn -version
```

### Diferencias importantes respecto a un contenedor local

- No necesitas instalar Docker en tu computadora.
- El entorno se crea en la nube y se conecta a tu editor.
- La configuración del proyecto sigue siendo la misma, porque el repositorio incluye el archivo de configuración del dev container.
- Si se actualiza la configuración de `.devcontainer/devcontainer.json`, la creación del codespace tomará esos cambios al abrir o recrear el entorno.

La documentación oficial de GitHub Codespaces y VS Code muestra que esta opción está pensada exactamente para casos en los que el desarrollador no quiere mantener una instalación local de Docker, Java o dependencias del proyecto.

## Crear un proyecto Spring Boot vacío para una API REST

Una vez que el contenedor está abierto y funcionando, puedes crear tu proyecto Spring Boot desde VS Code usando Spring Initializr.

### 1. Crear el proyecto con Spring Initializr

En la paleta de comandos de VS Code (Ctrl + Shift + P), ejecuta:

- Spring Initializr: Create a Maven Project

Selecciona lo siguiente:

- Project type: Maven
- Language: Java
- Group: com.example
- Artifact: apirest
- Name: apirest
- Package name: com.example.apirest
- Packaging: Jar
- Java version: la versión disponible en el contenedor
- Dependencies: Spring Web

Luego elige la carpeta actual del proyecto para generar la estructura.

Esto genera una aplicación Java con Spring Boot y Maven lista para ejecutarse.

Si quieres crear el proyecto desde la terminal, también puedes usar el wrapper generado por Spring Initializr:

```bash
cd apirest
./mvnw spring-boot:run
```

### 2. Revisar la estructura generada

Deberías ver algo similar a esto:

```text
apirest/
├── pom.xml
├── src/
│   ├── main/
│   │   ├── java/
│   │   │   └── com/example/apirest/
│   │   │       └── ApiRestApplication.java
│   │   └── resources/
│   │       └── application.properties
└── mvnw
```

El archivo principal normalmente se llama `ApiRestApplication.java` y contiene la clase con el método `main` que inicializa la aplicación.

### 3. Iniciar la aplicación desde la terminal

Entra a la carpeta del proyecto y ejecuta la aplicación con Maven desde la terminal:

```bash
cd apirest
mvn spring-boot:run
```

Si el proyecto fue generado con Spring Initializr, también puedes usar el wrapper incluido:

```bash
cd apirest
./mvnw spring-boot:run
```

Cuando la aplicación arranque correctamente, Spring Boot mostrará en la consola un mensaje similar a:

```text
Started ApiRestApplication in ...
```

Y el sistema estará escuchando en el puerto `8080` por defecto.

Si quieres inicializarla de forma explícita desde la línea de comandos, también puedes hacerlo con el comando equivalente:

```bash
cd apirest
java -jar target/apirest-0.0.1-SNAPSHOT.jar
```

Este último comando se usa cuando ya se ha construido el proyecto y se quiere ejecutar el archivo JAR compilado.

### 4. Crear un controlador REST básico

Crea un archivo dentro de `src/main/java/com/example/apirest/` con el nombre `HelloController.java`:

```java
package com.example.apirest;

import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class HelloController {

    @GetMapping("/hello")
    public String hello() {
        return "Hello World";
    }
}
```

### 5. Probar el endpoint

Abre tu navegador o usa curl desde la terminal:

```bash
curl http://localhost:8080/hello
```

La respuesta esperada será:

```text
Hello World
```

### 6. Detener la aplicación

Cuando termines de probarla, puedes detener la ejecución con:

```bash
Ctrl + C
```

### 7. Recomendación de buena práctica

En proyectos reales, la API REST suele tener una estructura con:

- controlador
- servicio
- repositorio
- entidad
- DTO
- configuración

Pero para un primer proyecto vacío, lo más importante es entender cómo se inicializa Spring Boot, cómo se define un endpoint y cómo se prueba la respuesta del servidor.
