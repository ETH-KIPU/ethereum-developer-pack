# Mappings

Los mappings en Solidity son una estructura de datos de tipo “key - value” (clave-valor) que proporciona una forma eficiente y flexible de almacenar y acceder a datos dentro de contratos inteligentes. Son comparables a los diccionarios o mapas en otros lenguajes de programación, donde cada clave única se asocia con un valor. Los mappings son particularmente útiles en el desarrollo de aplicaciones descentralizadas (DApps) para gestionar conjuntos de datos como balances de cuentas, relaciones entre entidades, y otros tipos de datos estructurados.

**Características de los Mappings**

* **Claves Únicas**: Cada clave en un mapping debe ser única y se utiliza para acceder a un valor específico. Las claves pueden ser de cualquier tipo primitivo como **`address`**, **`uint`**, **`bytes`**, etc. No se permiten tipos de datos complejos como arrays o structs como claves.
* **Valores Dinámicos**: Los valores almacenados en un mapping pueden ser de cualquier tipo, incluyendo tipos primitivos, arrays, structs, e incluso otros mappings.
* **Inicialización por Defecto**: Los mappings no tienen un tamaño fijo y no se puede obtener su longitud. Todos los posibles valores están virtualmente presentes y se inicializan por defecto al valor por defecto de su tipo (por ejemplo, **`0`** para **`uint`**, **`false`** para **`bool`**, etc.).
* **Privacidad**: No se puede iterar sobre los mappings ni obtener una lista de sus claves directamente en Solidity. Cada acceso a un valor debe hacerse mediante una clave conocida.

**Declaración**

Un mapping se declara especificando el tipo de las claves y el tipo de los valores. La sintaxis general es:

```solidity
mapping(tipoClave => tipoValor) public nombreMapping;
```

**Ejemplo Básico**

```solidity
pragma solidity ^0.8.0;

contract MyContract {
    // Declaración de un mapping que asocia direcciones con balances
    mapping(address => uint) public balances;

    // Actualiza el balance de la address que solicita la actualización
    function updateBalance(uint newBalance) public {
        balances[msg.sender] = newBalance;
    }

    // Lee el balance de una dirección
    function getBalance(address user) public view returns (uint) {
        return balances[user];
    }
}
```

En el ejemplo anterior creamos un mapping denominado **`balances`** conformado por pares donde la clave es un address y el valor es un entero sin signo.

Para actualizar el balance de una address específica utilizamos la expresión

**Uso común de Mappings en contratos inteligentes**

* **Balances de Tokens**: Para llevar un registro de los balances de criptomonedas o tokens ERC-20 de cada dirección.
* **Permisos y Roles**: Para gestionar permisos o roles específicos asignados a diferentes direcciones.
* **Relaciones entre Entidades**: Para mapear relaciones uno-a-uno o uno-a-muchos entre diferentes entidades, como usuarios y sus activos o tareas.

**Consideraciones de Seguridad**

Al trabajar con mappings, es crucial gestionar correctamente el acceso y las actualizaciones a los datos para prevenir vulnerabilidades de seguridad. Por ejemplo, cuando se actualiza un valor en un mapping, asegúrate de que solo las entidades autorizadas puedan realizar dicha actualización, utilizando modificadores de acceso o verificaciones de permisos adecuadas.
