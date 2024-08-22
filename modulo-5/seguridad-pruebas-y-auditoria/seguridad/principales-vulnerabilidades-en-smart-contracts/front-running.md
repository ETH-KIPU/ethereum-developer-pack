# Front-running

Ocurre cuando un atacante explota el orden de las transacciones pendientes en la mempool para ejecutar una transacción antes de otra transacción que ya está en espera, con el fin de obtener un beneficio injusto. Este tipo de ataque es común en plataformas DeFi, como intercambios descentralizados (DEXs), donde el precio de un activo puede verse afectado por grandes órdenes de compra o venta.

**Ejemplo:**

Imagina que un usuario A intenta comprar una gran cantidad de un token en un intercambio descentralizado (DEX). La transacción del usuario A se envía a la red y queda en la mempool a la espera de ser confirmada. Un atacante, que está monitoreando la mempool, observa la transacción y se da cuenta de que la compra del usuario A incrementará el precio del token.

El atacante puede entonces enviar una transacción con un gas más alto para comprar el token antes de que se ejecute la transacción del usuario A. Debido a que los mineros priorizan las transacciones con tarifas de gas más altas, la transacción del atacante se procesará primero, comprando el token a un precio más bajo. Luego, cuando la transacción del usuario A se ejecute, el precio del token habrá subido, permitiendo al atacante vender su token adquirido a un precio más alto, obteniendo una ganancia rápida.

```solidity
function swapTokens(uint amountIn, address tokenIn, address tokenOut) external {
    // Cálculo del precio antes de ejecutar el intercambio
    uint priceBefore = getPrice(tokenOut);

    // Ejecuta el intercambio de tokens
    uint amountOut = executeSwap(amountIn, tokenIn, tokenOut);

    // Cálculo del precio después del intercambio
    uint priceAfter = getPrice(tokenOut);

    // Potencialmente, un atacante podría observar y ejecutar una transacción antes de esta
}
```

**Mitigación:**

1.  **Uso de Commit-Reveal Schemes:** Este esquema implica dividir la transacción en dos fases: un "commit" donde se envía un hash de la transacción y un "reveal" donde se revela la transacción completa. Durante la fase de "commit", el atacante no tiene acceso a la información completa y no puede realizar un front running.

    ```solidity
    solidityCopiar código
    mapping(address => bytes32) public commitments;

    function commitSwap(bytes32 _commitment) external {
        commitments[msg.sender] = _commitment;
    }

    function revealSwap(uint amountIn, address tokenIn, address tokenOut, uint nonce) external {
        require(commitments[msg.sender] == keccak256(abi.encodePacked(amountIn, tokenIn, tokenOut, nonce)), "Invalid reveal");
        // Ejecutar el swap
    }
    ```
2. **Gas Limit y priorización de transacciones:** Los contratos inteligentes pueden establecer límites de gas o incluir reglas que prioricen las transacciones en función de factores distintos a la tarifa de gas, lo que hace más difícil para un atacante simplemente pagar más gas para adelantarse en la fila.
3.  **Slippage tolerance:** En los intercambios descentralizados, los usuarios pueden establecer una "tolerancia al deslizamiento" que define el rango de precios aceptable para la transacción. Si el precio se mueve más allá de esta tolerancia debido a un posible front running, la transacción se revertirá.

    ```solidity
    function swapTokens(uint amountIn, address tokenIn, address tokenOut, uint minAmountOut) external {
        uint amountOut = executeSwap(amountIn, tokenIn, tokenOut);
        require(amountOut >= minAmountOut, "Slippage too high");
    }
    ```
4. **Mecanismos de bloqueo temporal:** Introducir retrasos controlados o ventanas de tiempo en las que una transacción no puede ser confirmada inmediatamente puede reducir la capacidad de los atacantes de ejecutar transacciones en un orden ventajoso.
5. **Ordenación aleatoria de transacciones:** En lugar de procesar las transacciones en el orden en que llegan a la mempool, se pueden ordenar de forma aleatoria o utilizar loterías para determinar el orden de procesamiento, dificultando los ataques de front running.
6. **Flashbots y MEV-Protection:** Herramientas como Flashbots permiten a los usuarios enviar transacciones directamente a los validadores sin que pasen por la mempool pública, evitando que sean visibles para atacantes que buscan ejecutar un front running. Esta técnica ayuda a mitigar el riesgo de "Maximal Extractable Value" (MEV), que es la ganancia que los atacantes pueden obtener mediante front running.
