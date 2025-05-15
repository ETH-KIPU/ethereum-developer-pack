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

Neste exemplo, usando a função `withdraw()`, um usuário pode sacar seu saldo em ether, o mesmo que foi previamente depositado no contrato. A função lê o saldo do usuário, envia o valor correspondente em ether para o endereço que realizou a chamada e, por fim, redefine o saldo desse endereço.

Como sabemos, é possível criar uma função _fallback_ que receba ether e execute determinado código. Assim, um atacante pode implantar um contrato que inclua uma função _fallback_ do seguinte tipo:

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

No contrato anterior, `attackedAddress` é o endereço do contrato que contém ether e que será alvo do ataque. Um invasor pode implantar esse contrato e iniciar um ataque utilizando a função `attack()`, que drenará todos os fundos do contrato vulnerável.

**Mitigação:**

* Utilizar o padrão de "Retirada" (_withdraw pattern_) em vez do padrão de "Envio" (_send pattern_).
* Atualizar o estado interno antes de realizar chamadas externas.
* Limitar o uso de chamadas externas e utilizar bibliotecas de segurança, como a OpenZeppelin.

**Referência:**

* [OpenZeppelin ReentrancyGuard](https://docs.openzeppelin.com/contracts/4.x/api/security#ReentrancyGuard)
