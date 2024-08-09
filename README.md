# Crear un ambiente de desarrollo con SpringBoot

Sigue las instrucciones de este repositorio para crear tu primer hola mundo en un framework (spring).

1. Inicia un code space con github.
2. Verifica que la versión de java jdk sea mayor a la 17 en la terminal.
```java --version```
3. Si la versión es menor, actualiza el contenedor utilizando el siguiente manual: https://docs.github.com/en/codespaces/setting-up-your-project-for-codespaces/adding-a-dev-container-configuration/setting-up-your-java-project-for-codespaces
4. Una vez tengas una versión de java superior a la 17, abre la pestaña de extensiones e instala Spring Boot Extension Pack y Extension Pack for Java.
5. En la barra de comandos ejecuta el inicializador se spring
- ```>Spring Initializr: Create Maven Project```
- Selecciona la version más actual
- Selecciona Java
- com.example
- holamundo
- Jar
- Tu version de java previamente verificada (paso 2)
- Agrega el módulo de Spring Web
- Selecciona la ruta actual
6. Revisa tus archivos y activa la extensión de java en la barra inferior a la derecha de vscode
7. Abre el archivo *holamundo/src/main/java/com/example/holamundo/HolamundoApplication.java*
8. Debes ver una opción Run a la que harás clic sobre el siguiente código.
```java
public static void main(String[] args) {
    SpringApplication.run(HolamundoApplication.class, args);
}
```
9. Ejecutará spring y verás un pop up que diga: *la aplicación se ejecutó en el puerto 8080* con un botón con la leyenda “abrir en el navegador”, da click en él.
10. 10.Verás un sitio web plano con un mensaje Whitelabel Error Page.
11. Regresa a la terminal y presiona ```ctrl + c```
12. Crea el archivo con el siguiente contenido *holamundo/src/main/java/com/example/holamundo/HelloWoldController.java*
```java
 
package com.example.holamundo;
 
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;
@RestController
public class HelloWoldController {
 
    @GetMapping("hello")
    public static String GetHelloWorld(){
    return "Hello World";
 }
 
}
 
```
13. Ejecuta nuevamente spring abriendo nuevamente la ventana con el pop up
14. Agrega /hello al final de la url
15. Revisa tener hello world como contenido del sitio web
16. Pausa spring y agrega los cambios al repositorio
- ```git add *```
- ```git commit -m “mi primera aplicación con spring”```
- ```git push origin main```
