---
title: Consulta diferencial de la API de Azure AD Graph
author: JimacoMS
ms.TocTitle: Differential query
ms.ContentId: 893b0d6c-0c03-4108-94f5-30566d3e0df9
ms.topic: article (how-tos)
ms.date: 01/26/2016
ms.openlocfilehash: 13309599f5662627b9aa97d8befa8f8742b8d9a8
ms.sourcegitcommit: 520ed2c91e6cf3d2a7ba2f466a9a234df4d8072f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/29/2017
---
# <a name="differential-query--graph-api-concepts"></a>Consulta diferencial | Conceptos de la API Graph
    
 _**Se aplica a:** API Graph | Azure Active Directory_


En este tema se describe la característica de consulta diferencial de la API Graph de Azure AD. Una solicitud de consulta diferencial devuelve todos los cambios realizados en entidades específicas durante el tiempo entre dos solicitudes consecutivas. Por ejemplo, si realizas una consulta diferencial una hora después de la solicitud de consulta diferencial anterior, solo se devolverán los cambios realizados durante esa hora. Esta funcionalidad es especialmente útil al sincronizar datos del directorio de inquilino con el almacén de datos de una aplicación.

Para realizar una solicitud de consulta diferencial en un directorio de inquilino, el inquilino debe autorizar a la aplicación. Para más información, consulte [Integración de aplicaciones con Azure Active Directory](https://azure.microsoft.com/en-us/documentation/articles/active-directory-integrating-applications/).

> [!IMPORTANT]
> Se recomienda encarecidamente que utilice [Microsoft Graph](https://graph.microsoft.io/) en lugar de la API de Azure AD Graph para tener acceso a recursos de Azure Active Directory. Nuestros esfuerzos de desarrollo se concentran ahora en Microsoft Graph y no están previstas mejoras adicionales para la API de Azure AD Graph. Hay un número muy limitado de escenarios para los que la API de Azure AD Graph todavía podría ser adecuada; para más información, vea la entrada del blog [Microsoft Graph o Azure AD Graph](https://dev.office.com/blogs/microsoft-graph-or-azure-ad-graph) en el centro de desarrollo de Office.

## Solicitudes de consulta diferencial <a id="DifferentialQueryRequests"></a>

Esta sección describe las solicitudes de consulta diferencial. Todas las solicitudes necesitan los siguientes componentes:

- Una URL de solicitud válida, incluido el extremo de Graph para el inquilino y los parámetros de cadena de consulta correspondientes.

- Un encabezado de autorización, incluido un token de acceso válido emitido por Azure Active Directory. Para más información sobre la autenticación con la API Graph, consulte [Escenarios de autenticación para Azure AD](https://azure.microsoft.com/en-us/documentation/articles/active-directory-authentication-scenarios/).

### <a name="differential-query-request-url"></a>Dirección URL de solicitud de consulta diferencial

A continuación se muestra el formato de la URL de solicitud de consulta diferencial; los corchetes [] indican elementos opcionales.

```no-highlight
https://graph.windows.net/<tenantId>/<resourceSet>?api-version=<SupportedApiVersion>&deltaLink=<token>&[$filter=isof(<entityType>)]&[$select=<PropertyList>]
```

La URL se compone de segmentos jerárquicos seguidos de una serie de parámetros de cadena de consulta expresados como pares de clave-valor.

### <a name="url-hierarchical-segments"></a>Dirección URL: Segmentos jerárquicos

|Segmento|Descripción|
|---|---|
|tenantId|El identificador único del inquilino con respecto al cual se ejecuta la consulta. Normalmente es uno de los dominios comprobados (propiedad **verifiedDomains**) del inquilino, pero también puede ser el identificador de objeto del inquilino (propiedad **objectId**). No distingue mayúsculas de minúsculas.|
|resourceSet|El conjunto específico de recursos de inquilino con respecto al cual se debe ejecutar la consulta. Determina qué recursos se devuelven en la respuesta de la consulta. Los valores admitidos son: “directoryObjects”, “users”, “contacts” o “groups”. Los valores distinguen mayúsculas de minúsculas.|

### <a name="url-query-string-parameters"></a>Dirección URL: Parámetros de cadena de consulta

|Parámetro|Descripción|
|---|---|
|api-version|Especifica la versión de la API Graph con la que se emite la solicitud. Obligatorio. A partir de la versión 1.5, el valor se expresa como un número de versión numérico; por ejemplo, api-version = 1.5. Para las versiones anteriores, el valor es una cadena con la forma AAAA-MM-DD; por ejemplo, api-version = 2013-04-05.<br/> <br/>(Reemplaza el uso del encabezado **x-ms-dirapi-data-contract-version** en la versión preliminar de la API Graph).|
|deltaLink|El token devuelto en la propiedad **deltaLink** o en la propiedad **nextLink** en la última respuesta. Obligatorio, pero estará vacío en la primera solicitud. <br/> <br/>(Reemplaza el parámetro de cadena de consulta *skipToken* en la versión preliminar de la API Graph).|
|$filter|Indica qué tipos de entidad deben estar incluidos en la respuesta. Opcional. Los tipos de entidad admitidos son: User, Group y Contact. Solo es válido cuando &ltresourceSet&gt es “directoryObjects”; de lo contrario, &ltresourceSet&gt invalida el filtro. Por ejemplo, si &ltresourceSet&gt es “users” y el parámetro *$filter* también está especificado, solo se devolverán los cambios de usuarios independientemente de lo que se especifique en el valor del filtro. Si &ltresourceSet&gt es “directoryObjects” y *$filter* no está especificado, se devuelven los cambios de todos los tipos de entidad admitidos (User, Group y Contact).<br/><br/>Para especificar un solo tipo de entidad, usa una de las siguientes opciones:<ul><li>`$filter=isof('Microsoft.WindowsAzure.ActiveDirectory.Contact')`</li><li>`$filter=isof('Microsoft.WindowsAzure.ActiveDirectory.User')`</li><li>`$filter=isof('Microsoft.WindowsAzure.ActiveDirectory.Group')`</li></ul>Para especificar varios tipos de entidad, use el operador **o**; por ejemplo, `$filter=isof('Microsoft.WindowsAzure.ActiveDirectory.User')%20or%20isof('Microsoft.WindowsAzure.ActiveDirectory.Group')`. <br/> <br/>(Reemplaza el parámetro de cadena de consulta *objectScope* en la versión preliminar de la API Graph).<br/> <br/>**Importante** A partir de la versión 1.5, el espacio de nombres de la API Graph cambia de Microsoft.WindowsAzure.ActiveDirectory a Microsoft.DirectoryServices. Las versiones anteriores de la API Graph seguirán utilizando el espacio de nombres anterior; por ejemplo, `$filter=isof('Microsoft.WindowsAzure.ActiveDirectory.Contact')`.|
|$select|Especifica qué propiedades se deben incluir en la respuesta. Si no está especificado $*$select^, se incluyen todas las propiedades.<br/><br/>Las propiedades se pueden especificar como completamente cualificadas o no cualificadas. Varias propiedades se delimitan por comas.<ul><li>Las propiedades completas tienen el tipo de entidad especificado; por ejemplo, `User/displayName`. Las propiedades completas DEBEN usarse si &ltresourceSet&gt se especifica como "directoryObjects"; por ejemplo, `https://graph.windows.net/contoso.com/directoryObjects?api-version=2013-04-05&deltaLink=&$select=User/displayName,Group/description`.</li><li>Las propiedades no cualificadas no tienen el tipo de entidad especificado; por ejemplo, , `displayName`. Solo se pueden utilizar si &ltresourceSet&gt se especifica como un valor distinto de "directoryObjects"; por ejemplo, `https://graph.windows.net/contoso.com/users?api-version=2013-04-05&deltaLink=&$select=displayName,jobTitle`.</li></ul>

**Nota**: Los pares de clave-valor en la cadena de consulta distinguen mayúsculas de minúsculas pero su orden no es significativo.

A continuación se muestra un ejemplo de la consulta diferencial más sencilla. Esta se usa durante la sincronización inicial.

```no-highlight
https://graph.windows.net/contoso.com/directoryObjects?api-version=2013-04-05&deltaLink=
```

A continuación se muestra un ejemplo de una solicitud posterior.

```no-highlight
https://graph.windows.net/contoso.com/directoryObjects?api-version=2013-04-05&deltaLink=AAABAGCL8z4m%2bc9IJGIzYjFmYzU5LTg0YjgtNDQwMC1hNzE1LWVhOGE3ZTQwZjRmZQBuvX43ACZQT4LRVPug8An6AAABAANIABAfGgAQwAMAJDCHA5AAABAATCkAA44TADQnhAQAIAAAgAHAAwAQAAAA8rLSTyfq5U`
```

## Respuestas de consulta diferencial <a id="DifferentialQueryResponses"></a>

Esta sección describe el contenido de una respuesta de consulta diferencial que se devuelve cuando se realiza una solicitud de consulta diferencial. En la lista siguiente se describe el contenido de una respuesta:

- De cero a 200 entidades [DirectoryObject], cada una de las cuales contiene cambios de un objeto [User], [Group] o [Contact] específico.

- De cero a 3000 entidades [DirectoryLinkChange], cada una de las cuales contiene cambios de un vínculo **Member** o **Manager** específico.

- Una propiedad **deltaLink** o **nextLink**. En cualquier caso, su valor es una cadena con codificación URL que distingue mayúsculas de minúsculas y que incrusta información de estado sobre el conjunto de cambios del directorio que se han devuelto al cliente, en relación con los cambios restantes que se han producido en el directorio. Esta cadena (o token) se debe incluir en el parámetro de cadena de consulta **deltaLink** en la siguiente solicitud de consulta diferencial.

  - Si se devuelve la propiedad **deltaLink**, no quedan más cambios en el directorio para que los sincronice la aplicación después de esta respuesta. La aplicación puede esperar un tiempo predeterminado según sus propios requisitos para emitir la siguiente solicitud de consulta diferencial.

  - Si se devuelve la propiedad **nextLink**, hay cambios en el directorio pendientes para que la aplicación los sincronice después de esta respuesta. La aplicación debe emitir la siguiente solicitud de consulta diferencial con la mayor brevedad posible.

Las respuestas siempre se devuelven en formato JSON.

## Consideraciones al usar una consulta diferencial <a id="ConsiderationsWhenUsingDq"></a>

La siguiente lista resalta consideraciones importantes para aplicaciones que usan la consulta diferencial:

- Los cambios devueltos por la consulta diferencial representan el estado de los objetos del directorio en el momento de la respuesta. La aplicación no debe tratar estos cambios como registros de transacción para la reproducción.

- Los cambios aparecen en el orden en el que se produjeron. Los objetos modificados más recientes aparecen los últimos incluso si el objeto se actualizó varias veces. Su orden tampoco se ve afectado por el momento en que el cliente recibió los cambios. En consecuencia, es posible que los cambios se presenten desordenados en relación con la manera en que ocurrieron inicialmente en el directorio.

- Su aplicación debe estar preparada para las reproducciones, que ocurren cuando aparece el mismo cambio en respuestas posteriores. Si bien una consulta diferencial hace lo posible por reducir las reproducciones, estas todavía pueden tener lugar.

- Su aplicación debe estar preparada para tratar un cambio de eliminación de un objeto del que no tenía constancia.

- La consulta diferencial puede devolver un vínculo a un objeto de origen o de destino que aún no ha sido devuelto por otras respuestas.

- Consulte la sección [Características adicionales de consulta diferencial](#AdditionalDifferentialQueryFeatures), a continuación, para obtener más información sobre el uso de encabezados de solicitud para restringir las consultas y mejorar el rendimiento.

## Ejemplos de solicitud y respuesta <a id="ReqeustAndResponseExamples"></a>

A continuación se muestra un ejemplo de una solicitud de consulta diferencial:

```no-highlight
GET https://graph.windows.net/contoso.com/directoryObjects?api-version=2013-04-05&$filter=isof('Microsoft.WindowsAzure.ActiveDirectory.User')%20or%20isof('Microsoft.WindowsAzure.ActiveDirectory.Group')%20or%20isof('Microsoft.WindowsAzure.ActiveDirectory.Contact')&deltaLink= HTTP /1.1
Authorization: Bearer eyJ0eXAiOiJKV . . . KUAe1EQ
Host: graph.windows.net
```
A continuación se muestra un ejemplo de una solicitud de consulta diferencial incremental:

```no-highlight
https://graph.windows.net/contoso.com/directoryObjects?api-version=2013-04-05&$filter=isof('Microsoft.WindowsAzure.ActiveDirectory.User')%20or%20isof('Microsoft.WindowsAzure.ActiveDirectory.Group')%20or%20isof('Microsoft.WindowsAzure.ActiveDirectory.Contact')&deltaLink=AAABAGCL8z4m%2bc9IJGIzYjFmYzU5LTg0YjgtNDQwMC1hNzE1LWVhOGE3ZTQwZjRmZQBuvX43ACZQT4LRVPug8An6AAABAANIABAfGgAQwAMAJDCHA5AAABAATCkAA44TADQnhAQAIAAAgAHAAwAQAAAA8rLSTyfq5U
 HTTP /1.1
Authorization: Bearer eyJ0eXAiOiJKV . . . KUAe1EQ
Host: graph.windows.net
```

**Nota**: En ambas solicitudes de ejemplo, el parámetro de consulta *$filter* se muestra únicamente a efectos de demostración. Puesto que el filtro especifica todos los tipos de destino posibles para la consulta diferencial (User, Group y Contact), este se podría omitir y la consulta devolvería cambios de todos esos tipos de entidad de manera predeterminada.

La siguiente respuesta de ejemplo muestra el JSON devuelto:

```no-highlight
{
  "odata.metadata": "https://graph.windows.net/contoso.com/$metadata#directoryObjects",


  # This is the deltaLink to be used for the next query
  "aad.deltaLink": "https://graph.windows.net/contoso.com/directoryObjects?deltaLink=XARBN7ivjcS6QIhSZDQR3OkT15SO1eeY-01BZSS0sOct6sh5oEyqOLLKRVhRmBMLHhePUF... [Truncated]",
  "value": [

    # User object for John Smith
    {
      "odata.type": "Microsoft.WindowsAzure.ActiveDirectory.User",
      "objectType": "User",
      "objectId": "dca803ab-bf26-4753-bf20-e1c56a9c34e2",
      "accountEnabled": true,
      "displayName": "John Smith",
      "givenName": "John",
      "mailNickname": "johnsmith",
      "passwordPolicies": "None",
      "surname": "Smith",
      "usageLocation": "US",
      "userPrincipalName": "johnsmith@contoso.com"
    },

    # Group object for IT Administrators
    {
      "odata.type": "Microsoft.WindowsAzure.ActiveDirectory.Group",
      "objectType": "Group",
      "objectId": "7373b0af-d462-406e-ad26-f2bc96d823d8",
      "description": "IT Administrators",
      "displayName": "Administrators",
      "mailNickname": "Administrators",
      "mailEnabled": false,
      "securityEnabled": true
    },

    # Contact object for Jane Smith
    {
      "odata.type": "Microsoft.WindowsAzure.ActiveDirectory.Contact",
      "objectType": "Contact",
      "objectId": "d711a1f8-21cf-4dc0-834a-5583e5324c44",
      "displayName": "Jane Smith",
      "givenName": "Jane",
      "mail": "johnsmith@contoso.com",
      "mailNickname": "johnsmith",
      "proxyAddresses": [
        "SMTP:janesmith@fabrikam.com"
      ],
      "surname": "Smith"
    },

    # Member link indicating John Smith is a member of IT Admin Group
    {
      "odata.type": "Microsoft.WindowsAzure.ActiveDirectory.DirectoryLinkChange",
      "objectType": "DirectoryLinkChange",
      "objectId": "00000000-0000-0000-0000-000000000000",
      "associationType": "Member",
      "sourceObjectId": "7373b0af-d462-406e-ad26-f2bc96d823d8",
      "sourceObjectType": "Group",
      "sourceObjectUri": "https://graph.windows.net/contoso.com/groups/7373b0af-d462-406e-ad26-f2bc96d823d8",
      "targetObjectId": "dca803ab-bf26-4753-bf20-e1c56a9c34e2",
      "targetObjectType": "User",
      "targetObjectUri": "https://graph.windows.net/contoso.com/users/dca803ab-bf26-4753-bf20-e1c56a9c34e2"
    }
  ]
}
```
## Características adicionales de consulta diferencial <a id="AdditionalDifferentialQueryFeatures"></a>

Ahora, las consultas diferenciales pueden devolver solamente propiedades y vínculos actualizados: esto permite realizar un procesamiento más rápido y reducir las cargas de las llamadas de consulta diferencial. Esta opción se habilita estableciendo el encabezado **ocp-aad-dq-include-only-changed-properties** como verdadero, como se muestra en este ejemplo.

```no-highlight
GET https://graph.windows.net/contoso.com/users?api-version=2013-11-08&deltaLink= furK18V1T….
HTTP /1.1
ocp-aad-dq-include-only-changed-properties : true
Content-Type: application/json
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5nl0aEV1T….
Response: 200 OK
```

Por ejemplo, si solo ha cambiado la propiedad “displayName” del usuario.  El objeto devuelto sería similar al siguiente:

```no-highlight
{     
          "displayName" : "AnUpdatedDisplayName",
         "objectId" :  "c1bf5c59-7974-4aff-a59a-f8dfe9809b18",
         "objectType" :  "User",
          "odata.type" :  "Microsoft.WindowsAzure.ActiveDirectory.User"
},

```

**Compatibilidad con la sincronización diferencial para sincronizar desde "ahora"**: puede especificarse un encabezado especial que solicite solamente la obtención de un deltaToken actualizado. Este token puede usarse en las consultas posteriores, lo que devolverá solo los cambios desde "ahora".  Esta es la llamada de ejemplo:

```no-highlight
GET https://graph.windows.net/contoso.com/users?api-version=2013-11-08&deltaLink= smLYT8V1T…
HTTP /1.1
ocp-aad-dq-include-only-delta-token: true
Content-Type: application/json
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5nl0aEV1T….
Response: 200 OK
```

La respuesta contendrá la propiedad **deltaLink**, pero no tendrá los objetos modificados, similares al siguiente:

```no-highlight
{   …  "aad.deltaLink":https://graph.windows.net/contoso.com/users?deltaLink=MRa43......   }
```

**Detección de un elemento eliminado**: La respuesta también puede utilizarse para detectar un elemento eliminado.  Los vínculos eliminados y los objetos eliminados se indican con la propiedad “aad.isDeleted” con un valor establecido como verdadero. Esto es necesario para que las aplicaciones puedan aprender de la eliminación de los vínculos y los objetos creados anteriormente.

##Recursos adicionales <a id="AdditionalResources"></a>

- [Referencia de la API de Azure AD Graph](../api/api-catalog.md)

[Application]: ../api/entity-and-complex-type-reference.md#application-entity
[AppRoleAssignment]: ../api/entity-and-complex-type-reference.md#approleassignment-entity
[Contact]: ../api/entity-and-complex-type-reference.md#contact-entity
[Contract]: ../api/entity-and-complex-type-reference.md#contract-entity
[Device]: ../api/entity-and-complex-type-reference.md#device-entity
[DirectoryLinkChange]: ../api/entity-and-complex-type-reference.md#directorylinkchange-entity
[DirectoryObject]: ../api/entity-and-complex-type-reference.md#directoryobject-entity
[DirectoryRole]: ../api/entity-and-complex-type-reference.md#directoryrole-entity
[DirectoryRoleTemplate]: ../api/entity-and-complex-type-reference.md#directoryroletemplate-entity
[ExtensionProperty]: ../api/entity-and-complex-type-reference.md#extensionproperty-entity
[Group]: ../api/entity-and-complex-type-reference.md#group-entity
[OAuth2PermissionGrant]: ../api/entity-and-complex-type-reference.md#oauth2permissiongrant-entity
[ServicePrincipal]: ../api/entity-and-complex-type-reference.md#serviceprincipal-entity
[SubscribedSku]: ../api/entity-and-complex-type-reference.md#subscribedsku-entity
[TenantDetail]: ../api/entity-and-complex-type-reference.md#tenantdetail-entity
[User]: ../api/entity-and-complex-type-reference.md#user-entity

[AlternativeSecurityId]: ../api/entity-and-complex-type-reference.md#alternativesecurityid-type
[AppRole]: ../api/entity-and-complex-type-reference.md#approle-type
[AssignedLicense]: ../api/entity-and-complex-type-reference.md#assignedlicense-type
[AssignedPlan]: ../api/entity-and-complex-type-reference.md#assignedplan-type
[KeyCredential]: ../api/entity-and-complex-type-reference.md#keycredential-type
[LicenseUnitsDetail]: ../api/entity-and-complex-type-reference.md#licenseunitsdetail-type
[OAuth2Permission]: ../api/entity-and-complex-type-reference.md#oauth2permission-type
[PasswordCredential]: ./entity-and-complex-type-reference.md#passwordcredential-type
[PasswordProfile]: ../api/entity-and-complex-type-reference.md#passwordprofile-type
[ProvisionedPlan]: ../api/entity-and-complex-type-reference.md#provisionedplan-type
[ProvisioningError]: ../api/entity-and-complex-type-reference.md#provisioningerror-type
[RequiredResourceAccess]: ../api/entity-and-complex-type-reference.md#requiredresourceaccess-type
[ResourceAccess]: ../api/entity-and-complex-type-reference.md#resourceaccess-type
[ServicePlanInfo]: ../api/entity-and-complex-type-reference.md#serviceplaninfo-type
[ServicePrincipalAuthenticationPolicy]: ../api/entity-and-complex-type-reference.md#serviceprincipalauthenticationpolicy-type
[VerifiedDomain]: ../api/entity-and-complex-type-reference.md#verifieddomain-type

[assignLicense]: ../api/functions-and-actions.md#assignLicense
[checkMemberGroups]: ../api/functions-and-actions.md#checkMemberGroups
[getAvailableExtensionProperties]: ../api/functions-and-actions.md#getAvailableExtensionProperties
[getMemberGroups]: ../api/functions-and-actions.md#getMemberGroups
[getMemberObjects]: ../api/functions-and-actions.md#getMemberObjects
[getObjectsByObjectIds]: ../api/functions-and-actions.md#getObjectsByObjectIds
[isMemberOf]: ../api/functions-and-actions.md#isMemberOf
[restore]: ../api/functions-and-actions.md#restore
