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
* Conseguir datos de otra tabla usando sentencias UNION
* Ver la estructura de la base de datos
* Inyecciones SQL 'a ciegas' (Blind SQLi)

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
