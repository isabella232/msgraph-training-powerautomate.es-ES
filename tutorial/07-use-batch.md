---
ms.openlocfilehash: 2d9b6e29a5ddc71b8f7b78b1e2fe035a8fdbddac
ms.sourcegitcommit: 64947f11d367ffbebbafb700fdfdc20617275f35
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2020
ms.locfileid: "48829908"
---
<!-- markdownlint-disable MD002 MD041 -->

El flujo que ha creado en el ejercicio anterior usa la `$batch` API para realizar dos solicitudes individuales a Microsoft Graph. La llamada al `$batch` punto de conexión de esta forma proporciona alguna ventaja y flexibilidad, pero la verdadera potencia del `$batch` punto de conexión viene cuando ejecuta varias solicitudes a Microsoft Graph en una sola `$batch` llamada. En este ejercicio, ampliará el ejemplo de creación de un grupo unificado y asociando un equipo para incluir la creación de varios canales predeterminados para el equipo en una sola `$batch` solicitud.

Abra [Microsoft Power automatizada](https://flow.microsoft.com) en el explorador e inicie sesión con su cuenta de administrador de inquilinos de Office 365. Seleccione el flujo que creó en el paso anterior y elija **Editar**.

Elija **nuevo paso** y escriba `Batch` en el cuadro de búsqueda. Agregue la acción **conector por lotes de MS Graph** . Elija los puntos suspensivos y cambie el nombre de esta acción a `Batch POST-channels` .

Agregue el código siguiente en el cuadro de texto **Body** de la acción.

```json
{
  "requests": [
    {
      "id": 1,
      "url": "/teams/REPLACE/channels",
      "headers": {
        "Content-Type": "application/json"
      },
      "method": "POST",
      "body": {
        "displayName": "Marketing Collateral",
        "description": "Marketing collateral and documentation."
      }
    },
    {
      "id": 2,
      "dependsOn": [
        "1"
      ],
      "url": "/teams/REPLACE/channels",
      "headers": {
        "Content-Type": "application/json"
      },
      "method": "POST",
      "body": {
        "displayName": "Vendor Contracts",
        "description": "Vendor documents, contracts, agreements and schedules."
      }
    },
    {
      "id": 3,
      "dependsOn": [
        "2"
      ],
      "url": "/teams/REPLACE/channels",
      "headers": {
        "Content-Type": "application/json"
      },
      "method": "POST",
      "body": {
        "displayName": "General Client Agreements",
        "description": "General Client documents and agreements."
      }
    }
  ]
}
```

Observe que las tres solicitudes anteriores usan la propiedad [DEPENDSON](https://docs.microsoft.com/graph/json-batching#sequencing-requests-with-the-dependson-property) para especificar un orden secuencial, y cada una de ellas ejecutará una solicitud post para crear un nuevo canal en el nuevo equipo.

Seleccione cada instancia del `REPLACE` marcador de posición y, a continuación, seleccione **expresión** en el panel de contenido dinámico. Agregue la fórmula siguiente a la **expresión**.

```js
body('Batch_PUT-team').responses[0].body.id
```

![Captura de pantalla de la expresión en el panel de contenido dinámico](./images/dynamic-expression.png)

Elija **Guardar** y, a continuación, elija **prueba** para ejecutar el flujo. Seleccione el botón de opción **voy a realizar la acción desencadenadora** y, a continuación, elija **Guardar & prueba**. Escriba un nombre de grupo único en el campo **nombre** sin espacios y elija **Ejecutar flujo** para ejecutar el flujo.

Una vez que se inicie el flujo, seleccione el botón **listo** para ver el registro de actividades. Una vez finalizado el flujo, el resultado final de la `Batch POST-channels` acción tiene una respuesta de estado http de 201 para cada canal creado.

![Captura de pantalla del registro de la actividad de flujo correcta](./images/batch-success.png)

Vaya a [Microsoft Teams](https://teams.microsoft.com) e inicie sesión con su cuenta de administrador de inquilinos de Office 365. Compruebe que el equipo que acaba de crear aparece e incluye los tres canales creados por la `$batch` solicitud.

![Captura de pantalla de la aplicación Teams con el nuevo equipo y los canales que se muestran](./images/team-channels.png)

Aunque la `Batch POST-channels` acción anterior se implementó en este tutorial como una acción independiente, las llamadas para crear los canales podrían haberse agregado como llamadas adicionales en la `Batch PUT-team` acción. Esto habría creado el equipo y todos los canales en una sola llamada por lotes. Pruebe el propio.

Por último, recuerde que las llamadas de [procesamiento por lotes JSON](https://docs.microsoft.com/graph/json-batching) devolverán un código de estado http para cada solicitud. En un proceso de producción, es posible que desee combinar el posprocesamiento de los resultados con una [`Apply to each`](https://docs.microsoft.com/power-automate/apply-to-each) acción y validar cada respuesta individual que tenga un código de estado de 201 o compensar otros códigos de estado recibidos.
