# Una mentalidad distinta de diseño

El desarrollo de aplicaciones en un entorno blockchain tiene sus particularidades, como por ejemplo el hecho de que el costo de fallar es muy caro. En tal sentido, se parece más a la construcción de hardware que al desarrollo de software.

Se requiere una mentalidad de diseño particular, que implica tener en cuenta los siguientes principios:

**Prepárate para que ocurran fallas:** Tú código debe estar preparado para responder a escenarios de errores y vulnerabilidades, manejandolos adecuadamente.

* Tener un “rompe-circuitos” que pause el contrato cuando las cosas se ponen mal.
* Gestionar la cantidad de dinero en riesgo, limitando por ejemplo el número de transacciones que se hacen desde una misma dirección IP en un tiempo determinado (rate limiting).
* Tener una ruta de upgrades para corregir errores e implementar mejoras.

**Mantente actualizado:** Conoce los avances más recientes en seguridad.

* Tan pronto se identifica una nueva vulnerabilidad revisa tus contratos.
* Actualiza Upgrade to the latest version of any tool or library as soon as possible
* Adopta técnicas de seguridad que sean útiles.

**Keep it simple:** La complejidad aumenta la probabilidad de errores.

* Asegura que la lógica de tus contratos sea simple.
* Modulariza tu código para que los contratos y funciones sean pequeños.
* Usa librerías que haya soportado el paso del tiempo.
* Elige claridad sobre desempeño siempre que sea posible.
* Usa blockchain sólo para los componentes donde tenga sentido la descentralización.

**Pon a prueba tu código:** Siempre es mejor detectar errores tempranamente.

* Prueba tus contratos de forma exhaustiva y añade nuevas pruebas si aparecen nuevos vectores de ataque.
* Si está entre tus posibilidades convoca _bug bounties_.
* Haz un despliegue en fases, incrementado las pruebas y la actividad en cada pase.

**Ten en cuenta las características de blockchain:** Ethereum tiene sus particularidades y no hay que olvidarlas.

* Ten sumo cuidado con las llamadas a contratos externos, ya que podrían ejecutar código malicioso y alterar el flujo de control.
* Recuerda que tus funciones públicas son accesibles para todos y pueden ser llamadas de forma maliciosa y en cualquier orden. Además, los datos privados en los contratos inteligentes son visibles para cualquiera.
* Ten en cuenta los costos de gas y el límite de gas por bloque.
* La aleatoriedad no es algo resuelto en blockchain; la mayoría de los enfoques para la generación de números aleatorios son manipulables.
