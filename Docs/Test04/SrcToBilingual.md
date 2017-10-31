---
title: Azure AD Graph API Differential Query
author: JimacoMS
ms.TocTitle: Differential query
ms.ContentId: 893b0d6c-0c03-4108-94f5-30566d3e0df9
ms.topic: article (how-tos)
ms.date: 01/26/2016
---

# <span data-ttu-id="60272-102"><a name="differential-query--graph-api-concepts"></a>Differential query | Graph API concepts</span><span class="sxs-lookup"><span data-stu-id="60272-102"><a name="differential-query--graph-api-concepts"></a>Differential query | Graph API concepts</span></span>
    
 <span data-ttu-id="60272-103">_**Applies to:** Graph API | Azure Active Directory_</span><span class="sxs-lookup"><span data-stu-id="60272-103">_**Applies to:** Graph API | Azure Active Directory_</span></span>


<span data-ttu-id="60272-104">This topic discusses the differential query feature of Azure AD Graph API.</span><span class="sxs-lookup"><span data-stu-id="60272-104">This topic discusses the differential query feature of Azure AD Graph API.</span></span> <span data-ttu-id="60272-105">A differential query request returns all changes made to specified entities during the time between two consecutive requests.</span><span class="sxs-lookup"><span data-stu-id="60272-105">A differential query request returns all changes made to specified entities during the time between two consecutive requests.</span></span> <span data-ttu-id="60272-106">For example, if you make a differential query request an hour after the previous differential query request, only the changes made during that hour will be returned.</span><span class="sxs-lookup"><span data-stu-id="60272-106">For example, if you make a differential query request an hour after the previous differential query request, only the changes made during that hour will be returned.</span></span> <span data-ttu-id="60272-107">This functionality is especially useful when synchronizing tenant directory data with an application’s data store.</span><span class="sxs-lookup"><span data-stu-id="60272-107">This functionality is especially useful when synchronizing tenant directory data with an application’s data store.</span></span>

<span data-ttu-id="60272-108">To make a differential query request to a tenant’s directory, your application must be authorized by the tenant.</span><span class="sxs-lookup"><span data-stu-id="60272-108">To make a differential query request to a tenant’s directory, your application must be authorized by the tenant.</span></span> <span data-ttu-id="60272-109">For more information, see [Integrating Applications with Azure Active Directory](https://azure.microsoft.com/en-us/documentation/articles/active-directory-integrating-applications/).</span><span class="sxs-lookup"><span data-stu-id="60272-109">For more information, see [Integrating Applications with Azure Active Directory](https://azure.microsoft.com/en-us/documentation/articles/active-directory-integrating-applications/).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="60272-110">We strongly recommend that you use [Microsoft Graph](https://graph.microsoft.io/) instead of Azure AD Graph API to access Azure Active Directory resources.</span><span class="sxs-lookup"><span data-stu-id="60272-110">We strongly recommend that you use [Microsoft Graph](https://graph.microsoft.io/) instead of Azure AD Graph API to access Azure Active Directory resources.</span></span> <span data-ttu-id="60272-111">Our development efforts are now concentrated on Microsoft Graph and no further enhancements are planned for Azure AD Graph API.</span><span class="sxs-lookup"><span data-stu-id="60272-111">Our development efforts are now concentrated on Microsoft Graph and no further enhancements are planned for Azure AD Graph API.</span></span> <span data-ttu-id="60272-112">There are a very limited number of scenarios for which Azure AD Graph API might still be appropriate; for more information, see the [Microsoft Graph or the Azure AD Graph](https://dev.office.com/blogs/microsoft-graph-or-azure-ad-graph) blog post in the Office Dev Center.</span><span class="sxs-lookup"><span data-stu-id="60272-112">There are a very limited number of scenarios for which Azure AD Graph API might still be appropriate; for more information, see the [Microsoft Graph or the Azure AD Graph](https://dev.office.com/blogs/microsoft-graph-or-azure-ad-graph) blog post in the Office Dev Center.</span></span>

## <span data-ttu-id="60272-113">Differential query requests <a id="DifferentialQueryRequests"></a></span><span class="sxs-lookup"><span data-stu-id="60272-113">Differential query requests <a id="DifferentialQueryRequests"></a></span></span>

<span data-ttu-id="60272-114">This section describes differential query requests.</span><span class="sxs-lookup"><span data-stu-id="60272-114">This section describes differential query requests.</span></span> <span data-ttu-id="60272-115">All requests require the following components:</span><span class="sxs-lookup"><span data-stu-id="60272-115">All requests require the following components:</span></span>

- <span data-ttu-id="60272-116">A valid request URL, including the Graph endpoint for the tenant and applicable query string parameters.</span><span class="sxs-lookup"><span data-stu-id="60272-116">A valid request URL, including the Graph endpoint for the tenant and applicable query string parameters.</span></span>

- <span data-ttu-id="60272-117">An authorization header, including a valid access token issued by Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="60272-117">An authorization header, including a valid access token issued by Azure Active Directory.</span></span> <span data-ttu-id="60272-118">For more information on authenticating to the Graph API, see [Authentication Scenarios for Azure AD](https://azure.microsoft.com/en-us/documentation/articles/active-directory-authentication-scenarios/).</span><span class="sxs-lookup"><span data-stu-id="60272-118">For more information on authenticating to the Graph API, see [Authentication Scenarios for Azure AD](https://azure.microsoft.com/en-us/documentation/articles/active-directory-authentication-scenarios/).</span></span>

### <span data-ttu-id="60272-119"><a name="differential-query-request-url"></a>Differential query request URL</span><span class="sxs-lookup"><span data-stu-id="60272-119"><a name="differential-query-request-url"></a>Differential query request URL</span></span>

<span data-ttu-id="60272-120">The following shows the format of the URL for differential query request; square brackets [] indicate optional elements.</span><span class="sxs-lookup"><span data-stu-id="60272-120">The following shows the format of the URL for differential query request; square brackets [] indicate optional elements.</span></span>

```no-highlight
https://graph.windows.net/<tenantId>/<resourceSet>?api-version=<SupportedApiVersion>&deltaLink=<token>&[$filter=isof(<entityType>)]&[$select=<PropertyList>]
```

<span data-ttu-id="60272-121">The URL is made up of a hierarchical segments followed by a series of query string parameters expressed as key-value pairs.</span><span class="sxs-lookup"><span data-stu-id="60272-121">The URL is made up of a hierarchical segments followed by a series of query string parameters expressed as key-value pairs.</span></span>

### <span data-ttu-id="60272-122"><a name="url-hierarchical-segments"></a>URL: Hierarchical segments</span><span class="sxs-lookup"><span data-stu-id="60272-122"><a name="url-hierarchical-segments"></a>URL: Hierarchical segments</span></span>

|<span data-ttu-id="60272-123">Segment</span><span class="sxs-lookup"><span data-stu-id="60272-123">Segment</span></span>|<span data-ttu-id="60272-124">Description</span><span class="sxs-lookup"><span data-stu-id="60272-124">Description</span></span>|
|---|---|
|<span data-ttu-id="60272-125">tenantId</span><span class="sxs-lookup"><span data-stu-id="60272-125">tenantId</span></span>|<span data-ttu-id="60272-126">The unique identifier of the tenant that the query should be executed against.</span><span class="sxs-lookup"><span data-stu-id="60272-126">The unique identifier of the tenant that the query should be executed against.</span></span> <span data-ttu-id="60272-127">This is typically one of the verified domains (**verifiedDomains** property) of the tenant, but it can also be the tenant’s object ID (**objectId** property).</span><span class="sxs-lookup"><span data-stu-id="60272-127">This is typically one of the verified domains (**verifiedDomains** property) of the tenant, but it can also be the tenant’s object ID (**objectId** property).</span></span> <span data-ttu-id="60272-128">Not case-sensitive.</span><span class="sxs-lookup"><span data-stu-id="60272-128">Not case-sensitive.</span></span>|
|<span data-ttu-id="60272-129">resourceSet</span><span class="sxs-lookup"><span data-stu-id="60272-129">resourceSet</span></span>|<span data-ttu-id="60272-130">The specific set of tenant resources this query should be executed against.</span><span class="sxs-lookup"><span data-stu-id="60272-130">The specific set of tenant resources this query should be executed against.</span></span> <span data-ttu-id="60272-131">Determines what resources are returned in the query response.</span><span class="sxs-lookup"><span data-stu-id="60272-131">Determines what resources are returned in the query response.</span></span> <span data-ttu-id="60272-132">Supported values are: “directoryObjects”, “users”, “contacts” or “groups”.</span><span class="sxs-lookup"><span data-stu-id="60272-132">Supported values are: “directoryObjects”, “users”, “contacts” or “groups”.</span></span> <span data-ttu-id="60272-133">Values are case-sensitive.</span><span class="sxs-lookup"><span data-stu-id="60272-133">Values are case-sensitive.</span></span>|

### <span data-ttu-id="60272-134"><a name="url-query-string-parameters"></a>URL: Query string parameters</span><span class="sxs-lookup"><span data-stu-id="60272-134"><a name="url-query-string-parameters"></a>URL: Query string parameters</span></span>

|<span data-ttu-id="60272-135">Parameter</span><span class="sxs-lookup"><span data-stu-id="60272-135">Parameter</span></span>|<span data-ttu-id="60272-136">Description</span><span class="sxs-lookup"><span data-stu-id="60272-136">Description</span></span>|
|---|---|
|<span data-ttu-id="60272-137">api-version</span><span class="sxs-lookup"><span data-stu-id="60272-137">api-version</span></span>|<span data-ttu-id="60272-138">Specifies the version of the Graph API against which the request is issued.</span><span class="sxs-lookup"><span data-stu-id="60272-138">Specifies the version of the Graph API against which the request is issued.</span></span> <span data-ttu-id="60272-139">Required.</span><span class="sxs-lookup"><span data-stu-id="60272-139">Required.</span></span> <span data-ttu-id="60272-140">Beginning with version 1.5, the value is expressed as a numeric version number; for example, api-version=1.5.</span><span class="sxs-lookup"><span data-stu-id="60272-140">Beginning with version 1.5, the value is expressed as a numeric version number; for example, api-version=1.5.</span></span> <span data-ttu-id="60272-141">For previous versions, the value is a string of the form YYYY-MM-DD; for example, api-version=2013-04-05.</span><span class="sxs-lookup"><span data-stu-id="60272-141">For previous versions, the value is a string of the form YYYY-MM-DD; for example, api-version=2013-04-05.</span></span><br/> <br/><span data-ttu-id="60272-142">(Replaces the use of **x-ms-dirapi-data-contract-version** header in the preview version of Graph API.)</span><span class="sxs-lookup"><span data-stu-id="60272-142">(Replaces the use of **x-ms-dirapi-data-contract-version** header in the preview version of Graph API.)</span></span>|
|<span data-ttu-id="60272-143">deltaLink</span><span class="sxs-lookup"><span data-stu-id="60272-143">deltaLink</span></span>|<span data-ttu-id="60272-144">The token returned in either the **deltaLink** property or the **nextLink** property in the last response.</span><span class="sxs-lookup"><span data-stu-id="60272-144">The token returned in either the **deltaLink** property or the **nextLink** property in the last response.</span></span> <span data-ttu-id="60272-145">Required, but will be empty on the first request.</span><span class="sxs-lookup"><span data-stu-id="60272-145">Required, but will be empty on the first request.</span></span> <br/> <br/><span data-ttu-id="60272-146">(Replaces the *skipToken* query string parameter in the preview version of Graph API.)</span><span class="sxs-lookup"><span data-stu-id="60272-146">(Replaces the *skipToken* query string parameter in the preview version of Graph API.)</span></span>|
|<span data-ttu-id="60272-147">$filter</span><span class="sxs-lookup"><span data-stu-id="60272-147">$filter</span></span>|<span data-ttu-id="60272-148">Indicates which entity types should be included in the response.</span><span class="sxs-lookup"><span data-stu-id="60272-148">Indicates which entity types should be included in the response.</span></span> <span data-ttu-id="60272-149">Optional.</span><span class="sxs-lookup"><span data-stu-id="60272-149">Optional.</span></span> <span data-ttu-id="60272-150">The supported entity types are: User, Group and Contact.</span><span class="sxs-lookup"><span data-stu-id="60272-150">The supported entity types are: User, Group and Contact.</span></span> <span data-ttu-id="60272-151">Only valid when &ltresourceSet&gt is “directoryObjects”; otherwise, &ltresourceSet&gt overrides the filter.</span><span class="sxs-lookup"><span data-stu-id="60272-151">Only valid when &ltresourceSet&gt is “directoryObjects”; otherwise, &ltresourceSet&gt overrides the filter.</span></span> <span data-ttu-id="60272-152">For example, if &ltresourceSet&gt is “users”, and the *$filter* parameter is also specified, only changes for users will be returned regardless of what is specified in the filter value.</span><span class="sxs-lookup"><span data-stu-id="60272-152">For example, if &ltresourceSet&gt is “users”, and the *$filter* parameter is also specified, only changes for users will be returned regardless of what is specified in the filter value.</span></span> <span data-ttu-id="60272-153">If &ltresourceSet&gt is “directoryObjects” and *$filter* is not specified, changes for all of the supported entity types (User, Group, and Contact) are returned.</span><span class="sxs-lookup"><span data-stu-id="60272-153">If &ltresourceSet&gt is “directoryObjects” and *$filter* is not specified, changes for all of the supported entity types (User, Group, and Contact) are returned.</span></span><br/><br/><span data-ttu-id="60272-154">To specify a single entity type, use one of the following:</span><span class="sxs-lookup"><span data-stu-id="60272-154">To specify a single entity type, use one of the following:</span></span><ul><li>`$filter=isof('Microsoft.WindowsAzure.ActiveDirectory.Contact')`</li><li>`$filter=isof('Microsoft.WindowsAzure.ActiveDirectory.User')`</li><li>`$filter=isof('Microsoft.WindowsAzure.ActiveDirectory.Group')`</li></ul><span data-ttu-id="60272-155">To specify multiple entity types, use the **or** operator; for example, `$filter=isof('Microsoft.WindowsAzure.ActiveDirectory.User')%20or%20isof('Microsoft.WindowsAzure.ActiveDirectory.Group')`.</span><span class="sxs-lookup"><span data-stu-id="60272-155">To specify multiple entity types, use the **or** operator; for example, `$filter=isof('Microsoft.WindowsAzure.ActiveDirectory.User')%20or%20isof('Microsoft.WindowsAzure.ActiveDirectory.Group')`.</span></span> <br/> <br/><span data-ttu-id="60272-156">(Replaces  the *objectScope* query string parameter in the preview version of Graph API.)</span><span class="sxs-lookup"><span data-stu-id="60272-156">(Replaces  the *objectScope* query string parameter in the preview version of Graph API.)</span></span><br/> <br/><span data-ttu-id="60272-157">**Important** Beginning with version 1.5, the Graph API namespace is changed from Microsoft.WindowsAzure.ActiveDirectory to Microsoft.DirectoryServices.</span><span class="sxs-lookup"><span data-stu-id="60272-157">**Important** Beginning with version 1.5, the Graph API namespace is changed from Microsoft.WindowsAzure.ActiveDirectory to Microsoft.DirectoryServices.</span></span> <span data-ttu-id="60272-158">Earlier versions of the Graph API continue to use the previous namespace; for example, `$filter=isof('Microsoft.WindowsAzure.ActiveDirectory.Contact')`.</span><span class="sxs-lookup"><span data-stu-id="60272-158">Earlier versions of the Graph API continue to use the previous namespace; for example, `$filter=isof('Microsoft.WindowsAzure.ActiveDirectory.Contact')`.</span></span>|
|<span data-ttu-id="60272-159">$select</span><span class="sxs-lookup"><span data-stu-id="60272-159">$select</span></span>|<span data-ttu-id="60272-160">Specifies which properties should be included in the response.</span><span class="sxs-lookup"><span data-stu-id="60272-160">Specifies which properties should be included in the response.</span></span> <span data-ttu-id="60272-161">If *$select^ is not specified, all properties are included.</span><span class="sxs-lookup"><span data-stu-id="60272-161">If *$select^ is not specified, all properties are included.</span></span><br/><br/><span data-ttu-id="60272-162">Properties can be specified either fully qualified or non-qualified.</span><span class="sxs-lookup"><span data-stu-id="60272-162">Properties can be specified either fully qualified or non-qualified.</span></span> <span data-ttu-id="60272-163">Multiple properties are delimited by commas.</span><span class="sxs-lookup"><span data-stu-id="60272-163">Multiple properties are delimited by commas.</span></span><ul><li><span data-ttu-id="60272-164">Fully qualified properties have the entity type specified; for example, `User/displayName`.</span><span class="sxs-lookup"><span data-stu-id="60272-164">Fully qualified properties have the entity type specified; for example, `User/displayName`.</span></span> <span data-ttu-id="60272-165">Fully qualified properties MUST be used if &ltresourceSet&gt is specified as “directoryObjects”; for example, `https://graph.windows.net/contoso.com/directoryObjects?api-version=2013-04-05&deltaLink=&$select=User/displayName,Group/description`.</span><span class="sxs-lookup"><span data-stu-id="60272-165">Fully qualified properties MUST be used if &ltresourceSet&gt is specified as “directoryObjects”; for example, `https://graph.windows.net/contoso.com/directoryObjects?api-version=2013-04-05&deltaLink=&$select=User/displayName,Group/description`.</span></span></li><li><span data-ttu-id="60272-166">Non-qualified properties do not have the entity type specified; for example, `displayName`.</span><span class="sxs-lookup"><span data-stu-id="60272-166">Non-qualified properties do not have the entity type specified; for example, `displayName`.</span></span> <span data-ttu-id="60272-167">They can only be used when &ltresourceSet&gt is specified as a value other than “directoryObjects”; for example, `https://graph.windows.net/contoso.com/users?api-version=2013-04-05&deltaLink=&$select=displayName,jobTitle`.</span><span class="sxs-lookup"><span data-stu-id="60272-167">They can only be used when &ltresourceSet&gt is specified as a value other than “directoryObjects”; for example, `https://graph.windows.net/contoso.com/users?api-version=2013-04-05&deltaLink=&$select=displayName,jobTitle`.</span></span></li></ul>

<span data-ttu-id="60272-168">**Note**: The key-value pairs in the query string are case-sensitive, but their order is not significant.</span><span class="sxs-lookup"><span data-stu-id="60272-168">**Note**: The key-value pairs in the query string are case-sensitive, but their order is not significant.</span></span>

<span data-ttu-id="60272-169">The following is an example of the simplest differential query.</span><span class="sxs-lookup"><span data-stu-id="60272-169">The following is an example of the simplest differential query.</span></span> <span data-ttu-id="60272-170">This is used during initial sync.</span><span class="sxs-lookup"><span data-stu-id="60272-170">This is used during initial sync.</span></span>

```no-highlight
https://graph.windows.net/contoso.com/directoryObjects?api-version=2013-04-05&deltaLink=
```

<span data-ttu-id="60272-171">The following is an example of a subsequent request.</span><span class="sxs-lookup"><span data-stu-id="60272-171">The following is an example of a subsequent request.</span></span>

```no-highlight
https://graph.windows.net/contoso.com/directoryObjects?api-version=2013-04-05&deltaLink=AAABAGCL8z4m%2bc9IJGIzYjFmYzU5LTg0YjgtNDQwMC1hNzE1LWVhOGE3ZTQwZjRmZQBuvX43ACZQT4LRVPug8An6AAABAANIABAfGgAQwAMAJDCHA5AAABAATCkAA44TADQnhAQAIAAAgAHAAwAQAAAA8rLSTyfq5U`
```

## <span data-ttu-id="60272-172">Differential query responses <a id="DifferentialQueryResponses"></a></span><span class="sxs-lookup"><span data-stu-id="60272-172">Differential query responses <a id="DifferentialQueryResponses"></a></span></span>

<span data-ttu-id="60272-173">This section describes the contents of a differential query response that is returned when a differential query request is made.</span><span class="sxs-lookup"><span data-stu-id="60272-173">This section describes the contents of a differential query response that is returned when a differential query request is made.</span></span> <span data-ttu-id="60272-174">The following list describes the contents of a response:</span><span class="sxs-lookup"><span data-stu-id="60272-174">The following list describes the contents of a response:</span></span>

- <span data-ttu-id="60272-175">Zero to 200 [DirectoryObject] entities, each of which contains changes for a specific [User], [Group], or [Contact] object.</span><span class="sxs-lookup"><span data-stu-id="60272-175">Zero to 200 [DirectoryObject] entities, each of which contains changes for a specific [User], [Group], or [Contact] object.</span></span>

- <span data-ttu-id="60272-176">Zero to 3000 [DirectoryLinkChange] entities, each of which contains changes for a specific **member** or **manager** link.</span><span class="sxs-lookup"><span data-stu-id="60272-176">Zero to 3000 [DirectoryLinkChange] entities, each of which contains changes for a specific **member** or **manager** link.</span></span>

- <span data-ttu-id="60272-177">Either a **deltaLink** or a **nextLink** property.</span><span class="sxs-lookup"><span data-stu-id="60272-177">Either a **deltaLink** or a **nextLink** property.</span></span> <span data-ttu-id="60272-178">In either case, its value is a case-sensitive, URL-encoded string that embeds state information about the set of directory changes that have been returned to the client, with respect to remaining changes that have occurred in the directory.</span><span class="sxs-lookup"><span data-stu-id="60272-178">In either case, its value is a case-sensitive, URL-encoded string that embeds state information about the set of directory changes that have been returned to the client, with respect to remaining changes that have occurred in the directory.</span></span> <span data-ttu-id="60272-179">This string (or token) should be included in the **deltaLink** query string parameter in the next differential query request.</span><span class="sxs-lookup"><span data-stu-id="60272-179">This string (or token) should be included in the **deltaLink** query string parameter in the next differential query request.</span></span>

  - <span data-ttu-id="60272-180">If the **deltaLink** property is returned, there are no more directory changes left for the application to synchronize after this response.</span><span class="sxs-lookup"><span data-stu-id="60272-180">If the **deltaLink** property is returned, there are no more directory changes left for the application to synchronize after this response.</span></span> <span data-ttu-id="60272-181">The application can wait for some predetermined time according to its own requirements to issue the next differential query request.</span><span class="sxs-lookup"><span data-stu-id="60272-181">The application can wait for some predetermined time according to its own requirements to issue the next differential query request.</span></span>

  - <span data-ttu-id="60272-182">If the **nextLink** property is returned, there are directory changes remaining for the application to synchronize after this response.</span><span class="sxs-lookup"><span data-stu-id="60272-182">If the **nextLink** property is returned, there are directory changes remaining for the application to synchronize after this response.</span></span> <span data-ttu-id="60272-183">The application should issue the next differential query request at its earliest convenience.</span><span class="sxs-lookup"><span data-stu-id="60272-183">The application should issue the next differential query request at its earliest convenience.</span></span>

<span data-ttu-id="60272-184">Responses are always returned in JSON format.</span><span class="sxs-lookup"><span data-stu-id="60272-184">Responses are always returned in JSON format.</span></span>

## <span data-ttu-id="60272-185">Considerations when using differential query <a id="ConsiderationsWhenUsingDq"></a></span><span class="sxs-lookup"><span data-stu-id="60272-185">Considerations when using differential query <a id="ConsiderationsWhenUsingDq"></a></span></span>

<span data-ttu-id="60272-186">The following list highlights important considerations for applications that use differential query:</span><span class="sxs-lookup"><span data-stu-id="60272-186">The following list highlights important considerations for applications that use differential query:</span></span>

- <span data-ttu-id="60272-187">Changes returned by differential query represent the state of the directory objects at the time of the response.</span><span class="sxs-lookup"><span data-stu-id="60272-187">Changes returned by differential query represent the state of the directory objects at the time of the response.</span></span> <span data-ttu-id="60272-188">Your application must not treat these changes as transaction logs for replay.</span><span class="sxs-lookup"><span data-stu-id="60272-188">Your application must not treat these changes as transaction logs for replay.</span></span>

- <span data-ttu-id="60272-189">Changes appear in the order in which they occurred.</span><span class="sxs-lookup"><span data-stu-id="60272-189">Changes appear in the order in which they occurred.</span></span> <span data-ttu-id="60272-190">The most-recently changed objects appear last even if the object was updated multiple times.</span><span class="sxs-lookup"><span data-stu-id="60272-190">The most-recently changed objects appear last even if the object was updated multiple times.</span></span> <span data-ttu-id="60272-191">Their order is also not affected by when the client received the changes.</span><span class="sxs-lookup"><span data-stu-id="60272-191">Their order is also not affected by when the client received the changes.</span></span> <span data-ttu-id="60272-192">As a result, it is possible for changes to be presented out of order compared to how they initially occurred in the directory.</span><span class="sxs-lookup"><span data-stu-id="60272-192">As a result, it is possible for changes to be presented out of order compared to how they initially occurred in the directory.</span></span>

- <span data-ttu-id="60272-193">Your application must be prepared for replays, which occur when the same change appears in subsequent responses.</span><span class="sxs-lookup"><span data-stu-id="60272-193">Your application must be prepared for replays, which occur when the same change appears in subsequent responses.</span></span> <span data-ttu-id="60272-194">While differential query makes a best effort to reduce replays, they are still possible.</span><span class="sxs-lookup"><span data-stu-id="60272-194">While differential query makes a best effort to reduce replays, they are still possible.</span></span>

- <span data-ttu-id="60272-195">Your application must be prepared to handle a deletion change for an object it was not aware of.</span><span class="sxs-lookup"><span data-stu-id="60272-195">Your application must be prepared to handle a deletion change for an object it was not aware of.</span></span>

- <span data-ttu-id="60272-196">Differential query can return a link to a source or target object that has not yet been returned by other responses.</span><span class="sxs-lookup"><span data-stu-id="60272-196">Differential query can return a link to a source or target object that has not yet been returned by other responses.</span></span>

- <span data-ttu-id="60272-197">See the [Additional differential query features](#AdditionalDifferentialQueryFeatures) section below for more information on using request headers to constrain your queries to improve performance.</span><span class="sxs-lookup"><span data-stu-id="60272-197">See the [Additional differential query features](#AdditionalDifferentialQueryFeatures) section below for more information on using request headers to constrain your queries to improve performance.</span></span>

## <span data-ttu-id="60272-198">Request and response examples <a id="ReqeustAndResponseExamples"></a></span><span class="sxs-lookup"><span data-stu-id="60272-198">Request and response examples <a id="ReqeustAndResponseExamples"></a></span></span>

<span data-ttu-id="60272-199">The following is an example of an initial differential query request:</span><span class="sxs-lookup"><span data-stu-id="60272-199">The following is an example of an initial differential query request:</span></span>

```no-highlight
GET https://graph.windows.net/contoso.com/directoryObjects?api-version=2013-04-05&$filter=isof('Microsoft.WindowsAzure.ActiveDirectory.User')%20or%20isof('Microsoft.WindowsAzure.ActiveDirectory.Group')%20or%20isof('Microsoft.WindowsAzure.ActiveDirectory.Contact')&deltaLink= HTTP /1.1
Authorization: Bearer eyJ0eXAiOiJKV . . . KUAe1EQ
Host: graph.windows.net
```
<span data-ttu-id="60272-200">The following is an example of an incremental differential query request:</span><span class="sxs-lookup"><span data-stu-id="60272-200">The following is an example of an incremental differential query request:</span></span>

```no-highlight
https://graph.windows.net/contoso.com/directoryObjects?api-version=2013-04-05&$filter=isof('Microsoft.WindowsAzure.ActiveDirectory.User')%20or%20isof('Microsoft.WindowsAzure.ActiveDirectory.Group')%20or%20isof('Microsoft.WindowsAzure.ActiveDirectory.Contact')&deltaLink=AAABAGCL8z4m%2bc9IJGIzYjFmYzU5LTg0YjgtNDQwMC1hNzE1LWVhOGE3ZTQwZjRmZQBuvX43ACZQT4LRVPug8An6AAABAANIABAfGgAQwAMAJDCHA5AAABAATCkAA44TADQnhAQAIAAAgAHAAwAQAAAA8rLSTyfq5U
 HTTP /1.1
Authorization: Bearer eyJ0eXAiOiJKV . . . KUAe1EQ
Host: graph.windows.net
```

<span data-ttu-id="60272-201">**Note**: In both of these sample requests, the *$filter* query parameter is shown for demonstration purposes only.</span><span class="sxs-lookup"><span data-stu-id="60272-201">**Note**: In both of these sample requests, the *$filter* query parameter is shown for demonstration purposes only.</span></span> <span data-ttu-id="60272-202">Because the filter specifies all of the possible target types for the differential query (User, Group, and Contact), it could be omitted and the query would return changes for all of these entity types by default.</span><span class="sxs-lookup"><span data-stu-id="60272-202">Because the filter specifies all of the possible target types for the differential query (User, Group, and Contact), it could be omitted and the query would return changes for all of these entity types by default.</span></span>

<span data-ttu-id="60272-203">The following example response demonstrates the returned JSON:</span><span class="sxs-lookup"><span data-stu-id="60272-203">The following example response demonstrates the returned JSON:</span></span>

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
## <span data-ttu-id="60272-204">Additional differential query features <a id="AdditionalDifferentialQueryFeatures"></a></span><span class="sxs-lookup"><span data-stu-id="60272-204">Additional differential query features <a id="AdditionalDifferentialQueryFeatures"></a></span></span>

<span data-ttu-id="60272-205">Differential Queries can now return only updated properties and links – this allows faster processing and reduced payloads for Differential Query calls.</span><span class="sxs-lookup"><span data-stu-id="60272-205">Differential Queries can now return only updated properties and links – this allows faster processing and reduced payloads for Differential Query calls.</span></span> <span data-ttu-id="60272-206">This option is enabled by setting the header **ocp-aad-dq-include-only-changed-properties** to true as shown in this example.</span><span class="sxs-lookup"><span data-stu-id="60272-206">This option is enabled by setting the header **ocp-aad-dq-include-only-changed-properties** to true as shown in this example.</span></span>

```no-highlight
GET https://graph.windows.net/contoso.com/users?api-version=2013-11-08&deltaLink= furK18V1T….
HTTP /1.1
ocp-aad-dq-include-only-changed-properties : true
Content-Type: application/json
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5nl0aEV1T….
Response: 200 OK
```

<span data-ttu-id="60272-207">For example if only the “displayName” property of user has changed.</span><span class="sxs-lookup"><span data-stu-id="60272-207">For example if only the “displayName” property of user has changed.</span></span>  <span data-ttu-id="60272-208">The returned object would be similar to this:</span><span class="sxs-lookup"><span data-stu-id="60272-208">The returned object would be similar to this:</span></span>

```no-highlight
{     
          "displayName" : "AnUpdatedDisplayName",
         "objectId" :  "c1bf5c59-7974-4aff-a59a-f8dfe9809b18",
         "objectType" :  "User",
          "odata.type" :  "Microsoft.WindowsAzure.ActiveDirectory.User"
},

```

<span data-ttu-id="60272-209">**Differential Sync support to sync from “now”** - a special header can be specified, requesting to only get an up-to-date deltaToken, this token can be used in subsequent queries, which will return only changes from “now”.</span><span class="sxs-lookup"><span data-stu-id="60272-209">**Differential Sync support to sync from “now”** - a special header can be specified, requesting to only get an up-to-date deltaToken, this token can be used in subsequent queries, which will return only changes from “now”.</span></span>  <span data-ttu-id="60272-210">Here’s the example call:</span><span class="sxs-lookup"><span data-stu-id="60272-210">Here’s the example call:</span></span>

```no-highlight
GET https://graph.windows.net/contoso.com/users?api-version=2013-11-08&deltaLink= smLYT8V1T…
HTTP /1.1
ocp-aad-dq-include-only-delta-token: true
Content-Type: application/json
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5nl0aEV1T….
Response: 200 OK
```

<span data-ttu-id="60272-211">The response will contain the **deltaLink**, but will not have the changed object(s), similar to this:</span><span class="sxs-lookup"><span data-stu-id="60272-211">The response will contain the **deltaLink**, but will not have the changed object(s), similar to this:</span></span>

```no-highlight
{   …  "aad.deltaLink":https://graph.windows.net/contoso.com/users?deltaLink=MRa43......   }
```

<span data-ttu-id="60272-212">**Detecting a deleted item** – the response can also be used to detect a deleted item.</span><span class="sxs-lookup"><span data-stu-id="60272-212">**Detecting a deleted item** – the response can also be used to detect a deleted item.</span></span>  <span data-ttu-id="60272-213">Deleted objects and deleted links are indicated by the "aad.isDeleted" property with a value set to true; this is necessary to make sure applications can learn about the deletion of previously created objects and links.</span><span class="sxs-lookup"><span data-stu-id="60272-213">Deleted objects and deleted links are indicated by the "aad.isDeleted" property with a value set to true; this is necessary to make sure applications can learn about the deletion of previously created objects and links.</span></span>

##<span data-ttu-id="60272-214">Additional resources <a id="AdditionalResources"></a></span><span class="sxs-lookup"><span data-stu-id="60272-214">Additional resources <a id="AdditionalResources"></a></span></span>

- [<span data-ttu-id="60272-215">Azure AD Graph API reference</span><span class="sxs-lookup"><span data-stu-id="60272-215">Azure AD Graph API reference</span></span>](../api/api-catalog.md)

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
