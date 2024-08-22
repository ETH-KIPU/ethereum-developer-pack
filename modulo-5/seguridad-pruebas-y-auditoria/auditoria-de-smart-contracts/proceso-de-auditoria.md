# Proceso de Auditoría

Un proceso de auditoría consiste de los siguientes pasos:

1. Realizar pruebas.
2. Leer especificaciones y documentación.
3. Utilizar herramientas para pruebas (como slither, linters, análisis estático, etc).
4. Análisis manual.
5. Correr herramientas de pruebas más exhaustivas (como echidna, manticore, ejecución simbólica, MythX, etc.)
6. Discutir (y repetir los pasos anterior según se requiera)
7. Escribir un reporte de hallazgos ([Ejemplo](https://github.com/transmissions11/solmate/tree/main/audits))

Normalmente, el reporte se organiza clasificando los hallazgos según su probabilidad de ocurrencia y el tamaño del impacto que pueden tener, siendo los hallazgos críticos y de alta probabilidad/impacto los que deben corregirse con prioridad.

<figure><img src="../../../.gitbook/assets/impact.png" alt=""><figcaption></figcaption></figure>
