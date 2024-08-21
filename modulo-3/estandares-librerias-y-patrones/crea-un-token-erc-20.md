# Crea un Token ERC-20

Para crear un token ERC-20 utilizando la librería de OpenZeppelin en Solidity, debes seguir algunos pasos esenciales que involucran configurar tu entorno de desarrollo, escribir el contrato inteligente utilizando las bibliotecas de OpenZeppelin, desplegarlo en la blockchain y verificar su funcionamiento.

### **Paso 1: Configurar el Entorno de Desarrollo**

Antes de empezar a escribir tu contrato, necesitarás configurar un entorno de desarrollo. Nosotros venimos utilizando Remix, pero también se podría utilizar Hardhat o Truffle. Para este ejemplo, vamos a utilizar Remix por su simplicidad y facilidad de uso.

1.  **Instalación de OpenZeppelin**: En Remix, puedes importar directamente de GitHub. Para entornos como Truffle o Hardhat, deberás instalar OpenZeppelin a través de npm:

    ```bash
    npm install @openzeppelin/contracts
    ```

### **Paso 2: Escribir el Contrato ERC-20**

Aquí está un ejemplo básico de cómo definir un token ERC-20 utilizando OpenZeppelin. Este token tendrá un suministro fijo y características básicas de ERC-20.

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

import "@openzeppelin/contracts/token/ERC20/ERC20.sol";

contract MyToken is ERC20 {
    constructor(uint256 initialSupply) ERC20("MyToken", "MTK") {
        _mint(msg.sender, initialSupply);
    }
}
```

#### **Explicación del Código:**

* **Importación**: Importamos **`ERC20.sol`** de OpenZeppelin, que contiene la implementación estándar de un token ERC-20.
* **Contrato MyToken**: Extiende **`ERC20`** de OpenZeppelin. El constructor requiere un suministro inicial **`initialSupply`** y llama a **`ERC20`**, el constructor de OpenZeppelin, para establecer el nombre (”MyToken”) y el símbolo del token (”MTK”).
* **Minting**: **`_mint`** es una función que se utiliza para crear la cantidad inicial de tokens. Estos tokens se asignan a la dirección que despliega el contrato (generalmente tu dirección).

### **Paso 3: Desplegar el Contrato**

Usando Remix:

1. **Compila el Contrato**: En Remix, compila el contrato seleccionando la versión correcta de Solidity que coincida con la declaración en tu código.
2. **Desplegar**: Cambia a la pestaña "Deploy & Run Transactions". Selecciona el entorno "Injected Web3" si estás usando Metamask y asegúrate de estar conectado a la red que deseas (por ejemplo, Sepolia).
3. **Ejecuta el Despliegue**: Introduce el suministro inicial en el campo del constructor y haz clic en "deploy".

### **Paso 4: Interactuar con el Contrato**

Una vez desplegado, puedes interactuar con tu token a través de Remix. Bajo la pestaña "Deployed Contracts", encontrarás tu contrato desplegado con todas las funciones de ERC-20 disponibles para interactuar desde la interfaz.

### **Paso 5: Verificar y Probar**

Es crucial verificar que tu token funcione como esperas:

* **Prueba las Funciones**: Utiliza Remix para transferir tokens entre direcciones, consultar balances, y probar las aprobaciones.
* **Verifica y publica el contrato:** Usando Etherscan es recomendable que verifiques y publiques el contrato de forma que quienes quieran ver qué hace tu contrato puedan comprobar que cumple con el estándar.
* **Considera Auditorías**: Si planeas lanzar tu token en mainnet considera realizar una auditoría de seguridad.

Crear un token ERC-20 es relativamente sencillo con OpenZeppelin, pero asegúrate de comprender todos los aspectos legales y de seguridad asociados con la creación y distribución de un nuevo token.
