# <a name="cpu-heatmap"></a>Mapa térmico de la CPU

Este libro es una buena forma de visualizar las zonas activas de uso de la CPU de las máquinas virtuales.

![Imagen de un mapa térmico de CPU](cpu-heatmap.png)

## <a name="changing-the-cpu-threshold"></a>Cambio del umbral de CPU
De forma predeterminada, este libro destaca las máquinas virtuales con un promedio de `Percentage CPU` superior al 75 %. Si desea aumentar o disminuir este umbral, siga estos pasos:

1. Haga clic en el elemento `Edit` de la barra de herramientas.
2. Haga clic en el botón `↑ Edit` de la esquina inferior derecha del control Hive.
3. En la lista `Columns Available After Merge`, desplácese hacia abajo y seleccione el elemento `[Added column] - Cell Color`.
4. Haga clic en el botón `Edit added item` de la barra de herramientas de controles Hive.
5. En el panel que se abre, haga clic en `Edit` en el elemento que dice `Percentage CPU > 75 Result is E8976A` para ver una ventana emergente de configuración.
    1. Establezca el campo `Second operand` en el umbral que desea, por ejemplo, 90.
        ![Imagen de un mapa térmico de CPU](cpu-heatmap-column-settings.png)
    2. Haga clic en op en el menú emergente.
6. Haga clic en "Guardar y cerrar".
7. Elija `Done Editing` en la barra de herramientas del libro.
