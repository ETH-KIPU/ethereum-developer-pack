# Constructor

El constructor es una función especial que se ejecuta únicamente cuando se despliega un contrato inteligente en Ethereum. Los constructores son utilizados para inicializar las variables de estado del contrato, establecer configuraciones iniciales o realizar cualquier configuración preliminar necesaria.

En versiones antiguas de Solidity, el constructor tenía el mismo nombre que el contrato. Sin embargo, en versiones más recientes (0.4.22 y superiores), se utiliza la palabra clave **`constructor`** para definirlo, lo que mejora la claridad y reduce la posibilidad de errores.

El constructor se ejecuta solo una vez, en el momento en que el contrato es desplegado en la blockchain, y no puede ser llamado nuevamente una vez que el contrato está desplegado.

**Visibilidad:** Los constructores pueden ser declarados como public o internal. Un constructor public es aquel que cualquiera puede desplegar, mientras que un constructor internal solo puede ser desplegado a través de mecanismos internos, como por ejemplo, a través de otro contrato.

**Parámetros:** Al igual que otras funciones, los constructores pueden tener parámetros, lo que permite personalizar la configuración inicial del contrato durante el despliegue.

**No Genera Dirección:** A diferencia de las funciones normales, el constructor no tiene una dirección msg.sender. Durante la creación del contrato, msg.sender es la dirección que despliega el contrato.

**Ausencia de Valor de Retorno:** Los constructores no tienen un valor de retorno.

Ejemplo de un Constructor en Solidity:

```solidity
pragma solidity ^0.8.0;

contract MyContract {
uint256 public myNumber;
address public owner;
constructor(uint256 _myNumber) {
    myNumber = _myNumber;
    owner = msg.sender;
}
}
```

En este ejemplo, el constructor del contrato MyContract establece el valor inicial de la variable myNumber y asigna el propietario del contrato a la dirección que lo despliega. Estas operaciones se realizan una sola vez, en el momento del despliegue del contrato.
