# Pruebas manuales

Las pruebas manuales son asistidas por humanos e implican ejecutar cada caso de prueba en la suite de pruebas uno tras otro. Son diferentes de las pruebas automatizadas, donde se puede ejecutar simultáneamente múltiples pruebas distintas en un contrato y obtener un informe que muestre todas las pruebas fallidas y exitosas.

Las pruebas manuales efectivas requieren considerables recursos (habilidad, tiempo, dinero y esfuerzo) y sin embargo eso no implica que se deje de pasar por alto ciertos errores al ejecutarlas. Pero las pruebas manuales también pueden ser beneficiosas: por ejemplo, un tester humano (por ejemplo, un auditor) puede usar la intuición para detectar casos límite que una herramienta de pruebas automatizadas pasaría por alto.

Las pruebas manuales de contratos inteligentes a menudo se realizan al final en el ciclo de desarrollo después de ejecutar pruebas automatizadas. Esta forma de prueba evalúa el contrato inteligente como un producto completamente integrado para ver si funciona según lo especificado en los requisitos técnicos.

Las pruebas manuales se hacen normalmente en blockchains locales o en redes de prueba.

**Probar contratos en una blockchain local**

Si bien las pruebas automatizadas realizadas en un entorno de desarrollo local pueden proporcionar información útil de depuración, siempre nos interesará saber cómo se comporta tu contrato inteligente en un entorno de producción. Sin embargo, implementar en mainnet de Ethereum conlleva tarifas de gas, sin mencionar que los activos que gestiona el contrato se pueden perder si el contrato inteligente aún tiene errores.

Probar tu contrato en una blockchain local es una alternativa recomendada a probar en mainnet. Una blockchain local es una copia de la blockchain de Ethereum que se ejecuta localmente en tu computadora y simula el comportamiento de la capa de ejecución de Ethereum. Como tal, puedes programar transacciones para interactuar con un contrato sin incurrir en costos significativos.

**Probar contratos en redes de prueba**

Una red de prueba o testnet funciona exactamente como la mainnet de Ethereum, excepto que usa Ether (ETH) sin valor en el mundo real. Implementar tu contrato en una testnet como Sepeoli significa que cualquiera puede interactuar con él (por ejemplo, a través del frontend de la dapp) sin poner en riesgo fondos.

Esta forma de prueba manual es útil para evaluar el flujo de extremo a extremo de tu aplicación desde el punto de vista del usuario. Aquí, los beta testers también pueden realizar pruebas y reportar cualquier problema con la lógica de negocio del contrato y la funcionalidad general.

Implementar en una testnet después de probar en una blockchain local es ideal, ya que la primera está más cerca del comportamiento de la EVM de Ethereum. Por lo tanto, es común que muchos proyectos nativos de Ethereum implementen dapps en testnets para evaluar la operación de contratos inteligentes bajo condiciones del mundo real.
