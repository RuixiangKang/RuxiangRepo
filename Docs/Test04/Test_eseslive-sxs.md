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
# <a name="differential-query--graph-api-concepts"></a><span data-ttu-id="2d2a9-102">Consulta diferencial | Conceptos de la API Graph</span><span class="sxs-lookup"><span data-stu-id="2d2a9-102">Differential query | Graph API concepts</span></span>
    
 <span data-ttu-id="2d2a9-103">_**Se aplica a:** API Graph | Azure Active Directory_</span><span class="sxs-lookup"><span data-stu-id="2d2a9-103">_**Applies to:** Graph API | Azure Active Directory_</span></span>


<span data-ttu-id="2d2a9-104">En este tema se describe la característica de consulta diferencial de la API Graph de Azure AD.</span><span class="sxs-lookup"><span data-stu-id="2d2a9-104">This topic discusses the differential query feature of Azure AD Graph API.</span></span> <span data-ttu-id="2d2a9-105">Una solicitud de consulta diferencial devuelve todos los cambios realizados en entidades específicas durante el tiempo entre dos solicitudes consecutivas.</span><span class="sxs-lookup"><span data-stu-id="2d2a9-105">A differential query request returns all changes made to specified entities during the time between two consecutive requests.</span></span> <span data-ttu-id="2d2a9-106">Por ejemplo, si realizas una consulta diferencial una hora después de la solicitud de consulta diferencial anterior, solo se devolverán los cambios realizados durante esa hora.</span><span class="sxs-lookup"><span data-stu-id="2d2a9-106">For example, if you make a differential query request an hour after the previous differential query request, only the changes made during that hour will be returned.</span></span> <span data-ttu-id="2d2a9-107">Esta funcionalidad es especialmente útil al sincronizar datos del directorio de inquilino con el almacén de datos de una aplicación.</span><span class="sxs-lookup"><span data-stu-id="2d2a9-107">This functionality is especially useful when synchronizing tenant directory data with an application’s data store.</span></span>

<span data-ttu-id="2d2a9-108">Para realizar una solicitud de consulta diferencial en un directorio de inquilino, el inquilino debe autorizar a la aplicación.</span><span class="sxs-lookup"><span data-stu-id="2d2a9-108">To make a differential query request to a tenant’s directory, your application must be authorized by the tenant.</span></span> <span data-ttu-id="2d2a9-109">Para más información, consulte [Integración de aplicaciones con Azure Active Directory](https://azure.microsoft.com/en-us/documentation/articles/active-directory-integrating-applications/).</span><span class="sxs-lookup"><span data-stu-id="2d2a9-109">For more information, see [Integrating Applications with Azure Active Directory](https://azure.microsoft.com/en-us/documentation/articles/active-directory-integrating-applications/).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="2d2a9-110">Se recomienda encarecidamente que utilice [Microsoft Graph](https://graph.microsoft.io/) en lugar de la API de Azure AD Graph para tener acceso a recursos de Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="2d2a9-110">We strongly recommend that you use [Microsoft Graph](https://graph.microsoft.io/) instead of Azure AD Graph API to access Azure Active Directory resources.</span></span> <span data-ttu-id="2d2a9-111">Nuestros esfuerzos de desarrollo se concentran ahora en Microsoft Graph y no están previstas mejoras adicionales para la API de Azure AD Graph.</span><span class="sxs-lookup"><span data-stu-id="2d2a9-111">Our development efforts are now concentrated on Microsoft Graph and no further enhancements are planned for Azure AD Graph API.</span></span> <span data-ttu-id="2d2a9-112">Hay un número muy limitado de escenarios para los que la API de Azure AD Graph todavía podría ser adecuada; para más información, vea la entrada del blog [Microsoft Graph o Azure AD Graph](https://dev.office.com/blogs/microsoft-graph-or-azure-ad-graph) en el centro de desarrollo de Office.</span><span class="sxs-lookup"><span data-stu-id="2d2a9-112">There are a very limited number of scenarios for which Azure AD Graph API might still be appropriate; for more information, see the [Microsoft Graph or the Azure AD Graph](https://dev.office.com/blogs/microsoft-graph-or-azure-ad-graph) blog post in the Office Dev Center.</span></span>

## <span data-ttu-id="2d2a9-113">Solicitudes de consulta diferencial <a id="DifferentialQueryRequests"></a></span><span class="sxs-lookup"><span data-stu-id="2d2a9-113">Differential query requests <a id="DifferentialQueryRequests"></a></span></span>

<span data-ttu-id="2d2a9-114">Esta sección describe las solicitudes de consulta diferencial.</span><span class="sxs-lookup"><span data-stu-id="2d2a9-114">This section describes differential query requests.</span></span> <span data-ttu-id="2d2a9-115">Todas las solicitudes necesitan los siguientes componentes:</span><span class="sxs-lookup"><span data-stu-id="2d2a9-115">All requests require the following components:</span></span>

- <span data-ttu-id="2d2a9-116">Una URL de solicitud válida, incluido el extremo de Graph para el inquilino y los parámetros de cadena de consulta correspondientes.</span><span class="sxs-lookup"><span data-stu-id="2d2a9-116">A valid request URL, including the Graph endpoint for the tenant and applicable query string parameters.</span></span>

- <span data-ttu-id="2d2a9-117">Un encabezado de autorización, incluido un token de acceso válido emitido por Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="2d2a9-117">An authorization header, including a valid access token issued by Azure Active Directory.</span></span> <span data-ttu-id="2d2a9-118">Para más información sobre la autenticación con la API Graph, consulte [Escenarios de autenticación para Azure AD](https://azure.microsoft.com/en-us/documentation/articles/active-directory-authentication-scenarios/).</span><span class="sxs-lookup"><span data-stu-id="2d2a9-118">For more information on authenticating to the Graph API, see [Authentication Scenarios for Azure AD](https://azure.microsoft.com/en-us/documentation/articles/active-directory-authentication-scenarios/).</span></span>

### <a name="differential-query-request-url"></a><span data-ttu-id="2d2a9-119">Dirección URL de solicitud de consulta diferencial</span><span class="sxs-lookup"><span data-stu-id="2d2a9-119">Differential query request URL</span></span>

<span data-ttu-id="2d2a9-120">A continuación se muestra el formato de la URL de solicitud de consulta diferencial; los corchetes [] indican elementos opcionales.</span><span class="sxs-lookup"><span data-stu-id="2d2a9-120">The following shows the format of the URL for differential query request; square brackets [] indicate optional elements.</span></span>

```no-highlight
https://graph.windows.net/<tenantId>/<resourceSet>?api-version=<SupportedApiVersion>&deltaLink=<token>&[$filter=isof(<entityType>)]&[$select=<PropertyList>]
```

<span data-ttu-id="2d2a9-121">La URL se compone de segmentos jerárquicos seguidos de una serie de parámetros de cadena de consulta expresados como pares de clave-valor.</span><span class="sxs-lookup"><span data-stu-id="2d2a9-121">The URL is made up of a hierarchical segments followed by a series of query string parameters expressed as key-value pairs.</span></span>

### <a name="url-hierarchical-segments"></a><span data-ttu-id="2d2a9-122">Dirección URL: Segmentos jerárquicos</span><span class="sxs-lookup"><span data-stu-id="2d2a9-122">URL: Hierarchical segments</span></span>

|<span data-ttu-id="2d2a9-123">Segmento</span><span class="sxs-lookup"><span data-stu-id="2d2a9-123">Segment</span></span>|<span data-ttu-id="2d2a9-124">Descripción</span><span class="sxs-lookup"><span data-stu-id="2d2a9-124">Description</span></span>|
|---|---|
|<span data-ttu-id="2d2a9-125">tenantId</span><span class="sxs-lookup"><span data-stu-id="2d2a9-125">tenantId</span></span>|<span data-ttu-id="2d2a9-126">El identificador único del inquilino con respecto al cual se ejecuta la consulta.</span><span class="sxs-lookup"><span data-stu-id="2d2a9-126">The unique identifier of the tenant that the query should be executed against.</span></span> <span data-ttu-id="2d2a9-127">Normalmente es uno de los dominios comprobados (propiedad **verifiedDomains**) del inquilino, pero también puede ser el identificador de objeto del inquilino (propiedad **objectId**).</span><span class="sxs-lookup"><span data-stu-id="2d2a9-127">This is typically one of the verified domains (**verifiedDomains** property) of the tenant, but it can also be the tenant’s object ID (**objectId** property).</span></span> <span data-ttu-id="2d2a9-128">No distingue mayúsculas de minúsculas.</span><span class="sxs-lookup"><span data-stu-id="2d2a9-128">Not case-sensitive.</span></span>|
|<span data-ttu-id="2d2a9-129">resourceSet</span><span class="sxs-lookup"><span data-stu-id="2d2a9-129">resourceSet</span></span>|<span data-ttu-id="2d2a9-130">El conjunto específico de recursos de inquilino con respecto al cual se debe ejecutar la consulta.</span><span class="sxs-lookup"><span data-stu-id="2d2a9-130">The specific set of tenant resources this query should be executed against.</span></span> <span data-ttu-id="2d2a9-131">Determina qué recursos se devuelven en la respuesta de la consulta.</span><span class="sxs-lookup"><span data-stu-id="2d2a9-131">Determines what resources are returned in the query response.</span></span> <span data-ttu-id="2d2a9-132">Los valores admitidos son: “directoryObjects”, “users”, “contacts” o “groups”.</span><span class="sxs-lookup"><span data-stu-id="2d2a9-132">Supported values are: “directoryObjects”, “users”, “contacts” or “groups”.</span></span> <span data-ttu-id="2d2a9-133">Los valores distinguen mayúsculas de minúsculas.</span><span class="sxs-lookup"><span data-stu-id="2d2a9-133">Values are case-sensitive.</span></span>|

### <a name="url-query-string-parameters"></a><span data-ttu-id="2d2a9-134">Dirección URL: Parámetros de cadena de consulta</span><span class="sxs-lookup"><span data-stu-id="2d2a9-134">URL: Query string parameters</span></span>

|<span data-ttu-id="2d2a9-135">Parámetro</span><span class="sxs-lookup"><span data-stu-id="2d2a9-135">Parameter</span></span>|<span data-ttu-id="2d2a9-136">Descripción</span><span class="sxs-lookup"><span data-stu-id="2d2a9-136">Description</span></span>|
|---|---|
|<span data-ttu-id="2d2a9-137">api-version</span><span class="sxs-lookup"><span data-stu-id="2d2a9-137">api-version</span></span>|<span data-ttu-id="2d2a9-138">Especifica la versión de la API Graph con la que se emite la solicitud.</span><span class="sxs-lookup"><span data-stu-id="2d2a9-138">Specifies the version of the Graph API against which the request is issued.</span></span> <span data-ttu-id="2d2a9-139">Obligatorio.</span><span class="sxs-lookup"><span data-stu-id="2d2a9-139">Required.</span></span> <span data-ttu-id="2d2a9-140">A partir de la versión 1.5, el valor se expresa como un número de versión numérico; por ejemplo, api-version = 1.5.</span><span class="sxs-lookup"><span data-stu-id="2d2a9-140">Beginning with version 1.5, the value is expressed as a numeric version number; for example, api-version=1.5.</span></span> <span data-ttu-id="2d2a9-141">Para las versiones anteriores, el valor es una cadena con la forma AAAA-MM-DD; por ejemplo, api-version = 2013-04-05.</span><span class="sxs-lookup"><span data-stu-id="2d2a9-141">For previous versions, the value is a string of the form YYYY-MM-DD; for example, api-version=2013-04-05.</span></span><br/> <br/><span data-ttu-id="2d2a9-142">(Reemplaza el uso del encabezado **x-ms-dirapi-data-contract-version** en la versión preliminar de la API Graph).</span><span class="sxs-lookup"><span data-stu-id="2d2a9-142">(Replaces the use of **x-ms-dirapi-data-contract-version** header in the preview version of Graph API.)</span></span>|
|<span data-ttu-id="2d2a9-143">deltaLink</span><span class="sxs-lookup"><span data-stu-id="2d2a9-143">deltaLink</span></span>|<span data-ttu-id="2d2a9-144">El token devuelto en la propiedad **deltaLink** o en la propiedad **nextLink** en la última respuesta.</span><span class="sxs-lookup"><span data-stu-id="2d2a9-144">The token returned in either the **deltaLink** property or the **nextLink** property in the last response.</span></span> <span data-ttu-id="2d2a9-145">Obligatorio, pero estará vacío en la primera solicitud.</span><span class="sxs-lookup"><span data-stu-id="2d2a9-145">Required, but will be empty on the first request.</span></span> <br/> <br/><span data-ttu-id="2d2a9-146">(Reemplaza el parámetro de cadena de consulta *skipToken* en la versión preliminar de la API Graph).</span><span class="sxs-lookup"><span data-stu-id="2d2a9-146">(Replaces the *skipToken* query string parameter in the preview version of Graph API.)</span></span>|
|<span data-ttu-id="2d2a9-147">$filter</span><span class="sxs-lookup"><span data-stu-id="2d2a9-147">$filter</span></span>|<span data-ttu-id="2d2a9-148">Indica qué tipos de entidad deben estar incluidos en la respuesta.</span><span class="sxs-lookup"><span data-stu-id="2d2a9-148">Indicates which entity types should be included in the response.</span></span> <span data-ttu-id="2d2a9-149">Opcional.</span><span class="sxs-lookup"><span data-stu-id="2d2a9-149">Optional.</span></span> <span data-ttu-id="2d2a9-150">Los tipos de entidad admitidos son: User, Group y Contact.</span><span class="sxs-lookup"><span data-stu-id="2d2a9-150">The supported entity types are: User, Group and Contact.</span></span> <span data-ttu-id="2d2a9-151">Solo es válido cuando &ltresourceSet&gt es “directoryObjects”; de lo contrario, &ltresourceSet&gt invalida el filtro.</span><span class="sxs-lookup"><span data-stu-id="2d2a9-151">Only valid when &ltresourceSet&gt is “directoryObjects”; otherwise, &ltresourceSet&gt overrides the filter.</span></span> <span data-ttu-id="2d2a9-152">Por ejemplo, si &ltresourceSet&gt es “users” y el parámetro *$filter* también está especificado, solo se devolverán los cambios de usuarios independientemente de lo que se especifique en el valor del filtro.</span><span class="sxs-lookup"><span data-stu-id="2d2a9-152">For example, if &ltresourceSet&gt is “users”, and the *$filter* parameter is also specified, only changes for users will be returned regardless of what is specified in the filter value.</span></span> <span data-ttu-id="2d2a9-153">Si &ltresourceSet&gt es “directoryObjects” y *$filter* no está especificado, se devuelven los cambios de todos los tipos de entidad admitidos (User, Group y Contact).</span><span class="sxs-lookup"><span data-stu-id="2d2a9-153">If &ltresourceSet&gt is “directoryObjects” and *$filter* is not specified, changes for all of the supported entity types (User, Group, and Contact) are returned.</span></span><br/><br/><span data-ttu-id="2d2a9-154">Para especificar un solo tipo de entidad, usa una de las siguientes opciones:</span><span class="sxs-lookup"><span data-stu-id="2d2a9-154">To specify a single entity type, use one of the following:</span></span><ul><li>`$filter=isof('Microsoft.WindowsAzure.ActiveDirectory.Contact')`</li><li>`$filter=isof('Microsoft.WindowsAzure.ActiveDirectory.User')`</li><li>`$filter=isof('Microsoft.WindowsAzure.ActiveDirectory.Group')`</li></ul><span data-ttu-id="2d2a9-155">Para especificar varios tipos de entidad, use el operador **o**; por ejemplo, `$filter=isof('Microsoft.WindowsAzure.ActiveDirectory.User')%20or%20isof('Microsoft.WindowsAzure.ActiveDirectory.Group')`.</span><span class="sxs-lookup"><span data-stu-id="2d2a9-155">To specify multiple entity types, use the **or** operator; for example, `$filter=isof('Microsoft.WindowsAzure.ActiveDirectory.User')%20or%20isof('Microsoft.WindowsAzure.ActiveDirectory.Group')`.</span></span> <br/> <br/><span data-ttu-id="2d2a9-156">(Reemplaza el parámetro de cadena de consulta *objectScope* en la versión preliminar de la API Graph).</span><span class="sxs-lookup"><span data-stu-id="2d2a9-156">(Replaces  the *objectScope* query string parameter in the preview version of Graph API.)</span></span><br/> <br/><span data-ttu-id="2d2a9-157">**Importante** A partir de la versión 1.5, el espacio de nombres de la API Graph cambia de Microsoft.WindowsAzure.ActiveDirectory a Microsoft.DirectoryServices.</span><span class="sxs-lookup"><span data-stu-id="2d2a9-157">**Important** Beginning with version 1.5, the Graph API namespace is changed from Microsoft.WindowsAzure.ActiveDirectory to Microsoft.DirectoryServices.</span></span> <span data-ttu-id="2d2a9-158">Las versiones anteriores de la API Graph seguirán utilizando el espacio de nombres anterior; por ejemplo, `$filter=isof('Microsoft.WindowsAzure.ActiveDirectory.Contact')`.</span><span class="sxs-lookup"><span data-stu-id="2d2a9-158">Earlier versions of the Graph API continue to use the previous namespace; for example, `$filter=isof('Microsoft.WindowsAzure.ActiveDirectory.Contact')`.</span></span>|
|<span data-ttu-id="2d2a9-159">$select</span><span class="sxs-lookup"><span data-stu-id="2d2a9-159">$select</span></span>|<span data-ttu-id="2d2a9-160">Especifica qué propiedades se deben incluir en la respuesta.</span><span class="sxs-lookup"><span data-stu-id="2d2a9-160">Specifies which properties should be included in the response.</span></span> <span data-ttu-id="2d2a9-161">Si no está especificado $*$select^, se incluyen todas las propiedades.</span><span class="sxs-lookup"><span data-stu-id="2d2a9-161">If *$select^ is not specified, all properties are included.</span></span><br/><br/><span data-ttu-id="2d2a9-162">Las propiedades se pueden especificar como completamente cualificadas o no cualificadas.</span><span class="sxs-lookup"><span data-stu-id="2d2a9-162">Properties can be specified either fully qualified or non-qualified.</span></span> <span data-ttu-id="2d2a9-163">Varias propiedades se delimitan por comas.</span><span class="sxs-lookup"><span data-stu-id="2d2a9-163">Multiple properties are delimited by commas.</span></span><ul><li><span data-ttu-id="2d2a9-164">Las propiedades completas tienen el tipo de entidad especificado; por ejemplo, `User/displayName`.</span><span class="sxs-lookup"><span data-stu-id="2d2a9-164">Fully qualified properties have the entity type specified; for example, `User/displayName`.</span></span> <span data-ttu-id="2d2a9-165">Las propiedades completas DEBEN usarse si &ltresourceSet&gt se especifica como "directoryObjects"; por ejemplo, `https://graph.windows.net/contoso.com/directoryObjects?api-version=2013-04-05&deltaLink=&$select=User/displayName,Group/description`.</span><span class="sxs-lookup"><span data-stu-id="2d2a9-165">Fully qualified properties MUST be used if &ltresourceSet&gt is specified as “directoryObjects”; for example, `https://graph.windows.net/contoso.com/directoryObjects?api-version=2013-04-05&deltaLink=&$select=User/displayName,Group/description`.</span></span></li><li><span data-ttu-id="2d2a9-166">Las propiedades no cualificadas no tienen el tipo de entidad especificado; por ejemplo, , `displayName`.</span><span class="sxs-lookup"><span data-stu-id="2d2a9-166">Non-qualified properties do not have the entity type specified; for example, `displayName`.</span></span> <span data-ttu-id="2d2a9-167">Solo se pueden utilizar si &ltresourceSet&gt se especifica como un valor distinto de "directoryObjects"; por ejemplo, `https://graph.windows.net/contoso.com/users?api-version=2013-04-05&deltaLink=&$select=displayName,jobTitle`.</span><span class="sxs-lookup"><span data-stu-id="2d2a9-167">They can only be used when &ltresourceSet&gt is specified as a value other than “directoryObjects”; for example, `https://graph.windows.net/contoso.com/users?api-version=2013-04-05&deltaLink=&$select=displayName,jobTitle`.</span></span></li></ul>

<span data-ttu-id="2d2a9-168">**Nota**: Los pares de clave-valor en la cadena de consulta distinguen mayúsculas de minúsculas pero su orden no es significativo.</span><span class="sxs-lookup"><span data-stu-id="2d2a9-168">**Note**: The key-value pairs in the query string are case-sensitive, but their order is not significant.</span></span>

<span data-ttu-id="2d2a9-169">A continuación se muestra un ejemplo de la consulta diferencial más sencilla.</span><span class="sxs-lookup"><span data-stu-id="2d2a9-169">The following is an example of the simplest differential query.</span></span> <span data-ttu-id="2d2a9-170">Esta se usa durante la sincronización inicial.</span><span class="sxs-lookup"><span data-stu-id="2d2a9-170">This is used during initial sync.</span></span>

```no-highlight
https://graph.windows.net/contoso.com/directoryObjects?api-version=2013-04-05&deltaLink=
```

<span data-ttu-id="2d2a9-171">A continuación se muestra un ejemplo de una solicitud posterior.</span><span class="sxs-lookup"><span data-stu-id="2d2a9-171">The following is an example of a subsequent request.</span></span>

```no-highlight
https://graph.windows.net/contoso.com/directoryObjects?api-version=2013-04-05&deltaLink=AAABAGCL8z4m%2bc9IJGIzYjFmYzU5LTg0YjgtNDQwMC1hNzE1LWVhOGE3ZTQwZjRmZQBuvX43ACZQT4LRVPug8An6AAABAANIABAfGgAQwAMAJDCHA5AAABAATCkAA44TADQnhAQAIAAAgAHAAwAQAAAA8rLSTyfq5U`
```

## <span data-ttu-id="2d2a9-172">Respuestas de consulta diferencial <a id="DifferentialQueryResponses"></a></span><span class="sxs-lookup"><span data-stu-id="2d2a9-172">Differential query responses <a id="DifferentialQueryResponses"></a></span></span>

<span data-ttu-id="2d2a9-173">Esta sección describe el contenido de una respuesta de consulta diferencial que se devuelve cuando se realiza una solicitud de consulta diferencial.</span><span class="sxs-lookup"><span data-stu-id="2d2a9-173">This section describes the contents of a differential query response that is returned when a differential query request is made.</span></span> <span data-ttu-id="2d2a9-174">En la lista siguiente se describe el contenido de una respuesta:</span><span class="sxs-lookup"><span data-stu-id="2d2a9-174">The following list describes the contents of a response:</span></span>

- <span data-ttu-id="2d2a9-175">De cero a 200 entidades [DirectoryObject], cada una de las cuales contiene cambios de un objeto [User], [Group] o [Contact] específico.</span><span class="sxs-lookup"><span data-stu-id="2d2a9-175">Zero to 200 [DirectoryObject] entities, each of which contains changes for a specific [User], [Group], or [Contact] object.</span></span>

- <span data-ttu-id="2d2a9-176">De cero a 3000 entidades [DirectoryLinkChange], cada una de las cuales contiene cambios de un vínculo **Member** o **Manager** específico.</span><span class="sxs-lookup"><span data-stu-id="2d2a9-176">Zero to 3000 [DirectoryLinkChange] entities, each of which contains changes for a specific **member** or **manager** link.</span></span>

- <span data-ttu-id="2d2a9-177">Una propiedad **deltaLink** o **nextLink**.</span><span class="sxs-lookup"><span data-stu-id="2d2a9-177">Either a **deltaLink** or a **nextLink** property.</span></span> <span data-ttu-id="2d2a9-178">En cualquier caso, su valor es una cadena con codificación URL que distingue mayúsculas de minúsculas y que incrusta información de estado sobre el conjunto de cambios del directorio que se han devuelto al cliente, en relación con los cambios restantes que se han producido en el directorio.</span><span class="sxs-lookup"><span data-stu-id="2d2a9-178">In either case, its value is a case-sensitive, URL-encoded string that embeds state information about the set of directory changes that have been returned to the client, with respect to remaining changes that have occurred in the directory.</span></span> <span data-ttu-id="2d2a9-179">Esta cadena (o token) se debe incluir en el parámetro de cadena de consulta **deltaLink** en la siguiente solicitud de consulta diferencial.</span><span class="sxs-lookup"><span data-stu-id="2d2a9-179">This string (or token) should be included in the **deltaLink** query string parameter in the next differential query request.</span></span>

  - <span data-ttu-id="2d2a9-180">Si se devuelve la propiedad **deltaLink**, no quedan más cambios en el directorio para que los sincronice la aplicación después de esta respuesta.</span><span class="sxs-lookup"><span data-stu-id="2d2a9-180">If the **deltaLink** property is returned, there are no more directory changes left for the application to synchronize after this response.</span></span> <span data-ttu-id="2d2a9-181">La aplicación puede esperar un tiempo predeterminado según sus propios requisitos para emitir la siguiente solicitud de consulta diferencial.</span><span class="sxs-lookup"><span data-stu-id="2d2a9-181">The application can wait for some predetermined time according to its own requirements to issue the next differential query request.</span></span>

  - <span data-ttu-id="2d2a9-182">Si se devuelve la propiedad **nextLink**, hay cambios en el directorio pendientes para que la aplicación los sincronice después de esta respuesta.</span><span class="sxs-lookup"><span data-stu-id="2d2a9-182">If the **nextLink** property is returned, there are directory changes remaining for the application to synchronize after this response.</span></span> <span data-ttu-id="2d2a9-183">La aplicación debe emitir la siguiente solicitud de consulta diferencial con la mayor brevedad posible.</span><span class="sxs-lookup"><span data-stu-id="2d2a9-183">The application should issue the next differential query request at its earliest convenience.</span></span>

<span data-ttu-id="2d2a9-184">Las respuestas siempre se devuelven en formato JSON.</span><span class="sxs-lookup"><span data-stu-id="2d2a9-184">Responses are always returned in JSON format.</span></span>

## <span data-ttu-id="2d2a9-185">Consideraciones al usar una consulta diferencial <a id="ConsiderationsWhenUsingDq"></a></span><span class="sxs-lookup"><span data-stu-id="2d2a9-185">Considerations when using differential query <a id="ConsiderationsWhenUsingDq"></a></span></span>

<span data-ttu-id="2d2a9-186">La siguiente lista resalta consideraciones importantes para aplicaciones que usan la consulta diferencial:</span><span class="sxs-lookup"><span data-stu-id="2d2a9-186">The following list highlights important considerations for applications that use differential query:</span></span>

- <span data-ttu-id="2d2a9-187">Los cambios devueltos por la consulta diferencial representan el estado de los objetos del directorio en el momento de la respuesta.</span><span class="sxs-lookup"><span data-stu-id="2d2a9-187">Changes returned by differential query represent the state of the directory objects at the time of the response.</span></span> <span data-ttu-id="2d2a9-188">La aplicación no debe tratar estos cambios como registros de transacción para la reproducción.</span><span class="sxs-lookup"><span data-stu-id="2d2a9-188">Your application must not treat these changes as transaction logs for replay.</span></span>

- <span data-ttu-id="2d2a9-189">Los cambios aparecen en el orden en el que se produjeron.</span><span class="sxs-lookup"><span data-stu-id="2d2a9-189">Changes appear in the order in which they occurred.</span></span> <span data-ttu-id="2d2a9-190">Los objetos modificados más recientes aparecen los últimos incluso si el objeto se actualizó varias veces.</span><span class="sxs-lookup"><span data-stu-id="2d2a9-190">The most-recently changed objects appear last even if the object was updated multiple times.</span></span> <span data-ttu-id="2d2a9-191">Su orden tampoco se ve afectado por el momento en que el cliente recibió los cambios.</span><span class="sxs-lookup"><span data-stu-id="2d2a9-191">Their order is also not affected by when the client received the changes.</span></span> <span data-ttu-id="2d2a9-192">En consecuencia, es posible que los cambios se presenten desordenados en relación con la manera en que ocurrieron inicialmente en el directorio.</span><span class="sxs-lookup"><span data-stu-id="2d2a9-192">As a result, it is possible for changes to be presented out of order compared to how they initially occurred in the directory.</span></span>

- <span data-ttu-id="2d2a9-193">Su aplicación debe estar preparada para las reproducciones, que ocurren cuando aparece el mismo cambio en respuestas posteriores.</span><span class="sxs-lookup"><span data-stu-id="2d2a9-193">Your application must be prepared for replays, which occur when the same change appears in subsequent responses.</span></span> <span data-ttu-id="2d2a9-194">Si bien una consulta diferencial hace lo posible por reducir las reproducciones, estas todavía pueden tener lugar.</span><span class="sxs-lookup"><span data-stu-id="2d2a9-194">While differential query makes a best effort to reduce replays, they are still possible.</span></span>

- <span data-ttu-id="2d2a9-195">Su aplicación debe estar preparada para tratar un cambio de eliminación de un objeto del que no tenía constancia.</span><span class="sxs-lookup"><span data-stu-id="2d2a9-195">Your application must be prepared to handle a deletion change for an object it was not aware of.</span></span>

- <span data-ttu-id="2d2a9-196">La consulta diferencial puede devolver un vínculo a un objeto de origen o de destino que aún no ha sido devuelto por otras respuestas.</span><span class="sxs-lookup"><span data-stu-id="2d2a9-196">Differential query can return a link to a source or target object that has not yet been returned by other responses.</span></span>

- <span data-ttu-id="2d2a9-197">Consulte la sección [Características adicionales de consulta diferencial](#AdditionalDifferentialQueryFeatures), a continuación, para obtener más información sobre el uso de encabezados de solicitud para restringir las consultas y mejorar el rendimiento.</span><span class="sxs-lookup"><span data-stu-id="2d2a9-197">See the [Additional differential query features](#AdditionalDifferentialQueryFeatures) section below for more information on using request headers to constrain your queries to improve performance.</span></span>

## <span data-ttu-id="2d2a9-198">Ejemplos de solicitud y respuesta <a id="ReqeustAndResponseExamples"></a></span><span class="sxs-lookup"><span data-stu-id="2d2a9-198">Request and response examples <a id="ReqeustAndResponseExamples"></a></span></span>

<span data-ttu-id="2d2a9-199">A continuación se muestra un ejemplo de una solicitud de consulta diferencial:</span><span class="sxs-lookup"><span data-stu-id="2d2a9-199">The following is an example of an initial differential query request:</span></span>

```no-highlight
GET https://graph.windows.net/contoso.com/directoryObjects?api-version=2013-04-05&$filter=isof('Microsoft.WindowsAzure.ActiveDirectory.User')%20or%20isof('Microsoft.WindowsAzure.ActiveDirectory.Group')%20or%20isof('Microsoft.WindowsAzure.ActiveDirectory.Contact')&deltaLink= HTTP /1.1
Authorization: Bearer eyJ0eXAiOiJKV . . . KUAe1EQ
Host: graph.windows.net
```
<span data-ttu-id="2d2a9-200">A continuación se muestra un ejemplo de una solicitud de consulta diferencial incremental:</span><span class="sxs-lookup"><span data-stu-id="2d2a9-200">The following is an example of an incremental differential query request:</span></span>

```no-highlight
https://graph.windows.net/contoso.com/directoryObjects?api-version=2013-04-05&$filter=isof('Microsoft.WindowsAzure.ActiveDirectory.User')%20or%20isof('Microsoft.WindowsAzure.ActiveDirectory.Group')%20or%20isof('Microsoft.WindowsAzure.ActiveDirectory.Contact')&deltaLink=AAABAGCL8z4m%2bc9IJGIzYjFmYzU5LTg0YjgtNDQwMC1hNzE1LWVhOGE3ZTQwZjRmZQBuvX43ACZQT4LRVPug8An6AAABAANIABAfGgAQwAMAJDCHA5AAABAATCkAA44TADQnhAQAIAAAgAHAAwAQAAAA8rLSTyfq5U
 HTTP /1.1
Authorization: Bearer eyJ0eXAiOiJKV . . . KUAe1EQ
Host: graph.windows.net
```

<span data-ttu-id="2d2a9-201">**Nota**: En ambas solicitudes de ejemplo, el parámetro de consulta *$filter* se muestra únicamente a efectos de demostración.</span><span class="sxs-lookup"><span data-stu-id="2d2a9-201">**Note**: In both of these sample requests, the *$filter* query parameter is shown for demonstration purposes only.</span></span> <span data-ttu-id="2d2a9-202">Puesto que el filtro especifica todos los tipos de destino posibles para la consulta diferencial (User, Group y Contact), este se podría omitir y la consulta devolvería cambios de todos esos tipos de entidad de manera predeterminada.</span><span class="sxs-lookup"><span data-stu-id="2d2a9-202">Because the filter specifies all of the possible target types for the differential query (User, Group, and Contact), it could be omitted and the query would return changes for all of these entity types by default.</span></span>

<span data-ttu-id="2d2a9-203">La siguiente respuesta de ejemplo muestra el JSON devuelto:</span><span class="sxs-lookup"><span data-stu-id="2d2a9-203">The following example response demonstrates the returned JSON:</span></span>

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
## <span data-ttu-id="2d2a9-204">Características adicionales de consulta diferencial <a id="AdditionalDifferentialQueryFeatures"></a></span><span class="sxs-lookup"><span data-stu-id="2d2a9-204">Additional differential query features <a id="AdditionalDifferentialQueryFeatures"></a></span></span>

<span data-ttu-id="2d2a9-205">Ahora, las consultas diferenciales pueden devolver solamente propiedades y vínculos actualizados: esto permite realizar un procesamiento más rápido y reducir las cargas de las llamadas de consulta diferencial.</span><span class="sxs-lookup"><span data-stu-id="2d2a9-205">Differential Queries can now return only updated properties and links – this allows faster processing and reduced payloads for Differential Query calls.</span></span> <span data-ttu-id="2d2a9-206">Esta opción se habilita estableciendo el encabezado **ocp-aad-dq-include-only-changed-properties** como verdadero, como se muestra en este ejemplo.</span><span class="sxs-lookup"><span data-stu-id="2d2a9-206">This option is enabled by setting the header **ocp-aad-dq-include-only-changed-properties** to true as shown in this example.</span></span>

```no-highlight
GET https://graph.windows.net/contoso.com/users?api-version=2013-11-08&deltaLink= furK18V1T….
HTTP /1.1
ocp-aad-dq-include-only-changed-properties : true
Content-Type: application/json
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5nl0aEV1T….
Response: 200 OK
```

<span data-ttu-id="2d2a9-207">Por ejemplo, si solo ha cambiado la propiedad “displayName” del usuario.</span><span class="sxs-lookup"><span data-stu-id="2d2a9-207">For example if only the “displayName” property of user has changed.</span></span>  <span data-ttu-id="2d2a9-208">El objeto devuelto sería similar al siguiente:</span><span class="sxs-lookup"><span data-stu-id="2d2a9-208">The returned object would be similar to this:</span></span>

```no-highlight
{     
          "displayName" : "AnUpdatedDisplayName",
         "objectId" :  "c1bf5c59-7974-4aff-a59a-f8dfe9809b18",
         "objectType" :  "User",
          "odata.type" :  "Microsoft.WindowsAzure.ActiveDirectory.User"
},

```

<span data-ttu-id="2d2a9-209">**Compatibilidad con la sincronización diferencial para sincronizar desde "ahora"**: puede especificarse un encabezado especial que solicite solamente la obtención de un deltaToken actualizado. Este token puede usarse en las consultas posteriores, lo que devolverá solo los cambios desde "ahora".</span><span class="sxs-lookup"><span data-stu-id="2d2a9-209">**Differential Sync support to sync from “now”** - a special header can be specified, requesting to only get an up-to-date deltaToken, this token can be used in subsequent queries, which will return only changes from “now”.</span></span>  <span data-ttu-id="2d2a9-210">Esta es la llamada de ejemplo:</span><span class="sxs-lookup"><span data-stu-id="2d2a9-210">Here’s the example call:</span></span>

```no-highlight
GET https://graph.windows.net/contoso.com/users?api-version=2013-11-08&deltaLink= smLYT8V1T…
HTTP /1.1
ocp-aad-dq-include-only-delta-token: true
Content-Type: application/json
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5nl0aEV1T….
Response: 200 OK
```

<span data-ttu-id="2d2a9-211">La respuesta contendrá la propiedad **deltaLink**, pero no tendrá los objetos modificados, similares al siguiente:</span><span class="sxs-lookup"><span data-stu-id="2d2a9-211">The response will contain the **deltaLink**, but will not have the changed object(s), similar to this:</span></span>

```no-highlight
{   …  "aad.deltaLink":https://graph.windows.net/contoso.com/users?deltaLink=MRa43......   }
```

<span data-ttu-id="2d2a9-212">**Detección de un elemento eliminado**: La respuesta también puede utilizarse para detectar un elemento eliminado.</span><span class="sxs-lookup"><span data-stu-id="2d2a9-212">**Detecting a deleted item** – the response can also be used to detect a deleted item.</span></span>  <span data-ttu-id="2d2a9-213">Los vínculos eliminados y los objetos eliminados se indican con la propiedad “aad.isDeleted” con un valor establecido como verdadero. Esto es necesario para que las aplicaciones puedan aprender de la eliminación de los vínculos y los objetos creados anteriormente.</span><span class="sxs-lookup"><span data-stu-id="2d2a9-213">Deleted objects and deleted links are indicated by the "aad.isDeleted" property with a value set to true; this is necessary to make sure applications can learn about the deletion of previously created objects and links.</span></span>

##<span data-ttu-id="2d2a9-214">Recursos adicionales <a id="AdditionalResources"></a></span><span class="sxs-lookup"><span data-stu-id="2d2a9-214">Additional resources <a id="AdditionalResources"></a></span></span>

- [<span data-ttu-id="2d2a9-215">Referencia de la API de Azure AD Graph</span><span class="sxs-lookup"><span data-stu-id="2d2a9-215">Azure AD Graph API reference</span></span>](../api/api-catalog.md)

[Application]: ../api/entity-and-complex-type-reference.md#application-entity
[AppRoleAssignment]: ../api/entity-and-complex-type-reference.md#approleassignment-entity
<span data-ttu-id="2d2a9-216">[Contact]: ../api/entity-and-complex-type-reference.md#contact-entity</span><span class="sxs-lookup"><span data-stu-id="2d2a9-216">[Contact]: ../api/entity-and-complex-type-reference.md#contact-entity</span></span>
[Contract]: ../api/entity-and-complex-type-reference.md#contract-entity
[Device]: ../api/entity-and-complex-type-reference.md#device-entity
<span data-ttu-id="2d2a9-217">[DirectoryLinkChange]: ../api/entity-and-complex-type-reference.md#directorylinkchange-entity</span><span class="sxs-lookup"><span data-stu-id="2d2a9-217">[DirectoryLinkChange]: ../api/entity-and-complex-type-reference.md#directorylinkchange-entity</span></span>
<span data-ttu-id="2d2a9-218">[DirectoryObject]: ../api/entity-and-complex-type-reference.md#directoryobject-entity</span><span class="sxs-lookup"><span data-stu-id="2d2a9-218">[DirectoryObject]: ../api/entity-and-complex-type-reference.md#directoryobject-entity</span></span>
[DirectoryRole]: ../api/entity-and-complex-type-reference.md#directoryrole-entity
[DirectoryRoleTemplate]: ../api/entity-and-complex-type-reference.md#directoryroletemplate-entity
[ExtensionProperty]: ../api/entity-and-complex-type-reference.md#extensionproperty-entity
<span data-ttu-id="2d2a9-219">[Group]: ../api/entity-and-complex-type-reference.md#group-entity</span><span class="sxs-lookup"><span data-stu-id="2d2a9-219">[Group]: ../api/entity-and-complex-type-reference.md#group-entity</span></span>
[OAuth2PermissionGrant]: ../api/entity-and-complex-type-reference.md#oauth2permissiongrant-entity
[ServicePrincipal]: ../api/entity-and-complex-type-reference.md#serviceprincipal-entity
[SubscribedSku]: ../api/entity-and-complex-type-reference.md#subscribedsku-entity
[TenantDetail]: ../api/entity-and-complex-type-reference.md#tenantdetail-entity
<span data-ttu-id="2d2a9-220">[User]: ../api/entity-and-complex-type-reference.md#user-entity</span><span class="sxs-lookup"><span data-stu-id="2d2a9-220">[User]: ../api/entity-and-complex-type-reference.md#user-entity</span></span>

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
