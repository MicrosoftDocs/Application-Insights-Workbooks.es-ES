# <a name="picking-a-set-of-resources-to-analyze-in-workbooks"></a><span data-ttu-id="f9a8f-101">Selección de un conjunto de recursos para analizar en libros</span><span class="sxs-lookup"><span data-stu-id="f9a8f-101">Picking a set of resources to analyze in workbooks</span></span>

<span data-ttu-id="f9a8f-102">La plantilla `Resource Picker` le permite empezar a usar la suscripción, el grupo de recursos y los parámetros de recursos para configurar el contexto de entrada del libro.</span><span class="sxs-lookup"><span data-stu-id="f9a8f-102">The `Resource Picker` template gets you started with subscription, resource group and resource parameters to set up the input context of your workbook.</span></span> <span data-ttu-id="f9a8f-103">Los parámetros predeterminados se establecen para seleccionar máquinas virtuales, pero puede configurarlos para seleccionar cualquier tipo de recurso.</span><span class="sxs-lookup"><span data-stu-id="f9a8f-103">The default parameters are set to pick virtual machines, but you can configure it to pick any type of resource.</span></span> <span data-ttu-id="f9a8f-104">La plantilla también tiene un control de consulta ARG que muestra cómo usar los parámetros en los análisis.</span><span class="sxs-lookup"><span data-stu-id="f9a8f-104">The template also has a ARG query control that shows you how to use the parameters in your analyses.</span></span>

![Imagen](Full.png)

## <a name="setting-up-the-resource-type-to-pick"></a><span data-ttu-id="f9a8f-106">Configuración del tipo de recurso que desea seleccionar</span><span class="sxs-lookup"><span data-stu-id="f9a8f-106">Setting up the resource type to pick</span></span>

1. <span data-ttu-id="f9a8f-107">Use `Edit` en la barra de herramientas del libro.</span><span class="sxs-lookup"><span data-stu-id="f9a8f-107">Use `Edit` on the workbook toolbar.</span></span>
2. <span data-ttu-id="f9a8f-108">Ahora podrá ver una lista desplegable `Resource type` antes de `Subscriptions`:</span><span class="sxs-lookup"><span data-stu-id="f9a8f-108">You will now be able to see a drop down `Resource type` before `Subscriptions`:</span></span>

    ![Imagen](Parameter.png)
3. <span data-ttu-id="f9a8f-110">Expanda la lista desplegable y seleccione los tipos de recursos que desea seleccionar.</span><span class="sxs-lookup"><span data-stu-id="f9a8f-110">Expand the drop down and select the resource types you want picked.</span></span> <span data-ttu-id="f9a8f-111">Esto actualizará la suscripción, el grupo de recursos y la lista desplegable de recursos para que coincidan con la selección.</span><span class="sxs-lookup"><span data-stu-id="f9a8f-111">This will update the subscription, resource group and resources drop down to match your selection.</span></span>
4. <span data-ttu-id="f9a8f-112">Haga clic en el botón `Edit` situado en la parte inferior derecha del control de parámetros.</span><span class="sxs-lookup"><span data-stu-id="f9a8f-112">Click the `Edit` button at the bottom right of the parameter control.</span></span>
5. <span data-ttu-id="f9a8f-113">En la cuadrícula de parámetros, para la fila `Resources`, cambie la columna `Display name` de _Máquinas virtuales_ al nombre descriptivo del tipo de recurso seleccionado (por ejemplo, _Cuentas de almacenamiento_)</span><span class="sxs-lookup"><span data-stu-id="f9a8f-113">In the parameters grid, for the row `Resources`, change the `Display name` column from _Virtual machines_ to friendly name of your selected resource type (e.g. _Storage Accounts_)</span></span>
6. <span data-ttu-id="f9a8f-114">Haga clic en `Done Editing` en la barra de herramientas del libro.</span><span class="sxs-lookup"><span data-stu-id="f9a8f-114">Click `Done Editing` in the workbooks toolbar.</span></span>

## <a name="selecting-more-or-less-than-10-resources-by-default"></a><span data-ttu-id="f9a8f-115">Selección de aproximadamente 10 recursos de forma predeterminada</span><span class="sxs-lookup"><span data-stu-id="f9a8f-115">Selecting more or less than 10 resources by default</span></span>

1. <span data-ttu-id="f9a8f-116">Use `Edit` en la barra de herramientas del libro.</span><span class="sxs-lookup"><span data-stu-id="f9a8f-116">Use `Edit` on the workbook toolbar.</span></span>
2. <span data-ttu-id="f9a8f-117">Haga clic en el botón `Edit` situado en la parte inferior derecha del control de parámetros.</span><span class="sxs-lookup"><span data-stu-id="f9a8f-117">Click the `Edit` button at the bottom right of the parameter control.</span></span>
3. <span data-ttu-id="f9a8f-118">En la cuadrícula de parámetros, seleccione la fila de `Resources`</span><span class="sxs-lookup"><span data-stu-id="f9a8f-118">In the parameters grid, select the row for `Resources`</span></span>
4. <span data-ttu-id="f9a8f-119">Haga clic en el icono de `Edit` (icono de lápiz) en la barra de herramientas del control.</span><span class="sxs-lookup"><span data-stu-id="f9a8f-119">Click on the `Edit` (or pencil) icon control toolbar.</span></span>
5. <span data-ttu-id="f9a8f-120">En el panel `Edit Parameter` que aparece, desplácese hacia abajo hasta el editor de `Azure Resource Graph Query`.</span><span class="sxs-lookup"><span data-stu-id="f9a8f-120">In the `Edit Parameter` pane that pops up, scroll down to the `Azure Resource Graph Query` editor.</span></span> <span data-ttu-id="f9a8f-121">Debe tener una consulta similar a la siguiente:</span><span class="sxs-lookup"><span data-stu-id="f9a8f-121">It should have a query that looks like this:</span></span>
    ```sql
    Resources
    | where type in~({ResourceTypes})
    | extend resourceGroupId = strcat('/subscriptions/', subscriptionId, '/resourceGroups/', resourceGroup)
    | where resourceGroupId in~({ResourceGroups}) or '*' in~({ResourceGroups})
    | order by name asc
    | extend Rank = row_number()
    | project value = id, label = name, selected = Rank <= 10, group = resourceGroup
    ```
    <span data-ttu-id="f9a8f-122">El código para la selección de recursos está en la última línea de la consulta: `selected = Rank <= 10`.</span><span class="sxs-lookup"><span data-stu-id="f9a8f-122">The code for resource selection is in the last line of the query: `selected = Rank <= 10`.</span></span> 

6. <span data-ttu-id="f9a8f-123">Cambie el valor de 10 a otro diferente para cambiar la selección predeterminada.</span><span class="sxs-lookup"><span data-stu-id="f9a8f-123">Change the value from 10 to a different one to change the default selection</span></span>