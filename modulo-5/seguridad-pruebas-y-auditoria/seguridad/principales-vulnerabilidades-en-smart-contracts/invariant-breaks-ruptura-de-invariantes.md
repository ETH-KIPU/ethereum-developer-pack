# Invariant Breaks (Ruptura de invariantes)

Ocurre cuando las condiciones o "invariantes" que se suponen deben mantenerse siempre verdaderas dentro del contrato, son violadas. Las invariantes son propiedades o reglas fundamentales que no deben cambiar durante la ejecución del contrato. Si un atacante o incluso un usuario legítimo encuentra una forma de romper estas invariantes, pueden ocurrir comportamientos inesperados, errores graves, o pérdidas financieras.

**Ejemplo:**

Considera un contrato inteligente de una stablecoin que debe mantener una relación 1:1 entre los tokens emitidos y los colaterales en una reserva:

```solidity
contract Stablecoin {
    mapping(address => uint) public balances;
    uint public totalSupply;
    uint public totalCollateral;

    function mint(uint amount) external payable {
        require(msg.value == amount, "Must send exact collateral");
        balances[msg.sender] += amount;
        totalSupply += amount;
        totalCollateral += msg.value;
    }

    function burn(uint amount) external {
        require(balances[msg.sender] >= amount, "Insufficient balance");
        balances[msg.sender] -= amount;
        totalSupply -= amount;
        payable(msg.sender).transfer(amount);
        totalCollateral -= amount;
    }
}
```

En este contrato, la invariante crítica es que `totalSupply` debe ser siempre igual a `totalCollateral`, asegurando que cada token emitido esté respaldado por un valor equivalente en colateral.

Supongamos que el contrato permite que se realicen tanto operaciones de mint como de burn en una misma transacción, pero no verifica adecuadamente el estado intermedio:

```solidity
function mintAndBurn(uint mintAmount, uint burnAmount) external payable {
    mint(mintAmount);
    burn(burnAmount);
}
```

Si un atacante puede encontrar una secuencia específica de llamadas donde esta invariante se rompe temporalmente, podría aprovechar para manipular el estado del contrato en su beneficio. Por ejemplo, podría mintear más tokens de los que están respaldados por colateral o quemar tokens sin reducir el colateral equivalente, lo que llevaría a un desequilibrio en la reserva.

**Mitigación:**

1.  **Validación de invariantes:** Después de cada operación crítica (como mint o burn), el contrato debe validar explícitamente que las invariantes clave se mantengan. Esto puede hacerse mediante funciones de verificación que aseguren que las relaciones entre variables importantes no han sido violadas.

    ```solidity
    function checkInvariants() internal view {
        require(totalSupply == totalCollateral, "Invariant violation: totalSupply must equal totalCollateral");
    }

    function mint(uint amount) external payable {
        require(msg.value == amount, "Must send exact collateral");
        balances[msg.sender] += amount;
        totalSupply += amount;
        totalCollateral += msg.value;
        checkInvariants();
    }

    function burn(uint amount) external {
        require(balances[msg.sender] >= amount, "Insufficient balance");
        balances[msg.sender] -= amount;
        totalSupply -= amount;
        payable(msg.sender).transfer(amount);
        totalCollateral -= amount;
        checkInvariants();
    }
    ```
2. **Uso de modelos formales:** Aplicar técnicas de verificación formal para modelar y probar el comportamiento del contrato en diferentes escenarios, asegurando que las invariantes se mantengan en todos los casos posibles.
3. **Restricción de operaciones compuestas:** Evitar que múltiples operaciones críticas se realicen en una misma transacción sin validar el estado después de cada una. Si es necesario permitir tales operaciones, se debe hacer con extremo cuidado, asegurando que las invariantes se mantengan en todos los puntos intermedios.
