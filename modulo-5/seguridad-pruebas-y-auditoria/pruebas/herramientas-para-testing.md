# Herramientas para testing

A continuación revisaremos algunas herramientas para hacer diferentes tipos de pruebas.

#### Pruebas unitarias

* [**Hardhat Tests**](https://hardhat.org/hardhat-runner/docs/guides/test-contracts) - Uno de los principales entornos de pruebas basado en ethers.js, Mocha y Chai.
* [**Foundry Tests**](https://book.getfoundry.sh/forge/tests) - Foundry incluye Forge, un entorno de pruebas rápido y adaptable que puede realizar pruebas unitarias simples, verificaciones de optimización de gas y fuzzing de contratos.
* [**Remix Test**](https://remix-ide.readthedocs.io/en/latest/unittesting.html#test-directory) - Esta popular herramienta cuenta con el plugin "Solidity Unit Testing" en Remix IDE, que se utiliza para escribir y ejecutar casos de prueba de contratos.
* [**Truffle Tests**](https://hardhat.org/hardhat-runner/docs/other-guides/truffle-testing) - Este es un marco de pruebas automatizado diseñado para simplificar las pruebas de contratos. Si bien este software está siendo descontinuado, aún se puede utilizar desde Hardhat.
* [**Brownie unit testing framework**](https://eth-brownie.readthedocs.io/en/v1.0.0\_a/tests.html) - Brownie utiliza Pytest, un entorno que permite realizar pequeñas pruebas con código mínimo y es adecuado para proyectos grandes debido a su escalabilidad.
* [**ApeWorx**](https://docs.apeworx.io/ape/stable/userguides/testing.html) - Este es un entorno desarrollo y pruebas basado en Python para contratos inteligentes, dirigido a la EVM.
* [**Waffle**](https://ethereum-waffle.readthedocs.io/en/latest/) - Este es un marco para el desarrollo avanzado y la prueba de contratos inteligentes, basado en ethers.js.
* [**solidity-coverage**](https://github.com/sc-forks/solidity-coverage) - Esta herramienta proporciona cobertura de código para contratos inteligentes escritos en Solidity. Si bien es una herramienta independiente, también está incorporada en Hardhat.

#### Pruebas basadas en propiedades - Análisis Estático

* [**Slither**](https://github.com/crytic/slither) - Este framework, desarrollado por la auditora Trail of Bits, está basado en Python. Ayuda a encontrar vulnerabilidades, mejora la comprensión del código y soporta la escritura de análisis personalizados para contratos inteligentes. Revisa un breve tutorial [aquí](https://youtu.be/YNZ-3l4S4M0?si=qCcmuwjvUPxkd2NZ).
* [**Ethlint**](https://ethlint.readthedocs.io/en/latest/) - Es es un linter utilizado para aplicar mejores prácticas de estilo y seguridad en Solidity.

#### Pruebas basadas en propiedades - Análisis Dinámico

* [**Echidna**](https://github.com/crytic/echidna/) - Este es un fuzzer de contratos desarrollado por Trail of Bits para detectar vulnerabilidades en contratos inteligentes utilizando pruebas basadas en propiedades. Este [tutorial](https://youtu.be/QofNQxW\_K08?si=hWEiQ4YPVBnhagWF) te puede servir para entender el fuzzing y cómo funciona Echidna.
* [**Diligence Fuzzing**](https://consensys.net/diligence/fuzzing/) - Esta es una herramienta de fuzzing automatizada útil para identificar violaciones de propiedades en el código de smart contracts.
* [**Manticore**](https://manticore.readthedocs.io/en/latest/index.html) - Este es un entorno de ejecución simbólica dinámica que sirve para analizar el bytecode de EVM.
* [**Mythril**](https://github.com/ConsenSys/mythril-classic) - Esta herramienta evalúa el bytecode de EVM para detectar vulnerabilidades en contratos. Usa la ejecución simbólica para encontrar vulnerabilidades.
* [**Diligence Scribble**](https://consensys.net/diligence/scribble/) - Scribble es un lenguaje de especificación y herramienta de verificación en tiempo de ejecución que te permite anotar contratos inteligentes con propiedades para pruebas automáticas de contratos utilizando herramientas como Diligence Fuzzing o MythX.
