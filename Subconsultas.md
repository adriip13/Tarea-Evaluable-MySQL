# TAREA EVALUABLE II
### Primer pedido de cada representante: 

>Este indicador nos ayuda a obtener la fecha del primer pedido de cada representante con sus datos.

```mysql
SELECT codEmpleado, nombre, (SELECT min(fechaPedido)  	/*Aquí consigo el primer pedido de cada representante*/				
                            FROM pedidos WHERE codEmpleado=codRepresentante) 	/*Con esto solo me saldrán los empleados que sean representantes*/
FROM empleados;	
```

### Pedidos superiores a la media de ‘x’ representante: 

>Este indicador nos listará los pedidos superiores a la media de los pedidos del representante que nosotros asignemos.

```mysql
SELECT lineasPedido.* FROM lineasPedido
WHERE cantidad*precioVenta >(SELECT AVG(cantidad*precioVenta)  /*Obtenemos la media*/
                            FROM lineasPedido 
                            JOIN pedidos 
                            USING (codPedido) 
                            WHERE codRepresentante ='x'); /*En la ‘x’ asignaré el código de representante que yo quiera, ej :‘106’*/
```

### Clientes sin pedido:

>Este indicador nos mostrará los clientes que no tienen ningún pedido.

```mysql
SELECT codCliente, nombre 
FROM clientes 
WHERE NOT EXISTS (SELECT *	 /*Seleccionamos los clientes en los que NO existan coincidencias con la tabla pedidos*/
                  FROM pedidos      		
                  WHERE clientes.codCliente = pedidos.codCliente);
```

### Vendedores que superan 50% objetivo:

>Este indicador listará las oﬁcinas cuyos vendedores tengan un sueldo que supere al 50% del objetivo de la oﬁcina: 

```mysql
SELECT codOficina
FROM oficinas AS ofi
WHERE 0.5*objetivo < (SELECT min(sueldo)  /*Cogemos el sueldo mínimo de la oficina y comprobamos que es mayor que el 0.5*objetivo*/			
                      FROM empleados AS emp			
                      WHERE (emp.oficina = ofi.codOficina)	
                      AND (emp.categoria = 'Representante')		
                      GROUP BY emp.oficina);
```
