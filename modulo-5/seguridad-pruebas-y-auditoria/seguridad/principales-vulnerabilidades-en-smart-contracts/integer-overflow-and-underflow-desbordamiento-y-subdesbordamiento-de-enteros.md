# Integer overflow and underflow (desbordamiento y subdesbordamiento de enteros)

El desbordamiento ocurre cuando una operación aritmética excede el límite superior del tipo de dato entero, y el subdesbordamiento ocurre cuando la operación cae por debajo del límite inferior.

**Ejemplo**:

```solidity
uint8 a = 255;
a += 1; // Esto causa un desbordamiento, llevando el valor de 'a' a 0.
```

**Mitigación**:

* Usar Solidity 0.8.x o superior, que tiene controles de desbordamiento y subdesbordamiento incorporados.
* Usar bibliotecas de aritmética segura como SafeMath de OpenZeppelin para versiones anteriores.

**Referencia**:

* [OpenZeppelin SafeMath](https://docs.openzeppelin.com/contracts/2.x/api/math)
