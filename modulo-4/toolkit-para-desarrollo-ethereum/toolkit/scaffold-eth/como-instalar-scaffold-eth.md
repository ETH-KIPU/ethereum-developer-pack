# Cómo instalar Scaffold-ETH

#### Requisitos

Se requiere tener previamente instalado lo siguiente:

* Node.js
* npm
* Visual Studio Code
* Git

En los prerrequisitos de este módulo puedes encontrar las instrucciones para la instalación de estos componentes.

Adicionalmente es necesario instalar **yarn**.

* **Yarn:** Es un gestor de paquetes de código abierto que se utiliza para manejar dependencias en proyectos de JavaScript. Ayuda en el proceso de instalación, actualización, configuración y eliminación de dependencias de paquetes.

Para instalar yarn debemos ir a su [página web](https://yarnpkg.com/getting-started/install) y seguir las instrucciones para la instalación.

#### Instalación de Scaffold-ETH

Tenemos dos opciones para instalar Scaffold-ETH:

**Opción 1: Clonar el repositorio de GitHub e instalar las dependencias:**

```bash
git clone <https://github.com/scaffold-eth/scaffold-eth-2.git>
cd scaffold-eth-2
yarn install
```

**Opción 2: Instalación guiada utilizando npx:**

```bash
npx create-eth@latest
```

En este procedimiento se nos solicitará una serie de definiciones:

* **Nombre del proyecto:** Ingresa el nombre de tu proyecto.
* **Solidity Framework:** Elige tu framework de Solidity favorito, Hardhat o Foundry.
* **Instalar paquetes?:** Presiona `Enter` para `yes` (opción por defecto) o escribe  `n` y presiona  `Enter` para no.

Una vez hecha la configuración muévete hacia el directorio del proyecto:

`cd project-name`

Una vez instalado, luego de movernos al repositorio de trabajo y activar VS Code (`code.`) debemos ejecutar tres comandos utilizando tres ventanas distintas de la terminal:

**Ventana 1: Instalar la blockchain local.**

```bash
yarn chain
```

Este comando inicializa una red Ethereum local utilizando Hardhat o Foundry, dependiendo de lo que elegiste. Esta blockchain correrá de forma local en tu computadora y puede ser usada para desarrollo y pruebas.

**Ventana 2: Desplegar tu contrato inteligente.**

```bash
yarn deploy
```

Este comando despliega un contrato inteligente de prueba en la red local. Este contrato se puede adaptar a lo que necesites y los puedes ubicar en :

* Hardhat: `packages/hardhat/contracts`
* Foundry: `packages/foundry/contracts`

El comando  `yarn deploy` usa un script para desplegar el contrato en la red local. Puedes personalizar el scrip de despliegue que está ubicado en:

* Hardhat :`packages/hardhat/deploy`
* Foundry:  `packages/foundry/script`

**Ventana 3: Inicia el Front-End**

```bash
yarn start
```

Puedes editar el Front-End en  `packages/nextjs/app/page.tsx`

Si vas a la dirección `http://localhost:3000`. podrás ver la app funcionando.

<figure><img src="../../../../.gitbook/assets/Untitled (9).png" alt=""><figcaption></figcaption></figure>

Ya puedes interactuar con tu smart contract utilizando el componente de contrato o la UI de ejemplo en el Front-End.

La dapp de ejemplo permite registrar saludos y lleva la cuenta de cuántos saludos se registraron desde una address determinada. También permite al owner del contrato retirar los fondos que tenga el contrato.

<figure><img src="../../../../.gitbook/assets/Untitled (10).png" alt=""><figcaption></figcaption></figure>

Empieza a hacer cambios en el contrato de prueba ubicado en `packages/hardhat/contracts` , despliégalo utilizando `yarn deploy`y observa como el Front-End se va actualizando de forma automática como respuesta a los cambios realizados en el contrato.

Si no tienes ETH para ejecutar las transacciones utiliza el faucet incorporado que te asignará 1 ETH por solicitud.
