# Ethers.js

Ya vimos cómo es posible interactuar con Ethereum utilizando JSON-RPC. Sin embargo, existen librerías que facilitan esta interacción pues permiten incluir llamadas directamente dentro del código de las aplicaciones con instrucciones que utilizan JSON-RPC por detrás.

Una de las librerías más conocidas es ether.js que está escrita en JavaScript. Así como ésta existen otras en JavaScript (Web3.js) y otros lenguajes como Python (Web3py).

En nuestro caso, utilizaremos ether.js pues es la más usada actualmente.

### Hacer llamadas JSON-RPC usando ether.js

Veamos cómo empezar a utilizar ethers.js. Recuerda que dado que ejecutaremos código JavaScript necesitamos tener instalado node.js y npm (ver prerrequisitos de este módulo).

Ubícate en el repositorio de tu proyecto antes de empezar la instalación.

1.  **Instalar `ethers.js`:**

    ```bash
    npm init
    npm install ethers
    ```

    Asegúrate de que se hayan creado en tu directorio los componentes asociados a la instalación:

    * Un directorio `node_modules/` que contiene todos los módulos instalados, incluida la carpeta `ethers`.
    * Un archivo `package-lock.json` que describe las versiones exactas de las dependencias instaladas.
    * Tu archivo `package.json` debería contener `ethers` en la sección de dependencias.
2.  **Obtén una URL de acceso a Ethereum de un proveedor de servicios de acceso a nodos:**

    En nuestro caso utilizaremos Alchemy. Utiliza la URL que obtuviste en la sección anterior que tiene la siguiente estructura: “[https://eth-mainnet.g.alchemy.com/v2/YOUR\_ALCHEMY\_PROJECT\_ID”](https://eth-mainnet.g.alchemy.com/v2/YOUR\_ALCHEMY\_PROJECT\_ID%E2%80%9D).
3.  **Escribir un script para conectarse y hacer llamadas JSON-RPC:**

    Crea un archivo `index.js` en VS Code y añade el siguiente código:

    ```jsx
    const { ethers } = require("ethers");

    // Reemplaza con tu URL de Alchemy
    const alchemyUrl = '<https://eth-mainnet.g.alchemy.com/v2/YOUR_ALCHEMY_PROJECT_ID>';

    // Crear un proveedor usando la URL de Alchemy
    const provider = new ethers.JsonRpcProvider(alchemyUrl);

    // Obtener el número de bloque actual
    async function getBlockNumber() {
      try {
        const blockNumber = await provider.getBlockNumber();
        console.log("Número de bloque actual:", blockNumber);
      } catch (e) {
        console.error("Error al obtener el número de bloque", e);
      }
    }

    getBlockNumber();
    ```

    En este primer script obtuvimos el número del último bloque creado en Ethereum.

    Detallemos la estructura de este script:

    * Importamos la librería ethers.js
    * Definimos que nos conectaremos vía HTTPS con un proveedor de servicio de nodos como Alchemy y especificamos la URL para la conexión.
    * Creamos un nuevo `JsonRpcProvider` utilizando la URL de Alchemy. Un provider o proveedor en ethers.js es un objeto que se conecta a un nodo de Ethereum y permite realizar diversas llamadas a la blockchain, como obtener el número de bloque actual, enviar transacciones, etc.
    * Definimos una función asíncrona que llamamos `getBlockNumber`. Dentro de la función usamos `await provider.getBlockNumber()` para obtener el número de bloque actual de la blockchain. La palabra clave `await` hace que la función espere a que la promesa se resuelva y devuelve el resultado. Si la llamada tiene éxito, el número de bloque se imprime en la consola. Si hay un error durante la llamada, se captura en el bloque `catch` y se imprime un mensaje de error en la consola.
    * Ejecutamos la función `getBlockNumber`

    \<aside> ⚠️ Este script usa la versión 6 de ethers.js. Si utilizas una versión anterior es muy probable que obtengas un error.

    \</aside>
4.  **Ejecutar el Script:**

    ```bash
    node index.js
    ```
5.  **Escribir un script para obtener el saldo de una cuenta:**

    Ejecuta este código utilizando node.js para obtener el saldo de una cuenta.

    ```jsx
    const { ethers } = require("ethers");

    // Reemplaza con tu URL de Alchemy
    const alchemyUrl = '<https://eth-mainnet.g.alchemy.com/v2/YOUR_ALCHEMY_PROJECT_ID>';

    // Crear un proveedor usando la URL de Alchemy
    const provider = new ethers.JsonRpcProvider(alchemyUrl);

    // Indicar la cuenta de la que se quiere conocer el saldo
    const address = "0x742d35Cc6634C0532925a3b844Bc454e4438f44e";

    // Obtener el saldo de la cuenta
    async function getBalance() {
      try {
        const balance = await provider.getBalance(address);
        console.log("Saldo de la cuenta:", ethers.formatEther(balance), "ETH");
      } catch (e) {
        console.error("Error al obtener el saldo de la cuenta", e);
      }
    }

    getBalance();
    ```

    \<aside> 👨🏻‍💻 **Reto:** Utilizando este código, modifica el address para obtener el saldo de la cuenta pública de Vitalik.

    \</aside>

### El archivo .env

Cuando tengamos que hacer una llamada a la blockchain que involucre realizar una escritura, será necesario que firmemos las transacciones y para ello se requiere utilizar la clave privada de nuestra wallet. No podemos exponer a la vista de todos nuestra clave privada, o en general cualquier información sensible. ¿Cuál es la solución? Utilizar una combinación de un archivo .env (se lee “dot i-en-vi”) y un archivo gitignore.

Un .env es un archivo de texto que contiene pares clave-valor, donde cada par representa una variable de entorno. Estos archivos se utilizan para configurar aplicaciones de manera que la configuración específica del entorno (por ejemplo, desarrollo, prueba, producción) pueda mantenerse fuera del código fuente y cargarse dinámicamente según sea necesario.

#### ¿Por qué utilizar un archivo .env?

* **Seguridad**: Mantener claves API y otras credenciales fuera del código fuente ayuda a prevenir su exposición accidental.
* **Flexibilidad**: Permite cambiar configuraciones fácilmente sin modificar el código.
* **Portabilidad**: Facilita la configuración del entorno en diferentes máquinas y entornos (desarrollo, pruebas, producción).

#### Pasos para utilizar un archivo .env en un programa JavaScript

#### 1. Instalar dotenv

Primero, necesitas instalar la biblioteca `dotenv`, que carga las variables de entorno desde un archivo `.env` en `process.env`.

```bash
npm install dotenv
```

#### 2. Crear un archivo .env

En la raíz de tu proyecto, crea un archivo llamado `.env`. Este archivo contendrá tus variables de entorno. Por ejemplo:

```
ALCHEMY_URL=https://eth-mainnet.g.alchemy.com/v2/YOUR_ALCHEMY_PROJECT_ID
API_KEY=your_api_key
PORT=3000
```

#### 3. Cargar variables de entorno en tu aplicación

En tu archivo principal de JavaScript (por ejemplo, `index.js`), carga las variables de entorno usando `dotenv` y sentencias del tipo `process.env.VARIABLE_NAME` para incorporar los valores de las variables del archivo `.env` a tu aplicación.

```jsx
// Cargar dotenv
require('dotenv').config();

// Acceder a las variables de entorno
const alchemyUrl = process.env.ALCHEMY_URL;
const apiKey = process.env.API_KEY;
const port = process.env.PORT;

console.log(`Alchemy URL: ${alchemyUrl}`);
console.log(`API Key: ${apiKey}`);
console.log(`Port: ${port}`);
```

#### 4. Utilizar las variables de entorno en tu código

Puedes utilizar estas variables de entorno en tu aplicación. Por ejemplo, nuestro script para obtener el número de bloque actual utilizando un archivo `.env`sería el siguiente:

```jsx
const { ethers } = require('ethers');
require('dotenv').config();

const alchemyUrl = process.env.ALCHEMY_URL;
const provider = new ethers.JsonRpcProvider(alchemyUrl);

async function getBlockNumber() {
  try {
    const blockNumber = await provider.getBlockNumber();
    console.log('Número de bloque actual:', blockNumber);
  } catch (e) {
    console.error('Error al obtener el número de bloque', e);
  }
}

getBlockNumber();
```

#### 5. Usar un archivo .gitignore

Asegúrate de añadir tu archivo `.env` al archivo `.gitignore` para que no se suba a tu repositorio de Git y tus credenciales no se expongan accidentalmente.

```
.env
node_modules
```

Y la estructura de tu repositorio debería ser similar a esta:

```bash
mi-proyecto/
├── .env
├── .gitignore
├── index.js
└── package.json
```

Con estos pasos, tu aplicación puede usar variables de entorno definidas en un archivo `.env`, manteniendo la configuración flexible y segura.

### Hacer una llamada de escritura en Sepolia

Ahora veamos cómo debemos hacer para ejecutar una transacción que implica un cambio en la blockchain.

En este ejemplo haremos la transferencia de ETH de una cuenta a otra dentro de la red de Sepolia, para ello necesitamos contar con lo siguiente:

* Una URL de acceso a Sepolia del tipo: [https://eth-sepolia.g.alchemy.com/v2/TU\_ACCESO](https://eth-sepolia.g.alchemy.com/v2/TU\_ACCESO) (debes obtenerla de forma similar a la de mainnet en un servicio como Alchemy).
* La clave privada de la wallet de origen de los fondos.
* La dirección de destino de los fondos.
* Incluir los datos de la URL de acceso y clave privada en el fichero .env.

```jsx
const { ethers } = require('ethers');
require('dotenv').config();

// URL del proveedor de Sepolia desde el archivo .env
const sepoliaUrl = process.env.SEPOLIA_URL;

// Crear un proveedor usando la URL de Sepolia
const provider = new ethers.JsonRpcProvider(sepoliaUrl);

// Cargar la billetera desde una clave privada
const privateKey = process.env.PRIVATE_KEY;
const wallet = new ethers.Wallet(privateKey, provider);

async function sendTransaction() {
  const tx = {
    to: 'TARGET_ADDRESS', // Dirección de destino que debes especificar
    value: ethers.parseEther('0.1'), // Cantidad a enviar en ETH
    gasLimit: 21000,
    // gasPrice: opcional, puedes dejar que el proveedor determine el precio adecuado
  };

  try {
    const txResponse = await wallet.sendTransaction(tx);
    console.log('Transacción enviada:', txResponse.hash);

    // Esperar a que la transacción sea validada
    const receipt = await txResponse.wait();
    console.log('Transacción validada:', receipt);
  } catch (e) {
    console.error('Error al enviar la transacción', e);
  }
}

sendTransaction();
```

### Llamadas más usadas en ethers.js

#### Proveedores (providers)

1.  **Creación de un proveedor**

    ```jsx
    const provider = new ethers.JsonRpcProvider(url);
    ```
2.  **Obtener el número de bloque actual**

    ```jsx
    const blockNumber = await provider.getBlockNumber();
    ```
3.  **Obtener el saldo de una dirección**

    ```jsx
    const balance = await provider.getBalance(address);
    ```
4.  **Obtener información de un bloque**

    ```jsx
    const block = await provider.getBlock(blockNumber);
    ```
5.  **Obtener el historial de transacciones de una dirección**

    ```jsx
    const history = await provider.getHistory(address);
    ```
6.  **Obtener el precio del gas**

    ```jsx
    const gasPrice = await provider.getGasPrice();
    ```
7.  **Obtener una transacción**

    ```jsx
    const tx = await provider.getTransaction(transactionHash);
    ```
8.  **Obtener el estado de una transacción**

    ```jsx
    const txReceipt = await provider.getTransactionReceipt(txHash);
    ```

#### Wallets

1.  **Crear una billetera aleatoria**

    ```jsx
    const wallet = ethers.Wallet.createRandom();
    console.log("Address:", wallet.address);//para conocer la cuenta
    console.log("Private Key:", wallet.privateKey); //para conocer la clave privada
    ```
2.  **Cargar una billetera desde una clave privada**

    ```jsx
    const wallet = new ethers.Wallet(privateKey);
    ```
3.  **Conectar una billetera a un proveedor**

    ```jsx
    const connectedWallet = wallet.connect(provider);
    ```
4.  **Firmar un mensaje**

    ```jsx
    const signedMessage = await wallet.signMessage(message);
    ```
5.  **Enviar una transacción**

    ```jsx
    const txResponse = await wallet.sendTransaction(transaction);
    ```

#### Utilidades

1.  **Convertir Ether a Wei**

    ```jsx
    const weiAmount = ethers.parseEther("1.0");
    ```
2.  **Convertir Wei a Ether**

    ```jsx
    const etherAmount = ethers.formatEther(weiAmount);
    ```
3.  **Calcular el hash de un mensaje**

    ```jsx
    const messageHash = ethers.hashMessage(message);
    ```
4.  **Obtener la dirección de un contrato a partir de su bytecode**

    ```jsx
    const contractAddress = ethers.getContractAddress(transaction);
    ```

#### Contratos

1.  **Conectar a un contrato**

    ```jsx
    const contract = new ethers.Contract(contractAddress, abi, provider);
    ```
2.  **Llamar a una función de solo lectura**

    ```jsx
    const result = await contract.someReadOnlyFunction();
    ```
3.  **Enviar una transacción a una función de escritura**

    ```jsx
    const txResponse = await contract.someWriteFunction(params, { gasLimit, gasPrice });
    ```
4.  **Escuchar eventos de un contrato**

    ```jsx
    contract.on("EventName", (param1, param2, event) => {
      console.log(param1, param2, event);
    });
    ```
