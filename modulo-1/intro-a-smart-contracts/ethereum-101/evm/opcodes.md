# Opcodes

Para que la EVM pueda ejecutar transacciones que llaman a un smart contract debe convertir el bytecode del smart contract a opcodes.

Los opcodes son instrucciones que la Ethereum Virtual Machine (EVM) puede ejecutar. Los opcodes se utilizan para realizar operaciones básicas, como sumar, restar, comparar y asignar valores. También se utilizan para ejecutar operaciones más complejas, como crear contratos inteligentes y llamar a funciones.

Los opcodes de Ethereum se codifican como un byte (2 caracteres hexadecimales). Cada opcode tiene un significado específico. Por ejemplo, el opcode ADD se utiliza para sumar dos valores, el opcode SUB se utiliza para restar dos valores y el opcode EQ se utiliza para comparar dos valores.

La EVM tiene un conjunto de 144 opcodes. Los opcodes se dividen en las siguientes categorías:

* **Operaciones aritméticas.** Se utilizan para realizar operaciones básicas, como sumar, restar, multiplicar y dividir.
* **Operaciones lógicas.** Se utilizan para realizar operaciones lógicas, como AND, OR y NOT.
* **Operaciones de comparación.** Se utilizan para comparar dos valores.
* **Operaciones de asignación.** Se utilizan para asignar un valor a una variable.
* **Operaciones de control de flujo.** Se utilizan para controlar el flujo de ejecución de un contrato inteligente.
* **Operaciones de memoria.** Se utilizan para manipular la memoria de la EVM.
* **Operaciones de llamadas.** Se utilizan para llamar a funciones de contratos inteligentes.
* **Operaciones de creación de contratos.** Se utilizan para crear nuevos contratos inteligentes.

Ejemplos de opcodes

Aquí hay algunos ejemplos de cómo se utilizan los opcodes de Ethereum:

Para sumar dos valores, se puede utilizar el opcode ADD. Por ejemplo, el siguiente código sumará los valores de las variables x e y:

x = 10 y = 20

z = ADD(x, y)

Para comparar dos valores, se puede utilizar el opcode EQ. Por ejemplo, el siguiente código comparará los valores de las variables x e y:

x = 10 y = 20

z = EQ(x, y)

Para asignar un valor a una variable, se puede utilizar el opcode MSTORE. Por ejemplo, el siguiente código asignará el valor 10 a la variable x: x = 10

Una muy buena referencia para entender el funcionamiento de los opcodes es la página [EVM Opcodes](https://www.evm.codes/) donde puedes ver la relación entre el código de un smart contracts y sus opcodes.
