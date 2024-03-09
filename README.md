# **Consultas PostgreSQL EXAMEN** 

## **Consultas sobre una tabla**
1. Devuelve un listado con todos los pedidos que se han realizado. Los pedidos deben estar ordenados por la fecha de realización, mostrando en primer lugar los pedidos más recientes.

   ```sql
   SELECT * 
   FROM pedido 
   ORDER BY fecha DESC;
    id |  total  |   fecha    | id_cliente | id_comercial
   ----+---------+------------+------------+--------------
    16 | 2389.23 | 2019-03-11 |          1 |            5
    15 |  370.85 | 2019-03-11 |          1 |            5
    13 |  545.75 | 2019-01-25 |          6 |            1
     8 | 1983.43 | 2017-10-10 |          4 |            6
     1 |   150.5 | 2017-10-05 |          5 |            2
     3 |   65.26 | 2017-10-05 |          2 |            1
     5 |   948.5 | 2017-09-10 |          5 |            2
    12 |  3045.6 | 2017-04-25 |          2 |            1
    14 |  145.82 | 2017-02-02 |          6 |            1
     9 |  2480.4 | 2016-10-10 |          8 |            3
     2 |  270.65 | 2016-09-10 |          1 |            5
    11 |   75.29 | 2016-08-17 |          3 |            7
     4 |   110.5 | 2016-08-17 |          8 |            3
     6 |  2400.6 | 2016-07-27 |          7 |            1
     7 |    5760 | 2015-09-10 |          2 |            1
    10 |  250.45 | 2015-06-27 |          8 |            2
   ```
   
2. Devuelve todos los datos de los dos pedidos de mayor valor.

     ```sql
     SELECT * 
     FROM pedido 
     ORDER BY total 
     DESC LIMIT 2;
      id | total  |   fecha    | id_cliente | id_comercial
     ----+--------+------------+------------+--------------
       7 |   5760 | 2015-09-10 |          2 |            1
      12 | 3045.6 | 2017-04-25 |          2 |            1
     ```
     
3. Devuelve un listado con los identificadores de los clientes que han realizado algún pedido. Tenga en cuenta que no debe mostrar identificadores que estén repetidos.

     ```sql
     SELECT DISTINCT cliente.id
     FROM cliente
     JOIN pedido ON cliente.id = pedido.id_cliente;
      id
     ----
       4
       6
       2
       7
       3
       1
       5
       8
     ```
     
4. Devuelve un listado de todos los pedidos que se realizaron durante el año 2017, cuya cantidad total sea superior a 500€.

     ```sql
     SELECT * 
     FROM pedido
     WHERE EXTRACT(YEAR FROM fecha) = 2017 AND total > 500;
      id |  total  |   fecha    | id_cliente | id_comercial
     ----+---------+------------+------------+--------------
       5 |   948.5 | 2017-09-10 |          5 |            2
       8 | 1983.43 | 2017-10-10 |          4 |            6
      12 |  3045.6 | 2017-04-25 |          2 |            1
     ```
     
5. Devuelve un listado con el nombre y los apellidos de los comerciales que tienen una comisión entre 0.05 y 0.11.

     ```sql
     SELECT nombre, apellido1, apellido2
     FROM comercial
     WHERE comisión BETWEEN 0.05 AND 0.11;
     nombre |  apellido1  |   apellido2    
     ----+---------+------------+-------
     Diego  |   Flores    |   Salas 
     Antonio|    Vega     |   Hérnandez 
     Alfredo|    Ruiz     |   Flores 
     ```
     
6. Devuelve el valor de la comisión de mayor valor que existe en la tabla comercial.

     ```sql
     SELECT MAX(comisión)
     FROM comercial;
     +----------------+
     | MAX(comisión)  |
     +----------------+
     |           0.15 |
     +----------------+
     ```

7. Devuelve el identificador, nombre y primer apellido de aquellos clientes cuyo segundo apellido no es NULL. El listado deberá estar ordenado alfabéticamente por apellidos y nombre.

     ```sql
     SELECT id, nombre, apellido1
     FROM cliente
     WHERE apellido2 IS NOT NULL
     ORDER BY apellido1, nombre;
     +----+-----------+-----------+
     | id | nombre    | apellido1 |
     +----+-----------+-----------+
     |  9 | Guillermo | López     |
     |  5 | Marcos    | Loyola    |
     |  1 | Aarón     | Rivero    |
     |  3 | Adolfo    | Rubio     |
     |  8 | Pepe      | Ruiz      |
     |  2 | Adela     | Salas     |
     | 10 | Daniel    | Santana   |
     |  6 | María     | Santana   |
     +----+-----------+-----------+
     ```

8. Devuelve un listado de los nombres de los clientes que empiezan por A y terminan por n y también los nombres que empiezan por P. El listado deberá estar ordenado alfabéticamente.

     ```sql
     SELECT nombre
     FROM cliente
     WHERE (nombre LIKE 'A%n' OR nombre LIKE 'P%')
     ORDER BY nombre;
     +---------+
     | nombre  |
     +---------+
     | Aarón   |
     | Adrián  |
     | Pepe    |
     | Pilar   |
     +---------+
     ```

9. Devuelve un listado de los nombres de los clientes que no empiezan por A. El listado deberá estar ordenado alfabéticamente.

     ```sql
     SELECT nombre
     FROM cliente
     WHERE nombre NOT LIKE 'A%'
     ORDER BY nombre;
     +-----------+
     | nombre    |
     +-----------+
     | Daniel    |
     | Guillermo |
     | Marcos    |
     | María     |
     | Pepe      |
     | Pilar     |
     +-----------+
     ```

10. Devuelve un listado con los nombres de los comerciales que terminan por "el" o "o". Tenga en cuenta que se deberán eliminar los nombres repetidos.

     ```sql
     SELECT DISTINCT nombre 
     FROM comercial
     WHERE nombre LIKE '%el' OR nombre LIKE '%o';
     +---------+
     | nombre  |
     +---------+
     | Daniel  |
     | Diego   |
     | Antonio |
     | Manuel  |
     | Alfredo |
     +---------+
     ```

## **Consultas multitabla (Composición interna)**
1. Devuelve un listado con el identificador, nombre y los apellidos de todos los clientes que han realizado algún pedido. El listado debe estar ordenado alfabéticamente y se deben eliminar los elementos repetidos.
   ```sql
   SELECT DISTINCT cliente.id, nombre, apellido1, apellido2 
   FROM cliente
   JOIN pedido ON cliente.id = pedido.id_cliente
   ORDER BY nombre, apellido1, apellido2;
   +----+---------+-----------+-----------+
   | id | nombre  | apellido1 | apellido2 |
   +----+---------+-----------+-----------+
   |  1 | Aarón   | Rivero    | Gómez     |
   |  2 | Adela   | Salas     | Díaz      |
   |  3 | Adolfo  | Rubio     | Flores    |
   |  4 | Adrián  | Suárez    | NULL      |
   |  5 | Marcos  | Loyola    | Méndez    |
   |  6 | María   | Santana   | Moreno    |
   |  8 | Pepe    | Ruiz      | Santana   |
   |  7 | Pilar   | Ruiz      | NULL      |
   +----+---------+-----------+-----------+
   ```
   
2. Devuelve un listado que muestre todos los pedidos que ha realizado cada cliente. El resultado debe mostrar todos los datos de los pedidos y del cliente. El listado debe mostrar los datos de los clientes ordenados alfabéticamente.

     ```sql
     SELECT pedido.*, cliente.*
     FROM pedido
     JOIN cliente ON pedido.id_cliente = cliente.id
     ORDER BY cliente.*;
     +----+---------+------------+------------+--------------+----+---------+-----------+-----------+----------+------------+
     | id | total   | fecha      | id_cliente | id_comercial | id | nombre  | apellido1 | apellido2 | ciudad   | categoría  |
     +----+---------+------------+------------+--------------+----+---------+-----------+-----------+----------+------------+
     |  2 |  270.65 | 2016-09-10 |          1 |            5 |  1 | Aarón   | Rivero    | Gómez     | Almería  |        100 |
     | 15 |  370.85 | 2019-03-11 |          1 |            5 |  1 | Aarón   | Rivero    | Gómez     | Almería  |        100 |
     | 16 | 2389.23 | 2019-03-11 |          1 |            5 |  1 | Aarón   | Rivero    | Gómez     | Almería  |        100 |
     |  3 |   65.26 | 2017-10-05 |          2 |            1 |  2 | Adela   | Salas     | Díaz      | Granada  |        200 |
     |  7 |    5760 | 2015-09-10 |          2 |            1 |  2 | Adela   | Salas     | Díaz      | Granada  |        200 |
     | 12 |  3045.6 | 2017-04-25 |          2 |            1 |  2 | Adela   | Salas     | Díaz      | Granada  |        200 |
     | 11 |   75.29 | 2016-08-17 |          3 |            7 |  3 | Adolfo  | Rubio     | Flores    | Sevilla  |       NULL |
     |  8 | 1983.43 | 2017-10-10 |          4 |            6 |  4 | Adrián  | Suárez    | NULL      | Jaén     |        300 |
     |  1 |   150.5 | 2017-10-05 |          5 |            2 |  5 | Marcos  | Loyola    | Méndez    | Almería  |        200 |
     |  5 |   948.5 | 2017-09-10 |          5 |            2 |  5 | Marcos  | Loyola    | Méndez    | Almería  |        200 |
     | 13 |  545.75 | 2019-01-25 |          6 |            1 |  6 | María   | Santana   | Moreno    | Cádiz    |        100 |
     | 14 |  145.82 | 2017-02-02 |          6 |            1 |  6 | María   | Santana   | Moreno    | Cádiz    |        100 |
     |  6 |  2400.6 | 2016-07-27 |          7 |            1 |  7 | Pilar   | Ruiz      | NULL      | Sevilla  |        300 |
     |  4 |   110.5 | 2016-08-17 |          8 |            3 |  8 | Pepe    | Ruiz      | Santana   | Huelva   |        200 |
     |  9 |  2480.4 | 2016-10-10 |          8 |            3 |  8 | Pepe    | Ruiz      | Santana   | Huelva   |        200 |
     | 10 |  250.45 | 2015-06-27 |          8 |            2 |  8 | Pepe    | Ruiz      | Santana   | Huelva   |        200 |
     +----+---------+------------+------------+--------------+----+---------+-----------+-----------+----------+------------+
     ```
     
3. Devuelve un listado que muestre todos los pedidos en los que ha participado un comercial. El resultado debe mostrar todos los datos de los pedidos y de los comerciales. El listado debe mostrar los datos de los comerciales ordenados alfabéticamente.

     ```sql
     SELECT pedido.*, comercial.*
     FROM pedido
     JOIN comercial ON pedido.id_comercial = comercial.id
     ORDER BY comercial.*;
     +----+---------+------------+------------+--------------+----+---------+------------+------------+-----------+
     | id | total   | fecha      | id_cliente | id_comercial | id | nombre  | apellido1  | apellido2  | comisión  |
     +----+---------+------------+------------+--------------+----+---------+------------+------------+-----------+
     |  3 |   65.26 | 2017-10-05 |          2 |            1 |  1 | Daniel  | Sáez       | Vega       |      0.15 |
     |  6 |  2400.6 | 2016-07-27 |          7 |            1 |  1 | Daniel  | Sáez       | Vega       |      0.15 |
     |  7 |    5760 | 2015-09-10 |          2 |            1 |  1 | Daniel  | Sáez       | Vega       |      0.15 |
     | 12 |  3045.6 | 2017-04-25 |          2 |            1 |  1 | Daniel  | Sáez       | Vega       |      0.15 |
     | 13 |  545.75 | 2019-01-25 |          6 |            1 |  1 | Daniel  | Sáez       | Vega       |      0.15 |
     | 14 |  145.82 | 2017-02-02 |          6 |            1 |  1 | Daniel  | Sáez       | Vega       |      0.15 |
     |  1 |   150.5 | 2017-10-05 |          5 |            2 |  2 | Juan    | Gómez      | López      |      0.13 |
     |  5 |   948.5 | 2017-09-10 |          5 |            2 |  2 | Juan    | Gómez      | López      |      0.13 |
     | 10 |  250.45 | 2015-06-27 |          8 |            2 |  2 | Juan    | Gómez      | López      |      0.13 |
     |  4 |   110.5 | 2016-08-17 |          8 |            3 |  3 | Diego   | Flores     | Salas      |      0.11 |
     |  9 |  2480.4 | 2016-10-10 |          8 |            3 |  3 | Diego   | Flores     | Salas      |      0.11 |
     |  2 |  270.65 | 2016-09-10 |          1 |            5 |  5 | Antonio | Carretero  | Ortega     |      0.12 |
     | 15 |  370.85 | 2019-03-11 |          1 |            5 |  5 | Antonio | Carretero  | Ortega     |      0.12 |
     | 16 | 2389.23 | 2019-03-11 |          1 |            5 |  5 | Antonio | Carretero  | Ortega     |      0.12 |
     |  8 | 1983.43 | 2017-10-10 |          4 |            6 |  6 | Manuel  | Domínguez  | Hernández  |      0.13 |
     | 11 |   75.29 | 2016-08-17 |          3 |            7 |  7 | Antonio | Vega       | Hernández  |      0.11 |
     +----+---------+------------+------------+--------------+----+---------+------------+------------+-----------+
     ```
     
4. Devuelve un listado que muestre todos los clientes, con todos los pedidos que han realizado y con los datos de los comerciales asociados a cada pedido.

     ```sql
     SELECT cliente.id, cliente.nombre, pedido.id, pedido.total, pedido.fecha, comercial.id, comercial.nombre, comercial.apellido1, comercial.apellido2
     FROM cliente, pedido, comercial
     WHERE cliente.id = pedido.id_cliente AND pedido.id_comercial = comercial.id
     ORDER BY cliente.id, pedido.id;
     +----+---------+----+---------+------------+----+---------+------------+------------+
     | id | nombre  | id | total   | fecha      | id | nombre  | apellido1  | apellido2  |
     +----+---------+----+---------+------------+----+---------+------------+------------+
     |  1 | Aarón   |  2 |  270.65 | 2016-09-10 |  5 | Antonio | Carretero  | Ortega     |
     |  1 | Aarón   | 15 |  370.85 | 2019-03-11 |  5 | Antonio | Carretero  | Ortega     |
     |  1 | Aarón   | 16 | 2389.23 | 2019-03-11 |  5 | Antonio | Carretero  | Ortega     |
     |  2 | Adela   |  3 |   65.26 | 2017-10-05 |  1 | Daniel  | Sáez       | Vega       |
     |  2 | Adela   |  7 |    5760 | 2015-09-10 |  1 | Daniel  | Sáez       | Vega       |
     |  2 | Adela   | 12 |  3045.6 | 2017-04-25 |  1 | Daniel  | Sáez       | Vega       |
     |  3 | Adolfo  | 11 |   75.29 | 2016-08-17 |  7 | Antonio | Vega       | Hernández  |
     |  4 | Adrián  |  8 | 1983.43 | 2017-10-10 |  6 | Manuel  | Domínguez  | Hernández  |
     |  5 | Marcos  |  1 |   150.5 | 2017-10-05 |  2 | Juan    | Gómez      | López      |
     |  5 | Marcos  |  5 |   948.5 | 2017-09-10 |  2 | Juan    | Gómez      | López      |
     |  6 | María   | 13 |  545.75 | 2019-01-25 |  1 | Daniel  | Sáez       | Vega       |
     |  6 | María   | 14 |  145.82 | 2017-02-02 |  1 | Daniel  | Sáez       | Vega       |
     |  7 | Pilar   |  6 |  2400.6 | 2016-07-27 |  1 | Daniel  | Sáez       | Vega       |
     |  8 | Pepe    |  4 |   110.5 | 2016-08-17 |  3 | Diego   | Flores     | Salas      |
     |  8 | Pepe    |  9 |  2480.4 | 2016-10-10 |  3 | Diego   | Flores     | Salas      |
     |  8 | Pepe    | 10 |  250.45 | 2015-06-27 |  2 | Juan    | Gómez      | López      |
     +----+---------+----+---------+------------+----+---------+------------+------------+
     ```
     
5. Devuelve un listado de todos los clientes que realizaron un pedido durante el año 2017, cuya cantidad esté entre 300 € y 1000 €.

     ```sql
     SELECT nombre
     FROM cliente
     JOIN pedido ON cliente.id = pedido.id_cliente
     WHERE EXTRACT(YEAR FROM fecha) = 2017 AND total BETWEEN 300 AND 1000;
     +--------+
     | nombre |
     +--------+
     | Marcos |
     +--------+
     ```
     
6. Devuelve el nombre y los apellidos de todos los comerciales que ha participado en algún pedido realizado por María Santana Moreno.

     ```sql
     SELECT DISTINCT comercial.nombre, comercial.apellido1, comercial.apellido2
     FROM comercial
     JOIN pedido ON comercial.id = pedido.id_comercial
     LEFT JOIN cliente ON pedido.id_cliente = cliente.id
     WHERE cliente.nombre = 'María'
     AND cliente.apellido1 = 'Santana'
     AND cliente.apellido2 = 'Moreno';
     +--------+-----------+-----------+
     | nombre | apellido1 | apellido2 |
     +--------+-----------+-----------+
     | Daniel | Sáez      | Vega      |
     +--------+-----------+-----------+
     ```

7. Devuelve el nombre de todos los clientes que han realizado algún pedido con el comercial Daniel Sáez Vega.

     ```sql
     SELECT DISTINCT cliente.nombre
     FROM cliente
     JOIN pedido ON cliente.id = pedido.id_cliente
     JOIN comercial ON pedido.id_comercial = comercial.id
     WHERE comercial.nombre = 'Daniel'
     AND comercial.apellido1 = 'Sáez'
     AND comercial.apellido2 = 'Vega';
     +--------+
     | nombre |
     +--------+
     | Adela  |
     | Pilar  |
     | María  |
     +--------+
     ```

## **Consultas multitabla (Composición externa)**     
1. Devuelve un listado con todos los clientes junto con los datos de los pedidos que han realizado. Este listado también debe incluir los clientes que no han realizado ningún pedido. El listado debe estar ordenado alfabéticamente por el primer apellido, segundo apellido y nombre de los clientes.
   ```sql
   SELECT nombre, apellido1, apellido2, pedido.*
   FROM cliente
   LEFT JOIN pedido ON cliente.id = pedido.id_cliente
   ORDER BY apellido1, apellido2, nombre;
   +-----------+-----------+-----------+------+---------+------------+------------+--------------+
   | nombre    | apellido1 | apellido2 | id   | total   | fecha      | id_cliente | id_comercial |
   +-----------+-----------+-----------+------+---------+------------+------------+--------------+
   | Guillermo | López     | Gómez     | NULL |    NULL | NULL       |       NULL |         NULL |
   | Marcos    | Loyola    | Méndez    |    1 |   150.5 | 2017-10-05 |          5 |            2 |
   | Marcos    | Loyola    | Méndez    |    5 |   948.5 | 2017-09-10 |          5 |            2 |
   | Aarón     | Rivero    | Gómez     |    2 |  270.65 | 2016-09-10 |          1 |            5 |
   | Aarón     | Rivero    | Gómez     |   15 |  370.85 | 2019-03-11 |          1 |            5 |
   | Aarón     | Rivero    | Gómez     |   16 | 2389.23 | 2019-03-11 |          1 |            5 |
   | Adolfo    | Rubio     | Flores    |   11 |   75.29 | 2016-08-17 |          3 |            7 |
   | Pilar     | Ruiz      | NULL      |    6 |  2400.6 | 2016-07-27 |          7 |            1 |
   | Pepe      | Ruiz      | Santana   |    4 |   110.5 | 2016-08-17 |          8 |            3 |
   | Pepe      | Ruiz      | Santana   |    9 |  2480.4 | 2016-10-10 |          8 |            3 |
   | Pepe      | Ruiz      | Santana   |   10 |  250.45 | 2015-06-27 |          8 |            2 |
   | Adela     | Salas     | Díaz      |    3 |   65.26 | 2017-10-05 |          2 |            1 |
   | Adela     | Salas     | Díaz      |    7 |    5760 | 2015-09-10 |          2 |            1 |
   | Adela     | Salas     | Díaz      |   12 |  3045.6 | 2017-04-25 |          2 |            1 |
   | Daniel    | Santana   | Loyola    | NULL |    NULL | NULL       |       NULL |         NULL |
   | María     | Santana   | Moreno    |   13 |  545.75 | 2019-01-25 |          6 |            1 |
   | María     | Santana   | Moreno    |   14 |  145.82 | 2017-02-02 |          6 |            1 |
   | Adrián    | Suárez    | NULL      |    8 | 1983.43 | 2017-10-10 |          4 |            6 |
   +-----------+-----------+-----------+------+---------+------------+------------+--------------+
   ```
   
2. Devuelve un listado con todos los comerciales junto con los datos de los pedidos que han realizado. Este listado también debe incluir los comerciales que no han realizado ningún pedido. El listado debe estar ordenado alfabéticamente por el primer apellido, segundo apellido y nombre de los comerciales.

     ```sql
     SELECT apellido1, apellido2, nombre, pedido.*
     FROM comercial
     LEFT JOIN pedido ON comercial.id = pedido.id_comercial
     ORDER BY apellido1, apellido2, nombre;
     +------------+------------+---------+------+---------+------------+------------+--------------+
     | apellido1  | apellido2  | nombre  | id   | total   | fecha      | id_cliente | id_comercial |
     +------------+------------+---------+------+---------+------------+------------+--------------+
     | Carretero  | Ortega     | Antonio |    2 |  270.65 | 2016-09-10 |          1 |            5 |
     | Carretero  | Ortega     | Antonio |   15 |  370.85 | 2019-03-11 |          1 |            5 |
     | Carretero  | Ortega     | Antonio |   16 | 2389.23 | 2019-03-11 |          1 |            5 |
     | Domínguez  | Hernández  | Manuel  |    8 | 1983.43 | 2017-10-10 |          4 |            6 |
     | Flores     | Salas      | Diego   |    4 |   110.5 | 2016-08-17 |          8 |            3 |
     | Flores     | Salas      | Diego   |    9 |  2480.4 | 2016-10-10 |          8 |            3 |
     | Gómez      | López      | Juan    |    1 |   150.5 | 2017-10-05 |          5 |            2 |
     | Gómez      | López      | Juan    |    5 |   948.5 | 2017-09-10 |          5 |            2 |
     | Gómez      | López      | Juan    |   10 |  250.45 | 2015-06-27 |          8 |            2 |
     | Herrera    | Gil        | Marta   | NULL |    NULL | NULL       |       NULL |         NULL |
     | Ruiz       | Flores     | Alfredo | NULL |    NULL | NULL       |       NULL |         NULL |
     | Sáez       | Vega       | Daniel  |    3 |   65.26 | 2017-10-05 |          2 |            1 |
     | Sáez       | Vega       | Daniel  |    6 |  2400.6 | 2016-07-27 |          7 |            1 |
     | Sáez       | Vega       | Daniel  |    7 |    5760 | 2015-09-10 |          2 |            1 |
     | Sáez       | Vega       | Daniel  |   12 |  3045.6 | 2017-04-25 |          2 |            1 |
     | Sáez       | Vega       | Daniel  |   13 |  545.75 | 2019-01-25 |          6 |            1 |
     | Sáez       | Vega       | Daniel  |   14 |  145.82 | 2017-02-02 |          6 |            1 |
     | Vega       | Hernández  | Antonio |   11 |   75.29 | 2016-08-17 |          3 |            7 |
     +------------+------------+---------+------+---------+------------+------------+--------------+
     ```
     
3. Devuelve un listado que solamente muestre los clientes que no han realizado ningún pedido.

     ```sql
     SELECT nombre
     FROM cliente
     LEFT JOIN pedido ON cliente.id = pedido.id_cliente
     WHERE pedido.id_cliente IS NULL;
     +-----------+
     | nombre    |
     +-----------+
     | Guillermo |
     | Daniel    |
     +-----------+
     ```
     
4. Devuelve un listado que solamente muestre los comerciales que no han realizado ningún pedido.

     ```sql
     SELECT nombre
     FROM comercial
     LEFT JOIN pedido ON comercial.id = pedido.id_comercial
     WHERE pedido.id_comercial IS NULL;
     +---------+
     | nombre  |
     +---------+
     | Marta   |
     | Alfredo |
     +---------+
     ```
     
5. Devuelve un listado con los clientes que no han realizado ningún pedido y de los comerciales que no han participado en ningún pedido. Ordene el listado alfabéticamente por los apellidos y el nombre. En en listado deberá diferenciar de algún modo los clientes y los comerciales.

     ```sql
     SELECT nombre
     FROM cliente
     LEFT JOIN pedido ON cliente.id = pedido.id_cliente
     WHERE pedido.id_cliente IS NULL
     UNION
     SELECT nombre
     FROM comercial
     LEFT JOIN pedido ON comercial.id = pedido.id_comercial
     WHERE pedido.id_comercial IS NULL;
     +-----------+
     | nombre    |
     +-----------+
     | Guillermo |
     | Daniel    |
     | Marta     |
     | Alfredo   |
     +-----------+
     ```
     
6. ¿Se podrían realizar las consultas anteriores con NATURAL LEFT JOIN o NATURAL RIGHT JOIN? Justifique su respuesta.

     ```sql
     No, porque estos tipos de join utilizan todas las columnas con el mismo nombre en ambas tablas para realizar la unión, y en las consultas anteriores se requiere unir las tablas usando un campo específico (id_cliente o id_comercial) que no tiene el mismo nombre en ambas tablas.
     ```

## **Consultas resumen**     
1. Calcula la cantidad total que suman todos los pedidos que aparecen en la tabla pedido.
   ```sql
   SELECT SUM(total)
   FROM pedido;
   +--------------------+
   | SUM(total)         |
   +--------------------+
   | 20992.829999999998 |
   +--------------------+
   ```
   
2. Calcula la cantidad media de todos los pedidos que aparecen en la tabla pedido.

     ```sql
     SELECT AVG(total)
     FROM pedido;
     +--------------------+
     | AVG(total)         |
     +--------------------+
     | 1312.0518749999999 |
     +--------------------+
     ```
     
3. Calcula el número total de comerciales distintos que aparecen en la tabla pedido.

     ```sql
     SELECT COUNT(DISTINCT id_comercial)
     FROM pedido;
     +------------------------------+
     | COUNT(DISTINCT id_comercial) |
     +------------------------------+
     |                            6 |
     +------------------------------+
     ```
     
4. Calcula el número total de clientes que aparecen en la tabla cliente.

     ```sql
     SELECT COUNT(id)
     FROM cliente;
     +-----------+
     | COUNT(id) |
     +-----------+
     |        10 |
     +-----------+
     ```
     
5. Calcula cuál es la mayor cantidad que aparece en la tabla pedido.

     ```sql
     SELECT MAX(total)
     FROM pedido;
     +------------+
     | MAX(total) |
     +------------+
     |       5760 |
     +------------+
     ```
     
6. Calcula cuál es la menor cantidad que aparece en la tabla pedido.

     ```sql
     SELECT MIN(total)
     FROM pedido;
     +------------+
     | MIN(total) |
     +------------+
     |      65.26 |
     +------------+
     ```
     
7. Calcula cuál es el valor máximo de categoría para cada una de las ciudades que aparece en la tabla cliente.

     ```sql
     SELECT ciudad, MAX(categoría)
     FROM cliente
     GROUP BY ciudad
     ORDER BY MAX(categoría) DESC;
     +----------+-----------------+
     | ciudad   | MAX(categoría)  |
     +----------+-----------------+
     | Sevilla  |             300 |
     | Jaén     |             300 |
     | Granada  |             225 |
     | Almería  |             200 |
     | Huelva   |             200 |
     | Cádiz    |             100 |
     +----------+-----------------+
     ```
     
8. Calcula cuál es el máximo valor de los pedidos realizados durante el mismo día para cada uno de los clientes. Es decir, el mismo cliente puede haber realizado varios pedidos de diferentes cantidades el mismo día. Se pide que se calcule cuál es el pedido de máximo valor para cada uno de los días en los que un cliente ha realizado un pedido. Muestra el identificador del cliente, nombre, apellidos, la fecha y el valor de la cantidad.

     ```sql
     SELECT cliente.id, cliente.nombre, cliente.apellido1, cliente.apellido2, fecha, MAX(total) AS maximo_pedidos
     FROM pedido
     JOIN cliente ON pedido.id_cliente = cliente.id
     GROUP BY cliente.id, cliente.nombre, cliente.apellido1, cliente.apellido2, fecha
     ORDER BY maximo_pedidos DESC;
     +----+---------+-----------+-----------+------------+----------------+
     | id | nombre  | apellido1 | apellido2 | fecha      | maximo_pedidos |
     +----+---------+-----------+-----------+------------+----------------+
     |  2 | Adela   | Salas     | Díaz      | 2015-09-10 |           5760 |
     |  2 | Adela   | Salas     | Díaz      | 2017-04-25 |         3045.6 |
     |  8 | Pepe    | Ruiz      | Santana   | 2016-10-10 |         2480.4 |
     |  7 | Pilar   | Ruiz      | NULL      | 2016-07-27 |         2400.6 |
     |  1 | Aarón   | Rivero    | Gómez     | 2019-03-11 |        2389.23 |
     |  4 | Adrián  | Suárez    | NULL      | 2017-10-10 |        1983.43 |
     |  5 | Marcos  | Loyola    | Méndez    | 2017-09-10 |          948.5 |
     |  6 | María   | Santana   | Moreno    | 2019-01-25 |         545.75 |
     |  1 | Aarón   | Rivero    | Gómez     | 2016-09-10 |         270.65 |
     |  8 | Pepe    | Ruiz      | Santana   | 2015-06-27 |         250.45 |
     |  5 | Marcos  | Loyola    | Méndez    | 2017-10-05 |          150.5 |
     |  6 | María   | Santana   | Moreno    | 2017-02-02 |         145.82 |
     |  8 | Pepe    | Ruiz      | Santana   | 2016-08-17 |          110.5 |
     |  3 | Adolfo  | Rubio     | Flores    | 2016-08-17 |          75.29 |
     |  2 | Adela   | Salas     | Díaz      | 2017-10-05 |          65.26 |
     +----+---------+-----------+-----------+------------+----------------+
     ```
     
9. Calcula cuál es el máximo valor de los pedidos realizados durante el mismo día para cada uno de los clientes, teniendo en cuenta que sólo queremos mostrar aquellos pedidos que superen la cantidad de 2000 €.

     ```sql
     SELECT cliente.id, cliente.nombre, cliente.apellido1, cliente.apellido2, fecha, MAX(total) AS maximo_pedidos
     FROM pedido
     JOIN cliente ON pedido.id_cliente = cliente.id
     WHERE total > 2000
     GROUP BY cliente.id, cliente.nombre, cliente.apellido1, cliente.apellido2, fecha
     ORDER BY maximo_pedidos DESC;
     +----+--------+-----------+-----------+------------+----------------+
     | id | nombre | apellido1 | apellido2 | fecha      | maximo_pedidos |
     +----+--------+-----------+-----------+------------+----------------+
     |  2 | Adela  | Salas     | Díaz      | 2015-09-10 |           5760 |
     |  2 | Adela  | Salas     | Díaz      | 2017-04-25 |         3045.6 |
     |  8 | Pepe   | Ruiz      | Santana   | 2016-10-10 |         2480.4 |
     |  7 | Pilar  | Ruiz      | NULL      | 2016-07-27 |         2400.6 |
     |  1 | Aarón  | Rivero    | Gómez     | 2019-03-11 |        2389.23 |
     +----+--------+-----------+-----------+------------+----------------+
     ```

10. Calcula el máximo valor de los pedidos realizados para cada uno de los comerciales durante la fecha 2016-08-17. Muestra el identificador del comercial, nombre, apellidos y total.

       ```sql
       SELECT comercial.id, nombre, apellido1, apellido2, MAX(total) AS maximo_pedidos
       FROM pedido
       JOIN comercial ON pedido.id_comercial = comercial.id
       WHERE fecha = '2016-08-17'
       GROUP BY comercial.id, nombre, apellido1, apellido2
       ORDER BY maximo_pedidos DESC;
       +----+---------+-----------+------------+----------------+
       | id | nombre  | apellido1 | apellido2  | maximo_pedidos |
       +----+---------+-----------+------------+----------------+
       |  3 | Diego   | Flores    | Salas      |          110.5 |
       |  7 | Antonio | Vega      | Hernández  |          75.29 |
       +----+---------+-----------+------------+----------------+
       ```

11. Devuelve un listado con el identificador de cliente, nombre y apellidos y el número total de pedidos que ha realizado cada uno de clientes. Tenga en cuenta que pueden existir clientes que no han realizado ningún pedido. Estos clientes también deben aparecer en el listado indicando que el número de pedidos realizados es 0.

         ```sql
         SELECT cliente.id, nombre, apellido1, apellido2, COUNT(total) AS total_pedidos
         FROM cliente
         LEFT JOIN pedido ON cliente.id = pedido.id_cliente
         GROUP BY cliente.id, nombre, apellido1, apellido2
         ORDER BY total_pedidos DESC;
         +----+-----------+-----------+-----------+---------------+
         | id | nombre    | apellido1 | apellido2 | total_pedidos |
         +----+-----------+-----------+-----------+---------------+
         |  1 | Aarón     | Rivero    | Gómez     |             3 |
         |  2 | Adela     | Salas     | Díaz      |             3 |
         |  8 | Pepe      | Ruiz      | Santana   |             3 |
         |  5 | Marcos    | Loyola    | Méndez    |             2 |
         |  6 | María     | Santana   | Moreno    |             2 |
         |  3 | Adolfo    | Rubio     | Flores    |             1 |
         |  4 | Adrián    | Suárez    | NULL      |             1 |
         |  7 | Pilar     | Ruiz      | NULL      |             1 |
         |  9 | Guillermo | López     | Gómez     |             0 |
         | 10 | Daniel    | Santana   | Loyola    |             0 |
         +----+-----------+-----------+-----------+---------------+
         ```

12. Devuelve un listado con el identificador de cliente, nombre y apellidos y el número total de pedidos que ha realizado cada uno de clientes durante el año 2017.

         ```sql
         SELECT cliente.id, nombre, apellido1, apellido2, COUNT(total) as total_pedidos
         FROM cliente
         LEFT JOIN pedido ON cliente.id = pedido.id_cliente
         WHERE EXTRACT(YEAR FROM fecha) = '2017'
         GROUP BY cliente.id, nombre, apellido1, apellido2
         ORDER BY total_pedidos DESC;
         +----+---------+-----------+-----------+---------------+
         | id | nombre  | apellido1 | apellido2 | total_pedidos |
         +----+---------+-----------+-----------+---------------+
         |  5 | Marcos  | Loyola    | Méndez    |             2 |
         |  2 | Adela   | Salas     | Díaz      |             2 |
         |  4 | Adrián  | Suárez    | NULL      |             1 |
         |  6 | María   | Santana   | Moreno    |             1 |
         +----+---------+-----------+-----------+---------------+
         ```

13. Devuelve un listado que muestre el identificador de cliente, nombre, primer apellido y el valor de la máxima cantidad del pedido realizado por cada uno de los clientes. El resultado debe mostrar aquellos clientes que no han realizado ningún pedido indicando que la máxima cantidad de sus pedidos realizados es 0. Puede hacer uso de la función [IFNULL](https://dev.mysql.com/doc/refman/8.0/en/control-flow-functions.html#function_ifnull).

         ```sql
         SELECT cliente.id, nombre, apellido1, apellido2, COALESCE(MAX(total), 0) AS maxima_cantidad
         FROM cliente
         LEFT JOIN pedido ON cliente.id = pedido.id_cliente
         GROUP BY cliente.id, nombre, apellido1, apellido2
         ORDER BY maxima_cantidad DESC;
         +----+-----------+-----------+-----------+-----------------+
         | id | nombre    | apellido1 | apellido2 | maxima_cantidad |
         +----+-----------+-----------+-----------+-----------------+
         |  2 | Adela     | Salas     | Díaz      |            5760 |
         |  8 | Pepe      | Ruiz      | Santana   |          2480.4 |
         |  7 | Pilar     | Ruiz      | NULL      |          2400.6 |
         |  1 | Aarón     | Rivero    | Gómez     |         2389.23 |
         |  4 | Adrián    | Suárez    | NULL      |         1983.43 |
         |  5 | Marcos    | Loyola    | Méndez    |           948.5 |
         |  6 | María     | Santana   | Moreno    |          545.75 |
         |  3 | Adolfo    | Rubio     | Flores    |           75.29 |
         |  9 | Guillermo | López     | Gómez     |               0 |
         | 10 | Daniel    | Santana   | Loyola    |               0 |
         +----+-----------+-----------+-----------+-----------------+
         ```

14. Devuelve cuál ha sido el pedido de máximo valor que se ha realizado cada año.

         ```sql
         SELECT EXTRACT(YEAR FROM fecha) AS año, MAX(total) AS max_total
         FROM pedido
         GROUP BY año
         ORDER BY año;
         +------+-----------+
         | año  | max_total |
         +------+-----------+
         | 2015 |      5760 |
         | 2016 |    2480.4 |
         | 2017 |    3045.6 |
         | 2019 |   2389.23 |
         +------+-----------+
         ```

15. Devuelve el número total de pedidos que se han realizado cada año.

     ```sql
     SELECT EXTRACT(YEAR FROM fecha) AS año, COUNT(total) AS total_pedidos
     FROM pedido
     GROUP BY año
     ORDER BY año;
     +------+---------------+
     | año  | total_pedidos |
     +------+---------------+
     | 2015 |             2 |
     | 2016 |             5 |
     | 2017 |             6 |
     | 2019 |             3 |
     +------+---------------+
     ```

## Con operadores básicos de comparación  
1. Devuelve un listado con todos los pedidos que ha realizado Adela Salas Díaz. (Sin utilizar INNER JOIN).
   ```sql
   SELECT *
   FROM pedido
   WHERE id_cliente = (
       SELECT id
       FROM cliente
       WHERE nombre = 'Adela' AND apellido1 = 'Salas' AND apellido2 = 'Díaz'
   );
   +----+--------+------------+------------+--------------+
   | id | total  | fecha      | id_cliente | id_comercial |
   +----+--------+------------+------------+--------------+
   |  3 |  65.26 | 2017-10-05 |          2 |            1 |
   |  7 |   5760 | 2015-09-10 |          2 |            1 |
   | 12 | 3045.6 | 2017-04-25 |          2 |            1 |
   +----+--------+------------+------------+--------------+
   ```
   
2. Devuelve el número de pedidos en los que ha participado el comercial Daniel Sáez Vega. (Sin utilizar INNER JOIN)
   ```sql
   SELECT COUNT(total) AS numero_pedidos
   FROM pedido
   WHERE id_comercial = (
   	SELECT id
   	FROM comercial
   	WHERE nombre = 'Daniel' AND apellido1 = 'Sáez' AND apellido2 = 'Vega'
   );
   +----------------+
   | numero_pedidos |
   +----------------+
   |              6 |
   +----------------+
   ```
   
3. Devuelve los datos del cliente que realizó el pedido más caro en el año 2019. (Sin utilizar INNER JOIN)
   ```sql
   SELECT *
   FROM cliente
   WHERE id = (
       SELECT id_cliente
       FROM pedido
       WHERE EXTRACT(year FROM fecha) = 2019
       ORDER BY total DESC
       LIMIT 1
   );
   +----+--------+-----------+-----------+----------+------------+
   | id | nombre | apellido1 | apellido2 | ciudad   | categoría  |
   +----+--------+-----------+-----------+----------+------------+
   |  1 | Aarón  | Rivero    | Gómez     | Almería  |        100 |
   +----+--------+-----------+-----------+----------+------------+
   ```
   
4. Devuelve la fecha y la cantidad del pedido de menor valor realizado por el cliente Pepe Ruiz Santana.
   ```sql
   SELECT fecha, MIN(total) AS pedido_menor_valor
   FROM pedido
   WHERE id_cliente = (
   	SELECT id
   	FROM cliente
   	WHERE nombre = 'Pepe' AND apellido1 = 'Ruiz' AND apellido2 = 'Santana'
   )
   GROUP BY fecha
   ORDER BY pedido_menor_valor;
   +------------+--------------------+
   | fecha      | pedido_menor_valor |
   +------------+--------------------+
   | 2016-08-17 |              110.5 |
   | 2015-06-27 |             250.45 |
   | 2016-10-10 |             2480.4 |
   +------------+--------------------+
   ```
   
5. Devuelve un listado con los datos de los clientes y los pedidos, de todos los clientes que han realizado un pedido durante el año 2017 con un valor mayor o igual al valor medio de los pedidos realizados durante ese mismo año.
   ```sql
   SELECT cliente.*, pedido.*
   FROM cliente, pedido
   WHERE cliente.id = pedido.id_cliente
   AND pedido.total >= (
       SELECT AVG(total)
       FROM pedido
       WHERE EXTRACT(year FROM fecha) = 2017
   );
   +----+---------+-----------+-----------+----------+------------+----+---------+------------+------------+--------------+
   | id | nombre  | apellido1 | apellido2 | ciudad   | categoría  | id | total   | fecha      | id_cliente | id_comercial |
   +----+---------+-----------+-----------+----------+------------+----+---------+------------+------------+--------------+
   |  7 | Pilar   | Ruiz      | NULL      | Sevilla  |        300 |  6 |  2400.6 | 2016-07-27 |          7 |            1 |
   |  2 | Adela   | Salas     | Díaz      | Granada  |        200 |  7 |    5760 | 2015-09-10 |          2 |            1 |
   |  4 | Adrián  | Suárez    | NULL      | Jaén     |        300 |  8 | 1983.43 | 2017-10-10 |          4 |            6 |
   |  8 | Pepe    | Ruiz      | Santana   | Huelva   |        200 |  9 |  2480.4 | 2016-10-10 |          8 |            3 |
   |  2 | Adela   | Salas     | Díaz      | Granada  |        200 | 12 |  3045.6 | 2017-04-25 |          2 |            1 |
   |  1 | Aarón   | Rivero    | Gómez     | Almería  |        100 | 16 | 2389.23 | 2019-03-11 |          1 |            5 |
   +----+---------+-----------+-----------+----------+------------+----+---------+------------+------------+--------------+
   ```

## Subconsultas con ALL y ANY
1. Devuelve el pedido más caro que existe en la tabla pedido si hacer uso de MAX, ORDER BY ni LIMIT.
   ```sql
   SELECT id, total
   FROM pedido 
   WHERE total >= ALL (
       SELECT total
       FROM pedido
   );
   +----+-------+
   | id | total |
   +----+-------+
   |  7 |  5760 |
   +----+-------+
   ```
   
2. Devuelve un listado de los clientes que no han realizado ningún pedido. (Utilizando ANY o ALL).
   ```sql
   SELECT nombre
   FROM cliente
   WHERE NOT id = ANY (SELECT id_cliente FROM pedido);
   +-----------+
   | nombre    |
   +-----------+
   | Guillermo |
   | Daniel    |
   +-----------+
   ```
   
3. Devuelve un listado de los comerciales que no han realizado ningún pedido. (Utilizando ANY o ALL).
   ```sql
   SELECT nombre
   FROM comercial
   WHERE NOT id = ANY (SELECT id_comercial FROM pedido);
   +---------+
   | nombre  |
   +---------+
   | Marta   |
   | Alfredo |
   +---------+
   ```

## Subconsultas con IN y NOT IN 
1. Devuelve un listado de los clientes que no han realizado ningún pedido. (Utilizando IN o NOT IN).
   ```sql
   SELECT nombre
   FROM cliente
   WHERE id NOT IN (SELECT id_cliente FROM pedido);
   +-----------+
   | nombre    |
   +-----------+
   | Guillermo |
   | Daniel    |
   +-----------+
   ```
   
2. Devuelve un listado de los comerciales que no han realizado ningún pedido. (Utilizando IN o NOT IN).
   ```sql
   SELECT nombre
   FROM comercial
   WHERE id NOT IN (SELECT id_comercial FROM pedido);
   +---------+
   | nombre  |
   +---------+
   | Marta   |
   | Alfredo |
   +---------+
   ```

## Subconsultas con EXISTS y NOT EXISTS  
1. Devuelve un listado de los clientes que no han realizado ningún pedido. (Utilizando EXISTS o NOT EXISTS).
   ```sql
   SELECT nombre
   FROM cliente
   WHERE NOT EXISTS (
       SELECT 1
       FROM pedido
       WHERE pedido.id_cliente = cliente.id
   );
   +-----------+
   | nombre    |
   +-----------+
   | Guillermo |
   | Daniel    |
   +-----------+
   ```
   
2. Devuelve un listado de los comerciales que no han realizado ningún pedido. (Utilizando EXISTS o NOT EXISTS).
   ```sql
   SELECT nombre
   FROM comercial
   WHERE NOT EXISTS (
       SELECT 1
       FROM pedido
       WHERE pedido.id_comercial = comercial.id
   );
   +---------+
   | nombre  |
   +---------+
   | Marta   |
   | Alfredo |
   +---------+
   ```



