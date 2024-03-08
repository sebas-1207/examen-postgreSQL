# **Consultas PostgreSQL EXAMEN** 

## **Consultas sobre una tabla**
1. Devuelve un listado con todos los pedidos que se han realizado. Los pedidos deben estar ordenados por la fecha de realización, mostrando en primer lugar los pedidos más recientes.

   ```sql
   SELECT * 
   FROM pedido 
   ORDER BY fecha DESC;
   ```
   
2. Devuelve todos los datos de los dos pedidos de mayor valor.

     ```sql
     SELECT * 
     FROM pedido 
     ORDER BY total 
     DESC LIMIT 2;
     ```
     
3. Devuelve un listado con los identificadores de los clientes que han realizado algún pedido. Tenga en cuenta que no debe mostrar identificadores que estén repetidos.

     ```sql
     SELECT DISTINCT cliente.id
     FROM cliente
     JOIN pedido ON cliente.id = pedido.id_cliente;
     ```
     
4. Devuelve un listado de todos los pedidos que se realizaron durante el año 2017, cuya cantidad total sea superior a 500€.

     ```sql
     SELECT * 
     FROM pedido
     WHERE EXTRACT(YEAR FROM fecha) = 2017 AND total > 500;
     ```
     
5. Devuelve un listado con el nombre y los apellidos de los comerciales que tienen una comisión entre 0.05 y 0.11.

     ```sql
     SELECT nombre, apellido1, apellido2
     FROM comercial
     WHERE comisión BETWEEN 0.05 AND 0.11;
     ```
     
6. Devuelve el valor de la comisión de mayor valor que existe en la tabla comercial.

     ```sql
     SELECT MAX(comisión)
     FROM comercial;
     ```

7. Devuelve el identificador, nombre y primer apellido de aquellos clientes cuyo segundo apellido no es NULL. El listado deberá estar ordenado alfabéticamente por apellidos y nombre.

     ```sql
     SELECT id, nombre, apellido1
     FROM cliente
     WHERE apellido2 IS NOT NULL
     ORDER BY apellido1, nombre;
     ```

8. Devuelve un listado de los nombres de los clientes que empiezan por A y terminan por n y también los nombres que empiezan por P. El listado deberá estar ordenado alfabéticamente.

     ```sql
     SELECT nombre
     FROM cliente
     WHERE (nombre LIKE 'A%n' OR nombre LIKE 'P%')
     ORDER BY nombre;
     ```

9. Devuelve un listado de los nombres de los clientes que no empiezan por A. El listado deberá estar ordenado alfabéticamente.

     ```sql
     SELECT nombre
     FROM cliente
     WHERE nombre NOT LIKE 'A%'
     ORDER BY nombre;
     ```

10. Devuelve un listado con los nombres de los comerciales que terminan por "el" o "o". Tenga en cuenta que se deberán eliminar los nombres repetidos.

     ```sql
     SELECT DISTINCT nombre 
     FROM comercial
     WHERE nombre LIKE '%el' OR nombre LIKE '%o';
     ```

## **Consultas multitabla (Composición interna)**
1. Devuelve un listado con el identificador, nombre y los apellidos de todos los clientes que han realizado algún pedido. El listado debe estar ordenado alfabéticamente y se deben eliminar los elementos repetidos.
   ```sql
   SELECT DISTINCT cliente.id, nombre, apellido1, apellido2 
   FROM cliente
   JOIN pedido ON cliente.id = pedido.id_cliente
   ORDER BY nombre, apellido1, apellido2;
   ```
   
2. Devuelve un listado que muestre todos los pedidos que ha realizado cada cliente. El resultado debe mostrar todos los datos de los pedidos y del cliente. El listado debe mostrar los datos de los clientes ordenados alfabéticamente.

     ```sql
     SELECT pedido.*, cliente.*
     FROM pedido
     JOIN cliente ON pedido.id_cliente = cliente.id
     ORDER BY cliente.*;
     ```
     
3. Devuelve un listado que muestre todos los pedidos en los que ha participado un comercial. El resultado debe mostrar todos los datos de los pedidos y de los comerciales. El listado debe mostrar los datos de los comerciales ordenados alfabéticamente.

     ```sql
     SELECT pedido.*, comercial.*
     FROM pedido
     JOIN comercial ON pedido.id_comercial = comercial.id
     ORDER BY comercial.*;
     ```
     
4. Devuelve un listado que muestre todos los clientes, con todos los pedidos que han realizado y con los datos de los comerciales asociados a cada pedido.

     ```sql
     SELECT cliente.id, cliente.nombre, pedido.id, pedido.total, pedido.fecha, comercial.id, comercial.nombre, comercial.apellido1, comercial.apellido2
     FROM cliente, pedido, comercial
     WHERE cliente.id = pedido.id_cliente AND pedido.id_comercial = comercial.id
     ORDER BY cliente.id, pedido.id;
     ```
     
5. Devuelve un listado de todos los clientes que realizaron un pedido durante el año 2017, cuya cantidad esté entre 300 € y 1000 €.

     ```sql
     SELECT nombre
     FROM cliente
     JOIN pedido ON cliente.id = pedido.id_cliente
     WHERE EXTRACT(YEAR FROM fecha) = 2017 AND total BETWEEN 300 AND 1000;
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
     ```

## **Consultas multitabla (Composición externa)**     
1. Devuelve un listado con todos los clientes junto con los datos de los pedidos que han realizado. Este listado también debe incluir los clientes que no han realizado ningún pedido. El listado debe estar ordenado alfabéticamente por el primer apellido, segundo apellido y nombre de los clientes.
   ```sql
   
   ```
   
2. Devuelve un listado con todos los comerciales junto con los datos de los pedidos que han realizado. Este listado también debe incluir los comerciales que no han realizado ningún pedido. El listado debe estar ordenado alfabéticamente por el primer apellido, segundo apellido y nombre de los comerciales.

     ```sql
     
     ```
     
3. Devuelve un listado que solamente muestre los clientes que no han realizado ningún pedido.

     ```sql
     
     ```
     
4. Devuelve un listado que solamente muestre los comerciales que no han realizado ningún pedido.

     ```sql
     
     ```
     
5. Devuelve un listado con los clientes que no han realizado ningún pedido y de los comerciales que no han participado en ningún pedido. Ordene el listado alfabéticamente por los apellidos y el nombre. En en listado deberá diferenciar de algún modo los clientes y los comerciales.

     ```sql
     
     ```
     
6. ¿Se podrían realizar las consultas anteriores con NATURAL LEFT JOIN o NATURAL RIGHT JOIN? Justifique su respuesta.

     ```sql
     
     ```

## **Consultas resumen**     
1. Devuelve el número total de alumnas que hay.
   ```sql
   SELECT COUNT(*) AS total_alumnas
   FROM persona
   WHERE tipo = 'alumno' AND sexo = 'M';
   ```
   
2. Calcula cuántos alumnos nacieron en 2005.

     ```sql
     SELECT COUNT(*) 
     FROM persona
     WHERE tipo = 'alumno' AND EXTRACT(YEAR FROM fecha_nacimiento) = 2005;
     ```

3. Calcula cuántos profesores hay en cada departamento. El resultado sólo debe mostrar dos columnas, una con el nombre del departamento y otra con el número de profesores que hay en ese departamento. El resultado sólo debe incluir los departamentos que tienen profesores asociados y deberá estar ordenado de mayor a menor por el número de profesores.

     ```sql
     SELECT departamento.nombre, COUNT(profesor.id)
     FROM departamento
     JOIN profesor ON departamento.id = profesor.id_departamento
     GROUP BY departamento.nombre
     ORDER BY COUNT(profesor.id) DESC;
     
     ```

4. Devuelve un listado con todos los departamentos y el número de profesores que hay en cada uno de ellos. Tenga en cuenta que pueden existir departamentos que no tienen profesores asociados. Estos departamentos también tienen que aparecer en el listado.

     ```sql
     SELECT departamento.nombre, COUNT(profesor.id_departamento)
     FROM departamento
     LEFT JOIN profesor ON departamento.id = profesor.id_departamento
     GROUP BY departamento.nombre;
     ```

5. Devuelve un listado con el nombre de todos los grados existentes en la base de datos y el número de asignaturas que tiene cada uno. Tenga en cuenta que pueden existir grados que no tienen asignaturas asociadas. Estos grados también tienen que aparecer en el listado. El resultado deberá estar ordenado de mayor a menor por el número de asignaturas.

     ```sql
     SELECT grado.nombre, COUNT(asignatura.id_grado) AS num_asignaturas
     FROM grado
     LEFT JOIN asignatura ON grado.id = asignatura.id_grado
     GROUP BY grado.nombre
     ORDER BY num_asignaturas DESC;
     ```

6. Devuelve un listado con el nombre de todos los grados existentes en la base de datos y el número de asignaturas que tiene cada uno, de los grados que tengan más de 40 asignaturas asociadas.

     ```sql
     SELECT grado.nombre, COUNT(asignatura.id_grado) AS num_asignaturas
     FROM grado
     JOIN asignatura ON grado.id = asignatura.id_grado
     GROUP BY grado.nombre
     HAVING COUNT(asignatura.id_grado) > 40;
     ```

7. Devuelve un listado que muestre el nombre de los grados y la suma del número total de créditos que hay para cada tipo de asignatura. El resultado debe tener tres columnas: nombre del grado, tipo de asignatura y la suma de los créditos de todas las asignaturas que hay de ese tipo. Ordene el resultado de mayor a menor por el número total de crédidos.

     ```sql
     SELECT grado.nombre, asignatura.tipo, SUM(asignatura.creditos)
     FROM grado
     JOIN asignatura ON grado.id = asignatura.id_grado
     GROUP BY grado.nombre, asignatura.tipo
     ORDER BY SUM(asignatura.creditos) DESC;
     ```

8. Devuelve un listado que muestre cuántos alumnos se han matriculado de alguna asignatura en cada uno de los cursos escolares. El resultado deberá mostrar dos columnas, una columna con el año de inicio del curso escolar y otra con el número de alumnos matriculados.

     ```sql
     SELECT curso_escolar.anyo_inicio, COUNT(persona.id)
     FROM persona
     JOIN alumno_se_matricula_asignatura ON persona.id = alumno_se_matricula_asignatura.id_alumno
     LEFT JOIN asignatura ON alumno_se_matricula_asignatura.id_asignatura = asignatura.id
     JOIN curso_escolar ON alumno_se_matricula_asignatura.id_curso_escolar = curso_escolar.id
     WHERE persona.tipo = 'alumno'
     GROUP BY curso_escolar.anyo_inicio;
     ```

9. Devuelve un listado con el número de asignaturas que imparte cada profesor. El listado debe tener en cuenta aquellos profesores que no imparten ninguna asignatura. El resultado mostrará cinco columnas: id, nombre, primer apellido, segundo apellido y número de asignaturas. El resultado estará ordenado de mayor a menor por el número de asignaturas.

     ```sql
     SELECT persona.id, persona.nombre, persona.apellido1, persona.apellido2, COUNT(asignatura.id) AS numero_asignaturas
     FROM profesor
     LEFT JOIN asignatura ON profesor.id = asignatura.id_profesor
     JOIN persona ON profesor.id_profesor = persona.id
     WHERE persona.tipo = 'profesor'
     GROUP BY persona.id, persona.nombre, persona.apellido1, persona.apellido2
     ORDER BY numero_asignaturas DESC;
     ```

## **Subconsultas**     
1. Devuelve todos los datos del alumno más joven.
   ```sql
   SELECT *
   FROM persona
   WHERE fecha_nacimiento = (SELECT MIN(fecha_nacimiento) FROM persona WHERE tipo = 'alumno');
   ```
   
2. Devuelve un listado con los profesores que no están asociados a un departamento.
   ```sql
   SELECT persona.id, persona.nombre, persona.apellido1, persona.apellido2
   FROM persona
   WHERE persona.tipo = 'profesor'
   AND persona.id NOT IN (SELECT id_profesor FROM profesor WHERE id_departamento IS NOT NULL);
   ```
   
3. Devuelve un listado con los departamentos que no tienen profesores asociados.
   ```sql
   SELECT departamento.id, departamento.nombre
   FROM departamento
   WHERE departamento.id NOT IN (SELECT id_departamento FROM profesor);
   ```
   
4. Devuelve un listado con los profesores que tienen un departamento asociado y que no imparten ninguna asignatura.
   ```sql
   SELECT persona.id, persona.nombre, persona.apellido1, persona.apellido2
   FROM persona
   JOIN profesor ON persona.id = profesor.id_profesor
   WHERE profesor.id_departamento IS NOT NULL
   AND persona.id NOT IN (SELECT id_profesor FROM asignatura);
   ```
   
5. Devuelve un listado con las asignaturas que no tienen un profesor asignado.
   ```sql
   SELECT asignatura.nombre
   FROM asignatura
   WHERE asignatura.id_profesor IS NULL;
   ```
   
6. Devuelve un listado con todos los departamentos que no han impartido asignaturas en ningún curso escolar.
   ```sql
   SELECT DISTINCT departamento.nombre
   FROM departamento 
   LEFT JOIN profesor  ON departamento.id = profesor.id_departamento
   LEFT JOIN asignatura ON asignatura.id_profesor = profesor.id_profesor
   LEFT JOIN alumno_se_matricula_asignatura ON alumno_se_matricula_asignatura.id_asignatura = asignatura.id
   WHERE alumno_se_matricula_asignatura.id_curso_escolar IS NULL AND asignatura.curso IS NULL;
   ```