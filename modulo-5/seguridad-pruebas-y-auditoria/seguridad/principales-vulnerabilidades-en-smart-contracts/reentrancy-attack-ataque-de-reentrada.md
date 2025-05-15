# Ataque de reentrância (Reentrancy attack)

O ataque de reentrância foi responsável por diversos hacks, incluindo o da DAO em 2016. Esse tipo de ataque ocorre quando um contrato realiza uma chamada externa para um endereço não confiável sem antes modificar seu próprio estado. O contrato malicioso pode então invocar recursivamente a função vulnerável. Por exemplo, uma função de saque que envia ETH antes de atualizar o saldo do usuário pode ser explorada se o endereço do destinatário for um contrato com uma função fallback que chame novamente a função de saque.

**Exemplo:** Vamos ver um exemplo onde um ataque de reentrância é possível.

```solidity
//Mala práctica
function withdraw() public {
		uint amount = balances[msg.sender];
		msg.sender.transfer(amount);
		balances[msg.sender] = 0;
}
```

En este ejemplo, usando la función `withdraw()` un usuario puede retirar su saldo de ether, el mismo que depositó en el contrato previamente. La función lee el saldo del usuario y envía el importe de ether a la dirección que la llamó y finalmente resetea el saldo de la dirección que lo llamó.

Como sabemos, podemos crear una función fallback que pueda recibir ether y que ejecute cierto código. Así, un atacante puede desplegar un contrato, en el que incluirá una función fallback del tipo siguiente:

```solidity
//Código usado por un atacante 
address attackedAddress = 0x1234;
 function attack() public onlyOwner {
    attackedAddress.withdraw();
}
 function() external payable {
    while(attackedAddress.balance > 0) {
        attackedAddress.withdraw();
    }
}
```

En el contrato anterior `attackedAddress` es la dirección del contrato que contienen ether al que se va a atacar. Un atacante puede desplegar este contrato e iniciar un ataque utilizando la función `attack()` que drenará todos los fondos del contrato atacado.

**Mitigación**:

* Usar el patrón de "Retirar" en lugar del patrón de "Enviar".
* Actualizar el estado antes de hacer llamadas externas.
* Limitar el uso de llamadas externas y usar bibliotecas de seguridad como OpenZeppelin.

**Referencia**:

* [OpenZeppelin ReentrancyGuard](https://docs.openzeppelin.com/contracts/4.x/api/security#ReentrancyGuard)
