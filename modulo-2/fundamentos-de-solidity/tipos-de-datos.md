# Tipos de Datos

Solidity es un lenguaje de tipado estático. Esto significa que el tipo de una variable se determina al momento de la compilación y no puede cambiar durante la ejecución del programa. En Solidity, cuando declaras una variable, es necesario especificar su tipo de manera explícita, y este tipo no cambia a lo largo del código una vez establecido.

Por ejemplo una variable denominada **myNumber** de tipo entero se declara de la siguiente manera.

```solidity
uint256 myNumber; 
```

Este enfoque proporciona beneficios como la detección temprana de errores y la optimización de rendimiento, ya que el compilador puede realizar verificaciones y optimizaciones específicas basadas en el tipo de datos conocido. Por el contrario, lenguajes de tipado dinámico como Python o JavaScript permiten que el tipo de una variable pueda cambiar durante la ejecución del programa.

Existen dos tipos de datos: tipos de valor y tipos de referencia.

### Tipos de valor

Este tipo de datos almacenan directamente el valor.

A continuación indicamos los que corresponden a esta categoría.

#### Enteros sin signo (uint)

Corresponde a números enteros no negativos (unsigned) Ejemplo:

```solidity
uint256 varInt = 25; // se define un uint con valor 25
```

Dependiendo del tamaño de tus datos puedes elegir desde 8 bits hasta 256 bits, en saltos de 8. Por ejemplo, uint8, uint16, uint24 hasta uint256.

Los uint8 solo cubren valores de 0 a 255 (2^8-1).

🚨 Si la variable toma un valor fuera de estos extremos ocurrirá un error en la ejecución y la transacción se revertirá.

Si quisiéramos asignar a este tipo de datos el valor máximo posible tendríamos que utilizar la siguiente instrucción

```solidity
uint256 maxVal = type(uint256).max;
```

💡Si al momento de escribir tu código sólo indicas “uint” sin señalar el número de bits, se asumirá que corresponde a uint256.

🥸 El valor por defecto de este tipo de dato es cero.

#### Enteros con signo (int)

Números enteros con signo. Ejemplo:

```solidity
int8 varInt = -4; // se define un int con valor -4
```

Aplica las mismas consideraciones respecto al número de bits que en el caso de los uint.

Sin embargo, en este caso int8 cubre los valores entre -128 y 127 (256 valores enteros total).

🥸 Su valor por defecto es cero.

#### Booleanos (bool)

Valores booleanos, es decir, true o false.

Ejemplo:

```solidity
bool isAllowed = true; // se define isAllowed como verdadero
```

🥸 Su valor por defecto es “false”.

#### Dirección (address)

Direcciones de 20 bytes en Ethereum. Representan direcciones públicas de EOA o contratos inteligentes.

Ejemplo:

```solidity
address owner = 0x742d35Cc6634C0532925a3b844Bc454e4438f44e;
```

💡 Una dirección Ethereum tiene 40 caracteres hexadecimales y un prefijo “0x” que indica que el número que sigue a continuación es un hexadecimal.

🥸 Su valor por defecto es: 0x0000000000000000000000000000000000000000 que es conocido también como 0x0, address(0) o address(0x0).

#### Enumeración (enum):

Ofrece una forma legible de asignar valores significativos a variables. Los enums son útiles para definir estados o categorías específicas. Esto proporciona claridad y facilita la comprensión del código al asignar significado semántico a los valores. Los enums mejoran la legibilidad, la mantenibilidad y evitan errores asociados con el uso de valores numéricos directos.

```solidity
enum Estado = {Pendiente, Aprobado, Rechazado};
```

### Tipos de referencia - Bytes y Strings

Los tipos de referencia son tipos de datos más complejos que almacenan una referencia a una ubicación de memoria. Cuando se asignan a otras variables o se pasan como argumentos a funciones, se pasa la referencia, no una copia de los datos subyacentes.

Cuando se manejan estos tipos en funciones, especialmente cuando se pasan como argumentos o se devuelven desde funciones, es importante tener en cuenta que se manejan por referencia. Esto significa que si modificas un string o un bytes dentro de una función, podrías estar modificando el dato original, no una copia de él.

Los tipos de referencia que veremos por ahora son los bytes y los strings.

#### Bytes (bytes):

Es un tipo de datos que se utiliza para almacenar secuencias de bytes. Solidity proporciona bytes como una forma de almacenar datos binarios arbitrarios.

Pueden ser de dos tipos: estáticos o dinámicos.

**Estáticos (bytesN)**: Donde 'N' puede ser cualquier número entero entre 1 y 32. Estos son tipos de bytes de longitud fija. Por ejemplo, **`bytes1`**, **`bytes2`**, **`bytes32`**, etc. Cada uno de estos almacena una secuencia de bytes de longitud exactamente 'N'. Son útiles cuando sabes la longitud exacta de los datos que necesitas almacenar, como hashes o identificadores.

```solidity
// Definiendo una variable de tipo bytes32
bytes32 public exampleBytes32;
```

**Dinámicos (bytes)**: Este es un tipo de datos dinámico que puede almacenar una secuencia de bytes de longitud variable. Es similar a un array de **`bytes1`**, pero su longitud no está fijada y puede cambiar. Este tipo es más flexible pero también más costoso en términos de gas.

**`bytes`** es especialmente útil para almacenar datos que no tienen una longitud fija predecible, como cadenas de texto arbitrarias o datos binarios.

```solidity
contract Example {
// Definiendo una variable de tipo bytes (dinámica)
bytes public dynamicBytes;

function setBytesValue() public {
    // Asignando un valor a la variable dynamicBytes
    // Puede ser de cualquier longitud
    dynamicBytes = "Hello, Solidity!";
}
}
```

🥸 Su valor por defecto en formato entero es 0, en formato hexadecimal es 0x00.

#### Cadenas (string):

Este tipo está diseñado específicamente para almacenar cadenas de caracteres. Los string en Solidity son dinámicos y pueden cambiar de tamaño. Representa secuencias de caracteres Unicode de longitud variable.

Si la cadena es de menos de 32 bytes, es más eficiente guardarla un formato estático de bytes como **`bytes32`** . Esto simplifica el procesamiento dado que la memoria se asigna de forma anticipada. Por el contrario, \*\*`string`\*\*y **`bytes`** asignan la memoria de forma dinámica dependiendo del tamaño de la cadena.

```solidity

contract StringExample {
//Definiendo una variable de tipo string
    string public exampleString;

// y una función que le asignará un valor de cualquier tamaño al string
    function setString(string memory newString) public {
        exampleString = newString;
    }
}
```

🚨 Los string están codificados en UTF-8. Un carácter en UTF-8 puede tener más de un byte, por ejemplo una vocal con tilde, por ello normalmente el compilador nos mostrará un error al utilizar tildes en una cadena de caracteres asignada a una variable string.

Las operaciones con string pueden ser costosas en términos de gas, especialmente cuando se trata de cadenas grandes. Esto se debe a que el almacenamiento y la manipulación de cadenas requieren un uso intensivo de la memoria.

🚨 Solidity no permite la comparación directa de cadenas con **`==`** . En su lugar, debes comparar sus hashes (por ejemplo, usando keccak256).

💡 Puedes declarar variables string en el almacenamiento (para datos persistentes) o en la memoria (para datos temporales). El uso de la memoria es más económico en términos de gas, pero su alcance está limitado al contexto de la función. Por ello normalmente al definir un parámetro string para una función se utiliza la palabra **`memory`** (ver ejemplo anterior), para especificar que el string se guardará en memoria.

🥸 Su valor por defecto es una cadena vacía.
