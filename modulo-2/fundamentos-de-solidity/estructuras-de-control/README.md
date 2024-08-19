# Estructuras de Control

### Condicionales

Las estructuras condicionales permiten que el contrato inteligente realice acciones dependiendo del cumplimiento de condiciones específicas.

* **if-else**: Es la estructura condicional más básica, utilizada para ejecutar bloques de código si una condición es verdadera o falsa.

```solidity

if (condicion) {
    // Código a ejecutar si la condición es verdadera
} else {
    // Código a ejecutar si la condición es falsa
}
```

### Bucles

Los bucles permiten ejecutar un bloque de código repetidamente bajo ciertas condiciones.

* **for**: Utilizado para repetir un bloque de código un número determinado de veces.

```solidity
for (inicialización; condición; incremento) {
    // Código a ejecutar en cada iteración
}
```

* **while**: Ejecuta un bloque de código mientras una condición específica sea verdadera.

```solidity
while (condición) {
    // Código a ejecutar mientras la condición sea verdadera
}
```

* **do-while**: Similar al bucle while, pero garantiza que el bloque de código se ejecute al menos una vez.

```solidity
do {
    // Código a ejecutar
} while (condición);
```

### Control de Flujo

Solidity también incluye declaraciones de control de flujo que afectan cómo se ejecutan los bucles y condicionales.

*   **break**: Termina la ejecución del bucle más interno. Se utiliza para salir de un bucle cuando se cumple una condición específica.

    ```solidity
    while (condición) {
        // Código a ejecutar mientras la condición sea verdadera
    		if (expresión){
    			// Detiene la ejecución del bucle si la expresión es verdadera
    			break;
    			}
     }
    ```
*   **continue**: Salta el resto del código en el bucle actual e inicia la siguiente iteración del bucle.

    ```solidity
    while (condición) {
        // Código a ejecutar mientras la condición sea verdadera
    		if (expresión){
    			// Sale de esta iteración del bucle si la expresión es verdadera
    			continue;
    			}
     }
    ```

### Manejo de Errores

Existen también estructuras para manejar errores y revertir transacciones si es necesario.

* **require**: Se utiliza para validar condiciones y revertir la transacción si la condición no se cumple, opcionalmente con un mensaje de error.

```solidity
require(condición, "Mensaje de error");
```

* **revert**: Revierte la transacción. Puede ser utilizado con un mensaje de error específico.

```solidity
revert("Mensaje de error");
```

* **assert**: Utilizado para realizar comprobaciones internas y de invariantes. Si la condición evaluada es falsa, la transacción se revierte y consume todo el gas disponible, indicando un error grave.

```solidity
assert(condición);
```
