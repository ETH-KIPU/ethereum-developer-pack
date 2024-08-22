# Replay attack (ataque de repetición)

Un ataque de repetición firmado en un contrato inteligente es un tipo de vulnerabilidad de seguridad en la que un atacante intercepta un mensaje firmado válido y luego lo reproduce en el contrato. El mensaje típicamente está firmado con una clave privada y contiene instrucciones ejecutables para el contrato inteligente. Al reproducir el mensaje, un atacante puede engañar al contrato inteligente para que ejecute las mismas instrucciones nuevamente, lo que puede resultar en un comportamiento no deseado o incluso en pérdidas financieras.

**Ejemplo:**

Un contrato inteligente que permite a los usuarios retirar fondos de su cuenta mediante el envío de un mensaje autorizado y firmado. Un atacante podría interceptar un mensaje de retiro válido de un usuario legítimo y luego reproducir ese mensaje más tarde. Aunque el contexto haya cambiado, el contrato inteligente asumiría que el mensaje firmado sigue siendo válido porque ha sido firmado digitalmente. Esto podría permitir al atacante retirar fondos de la cuenta del usuario.

**Mitigación:**

Para prevenir los ataques de repetición firmados, los desarrolladores de contratos inteligentes pueden implementar varias medidas de seguridad, incluyendo el uso de identificadores de mensaje únicos y la adición de restricciones basadas en el tiempo para la validez de los mensajes firmados.
