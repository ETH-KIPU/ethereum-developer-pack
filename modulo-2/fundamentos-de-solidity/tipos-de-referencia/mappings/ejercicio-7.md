---
icon: person-running
---

# Ejercicio 7

Crearemos una whitelist para que solo las personas que estén en ella puedan modificar el valor de la variable **`storedInfo` .** El dueño del contrato será el único que podrá añadir o retirar personas de la whitelist.

**Pasos a seguir:**

1. Programe el contrato en Remix,
2. Despliegue el contrato en Remix.
3. Añada addresses en la whitelist utilizando las que Remix le proporciona.
4. Verifique que solo las personas incluidas en la whitelist pueden modificar el valor de **`storedInfo` .**

```solidity
// SPDX-License-Identifier: MIT
pragma solidity 0.8.19;

/// @title Concepts: mapping and access control: whiteList
/// @author Solange Gueiros
contract Whitelist {
    string private storedInfo;
    address public owner;
    mapping (address => bool) public whiteList;
    // El constructor inicializa al owner y lo incluye en la whitelist
    constructor() {
        owner = msg.sender;
        whiteList[msg.sender] = true;
        storedInfo = "Hello world";
    }
    
    modifier onlyOwner {
        require(msg.sender == owner,"Only owner");
        _;
    }
    // Se requiere que quien envíe la transacción esté en la whitelist
    modifier onlyWhitelist {
        require(whiteList[msg.sender] == true, "Only whitelist");
        _;
    }

    // setinfo solo accesible para quien esté en la whitelist
    function setInfo(string memory myInfo) external onlyWhitelist {
        storedInfo = myInfo;
    }

    // addMember sólo accesible para el dueño del contrato
    function addMember (address member) external onlyOwner {
        whiteList[member] = true;
    }

     // delMember sólo accesible para el dueño del contrato   
    function delMember (address member) external onlyOwner {
        whiteList[member] = false;
    }

    function getInfo() external view returns (string memory) {
        return storedInfo;
    }
}
```
