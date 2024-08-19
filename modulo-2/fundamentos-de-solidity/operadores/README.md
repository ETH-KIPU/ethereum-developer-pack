# Operadores

Los operadores de Solidity permiten realizar operaciones matemáticas, lógicas, de comparación, de asignación entre otras. Comprender estos operadores es fundamental para el desarrollo efectivo de contratos inteligentes.

### Aritméticos

| Operación      | Operador | Descripción                                  |
| -------------- | -------- | -------------------------------------------- |
| Suma           | +        | Suma dos operandos                           |
| Resta          | -        | Resta el segundo operando del primero        |
| Multiplicación | \*       | Multiplica los operandos                     |
| División       | /        | Divide el numerador por el denominador       |
| Módulo         | %        | Da como resultado el residuo de una división |
| Incremento     | ++       | Incrementa un valor entero en uno            |
| Decremento     | —        | Reduce un valor entero en uno                |

Ejemplo

```solidity
// SPDX-License-Identifier: MIT 
pragma solidity ^0.8.13; 
contract OperatorDemo {
	 // Inicializar variables
	 uint16 public first = 10;
	 uint16 public second = 30;
	 // Inicializar una variable con el operador de suma
	 uint public addition  = first + second;
	 // Inicializar una variable con el operador de resta
	 uint public subtraction  = second - first; 
		// Inicializar una variable con una multiplicación
	 uint public multiplication  = first * second;
	 // Inicializar una variable con el cociente de una división
	 uint public division = first / second; 
		// Inicializando una variable con módulo
		uint public modulus = first % second; 
		// Inicializar una variable con un valor reducido en uno
		uint public decrement = --second;
		// Inicializar una variable con un valor incrementado en uno
		uint public increment = ++first; 
}
```

### Relacionales

Utiliza estos operadores para comparar dos valores.

| Relación          | Operador | Descripción                                                                                           |
| ----------------- | -------- | ----------------------------------------------------------------------------------------------------- |
| Igual             | ==       | Compara si los valores son iguales. Si lo son, devuelve verdadero (true).                             |
| Diferente         | !=       | Compara si los valores son diferentes. Si lo son, devuelve verdadero (true).                          |
| Mayor que         | >        | Determina si el valor de la izquierda es mayor que el de la derecha. Devuelve true si es así.         |
| Menor que         | <        | Determina si el valor de la izquierda es menor que el de la derecha. Devuelve true si es así.         |
| Mayor o igual que | >=       | Determina si el valor de la izquierda es mayor o igual que el de la derecha. Devuelve true si es así. |
| Menor o igual que | <=       | Determina si el valor de la izquierda es menor o igual que el de la derecha. Devuelve true si es así. |

Ejemplo

```solidity
SPDX-License-Identifier: MIT 
pragma solidity ^0.8.13; 
contract OperatorDemo {
		// Inicializar variables
		 uint16 public first = 10;
		 uint16 public second = 30;
	  // Inicializar una variable (bool) con el resultado de una comparación igual  
			bool public equal = first == second; 
		// Inicializar una variable (bool) con el resultado de una comparación diferente 
		 bool public not_equal = first != second;
		// Inicializar una variable (bool) con el resultado de una comparación mayor
		bool public greater = second > first; 
		// Inicializar una variable (bool) con el resultado de una comparación menor
		bool public less = first < second; 
		// Inicializar una variable (bool) con el resultado de una comparación mayor o igual que
		bool public greaterThanEqualTo = second >= first; 
		/// Inicializar una variable (bool) con el resultado de una comparación menor o igual que
		bool public lessThanEqualTo = first <= second; 
}
```

### Lógicos

Combinan condiciones para determinar un valor lógico resultante.

| Función lógica | Operador | Descripción                                                                                             |
| -------------- | -------- | ------------------------------------------------------------------------------------------------------- |
| AND            | &&       | Devuelve verdadero (true) si ambas condiciones son verdaderas y falso (false) si al menos una es falsa. |
| OR             |          |                                                                                                         |
| NOT            | !        | Devuelve verdadero si la condición no se ha satisfecho.                                                 |

Ejemplo

```solidity
// SPDX-License-Identifier: MIT 
pragma solidity ^0.8.13; 
contract OperatorDemo { 
		// Inicializar variables
		 bool public first = true; 
		 bool public second = false; 
		// Inicializar una variable con el resultado de un AND
		 bool public and = first&&second; 
		// Inicializar una variable con el resultado de un OR
		 bool public or = first||second;
		// Inicializar una variable con el resultado de un NOT
		 bool public not = !second; 
}
```

### De asignación

Permiten asignar un valor a una variable. Al lado derecho del operador se ubica un valor y al lado izquierdo una variable.

| Tipo                      | Operador | Descripción                                                                                                     |
| ------------------------- | -------- | --------------------------------------------------------------------------------------------------------------- |
| Asignación simple         | =        | Se asigna el valor de la derecha a la variable a la izquierda del operador.                                     |
| Asignación suma           | +=       | Añade el operando de la derecha al de la izquierda, luego asigna el resultado al operando de la izquierda.      |
| Asignación resta          | -=       | Resta el operando de la derecha al de la izquierda, luego asigna el resultado al operando de la izquierda.      |
| Asignación multiplicación | \*=      | Multiplica ambos operandos, luego asigna el resultado al operando de la izquierda.                              |
| Asignación división       | /=       | Divide el operando de la izquierda por el de la derecha, luego asigna el resultado al operando de la izquierda. |
| Asignación módulo         | %=       | Divide el operando de la izquierda por el de la derecha, luego asigna el residuo al operando de la izquierda.   |

Ejemplo

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.13;
 contract OperatorDemo {
		// Inicializa variable de estado 
		uint  public first = 10; 
		//Asignación simple
		 function simpleAssignment() public {
		 first = 20; 
			} 
		//Asignación suma
		 function addAssignment() public {
			first += 10; 
			} 
		//Asignación resta
		function substractAssignment() public {
		 first -= 10; 
			}
		 //Asignación multiplicación
		 function multiplyAssignment() public {
		 first *= 5; 
			} 
		 //Asignación división
		function divideAssignment() public {
		 first /= 3;
			} 
		//Asignación módulo
		function modulusAssignment() public {
		 first %= 3; 
			} 
}
```

Sugerencia: Lleva este ejemplo a Remix y fíjate qué pasa con el valor de \*\*`first`\*\*cuando ejecutas cada una de las funciones.

### Bitwise

Son operadores utilizados para realizar operaciones a nivel de bit.

| Tipo                     | Operador | Descripción                                                                                                            |
| ------------------------ | -------- | ---------------------------------------------------------------------------------------------------------------------- |
| Bitwise AND              | &        | Se aplica un operador lógico AND a los operandos enteros a nivel de cada bit.                                          |
| Bitwise OR               |          |                                                                                                                        |
| Bitwise XOR              | ^        | Se aplica un operador lógico XOR a los operandos enteros a nivel de cada bit.                                          |
| Bitwise NOT              | \~       | Se aplica un operador lógico NOT al operando a nivel de cada bit.                                                      |
| Desplazamiento izquierdo | <<       | Los bits del primer operando se desplazan hacia la izquierda un número de posiciones indicada por el segundo operando. |
| Desplazamiento derecho   | >>       | Los bits del primer operando se desplazan hacia la derecha un número de posiciones indicada por el segundo operando.   |

Para entender estas funciones veamos unos ejemplos de cómo se hacen operaciones a nivel de bit o binario.

Para expresar un número en binario sólo utilizamos 0 y 1. Cada posición del número binario representa una potencia de 2. Así un 1 en la primera posición de la derecha es 2^0 = 1, en la segunda 2^1 = 2, en la tercera 2^2 = 4, en la cuarta 2^3 = 8, y así sucesivamente.

Supongamos que tenemos dos valores x e y:

x = 12, y = 5, que debemos expresar en binario para hacer operaciones bitwise.

x = 12 = 8 + 4 + 0 + 0 = 1100 (binario)

y = 5 = 0 + 4 + 0 + 1 = 0101 (binario)

Si realizamos la operación AND a nivel de bit tendríamos lo siguiente:

x & y = 0100 = 0 + 4 + 0 + 0 = 4

Si realizamos la operación OR a nivel de bit:

x | y = 1101 = 8 + 4 + 0 + 1 = 13

Si realizamos la operación XOR a nivel de bit, que es uno cuando uno de los operandos es 1:

x ^ y = 1001 = 8 + 0 + 0 + 1 = 9

Si realizamos la operación NOT a x a nivel de bit, lo que implica cambiar los ceros por uno y viceversa:

NOT x = 0011 = 0 + 0 + 2 + 1 = 3

Si desplazamos x hacia la izquierda dos posiciones:

x = 12 = 1100 (binario)

x << 2 = 110000 = 48

Si desplazamos x hacia la derecha dos posiciones:

x = 12 = 1100 (binario)

x >> 2 = 0011 = 3

Ejemplo. Despliega este contrato y prueba si los valores para x = 12, y = 5 coinciden con los resultados anteriores. Prueba con otros valores también.

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract BitwiseOps {
    
    function and(uint x, uint y) external pure returns (uint) {
        return x & y;
    }
    function or(uint x, uint y) external pure returns (uint) {
        return x | y;
    }

    function xor(uint x, uint y) external pure returns (uint) {
        return x ^ y;
    }

    function not(uint8 x) external pure returns (uint8) {
        return ~x;
    }

    function shiftLeft(uint x, uint bits) external pure returns (uint) {
        return x << bits;
    }

    function shiftRight(uint x, uint bits) external pure returns (uint) {
        return x >> bits;
    }
}
```

### Condicional

Es un operador ternario que evalúa inicialmente una expresión y ejecuta una acción si es verdadera u otra si es falsa.

El formato del operador ternario es el siguiente:

```solidity
<condición> ? <si es verdadera> : <si es falsa>
```

Ejemplo

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract Conditional {

    function ternary(uint _x) public pure returns (uint) {
        // Si _x es menor que 10, que la función devuelva 1, sino que devuelva 2
        return _x < 10 ? 1 : 2;
    }
}
```
