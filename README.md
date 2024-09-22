# Dim Sum Vault

## Descripción del contrato

> Explicación del contrato desarrollado, incluyendo la funcionalidad principal.
> [al final removemos este enunciado]

Se decidió implementar una bóveda siguiendo el estándar ERC-4626 de OpenZeppelin, en la cual los usuarios depositan token ERC-20 _(assets)_, y reciben a cambio tokens de la bóveda _(shares)_ que pueden ser redimidos y dependiendo del tiempo generan algún valor o ganancia al usuario.

Se le permite al usuario el retiro de las ganancias solamente y mantener su inversion en la boveda.

Existe una penalizacion del 10% por retiro anticipado de los _(assets)_ depositados.

El dueno del contrato ejecutara la funcion para calcular las ganancias periodicamente en ciclos de 7 dias.


## Razonamiento detrás del diseño

> Breve explicación de las decisiones técnicas y de diseño tomadas durante el desarrollo, haciendo énfasis en el uso de patrones de diseño en Solidity donde sea necesario.
> [al final removemos este enunciado]

### Decisiones técnicas

Los ciclos de distribucion de ganancias se definio en 7 dias.
La penalizacion de salida anticipada es del 10%
Se contabiliza un contador del total de fees generados al contrato mediante la variable 'totalFees'
Se contabiliza un contador de ganancias totales generados al usuario mediante la varia
Se utiliza un mapping para registrar los usuarios del contrato que han depositado
Se utiliza un mapping para registrar la ultima fecha de deposito de cada usuario

### Patrones de diseño utilizados

- **Ownable:** El patron se utiliza para restringir el acceso a algunas funciones del contrato.  Solo el propietario tendra derechos elevados para ejecutar algunas funciones como por ejemplo 'Pausa' o 'vaultReem' para la distribucion de las ganancias a los inversores. Se implemento una variable 'owner' de tipo address, la cual se inicializa al momento de desplegar el contrato con la dirección de quien despliegó el contrato. Se agrego un modifiador 'onlyOwner' para validar quien ejecuta el llamado es la direccion del dueno para garantizar la condición de que solo el dueño puede ejecutar las funciones restringidas.

- **Pausable:** En casos de emergencia, el contrato permite pausar las funciones 'steak', 'unsteak' y 'vaultRedeem' para preveneir el saqueo de activos de manera momentanea mientras se corrigen los problemas. Solo el dueño del contrato tiene acceso a pausar y reanudar el contrato, siguiendo los patrones de Ownable y Paused. Se implemento una variable booleana "paused" la cual se cambia a true cuando el dueno del contrato ejecuta la funcion pause() y emite un evento de Paused().  Inversamente solo el dueño puede llamar a la funcion unPause() la cual regresa el valor boolean a falso y emite un evento de información Unpaused().

- **Reentrancy Guard:** para prevenir ataques de reentrada que permitan a un atacante ejecutar repetidamente una función de la bóveda y drenar fondos o manipular el estado nuestro contrato de manera inesperada, hemos implementado el patrón Reentrancy Guard sobre las funciones que modifican los balances de la bóveda y que realizan transferencias de tokens, pues dichas funciones son más vulnerables a este tipo de ataques.

## Integrantes del equipo

[@ccalvarez](https://github.com/ccalvarez) - Carolina Cordero\
[@fedejim](https://github.com/fedejim) - Federico Jiménez\
[@aorue1](https://github.com/aorue1) - Andrés Orué Moraga\
[@cañas](https://github.com/Z3R0BYT3) - Alejandro Cañas
