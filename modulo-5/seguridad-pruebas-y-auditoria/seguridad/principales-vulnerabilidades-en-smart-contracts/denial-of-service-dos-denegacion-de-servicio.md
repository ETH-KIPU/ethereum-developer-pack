# Denial of Service (DoS - Denegación de Servicio)

Un ataque de denegación de servicio ocurre cuando una función de un contrato se bloquea debido a que un actor malintencionado hace que se consuma todo el gas o explota una vulnerabilidad lógica para impedir la ejecución de la función.

**Ejemplo**: Un atacante podría aprovechar una lista de beneficiarios y enviar transacciones maliciosas para impedir que otros beneficiarios reciban sus pagos.

**Mitigación**:

* Evitar bucles sin límites.
* Limitar la cantidad de gas disponible para las transacciones.
* Diseñar smart contracts de manera que no dependan de la ejecución secuencial de tareas críticas.
