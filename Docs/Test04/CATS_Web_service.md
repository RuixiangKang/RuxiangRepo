# CATS Web Service for broken links check

## <a id='contacts'></a> Contacts
|Role|Name; Alias|
|-----|----------|
|Program Manager|Ke Xu (kexu@microsoft.com)|
|Developer|Bin Zhou (v-bzho@microsoft.com)|
|Quality|cpebystest@microsoft.com;<br>Kalyan C Kesireddy(kalyanke@microsoft.com)|

## <a id='overview'></a>Overview
### Goals

* To enable CATS and SkyEye get broken links with input url list
* Provide high accuracy rate and performance

### Dependencies and Risks
* The input urls count of this web service is limited to 1000
* The performance of the web service is depend on network and host machine performance

## <a id='requirement'></a> Functional Requirements
List the requirements that the web service should satisfy in order of priority:
|Must-Have Requirements (P1)|Desired Requirements (P2 – P3)|
|--------|-----------|
|Ability to handle any exception|Able to export outputs to a local file or database|
|Ability to output accurate broken links|Host on Azure web service|
|Ability to provide high performance||

## <a id='usage'></a> Usage
### <a id='host'></a> Host
* The web service host URL is http://10.213.224.35/, 
* Access url is: http://10.213.224.35/api/link/getbrokenlink

### Call
User could call the web service api via UI tool (PostMan)

#### Via UI Tool, recommended tool is Chrome [PostMan](https://www.getpostman.com/), it’s a Chrome app that operation easily
1. Launch **PostMan** from Chrome (Open blank page in chrome, click Apps in the upper left corner then click PostMan)
2. Entry access url mentioned in [Host](#host) part. Select *HttpMethod* as **Post**
3. Click Body, select **raw** and select display text as **JSON(application/json)**
4. In request body, input url list with **JSON** format. The input urls count is limited to 1000

#### Request header
   - **Method**: Post
   - **Request URL**: {Host}/api/link/getbrokenlink  (e.g. http://10.213.224.35/api/link/getbrokenlink)
   - **Body**:
     - Option: raw, Json 
     - Content: 
       - ["Single URL"]
       - ["URL1", "URL2","URL3"]   
5. Click **Send** to get output broken links
![Call Web Service Througn PostMan image](../Images/Call_Web_Service_Through_PostMan.png)

### View Result
 User could get the result either via [**SkyEye Broken link report**](http://aka.ms/skyeye/brokenlink) or via **UI tool** (PostMan)
 - SkyEye will run broken link daily check and generate the latest report in [SkyEye Broken link report](http://aka.ms/skyeye/brokenlink)
   - User can open the above report link to check the latest report for broken link.
   [The_Latest_SkyeEyeBrokenLink_Report](The_Latest_SkyeEyeBrokenLink_Report.png)
  
 - In UI tool response body, after clicking "Send" at step5, select Json format, and the test result will display:
   
   {
     - **Url**: the input URL   
     - **BrokenLinks**: 
        * The webservice will get urls in main body. 
        * If there is Broken Link in request test pages, it would report the link and link text, or the value is null.
        * If there is one link with multiple link text, it will report all.
        * If there are multiple link and link text is the same, will report one and skip the rest.
        * It would skip example link. (exclude example links by keywords and [example link writelist](https://review.docs.microsoft.com/en-us/help/onboard/example-link-white-list))
     - **ResponseCode**: The response status of URL request
     
    }
    
   for example:   
   <pre> 
   {
    "https://docs.microsoft.com/en-us/": 
    {
        "Url": "https://docs.microsoft.com/en-us/",
        "BrokenLinks": [],
        "ResponseCode": 200
    }
   }
    </pre>
    
 #### Meaning of ResponseCode
  - **200**: Success
  - **1000**: Can't get http response
  - **1001**: Can get http response, but IsSuccessStatusCode of http response is falseCan get http response, but IsSuccessStatusCode of http response is false
  - **1002**: Input url is not a validate http url
  - **1003**: Internal docs url and need user to login
    
 Also, you can call the API via *httpget* method with no input parameter to check the definitions of response code.
    |Field	|Value|
    |------|-------|
	|Method|get|
	|Request URL	|{Host}/api/link/getresponsecode <br>e.g. http://10.213.224.35/api/link/getresponsecode |

    ![Get Response Code]()   


