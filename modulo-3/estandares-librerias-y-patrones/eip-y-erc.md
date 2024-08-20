# EIP y ERC

### **EIP: Ethereum Improvement Proposal**

Un EIP, o Propuesta de Mejora de Ethereum, es un documento de diseño que proporciona información a la comunidad de Ethereum o describe una nueva característica, procesos o cambios en el entorno. Los EIPs son el principal mecanismo para proponer nuevas características, recoger comentarios de la comunidad y documentar los cambios en Ethereum de una manera formal. Los EIPs cubren cambios técnicos como modificaciones en el protocolo core de Ethereum, estándares para contratos inteligentes, y cambios en las prácticas de desarrollo.

Los EIPs se dividen en tres categorías principales:

1. **EIPs Estándar:** Propuestas que afectan de manera directa el protocolo de Ethereum, incluyendo cambios en el funcionamiento de la blockchain y estándares para contratos inteligentes (como los tokens ERC).
2. **EIPs Informativos:** Documentos que proporcionan directrices generales o información a la comunidad de Ethereum, pero no proponen cambios en el protocolo per se.
3. **EIPs Meta:** Propuestas que describen procesos o cambios que no necesariamente son técnicos, como procedimientos y pautas para la gobernanza de Ethereum.

Algunos ejemplo de EIP recientes de alto impacto son los siguientes:

* **EIP-1559:** Propone una reforma significativa del mecanismo de tarifas en Ethereum, introduciendo un precio base por gas y quemando parte del ether utilizado en las tarifas de transacción. Este cambio busca mejorar la predictibilidad de las tarifas y reducir su volatilidad. Ha modificado cómo los usuarios pagan por las transacciones y cómo se calculan las tarifas, además de introducir un mecanismo de quema de ETH que afecta la emisión y, potencialmente, el valor de ETH a largo plazo (Ultra Sound Money).
* **EIP-4844:** Conocido comúnmente como "Proto-Danksharding", es una propuesta que tiene como objetivo mejorar la escalabilidad de Ethereum mediante la introducción de blobs de datos. Estos blobs son grandes bloques de datos que se pueden añadir a la blockchain sin requerir el mismo nivel de procesamiento por parte de los nodos, como sería necesario para los datos de transacción normales. Proto-Danksharding está diseñado para ser un paso intermedio hacia la implementación completa del Danksharding. Su implementación pretende aliviar los cuellos de botella de escalabilidad , haciendo a Ethereum más escalable y reduciendo los costos de transacción en las L2s.
* **EIP-4833:** Propone una solución de Account Abstraction que permite una mayor flexibilidad en la gestión de cuentas en Ethereum. La abstracción de cuentas es un concepto que busca hacer que las cuentas controladas por contratos inteligentes (cuentas de contrato) y las cuentas externamente controladas (EOA) sean más interoperables y flexibles. Básicamente, permite que las cuentas de usuario se comporten más como contratos inteligentes, habilitando capacidades como la autorización de transacciones mediante reglas de contrato inteligente, pago de tarifas de gas en tokens diferentes a ETH, y la creación de sistemas de recuperación de cuentas más robustos.

### **ERC: Ethereum Request for Comments**

Un ERC, o Solicitud de Comentarios de Ethereum, es un subconjunto de los EIPs que se enfoca en definir nuevos estándares, principalmente para contratos inteligentes. Los ERCs establecen reglas claras y específicas que deben seguir los contratos inteligentes para asegurar la interoperabilidad entre aplicaciones en el ecosistema Ethereum. Esto incluye, por ejemplo, estándares para tokens fungibles y no fungibles, mecanismos de intercambio, y más.

Algunos de los ERCs más conocidos incluyen:

* **ERC-20:** El estándar de facto para tokens fungibles en Ethereum, permitiendo la interoperabilidad entre tokens. Define un conjunto común de reglas para los tokens dentro de contratos inteligentes, facilitando su implementación y compatibilidad.
* **ERC-721:** Un estándar para tokens no fungibles (NFTs), permitiendo la representación de activos únicos y la propiedad en la blockchain.
* **ERC-1155:** Un estándar más avanzado que permite la creación tanto de tokens fungibles como no fungibles dentro del mismo contrato, optimizando las transacciones y el almacenamiento de datos.

Los ERCs son esenciales para el desarrollo de aplicaciones descentralizadas (DApps) en Ethereum, proporcionando un marco estandarizado que asegura la compatibilidad y la funcionalidad a través de diferentes proyectos y plataformas.
