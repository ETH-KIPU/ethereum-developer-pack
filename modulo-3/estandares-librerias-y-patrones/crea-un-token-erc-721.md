# Crea un Token ERC-721

Es el momento de crear nuestro primer NFT. Utilizaremos nuevamente Remix y subiremos la imagen y metadatos a IPFS. Empecemos

### **Paso 1: Configurar el entorno**

Utilizaremos Remix.

1. **Instalación de OpenZeppelin**: Esta operación ya la hicimos para la creación del token ERC-20, así que ya tendremos acceso a la librería de contratos de Open Zeppelin.

### **Paso 2: Crea una imagen o utiliza una existente**

Crea una imagen con tu software favorito (Canva, Dall-e, Pain, etc.).

En nuestro caso utilizaremos la siguiente imagen en un formato cuadrado.

<figure><img src="../../.gitbook/assets/Modulo 3-4.png" alt="" width="375"><figcaption></figcaption></figure>

### **Paso 3: Subimos nuestra imagen a IPFS**

Utilizaremos Pinata ([https://www.pinata.cloud/](https://www.pinata.cloud/)) que nos permita subir archivos a IPFS de forma sencilla y gratuita. Es necesario crear una cuenta.

Una vez hayas ingresado, vas a la sección Files y subes (Upload)el archivo.

Obtendrás un CID (Content Identifier) que es el código que representa de forma única a tu imagen y que a la vez es el hash de ella.

Para nuestro ejemplo, el CID de la imagen es: Qmdae238wjRaCrNuBzNTJDeJNbuemGWQB9tioDKrrjosMY

### **Paso 4: Creamos el JSON del NFT y lo subimos a IPFS**

Del módulo de Fundamento de Solidity, donde aprendimos que los ABI se escriben en formato JSON, recordarás que JSON significa JavaScript Object Notation y que es un formato de intercambio de datos ligero y de fácil lectura para humanos, pero también fácil de generar y analizar por las máquinas.

Dentro de la creación de NFTs se utilizan mucho los JSON para guardar los metadatos de los NFTs.

El objeto JSON que utilizaremos será muy sencillo en esta oportunidad:

```json
{
    "name": "Mi primer NFT",
    "description": "Un NFT creado en el Ethereum Developer Pack",
    "image": "<https://ipfs.io/ipfs/Qmdae238wjRaCrNuBzNTJDeJNbuemGWQB9tioDKrrjosMY>"
}
```

En este JSON definimos un nombre y descripción para este primer NFT, añadiremos también la referencia de IPFS donde está almacenada la imagen correspondiente.

Asegúrate de reemplazar el hash Qmdae238wjRaCrNuBzNTJDeJNbuemGWQB9tioDKrrjosMY del ejemplo, por el CID de tu imagen (que obtuviste en Pinata en el paso anterior).

Esta es la única forma que tienen de enterarse de la información de tu NFT los marketplaces de NFTs como OpenSea o Rarible.

Ahora es necesario subir este objeto JSON a IPFS. Para ello copia el contenido completo del objeto en un archivo con extensión json. Para ello utiliza tu editor de texto favorito.

En nuestro caso denominaremos al archivo MiNFT.json y lo subiremos a IPFS utilizando Pinata.

Obtenemos el CID QmVjHeKjQk5snH1KAcVUPXzwSrW2e7oX1KmVQHHoPwKkuJ

### **Paso 5: Creamos el smart contract y lo desplegamos en Sepolia**

Utilizaremos la librería de Open Zeppelin para obtener alguno de los contratos ERC-721.

Utilizamos Remix para desplegar el siguiente contrato en Sepolia. Antes de desplegarlo asegúrate de reemplazar el CID QmVjHeKjQk5snH1KAcVUPXzwSrW2e7oX1KmVQHHoPwKkuJ por el CID del JSON que creaste en el paso anterior.

También puedes personalizar en el constructor el nombre del NFT y su identificador o símbolo.

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

import "@openzeppelin/contracts/token/ERC721/ERC721.sol";

contract MyNFT is ERC721 {
    uint256 token_count;

    constructor() ERC721("My NFT", "MNFT") {}

    function tokenURI(uint256 tokenId) public view virtual override returns (string memory) {
        require(_ownerOf(tokenId) != address(0), "ERC721Metadata: URI query for nonexistent token");
        return "<https://ipfs.io/ipfs/QmVjHeKjQk5snH1KAcVUPXzwSrW2e7oX1KmVQHHoPwKkuJ>";
    }

    function mintNFT(address to) public
    {
        token_count  += 1;
        _mint(to, token_count);
    }
}
```

Una vez desplegado el contrato en Sepolia. Estarán disponibles en Remix las funciones para ejecutar con el contrato.

<figure><img src="../../.gitbook/assets/Modulo 3-5.png" alt="" width="375"><figcaption></figcaption></figure>

### **Paso 6: Mintear el NFT**

Crearemos el primer NFT utilizando la función **`mintNFT` .** Debemos indicar la dirección del destinatario **`to` ,** para el ejemplo utilicemos nuestra misma address, de esa forma seremos el propietario de ese primer NFT minteado.

Aprobamos la transacción y ¡habremos minteado nuestro primer NFT!

Recuerda que si validas y publicas este contrato en Etherscan podrás acceder a estas mismas funciones desde ese explorador de bloques.

### **Paso 7: Verifica que tu NFT ha sido creado**

Para verificar que nuestro NFT ha sido creado y que toda la información está correcta podemos ingresar a un marketplace de NFT como OpenSea.

Para ello ingresamos al entorno de pruebas de OpenSea ya que hemos desplegado nuestro contrato en una testnet como es Sepolia. La dirección es: [https://testnets.opensea.io/](https://testnets.opensea.io/)

Nos conectamos con la misma wallet con la que hicimos el minteo del NFT.

Vamos a la sección de datos de la cuenta y seleccionamos nuestro perfil (Profile) para ver los NFT que son de nuestra propiedad.

<figure><img src="../../.gitbook/assets/Modulo 3-6.png" alt=""><figcaption></figcaption></figure>

Si seguimos los pasos correctamente, podremos encontrar nuestro NFT en el marketplace.

Haciendo click en él, podremos verificar que los metadatos están registrados correctamente.

<figure><img src="../../.gitbook/assets/Modulo 3-7.png" alt=""><figcaption></figcaption></figure>

Podemos hacer la misma verificación en Rarible: [https://testnet.rarible.com/](https://testnet.rarible.com/)

Felicitaciones por haber completado este reto.

### 🫶🏻 **Nota:** Agradecemos a Ahmed Casto (@filosofiacodigo) por darnos la inspiración para desarrollar esta sección.
