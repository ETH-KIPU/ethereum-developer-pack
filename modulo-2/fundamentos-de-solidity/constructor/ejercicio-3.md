---
icon: person-running
---

# Ejercicio 3

En este ejercicio utilizaremos un constructor para inicializar el valor de la variable **`storedInfo` .** Para ello utilizaremos un contador denominado **`countChanges`**que será una variable de tipo uint que debe ser visible de forma pública.

**Pasos a seguir:**

1. Programe el contrato en Remix,
2. Despliéguelo en una red de prueba de Ethereum como Sepolia,
3. Publique y verifique el contrato utilizando un explorador de bloques
4. Interactúe con el contrato a través del explorador de bloques y verifique que el constructor inicializó la variable **`storedInfo`.**

```solidity
// SPDX-License-Identifier: MIT
pragma solidity 0.8.19;

/// @title Concepts: Constructor
/// @author Solange Gueiros
contract FirstConstructor {
    string private storedInfo;
    uint public countChanges = 0;

    /**
    * Usamos el constructor para inicializar la variable stored Info
    */
    constructor() {
        storedInfo = "Hello world";
        // Considera el contador esta inicialización?
        // A revisar en clase
    }
    

    function setInfo(string memory myInfo) external {
        storedInfo = myInfo;
        countChanges++;
    }
    
    function getInfo() external view returns (string memory) {
        return storedInfo;
    }  
}
```
