# Variables

En esta sección hablaremos de la visibilidad de las variables y de los tipos de variables que existen.

### Visibilidad de variables

Al igual que las funciones, las variables también tienen niveles de visibilidad. La visibilidad de una variable determina cómo y desde dónde puede ser accedida en el contrato y en otros contratos.

Hay tres niveles principales de visibilidad para las variables en Solidity:

*   **Public**: Las variables marcadas como **`public`** son automáticamente accesibles desde fuera del contrato. Solidity crea automáticamente una función "getter" para cada variable pública, lo que permite a otros contratos y a las llamadas externas leer su valor. Sin embargo, estas variables no pueden ser modificadas directamente desde fuera del contrato; para cambiar su valor, se requiere una función dentro del contrato que lo haga.

    ```solidity
    uint public publicVariable;
    ```
*   **Internal**: Las variables **`internal`** son accesibles dentro del contrato donde están declaradas y en contratos que heredan de él. No son accesibles desde fuera del contrato, a menos que haya una función que proporcione acceso a ellas.

    ```solidity
    uint internal internalVariable;
    ```
*   **Private**: Las variables **`private`** son las más restrictivas. Solo pueden ser accedidas y modificadas dentro del contrato que las declara. Incluso los contratos que heredan del contrato no pueden acceder a las variables privadas del contrato padre.

    ```solidity
    uint private privateVariable;
    ```

A diferencia de las funciones, no existe la visibilidad **`external`** para las variables.

Veamos un ejemplo en un contrato:

```solidity
contract MyContract {
    uint public publicVar = 10;
    uint internal internalVar = 20;
    uint private privateVar = 30;

    function accessVariables() public view returns (uint, uint, uint) {
        // Puede acceder a todas las variables ya que están dentro del mismo contrato
        return (publicVar, internalVar, privateVar);
    }
}

contract DerivedContract is MyContract {
    function accessInheritedVariables() public view returns (uint, uint) {
        // Puede acceder a publicVar e internalVar, pero no a privateVar
        return (publicVar, internalVar);
    }
}
```

En este ejemplo, **`publicVar`**, **`internalVar`** y **`privateVar`** tienen diferentes niveles de visibilidad, afectando cómo y dónde pueden ser accedidas. La función **`accessVariables`** dentro de **`MyContract`** puede acceder a todas ellas, mientras que **`accessInheritedVariables`** en **`DerivedContract`** puede acceder solo a las variables **`public`** e **`internal`** heredadas, pero no a la **`private`**.

🚨 La elección de la visibilidad adecuada para las variables es crucial para la seguridad y la funcionalidad correcta del contrato. Las variables **`public`** son útiles para proporcionar transparencia y acceso a datos importantes del contrato, mientras que las variables **`internal`**y **`private`**son importantes para proteger el estado interno del contrato de accesos no autorizados o malintencionados.

### Tipos de variables

Las variables se pueden clasificar en tres categorías principales según su alcance y duración: variables de estado, variables locales y variables globales. Cada tipo tiene características y usos específicos:

* **Variables de Estado:** Son variables que se almacenan permanentemente en la blockchain. Para cambiar una variable de estado es necesario ejecutar una transacción en Ethereum. Son accesibles desde cualquier función dentro del contrato y, dependiendo de su visibilidad, pueden ser accesibles desde otros contratos. Las variables **`public`** generan automáticamente una función “getter”.

```solidity
contract MyContract {
uint public stateVariable;  // Variable de estado
}
```

* **Variables Locales:** Son variables que se usan dentro de funciones y no se guardan en la blockchain de manera permanente. Su vida útil es solo durante la ejecución de la función. Solo son accesibles dentro de la función en la que se declaran. Generalmente se almacenan en la memoria (memory) o en la pila, y no en el almacenamiento permanente del contrato.

```solidity
contract MyContract {
function myFunction() public {
uint localVariable = 5;  // Variable local
}
}
```

*   **Variables globales:** Son variables predefinidas en Solidity que proporcionan información sobre la blockchain y las transacciones. No se necesita declararlas, ya que Solidity las proporciona automáticamente. Están disponibles globalmente en cualquier parte del contrato.

    Esta es la lista de variables globales de Ethereum

| Variable global                               | Descripción                                                                                                         |
| --------------------------------------------- | ------------------------------------------------------------------------------------------------------------------- |
| blockhash(uint blockNumber) returns (bytes32) | Hash del bloque indicado cuando blocknumber es uno de las 256 bloques más recientes, de lo contrario devuelve cero. |
| block.basefee (uint)                          | Tarifa base del bloque actual.                                                                                      |
| block.chainid (uint)                          | Id de la blockchain.                                                                                                |
| block.coinbase (address payable)              | Dirección del minero (ahora validador) del bloque actual.                                                           |
| block.difficulty (uint)                       | Dificultad del bloque actual (EVM < Paris). Actualmente deprecado.                                                  |
| block.gaslimit (uint)                         | Límite de gas del bloque actual                                                                                     |
| block.number (uint)                           | Número de bloque actual                                                                                             |
| block.prevrandao (uint)                       | Número aleatorio proporcionado por la beacon chain (EVM >= Paris)                                                   |
| block.timestamp (uint)                        | Marca de tiempo del bloque actual como segundos a partir de la época unix (00:00:00 UTC del 1 de enero de 1970).    |
| gasleft() returns (uint256)                   | Cantidad de gas restante.                                                                                           |
| msg.data (bytes calldata)                     | calldata de la llamada actual.                                                                                      |
| msg.sender (address)                          | Dirección del remitente de la llamada actual.                                                                       |
| msg.sig (bytes4)                              | Los primeros cuatro bytes de la calldata (identificador de función).                                                |
| msg.value (uint)                              | Cantidad de wei enviados con la llamada.                                                                            |
| tx.gasprice (uint)                            | Precio de gas de la transacción.                                                                                    |
| tx.origin (address)                           | Dirección del remitente original de la transacción.                                                                 |

Para terminar veamos un ejemplo de cómo declarar los diferentes tipos de variables.

```solidity
contract MyContract {
uint public stateVariable;  // Variable de estado
function myFunction() public {
    uint localVariable = 5;  // Variable local
    address sender = msg.sender;  // Variable global
}
}
```

En este ejemplo, **`stateVariable`** es una variable de estado, **`localVariable`** es una variable local, y **`sender`** es una variable global.
