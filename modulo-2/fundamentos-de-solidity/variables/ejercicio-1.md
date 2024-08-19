---
icon: person-running
---

# Ejercicio 1

Es hora de poner en práctica los conceptos anteriores creando un contrato que tenga una variable de estado privada de tipo string llamada **`storedInfo`** y dos funciones:

* Una primera función llamada **`setInfo`** con visibilidad externa que se utilizará para cambiar el valor de la variable **`storedInfo`**
* Una segunda función denominada **`getInfo`** de visibilidad externa y que sólo leerá y retornará el contenido de **`storedInfo`**

**Pasos a seguir:**

1. Programe el contrato en Remix,
2. Despliéguelo en una red de prueba de Ethereum como Sepolia,
3. Publique y verifique el contrato utilizando un explorador de bloques
4. Interactúe con el contrato a través del explorador de bloques modificando dos veces el valor de **`setInfo`**

```solidity
// SPDX-License-Identifier: GPL-3.0
pragma solidity 0.8.19;
/// @title Storage string
/// @author Solange Gueiros
contract Register {
string private storedInfo;
/// Store `myInfo`
/// @dev stores the string in the state variable `storedInfo`
/// @param myInfo the new string to store
function setInfo(string memory myInfo) external {
    storedInfo = myInfo;
}

/// Return the stored string
/// @dev retrieves the string of the state variable `storedInfo`
/// @return the stored string
function getInfo() external view returns (string memory) {
    return storedInfo;
}
}
```

> La mayoría de ejercicios de este curso están basados en el GitHub [https://github.com/solangegueiros/register-learn-solidity](https://github.com/solangegueiros/register-learn-solidity) de Solange Gueiros, destacada educadora blockchain brasileña, a quien agradecemos por su apoyo continuo a nuestro programa 🙏🏻.
