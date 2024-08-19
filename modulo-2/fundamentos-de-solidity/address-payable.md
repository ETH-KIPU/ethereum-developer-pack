# Address Payable

La distinción entre **`address`** y **`address payable`** es una característica importante en Solidity para mejorar la seguridad y claridad del código, asegurando que solo las direcciones explícitamente marcadas puedan manejar y recibir Ether.

### **Address**

Una variable de tipo **`address`** puede almacenar una dirección Ethereum de 20 bytes. Este tipo se utiliza para representar direcciones de contratos o direcciones externas (cuentas de usuario) dentro de los contratos inteligentes. Sin embargo, una variable de tipo **`address`** por sí sola no puede recibir Ether directamente a través de transacciones de contrato porque no tiene el modificador **`payable`**.

### **Address payable**

Una dirección **`address payable`**, en cambio, es una dirección especial que puede recibir Ether. Al requerir que una dirección sea explícitamente marcada como **`payable`**, Solidity asegura que solo las direcciones destinadas a manejar Ether puedan hacerlo, evitando así transferencias accidentales o no autorizadas de fondos.

**Cómo Convertir una address a address payable**

En algunos casos, puede ser necesario convertir una **`address`** a **`address payable`**. Esto se puede hacer utilizando la conversión explícita en Solidity, como se muestra a continuación:

```solidity
address addr = msg.sender;
address payable payableAddr = payable(addr);
```

Este mecanismo de conversión es seguro y sigue las prácticas de programación recomendadas en Solidity, asegurando que solo las direcciones que el desarrollador desea que sean **`payable`** puedan recibir Ether.

**Uso de address y address payable**

* **`address`**: Utilizado para la mayoría de las interacciones que no involucran transferencias de Ether, como la consulta de balances de tokens ERC-20 o la interacción con contratos que no requieren el envío de Ether.
* **`address payable`**: Necesario cuando se desea enviar Ether a una dirección desde un contrato, utilizando métodos como **`transfer`**, **`send`**, o llamadas de bajo nivel.
