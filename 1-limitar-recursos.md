# Limitar el uso de recursos
En Docker, limitar el uso de recursos se refiere a establecer restricciones específicas en los recursos del sistema (como CPU, memoria, E/S de disco, etc.) que un contenedor puede utilizar.

- Prevención de abusos: Un contenedor sin restricciones podría consumir una cantidad desproporcionada de recursos del sistema, como CPU, memoria o ancho de banda de red, afectando negativamente el rendimiento de otros contenedores o aplicaciones que se ejecutan en el mismo sistema host.
- Aislamiento y seguridad: Establecer límites de recursos ayuda a aislar los contenedores entre sí, reduciendo el riesgo de que un contenedor sobrecargado o comprometido afecte a otros contenedores o al sistema host.
- Gestión eficiente de recursos: En entornos de producción o en la nube, los recursos suelen ser limitados y costosos. Limitar el uso de recursos permite una distribución más eficiente y equitativa de los recursos disponibles entre múltiples contenedores y aplicaciones.
- Estabilidad y rendimiento: La limitación de recursos ayuda a evitar situaciones en las que la contención de recursos pueda llevar a la degradación del rendimiento o incluso a la caída del sistema. Esto es especialmente importante en entornos de producción donde la estabilidad y el rendimiento son críticos.
- Cumplimiento de políticas: En algunos casos, puede haber políticas organizativas o regulaciones que exijan la limitación del uso de recursos para cumplir con estándares de seguridad, rendimiento o costos.

Limitar el uso de recursos en Docker es una práctica importante para garantizar un entorno de contenedores seguro, estable y eficiente, y para asegurar que los recursos del sistema se utilicen de manera óptima.
