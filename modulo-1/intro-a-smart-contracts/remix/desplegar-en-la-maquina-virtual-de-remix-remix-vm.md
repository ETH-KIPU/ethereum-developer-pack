# Desplegar en la máquina virtual de Remix (Remix VM)

De lo revisado anteriormente, recordarás que compilar un contrato implica convertir el código de Solidity en Bytecode. Ahora llevaremos ese código a una blockchain de prueba.

Para ello, haz clic en el ícono de desplegar y ejecutar transacciones.

<figure><img src="../../../../.gitbook/assets/mod1_30.png" alt="" width="375"><figcaption></figcaption></figure>

En el campo ENVIRONMENT puedes seleccionar diferentes máquinas virtuales.

También pueden visualizar la cuenta desde la que desplegarás el contrato (campo ACCOUNT), donde Remix te ha asignado 100 ETH de prueba. En realidad tienes disponible 15 cuentas de prueba cada una con 100 ETH. Puedes seleccionar cualquiera de ellas en ese campo.

El GAS LIMIT es la cantidad de gas máxima que estás dispuesto a consumir en el despliegue del contrato. Es relevante cuando tengas que hacer el despliegue en una blockchain pública.

El campo VALUE es la cantidad de ETH que enviarás con la transacción, puedes seleccionar expresar el valor en ETH, wei, gwei o Finney.

El campo CONTRACT indica cuál es el contrato que vas a desplegar. Es importante verificarlo cuando tienes un código que contiene varios contratos.

Cuando hayas verificado todos los valores haz click en el botón naranja Deploy.

Habrás desplegado tu contrato compilado en la Remix VM, una blockchain simulada que corre en tu ventana de navegador. Es un ambiente muy conveniente para realizar prototipos muy rápidamente. A diferencia de una blockchain pública como Ethereum no requieres aprobar cada transacción.
