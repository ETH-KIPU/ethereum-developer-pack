# Abstract

El concepto de contratos abstractos se usa para crear contratos que sirven como plantillas base para otros contratos, pero que no están destinados a ser desplegados por sí mismos. Un contrato se considera abstracto si al menos una de sus funciones no tiene implementación completa dentro del contrato. Esta falta de implementación indica que el contrato está destinado a ser extendido por otros contratos que completarán estas implementaciones. Los contratos abstractos son una herramienta fundamental en lSolidity, permitiendo a los desarrolladores definir interfaces y comportamientos comunes que pueden ser compartidos y extendidos por múltiples contratos.

**Características de los Contratos Abstractos**

1. **Funciones sin implementar**: La principal característica de un contrato abstracto es que contiene al menos una función sin una implementación completa. Esto se indica mediante la ausencia del cuerpo de la función (las llaves **`{}`**) y finalizando la declaración de la función con un punto y coma (**`;`**).
2. **Herencia**: Los contratos abstractos están diseñados para ser heredados por otros contratos. Un contrato que hereda de un contrato abstracto debe implementar todas las funciones sin implementar, a menos que dicho contrato también sea declarado como abstracto.
3. **Palabra clave `abstract`**: Se utiliza para para declarar explícitamente un contrato como abstracto. La introducción de esta palabra clave permite a los desarrolladores comunicar sus intenciones de manera más clara y evitar el despliegue accidental de contratos incompletos.
4. **Uso de `virtual` y `override`**: En el contexto de contratos abstractos, las funciones sin implementar se pueden marcar como **`virtual`**, y los contratos que heredan estas funciones deben usar la palabra clave **`override`** al proporcionar una implementación, siguiendo el sistema de herencia y polimorfismo de Solidity.

Veamos un ejemplo a continuación.

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

abstract contract Animal {
    function makeSound() public virtual returns (string memory);
}

contract Dog is Animal {
    function makeSound() public pure override returns (string memory) {
        return "Woof";
    }
}

contract Cat is Animal {
    function makeSound() public pure override returns (string memory) {
        return "Meow";
    }
}
```

En este ejemplo, **`Animal`** es un contrato abstracto porque tiene una función, **`makeSound`**, sin implementar. La palabra clave **`virtual`** indica que esta función está destinada a ser sobrescrita por los contratos herederos. Los contratos **`Dog`** y **`Cat`** heredan de **`Animal`** y proporcionan sus propias implementaciones de la función **`makeSound`**, utilizando la palabra clave **`override`** para indicar que están sobrescribiendo la función abstracta.
