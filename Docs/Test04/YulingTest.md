# CATS API for triggering run
In this article
* [Contacts](#contacts)
* [Overview](#overview)
* [Call CATS API via UI tool or code](#usage)

## <a id='contacts'></a> Contacts
|Role |Name; Alias|
|-----|-----|
|Program Manager|Ke Xu (kexu@microsoft.com)|
|Quality|cpebystest@microsoft.com;|

## <a id='overview'></a> Overview
### Goals
Triggering CATS run via call CATS API

### <a id='api'></a> API
* **Input parameters**: runname, createdBy, testCases and testUrls
* **Output**: RunId in CATS
* **Request URL**: http://csicesvr/Api/ContentValidation/CreateRun_ForOPS
* **HttpMethod**: Post

## <a id='usage'></a> Usage
### Via PostMan call CAPS API
#### Pre-requisites:
Get Postman installed
- If you have not add PostMan App to Chrome. You need to add it to Chrome first.  Step as below:
    1. Open PostMan website: https://www.getpostman.com/
    2. click "Chrome" button, then click "ADD TO CHROME" on the top right corner of pop window.
    3. Launch PostMan App. 
- If the PostMan App had added to Chrome, you just need to launch it. Below is the step.
    1. Launch Chrome, open a new tab
    2. Click the Chrome App icon on the TOP left corner, if there are no Chrome App icon, if not you need to enable it first. 
    3. Click PostMan, the PostMan App will be launched.


> [!Note] 
> Neet to call via chrome PostMan app. CATS API does not support call web service via Windows PostMan app, it will return 401.2 - Unauthorized error message

1. Launch [PostMan](https://www.getpostman.com/) from Chrome (Open blank page in chrome, click Apps in the upper left corner then click PostMan)
2. Enter request URL mentioned in [API](#api) part. Select *HttpMethod* as **Post**
3. Click **Body**, select **raw** and **JSON(application/json)**
4. In request body, input request body with **JSON** format. 
    Request body
        - runName: Required. Any name of you CATS run, not exceed X characters
		- createdBy: Required. Your alias, format: "{Domain}\\alias", it does not check the format of createdBy, but if it does not correct, After the CATS run is kick off, it could not filter the run which is created by yourself via check the "Only Show Runs Created By Me" on CATS.
		- testCaseIds: Required. Select the CATS test cases by ID. It support input one or more CATS test cases. You can get all the CATS cases list and priority on [Get CATS test case list](#get-cats-test-cases-list). 
		- testUrls: Required. The Urls you need to verify, it support input one or more test URLs. CATS will verify the each URL itself. 

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

5. Click **Send**, the output is Runid in CATS, Go back to CATS, you could check results of this run
    ![Trigger a CATS Run](../Images/Trigger_a_CATS_Run.png)

### <a id='get-cats-test-cases-list'></a> Get CATS test case list
1. Launch PostMan from Chrome
2. Enter request URL: http://csicesvr/Api/TestCase/ContentValidation 
3. Select **HttpMethod** as **Get**
4. Click **Send**, the output is the CATS test case list.
    ![Get CATS Test Case List](../Images/Get_CATS_Test_Case_List.png)

