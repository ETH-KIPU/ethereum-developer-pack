# Missing Access Control (Pérdida de Control de Acceso)

Ocurre cuando las funciones críticas dentro del contrato no están protegidas adecuadamente con restricciones de acceso. Esto permite que cualquier usuario, incluso aquellos no autorizados, pueda ejecutar esas funciones, lo que puede llevar a la manipulación de los datos del contrato, la apropiación de fondos, o la interrupción de su funcionamiento.

**Ejemplo:**

Imagina un contrato inteligente que gestiona un fondo común, donde varias personas pueden depositar y retirar fondos. El contrato tiene una función `withdrawAllFunds` que permite retirar todos los fondos del contrato y transferirlos a una dirección específica.

Si esta función no está protegida por un mecanismo de control de acceso, cualquier usuario podría llamar a `withdrawAllFunds` y especificar su propia dirección, retirando todos los fondos del contrato sin restricciones. Esto resultaría en la pérdida de todos los fondos depositados por otros usuarios.

```solidity
contract VulnerableContract {
    address public owner;
    uint public totalFunds;

    constructor() {
        owner = msg.sender;
    }

    function deposit() external payable {
        totalFunds += msg.value;
    }

    function withdrawAllFunds(address payable _to) external {
        require(totalFunds > 0, "No funds available");
        _to.transfer(totalFunds);
        totalFunds = 0;
    }
}
```

En este ejemplo, la función `withdrawAllFunds` no tiene ningún control de acceso, por lo que cualquier persona podría ejecutarla, incluso si no es el propietario del contrato.

**Mitigación:**

1.  **Implementar Modificadores de Acceso:** La solución más directa es implementar modificadores que restrinjan el acceso a las funciones críticas solo a usuarios específicos, como el propietario del contrato o una lista de administradores. En Solidity, esto se puede hacer con un modificador `onlyOwner`, que permite que solo el propietario del contrato ejecute ciertas funciones.

    ```solidity
    modifier onlyOwner() {
        require(msg.sender == owner, "Not the contract owner");
        _;
    }

    function withdrawAllFunds(address payable _to) external onlyOwner {
        require(totalFunds > 0, "No funds available");
        _to.transfer(totalFunds);
        totalFunds = 0;
    }
    ```

    Con este modificador, solo el propietario del contrato puede llamar a la función `withdrawAllFunds`.
2. **Usar bibliotecas de control de acceso:** Utilizar bibliotecas probadas y bien auditadas, como [`OpenZeppelin AccessControl`](https://docs.openzeppelin.com/contracts/2.x/access-control), para manejar los permisos de manera segura y estructurada. Estas bibliotecas permiten la creación de roles específicos y la asignación de permisos a funciones concretas.
3. **Pruebas y simulaciones:** Implementar pruebas rigurosas que simulen ataques potenciales, donde usuarios no autorizados intenten acceder a funciones críticas. Estas pruebas ayudan a garantizar que todas las funciones importantes estén adecuadamente protegidas.
