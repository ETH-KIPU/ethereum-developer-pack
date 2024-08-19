# Hashing

El hashing se utiliza comúnmente para diversos propósitos, como verificar la integridad de los datos, crear identificadores únicos para los conjuntos de datos, y más. Solidity proporciona funciones de hash incorporadas que implementan algoritmos de hash seguros y eficientes.

Cabe destacar dos funciones: **`keccak256`** y **`sha256`.**

El algoritmo **`keccak256`** es una versión del SHA-3 (Secure Hash Algorithm 3) y es ampliamente utilizado en el ecosistema Ethereum, por ejemplo, para calcular direcciones Ethereum, para generar identificadores únicos, crear pruebas de existencia de datos (a través de merkle trees), y en la implementación de mecanismos de compromiso.

**`sha256`** es parte de la familia de algoritmos SHA-2 y es ampliamente utilizado fuera de Ethereum para garantizar la integridad de los datos. Aunque menos común en Ethereum que **`keccak256`**, **`sha256`** se utiliza cuando se necesita interoperabilidad con sistemas que utilizan SHA-256 como estándar de hashing.

#### Funciones de hashing

| Funciones                                                                | Resultado                                                                                                                   |
| ------------------------------------------------------------------------ | --------------------------------------------------------------------------------------------------------------------------- |
| keccak256(bytes memory) returns (bytes32)                                | Calcula el hash Keccak-256 de la entrada                                                                                    |
| sha256(bytes memory) returns (bytes32)                                   | Calcula el hash SHA-256 de la entrada                                                                                       |
| ripemd160(bytes memory) returns (bytes20                                 | Calcula el hash RIPEMD-160 de la entrada                                                                                    |
| ecrecover(bytes32 hash, uint8 v, bytes32 r, bytes32 s) returns (address) | Utilizando una firma de curva elíptica, devuelve la dirección asociada con la clave pública, o si hay error, devuelve cero. |

**Ejemplo**

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract HashingExample {
    // Función para calcular el hash de una cadena de caracteres
    function calculateHash(string memory _input) public pure returns (bytes32) {
        return keccak256(abi.encodePacked(_input));
    }
}
```
