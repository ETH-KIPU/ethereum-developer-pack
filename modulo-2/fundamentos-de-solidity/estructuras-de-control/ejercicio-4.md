---
icon: person-running
---

# Ejercicio 4

En este ejercicio haremos una variante respecto al ejercicio 3 pues verificaremos si quien envía la transacción para asignar una valor a la variable **`storedInfo`** es el dueño (owner) del contrato. Solo el dueño puede modificar el valor de **`storedInfo` .**

**Pasos a seguir:**

1. Programe el contrato en Remix,
2. Despliéguelo en una red de prueba de Ethereum como Sepolia,
3. Publique y verifique el contrato utilizando un explorador de bloques
4. Interactúe con el contrato a través del explorador de bloques y verifique si puede modificar el valor de la variable **`storedInfo`.**
5. Intente modificar el valor de **`storedInfo`** conectándose con otra wallet.

```solidity
// SPDX-License-Identifier: MIT
pragma solidity 0.8.19;

/// @title Concepts: Type address, msg.sender, owner, if
/// @author Solange Gueiros
contract Owner {
    string private storedInfo;
    address public owner;
		// El constructor define como dueño a quien despliega el contrato.
    constructor() {
        owner = msg.sender;
    }

    /**
    * La función setInfo verifica si quien envía la transacción
    * es el dueño del contrato.
    * Si es así, modifica el valor de la variable. Sino no hace nada.
    */
    function setInfo(string memory myInfo) external {
        if (msg.sender == owner) {
            storedInfo = myInfo;
        }
    }

    function getInfo() external view returns (string memory) {
        return storedInfo;
    }
}
```

¿Cómo modificaría la función **`setInfo`** en el contrato anterior para utilizar la estructura de control **`require`** en lugar de **`if`** ?

Respuesta:

```solidity
 	function setInfo(string memory myInfo) external {
        require(msg.sender == owner, "Only owner");
        storedInfo = myInfo;
    }
```
