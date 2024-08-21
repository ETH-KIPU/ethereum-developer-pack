---
icon: person-running
---

# Ejercicio

Es hora de interactuar con un contrato que hayas desplegado en las clases del Módulo 2. Elige un contrato y una función a ejecutar dentro de ese contrato.

Necesitarás lo siguiente:

* Una URL de acceso a Sepolia.
* La clave privada de la wallet.
* La dirección del contrato.
* El ABI del contrato desplegado.
* El nombre de la función y los parámetros que requiere para su ejecución.

&#x20;💡 **TIP:** Necesitarás incluir los siguientes métodos en tu script

```jsx
const contract = new ethers.Contract(contractAddress, abi, provider);
const txResponse = await contract.someWriteFunction(params, { gasLimit, gasPrice });
```

Recuerda utilizar el archivo `.env` para guardar las variables de entorno y para proteger la privacidad de tu clave privada.

Incluye sentencias `console.log` para asegurar que el script se ejecutó correctamente y para mostrar el resultado de la función ejecutada.
