# JSON-RPC

¿Cómo conectamos una aplicación o nuestra terminal a una blockchain para que pueda interactuar con ella? Debemos conectarnos a un nodo de la blockchain utilizando un protocolo de comunicación.

En el caso de Ethereum, cada cliente de ejecución (Geth, Erigon, Besu, Nethermind) implementa una misma especificación JSON-RPC, de forma que existe un grupo de métodos al que las aplicaciones tienen acceso independientemente del cliente usado.

JSON-RPC es un protocolo para ejecutar métodos de forma remota que utiliza el formato JSON como estándar para transferir datos. JSON-RPC es independiente del lenguaje de programación, lo que permite su implementación en diversas tecnologías y entornos.

¿A qué nodo nos conectamos? A cualquier nodo de la blockchain, ya sea un nodo que nosotros controlemos o el de un tercero. En el segundo caso existen proveedores de acceso a blockchain como Alchemy, Quicknode o Infura, que brindan nodos para realizar esta conexión.

<figure><img src="../../../.gitbook/assets/Untitled (7).png" alt=""><figcaption></figcaption></figure>

JSON-RPC puede ser transportado sobre diversos protocolos de comunicación, como HTTP, WebSockets, TCP, etc. En el caso de HTTP no mantiene la conexión abierta, a diferencia de WebSockets que sí lo hace.

### Estructura de una solicitud JSON-RPC

Una solicitud JSON-RPC contiene los siguientes campos:

* **jsonrpc**: La versión del protocolo (actualmente "2.0").
* **method**: El nombre del método a invocar.
* **params**: (Opcional) Los parámetros para el método, que pueden ser un array o un objeto.
* **id**: (Opcional) Un identificador único para la solicitud, utilizado para correlacionar las respuestas. Si no se especifica, la solicitud se considera una notificación y no se espera una respuesta.

Veamos un ejemplo:

```json
{
  "jsonrpc": "2.0",
  "method": "subtract",
  "params": [42, 23],
  "id": 1
}
```

#### Estructura de una respuesta JSON-RPC

Una respuesta JSON-RPC contiene los siguientes campos:

* **jsonrpc**: La versión del protocolo (actualmente "2.0").
* **result**: El resultado de la ejecución del método (presente si no hubo errores).
* **error**: Un objeto de error (presente si hubo un error en la ejecución).
* **id**: El identificador de la solicitud correspondiente.

Ejemplo de una respuesta exitosa

```json
{
  "jsonrpc": "2.0",
  "result": 19,
  "id": 1
}
```

Ejemplo de una respuesta con error

```json
{
  "jsonrpc": "2.0",
  "error": {
    "code": -32601,
    "message": "Method not found"
  },
  "id": 1
}
```

#### Estructura de una notificación JSON-RPC

Una notificación es una solicitud sin campo `id`, lo que indica que el cliente no espera una respuesta.

```json
{
  "jsonrpc": "2.0",
  "method": "update",
  "params": ["parameter1", "parameter2"]
}
```

### Uso de JSON-RPC en Ethereum

Los métodos JSON RPC en Ethereum permiten enviar transacciones, consultar balances, interactuar con contratos inteligentes, entre otras operaciones. A continuación algunos ejemplos:

1.  **eth\_blockNumber:** Obtiene el número del bloque más reciente.

    Solicitud:

    ```json
    {
      "jsonrpc": "2.0",
      "method": "eth_blockNumber",
      "params": [],
      "id": 1
    }
    ```

    Respuesta:

    ```json
    {
      "jsonrpc": "2.0",
      "result": "0x5b8d80",
      "id": 1
    }
    ```

    Se puede observar que el resultado de la respuesta es devuelto en hexadecimal. Expresado en decimal el resultado sería “6000000”.
2.  **eth\_getBalance:** Obtiene el saldo de una dirección especificada.

    Solicitud:

    ```json
    {
      "jsonrpc": "2.0",
      "method": "eth_getBalance",
      "params": ["0x742d35Cc6634C0532925a3b844Bc454e4438f44e", "latest"],
      "id": 1
    }
    ```

    En este caso hubo que especificar como parámetros el address y el número bloque en el que queremos conocer el saldo (el más reciente).

    Respuesta:

    ```json
    {
      "jsonrpc": "2.0",
      "result": "0x0234c8a3397aab58",
      "id": 1
    }
    ```

    Expresado en decimal el resultado sería “158972490234375000” wei, o lo que es lo mismo 0.159 ETH.
3.  **eth\_sendTransaction:** Envía una transacción.

    Solicitud:

    ```json
    jsonCopiar código
    {
      "jsonrpc": "2.0",
      "method": "eth_sendTransaction",
      "params": [{
        "from": "0xYourAddress",
        "to": "0xReceiverAddress",
        "value": "0x9184e72a000"  // 0.01 ether
      }],
      "id": 1
    }
    ```

    Respuesta:

    ```json
    {
      "jsonrpc": "2.0",
      "result": "0xTransactionHash",
      "id": 1
    }
    ```

    El resultado que obtendremos como respuesta en este caso es el hash de la transacción.

### Conexión a Ethereum utilizando la terminal y JSON RPC.

Para establecer comunicación entre tu terminal y la blockchain de Ethereum utilizando llamadas JSON-RPC debemos seguir los siguientes pasos:

#### Paso 1: Configurar un nodo Ethereum o utilizar un nodo público

Como ya lo indicamos anteriormente, para interactuar con la blockchain de Ethereum, necesitas conectarte a un nodo. Puedes configurar tu propio nodo usando software como Geth o Nethermind, o utilizar un nodo público como Alchemy, Infura o QuickNode. Para efectos de este ejemplo utilizaremos un nodo de Alchemy.

#### Paso 2: Obtener un Endpoint JSON-RPC

Para conectarte a un nodo de Alchemy debes crear una cuenta en [https://www.alchemy.com/](https://www.alchemy.com/) y obtener una URL de endpoint JSON-RPC.

* Regístrate en [Alchemy](https://www.alchemy.com/).
*   Crea un nuevo proyecto y obtén la URL de tu endpoint, que tendrá un formato similar a:

    ```bash
    <https://eth-mainnet.g.alchemy.com/v2/YOUR_ALCHEMY_PROJECT_ID>
    ```

#### Paso 3: Hacer Llamadas JSON-RPC desde la terminal

Puedes utilizar herramientas como `curl` para hacer llamadas JSON-RPC directamente desde la terminal.

#### Ejemplo: Obtener el número de bloque Actual

Usando `curl`, puedes hacer una llamada JSON-RPC para obtener el número de bloque actual de la red Ethereum.

```bash
curl -X POST <https://eth-mainnet.g.alchemy.com/v2/YOUR_ALCHEMY_PROJECT_ID> \\
     -H "Content-Type: application/json" \\
     -d '{
           "jsonrpc":"2.0",
           "method":"eth_blockNumber",
           "params":[],
           "id":1
         }'
```

#### Explicación del comando

* **`X POST`**: Indica que se trata de una solicitud POST.
* **`https://eth-mainnet.g.alchemy.com/v2/YOUR_ALCHEMY_PROJECT_ID`**: URL del nodo Ethereum al que te estás conectando.
* **`H "Content-Type: application/json"`**: Establece el tipo de contenido de la solicitud como JSON.
* **`d '...'`**: Define el cuerpo de la solicitud, que contiene los parámetros de la llamada JSON-RPC.

#### Respuesta esperada

La respuesta será un objeto JSON que contiene el número de bloque actual en formato hexadecimal.

```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "result": "0x131f640" // Número de bloque en formato hexadecimal
}
```

### Algunos métodos JSON-RPC

| Método                | Descripción                                                               | Parámetros                                                                                                                  | JSON                                                                                                                                                                                                                                                                                                              |
| --------------------- | ------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| eth\_chainId          | Devuelve el ID de la blockchain actual. En el caso de Ethereum es 1.      |                                                                                                                             | { "jsonrpc": "2.0", "method": "eth\_chainId", "params": \[] }                                                                                                                                                                                                                                                     |
| eth\_blockNumber      | Devuelve el número del bloque más reciente.                               |                                                                                                                             | { "jsonrpc": "2.0", "method": "eth\_blockNumber", "params": \[] }                                                                                                                                                                                                                                                 |
| eth\_gasPrice         | Devuelve el precio actual del gas expresado en wei.                       |                                                                                                                             | { "jsonrpc": "2.0", "method": "eth\_gasPrice", "params": \[] }                                                                                                                                                                                                                                                    |
| eth\_blobBaseFee      | Devuelve la tarifa base por gas de blob expresado en wei..                |                                                                                                                             | { "jsonrpc": "2.0", "method": "eth\_blobBaseFee", "params": \[] }                                                                                                                                                                                                                                                 |
| eth\_getBalance       | Devuelve el saldo de una cuenta en un bloque determinado                  | account, block (earliest, latest, safe, finalized, pending)                                                                 | { "jsonrpc": "2.0", "method": "eth\_getBalance", "params": \[ "0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045", "latest" ] }                                                                                                                                                                                          |
| eth\_getCode          | Devuelve el código almacenado en una dirección determinada.               | account, block (earliest, latest, safe, finalized, pending)                                                                 | { "jsonrpc": "2.0", "method": "eth\_getCode", "params": \[ "0xC02aaA39b223FE8D0A0e5C4F27eAD9083C756Cc2", "latest" ] }                                                                                                                                                                                             |
| eth\_call             | Ejecuta una llamada sin crear una transacción en la blockchain (lectura). | nonce, type, from, to, gas, value, data, gas price, max fee per gas, max priority fee per gas, max fee per blob gas, block. | { "jsonrpc": "2.0", "method": "eth\_call", "params": \[ { "nonce": null, "type": null, "from": null, "to": "0xC02aaA39b223FE8D0A0e5C4F27eAD9083C756Cc2", "gas": null, "value": null, "data": null, "gasPrice": null, "maxFeePerGas": null, "maxPriorityFeePerGas": null, "maxFeePerBlobGas": null }, "latest" ] } |
| eth\_getBlockByNumber | Devuelve información de un bloque en función del número de bloque.        | block, is full (boolean)                                                                                                    | { "jsonrpc": "2.0", "method": "eth\_getBlockByNumber", "params": \[ "latest", false ] }                                                                                                                                                                                                                           |

### Información adicional

* [JSON-RPC en ethereum.org](https://ethereum.org/en/developers/docs/apis/json-rpc/)
* [Playground de JSON-RPC](https://ethereum-json-rpc.com/)
* [Especificación técnica de los métodos JSON-RPC](https://github.com/ethereum/execution-apis)
