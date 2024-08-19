---
icon: person-running
---

# Ejercicio 6

En este ejercicio utilizaremos el concepto de arrays dinámicos para almacenar una lista de datos que iremos almacenando. También veremos cómo modificar los valores de posiciones específicas del array y cómo recuperar la lista completa de valores del array.

**Pasos a seguir:**

1. Programe el contrato en Remix,
2. Despliéguelo en una red de prueba de Ethereum como Sepolia,
3. Publique y verifique el contrato utilizando un explorador de bloques
4. Interactúe con el contrato a través del explorador de bloques añadiendo al menos cuatro valores al array **`storedInfos` .**
5. Modifique posiciones específicas de **`storedInfos`** usando la función **`updateInfo` .**
6. Lea el valor de posiciones espefícas de **`storedInfos`** usando la función **`getOneInfo` .**

```solidity
// SPDX-License-Identifier: MIT
pragma solidity 0.8.19;

/// @title Concepts: array = list of infos stored
/// @author Solange Gueiros
contract FirsArray {
    string[] private storedInfos;

    /**
    * Graba nuevos valores en el array usando push()
    * index devuelve la posición en la que se grabó el valor 
    */
    function addInfo(string memory myInfo) external returns (uint index) {
        storedInfos.push(myInfo);
        index = storedInfos.length -1;
    }

    /**
    * Modifica el valor de la posición index del array con un nuevo valor
    * Verifica que la posición sea válida, sino devuelve un error
    */
    function updateInfo(uint index, string memory newInfo) external {
        require (index < storedInfos.length, "invalid index");
        storedInfos[index] = newInfo;
    }

    /**
    * Devuelve el valor almacenado en la posición index del array
    * Verifica que la posición sea válida, sino devuelve un error
    */
    function getOneInfo(uint index) external view returns (string memory) {
        require (index < storedInfos.length, "invalid index");
        return storedInfos[index];
    }

    // Devuelve todos los valores contenidos en el array
    function listAllInfo() external view returns (string[] memory) {
        return storedInfos;
    } 
}
```
