# Llamadas entre contratos

Llamar a un contrato desde otro contrato en Solidity es una práctica común que permite la interacción entre diferentes contratos inteligentes en la blockchain de Ethereum. Este proceso se puede realizar de varias maneras, dependiendo de si conoces la interfaz del contrato al que deseas llamar de antemano. A continuación, se describen dos métodos comunes para realizar estas llamadas: mediante Interfaces y mediante llamadas de bajo nivel.

**Método 1: Uso de Interfaces**

Si conoces la interfaz del contrato al que deseas llamar, puedes definir una interface en Solidity que declare los métodos del contrato externo. Luego, puedes usar esta interface para interactuar con el contrato externo como si estuvieras llamando a métodos locales.

1. **Definir la Interface del Contrato Externo**

Primero, defines una interface con las firmas de las funciones del contrato externo que deseas llamar.

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

interface ContratoExterno {
    function funcionExterna(string calldata mensaje) external returns (bool);
}
```

1. **Interactuar con el Contrato Externo**

Luego, en tu contrato, puedes utilizar esta interface para crear una instancia del contrato externo y llamar a sus funciones.

```solidity
contract MiContrato {
    function llamarFuncionExterna(address direccionExterna, string memory mensaje) public returns (bool) {
        ContratoExterno contrato = ContratoExterno(direccionExterna);
        return contrato.funcionExterna(mensaje);
    }
}
```

**Método 2: Llamadas de Bajo Nivel**

Las llamadas de bajo nivel (**`call`**, **`delegatecall`**, y **`staticcall`**) ofrecen más flexibilidad pero son menos seguras ya que no proporcionan verificación de tipos y pueden fallar silenciosamente si la llamada no tiene éxito. Debes utilizarlas solo si entiendes bien las implicancias.

```solidity
contract MiContrato {
    function llamarFuncionExternaBajoNivel(address direccionExterna, string memory mensaje) public returns (bool, bytes memory) {
        (bool exito, bytes memory respuesta) = direccionExterna.call(
            abi.encodeWithSignature("funcionExterna(string)", mensaje)
        );
        return (exito, respuesta);
    }
}
```

En este ejemplo, **`call`** se utiliza para llamar a **`funcionExterna`** del contrato en **`direccionExterna`**. **`abi.encodeWithSignature`** se usa para codificar la llamada a la función, incluyendo el nombre de la función y los parámetros que se pasan. La función devuelve dos valores: un booleano que indica si la llamada fue exitosa y los datos de respuesta.

Al llamar a otro contrato, es crucial tener en cuenta las implicaciones de seguridad, especialmente al aceptar direcciones de contratos como entrada de usuarios no confiables. Las llamadas a contratos no confiables pueden exponer tu contrato a riesgos de reentrancy u otros vectores de ataque. Utiliza patrones de seguridad como los checks-effects-interactions y considera el uso de **`reentrancy guards`** si tu contrato actualiza estados o transfiere fondos en respuesta a llamadas externas.
