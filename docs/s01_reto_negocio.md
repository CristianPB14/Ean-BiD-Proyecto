# reto_negocio.md

## Reto de negocio

**Decisión Recomendada**

Rechazar la compra inmediata del clúster y mantener el equipo actual, implementando una política de almacenamiento híbrido con una ventana móvil de 36 horas para el detalle operativo.

**Estrategia ante la Saturación**
Frente a las opciones tecnológicas disponibles, aplicaremos la primera salida ante la saturación: **reducir el dato**. El sistema conservará la telemedición horaria de los 180.000 medidores únicamente durante las últimas 36 horas, garantizando que el área de operaciones pueda detectar fugas en tiempo casi real. Pasado este período, el sistema descartará el volumen innecesario y archivará exclusivamente un consolidado mensual por medidor junto con los registros de anomalías, unificando la necesidad operativa con la restricción técnica.

**Evidencia Numérica**
El colapso actual no es un fallo del software, sino un desbordamiento matemático. Guardar el detalle horario genera archivos mensuales de 7.8 GB en disco. Debido al empaquetamiento de las estructuras, estos datos se expanden 5.4 veces al cargarse en la memoria RAM ($k=5.4$). Al cruzar esto con nuestro crecimiento mensual (4%) y la memoria útil del equipo (12 GB), el cálculo demuestra que cruzamos el umbral de saturación hace 32 meses ($t_{umbral} = -32$). Al cambiar la estrategia y purgar el histórico horario, el peso real a almacenar a largo plazo cae a 0.0108 GB.

**Horizonte Operativo**
Esta decisión elimina el déficit actual y nos proporciona un margen de operatividad comprobado de 136 meses (más de 11 años) usando exactamente el mismo computador.

**Sensibilidad al Crecimiento Acelerado**

Si la demanda se dispara y la tasa de crecimiento mensual duplica su velocidad (pasando del 4% al 8%), nuestra recomendación estratégica se mantiene. El horizonte de vida útil se reduciría a unos 60 meses, dándonos una ventana de 5 años para planificar una simple y económica actualización de memoria RAM (escalamiento vertical), demostrando que un clúster complejo sigue siendo innecesario a corto y mediano plazo.

*Declaración de uso de inteligencia artificial: Se utilizó el asistente Gemini para la estructuración ejecutiva y corrección de estilo de este documento. Las cifras que sostienen el argumento $(S_0, k=5.4, M=12 GB, t_{umbral} = -32    y    136   meses)$ fueron extraídas y verificadas manualmente contra las métricas empíricas del caso de estudio antes de su integración.*