# Open Zeppelin

OpenZeppelin es un proyecto de código abierto que ofrece una suite de herramientas seguras y reutilizables para el desarrollo de contratos inteligentes en la blockchain de Ethereum y otras blockchains compatibles con la EVM. Es ampliamente reconocido y utilizado por desarrolladores de todo el mundo para construir, desplegar y operar aplicaciones descentralizadas (DApps) y tokens digitales, incluyendo tokens fungibles (como los que siguen el estándar ERC-20) y no fungibles (NFTs, siguiendo el estándar ERC-721 y ERC-1155).

### **Principales Características de las Librerías de OpenZeppelin**

* **Seguridad:** Las librerías de OpenZeppelin son revisadas y auditadas minuciosamente por la comunidad y profesionales en seguridad blockchain, lo que ayuda a prevenir errores comunes y vulnerabilidades de seguridad en los contratos inteligentes.
* **Reusabilidad:** Proporcionan módulos y contratos inteligentes predefinidos que cubren funcionalidades comunes y patrones de diseño en el desarrollo de DApps y tokens, como la emisión de tokens, votaciones, y gestión de acceso.
* **Comunidad:** OpenZeppelin tiene una comunidad activa y en crecimiento, lo que garantiza que las herramientas estén constantemente actualizadas y mejoradas basándose en las necesidades y retroalimentación de sus usuarios.

### **Componentes de OpenZeppelin**

### **1. Tokens**

* **ERC20:** Implementaciones estándar del popular token fungible ERC-20, incluyendo versiones básicas, detalladas, minteables, quemables, con capacidades de pausa y más.
* **ERC721:** Implementaciones para tokens no fungibles (NFTs), siguiendo el estándar ERC-721, con soporte para enumeración, metadata extendida, y transferencias seguras.
* **ERC1155:** Un contrato más avanzado que soporta la creación de tokens tanto fungibles como no fungibles dentro de un único contrato, optimizando la gestión y transferencia de múltiples tipos de tokens.

### **2. Acceso**

* **Ownable:** Un contrato simple para proporcionar un control básico de acceso basado en un único dueño, útil para operaciones administrativas.
* **AccessControl:** Un sistema de control de acceso más flexible y completo que permite definir roles y permisos complejos para diferentes acciones dentro de tu contrato.

### **3. Seguridad**

* **Pausable:** Permite pausar y despausar ciertas funcionalidades del contrato, útil para detener operaciones en caso de emergencia o mantenimiento.
* **ReentrancyGuard:** Ayuda a prevenir ataques de reentrancia, uno de los vectores de ataque más comunes, mediante el uso de un modificador simple.

### **4. Gobernanza**

* **Governor:** Es el corazón del sistema de gobernanza de OpenZeppelin. Proporciona una base sólida sobre la cual se pueden construir mecanismos de votación y gobernanza, ofreciendo funcionalidades como la creación y gestión de propuestas, votación, y ejecución de decisiones aprobadas. Este contrato puede ser extendido y personalizado para adaptarse a las necesidades específicas de diferentes proyectos.
* **Timelock:** Añade un elemento de seguridad y transparencia al proceso de gobernanza al introducir un período de bloqueo entre la aprobación de una propuesta y su ejecución. Esto da tiempo a los participantes para revisar las decisiones aprobadas y, si es necesario, actuar en consecuencia antes de que se implementen cambios significativos.
* **Votes:** Proporciona funcionalidades para el conteo y delegación de votos, permitiendo a los titulares de tokens participar en el proceso de gobernanza. Esto incluye soporte para tokens con capacidades de voto, como los tokens ERC20 con extensiones que permiten la votación.
* **Compounded Votes:** Permite a los votantes acumular poder de voto a lo largo del tiempo o en función de otros factores, añadiendo una dimensión dinámica y estratégica al proceso de votación.

### **5. Finanzas**

* **PaymentSplitter:** Permite dividir los ingresos de ether entre múltiples cuentas, útil para dividir pagos o ingresos de forma transparente y segura.
* **Escrow:** Un contrato de depósito en garantía básico que puede ser extendido para crear mecanismos de pago seguros y confiables entre partes.

### **6. Utilidades**

* **Counters:** Proporciona una manera segura de incrementar y decrementar contadores, útil para llevar un registro de IDs de tokens, cantidades de elementos, entre otros.
* **Address:** Ofrece funciones para realizar operaciones seguras con direcciones de Ethereum, incluyendo la verificación de que una dirección es un contrato.
* **MerkleProof:** Se usa para verificar si un elemento pertenece a un árbol de Merkle dado una raíz y una prueba (conjunto de hashes).
* **EnumerableMap:** Cuando se necesita un mapping cuyos elementos puedan ser listados o contados, como el seguimiento de un conjunto de elementos únicos con identificadores.
* **SafeCast:** Cuando se requiere cambiar el tipo de una variable numérica a otro tipo con un tamaño menor de almacenamiento, asegurando que el valor se ajusta al nuevo tipo sin causar errores.

### **7. Proxies**

* **TransparentProxy y UUPS:** Ofrecen infraestructuras para desplegar y administrar contratos inteligentes que pueden ser actualizados, permitiendo cambiar la lógica de un contrato sin cambiar la dirección del contrato.

Esta lista es solo una referencia de los contratos disponibles en Open Zeppelin. Para revisar la lista completa ingresar a: [https://docs.openzeppelin.com/contracts/5.x/](https://docs.openzeppelin.com/contracts/5.x/) sección API.

### **Cómo Usar OpenZeppelin**

Para utilizar las librerías de OpenZeppelin en tu proyecto, primero debes instalarlas. Si estás utilizando un entorno de desarrollo basado en Node.js, puedes instalarlas fácilmente a través de npm o yarn. Aquí tienes un ejemplo de cómo instalar la librería de contratos OpenZeppelin mediante npm:

```bash
npm install @openzeppelin/contracts
```

Una vez instalada, puedes importar y utilizar los contratos y módulos de OpenZeppelin en tus contratos inteligentes. Por ejemplo, para crear un token ERC-20:

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

import "@openzeppelin/contracts/token/ERC20/ERC20.sol";

contract MyToken is ERC20 {
    constructor() ERC20("MyToken", "MTK") {
        _mint(msg.sender, 1000000 * (10 ** uint256(decimals())));
    }
}
```

Desde Remix también puedes acceder a las librerías directamente utilizando la URL de GitHub. Para ello debes incluir una directiva de importación en la parte superior de tu archivo de contrato Solidity que especifique la ruta de la biblioteca en el repositorio de GitHub de OpenZeppelin.

Por ejemplo, para importar la implementación ERC-20 de OpenZeppelin, tu directiva de importación se vería así:

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

import "<https://github.com/OpenZeppelin/openzeppelin-contracts/blob/master/contracts/token/ERC20/ERC20.sol>";

contract MyToken is ERC20 {
    constructor() ERC20("MyToken", "MTK") {
        _mint(msg.sender, 1000 * (10 ** uint256(decimals())));
    }
}
```

Asegúrate de que la versión de Solidity especificada en tu contrato sea compatible con la versión utilizada por los contratos de OpenZeppelin que estás importando.

Una forma más sencilla de obtener los contratos ERC20 desde Remix es directamente desde el botón de la pantalla principal de Remix.

<figure><img src="../../.gitbook/assets/Modulo 3-3.png" alt=""><figcaption></figcaption></figure>

Para conocer en detalle los contratos y utilidades disponibles en Open Zeppelin te sugerimos revisar [su GitHub](https://github.com/OpenZeppelin). También puedes revisar [este video](https://www.youtube.com/live/mP4jJ8SwtqU?si=fwdxx7A9OwMz8QlP) explicativo en español.
