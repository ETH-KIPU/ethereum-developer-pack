# Cómo funciona la Blockchain

Para entender cómo funciona una blockchain utilizaremos la primera y más conocida: Bitcoin. La explicaremos de forma simplificada pero mostrando los principales elementos de la blockchain.

Partamos del hecho de que existe una red distribuida de tipo P2P de computadoras (nodos).

Una red P2P que es clave para entender cómo funciona una blockchain Esta red está permanentemente activa y se encarga de hacer funcionar el sistema de Bitcoin, para ello todos los nodos corren un mismo programa que contiene las reglas de funcionamiento de la blockchain.

Empecemos:

1. Iris quiere enviar un bitcoin a Pepe. Para ello Iris inicia una transferencia desde su billetera digital hacia la dirección bitcoin de Pepe. La transacción registra los datos de dirección de origen, dirección de destino, importe de la transferencia y la comisión si está dispuesta a pagarla.
2. La transacción se difunde desde los nodos más cercanos hacia toda la red P2P. Esto ocurre también en simultáneo con otras transacciones que realizan otros usuarios de la red.
3. Las transacciones se van acumulando en una mempool, que es un fichero en el que los nodos almacenan las transacciones que aún no han sido incorporadas en la blockchain y que por tanto están pendientes de validación.
4. Los nodos mineros (computadoras), que almacenan la blockchain completa, compiten en la creación de un nuevo bloque. Para ello conforman su bloque candidato tomando transacciones de la mempool (en promedio unas mil), añadiendo una cabecera, que entre otros datos contiene: el número de bloque (o altura de bloque), el hash del último bloque creado, la hora de creación , un resumen de las transacciones incluidas y un número dummy denominado nonce que le servirá para participar en la prueba de trabajo a realizar.
5. Con su bloque candidato conformado, cada nodo minero participa en una competencia denominada prueba de trabajo, que requiere consumo a tope de su capacidad de procesamiento, generando hashes utilizando una función como SHA-256. La competencia demora en promedio 10 minutos hasta que un nodo minero obtiene un hash correcto. El nodo minero que obtuvo la solución transmite a toda los nodos de la red su bloque ganador para su verificación.
6. Los nodos verifican que las transacciones sean correctas (que, por ejemplo, Iris tenga bitcoins para transferir a Pepe y que Iris haya autorizado esa transferencia utilizando su clave privada) y que se ha obtenido un hash que cumple los requisitos de la prueba de trabajo. Si todo está bien, se acepta el nuevo bloque y se añade a la blockchain. El nodo ganador recibe del sistema los bitcoins de premio definidos en ese momento (actualmente 6.25 BTC) y las comisiones incluidas en las transacciones de su bloque.
7. Los nodos de la red, retiran de sus mempools las transacciones que ya están en el nuevo bloque y vuelven a trabajar desde el punto 4. La transferencia de Iris a Pepe ya ha sido validada por la red y registrada para siempre en la blockchain.

Aquí tienes un video que resume el proceso anterior:

{% embed url="https://youtu.be/-n4691RFg9c?si=En3P9qSm8dl2CxsB" %}
