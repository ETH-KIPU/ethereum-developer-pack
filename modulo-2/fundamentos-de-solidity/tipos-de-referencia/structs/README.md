# Structs

Son estructuras de datos complejas que permiten agrupar varias variables, posiblemente de diferentes tipos, bajo una única entidad. Esto los hace particularmente útiles para modelar objetos o conceptos con múltiples atributos dentro de los contratos inteligentes. Al igual que en otros lenguajes de programación como C, C++, o JavaScript, los **`structs`** ofrecen una forma de organizar datos que están lógicamente relacionados en una estructura comprensible y manejable.

**Características de los Structs**

* **Tipos de Datos Compuestos**: Permiten combinar varios tipos de datos, incluidos otros **`structs`**, **`arrays`**, y **`mappings`**, en una sola unidad.
* **Personalizables**: Los desarrolladores pueden definir **`structs`** según las necesidades específicas de su aplicación, eligiendo los tipos de datos y nombres de variables que mejor se ajusten a su caso de uso.
* **Flexibles**: Se pueden utilizar dentro de **`arrays`** y **`mappings`** para crear estructuras de datos complejas y dinámicas.

**Declaración**

Para declarar un **`struct`**, se utiliza la palabra clave **`struct`**, seguida del nombre de la estructura y un bloque de código que define los miembros de la estructura. Cada miembro puede tener un tipo de dato diferente.

```solidity
struct Persona {
    string nombre;
    uint edad;
    bool estaActivo;
}
```

**Uso**

Una vez declarado, el **`struct`** se puede utilizar para crear variables de ese tipo dentro del contrato, permitiendo almacenar y gestionar datos estructurados de manera eficiente.

```solidity
contract MiContrato {
    // Instancia de un struct
    Persona public persona;

    // Inicializar una instancia del struct
    function crearPersona(string memory _nombre, uint _edad) public {
        persona = Persona(_nombre, _edad, true);
    }

    // Actualizar un campo específico del struct
    function actualizarEdad(uint _edad) public {
        persona.edad = _edad;
    }
}
```

**Ejemplos de Uso Común**

Los **`structs`** se utilizan ampliamente en aplicaciones descentralizadas para representar entidades complejas, como:

* **Usuarios o Perfiles**: Agrupar información relevante de usuarios, como nombre, dirección de correo electrónico, y balance de tokens.
* **Productos o Servicios**: Modelar productos en un marketplace, incluyendo su precio, descripción, y disponibilidad.
* **Operaciones o Transacciones**: Registrar detalles de transacciones, como el remitente, receptor, cantidad y estado.

**Consideraciones**

* **Gestión de Memoria**: En Solidity, los **`structs`** pueden almacenarse en **`storage`**, **`memory`**, o **`calldata`**, dependiendo de su uso y ciclo de vida. Es importante elegir el lugar adecuado de almacenamiento para optimizar el uso de gas y la eficiencia del contrato.
* **Límites y Restricciones**: Aunque los **`structs`** son poderosos, su uso incorrecto puede llevar a un aumento en el costo del gas o a dificultades en la gestión de datos. Por ejemplo, los **`structs`** anidados o las estructuras de datos muy complejas pueden aumentar la complejidad y el costo de las operaciones.
