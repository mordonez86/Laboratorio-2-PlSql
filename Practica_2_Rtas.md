### Práctica Unidad 2 : PL/SQL – Bloques Anónimos
1)Determinar si estas declaraciones son correctas. (se puede crear bloques anónimos con la parte ejecutable con null;)
V_var			number(8,3); **si**
V_a, V-b		number; **no**
V_fec_ingreso	Date := sysdate +2; **ni**
V_nombre		varchar2(30) not null; **?**
V_logico		boolean default ‘TRUE’;**no**

2)Crear un bloque anónimo para desplegar los siguientes mensajes:
‘Hola , soy ‘ username 
‘Hoy es: ‘  dd – Mon – yyyy’.  (mostrar la fecha del día)
```plsql
declare
v_nombre varchar(20);
begin
v_nombre:= :ingrese_nombre;
DBMS_OUTPUT.PUT_LINE('hola, soy: '||v_nombre);
DBMS_OUTPUT.PUT_LINE('Hoy es: '||to_char(SYSDATE,'dd-mm-YYYY'));
end;
```

3)Crear un bloque anónimo para desplegar los primeros n números múltiplos de 3. El valor de n debe ingresarse por pantalla usando una variable de sustitución del SqlDeveloper. Si n >10  desplegar un mensaje de advertencia y terminar el bloque.
```plsql
declare
v_contador number:=1;
v_maximo number;
begin
v_maximo:= :ingrese_maximo;
loop
DBMS_OUTPUT.PUT_LINE(v_contador*3);
v_contador:= v_contador+1;
exit when v_contador>v_maximo;
end loop;
end;
```

4)Crear un bloque Pl/Sql para consultar el salario de un empleado dado:
Ingresar el id del empleado usando una variable de sustitución
Desplegar por pantalla el siguiente texto:
First_name, Last_name  tiene un salario de Salary pesos.
```plsql
declare
v_employee_row employee%rowtype;
begin
select * into v_employee_row
from employee
where employee.employee_id=:ingrese_empleado;
DBMS_OUTPUT.PUT_LINE(v_employee_row.first_name||', '||v_employee_row.last_name||' Tiene un salario de '||v_employee_row.salary||' pesos');
end;
```

5)Escribir un bloque para desplegar todos los datos de una orden dada. 
Ingresar el nro de orden  usando una variable de sustitución
En una variable de tipo record recuperar toda la información y desplegarla usando Dbms_output.
Que pasa si la orden no existe?
```plsql
DECLARE
    v_sales_row sales_order%rowtype;
    v_input_sales_order sales_order.order_id%type;
BEGIN
    
    
    SELECT * INTO v_sales_row FROM sales_order WHERE order_id = :v_input_sales_order;

    DBMS_OUTPUT.PUT_LINE(v_sales_row.order_id||' '||v_sales_row.order_date);
EXCEPTION
WHEN no_data_found
    THEN DBMS_OUTPUT.PUT_LINE('Orden Inexistente');
END;
```
6)Escribir un bloque para mostrar la cantidad de órdenes que emitió un cliente dado siguiendo las siguientes consignas:
Ingresar el id del cliente una variable de sustitución
Si el cliente emitió menos de 3 órdenes desplegar:
	“El cliente nombre  ES REGULAR”.
Si emitió entre 4 y 6
	“El cliente  nombre ES BUENO”.
Si emitió más:
	“El cliente nombre ES MUY BUENO”.
```plsql

``` 
Ingresar un número de departamento n y mostrar el nombre del departamento y la cantidad de empleados que trabajan en él.
Si no tiene empleados sacar un mensaje “Sin empleados”
Si tiene entre 1 y 10 empleados desplegar “Normal”
Si tiene más de 10 empleados, desplegar “Muchos”.

Crear un bloque que pida un número de empleado y muestre su apellido y nombre y tantos ‘*’ como resulte de dividir su salario por 100.
Ej: empleado 7900 gana 950, entonces muestro *********

Hacer un bloque que guarde en las posiciones pares de una tabla en memoria (tabla Pl/Sql) de números enteros el múltiplo de 2 de la posición. Ej: T(4) := 8. Ingresar la cantidad de elementos que debe tener la tabla. Por último desplegar la cantidad de elementos que tiene la tabla y todo su contenido.

Opcional:
Crear un bloque que pida código de producto y guarde en una tabla en memoria (Tabla Pl/Sql indexada) el precio mínimo y precio de lista del producto.
Los precios deben guardarse en la posición de la tabla correspondiente al código de producto.
Ej: Producto 100 guardar en la posición T(100).
