---
ms.openlocfilehash: bf83723de025f3e1f6bde5b4226fc8966dff6b9a
ms.sourcegitcommit: 64947f11d367ffbebbafb700fdfdc20617275f35
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2020
ms.locfileid: "48829856"
---
<!-- markdownlint-disable MD002 MD041 -->

El último paso de configuración para asegurarse de que el conector está listo para su uso es autorizar y probar el conector personalizado para crear una conexión en caché.

> [!IMPORTANT]
> Los pasos siguientes requieren que haya iniciado sesión con privilegios de administrador.

En [Microsoft Power Automate](https://flow.microsoft.com), vaya al elemento de menú **datos** de la izquierda y seleccione la página **conexiones** . Elija el vínculo **nueva conexión** .

![Captura de pantalla del botón nueva conexión](./images/new-connection.png)

Busque el conector personalizado y complete la conexión haciendo clic en el botón más. Inicie sesión con su cuenta de Azure Active Directory del administrador de inquilinos de Office 365.

![Captura de pantalla de la lista de conexiones](./images/connection-sign-in.png)

Cuando se le pidan los permisos solicitados, compruebe el **consentimiento en nombre de su organización** y, después, elija **Aceptar** para autorizar permisos.

![Captura de pantalla de la solicitud de consentimiento](./images/consent-prompt.png)

Después de autorizar los permisos, se crea una conexión con Power Automated.

El conector personalizado ahora está configurado y habilitado. Es posible que se produzca un retraso en los permisos aplicados y disponibles, pero el conector ya está configurado.
