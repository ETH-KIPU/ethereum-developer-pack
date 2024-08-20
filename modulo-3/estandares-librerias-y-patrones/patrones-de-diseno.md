# Patrones de Diseño

En el desarrollo de contratos inteligentes con Solidity, aplicar patrones de diseño probados puede mejorar significativamente la seguridad, eficiencia y mantenibilidad de tu código. Estos patrones ayudan a resolver problemas comunes en el desarrollo de software y son particularmente importantes en Ethereum, donde los errores pueden tener consecuencias costosas y la optimización del gas es crucial. A continuación, se detallan algunos patrones de diseño importantes en Solidity:

### 1. Patrón de Chequeo-Efectos-Interacción (Checks-Effects-Interactions)

Este patrón ayuda a prevenir ataques de reentrancia al asegurar que las llamadas a contratos externos se realicen al final de una función. Se estructura de la siguiente manera:

* **Chequeo**: Primero, verifica todas las condiciones y valida las entradas.
* **Efectos**: Luego, actualiza el estado del contrato.
* **Interacción**: Finalmente, realiza interacciones con otros contratos.

A continuación un ejemplo de este patrón:

<pre class="language-solidity"><code class="lang-solidity">// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract SafeWithdrawal {
    mapping(address => uint) public balances;
    
<strong>    // Función para depositar Ether en el contrato
</strong>    function deposit() public payable {
        balances[msg.sender] += msg.value;
    }
    
    // Función para retiro, siguiendo el patrón de Chequeo-Efectos-Interacción
    function withdraw(uint _amount) public {
        // Chequeo: Verifica si remitente tiene suficiente balance para retirar
        require(balances[msg.sender] >= _amount, "Saldo insuficiente");
    
        // Efecto: Actualiza el balance antes de la interacción
        balances[msg.sender] -= _amount;
    
        // Interacción: Transfiere Ether al remitente
        (bool sent, ) = msg.sender.call{value: _amount}("");
        require(sent, "Fallo al enviar Ether");
    }
    
    // Función para consultar el balance del contrato
    function getBalance() public view returns (uint) {
        return address(this).balance;
    }
}
</code></pre>

### **2. Patrón de Retiro (Withdrawal Pattern)**

En lugar de enviar Ether directamente a los usuarios (por ejemplo, con **`send`** o **`transfer`**), este patrón permite que los usuarios retiren fondos por sí mismos. Reduce el riesgo de errores y ataques de reentrancia.

```solidity
contract WithdrawalContract {
    mapping(address => uint) public balances;

    function withdraw() public {
        uint amount = balances[msg.sender];
        require(amount > 0);

        balances[msg.sender] = 0;
        (bool success, ) = msg.sender.call{value: amount}("");
        require(success);
    }
}
```

### **3. Patrón de Acceso Restringido (Access Restriction)**

Este patrón se utiliza para restringir el acceso a ciertas funciones del contrato a ciertos usuarios. Es comúnmente implementado a través de modificadores de función.

```solidity
contract RestrictedAccess {
    address public owner = msg.sender;

    modifier onlyOwner() {
        require(msg.sender == owner);
        _;
    }

    function restrictedAction() public onlyOwner {
        // Lógica restringida
    }
}
```

De la misma forma que en este caso se restringe el acceso para que solo el Owner pueda acceder a la función restringida, se puede crear modificadores para otros perfiles como administradores o personas autorizadas.

### **4. Patrón de Parada de Emergencia (Emergency Stop)**

Este patrón también conocido como "Circuit Breaker" permite pausar la ejecución de ciertas funciones críticas en caso de emergencia o cuando se detecta un comportamiento anómalo. Este patrón es particularmente útil para prevenir daños mayores, como la pérdida de fondos o la explotación de vulnerabilidades, hasta que se pueda investigar y resolver el problema.

Para implementar este patrón, se suele utilizar una variable de estado que indica si el contrato está en modo "pausado" o no. Las funciones que podrían ser vulnerables a ataques o fallos se modifican para que su ejecución dependa del estado de esta variable. Además, se implementan funciones para cambiar el estado de esta variable, permitiendo activar o desactivar el "modo de emergencia". Estas funciones de control deben ser restringidas a direcciones autorizadas para evitar mal uso.

A continuación un ejemplo simple de cómo implementarlo:

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract EmergencyStopExample {
    address private owner;
    bool private paused;

    modifier onlyOwner() {
        require(msg.sender == owner, "No eres el propietario");
        _;
    }

    modifier whenPaused() {
        require(paused, "El contrato no está pausado");
        _;
    }

    event Paused();
    event Unpaused();
		event FundsWithdrawn(address owner, uint amount);
		
    constructor() {
        owner = msg.sender;
        paused = false;
    }

    function pause() public onlyOwner {
        paused = true;
        emit Paused();
    }

    function unpause() public onlyOwner {
        paused = false;
        emit Unpaused();
    }

    function emergencyWithdraw() public onlyOwner whenPaused {
        // Suponiendo que esta función retira todos los fondos del contrato a la dirección del propietario
        uint balance = address(this).balance;
        (bool sent, ) = owner.call{value: balance}("");
        require(sent, "Fallo al enviar Ether");
        emit FundsWithdrawn(owner, balance);
    }

    // Otras funciones del contrato deben también verificar el estado de `paused` usando el modificador `whenNotPaused`
}
```

#### **Consideraciones:**

* **Autorización:** Es crucial restringir quién puede activar o desactivar el modo de emergencia, generalmente el propietario del contrato o un conjunto de direcciones confiables.
* **Transparencia:** La capacidad de poner el contrato en modo de emergencia debe ser comunicada claramente a los usuarios, explicando en qué circunstancias se utilizará.
* **Recuperación:** Debe haber un plan claro sobre cómo se resolverán los problemas que llevaron a activar el modo de emergencia y cómo se reanudará la normalidad.

### **5. Patrón de Fábrica de Contratos (Factory Pattern)**

Este patrón permite la creación de nuevos contratos desde otro contrato. Este patrón es especialmente útil cuando se necesita crear múltiples instancias de un contrato con configuraciones similares o cuando se desea centralizar la lógica de creación de contratos para facilitar el mantenimiento y la actualización. La "fábrica" actúa como un creador centralizado que puede generar instancias de contratos a petición.

A continuación, se muestra un ejemplo simple de cómo implementar el patrón Factory Contract. En este ejemplo, el contrato **`ChildContract`** será el contrato que la fábrica produce, y **`FactoryContract`** actuará como la fábrica que crea instancias de **`ChildContract`**.

#### **Contrato Hijo**

Primero, definimos el contrato que queremos producir. Este contrato puede ser cualquier cosa, pero para fines de este ejemplo, será un simple contrato que almacena un número.

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

// Contrato hijo que nuestra fábrica producirá
contract ChildContract {
    uint public number;

    // Constructor que inicializa el contrato con un número específico
    constructor(uint _number) {
        number = _number;
    }
}
```

**Contrato Fábrica**

El contrato fábrica se encargará de crear nuevas instancias del contrato hijo. Mantendrá un registro de todas las instancias creadas para poder interactuar con ellas más tarde si es necesario.

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

// Referencia al contrato hijo
import "./ChildContract.sol";

contract FactoryContract {
    // Array para almacenar las direcciones de los contratos hijos creados
    ChildContract[] public children;

    event ChildCreated(uint number, address childAddress);

    // Función para crear un nuevo contrato hijo
    function createChild(uint _number) public {
        ChildContract child = new ChildContract(_number);
        children.push(child);
        emit ChildCreated(_number, address(child));
    }

    // Función para obtener la dirección de un contrato hijo en el array
    function getChild(uint _index) public view returns (ChildContract) {
        require(_index < children.length, "Índice fuera de límites");
        return children[_index];
    }
}
```

En este ejemplo, **`FactoryContract`** tiene una función **`createChild`** que despliega una nueva instancia de **`ChildContract`** con un número específico. Cada nueva instancia de **`ChildContract`** se almacena en el array **`children`**, y se emite un evento **`ChildCreated`** que registra el número asignado y la dirección del nuevo contrato hijo.

Este patrón es poderoso porque permite la creación de contratos de manera programática, lo que facilita la gestión de múltiples instancias de contratos y la centralización de la lógica de creación de contratos. Además, al tener un registro de todos los contratos creados, el contrato fábrica puede interactuar con estos o proporcionar funcionalidades adicionales como la gestión o actualización de los contratos hijos.

### **6. Patrón de Máquina de Estados (State Machine)**

Este patrón es una forma eficaz de gestionar el ciclo de vida de un contrato, modelando explícitamente los diferentes estados por los que puede pasar un contrato y las transiciones permitidas entre estos estados. Este patrón es particularmente útil para contratos con lógicas de negocio complejas que necesitan manejar diferentes fases o etapas de manera clara y segura, como contratos de votación, de subastas, o de crowdfunding.

#### **Implementación básica del patrón State Machine**

Para implementar una máquina de estados en un contrato inteligente, puedes seguir estos pasos:

1. **Definir los estados:** Utiliza un **`enum`** para definir todos los posibles estados del contrato.
2. **Almacenar el estado actual:** Utiliza una variable para almacenar el estado actual del contrato.
3. **Restringir funciones a estados específicos:** Usa modificadores para permitir que ciertas funciones se ejecuten solo en estados específicos.
4. **Cambiar de estado:** Implementa funciones que cambien el estado del contrato de manera controlada.

Veamos un ejemplo simplificado de un contrato de subasta que utiliza el patrón State Machine:

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract AuctionStateMachine {
    enum Stages {
        AcceptingBlindBids,
        RevealBids,
        Finished
    }

    // Variable para almacenar el estado actual del contrato
    Stages public stage = Stages.AcceptingBlindBids;

    // Variable para almacenar el tiempo en el que el contrato avanza al siguiente estado
    uint public nextStageTime = block.timestamp + 1 days;

    // Modificador para controlar el acceso a funciones según el estado actual
    modifier atStage(Stages _stage) {
        require(stage == _stage, "Función no permitida en el estado actual.");
        _;
    }

    // Modificador para avanzar al siguiente estado basado en el tiempo
    modifier transitionNext() {
        if (block.timestamp >= nextStageTime) {
            nextStage();
        }
        _;
    }

    // Función para avanzar al siguiente estado
    function nextStage() internal {
        stage = Stages(uint(stage) + 1);
        nextStageTime = block.timestamp + 1 days;
    }

    // Ejemplo de función que puede ser llamada en un estado específico
    function acceptBlindBid() public atStage(Stages.AcceptingBlindBids) transitionNext {
        // Lógica para aceptar una puja a ciegas
    }

    function revealBids() public atStage(Stages.RevealBids) transitionNext {
        // Lógica para revelar las pujas
    }

    // Lógica para finalizar la subasta, se puede implementar en el cambio a `Finished`
}
```

En este contrato de subasta, hay tres estados definidos: **`AcceptingBlindBids`** (aceptando pujas a ciegas), **`RevealBids`** (revelando pujas), y **`Finished`** (finalizada). El contrato comienza en el estado **`AcceptingBlindBids`** y avanza secuencialmente a través de los estados basado en el paso del tiempo (cada día avanza al siguiente estado). Se utilizan modificadores para asegurar que ciertas funciones solo puedan ejecutarse en sus respectivos estados, y hay una función **`nextStage`** que gestiona la transición entre estados.

#### **Beneficios del patrón State Machine**

* **Claridad:** Facilita la comprensión del flujo lógico del contrato y sus distintas fases.
* **Seguridad:** Ayuda a prevenir ejecuciones no autorizadas de funciones y asegura que el contrato solo realice operaciones válidas en su estado actual.
* **Flexibilidad:** Permite gestionar complejidades en contratos con múltiples fases o etapas de manera estructurada.

### **7. Patrón para obtener datos de un oráculo**

El uso de oráculos en contratos inteligentes de Ethereum permite acceder a datos externos a la blockchain, lo cual es esencial para muchas aplicaciones descentralizadas (DApps) que necesitan interactuar con el mundo real. Sin embargo, Ethereum y otras blockchains similares no pueden acceder directamente a datos externos debido a su naturaleza aislada y determinista. Aquí es donde entran en juego los oráculos, actuando como intermediarios que traen información del exterior hacia la blockchain.

Un oráculo es un servicio (generalmente operado por un tercero) que envía datos del mundo real a la blockchain. Estos datos pueden ser cualquier cosa, desde precios de activos y resultados deportivos hasta la temperatura de una ciudad. Los oráculos juegan un papel crucial en el funcionamiento de muchos tipos de DApps, como las financieras (DeFi), las de seguros, y las de juegos de azar, entre otras.

#### **Implementación Básica**

La implementación de un oráculo en Solidity generalmente sigue estos pasos:

1. **Solicitud de Datos:** El contrato inteligente envía una solicitud de datos al oráculo. Esta solicitud puede ser el resultado de una acción del usuario o de otra función del contrato.
2. **Obtención de Datos:** El oráculo recibe la solicitud, obtiene los datos necesarios del mundo real y los envía de vuelta a la blockchain.
3. **Procesamiento de Datos:** El contrato inteligente recibe los datos y ejecuta la lógica correspondiente con esta información.

#### **Ejemplo con Chainlink**

Chainlink es uno de los servicios de oráculo más populares y ampliamente utilizados en el ecosistema Ethereum. A continuación, se muestra un ejemplo básico de cómo un contrato inteligente puede interactuar con Chainlink para obtener el precio de ETH en USD.

Primero, asegúrate de tener importadas las dependencias de Chainlink en tu archivo Solidity, lo cual generalmente se hace a través de importaciones de NPM o directamente con URLs.

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

import "@chainlink/contracts/src/v0.8/interfaces/AggregatorV3Interface.sol";

contract PriceConsumerV3 {

    AggregatorV3Interface internal priceFeed;

    /**
     * Network: Ethereum Mainnet
     * Aggregator: ETH/USD
     * Address: 0x... (Dirección del contrato de Chainlink para ETH/USD)
     */
    constructor() {
        priceFeed = AggregatorV3Interface(0x...); // Dirección del oráculo de Chainlink para ETH/USD
    }

    /**
     * Retorna el precio más reciente de ETH/USD
     */
    function getLatestPrice() public view returns (int) {
        (
            /* uint80 roundID */,
            int price,
            /* uint startedAt */,
            /* uint timeStamp */,
            /* uint80 answeredInRound */
        ) = priceFeed.latestRoundData();
        return price; // El precio se devuelve con 8 decimales
    }
}
```

En este ejemplo, el contrato **`PriceConsumerV3`** utiliza la interfaz **`AggregatorV3Interface`** de Chainlink para interactuar con un contrato oráculo de Chainlink que proporciona el precio actual de ETH en USD. La función **`getLatestPrice`** consulta el último dato de precio disponible y lo devuelve.

Cuando se trabaja con oráculos, es crucial tener en cuenta la confiabilidad y la seguridad del proveedor de datos. Dependiendo de un único oráculo puede introducir un punto de fallo centralizado. Para mitigar esto, algunas aplicaciones utilizan múltiples oráculos o servicios de oráculos descentralizados como Chainlink, que agregan datos de varias fuentes para proporcionar una medida más confiable y resistente a la manipulación.

### **8. Patrón de Actualización de Contratos (Upgradeable Contracts)**

Este patrón comúnmente conocido como el patrón "Proxy" o "Upgradeable Contracts", es una técnica avanzada que permite a los desarrolladores cambiar el código de un contrato inteligente después de haber sido desplegado en la blockchain. Dado que el código de un contrato inteligente es inmutable una vez desplegado, este patrón proporciona una forma flexible de actualizar la lógica del contrato sin perder el estado o los datos almacenados, ni cambiar la dirección del contrato.

#### **Cómo Funciona el Patrón Proxy**

El patrón se basa en dos componentes principales: el contrato Proxy y el contrato de Implementación (o lógica).

* **Contrato Proxy:** Es el contrato que los usuarios interactúan directamente. Mantiene el estado del contrato y delega llamadas a un contrato de implementación que contiene la lógica del negocio. La dirección del contrato proxy permanece constante, incluso a través de las actualizaciones.
* **Contrato de Implementación:** Contiene la lógica del negocio y el código que puede ser actualizado. Cuando se actualiza la lógica del contrato, se despliega un nuevo contrato de implementación, y el proxy es actualizado para apuntar a la nueva dirección.

A continuación, se muestra un ejemplo simplificado de cómo se podría implementar un patrón Proxy.

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

// Contrato de Implementación
contract LogicContractV1 {
    uint public counter;

    function increment() public {
        counter += 1;
    }
}

// Contrato Proxy
contract Proxy {
    address public implementation;

    constructor(address _logic) {
        implementation = _logic;
    }

    function upgrade(address _newImplementation) external {
        implementation = _newImplementation;
    }

    fallback() external payable {
        address _impl = implementation;
        require(_impl != address(0));
        assembly {
            let ptr := mload(0x40)
            calldatacopy(ptr, 0, calldatasize())
            let result := delegatecall(gas(), _impl, ptr, calldatasize(), 0, 0)
            let size := returndatasize()
            returndatacopy(ptr, 0, size)
            switch result
            case 0 { revert(ptr, size) }
            default { return(ptr, size) }
        }
    }
}
```

En este ejemplo, el **`Proxy`** delega todas las llamadas al **`LogicContractV1`**. Si queremos actualizar la lógica, desplegaríamos un nuevo contrato de implementación (**`LogicContractV2`**, por ejemplo) y luego llamaríamos a **`upgrade`** en el **`Proxy`** para cambiar la dirección de la implementación.

#### **Consideraciones de Seguridad**

* **Almacenamiento consistente:** Es crucial asegurarse de que la estructura del almacenamiento permanezca consistente entre las versiones de los contratos de implementación para evitar problemas de corrupción de datos.
* **Transparencia de las actualizaciones:** Las actualizaciones deben ser manejadas con cuidado para mantener la confianza de los usuarios, idealmente mediante un proceso de gobernanza claro o un período de tiempo de aviso antes de realizar cambios significativos.
* **Control de acceso:** El contrato Proxy debe tener controles de acceso robustos para asegurar que solo las entidades autorizadas puedan actualizar el contrato de implementación.

### **9. Patrón de Mapa Iterable**

Implementar un mapa iterable en Solidity permite combinar las ventajas de los mapas (acceso rápido a los datos por clave) con las de los arrays (capacidad de iterar sobre los elementos). Dado que Solidity no ofrece de forma nativa una estructura de datos que sea al mismo tiempo un mapa y que permita la iteración sobre sus elementos, se debe diseñar una estructura que cumpla con ambos requisitos.

A continuación veamos una implementación detallada del patrón de mapa iterable, el cual se compone de un mapa para almacenar los datos y un array para mantener el orden de las claves y permitir la iteración.

#### **Implementación**

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract IterableMapping {
    // Estructura para almacenar los datos junto con un flag para marcar si el elemento está en el array
    struct MapData {
        uint value;
        bool exists;
    }

    // Mapa para almacenar los datos asociados a una clave
    mapping(address => MapData) public dataMap;

    // Array para mantener el orden de las claves
    address[] public keys;

    // Función para añadir o actualizar un valor
    function set(address key, uint value) public {
       // Si es la primera vez que se añade, también se agrega la clave al array
        if (!dataMap[key].exists) {
            keys.push(key);
            dataMap[key] = MapData(value, true);
        } else {
            // Si ya existía, solo actualizamos el valor
            dataMap[key].value = value;
        }
    }

    // Función para obtener un valor
    function get(address key) public view returns (uint) {
        require(dataMap[key].exists, "La clave no existe");
        return dataMap[key].value;
    }

    // Función para eliminar un elemento
    function remove(address key) public {
        require(dataMap[key].exists, "La clave no existe");

        // Eliminar la clave del mapa
        delete dataMap[key];

        // Encontrar el índice de la clave en el array y eliminarlo
        for (uint i = 0; i < keys.length; i++) {
            if (keys[i] == key) {
                // Mover el último elemento al lugar del elemento eliminado y luego eliminar el último elemento
                keys[i] = keys[keys.length - 1];
                keys.pop();
                break;
            }
        }
    }

    // Función para obtener el tamaño del mapa
    function size() public view returns (uint) {
        return keys.length;
    }

    // Función para comprobar si una clave existe en el mapa
    function exists(address key) public view returns (bool) {
        return dataMap[key].exists;
    }

    // Función para obtener una clave en un índice específico
    function getKeyAtIndex(uint index) public view returns (address) {
        require(index < keys.length, "Índice fuera de rango");
        return keys[index];
    }
}
```

#### **Características y consideraciones**

* **Flexibilidad:** Este patrón permite tanto la recuperación de valores por clave como la iteración sobre todas las entradas.
* **Gestión de estado:** Se mantiene la consistencia entre el mapa y el array, asegurando que las operaciones de añadir, actualizar y eliminar se reflejen en ambos.
* **Eficiencia de Gas:** La eliminación y la adición de elementos, especialmente en un conjunto grande, pueden consumir una cantidad significativa de gas debido a las operaciones sobre el array. La eficiencia debe ser considerada al diseñar el contrato, especialmente para operaciones que modifican el array.

Este patrón es especialmente útil en situaciones donde se necesita tanto un acceso rápido a los datos mediante claves como la capacidad de iterar sobre todas las entradas almacenadas.

### **10. Patrón de Lista de Direcciones**

Este patrón se utiliza para mantener una lista curada de direcciones por el propietario. Esto se requiere por ejemplo cuando se necesita una lista de direcciones en una whitelist que estén autorizadas para ejecutar determinadas funciones en un contrato. En este patrón, solo el dueño del contrato puede añadir y retirar direcciones de la lista.

Veamos un ejemplo.

```solidity
contract AddressList is Ownable {
		 mapping(address => bool) internal map;
		 function add(address _address) public onlyOwner {
		        map[_address] = true;
		    }

		 function remove(address _address) public onlyOwner {
		        map[_address] = false;
		    }

		 function isExists(address _address) public view returns (bool) {
		        return map[_address];
    }
}
```

### **11. Patrón de Comparación de Strings**

En Solidity, los strings son un tipo de dato especial que representa secuencias de caracteres. Sin embargo, Solidity no proporciona una forma directa de comparar strings debido a su naturaleza de bajo nivel y su enfoque en la eficiencia de gas. Comparar strings implica comparar el contenido de los datos en lugar de las direcciones de memoria, lo cual requiere una lógica específica dada la forma en que se almacenan los strings en la EVM.

Una forma común de comparar strings en Solidity es convertirlos primero a **`bytes`** y luego comparar estos **`bytes`** usando **`keccak256`**. Si dos strings son idénticos, sus hashes **`keccak256`** también serán idénticos. Este método es eficiente y efectivo para la comparación de strings en contratos inteligentes.

A continuación, se muestra cómo implementar la comparación de strings usando **`keccak256`**:

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract StringComparison {
    // Función para comparar dos strings
    function compareStrings(string memory string1, string memory string2) public pure returns (bool) {
        return keccak256(abi.encodePacked(string1)) == keccak256(abi.encodePacked(string2));
    }
}
```

* **`abi.encodePacked(...)`**: Esta función toma cualquier número de argumentos de cualquier tipo y los codifica en un único **`bytes`** continuo. Se usa aquí para convertir los strings en **`bytes`**.
* **`keccak256(...)`**: Calcula el hash KECCAK-256 de los datos. En este contexto, se usa para obtener el hash del **`bytes`** resultante de cada string.
* **`==`**: Compara los hashes de los dos strings. Si los strings son iguales, sus hashes también lo serán, resultando en **`true`**. De lo contrario, el resultado será **`false`**.

#### **Consideraciones**

* **Eficiencia de Gas**: Usar **`keccak256`** para comparar strings es generalmente eficiente en términos de gas, especialmente comparado con métodos que implicarían iterar sobre cada carácter de los strings.
* **Colisiones**: Teóricamente, las funciones hash pueden tener colisiones (diferentes entradas produciendo el mismo hash), aunque la probabilidad de esto es extremadamente baja con **`keccak256`**.
* **Limitaciones**: Este método compara el contenido completo de los strings. Si necesitas realizar comparaciones más complejas (como comprobaciones de prefijo o patrones), necesitarás una lógica adicional.
