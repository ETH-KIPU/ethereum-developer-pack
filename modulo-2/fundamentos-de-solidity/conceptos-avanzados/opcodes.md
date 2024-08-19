# Opcodes

En el contexto de Ethereum y Solidity, los **opcodes** (código de operación) son instrucciones de bajo nivel que la Ethereum Virtual Machine (EVM) puede ejecutar. Cada opcode representa una operación específica, como realizar cálculos matemáticos, leer y escribir en el almacenamiento, manipular el stack, realizar saltos condicionales e incondicionales, gestionar llamadas a otros contratos, y más. Los opcodes son los elementos fundamentales del bytecode que se ejecuta en la EVM.

Los opcodes de Ethereum se codifican como un byte (2 caracteres hexadecimales). Cada opcode tiene un significado específico. Por ejemplo, el opcode ADD se utiliza para sumar dos valores, el opcode SUB se utiliza para restar dos valores y el opcode EQ se utiliza para comparar dos valores.

Si empezamos a desmenuzar el bytecode de la sección anterior encontraremos al principio los siguientes opcodes:

* **`60`** es el opcode para **`PUSH1`**, lo que indica que el siguiente byte (u octeto) debe ser colocado en el stack.
* **`80`** sería el dato empujado por el **`PUSH1`**.
* **`60`** nuevamente indica otro **`PUSH1`**.
* **`40`** es el dato para este segundo **`PUSH1`**.
* **`52`** corresponde al opcode **`MSTORE`**, que guarda la palabra en memoria.

La EVM tiene un conjunto de 144 opcodes. Los opcodes se dividen en las siguientes categorías:

* **Operaciones aritméticas.** Se utilizan para realizar operaciones básicas, como sumar, restar, multiplicar y dividir.
* **Operaciones lógicas.** Se utilizan para realizar operaciones lógicas, como AND, OR y NOT.
* **Operaciones de comparación.** Se utilizan para comparar dos valores.
* **Operaciones de asignación.** Se utilizan para asignar un valor a una variable.
* **Operaciones de control de flujo.** Se utilizan para controlar el flujo de ejecución de un contrato inteligente.
* **Operaciones de memoria.** Se utilizan para manipular la memoria de la EVM.
* **Operaciones de llamadas.** Se utilizan para llamar a funciones de contratos inteligentes.
* **Operaciones de creación de contratos.** Se utilizan para crear nuevos contratos inteligentes.

**Características de los Opcodes**

* **Bajo Nivel**: Los opcodes operan a un nivel más bajo que el código Solidity. Mientras que los desarrolladores escriben contratos inteligentes en Solidity (u otros lenguajes de alto nivel), estos contratos son compilados a bytecode que consiste en una secuencia de opcodes.
* **Determinismo**: Cada opcode realiza una función específica y determinista, lo que es crucial para asegurar que todas las copias de la EVM en diferentes nodos de la red Ethereum ejecuten el mismo conjunto de instrucciones de manera idéntica y lleguen al mismo estado final.
* **Consumo de Gas**: Ejecutar opcodes en la EVM consume una cantidad específica de gas.

**Ejemplos de Opcodes**

Aquí hay algunos ejemplos de opcodes en Ethereum y sus funciones:

* **ADD, SUB, MUL, DIV**: Realizan operaciones aritméticas básicas como sumar, restar, multiplicar y dividir.
* **PUSH1, PUSH2, ..., PUSH32**: Colocan una constante de 1 a 32 bytes en el stack.
* **CALL**: Realiza una llamada a otro contrato.
* **STORE, LOAD**: Escriben y leen datos del almacenamiento persistente del contrato.
* **JUMP, JUMPI**: Realizan saltos incondicionales y condicionales, respectivamente, lo que permite la ejecución de bucles y condicionales.
* **STOP, RETURN, REVERT**: Controlan la terminación de la ejecución, devolviendo datos o revirtiendo la transacción.

Aunque los desarrolladores de Solidity generalmente no trabajan directamente con opcodes (ya que escriben código en un nivel más alto de abstracción), entender cómo el código Solidity se compila a opcodes puede ser útil para optimizar contratos inteligentes, especialmente en términos de consumo de gas. Además, la seguridad de los contratos inteligentes puede depender de comprender las implicaciones de bajo nivel de las operaciones de la EVM.

Una muy buena referencia para entender el funcionamiento de los opcodes es la página [EVM Opcodes](https://www.evm.codes/) donde puedes ver la relación entre el código de un smart contracts y sus opcodes.

Finalmente veamos la relación entre ABI, bytecode y opcodes.

<figure><img src="../../../.gitbook/assets/Bytecode ABI y opcodes (1).png" alt=""><figcaption></figcaption></figure>

A través de la compilación se obtiene el ABI y el bytecode.

Cuando se quiere ejecutar alguna función del smart contract que está en la blockchain bajo la forma de bytecode, se llama a esta función utilizando el ABI.

Con la información del ABI se identificará dentro del bytecode la función que luego se descompondrá en sus opcodes que a su vez serán procesados por la EVM.
