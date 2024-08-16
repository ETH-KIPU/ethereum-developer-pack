# Hello World

### License

Todos los programas deben empezar indicando su tipo de licencia, por ejemplo:

```solidity
// SPDX-License-Identifier: MIT
```

SPDX señala el tipo de licencia que corresponde al código. Si no se incluyen un tipo de licencia, el compilador de Solidity dará una advertencia. El SPDX se incluye dentro del bytecode por el compilador. En el caso de programas que son de código abierto como los que se utilizan en Ethereum, es importante dejar en claro los derechos de autor involucrados para que quienes quieran utilizarlos, o incluso copiarlos, tengan en cuenta las consecuencias legales. Por ejemplo los contratos inteligentes de Uniswap V3 utilizan la siguiente licencia:

```solidity
// SPDX-License-Identifier: BUSL-1.1
```

La licencia BUSL protege los derechos de autor por un período de tiempo (por ejemplo 3 años) después del cual la licencia pasa a ser de dominio público.

Una lista completa del tipo de licencias se encuentra [aquí.](https://spdx.org/licenses/)

Una de las licencias más usadas es la MIT, que es una licencias sin restricciones, pero que por otro lado no asume responsabilidades por el uso del software.

### Pragma

La versión de Pragma indica la versión del compilador que debe ser utilizada para que el código sea compilado sin fallos. Si el compilador utilizado no corresponde al pragma del código se emitirá un error.

Por ejemplo, una declaración de pragma típica en Solidity puede verse así:

```solidity
pragma solidity ^0.8.0;
```

En este caso, "^0.8.0" significa que el contrato puede ser compilado por cualquier compilador de Solidity desde la versión 0.8.0 hasta versiones que no introduzcan cambios incompatibles. Esto ayuda a prevenir problemas de incompatibilidad cuando se utiliza el contrato en diferentes entornos y versiones del compilador.

### Contract

**Contract** es la palabra que representa en Solidity a un contrato. Un contrato en Solidity es una entidad que contiene código ejecutable en Ethereum. Los contratos pueden interactuar con otros contratos y contener variables, funciones y eventos que definen su comportamiento.

En lenguajes orientados a objetos su equivalente sería una clase. Este sería un ejemplo de un contrato muy simple.

```solidity
contract HelloWorld {
string public greet = “Hello World!”;
}
```

Este es un contrato inteligente denominado **HelloWorld** que contiene una sola variable llamada **greet** que es de tipo string y es visible públicamente, a la que se le ha asignado el valor “Hello World!”. El código del contrato inteligente está delimitado por las llaves (”{” y “}”).

### Comentarios

Los comentarios en Solidity, al igual que en muchos otros lenguajes de programación, tienen como finalidad proporcionar explicaciones y documentación dentro del código fuente. Estos comentarios no afectan la ejecución del programa y son ignorados por el compilador.

Los comentarios son una forma de documentar el código fuente para que otras personas (o incluso el mismo programador en el futuro) puedan entender rápidamente la lógica y el propósito de las diferentes partes del contrato.

Los comentarios también se utilizan para explicar el propósito y el funcionamiento de funciones y variables, lo que facilita la comprensión del código y su uso.

Solidity permite realizar comentarios en una sola línea o en múltiples líneas.

Para un comentario en una sola línea se utilizan el símbolo “//” para iniciarlo. Lo que esté a continuación del símbolo y hasta el final de la línea será ignorado por el compilador.

```solidity
// Este es un comentario y será ignorado por el compilador 
```

Para incluir un comentario que utilice múltiples líneas se debe utilizar los símbolos “/_” y “_/” para delimitarlo.

```solidity
/* Este es un comentario de varias
líneas que también
será ignorado por el compilador*/ 
```
