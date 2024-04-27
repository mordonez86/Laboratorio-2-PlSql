### Práctica Unidad 1 : SQL
#### Primera Parte: Consultas
1.Consultar las tablas existentes en su cuenta, ver su estructura y contenido. (UsarVentana de Conexiones del SqlDeveloper).
```
listo
```
2.Mostrar las distintas funciones (jobs) que pueden cumplir los empleados.
```plsql
select distinct(JOB_ID) from employee
```
3.Desplegar el nombre completo de todos los empleados (Ej: Adam, Diane) ordenadospor apellido.
```plsql
select CONCAT(CONCAT(first_name,', '),last_name) from employee order by last_name;
```
4.Mostrar el nombre y el apellido de los empleados que ganan entre $1500 y $2850.
```plsql
select first_name , last_name from employee where salary between 1500 AND 2850
```
5.Mostrar el nombre y la fecha de ingreso de todos los empleados que ingresaron en elaño 2006.
```plsql
select first_name , hire_date from employee where extract (YEAR from hire_date) = 2006
```
6.Mostrar el id y nombre de todos los departamentos de la localidad 122.
```plsql
select department_id , name from department where location_id = 122
```
7.Modificar el ejercicio anterior para que la localidad pueda ser ingresada en elmomento de efectuar la consulta.
```plsql
select department_id , name from department where location_id = :ingrese_loc;
```
8.Mostrar el nombre y salario de los empleados que no tienen jefe.
```plsql
select first_name , salary from employee where manager_id is null
```
9.Mostrar el nombre de los empleados, su comisión y un cartel que diga  ̈Sincomisión ̈ para aquellos empleados que tienen su comisión en nulo.
```plsql
select  first_name , NVL(to_CHAR(commission),'sin_comision') COMMISION from employee
```
#### Joins
1.Mostrar el nombre completo de los empleados, el número de departamento y elnombre del departamento donde trabajan.
```plsql
select e.first_name||', '|| e.last_name ,e.department_id,d.name from employee e join department d on e.department_id = d.department_id
```
2.Mostrar el nombre y apellido, la función que ejercen, el nombre del departamento yel salario de todos los empleados ordenados por su apellido.
```plsql
select employee.first_name , employee.last_name, job.function, department.name ,employee.salary from employee 
join department on employee.department_id = department.department_id
join job on employee.job_id = job.job_id
```
3.Para todos los empleados que cobran comisión, mostrar su nombre, el nombre deldepartamento donde trabajan y el nombre de la región a la que pertenece eldepartamento.
```plsql
select employee.first_name , department.name , location.regional_group from employee
join department on employee.department_id = department.department_id
join location on department.location_id = location.location_id
where commission is not null and commission > 0
```
4.Para cada empleado mostrar su id, apellido, salario y grado de salario.
```plsql
SELECT e.employee_id AS id_empleado, 
       e.last_name AS apellido,
       e.salary AS salario,
       sg.grade_id AS grado_salario
FROM employee e
JOIN salary_grade sg ON e.salary BETWEEN sg.lower_bound AND sg.upper_bound;
```
5.Mostrar el número y nombre de cada empleado junto con el número de empleado ynombre de su jefe.
```plsql
select e.employee_id, e.manager_id, e.last_name ,e1.employee_id , e1.last_name from employee e 
join employee e1 on e.manager_id= e1.employee_id
```
6.Modificar el ejercicio anterior para mostrar también aquellos empleados que notienen jefe.
```plsql
select e.employee_id, e.manager_id, e.last_name ,e1.employee_id , e1.last_name from employee e 
left outer join employee e1 on e.manager_id= e1.employee_id

```
7.Mostrar las órdenes de venta, el nombre del cliente al que se vendió y la descripciónde los productos. Ordenar la consulta por nro. de orden.
```plsql
select sales_order.order_id,customer.name,product.description from sales_order 
join item on sales_order.order_id=item.order_id
join customer on sales_order.customer_id=customer.customer_id
join product on item.product_id=product.product_id
order by sales_order.order_id
```
8.Mostrar la cantidad de clientes.
```plsql
select count(*) from customer
```
9.Mostrar la cantidad de clientes del estado de Nueva York (NY).
```plsql
select count(*) from customer where city like 'N% Y%'
```
10.Mostrar la cantidad de empleados que son jefes. Nombrar a la columna JEFES.
```plsql
select count (*) from employee where employee_id in (select distinct(manager_id) from employee where manager_id is not null)
```
11.Mostrar toda la información del empleado más antiguo.
```plsql
select * from employee WHERE ROWNUM <= 1 order by hire_date asc
```
12.Generar un listado con el nombre completo de los empleados, el salario, y el nombrede su departamento para todos los empleados que tengan el mismo cargo que JohnSmith. Ordenar la salida por salario y apellido.
```plsql
select employee.first_name,employee.last_name ,employee.salary,department.name from employee
join department on employee.department_id=department.department_id
where employee.department_id = (select department_id from employee where first_name='JOHN' and last_name='SMITH')
order by employee.salary desc

```
13.Seleccionar los nombres completos, el nombre del departamento y el salario deaquellos empleados que ganan más que el promedio de salarios.
```plsql
select * from employee where salary > (select round(avg(salary),2) from employee where salary is not null and salary>0)
```
14.Mostrar los datos de las órdenes máxima y mínima
```plsql
SELECT *
FROM sales_order
WHERE order_id = (SELECT MAX(order_id) FROM sales_order)
   OR order_id = (SELECT MIN(order_id) FROM sales_order);
```
15.Mostrar la cantidad de órdenes agrupadas por cliente.
```plsql
select customer_id, count(*) from sales_order group by customer_id order by customer_id DESC
```
16.Modificar el ejercicio anterior para desplegar también el nombre y teléfono delcliente.
```plsql
select sales_order.customer_id, customer.NAME, customer.PHONE_NUMBER, count(*) as total from sales_order

join customer on sales_order.CUSTOMER_ID=customer.CUSTOMER_ID

group by sales_order.CUSTOMER_ID,customer.NAME,customer.PHONE_NUMBER order by total desc
```
17.Mostrar aquellos empleados que tienen dos ó más personas a su cargo.
```plsql
select manager_id, count(*) from employee
where manager_id is not NULL
group by manager_id 
HAVING count(*) >= 2
```
18.Desplegar el nombre del empleado más antiguo y del empleado más nuevo, (segúnsu fecha de ingreso).
```plsql

```
19.Mostrar la cantidad de empleados que tiene los departamentos 20 y 30.
```plsql
select department_id,count(*) from employee 
 where department_id=20 or department_id=30
  group by department_id
```
20.Mostrar el promedio de salarios de los empleados de los departamentos deinvestigación (Research). Redondear el promedio a dos decimales.
```plsql
select round(AVG(salary),2) from employee where department_id in (select department_id from department where name = 'RESEARCH')
```
21.Por cada departamento desplegar su id, su nombre y el promedio de salarios (sindecimales) de sus empleados. El resultado ordenarlo por promedio.
```plsql

```
22.Modificar el ejercicio anterior para mostrar solamente los departamentos que tienenmás de 3 empleados.
23.Por cada producto (incluir todos los productos) mostrar la cantidad de unidades quese han pedido y el precio máximo que se ha facturado.
24.Para cada cliente mostrar nombre, teléfono, la cantidad de órdenes emitidas y lafecha de su última orden. Ordenar el resultado por nombre de cliente.
25.Para todas las localidades mostrar sus datos, la cantidad de empleados que tiene y eltotal de salarios de sus empleados. Ordenar por cantidad de empleados.
26.Mostrar los empleados que ganan más que su jefe. El reporte debe mostrar elnombre completo del empleado, su salario, el nombre del departamento al quepertenece y la función que ejerce.


#### Segunda parte - Manipulación y Definición de datos

1.Insertar un par de filas en la tabla JOB.
2.Hacer COMMIT.
3.Eliminar las filas insertadas en la tabla JOB.
4.Hacer ROLLBACK.
5.Seleccionar todas las filas de la tabla JOB.
6.Modificar el nombre de un cliente.
7.Crear un SAVEPOINT A.
8.Modificar el nombre de otro cliente.
9.Crear un SAVEPOINT B.
10.Hacer un ROLLBACK hasta el último SAVEPOINT creado.
11.Hacer un SELECT de toda la tabla CUSTOMER.
12.Si quiero que la primera modificación del nombre de un cliente que hice quedeasentada definitivamente en la base, debo hacer algo?.
13.Eliminar el departamento 10. Se puede? Por que?
14.Insertar el departamento 50, ‘EDUCATION’ en la localidad 100. Se puede?
15.Insertar el departamento 43, ‘OPERATIONS’ sin indicar la localidad. Se puede?
16.Modificar la localidad del departmento 20, para que pertenezca a la localidad 155.Se puede?
17.Incrementar en un 10% el salario a todos los empleados que ganan menos que elpromedio de salarios.
18.A todos los clientes que han generado más de 5 órdenes, incrementar su límite decrédito en un 5%.
19.Deshacer todos estos cambios.
20.Crear una tabla EMP2 con 4 columnas: id number(3), nombre varchar(10), salarionumber( no puede ser nulo) y depto number(2). Definir id como clave primaria,nombre debe ser único y depto debe referenciar a la tabla de Department