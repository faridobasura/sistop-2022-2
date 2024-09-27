# Amazon Go: Las tiendas del futuro 💵 📨 🛒

En la actualidad, hacer las compras y tener que formarse para pagarlas suele significar una pérdida de tiempo, sobre todo el tener que formarse por media hora esperando a que el cajero te atienda, te marque los productos que llevas y te cobre. Amazon Go en los últimos años ha implementado una serie de tiendas inteligentes (muy pocas realmente, ya que sale caro :moneybag: ) que busca resolver este problema.
Básicamente el cliente **entra** a la tienda, **escoge** sus productos y se va. **¿Y el cobro?**. El cliente tiene que escanear un código QR con su celular al entrar a la tienda, luego escoge sus productos. Como la tienda tiene cientos de sensores, cámaras y demás, se puede reconocer cuando el cliente toma un producto o lo deja en algún lugar. Así la tienda *sabe* que productos toma el cliente. Una vez que el cliente sale, se envía una notificación a su celular con el total de la cuenta, y el costo total de ella, *cargando* el cobro en su tarjeta de crédito.
Así se evita la interacción con cajeros, y las molestas filas de supermercado. Dentro de la tienda, solo existe el personal que se *encarga* de llenar los estantes, y el personal de *seguridad*. El personal de seguridad estará encargado de resolver situaciones como un intento de robo, pero en un mundo ideal, los robos no existen, y por tanto los de seguridad no hacen nada. Aún así, si un cliente que ya este dentro de la tienda quiere robar algo, el sistema lo *detecta* y se lo *cobra*.
Como este proyecto (*el de las tiendas, no el mío*) sigue en desarrollo y adaptandosé a las diferentes circunstancias, vamos a suponer que ya llegó a Latinoamerica. Espero tener la suficiente capacidad para resolver este problema, ya que como sabemos, en Latinoamerica si existen los robos, y también los de seguridad que solo van a comer y hacen cualquier otra cosa (*dormir* 💤 ) excepto cuidar la tienda como tal, ya que según argumentan *los farderos le roban a la tienda, y no a ellos*. 

## Descripción del problema

Los clientes pueden entrar solos o en grupo. Si van en grupo, solo uno de ellos escanea el código y entra junto con los demás de su grupo. Así cada persona del grupo puede escoger un producto, y el sistema lo agregará a la lista de compras de la persona que escaneo el código. Se tiene que asegurar que el último producto en ser tomado se agregue a la lista de compras correcta, ya que el grupo puede salir junto o en cualquier orden por separado. Cuando se acabe un producto, se *despiertan* los encargados de rellenar los anaqueles y surten el producto faltante. Una vez rellenados, estos empleados pueden irse a dormir. Para este problema solo vamos a suponer que venden *leche*, *queso*, *papel higiénico*, *agua*, *jugo*, *coca cola*, *pan*, *galletas*, *sopa*, *aceite*, y *café*.
Por último, en el intento de robo, el sistema lo detecta cuando el ladrón entra a la tienda **sin escanear el código QR**, y se le manda una *señal* al empleado de seguridad, quién al llegar de *dormir* 💤 le quitará lo que ha agarrado y lo detiene (aunque en realidad solo lo saca de la tienda y lo deja ir 🤷‍♂️ ).

### Situación a modelar

Se modelará los puntos propuestos en la descripción del problema, exceptuando la situación del robo. Vamos a suponer el mundo ideal, por lo que no modelaré la situación de robo, aunque creo que no tiene mucha complejidad, ya que un ladrón podría tratarse como un cliente más (que no escaneo su código QR 🦹‍♂️) que toma productos; y al de seguridad puede tratarse como un proveedor más, que incrementa la cantidad de productos en el estante 🥛 🧀.
Lo demás si se tratará modelar, la situación en que los clientes pueden entrar solos o en grupo, y que escogen varios productos. Pueden ser productos repetidos o diferentes, pero no excediendo de un cierto número (digamos, 6).

### ¿Consecuencias nocivas de la concurrencia? ¿Eventos que queramos controlar?

Para este problema, se necesita que un cliente solo tome productos para sí mismo y no para los demás. En cuestión de los grupos, se debe de contar con una lista de compras en común que pueda ser accesada por cualquier cliente que forma parte de un grupo de clientes. Si no se controla lo anterior, en un sistema real, a un cliente se le puede cobrar de más o menos 💵.
A su vez, cuando un producto este agotandosé en el anaquel, un empleado deberá ver que es lo que esta vació y rellenarlo. Si sale mal, el empleado podría llenar lo que esta lleno y no hacerle caso a lo que realmente hace falta.

### ¿Eventos concurrentes donde el ordenamiento relativo no importa?

Para este caso, el poder tomar productos de estante, ya que varios clientes pueden tomar lo que sea cuando sea (claro, una vez adentro) sin importar que un cliente lo haga primero o al último. Solo es necesario cuidar que un solo hilo modifique la lista de productos o compras, para evitar comportamientos no deseados.
