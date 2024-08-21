# Despliegue de un contrato en una red pública

Describiremos este procedimiento utilizando la red Sepolia y un acceso a través de Alchemy.

Los pasos 1-5 que seguimos en el ejemplo anterior son los mismos para este caso pues debemos partir de un contrato que ya ha sido compilado para proceder a desplegarlo en Sepolia.

#### 1. Definir variables de entorno

Necesitamos configurar las siguientes variables:

* La clave API de Alchemy que nos permitirá el acceso al nodo Sepolia de Alchemy.
* La clave privada de nuestra address en Sepolia, necesaria para firmar la transacción de creación del contrato en Sepolia.
* La clave API de Etherscan que nos permitirá publicar y verificar el contrato. Para obtener esta clave ve a la página de [Etherscan](https://etherscan.io/login) , crea tu usuario si aún no lo tienes y luego crea tu clave en la sección de [API Keys](https://etherscan.io/myapikey).

Normalmente hubiéramos utilizado un archivo `.env` para guardar esta información, en este ejemplo utilizaremos tareas de configuración de variables dentro del nuevo scope `vars` de Hardhat. Si quieres profundizar sobre el tema, lee [este artículo](https://blog.nomic.foundation/hardhat-v2-19-0-introducing-configuration-variables-b528c0c9a7c0).

Nuestro archivo `hardhat.config.js` debe contener la siguiente información:

```jsx
require("@nomicfoundation/hardhat-toolbox");
const { vars } = require("hardhat/config");

const ALCHEMY_API_KEY = vars.get("ALCHEMY_API_KEY");
const SEPOLIA_PRIVATE_KEY = vars.get("SEPOLIA_PRIVATE_KEY");
const ETHERSCAN_API_KEY = vars.get("ETHERSCAN_API_KEY");

module.exports = {
  solidity: "0.8.24",
  networks: {
    sepolia: {
      url: `https://eth-sepolia.g.alchemy.com/v2/${ALCHEMY_API_KEY}`,
      accounts: [SEPOLIA_PRIVATE_KEY],
    },
  },
  etherscan: {
    apiKey: {
      sepolia: ETHERSCAN_API_KEY,
    },
  },
};
```

En nuestro archivo de configuración indicamos que utilizaremos las tareas de `vars` para gestionar las variables de entorno.

A través de la función `vars.get` incorporamos en la configuración las variables `ALCHEMY_API_KEY` , `SEPOLIA_PRIVATE_KEY` y `ETHERSCAN_API_KEY`.

Ahora debemos declarar los valores de estas tres variables usando `vars set` a través de las siguientes tareas en el terminal:

**a) Asignación de la clave API de Alchemy**

```bash
npx hardhat vars set ALCHEMY_API_KEY
```

Solicitará ingresar la clave API de Alchemy.

**b) Asignación de la clave privada**

```bash
npx hardhat vars set SEPOLIA_PRIVATE_KEY
```

Solicitará ingresar la clave privada de la wallet.

**c) Asignación de la clave API de Etherscan**

```bash
npx hardhat vars set ETHERSCAN_API_KEY
```

Solicitará ingresar la clave API de Etherscan.

Una vez definidos, estos valores se guardan fuera del repositorio para mayor seguridad.

Si deseas conocer cuáles son las variables de entorno de tu proyecto utiliza la siguiente tarea

```bash
npx hardhat vars setup
```

Si deseas conocer que variables ya han sido definidas y guardadas, usa la siguiente tarea.

```bash
npx hardhat vars list
```

#### 2. Desplegar el contrato

Para desplegar el contrato, procedamos a ingresar la siguiente tarea:

```bash
npx hardhat ignition deploy ignition/modules/Greeter.js --network sepolia --verify
```

Obtendrás como respuesta el número de contrato y la URL del contrato verificado y publicado.

&#x20;🙌🏻 **¡¡Felicitaciones!!** Has desplegado tu primer contrato utilizando Hardhat.

Si deseas borrar posteriormente las variables de entorno, como por ejemplo la clave privada, utiliza la siguiente tarea:

```bash
$ npx hardhat vars delete SEPOLIA_PRIVATE_KEY
```
