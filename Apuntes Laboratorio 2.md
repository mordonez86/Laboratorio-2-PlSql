## Apuntes Clase 2

---

#### Funciones Importantes

```plsql
extract(YEAR from hire_date)=2006; --extrae el año
```

```plsql
NVL(nombre_campo,0) &rarr; devuelve 0 si el valor es NULL -- tener en cuenta que los dos argumentos deben ser el mismo tipo de dato

decode() -- aca podemos mejorar lo anterior para no castear toda la columna entera y puede ir asignando valores tipo case-when

SYSDATE -- accedemos al horario de sistema formato dd-mm-YYYY

to_char -- casteamos a char
```



#### Constraints

##### En apex si ingresamos a constraints vamos  a poder acceder a todas las condiciones entre tablas, sea foreign keys , primary keys , tipos de datos, not null , etc etc

* Es decir que si por ejemplo queremos insertar un dato en una tabla A que tiene una constraint en un campo foreign key a otra tabla B y queremos insertar un dato en la primer tabla que no existe en la segunda tabla nos va a dar error.

* En el caso de querer eliminar un dato , puede pasar algo similar , si queremos borrar un dato que tiene hijos(es decir esta relacionado con otras filas de otra tabla ) nos va a dar error porque nos va avisar que estamos queriendo borrar un dato que es PADRE de otros en otra tabla
* En el caso de actualizar un dato tambien nos puede suceder que nos de un error de constraint si el nuevo dato que queremos insertar no existe en la tabla padre o hijo

##### PL / SQL

Es la version procedural de SQL , vemos su implementacion en oracle en esta clase , permite mediante cursores hacer cambios en la base y ver el estado de las operaciones

###### Estructura de consultas/declaraciones

Se utilizan BLOQUES DE CODIGO , los mismos pueden ser anonimos (cuando la consulta se escribe directamente en la ventana de SQL)o procedimientos

```  plsql
DECLARE
/*declaracion de variables*/
/*Esta parte puede no tenerla*/
BEGIN 
/*ejecucion*/
EXCEPTION
/*area de excepciones*/
/*Esta parte puede no tenerla*/
END;

```

##### Zona de declaracion (DECLARE)

Es donde se declaran todas las variables que se van a utilizar dentro del bloque y se pueden almacenar:

* Variables
* Constantes
* Cursores

Las variables se declaran directamente:

``` plsql
NOMBRE_VARIABLE [CONSTANT] TIPO DE DATO [NOT NULL]
-- si quiero que sea una constante debo agregar el CONSTANT 
-- si quiero agregar una constraint de not null , debo agregarlo como en el ejemplo
-- si quiero asignarle un valor por defecto se hace utilizando ":=" este es el operador de asignacion
-- el tipo de dato puede tomarse desde la columna usando %type o desde toda la tabla con %rowtype, de esta forma nos ahorramos mirar manualmente

--EJ
DECLARE
mivariable number(6,2);-- longitud 6 con dos decimales
mivariable_texto varchar(5);-- longitud 5
miconstante CONSTANT varchar(5) :='hola'; -- declaracion de constante
mivariable_bool boolean NOT NULL := TRUE; -- declaracion de booleana
mivariable_desdecolumna %type employee.name; --aca declaramos la variable y referenciamos al campo name de la tabla employee
mivariable_desdetabla EMPLOYEE%ROWTYPE; -- aca declaramos la variable usando la estructura de la tabla
BEGIN
END

```

##### Zona de EJECUCION (BEGIN)

Dentro del bloque de ejecucion es donde nosotros vamos a desarrollar lo que necesita ejecutar la aplicacion

```plsql
-- Sentencia Select
-- En PL SQL tenemos que tener en cuenta que solo nos puede devolver UNA SOLA FILA para esto se usa el atributo INTO que es la palabra reservada para asginarle a una variable el valor que retorne el SELECT , NO PUEDE DEVOLVER VARIAS NI NINGUNA porque va a dar una excepcion

DECLARE
v_employee_id  Employee.EMPLOYEE_ID%type;
v_employee_name  Employee.first_name%type;
v_employee_lastname  Employee.last_name%type;
BEGIN
SELECT EMPLOYEE_ID,FIRST_NAME,LAST_NAME INTO
 v_employee_id,v_employee_name,v_employee_lastname
 FROM EMPLOYEE
  WHERE EMPLOYEE_ID =7369;
END
```

Si bien compila y no da error , no nos mostro valores ,oracle tiene un paquete que nos permite imprimir el valor de las variables llamado DBMS_OUTPUT y el metodo es PUT_LINE()

```plsql
DECLARE
v_employee_id  Employee.EMPLOYEE_ID%type;
v_employee_name  Employee.first_name%type;
v_employee_lastname  Employee.last_name%type;
BEGIN
SELECT EMPLOYEE_ID,FIRST_NAME,LAST_NAME INTO
 v_employee_id,v_employee_name,v_employee_lastname
 FROM EMPLOYEE
  WHERE EMPLOYEE_ID =7369;
 DBMS_OUTPUT.PUT_LINE('el valor del id es: '||v_employee_id||' y el nombre es: '||v_employee_name);
 -- el || es el operador de concatenacion
END
```

Ahora vamos a ver un ejemplo usando %rowtype y una variable de fila entera

```plsql
DECLARE
v_tabla EMPLOYEE%rowtype; -- esta variable contiene todos los campos de la fila
BEGIN
SELECT * INTO v_tabla
FROM EMPLOYEE
WHERE EMPLOYEE_ID=7369;
-- accedemos al valor de cada campo usando el . y el nombre de la columna
DBMS_OUTPUT.PUT_LINE('el valor del id es: '||v_tabla.employee_id||' y el nombre es: '||v_tabla.first_name);
END

```

##### Commits

En el bloque de ejecucion es donde tambien vamos a realizar commits sobre la base de datos

```plsql
--EJEMPLO CON INSERT
DECLARE
BEGIN
INSERT INTO LOCATION (LOCATION_ID,REGIONAL_GROUP) 
VALUES (22,'Nueva Locacion');
COMMIT;
END
```

##### Solicitar datos al usuario mediante un prompt (variables de sustitucion)

Variables de sustitucion se utilizan para 'testear' el funcionamiento, ya que estamos trabajando directamente sobre la base y no tenemos ninguna otra herramienta para simular el uso de un input desde una app

Para solicitar datos al usuario usamos el : (dos puntos)

```plsql
DECLARE
v_nombre EMPLOYEE.first_name%type;
v_employee_id EMPLOYEE.employee_id%type;
BEGIN
SELECT first_name INTO
v_nombre FROM EMPLOYEE
WHERE employee_id= :v_employee_id; -- aca usamos el : para pedir al usuario el dato del where
DBMS_OUTPUT.PUT_LINE('El nombre del empleado es: '||v_nombre ||' y su id es: '||v_employee_id);
END
```

##### Estructuras de control

Tambien se pueden utilizar if , if else , ciclos (for y while) para controlar el funcionamiento de nuestro bloque

```plsql
-- estructura de if IF -> THEN -> ELSE -> END IF;
-- IF -> ELSIF -> ELSIF -> ELSE -> END IF;
DECLARE
v_employee EMPLOYEE%rowtype;
BEGIN
SELECT * INTO v_employee
	FROM EMPLOYEE
		WHERE employee_id=7369;

IF v_employee.manager_id=7839
	THEN DBMS_OUTPUT.PUT_LINE('es del manager');
ELSE
	DBMS_OUTPUT.PUT_LINE('no es del manager');
END IF;
END;
```

##### Ciclos

Existen 3 tipos de ciclos

* loop basico (ejecuta  con condicion , pero siempre entra x lo menos 1 vez)
* for loop (ejecuta n cantidad de veces)
* while loop (ejecuta pero primero busca la condicion sino sale)

```plsql
--LOOP basico
loop
....
exit when <condicion>;

-- FOR LOOP
for i in 1..10 loop
	....
end loop

-- while
WHILE <condicion> loop
....
end loop;
```



```plsql
-- Ejemplo funcional FOR LOOP

begin
for i in  1..10 loop
    DBMS_OUTPUT.PUT_LINE(i);
   END LOOP;
end

-- ejemplo funcional LOOP

declare
v_contador number(2,0) :=0;
begin
loop
DBMS_OUTPUT.PUT_LINE(v_contador);
v_contador:=v_contador+1;
exit when v_contador >=10;
end loop;
end;

-- ejemplo funcional while

declare
    v_contador number(2,0) :=0;
begin
while v_contador<=10 loop
    DBMS_OUTPUT.PUT_LINE(v_contador);
    v_contador:=v_contador+1;
end loop;
end;
```

#### CURSORES 

pueden ser explicitos o implicitos los implicitos no tienen nombre , se llaman sql que se lo pone el motor  y tienen atributos a los cuales accedemos desde el %

```plsql
-- ESTAMOS USANDO UN CURSOR IMPLICITO , PODEMOS ACCEDER AL CURSOR USANDO sql% ,ej: sql%rowcount o sql_notfound o sql%found ..etc
declare
	v_user_input customer.credit%type;
	v_user_id customer.customer_id%type;
begin
	v_user_input := :ingrese_limite;
	v_user_id := :ingrese_id;
UPDATE customer
set customer.credit=v_user_input
where customer.customer_id=v_user_id;
IF sql%rowcount > 0 THEN -- desde aqui manejamos el usuario no encontrado , ya que no da error si no aparece
	DBMS_OUTPUT.PUT_LINE('se actualizo');
ELSE
	DBMS_OUTPUT.PUT_LINE('no se encontro el cliente');
END IF;	
end;
```

En el caso del cursor explicito tengo que:

* declarar **declaro la variable tipo cursor**
* abrir **asigno los datos a la estructura**
* recorrer **recorro con un ciclo el resultado de determinada consulta**
* cerrar **cierro el cursor**

```plsql
--recorriendo con un loop
declare
    cursor c_emple is -- declaro el cursor
    select first_name , last_name , salary from employee where salary >600;
    r_emple c_emple%rowtype; -- aca declaro una estructura para guardar el dato desde el cursor
begin
    open c_emple;
	loop 
		fetch c_emple into r_emple;
		exit when c_emple%notfound;
			dbms_ouput.put_line(r_emple.first_name);
	end loop;
    close c_emple;
end

-- recorriendo con un for (mas facil)

declare
    cursor c_emple is -- declaro el cursor
    select first_name , last_name , salary from employee where salary >600;
    r_emple c_emple%rowtype; -- aca declaro una estructura para guardar el dato desde el cursor
begin
    for r_emple in c_emple loop
    	dbms_ouput.put_line(r_emple.first_name);
    end loop;
end
```

##### Cursores con parametros

Podemos indicarle al usuario que nos de un parametro para usar el cursor

```plsql
declare
    cursor c_depto(p_loc department.location_id%type) is -- aca declaro el cursor con parametros
    select * from department where location_id = p_loc
    v_loc department.department_id
begin
	v_loc:= :ingrese_depto
    for r_depto in c_depto(v_loc) loop --aca tomo el parametro
    	dbms_ouput.put_line(r_depto.name);
    end loop;
end
```



##### Zona Excepciones (EXCEPTION)

Hay dos tipos de excepciones

* del usuario van del -20000 hasta abajo (siempre negativos) ej: reg;as del negocio (clientes no pueden tener + de 10000 de linea de credito)
* del motor van desde -19999 hasta -1 (siempre negativos) ej no_data_found , too_many_rows , value_error , dup_val_on_index , zero_divide , integrity_constraint

La zona de excepciones es opcional , podemos dejar que el motor se encargue de forma basica o como es recomendable , manejarlas de forma personalizada. Ahora vamos a ver una forma bastante basica de manejar las excepciones y es utilizando WHEN-THEN

##### Ejemplo de excepcion del motor

```plsql
DECLARE
v_tabla EMPLOYEE%rowtype;
BEGIN
SELECT * INTO v_tabla
FROM EMPLOYEE
WHERE EMPLOYEE_ID=736;
DBMS_OUTPUT.PUT_LINE('el valor del id es: '||v_tabla.employee_id||' y el nombre es: '||v_tabla.first_name);
EXCEPTION
-- comportamiento para no_data_found
WHEN NO_DATA_FOUND THEN DBMS_OUTPUT.PUT_LINE('No se encontro el registro');
-- comportamiento para too_many_rows
WHEN TOO_MANY_ROWS THEN DBMS_OUTPUT.PUT_LINE('Devolvio mas de un registro');
-- comportamiento para todas las demas excepciones
WHEN OTHERS THEN DBMS_OUTPUT.PUT_LINE('Error');
END
```

> Es importante notar que si escribimos especificamente la excepcion y usamos el OTHERS, ya no va a hacer uso de las excepciones de sistema para las que no estan declaradas sino siempre van a entrar por el others, es decir que si no especifico el comportamiento para too_many_rows y me devuelve esa excepcion, entonces va a salir por OTHERS sin usar la que viene por defecto en sistema.

##### Excepciones Personalizadas a las existentes del motor.

Se debe primero declarar una variable del tipo excepcion  y la palabra reservada pragma:

```plsql
declare
e_regla_de_negocio exception;
pragma exception_init(e_regla_de_negocio,-2291); -- asociamos el nro -2291 al nombre de la excepcion q queremos.
begin
end
```

```plsql
--EJ
declare
	v_user_input customer.credit%type;
	v_user_id customer.customer_id%type;
	e_excepcion_de_usuario exception; -- declaramos la variable tipo excepcion
	pragma exception_init(e_excepcion_de_usuario,-2291) -- es el nro de no data found
begin
	v_user_input := :ingrese_limite;
	v_user_id := :ingrese_id;
UPDATE customer
set customer.credit=v_user_input
where customer.customer_id=v_user_id;
exception
when e_excepcion_de_usuario then
	DBMS_OUTPUT.PUT_LINE('El usuario no existe'); -- capturamos la excepcion
end;
```

### Procedimientos

Los procedimientos a diferencia de los bloques anonimos , tienen un nombre que es unico e irrepetible y se pueden ejecutar la cantidad de veces que sea necesario ya que se almacenan en la base

* Tienen un nombre para invocarlos
* No se pueden usar con variables de sustitucion , sino que reciben **PARAMETROS**

```plsql
-- Para crear los procedimientos en apex deberiamos ir a project objects y buscar procedures
-- automaticamente nos completa una estructura similar a esta
create or replace procedure "nombre"
as -- desde create hasta AS es el reemplazo del declare en el bloque anonimo
begin
null;
end "nombre";
```

```plsql
--ejemplo
create or replace procedure calculo_sueldo
as
v_salario number;
begin
select AVG(salary) into v_salario
where department_id=10;
dbms_output.put_line('el promedio es: '||v_salario)
end calculo_sueldo;

--ahora presionamos en grabar y compilar y se debe llamar desde un bloque anonimo.

begin
calculo_sueldo
end;
```

##### Procedimientos con parametros

Los parametros pueden ser de

* Entrada
* Salida
* Entrada-Salida

```plsql
--ejemplo
create or replace procedure calculo_sueldo (pi_department_id IN employee.department_id%type) -- aca recibo parametros IN , OUT o IO el in se puede omitir y ser por defecto
as
v_salario number;
begin
select AVG(salary) into v_salario
from employee
where department_id=pi_department_id;
dbms_output.put_line('el promedio es: '||v_salario)
end calculo_sueldo;

--ahora presionamos en grabar y compilar y se debe llamar desde un bloque anonimo.

begin
calculo_sueldo(10)
end;
```

```plsql
-- procedimiento mas largo

create or replace procedure pr_alta_dep (pi_dep_id department.department_id%type,
                                         pi_nombre department.name%type,
                                         pi_loc department.location_id%type default 122)-- valor por defecto
as
--declaracion de variables
e_fk exception;
pragma exception_init(e_fk,-2291);
begin
insert into department (department_id,name,location_id)
values(pi_dep_id,pi_nombre,pi_loc);
dbms_output.put_line('se inserto correctamente');
exception
when no_data_found 
	then dbms_output.put_line('no existe el depto');
when dup_val_on_index 
	then dbms_output.put_line('el departamento ya existe');
when e_fk
	then dbms_output.put_line('excepcion personalizada');
when others
	then dbms_output.put_line('excepcion personalizada'||sqlerrm);
end
```

##### Procedimiento con cursor

```plsql
create or replace procedure pr_cambia_loc(pi_dep_id department.department_id%type,
                                         pi_loc_id department.location_id%type,
                                         po_cant_emp OUT number) -- parametro de salida que se debe agarrar con una variable
as
	e_fk exception;
	pragma exception_init(-2291,e_fk);
begin
	update department
	set location_id = pi_loc_id
	where department_id=pi_dep_id
	if sql%rowcount > 0
		THEN dbms_output.put_line('se actualizo sin problema');
		select count(employee_id) into po_cant_emp from department where department_id=pi_dep_id;
	else dbms_output.put_line('no existe el depto');
	end if;	
exception
	when e_fk then dbms_output.put_line('la localidad no existe');
end;


declare
v_cant_emp number;
begin
pr_cambia_loc(1,124,v_cant_emp);-- aca agarro el parametro que viene de salida
dbms_output.put_line(v_cant_emp)
end
```

##### Funciones

Tambien son stored procedures , que quedan almacenados en la base y pueden o no tener parametros y tienen que tener nombres unicos , la diferencia es que las funciones si o si **DEBEN RETORNAR UN VALOR**

Ej cual es la cantidad de empleados de un determinado depto

```plsql
create or replace function fu_cant_emp(pi_department_id department.department_id%type)
return number
as
val_ret number;
begin
	select count(employee_id) into val_ret 
	from employee 
	where department_id=pi_department_id;
 		return val_ret;
end;


declare
v_cantidad_empleados number;
begin
v_cantidad_empleados = fu_cant_emp(122); -- guardamos en una variable
dbms_output.put_line(fu_cant_emp(122)); -- tambien podemos imprimirlo directamente
end
```











