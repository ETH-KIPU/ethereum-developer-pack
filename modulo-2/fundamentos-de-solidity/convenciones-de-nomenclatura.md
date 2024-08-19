# Convenciones de nomenclatura

A continuación, presentamos algunas de las convenciones más comunes utilizadas en la comunidad de Solidity:

1. **Contratos**: Los nombres de los contratos suelen estar en UpperCamelCase (también conocido como PascalCase). Cada palabra en el nombre del contrato comienza con una letra mayúscula sin espacios. Ejemplo: **`MySmartContract`**.
2. **Funciones**: Las funciones suelen nombrarse usando lowerCamelCase. La primera palabra comienza con una minúscula y las siguientes palabras comienzan con mayúsculas. Ejemplo: **`myFunction`**.
3. **Variables de Estado**: Las variables de estado también suelen usar lowerCamelCase. Estas son las variables que se almacenan permanentemente en el almacenamiento del contrato. Ejemplo: **`myVariable`**.
4. **Variables Locales y Parámetros**: Al igual que las variables de estado, las variables locales y los parámetros de las funciones también se nombran usando lowerCamelCase. Ejemplo: **`localVariable`**, **`functionParameter`**.
5. **Constantes**: Las constantes se nombran con todas las letras en mayúsculas y palabras separadas por guiones bajos (snake\_case en mayúsculas). Ejemplo: **`MAX_COUNT`**, **`TOTAL_SUPPLY`**.
6. **Enums**: Los nombres de los enumerados (enums) suelen seguir UpperCamelCase, al igual que los nombres de los contratos. Ejemplo: **`TokenState`**.
7. **Eventos**: Los eventos generalmente siguen UpperCamelCase. Ejemplo: **`TransferCompleted`**.
8. **Modificadores**: Para los modificadores se utiliza lowerCamelCase, similar a las funciones. Ejemplo: **`onlyOwner`**.
9. **Structs**: Los nombres de structs generalmente se escriben en UpperCamelCase, siguiendo la misma convención que los contratos y enums. Ejemplo: **`PlayerInfo`**.

Estas convenciones no son obligatorias pero son ampliamente adoptadas en la comunidad de desarrollo de Solidity. Seguir estas prácticas estándar ayuda a mantener el código fuente de los contratos inteligentes organizado y fácil de entender para otros desarrolladores que puedan trabajar con tu código o revisarlo. Además, estas convenciones ayudan a distinguir rápidamente entre diferentes tipos de entidades en el código, como contratos, funciones y variables.
