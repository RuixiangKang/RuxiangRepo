# CATS API for triggering run
In this article
* [Contacts](#contacts)
* [Overview](#overview)
* [Call CATS API via UI tool or code](#usage)

## <a id='contacts'></a> Contacts
|Role |Name; Alias|
|-----|-----|
|Program Manager|Ke Xu (kexu@microsoft.com)|
|Quality|CATS Support (catssupport@microsoft.com)|

## <a id='overview'></a> Overview
### Goals
Triggering CATS run via call CATS API

### <a id='api'></a> API
* **Input parameters**: runname, createdBy, testCaseIds and testUrls
* **Output**: RunId in CATS
* **Request URL**: http://csicesvr/Api/ContentValidation/CreateRun_ForOPS
* **HttpMethod**: Post

## <a id='usage'></a> Usage
### Via PostMan call CAPS API
#### Pre-requisites:
Get Postman for Chrome installed
- Install Postman for Chrome from: https://www.getpostman.com/apps 
> [!TIP] 
> Need to use PostMan for chrome app. You'll get 401.2 - Unauthorized error message if use Postman for windowns, because CATS requires windows authentication and it has not supported in Postman for Windows App now. 

#### How to use in postman
1. Launch Postman for Chrome -- Open "chrome://apps/", then click PostMan
2. Enter request URL
    - **HttpMethod**: *Post*
    - **URL**: mentioned in [API](#api) part
3. Enter request body
    - Click **Body**, select **raw** and **JSON(application/json)**
    - Input Request body with **JSON** format

|Field    |Required |Format   |Comment  |
|---------|---------|---------|---------|
|runName  |Y        | string  |<=32 chars|
|CreatedBy|Y        | Domain\\alias |         |
|testCaseIds |   Y  |  string(split with ', if multi')       | Support multiple testcases, you can get all the CATS cases info from [Get CATS test case list](#get-cats-test-cases-list)  <br> <ul><li>Test case - "IsRequired: true" is required</li><li>Only active cases is allowed - "IsActive: true" </li> </ul>     |
|testUrls    |   Y  |  Standard http(s) url, support docs only now |   Support multiple urls      |

    - Request body format
    <pre> 
    {
        "runName":"{your run name}",
        "createdBy":"{your alias}",
        "testCaseIds": "CATS test CaseId1", "CATS test CaseId2", …,
        "testUrls":["URL1", "URL2"]
    }
    </pre>
    - Request body example
    <pre>
    {
        "runName":"testOps",
        "createdBy":"FAREAST\\v-yulwa",
        "testCaseIds": "22,7",
        "testUrls":["https://docs.microsoft.com/en-us/", "https://docs.microsoft.com/en-us/windows/"]
    }
    </pre>

4. Click **Send**, runid and resultUrl(report page url) will returned,
    ![Trigger a CATS Run](../Images/Trigger_a_CATS_Run.png)

### <a id='get-cats-test-cases-list'></a> Get CATS test case list
1. Launch PostMan from Chrome
2. Enter request URL: http://csicesvr/Api/TestCase/ContentValidation 
3. Select **HttpMethod** as **Get**
4. Click **Send**, the output is the CATS test case list.
    ![Get CATS Test Case List](../Images/Get_CATS_Test_Case_List.png)

