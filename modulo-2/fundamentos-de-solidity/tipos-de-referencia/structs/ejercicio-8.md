# Ejercicio 8

Haremos un ejercicio integrador que utilizará arrays, structs, mappings y enums.

Cada address tendrá asignado un array de structs. En cada struct se puede guardar un string, un color y se llevará la cuenta de las veces en que se modificó el string.

**Pasos a seguir:**

1. Programe el contrato en Remix,
2. Despliegue el contrato en Remix.
3. Con una address añada información en su correspondiente array para al menos dos posiciones (2 structs). Use la función **`addInfo`** para esto.
4. Haga lo mismo utilizando utilizando otra address.
5. Ejecute las funciones **`setInfo` , `setColor`, `getOneInfo`, `getMyInfoAtIndex`** y **`listAllInfo`.**

```solidity
// SPDX-License-Identifier: MIT
pragma solidity 0.8.19;

/// @title Concepts: All together 3
/// @author Solange Gueiros
// One array of structs per address = mapping, struct and array
// countChanges per address
contract AllTogether {

    //enum Colors {Undefined = 0, Blue = 1, Red = 2}
    enum Colors {Undefined, Blue, Red}

    struct InfoStruct {
        string info;
        Colors color;
        uint countChanges;
    }
    mapping (address => InfoStruct[]) public storedInfos;

    constructor() {
        InfoStruct memory auxInfo = InfoStruct ({
            info: "Hello world",
            color: Colors.Undefined,
            countChanges: 0
        });
        storedInfos[msg.sender].push(auxInfo);
    }

    event InfoChange(address person, uint countChanges, string oldInfo, string newInfo);

    // Añade un struct en la última posición del array asociado a quien envía la transacción 
    function addInfo(Colors myColor, string memory myInfo) public returns (uint index) {
        InfoStruct memory auxInfo = InfoStruct ({
            info: myInfo,
            color: myColor,
            countChanges: 0
        });
        storedInfos[msg.sender].push(auxInfo);
        index = storedInfos[msg.sender].length -1;
    }
    
    // Modifica la info dentro del struct de una posición específica del array
    function setInfo(uint index, string memory newInfo) public {
        storedInfos[msg.sender][index].countChanges++;
        emit InfoChange (msg.sender, storedInfos[msg.sender][index].countChanges, storedInfos[msg.sender][index].info, newInfo);
        storedInfos[msg.sender][index].info = newInfo;
    }

    // Modifica el color dentro del struct de una posición específica del array
    function setColor(uint index, Colors myColor) public {
        storedInfos[msg.sender][index].color = myColor;
        storedInfos[msg.sender][index].countChanges++;
    }

    // Devuelve el struct de una posición específica del array asociado a una address
    function getOneInfo(address account, uint index) public view returns (InfoStruct memory) {
        require (index < storedInfos[account].length, "invalid index");
        return storedInfos[account][index];
    }

    // Devuelve el struct de una posición específica del array asociado a quien envía la transacción
    function getMyInfoAtIndex(uint index) external view returns (InfoStruct memory) {
        return getOneInfo(msg.sender, index);
    }

    // Lista el array de structs asociado a una address
    function listAllInfo(address account) external view returns (InfoStruct[] memory) {
        return storedInfos[account];
    }
   
}
```
