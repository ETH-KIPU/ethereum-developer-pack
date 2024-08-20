# ERC-20

El estándar ERC-20 es un estándar de tokens en la blockchain de Ethereum que define un conjunto común de reglas para los tokens emitidos mediante contratos inteligentes. Los tokens ERC-20 son activos digitales que pueden representar cualquier cosa, desde monedas virtuales hasta derechos de voto, pasando por certificados de propiedad y más. Este estándar permite la interoperabilidad entre diferentes aplicaciones y contratos en el ecosistema Ethereum, facilitando a los desarrolladores la implementación de tokens que funcionarán de manera consistente en una amplia gama de servicios y billeteras.

Un contrato inteligente ERC-20 debe implementar las siguientes funciones y eventos:

### **Funciones Requeridas:**

* **`totalSupply()`**: Devuelve la cantidad total de tokens existentes.
* **`balanceOf(address account)`**: Muestra la cantidad de tokens que tiene una dirección específica.
* **`transfer(address recipient, uint256 amount)`**: Permite a una dirección enviar una cantidad de tokens a otra.
* **`allowance(address owner, address spender)`**: Muestra la cantidad de tokens que el dueño permite a otra dirección gastar en su nombre.
* **`approve(address spender, uint256 amount)`**: Permite a un gastador retirar tokens hasta una cantidad máxima.
* **`transferFrom(address sender, address recipient, uint256 amount)`**: Permite a un gastador transferir tokens en nombre de otra dirección, dentro del límite establecido por **`approve`**.

### **Eventos Requeridos:**

* **`Transfer(address indexed from, address indexed to, uint256 value)`**: Debe emitirse cuando los tokens pasen de una dirección a otra.
* **`Approval(address indexed owner, address indexed spender, uint256 value)`**: Debe emitirse cuando una dirección aprueba a otra para gastar tokens en su nombre.

Aquí tienes un ejemplo simplificado de un contrato ERC-20:

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract SimpleERC20Token {
    string public constant name = "SimpleERC20Token";
    string public constant symbol = "SET";
    uint8 public constant decimals = 18;

    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(address indexed owner, address indexed spender, uint256 value);

    mapping(address => uint256) balances;
    mapping(address => mapping(address => uint256)) allowed;
    uint256 totalSupply_;

    constructor(uint256 total) {
        totalSupply_ = total;
        balances[msg.sender] = totalSupply_;
    }

    function totalSupply() public view returns (uint256) {
        return totalSupply_;
    }

    function balanceOf(address tokenOwner) public view returns (uint256) {
        return balances[tokenOwner];
    }

    function transfer(address receiver, uint256 numTokens) public returns (bool) {
        require(numTokens <= balances[msg.sender]);
        balances[msg.sender] = balances[msg.sender] - numTokens;
        balances[receiver] = balances[receiver] + numTokens;
        emit Transfer(msg.sender, receiver, numTokens);
        return true;
    }

    function approve(address delegate, uint256 numTokens) public returns (bool) {
        allowed[msg.sender][delegate] = numTokens;
        emit Approval(msg.sender, delegate, numTokens);
        return true;
    }

    function allowance(address owner, address delegate) public view returns (uint) {
        return allowed[owner][delegate];
    }

    function transferFrom(address owner, address buyer, uint256 numTokens) public returns (bool) {
        require(numTokens <= balances[owner]);
        require(numTokens <= allowed[owner][msg.sender]);

        balances[owner] = balances[owner] - numTokens;
        allowed[owner][msg.sender] = allowed[owner][msg.sender] - numTokens;
        balances[buyer] = balances[buyer] + numTokens;
        emit Transfer(owner, buyer, numTokens);
        return true;
    }
}
```

También puedes ver la implementación de Open Zeppelin en: [https://github.com/OpenZeppelin/openzeppelin-contracts/blob/master/contracts/token/ERC20/ERC20.sol](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/master/contracts/token/ERC20/ERC20.sol)

<figure><img src="../../.gitbook/assets/Modulo 3-1.png" alt=""><figcaption></figcaption></figure>
