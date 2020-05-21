# <a name="picking-a-set-of-resources-to-analyze-in-workbooks"></a>Selección de un conjunto de recursos para analizar en libros

La plantilla `Resource Picker` le permite empezar a usar la suscripción, el grupo de recursos y los parámetros de recursos para configurar el contexto de entrada del libro. Los parámetros predeterminados se establecen para seleccionar máquinas virtuales, pero puede configurarlos para seleccionar cualquier tipo de recurso. La plantilla también tiene un control de consulta ARG que muestra cómo usar los parámetros en los análisis.

![Imagen](Full.png)

## <a name="setting-up-the-resource-type-to-pick"></a>Configuración del tipo de recurso que desea seleccionar

1. Use `Edit` en la barra de herramientas del libro.
2. Ahora podrá ver una lista desplegable `Resource type` antes de `Subscriptions`:

    ![Imagen](Parameter.png)
3. Expanda la lista desplegable y seleccione los tipos de recursos que desea seleccionar. Esto actualizará la suscripción, el grupo de recursos y la lista desplegable de recursos para que coincidan con la selección.
4. Haga clic en el botón `Edit` situado en la parte inferior derecha del control de parámetros.
5. En la cuadrícula de parámetros, para la fila `Resources`, cambie la columna `Display name` de _Máquinas virtuales_ al nombre descriptivo del tipo de recurso seleccionado (por ejemplo, _Cuentas de almacenamiento_)
6. Haga clic en `Done Editing` en la barra de herramientas del libro.

## <a name="selecting-more-or-less-than-10-resources-by-default"></a>Selección de aproximadamente 10 recursos de forma predeterminada

1. Use `Edit` en la barra de herramientas del libro.
2. Haga clic en el botón `Edit` situado en la parte inferior derecha del control de parámetros.
3. En la cuadrícula de parámetros, seleccione la fila de `Resources`
4. Haga clic en el icono de `Edit` (icono de lápiz) en la barra de herramientas del control.
5. En el panel `Edit Parameter` que aparece, desplácese hacia abajo hasta el editor de `Azure Resource Graph Query`. Debe tener una consulta similar a la siguiente:
    ```sql
    Resources
    | where type in~({ResourceTypes})
    | extend resourceGroupId = strcat('/subscriptions/', subscriptionId, '/resourceGroups/', resourceGroup)
    | where resourceGroupId in~({ResourceGroups}) or '*' in~({ResourceGroups})
    | order by name asc
    | extend Rank = row_number()
    | project value = id, label = name, selected = Rank <= 10, group = resourceGroup
    ```
    El código para la selección de recursos está en la última línea de la consulta: `selected = Rank <= 10`. 

6. Cambie el valor de 10 a otro diferente para cambiar la selección predeterminada.