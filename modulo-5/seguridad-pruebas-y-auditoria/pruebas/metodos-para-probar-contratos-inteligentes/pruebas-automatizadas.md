# Pruebas automatizadas

#### Pruebas automatizadas

Las pruebas automatizadas utilizan herramientas que verifican automáticamente el código de un contrato inteligente en busca de errores en la ejecución. El beneficio de las pruebas automatizadas proviene del uso de scripts para guiar la evaluación de las funcionalidades del contrato. Las pruebas con scripts pueden programarse para ejecutarse repetidamente con mínima intervención humana, haciendo que las pruebas automatizadas sean más eficientes que las manuales.

Las pruebas automatizadas son particularmente útiles cuando las pruebas son repetitivas y consumen mucho tiempo, son difíciles de realizar manualmente, son susceptibles a errores humanos o implican evaluar funciones críticas del contrato. Pero las herramientas de pruebas automatizadas pueden tener desventajas: pueden pasar por alto ciertos errores y generar muchos [f](https://www.contrastsecurity.com/glossary/false-positive)alsos positivos. Por lo tanto, combinar pruebas automatizadas con pruebas manuales para contratos inteligentes es ideal.

**Tipos de pruebas automatizadas**

1.  **Pruebas unitarias**: Se enfocan principalmente en funciones y componentes individuales. Cada función opera en base a ciertas suposiciones sobre los parámetros que recibe y el estado del sistema con el que interactúa. Las pruebas unitarias deben, por lo tanto, abarcar todos los posibles escenarios, tanto válidos (qué debe hacer el contrato) como inválidos (qué no debe hacer el contrato), en los que una función puede ser llamada para descubrir errores de lógica.

    Ten en cuenta las siguientes recomendaciones para realizar las pruebas unitarias.

    * Ten en claro cuáles son las funciones del contrato y cómo los usuarios accederán a ellas y las utilizarán.
    * Evalúa todos los supuestos relacionados con la ejecución del contrato. ¿Qué puede hacer y qué no puede hacer el contrato?
    * Mide la cobertura del código. Mientras más, mejor.
    * Usa herramientas de prueba robustas y bien posicionadas en el mercado.
2.  **Pruebas de integración**:  Forman la siguiente capa de pruebas, construyéndose sobre las pruebas unitarias. Tras confirmar la funcionalidad de los componentes individuales, se verifica su integración. Este nivel se enfoca en las interacciones entre componentes, internos o externos. Por ejemplo, comprueba si un sistema se comporta correctamente al llamar a un sistema de terceros. Las pruebas de integración son esenciales ya que estas relaciones no se prueban en las pruebas unitarias.

    Las pruebas de integración son útiles si el contrato adopta una arquitectura modular o interactúa con otros contratos onchain durante la ejecución. Una forma de realizar pruebas de integración es hacer un fork de mainnet en una altura específica utilizando herramientas como Hardhat y Foundry y simular interacciones entre nuestros contratos y los contratos desplegados.

    El fork se comportará de manera similar a mainnet y tendrá cuentas con estados y saldos asociados. Pero solo actúa como un entorno de desarrollo local aislado, lo que significa que no necesitarás ETH real para las transacciones, por ejemplo, ni tus cambios afectarán el entorno real de Ethereum.
3.  **Pruebas basadas en propiedades:** Las pruebas basadas en propiedades son el proceso de verificar que un contrato inteligente cumpla con alguna propiedad definida. Las propiedades afirman hechos sobre el comportamiento de un contrato que se espera que se mantengan verdaderos en diferentes escenarios. Un ejemplo de una propiedad de un contrato inteligente podría ser "Las operaciones aritméticas en el contrato nunca harán overflow ni underflow."

    El análisis estático y el análisis dinámico son dos técnicas comunes para ejecutar pruebas basadas en propiedades, y ambas pueden verificar que el código de un contrato cumpla con alguna propiedad predefinida. Algunas herramientas de pruebas basadas en propiedades vienen con reglas predefinidas sobre las propiedades esperadas del contrato y verifican el código contra esas reglas, mientras que otras permiten crear propiedades personalizadas para un contrato inteligente.

    1.  Análisis estático

        Un analizador estático toma como entrada el código fuente de un contrato inteligente y produce resultados declarando si un contrato satisface una propiedad o no. A diferencia del análisis dinámico, el análisis estático no implica ejecutar un contrato para analizarlo en cuanto a su corrección. En cambio, el análisis estático razona sobre todos los posibles caminos que un contrato inteligente podría tomar durante la ejecución (es decir, examinando la estructura del código fuente para determinar lo que significaría para la operación del contrato en tiempo de ejecución). El linting (comprobación automatizada del código en busca de errores programáticos y estilísticos) y el testing estático son métodos comunes para ejecutar análisis estático en contratos. Ambos requieren analizar representaciones de bajo nivel de la ejecución de un contrato como árboles de sintaxis abstracta y gráficos de flujos de control producidos por el compilador. En la mayoría de los casos, el análisis estático es útil para detectar problemas de seguridad como el uso de constructs inseguros, errores de sintaxis o violaciones de estándares de codificación en el código de un contrato. Sin embargo, se sabe que los analizadores estáticos son generalmente poco confiables para detectar vulnerabilidades más profundas y pueden producir muchos falsos positivos.
    2.  Análisis dinámico

        El análisis dinámico genera entradas simbólicas o entradas concretas para las funciones de un contrato inteligente para ver si algún rastro de ejecución viola propiedades específicas. Esta forma de pruebas basadas en propiedades difiere de las pruebas unitarias en que los casos de prueba cubren múltiples escenarios y un programa maneja la generación de casos de prueba.

        El fuzzing es un método automatizado que inyecta entradas inválidas o inesperadas en un sistema, revelando vulnerabilidades de seguridad. Utiliza herramientas especializadas que varían según la tecnología. Un fuzzer invoca funciones en un contrato objetivo con variaciones aleatorias o incorrectas de un valor de entrada definido. Si el contrato inteligente entra en un estado de error (por ejemplo, uno donde una afirmación falla), el problema se marca y las entradas que conducen hacia el error se registran en un informe.

        Esta forma de pruebas basadas en propiedades puede ser ideal por muchas razones:

        * **Es difícil escribir casos de prueba para cubrir muchos escenarios.** Una prueba de propiedad solo requiere que definas un comportamiento y un rango de datos para probar el comportamiento; el programa genera automáticamente casos de prueba basados en la propiedad definida.
        * **Tu conjunto de pruebas puede no cubrir suficientemente todos los posibles caminos dentro del programa.** Incluso con una cobertura del 100%, es posible perder casos extremos.
        * **Las pruebas unitarias demuestran que un contrato se ejecuta correctamente para datos de una muestra, pero si el contrato se ejecuta correctamente para entradas fuera de la muestra sigue siendo desconocido.** Las pruebas de propiedades ejecutan un contrato objetivo con múltiples variaciones de un valor de entrada dado para encontrar caminos de ejecución que causen fallos en las afirmaciones. Por lo tanto, una prueba de propiedad proporciona más garantías de que un contrato se ejecuta correctamente para una amplia clase de datos de entrada.
