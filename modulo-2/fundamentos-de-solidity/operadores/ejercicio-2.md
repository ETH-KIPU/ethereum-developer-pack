---
icon: person-running
---

# Ejercicio 2

En este ejercicio haremos unas modificaciones al contrato anterior para poder contar el número de veces que modificamos la variable **`storedInfo` .** Para ello utilizaremos un contador denominado **`countChanges`**que será una variable de tipo uint que debe ser visible de forma pública.

**Pasos a seguir:**

1. Programe el contrato en Remix,
2. Despliéguelo en una red de prueba de Ethereum como Sepolia,
3. Publique y verifique el contrato utilizando un explorador de bloques
4. Interactúe con el contrato a través del explorador de bloques modificando tres veces el valor de **`setInfo`** y verificando que así lo hizo con el contador **`countChanges`**

```solidity
// SPDX-License-Identifier: GPL-3.0
pragma solidity 0.8.19;
 
/// @title Concepts: variable uint with public visibility
/// @author Solange Gueiros
contract ChangeCounter {
    string private storedInfo;
    uint public countChanges = 0;

    /**
    * Store `myInfo`
    * Increase the counter which manage how many times storedInfo is updated
    * @dev stores the string in the state variable `storedInfo` and update the counter
    * @param myInfo the new string to store
    */
    function setInfo(string memory myInfo) external {
        storedInfo = myInfo;
        countChanges++;
    }

    /**
    * Return the stored string
    * @dev retrieves the string of the state variable `storedInfo`
    * @return the stored string
    */
    function getInfo() external view returns (string memory ) {
        return storedInfo;
    }
}
```
