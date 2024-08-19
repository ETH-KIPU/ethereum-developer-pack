# Arrays

Son una manera de almacenar datos de un mismo tipo. Pueden ser de dos tipos: estáticos y dinámicos. Los arrays estáticos tienen un tamaño fijo que se define en el momento de su declaración, mientras que los arrays dinámicos pueden cambiar de tamaño durante la ejecución del contrato.

**Declaración de Arrays**

Para declarar un array, especificas el tipo de los elementos que contendrá, seguido de corchetes **`[]`**. Si los corchetes están vacíos, el array es dinámico; si contienen un número, es estático y el número indica su tamaño.

```solidity
pragma solidity ^0.8.0;

contract EjemploArrays {
    // Array estático con espacio para 5 enteros
    uint[5] public arrayEstatico;

    // Array dinámico que puede contener un número arbitrario de enteros
    uint[] public arrayDinamico;
}
```

**Operaciones Básicas**

Acceso a Elementos

Puedes acceder a elementos de un array utilizando su índice, empezando por el 0.

```solidity
uint valor = arrayEstatico[0]; // Accede al primer elemento del array estático
```

Cambiar Tamaño (Solo Arrays Dinámicos)

Para cambiar el tamaño de un array dinámico, puedes usar la función **`push()`** para agregar un nuevo elemento al final del array o **`pop()`** para eliminar el último elemento.

```solidity
arrayDinamico.push(10); // Añade el valor 10 al final del array dinámico
arrayDinamico.pop(); // Elimina el último elemento del array dinámico
```

Longitud del Array

La propiedad **`length`** te permite obtener la longitud de un array, ya sea estático o dinámico.

```solidity
uint longitud = arrayDinamico.length; // Obtiene la longitud del array dinámico
```

A continuación un ejemplo de un contrato que inicializa un array con valores enteros utilizando \*\*`push()`\*\*y que puede también eliminar elementos del array utilizando **`pop()`** .

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.13; 

contract ArraysDS { 
		// Se declara un array dinámico de tipo entero 
		int[] numbers; 
		// función para añadir valores a un array  
		function store() public returns (int[] memory){ 
			numbers.push(5);
			numbers.push(10);
			numbers.push(20);
			return  numbers; 
		}  

		//función para retirar el ultimo valor del array
		function deleteLastElement() public returns(int[] memory){
		  numbers.pop(); 
			return numbers;  
		} 
} 
```

**Consideraciones de Uso**

* **Costo de Gas:** Las operaciones que cambian el tamaño de un array dinámico, como **`push`** y **`pop`**, incurren en costos de gas que pueden variar según la complejidad de la operación.
* **Arrays Multidimensionales:** Solidity también soporta arrays multidimensionales, pero ten en cuenta que el manejo de estos puede aumentar rápidamente los costos de gas debido a la complejidad adicional.
* **Limitaciones en Arrays Estáticos:** Aunque los arrays estáticos pueden parecer limitantes, su tamaño fijo puede ser beneficioso para optimizar el uso de gas en ciertos casos, ya que el coste de las operaciones es predecible y no varía.
