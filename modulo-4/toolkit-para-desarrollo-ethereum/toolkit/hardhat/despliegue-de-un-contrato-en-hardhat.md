# Despliegue de un contrato en Hardhat

### Despliegue de un smart contract en Hardhat

#### 1. Instalación

Primero, asegúrate de tener Node.js y npm instalados. Luego, crea un nuevo proyecto y añade Hardhat:

```bash
mkdir my-hardhat-project
cd my-hardhat-project
npm init -y
npm install --save-dev hardhat
```

#### 2. Crear un Proyecto de Hardhat

Inicializa un nuevo proyecto de Hardhat:

```bash
npx hardhat init
```

Sigue las instrucciones para crear un archivo `hardhat.config.js` vacío.

<figure><img src="../../../../.gitbook/assets/Untitled (8).png" alt=""><figcaption></figcaption></figure>

Esto te dejará con solo el fichero `hardhat.config.js` en tu repositorio.

#### 3. Tareas y plugins

Hardhat funciona ejecutando tareas (tasks) y llamando a plugins.

Las tareas disponibles las puedes ver ejecutando

```bash
npx hardhat
```

Como ejemplos de tareas tenemos: compilar, ejecutar scripts, habilitar un servidor JSON-RPC, habilitar la consola, verificar un contrato en Etherscan, etc.

Por otro lado, se utilizan plugins creados por Hardhat o terceros que contienen diferentes herramientas útiles para las diferentes etapas del desarrollo. Un plugin esencial es

[`@nomicfoundation/hardhat-toolbox`](https://hardhat.org/hardhat-runner/plugins/nomicfoundation-hardhat-toolbox) que contiene prácticamente todo lo que se necesita para desarrollar:

* Usar ethers.js para interactuar con contratos.
* Hacer pruebas con Mocha y Chai.
* Desplegar contratos con Hardhat Ignition.
* Interactuar con la red de prueba local de Hardhat.
* Obtener métricas de gas utilizado.
* Medir tu cobertura de pruebas.

Para instalarlo ejecuta la siguiente instrucción en la raíz del directorio de tu proyecto:

```bash
npm install --save-dev @nomicfoundation/hardhat-toolbox
```

también debes añadir la siguiente línea al inicio de tu fichero `hardhat.config.js`

```jsx
require("@nomicfoundation/hardhat-toolbox");
```

#### 4. Escribir un Contrato Inteligente

Crea un directorio llamado `contracts/` y dentro de él un archivo `Greeter.sol` con el siguiente código:

```solidity
//SPDX-License-Identifier: MIT
pragma solidity ^0.8.9;

import "hardhat/console.sol";

contract Greeter {
  string greeting;

  constructor(string memory _greeting) {
    console.log("Deploying a Greeter with greeting:", _greeting);
    greeting = _greeting;
  }

  function greet() public view returns (string memory) {
    return greeting;
  }

  function setGreeting(string memory _greeting) public {
    console.log("Changing greeting from '%s' to '%s'", greeting, _greeting);
    greeting = _greeting;
  }
}
```

#### 5. Compilar contratos

Para compilar tus contratos, ejecuta:

```bash
npx hardhat compile
```

En nuestro ejemplo, la ejecución de esta tarea nos creará un directorio denominado `artifacts`. El ABI de `Greeter.sol`está ubicado en `.artifacts/contracts/Greeter.sol/` y se llama `Greeter.json` .

#### 6. Activar una blockchain local

Al igual que Remix, Hardhat habilita una blockchain local. En esta red tendrás disponible 20 cuentas (addresses con sus claves privadas) con 10000 ETH de prueba cada una.

Para habilitar la blockchain local ejecuta desde el directorio raíz de tu repositorio:

```bash
npx hardhat node
```

Ten en cuenta que las claves addresses y sus claves privadas son públicas, dado que todos los usuarios de Hardhat las tienen a su disposición. Si utilizas esas addresses en mainnet o en alguna red pública perderás todo lo que tienes en ellas. Sólo debes usar estas addresses en el entorno de pruebas de Hardhat.

#### 7. Desplegar un contrato en la red local

En la raíz principal de tu repositorio crea un directorio `ignition` y dentro de él otro llamado `modules` . Luego dentro de `ignition/modules` crea el script de despliegue `Greeter.js` con el siguiente contenido:

```
const { buildModule } = require("@nomicfoundation/hardhat-ignition/modules");

const GreeterModule = buildModule("GreeterModule", (m) => {
  const greet = m.contract("Greeter", ["Hola"]);

  return { greet };
});

module.exports = GreeterModule;
```

Este código hace lo siguiente:

Llama a la función `buildModule` que requiere un ID de módulo y una función callback. Nuestro módulo se llamará `"GreeterModule"` .

La función callback es donde ocurre la definición del módulo. El parámetro `m` que se pasa al callback es una instancia de un `ModuleBuilder` , que es un objeto con métodos para definir y configurar las instancias de contratos.

Cuando llamamos a los métodos del `ModuleBuilder` se crea un objeto `Future` , que representa el resultado de la ejecución de un paso que Hardhat Ignition requiere para desplegar una instancia del contrato o interactuar con una existente.

Esto no ejecuta nada contra la red, solo se representa internamente. Luego de que se crea el `Future`, queda registrado dentro del módulo y el método lo devuelve.

En este módulo, se crean un objeto `Future` llamando al método `contract` que instruye a Hardhat Ignition que despliegue una instancia del contrato `Greeter` especificando `"Hola"`como el único parámetro del constructor.

Finalmente, se devuelve el objeto `Future` representando la instancia del contrato `Greeter` para hacerlo accesible a otros módulos y para realizar pruebas.

Para desplegar el smart contract en la red local ejecuta la siguiente instrucción en una nueva sesión del terminal.

```bash
npx hardhat ignition deploy ./ignition/modules/Greeter.js
```

Si todo funciona correctamente, obtendrás la dirección del contrato desplegado en la red local, en un mensaje como el siguiente:

```bash
Deployed Addresses

GreeterModule#Greeter - 0x5FbDB2315678afecb367f032d93F642f64180aa3
```
