# Herencia

La herencia es un principio fundamental de la programación orientada a objetos (OOP) que también se encuentra en Solidity. La herencia permite que un contrato herede propiedades y comportamientos (variables de estado y funciones) de uno o más contratos "padres", promoviendo así la reutilización de código y la creación de relaciones jerárquicas entre contratos.

**Características de la Herencia en Solidity**

* **Reutilización de Código**: La herencia permite que los contratos inteligentes reutilicen el código de otros contratos. Esto no solo reduce la redundancia sino que también fomenta la práctica del principio DRY (Don't Repeat Yourself).
* **Jerarquía de Contratos**: Solidity permite la creación de una jerarquía de contratos, donde un contrato hijo puede heredar de múltiples contratos padres, siguiendo un patrón de herencia múltiple.
* **Sobrescritura de Funciones**: Los contratos hijos pueden sobrescribir funciones heredadas de sus contratos padres, lo que permite personalizar o extender la funcionalidad base. Para esto se utilizará el atributo **`override`** en la función.
* **Modificadores de Visibilidad**: Solidity usa modificadores de visibilidad (**`public`**, **`internal`**, **`private`**, y **`external`**) para controlar el acceso a las funciones y variables de estado. Las reglas de visibilidad también aplican a los contratos heredados.

**Ejemplo Básico de Herencia**

<div>

<img src="https://prod-files-secure.s3.us-west-2.amazonaws.com/1c4f016a-3ef4-422a-bee4-76cbe41f54a1/dbe51a31-2623-4831-b4d9-ccfa928a00c9/Untitled.png" alt="">

 

<figure><img src="../../../.gitbook/assets/Herencia básica.png" alt="" width="361"><figcaption></figcaption></figure>

</div>

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

// Contrato padre
contract Base {
    uint public data;

    constructor(uint _data) {
        data = _data;
    }

    function setData(uint _data) public {
        data = _data;
    }
}

// Contrato hijo que hereda de Base
contract Derived is Base {
    constructor(uint _initialData) Base(_initialData) {
        // Inicializa el contrato padre con _initialData
    }

    // Sobrescritura de la función setData
    function setData(uint _data) public override {
        data = _data + 10; // Cambia la implementación para sumar 10 antes de almacenar
    }
}
```

En este ejemplo, **`Derived`** hereda de **`Base`**. Esto significa que **`Derived`** tiene acceso a la variable **`data`** y puede usar o sobrescribir la función **`setData`**. En la función **`setData`** sobrescrita, modificamos la implementación para sumar 10 al **`_data`** antes de almacenarlo.

**Herencia multinivel**

La herencia multinivel es un concepto donde un contrato hereda de otro contrato, que a su vez hereda de otro, creando así una jerarquía de herencia "multinivel". Este patrón permite la construcción de relaciones jerárquicas complejas y la reutilización de código a través de varios niveles de contratos.

<figure><img src="../../../.gitbook/assets/Herencia multinivel.png" alt="" width="375"><figcaption></figcaption></figure>

Aquí tienes un ejemplo de herencia multinivel en Solidity, que ilustra cómo un contrato puede heredar propiedades y comportamientos de varios contratos padres situados en diferentes niveles de la jerarquía:

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

// Contrato de nivel base
contract Grandparent {
    uint public grandparentValue;

    constructor(uint _value) {
        grandparentValue = _value;
    }

    function setGrandparentValue(uint _value) public {
        grandparentValue = _value;
    }
}

// Primer nivel de herencia
contract Parent is Grandparent {
    uint public parentValue;

    constructor(uint _grandparentValue, uint _parentValue) Grandparent(_grandparentValue) {
        parentValue = _parentValue;
    }

    function setParentValue(uint _value) public {
        parentValue = _value;
    }
}

// Segundo nivel de herencia
contract Child is Parent {
    uint public childValue;

    constructor(uint _grandparentValue, uint _parentValue, uint _childValue) Parent(_grandparentValue, _parentValue) {
        childValue = _childValue;
    }

    function setChildValue(uint _value) public {
        childValue = _value;
    }
}
```

#### **Explicación del código:**

* **`Grandparent`**: Este es el contrato de nivel base que define una variable **`grandparentValue`** y una función **`setGrandparentValue`** para modificarla. También tiene un constructor que inicializa **`grandparentValue`**.
* **`Parent`**: Este contrato hereda de **`Grandparent`**. Añade su propia variable **`parentValue`** y una función **`setParentValue`** para modificarla. Su constructor llama al constructor de **`Grandparent`** para inicializar **`grandparentValue`**, y también inicializa **`parentValue`**.
* **`Child`**: Este es el contrato de nivel más bajo que hereda de **`Parent`** (y, por lo tanto, indirectamente de **`Grandparent`**). Añade una variable **`childValue`** y una función **`setChildValue`**. Su constructor inicializa las variables de todos los niveles de la jerarquía llamando al constructor de **`Parent`**, que a su vez llama al constructor de **`Grandparent`**.

Este ejemplo muestra cómo se pueden crear y utilizar relaciones jerárquicas en Solidity mediante la herencia multinivel, permitiendo a los contratos hijos acceder y sobrescribir propiedades y funciones de sus ancestros, promoviendo la reutilización y la modularidad del código.

**Herencia jerárquica**

La herencia jerárquica en Solidity se refiere a un patrón donde múltiples contratos hijos heredan de un único contrato padre, creando una estructura jerárquica en "forma de árbol" con un nodo raíz común. Este patrón permite compartir lógica común y propiedades entre varios contratos derivados, manteniendo al mismo tiempo diferencias específicas en cada uno de ellos.

<figure><img src="../../../.gitbook/assets/Herencia jerárquica.png" alt="" width="375"><figcaption></figcaption></figure>

Aquí te muestro un ejemplo de herencia jerárquica en Solidity:

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

// Contrato padre común
contract Vehicle {
    string public brand;
    string public model;

    constructor(string memory _brand, string memory _model) {
        brand = _brand;
        model = _model;
    }

    function getVehicleInfo() public view returns (string memory, string memory) {
        return (brand, model);
    }
}

// Primer contrato hijo que hereda de Vehicle
contract Car is Vehicle {
    uint public carMaxSpeed;

    constructor(string memory _brand, string memory _model, uint _maxSpeed)
        Vehicle(_brand, _model) {
        carMaxSpeed = _maxSpeed;
    }

    function getMaxSpeed() public view returns (uint) {
        return carMaxSpeed;
    }
}

// Segundo contrato hijo que hereda de Vehicle
contract Truck is Vehicle {
    uint public truckLoadCapacity;

    constructor(string memory _brand, string memory _model, uint _loadCapacity)
        Vehicle(_brand, _model) {
        truckLoadCapacity = _loadCapacity;
    }

    function getLoadCapacity() public view returns (uint) {
        return truckLoadCapacity;
    }
}
```

**Explicación del Código:**

* **`Vehicle`**: Este es el contrato base o "padre" que define propiedades comunes (**`brand`** y **`model`**) y una función (**`getVehicleInfo`**) aplicable a cualquier tipo de vehículo. Funciona como el nodo raíz común en la herencia jerárquica.
* **`Car` y `Truck`**: Estos son contratos "hijos" que heredan del contrato **`Vehicle`**. Cada uno de ellos extiende la funcionalidad base incluyendo propiedades específicas (**`carMaxSpeed`** para **`Car`** y **`truckLoadCapacity`** para **`Truck`**) y funciones adicionales para interactuar con esas propiedades (**`getMaxSpeed`** y **`getLoadCapacity`**, respectivamente). Aunque comparten algunas características comunes definidas en **`Vehicle`**, **`Car`** y **`Truck`** se especializan en diferentes aspectos de los vehículos que representan.

**Características de la Herencia Jerárquica:**

* **Especialización**: Permite que los contratos hijos se especialicen añadiendo o modificando funcionalidades específicas.
* **Organización Clara**: Facilita una estructura clara y lógica para los contratos, reflejando relaciones del mundo real y facilitando la comprensión y el mantenimiento del código.

Este patrón de herencia es especialmente útil en situaciones donde diferentes entidades comparten características comunes pero también necesitan sus propias implementaciones y características específicas.

**Herencia múltiple**

La herencia múltiple permite que un contrato herede comportamientos y características de múltiples contratos padres.

<figure><img src="../../../.gitbook/assets/Herencia múltiple.png" alt="" width="344"><figcaption></figcaption></figure>

La herencia múltiple puede ser muy poderosa pero también puede introducir complejidad y ambigüedades. Es importante diseñar cuidadosamente la arquitectura de los contratos para evitar problemas comunes como la colisión de nombres o la dependencia circular entre contratos.

Ejemplo:

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract A {
    function foo() public pure returns(string memory) {
        return "A";
    }
}

contract B {
    function bar() public pure returns(string memory) {
        return "B";
    }
}

contract C is A, B {
    function fooBar() public pure returns(string memory) {
        return string(abi.encodePacked(foo(), bar()));
    }
}
```

En este ejemplo, el contrato **`C`** hereda de ambos, **`A`** y **`B`**, y tiene acceso a sus funciones **`foo`** y **`bar`**, respectivamente. El contrato **`C`** introduce una nueva función **`fooBar`** que combina las salidas de las funciones heredadas.
