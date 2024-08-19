# Codificación ABI

### Codificación ABI

Application Binary Interface (ABI) es un componente que actúa como la capa de interacción entre dos programas binarios, en este caso, entre contratos inteligentes y el mundo exterior (como aplicaciones frontend, otras herramientas de blockchain o incluso otros contratos inteligentes). El ABI especifica cómo codificar y decodificar datos para que puedan ser leídos por la Máquina Virtual de Ethereum (EVM). Esencialmente, el ABI es un conjunto de reglas que dictan cómo convertir las llamadas de funciones y sus parámetros en una forma que el contrato inteligente puede entender, y viceversa, cómo los datos de salida deben ser convertidos de vuelta para el mundo exterior.

Solidity proporciona funciones integradas para codificar y decodificar datos según estas reglas, conocidas colectivamente como funciones **`abi`**.

#### **Funciones abi**

1.  **`abi.encode(...) returns (bytes memory)`**

    Codifica los argumentos dados en una secuencia de bytes según las reglas ABI. Es útil para preparar datos para, por ejemplo, una llamada de función de bajo nivel.

    **Ejemplo**

    ```solidity
    bytes memory encodedData = abi.encode(arg1, arg2);
    ```

    En el ejemplo **`arg1, arg2`**: Son los argumentos que se pasan a la función **`abi.encode`**. Estos argumentos pueden ser de cualquier tipo soportado por Solidity, como **`uint`**, **`address`**, **`string`**, etc. La función **`abi.encode`** toma estos argumentos, los codifica en un formato binario único y devuelve este dato codificado como un arreglo de bytes.
2. **`abi.encodePacked(...) returns (bytes memory)`**

Similar a **`abi.encode`**, pero codifica los argumentos de una manera más compacta, sin rellenar, lo que puede ser útil para ciertas operaciones criptográficas. Sin embargo, la falta de relleno puede llevar a ambigüedades en ciertas situaciones, así que debe usarse con precaución.

**Ejemplo**:

```solidity
bytes memory packedData = abi.encodePacked(arg1, arg2);
```

3. **`abi.encodeWithSelector(bytes4 selector, ...) returns (bytes memory)`**

**Uso**: Codifica los argumentos dados con un selector de función específico. Útil para llamadas a funciones en contratos externos cuando se conoce el selector de la función.

**Ejemplo**:

```solidity
bytes memory dataWithSelector = abi.encodeWithSelector(selector, arg1, arg2);
```

El selector de función es esencialmente la firma de la función codificada en 4 bytes. Esta funcionalidad es particularmente útil cuando se realizan llamadas de bajo nivel o se interactúa con contratos cuyas interfaces pueden no estar completamente definidas en tiempo de compilación.

Incluyamos un ejemplo de cómo usar **`abi.encodeWithSelector`** en Solidity para preparar datos para una llamada de contrato de bajo nivel:

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract Receiver {
    event Received(uint256 indexed value, address sender);

    // Una función simple que emite un evento con el valor y el remitente
    function receiveValue(uint256 value) public {
        emit Received(value, msg.sender);
    }
}

contract Caller {
    // Función que llama a `receiveValue` en el contrato Receiver usando abi.encodeWithSelector
    function callReceiveValue(address _receiver, uint256 _value) public {
        // Primero, calculamos el selector de la función.
        // La firma de la función es "receiveValue(uint256)"
        bytes4 selector = bytes4(keccak256("receiveValue(uint256)"));

        // Luego, codificamos el selector junto con los argumentos de la función.
        bytes memory data = abi.encodeWithSelector(selector, _value);

        // Realizamos la llamada de bajo nivel.
        (bool success, ) = _receiver.call(data);
        require(success, "La llamada fallo.");
    }
}
```

En este ejemplo:

* **Receiver**: Es un contrato que tiene una función **`receiveValue`**, la cual emite un evento cuando se llama. Esta función podría ser cualquier lógica que quieras invocar en otro contrato.
* **Caller**: Es un contrato que realiza una llamada al contrato **`Receiver`**. Utiliza **`abi.encodeWithSelector`** para preparar los datos de la llamada. Esto incluye el selector de la función, que identifica qué función llamar en el contrato **`Receiver`**, y los argumentos para esa función.
  * **`bytes4 selector = bytes4(keccak256("receiveValue(uint256)"));`** calcula el selector de la función basado en su firma. La firma es simplemente el nombre de la función seguido de los tipos de sus parámetros entre paréntesis, codificado en 4 bytes.
  * **`abi.encodeWithSelector(selector, _value)`** codifica estos datos en un formato que el contrato **`Receiver`** puede decodificar y ejecutar.

4. **`abi.encodeWithSignature(string memory signature, ...) returns (bytes memory)`**

Similar a **`abi.encodeWithSelector`**, pero en lugar de proporcionar el selector de función como un valor **`bytes4`**, se proporciona la firma de la función como una cadena. Solidity calcula el selector de función correspondiente.

**Ejemplo**:

```solidity
bytes memory dataWithSignature = abi.encodeWithSignature("functionName(uint256,address)", arg1, arg2);
```

En este ejemplo, **`functionName(uint256,address)`** es la signature o firma.

5. **`abi.decode(bytes memory data, (type1, type2, ...)) returns (type1, type2, ...)`**

Decodifica los datos codificados según las reglas ABI en los tipos especificados. Es útil para interpretar los datos de salida de las llamadas a funciones de bajo nivel o las respuestas de las llamadas a otros contratos.

**Ejemplo**:

```solidity
(type1 var1, type2 var2) = abi.decode(encodedData, (type1, type2));
```
