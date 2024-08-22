# Conceptos importantes en testing

### Mainnet Forking

Mainnet forking es una técnica en las pruebas de blockchain donde se toma un número de bloque específico y una referencia a un nodo de blockchain (como Geth), y se copia toda la información de estado relevante hasta ese número de bloque. Este estado clonado permite a los desarrolladores ejecutar pruebas contra él. Esta estrategia se implementa más comúnmente contra mainnet, especialmente cuando se prueban interacciones con código de terceros que ya está desplegado.

Una de las ventajas significativas del mainnet forking es la capacidad de ejecutar todas las pruebas localmente, reduciendo significativamente tanto los riesgos como los costos asociados con las pruebas. Dado que la red local refleja el estado en cadena, también puede revertirse a su estado original después de cada prueba, lo que permite pruebas eficientes y aisladas. Además, el estado local puede manipularse según sea necesario para simular escenarios específicos que podrían no estar presentes en el estado del fork. Mainnet forking es, por lo tanto, una herramienta potente, especialmente para probar escenarios que no están fácilmente cubiertos por las pruebas unitarias y de integración.

### Desarrollo guiado por pruebas

El Desarrollo Guiado por Pruebas (Test Driven Developmente - TDD) es una metodología en la que se escriben pruebas antes del código real. Este enfoque intenta asegurar que todo el código se alinee con las especificaciones impuestas por las pruebas. El flujo de trabajo implica escribir pruebas, verificar que están fallando, escribir el código y confirmar que las pruebas pasan. Es importante notar que el código que hace que la prueba pase debe ser la cantidad mínima requerida, evitando así que cualquier código superfluo se infiltre en el proyecto. Si las pruebas fallan, es porque las pruebas o el código contienen un error.

En consecuencia, el proceso se repite después de una ronda de restructuración del código y las pruebas. Al final de una ronda de TDD, es común revisar nuevamente toda la base de código y estructurarla de manera más ordenada, por ejemplo, externalizando el código en bibliotecas o componentes individuales y volviendo a ejecutar el conjunto de pruebas para asegurar que no se han introducido errores. Aunque TDD no puede garantizar un código libre de errores, generalmente proporciona garantías más fuertes que adaptar un conjunto de pruebas a posteriori. También agiliza el proceso de escritura de pruebas, ya que los desarrolladores a menudo escriben pruebas al final del proceso de desarrollo, lo que puede llevar a fatiga y falta de cobertura en los escenarios de prueba. TDD se aplica principalmente a pruebas unitarias, pero también puede utilizarse para pruebas de integración, asumiendo que los componentes están bien definidos y no es probable que cambien. Aunque a menudo se percibe como una desventaja, TDD obliga a los desarrolladores y gerentes de proyecto a reflexionar primero sobre la arquitectura y diseño de sus contratos inteligentes y establecer un conjunto de requisitos de usuario que el sistema necesita cumplir. No obstante, cabe señalar que todas las ventajas de TDD tienen un costo en la velocidad de desarrollo, especialmente cuando los desarrolladores aún no están bien versados en el desarrollo guiado por pruebas.

### Cobertura de pruebas

La cobertura de pruebas (test coverage) de contratos inteligentes se refiere a la medida en que el código del contrato ha sido ejecutado y verificado a través de pruebas automatizadas. En términos simples, es una métrica que indica qué porcentaje del código ha sido probado mediante casos de prueba específicos.

Una cobertura de pruebas del 100% es el objetivo, pero es difícil de lograr en la práctica. Sin embargo, los desarrolladores deben aspirar a cubrir todas las funciones y casos de uso críticos del contrato. Las herramientas de cobertura de código, como `solidity-coverage`, pueden ayudar a medir qué partes del código han sido probadas

**Importancia de la cobertura de pruebas**

1. **Seguridad**:
   * Asegura que todas las partes críticas del contrato han sido verificadas y son seguras contra posibles vulnerabilidades.
2. **Funcionalidad**:
   * Verifica que todas las funciones del contrato operen como se espera bajo diversas condiciones y entradas.
3. **Confianza**:
   * Proporciona confianza tanto a los desarrolladores como a los usuarios de que el contrato ha sido exhaustivamente probado y es confiable.
4. **Mantenimiento**:
   * Facilita el mantenimiento y la actualización del contrato, asegurando que cualquier cambio o adición al código sea también probado adecuadamente.

**Tipos de cobertura de pruebas**

1. **Cobertura de Líneas**:
   * Mide el porcentaje de líneas de código que han sido ejecutadas durante las pruebas.
2. **Cobertura de Funciones**:
   * Mide el porcentaje de funciones que han sido llamadas al menos una vez durante las pruebas.
3. **Cobertura de Condiciones**:
   * Mide el porcentaje de posibles condiciones booleanas que han sido evaluadas como verdaderas y falsas durante las pruebas.
4. **Cobertura de Ramas**:
   * Mide el porcentaje de caminos de ejecución (ramas) que han sido seguidos durante las pruebas, incluyendo las bifurcaciones en las sentencias `if`, `else`, `switch`, etc.

**Herramienta para medir la cobertura de pruebas**

Una de las herramientas más utilizadas para medir la cobertura de pruebas en contratos inteligentes escritos en Solidity es solidity-coverage. Genera un informe detallado sobre qué partes del código han sido ejecutadas durante las pruebas. Se puede ejecutar de forma independiente o dentro de Hardhat. Puedes encontrar más información en [su repositorio](https://github.com/sc-forks/solidity-coverage) en GitHub.

### Fixtures

Los fixtures son escenarios de prueba que se ejecutan una vez y luego son recordados haciendo fotos de la blockchain. Entre los beneficios que ofrecen podemos considerar:

* Nos liberan de la necesidad de volver a desplegar el contrato antes de hacer una nueva prueba.
* Garantizan que las pruebas se ejecuten siempre bajo las mismas condiciones iniciales, lo que es crucial para la consistencia y confiabilidad de las pruebas.
* Permiten que cada prueba se ejecute en un entorno limpio y aislado, evitando que el estado de una prueba afecte a otra.
* Ayudan a reducir el tiempo de configuración para cada prueba individual al definir un estado inicial común para un grupo de pruebas.

Hardhat permite configurar fixtures en su entorno de pruebas.
