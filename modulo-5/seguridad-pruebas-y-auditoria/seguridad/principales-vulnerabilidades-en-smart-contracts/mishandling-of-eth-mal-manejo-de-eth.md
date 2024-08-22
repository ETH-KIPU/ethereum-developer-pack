# Mishandling of ETH (Mal manejo de ETH)

Se refiere a errores en la gestión de transacciones con ETH, lo que puede resultar en pérdidas de fondos, bloqueos de contratos o un comportamiento inesperado. Estas vulnerabilidades suelen ocurrir debido a una incorrecta implementación de funciones que manejan la recepción, el envío o la transferencia de ETH.

**Ejemplo:**

Considera un contrato que permite a los usuarios depositar ETH y luego retirar su saldo en cualquier momento. Un problema común es el uso de la función `transfer()` o `send()` para enviar ETH, sin considerar que estas funciones limitan la cantidad de gas disponible para la ejecución de la lógica del contrato receptor. Esto puede ser explotado si el receptor es un contrato que consume mucho gas, provocando que la transacción falle y el ETH no sea transferido, bloqueando potencialmente los fondos.

```solidity
contract VulnerableContract {
    mapping(address => uint) public balances;

    function deposit() external payable {
        balances[msg.sender] += msg.value;
    }

    function withdraw(uint _amount) external {
        require(balances[msg.sender] >= _amount, "Insufficient balance");
        balances[msg.sender] -= _amount;
        msg.sender.transfer(_amount);  // Vulnerable to failure if the recipient contract consumes too much gas
    }
}
```

En este ejemplo, si un usuario intenta retirar ETH y su dirección es un contrato que requiere más gas de lo que `transfer()` o `send()` permite (2300 gas), la transacción fallará y los fondos podrían quedar bloqueados.

**Mitigación:**

1.  **Usar `call` para Enviar ETH:** En lugar de usar `transfer()` o `send()`, se recomienda usar la función `call{value: amount}("")` que proporciona más flexibilidad en el manejo del gas. Sin embargo, `call` devuelve un valor booleano que debe ser verificado para asegurar que la transferencia fue exitosa.

    ```solidity
    function withdraw(uint _amount) external {
        require(balances[msg.sender] >= _amount, "Insufficient balance");
        balances[msg.sender] -= _amount;
        (bool success, ) = msg.sender.call{value: _amount}("");
        require(success, "Transfer failed");
    }
    ```
2.  **Considerar la reentrada:** Al utilizar `call` para enviar ETH, el contrato es vulnerable a ataques de reentrada si no se implementan las medidas de seguridad adecuadas. Para mitigar esto, es esencial usar el patrón "efecto-interacción" (checks-effects-interactions), asegurando que el estado del contrato se actualice antes de hacer la llamada externa.

    ```solidity
    function withdraw(uint _amount) external {
        require(balances[msg.sender] >= _amount, "Insufficient balance");
        balances[msg.sender] -= _amount; // Actualizar el estado antes de la llamada externa
        (bool success, ) = msg.sender.call{value: _amount}("");
        require(success, "Transfer failed");
    }
    ```
3.  **Funciones Fallback y recepción de ETH:** Implementar correctamente las funciones `fallback` o `receive` para manejar de forma segura los ETH recibidos por el contrato. Evitar que estas funciones incluyan demasiada lógica para reducir el riesgo de errores o exploits.

    ```solidity
    receive() external payable {
        balances[msg.sender] += msg.value;
    }
    ```
4.  **Gestión de fondos bloqueados:** Implementar una función de emergencia que permita al propietario del contrato recuperar fondos bloqueados si ocurre un error en las transferencias, asegurando que los fondos no queden permanentemente inaccesibles.

    ```solidity
    function emergencyWithdraw() external onlyOwner {
        payable(owner).transfer(address(this).balance);
    }
    ```
