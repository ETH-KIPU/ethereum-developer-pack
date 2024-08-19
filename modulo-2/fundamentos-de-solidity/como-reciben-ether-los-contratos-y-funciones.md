# Cómo reciben Ether los contratos y funciones

En Solidity, los conceptos de funciones **`payable`**, **`receive`**, y **`fallback`** juegan roles cruciales en la interacción de los contratos inteligentes con Ether (ETH) y en la gestión de llamadas de función inesperadas o datos enviados a un contrato. Vamos a desglosar cada uno de estos conceptos para entender su importancia y uso en el desarrollo de contratos inteligentes.

### **Funciones payable**

Una función marcada como **`payable`** es capaz de recibir Ether junto con la llamada a la función. Este modificador es necesario para que cualquier función acepte transacciones de Ether; sin él, si un contrato recibe Ether en una llamada a una función que no es **`payable`**, la transacción será rechazada.

```solidity
function deposit() public payable {
    // Lógica para manejar el depósito de Ether
}
```

El uso de **`payable`** es esencial en contratos que necesitan manejar fondos de Ether, como billeteras, juegos, o cualquier sistema que requiera pagos o donaciones.

### **Función receive**

Solidity permite definir una función **`receive()`** especial dentro de un contrato inteligente. Esta función no puede tener argumentos, no devuelve nada y debe tener visibilidad externa y el modificador **`payable`**. Se ejecuta en transacciones de Ether que no incluyen datos (es decir, transacciones puras de Ether) y no puede ser llamada directamente.

```solidity
receive() external payable {
    // Lógica específica para manejar Ether recibido
}
```

Si un contrato recibe Ether y no tiene una función **`receive()`** definida (o si la transacción incluye datos pero no coincide con ninguna función), se intentará ejecutar la función **`fallback()`** si está presente.

### **Función fallback**

La función **`fallback()`** en Solidity se invoca cuando un contrato recibe Ether sin datos o si ninguna de sus funciones coincide con la firma de la función llamada. Es una función de seguridad que permite al contrato manejar Ether o llamadas de función arbitrarias. Al igual que **`receive()`**, **`fallback()`** no puede tener argumentos, no devuelve nada, y debe tener visibilidad externa. Sin embargo, a diferencia de **`receive()`**, **`fallback()`** puede ser **`payable`** o no.

```solidity
fallback() external payable {
    // Lógica para manejar llamadas inesperadas o Ether recibido sin datos
}
```

Si **`fallback()`** es **`payable`**, el contrato puede recibir Ether a través de esta función. Si no es **`payable`**, el contrato todavía puede procesar llamadas de función que no coinciden, pero rechazará cualquier intento de enviar Ether.

**Consideraciones de Uso y Seguridad**

* **Manejo de Fondos**: Es crucial implementar adecuadamente la lógica de manejo de fondos en **`payable`**, **`receive()`**, y **`fallback()`** para evitar la pérdida de Ether y asegurar que el contrato solo realice acciones esperadas.
* **Seguridad de `fallback()`**: Dado que **`fallback()`** se ejecuta en llamadas de función no esperadas o en el envío de Ether sin datos, es importante limitar su complejidad y las operaciones que realiza para prevenir vulnerabilidades, como ataques de reentrancia.
* **Gas y `fallback()`**: Las llamadas a **`fallback()`** tienen un límite de gas más bajo que las llamadas a funciones normales. Por lo tanto, cualquier operación costosa en gas podría fallar.

<figure><img src="../../.gitbook/assets/Fallback or receive (1) (1).png" alt="" width="375"><figcaption></figcaption></figure>
