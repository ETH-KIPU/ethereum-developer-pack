# Tipos de cuentas

Hay dos tipos de cuentas:

* **EOA (Externally Owned Account).** Una cuenta de un usuario de Ethereum, controlada por cualquiera que tenga las claves privadas.
* **Contract Account**. Un cuenta de que tiene código ejecutable de contrato inteligente y es controlada por la lógica de su código. No tiene claves privadas.

Ambos tipos de cuentas pueden:

* Recibir, mantener y enviar ETH y tokens.
* Interactuar con smart contracts existentes.

Sin embargo, tienen diferencias importantes.

|                        | Cuenta EOA                          | Contract Account                                                             |
| ---------------------- | ----------------------------------- | ---------------------------------------------------------------------------- |
| Costo de creación      | Ninguno                             | Tiene costo porque utiliza almacenamiento de la blockchain                   |
| Transacciones posibles | Puede iniciar cualquier transacción | Solo puede enviar transacciones en respuesta a una transacción recibida      |
| Interacción con EOAs   | Solo transferencias de ETH          | Una EOA puede enviarle transacciones que disparen la ejecución de su código. |

La dirección Ethereum (address) de una EOA se genera a partir de su clave privada. Primero se crea la clave pública con un primer cifrado y luego la dirección, aplicando un hash a la clave pública (ver claves públicas y privadas en la clase anterior).

Las direcciones de las _contract accounts_ también tienen 42 caracteres y empiezan con «0x», pero se generan criptográficamente a partir de la dirección de la cuenta del creador del contrato y del número de transacciones que se han enviado desde esa dirección (nonce).
