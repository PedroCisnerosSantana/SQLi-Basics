# SQLi-Basics
Este repositorio dará una visión básica de las inyecciones SQL. Para los laboratios de SQLi vamos a usar portswigger
NOTA: Necesitarás una cuenta de portswigger para acceder a los laboratios.

# ¿Qué es una inyección SQL? 
Una inyección SQL es una vulnerabilidad web, es de las más antiguas y de las más peligrosas. Permite realizar consultas SQL a los atacantes, gracias a esto pueden ver información de otros usuarios, modificar e incluso borrar bases de datos al completo.

Aquí podemos ver un ejemplo de como funciona un ataque de inyección SQL:
![SQLi example](https://portswigger.net/web-security/images/sql-injection.svg)
Como vemos el atacante es capaz de modificar la consulta SQL y de esta forma conseguir los nombres de usuario y contraseña de la tabla de usuarios.

# Ejemplos de inyecciones SQL

* Conseguir datos ocultos
* Cambiar la logica de la aplicación
* Como prevenir los ataques de inyecciones SQL

Estos aspectos se verán a lo largo del documento

## Conseguir datos ocultos

Para esta parte iremos a este enlace de portswigger [URL](https://portswigger.net/web-security/sql-injection/lab-retrieve-hidden-data)  
Una vez ahí le damos a "Access the lab" para ir al lab indicado. Verás algo como esto:
![hidden data](https://i.imgur.com/c9deTbc.png)

El SQL que ejecuta es el siguiente:
```sql
SELECT * FROM products WHERE category = 'Gifts' AND releadsed = 1
```

Tenemos que conseguir que nos muestre todos los regalos sin importar el campo released, es decir tanto 0 como 1.  

Solo tenemos que filtrar por una categoría y al final añadir lo siguiente  
```sql
' OR 1 = 1--
```
Esto omitirá el AND de la query ya que comentamos el resto y ponemos un 1 = 1, lo cual siempre es True  
La query que hará será la siguiente:  
```sql
SELECT * FROM products WHERE category = 'Gifts' OR 1=1--' AND released = 1
```

## Cambiar la lógica de la aplicacion
Supongamos que tenemos una aplicacion que tiene un login de usuario, siendo el usuario wiener y la contraseña bluecheese la query sería la siguiente:  
```sql
SELECT * FROM users WHERE username='wiener' AND password='bluecheese'
```
Para esta parte iremos a este enlace de portswigger [URL](https://portswigger.net/web-security/sql-injection/lab-login-bypass)  
Solo tenemos que ir al login y en el username ponemos esto: `administrator'--` y en la contraseña lo que quieras  

La sentencia SQL sería algo como:  
```sql
SELECT * FROM users WHERE username = 'administrator'--' AND password = ''
```  

## Como prevenir los ataques de inyeccion SQL
Dado el siguiente codigo java para realizar la query, veremos como se puede blindar 
```java
String query = "SELECT * FROM products WHERE category = '"+ input + "'";
Statement statement = connection.createStatement();
ResultSet resultSet = statement.executeQuery(query);
```
La forma mas fácil es parametrizar la query, es decir que cada input sea un parametro que se le pasa por el programa:
```java
PreparedStatement statement = connection.prepareStatement("SELECT * FROM products WHERE category = ?");
statement.setString(1, input);
ResultSet resultSet = statement.executeQuery();
```
La string que se usa para la query debe ser una constante hardcodeada.
