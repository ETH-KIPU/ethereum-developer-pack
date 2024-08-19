# Tipos de almacenamiento para variables

En Solidity, hay tres tipos de ubicaciones de almacenamiento para variables, cada una con sus propias características y usos. Estas ubicaciones son: almacenamiento (storage), memoria (memory) y área de calldata. Comprender la diferencia entre ellas es crucial para un desarrollo eficiente y seguro de contratos inteligentes en Ethereum.

1. **Storage (Almacenamiento)**:
   * **Descripción**: Las variables de almacenamiento son persistentes y se guardan en la blockchain entre transacciones. Son como el "disco duro" de un contrato inteligente.
   * **Uso**: Se utiliza principalmente para variables de estado, es decir, variables que necesitan persistir entre llamadas a funciones y transacciones.
   * **Costo**: Es la forma más costosa de almacenamiento en términos de gas, especialmente en escrituras y modificaciones.
   * **Acceso**: Por defecto, las variables de estado son de almacenamiento.
2. **Memory (Memoria)**:
   * **Descripción**: Las variables en memoria son temporales y solo existen mientras una función está siendo ejecutada. Son borradas entre las llamadas a funciones externas y no se almacenan en la blockchain.
   * **Uso**: Útil para almacenar datos temporales durante la ejecución de una función. Por ejemplo, variables dentro de funciones o argumentos pasados a funciones internas.
   * **Costo**: Mucho más barato que el almacenamiento en términos de gas. No tiene un costo de gas permanente ya que los datos se eliminan al final de la ejecución de la función.
   * **Acceso**: Debes especificar explícitamente **`memory`** para variables de función y parámetros (excepto para tipos de referencia internos como mappings).
3. **Calldata**:
   * **Descripción**: Calldata es una ubicación de almacenamiento no modificable y temporal que contiene los datos de entrada de las transacciones y llamadas a funciones. Es similar a **`memory`**, pero solo se utiliza para datos de entrada de funciones externas.
   * **Uso**: Se utiliza para argumentos de función en funciones **`external`**. Es la forma más eficiente de pasar datos en términos de gas, especialmente para arrays y estructuras complejas.
   * **Costo**: Es generalmente más barato en términos de gas que **`memory`** porque los datos en **`calldata`** no se copian sino que se leen directamente de la transacción o llamada.
   * **Acceso**: Es de solo lectura y no puede modificarse.

**Ejemplo en Código**:

```solidity
pragma solidity ^0.8.0;

contract StorageTypes {
    // Storage: persistente entre transacciones
    uint public storageVar = 10;

    function exampleFunction(uint[] calldata calldataVar) external {
        // Calldata: datos de entrada en una función external
        uint localVar = calldataVar[0];

        // Memory: temporal durante la ejecución de la función
        uint[] memory memoryArray = new uint[](5);
    }
}
```

En este ejemplo, **`storageVar`** es una variable de almacenamiento, **`calldataVar`** es una variable de calldata (ya que es un argumento en una función **`external`**), y **`memoryArray`** es una variable de memoria.

La comprensión de estas tres ubicaciones de almacenamiento es fundamental para optimizar el uso de gas en contratos inteligentes y evitar errores comunes en Solidity.
