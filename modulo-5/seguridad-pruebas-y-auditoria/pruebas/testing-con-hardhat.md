# Testing con Hardhat

A continuación, se presenta un ejemplo completo de cómo escribir y ejecutar pruebas unitarias para un contrato inteligente en Ethereum utilizando Hardhat. Este ejemplo incluirá la creación del contrato inteligente, la configuración de los fixtures, y la escritura de diversas pruebas.

#### Paso 1: Configuración del proyecto

1. Debemos tener instalado Hardhat y haber creado un proyecto básico (si no sabes cómo hacerlo, revisar el [módulo 4](https://www.notion.so/Toolkit-Desarrollo-Ethereum-efe8e0c980a041a9a6378a54ada4dd09?pvs=21)).
2.  Asegúrate de tener instalado el plugin `hardhat-toolbox`

    ```bash
    npm install --save-dev @nomicfoundation/hardhat-toolbox
    ```

    Y también debes añadir la siguiente línea al inicio de tu fichero `hardhat.config.js`

    ```jsx
    require("@nomicfoundation/hardhat-toolbox");
    ```

#### Paso 2: Definir el contrato a probar

Utilizaremos el contrato `Token.sol` que ubicaremos en la carpeta `contracts` con el siguiente contenido:

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract Token {
    string public name = "KIPU";
    string public symbol = "KIP";
    uint8 public decimals = 18;
    uint256 public totalSupply;
    mapping(address => uint256) public balanceOf;

    event Transfer(address indexed from, address indexed to, uint256 value);

    constructor(uint256 _initialSupply) {
        totalSupply = _initialSupply * (10 ** uint256(decimals));
        balanceOf[msg.sender] = totalSupply;
    }

    function transfer(address _to, uint256 _value) public returns (bool success) {
        require(_to != address(0), "Invalid address");
        require(balanceOf[msg.sender] >= _value, "Insufficient balance");
        
        balanceOf[msg.sender] -= _value;
        balanceOf[_to] += _value;
        
        emit Transfer(msg.sender, _to, _value);
        
        return true;
    }
}
```

#### Paso 3: Escribir las pruebas

Crea un archivo de pruebas llamado `TokenTest.js` en la carpeta `test` con el siguiente contenido:

```jsx
const { expect } = require("chai");
const { ethers } = require("hardhat");
const { loadFixture } = require("@nomicfoundation/hardhat-network-helpers");

describe("Token contract", function () {
  async function deployTokenFixture() {
    const [owner, addr1, addr2] = await ethers.getSigners();

    const Token = await ethers.getContractFactory("Token");
    const initialSupply = 1000n;  // Use BigInt
    const token = await Token.deploy(initialSupply);

    return { token, owner, addr1, addr2 };
  }

  it("Should set the right owner", async function () {
    const { token, owner } = await loadFixture(deployTokenFixture);
    expect(await token.balanceOf(owner.address)).to.equal(1000n * 10n ** 18n);  // Use BigInt
  });

  it("Should assign the total supply of tokens to the owner", async function () {
    const { token, owner } = await loadFixture(deployTokenFixture);
    const ownerBalance = await token.balanceOf(owner.address);
    expect(await token.totalSupply()).to.equal(ownerBalance);
  });

  describe("Transactions", function () {
    it("Should transfer tokens between accounts", async function () {
      const { token, owner, addr1, addr2 } = await loadFixture(deployTokenFixture);

      await token.transfer(addr1.address, 50n * 10n ** 18n);  // Use BigInt
      expect(await token.balanceOf(addr1.address)).to.equal(50n * 10n ** 18n);  // Use BigInt

      await token.connect(addr1).transfer(addr2.address, 50n * 10n ** 18n);  // Use BigInt
      expect(await token.balanceOf(addr2.address)).to.equal(50n * 10n ** 18n);  // Use BigInt
      expect(await token.balanceOf(addr1.address)).to.equal(0n);  // Use BigInt
    });

    it("Should fail if sender doesn’t have enough tokens", async function () {
      const { token, owner, addr1 } = await loadFixture(deployTokenFixture);
      const initialOwnerBalance = await token.balanceOf(owner.address);

      await expect(
        token.connect(addr1).transfer(owner.address, 1n * 10n ** 18n)  // Use BigInt
      ).to.be.revertedWith("Insufficient balance");

      expect(await token.balanceOf(owner.address)).to.equal(initialOwnerBalance);
    });

    it("Should update balances after transfers", async function () {
      const { token, owner, addr1, addr2 } = await loadFixture(deployTokenFixture);
      const initialOwnerBalance = await token.balanceOf(owner.address);

      await token.transfer(addr1.address, 100n * 10n ** 18n);  // Use BigInt
      await token.transfer(addr2.address, 50n * 10n ** 18n);  // Use BigInt

      const finalOwnerBalance = await token.balanceOf(owner.address);
      expect(finalOwnerBalance).to.equal(initialOwnerBalance - 150n * 10n ** 18n);  // Use BigInt

      const addr1Balance = await token.balanceOf(addr1.address);
      expect(addr1Balance).to.equal(100n * 10n ** 18n);  // Use BigInt

      const addr2Balance = await token.balanceOf(addr2.address);
      expect(addr2Balance).to.equal(50n * 10n ** 18n);  // Use BigInt
    });
  });
});
```

Es importante explicar el script anterior.

**Hacemos las importaciones y configuraciones que requerimos.**

```jsx
const { expect } = require("chai");
const { ethers } = require("hardhat");
const { loadFixture } = require("@nomicfoundation/hardhat-network-helpers");
```

* **chai**: Una biblioteca de afirmaciones (assertions) que facilita la verificación de los resultados esperados en las pruebas.
* **ethers**: Una biblioteca para interactuar con contratos inteligentes y la blockchain.
* **loadFixture**: Una función que carga un estado inicial fijo para las pruebas, asegurando que todas las pruebas comiencen con las mismas condiciones.

**Luego definimos la fixture**

```jsx
async function deployTokenFixture() {
  const [owner, addr1, addr2] = await ethers.getSigners();
  const Token = await ethers.getContractFactory("Token");
  const initialSupply = 1000n;  // Usamos BigInt para evitar desbordamientos
  const token = await Token.deploy(initialSupply);

  return { token, owner, addr1, addr2 };
}
```

* **deployTokenFixture**: Esta función despliega el contrato `Token` con un suministro inicial. También obtiene tres cuentas para usar en las pruebas (owner, addr1, addr2).

**Primera prueba: Verificar el propietario**

```jsx
it("Should set the right owner", async function () {
  const { token, owner } = await loadFixture(deployTokenFixture);
  expect(await token.balanceOf(owner.address)).to.equal(1000n * 10n ** 18n);
});
```

* **Propósito**: Verificar que el propietario inicial (la cuenta que despliega el contrato) reciba el suministro total de tokens.
* **Proceso**:
  1. Desplegar el contrato utilizando la fixture.
  2. Obtener el balance del propietario.
  3. Verificar que el balance sea igual al suministro inicial (1000 tokens convertidos a la unidad mínima).

**Segunda prueba: Verificar el suministro total**

```jsx
it("Should assign the total supply of tokens to the owner", async function () {
  const { token, owner } = await loadFixture(deployTokenFixture);
  const ownerBalance = await token.balanceOf(owner.address);
  expect(await token.totalSupply()).to.equal(ownerBalance);
});
```

* **Propósito**: Asegurarse de que el suministro total de tokens se asigna correctamente al propietario.
* **Proceso**:
  1. Desplegar el contrato utilizando la fixture.
  2. Obtener el balance del propietario.
  3. Verificar que el balance del propietario sea igual al suministro total de tokens.

**Tercera prueba: Transferir tokens**

```jsx
describe("Transactions", function () {
  it("Should transfer tokens between accounts", async function () {
    const { token, owner, addr1, addr2 } = await loadFixture(deployTokenFixture);

    await token.transfer(addr1.address, 50n * 10n ** 18n);
    expect(await token.balanceOf(addr1.address)).to.equal(50n * 10n ** 18n);

    await token.connect(addr1).transfer(addr2.address, 50n * 10n ** 18n);
    expect(await token.balanceOf(addr2.address)).to.equal(50n * 10n ** 18n);
    expect(await token.balanceOf(addr1.address)).to.equal(0n);
  });
```

* **Propósito**: Verificar que los tokens se transfieren correctamente entre cuentas.
* **Proceso**:
  1. Desplegar el contrato utilizando la fixture.
  2. Transferir 50 tokens de `owner` a `addr1`.
  3. Verificar que `addr1` haya recibido 50 tokens.
  4. Transferir 50 tokens de `addr1` a `addr2`.
  5. Verificar que `addr2` haya recibido 50 tokens y que `addr1` tenga un balance de 0 tokens.

**Cuarta prueba: Verificar que una cuenta no transfiere si no tienen fondos suficientes**

```jsx
it("Should fail if sender doesn’t have enough tokens", async function () {
  const { token, owner, addr1 } = await loadFixture(deployTokenFixture);
  const initialOwnerBalance = await token.balanceOf(owner.address);

  await expect(
    token.connect(addr1).transfer(owner.address, 1n * 10n ** 18n)
  ).to.be.revertedWith("Insufficient balance");

  expect(await token.balanceOf(owner.address)).to.equal(initialOwnerBalance);
});
```

* **Propósito**: Asegurarse de que la transferencia falla si el remitente no tiene suficientes tokens.
* **Proceso**:
  1. Desplegar el contrato utilizando la fixture.
  2. Intentar transferir 1 token desde `addr1` (que no tiene tokens) al `owner`.
  3. Verificar que la transacción sea revertida con el mensaje "Insufficient balance".
  4. Verificar que el balance del `owner` no haya cambiado.

**Quinta prueba: Actualización de balances después de transferencia**

```jsx
it("Should update balances after transfers", async function () {
  const { token, owner, addr1, addr2 } = await loadFixture(deployTokenFixture);
  const initialOwnerBalance = await token.balanceOf(owner.address);

  await token.transfer(addr1.address, 100n * 10n ** 18n);
  await token.transfer(addr2.address, 50n * 10n ** 18n);

  const finalOwnerBalance = await token.balanceOf(owner.address);
  expect(finalOwnerBalance).to.equal(initialOwnerBalance - 150n * 10n ** 18n);

  const addr1Balance = await token.balanceOf(addr1.address);
  expect(addr1Balance).to.equal(100n * 10n ** 18n);

  const addr2Balance = await token.balanceOf(addr2.address);
  expect(addr2Balance).to.equal(50n * 10n ** 18n);
});
```

* **Propósito**: Verificar que los balances se actualicen correctamente después de las transferencias.
* **Proceso**:
  1. Desplegar el contrato utilizando la fixture.
  2. Transferir 100 tokens de `owner` a `addr1`.
  3. Transferir 50 tokens de `owner` a `addr2`.
  4. Verificar que el balance final del `owner` sea el balance inicial menos 150 tokens.
  5. Verificar que `addr1` tenga 100 tokens y `addr2` tenga 50 tokens.

#### Paso 4: Ejecutar las pruebas

Para ejecutar las pruebas, utiliza el siguiente comando:

```bash
npx hardhat test
```

Este comando ejecutará todas las pruebas definidas en el archivo `TokenTest.js` y mostrará los resultados en la consola.

Si las pruebas han sido exitosas, obtendrás un resultado de este tipo:

<figure><img src="../../../.gitbook/assets/Untitled (11).png" alt=""><figcaption></figcaption></figure>

¡¡Felicitaciones!! Has ejecutado tus primeras pruebas de forma exitosa. Ahora empieza a hacer pruebas con otros contratos que hayas creado.

Hardhat también te permite obtener la cobertura de tus pruebas, para ello solo necesitas ingresar el siguiente comando:

```bash
npx hardhat coverage
```

y obtendrás un reporte como este

<figure><img src="../../../.gitbook/assets/Untitled (12).png" alt=""><figcaption></figcaption></figure>

#### Una cosa más: Chai

En este ejemplo hemos utilizado Chai para realizar nuestras pruebas. Chai es una biblioteca de assertions (afirmaciones) que existe para Node.js que se puede combinar con cualquier framework de pruebas como Hardhat. En el contexto de pruebas para contratos inteligentes, Chai se utiliza ampliamente debido a su sintaxis amigable y capacidades de aserción robustas.

Para usar Chai en tus pruebas, primero necesitas requerir la biblioteca y luego utilizar sus métodos en tus pruebas. Aquí hay un ejemplo básico de configuración:

```jsx
const { expect } = require("chai");
```

Chai ofrece tres estilos de aserción principales:

1. **Assert**: Estilo clásico basado en funciones.
2. **Expect**: Estilo BDD (Behavior-Driven Development) que es más legible.
3. **Should**: Estilo BDD que añade propiedades al objeto Object.prototype.

En nuestro ejemplo utilizamos el estilo `expect`, ya que es uno de los más utilizados en pruebas de contratos inteligentes con Hardhat.

**Principales Comandos de Chai**

1. Igualdad (`equal`, `eql`)

* `equal` verifica igualdad estricta (`===`).
* `eql` verifica igualdad profunda para objetos.

```jsx
expect(1).to.equal(1);
expect({ foo: 'bar' }).to.eql({ foo: 'bar' });
```

2. Booleanos (`true`, `false`)

Verifica si un valor es verdadero o falso.

```jsx
expect(true).to.be.true;
expect(false).to.be.false;
```

3. Existencia (`exist`)

Verifica si un valor no es `null` ni `undefined`.

```jsx
let foo = 'bar';
expect(foo).to.exist;
```

4. Tipos (`a`, `an`)

Verifica el tipo de un valor.

```jsx
expect('foo').to.be.a('string');
expect({ foo: 'bar' }).to.be.an('object');
```

5. Contenido (`include`, `contain`)

Verifica si un valor contiene otro valor (en arrays, strings, o objetos).

```jsx
expect([1, 2, 3]).to.include(2);
expect('foobar').to.contain('foo');
expect({ foo: 'bar', baz: 'qux' }).to.include({ foo: 'bar' });
```

6. Longitud (`lengthOf`)

Verifica la longitud de un array, string, o Map.

```jsx
expect([1, 2, 3]).to.have.lengthOf(3);
expect('foo').to.have.lengthOf(3);
```

7. Mayor y menor que (`above`, `below`)

Verifica si un valor es mayor o menor que otro.

```jsx
expect(10).to.be.above(5);
expect(5).to.be.below(10);
```

8. Cerca de (`closeTo`)

Verifica si un número está cerca de otro, con un margen de error.

```jsx
expect(1.5).to.be.closeTo(1.4, 0.1);
```

9. Emisión de eventos (`emit`, `withArgs`)

En el contexto de pruebas de contratos inteligentes, verificamos si un evento se emite.

```jsx
await expect(contract.emitEvent()).to.emit(contract, "EventName").withArgs(expectedArgs);
```

Para mayor referencia de Chai, revisa la [documentación de Hardhat](https://hardhat.org/hardhat-chai-matchers/docs/reference).
