---
icon: person-running
---

# Ejercicio 5

Incorporaremos en nuestro contrato un evento que informará cuando se ha cambiado el valor de la variable **`storedInfo`** indicando cuál era el valor original y cuál es el nuevo valor.

**Pasos a seguir:**

1. Programe el contrato en Remix,
2. Despliéguelo en una red de prueba de Ethereum como Sepolia,
3. Publique y verifique el contrato utilizando un explorador de bloques
4. Interactúe con el contrato a través del explorador de bloques modificando el valor de **`storedInfo` .**
5. Verifique en las transacciones del contrato en el explorador de bloques que se ha ejecutado la función **`setInfo`** y en la pestaña de eventos verifique que se ha generado un evento.

```solidity
// SPDX-License-Identifier: MIT
pragma solidity 0.8.19;

/// @title Concepts: event
/// @author Solange Gueiros
contract EventEmitter {
    string private storedInfo;
		// Define un evento para informar un cambio en storedInfo
    event InfoChange(string oldInfo, string newInfo);

    // Emite el evento cuando el valor de storedInfo va a ser cambiado
    
    function setInfo(string memory myInfo) external {
        emit InfoChange (storedInfo, myInfo);
        storedInfo = myInfo;
    } 

    function getInfo() external view returns (string memory) {
        return storedInfo;
    }   
}
```
