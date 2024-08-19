# Modificadores

Los modificadores en Solidity son una característica poderosa que permite a los desarrolladores cambiar o ampliar la semántica de las funciones en contratos inteligentes. Proporcionan una forma reutilizable y flexible de controlar el comportamiento de las funciones. Los modificadores pueden ser utilizados para añadir requisitos previos a la ejecución de una función o para modificar su comportamiento de alguna manera.

**Propósito de los Modificadores**

El principal propósito de los modificadores es añadir una lógica común a varias funciones de un contrato inteligente, lo que ayuda a reducir la redundancia del código y mejora su mantenibilidad. Por ejemplo, se pueden usar para:

* Restringir el acceso a funciones específicas solo a ciertos usuarios (como el propietario del contrato).
* Validar entradas a las funciones.
* Guardar condiciones o estados específicos antes y después de la ejecución de una función.

**Cómo Funcionan los Modificadores**

Un modificador es definido de manera similar a una función, pero se utiliza para envolver la lógica de otra función. Dentro del cuerpo del modificador, el código especial **`_;`** indica dónde se debe insertar el código de la función modificada. Cuando una función se llama, primero se ejecuta el código del modificador, hasta que se encuentra el **`_;`**, momento en el cual se ejecuta el código de la función, y después, si es necesario, el código restante del modificador.

**Ejemplo Básico**

Consideremos un modificador simple que restringe el acceso a una función solo al dueño (owner) del contrato:

```solidity
pragma solidity ^0.8.0;

contract MyContract {
    address public owner;

    constructor() {
        owner = msg.sender; // Establece el dueño del contrato al ser desplegado
    }

    // Definición del modificador
    modifier onlyOwner() {
        require(msg.sender == owner, "Solo el propietario puede ejecutar esta función.");
        _; // Continúa con la ejecución de la función modificada
    }

    // Uso del modificador en una función
    function myRestrictedFunction() public onlyOwner {
        // Lógica de la función aquí
    }
}
```

**Características Importantes**

* **Composición**: Los modificadores pueden ser aplicados a una función en secuencia, permitiendo componer diferentes comportamientos y restricciones de manera flexible.
* **Parámetros**: Al igual que las funciones, los modificadores pueden recibir parámetros. Esto permite crear modificadores más dinámicos y reutilizables que pueden operar basándose en argumentos pasados durante la llamada a la función.
* **Visibilidad**: Los modificadores pueden ser declarados como **`public`** o **`internal`**. Esto afecta cómo pueden ser utilizados dentro del mismo contrato o en contratos derivados.
