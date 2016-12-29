## Alert with Link(Nest)

> [!NOTE] 
> Nest with External Link [baidu](http://www.baidu.com/) Nest with External Link 

> [!WARNING] 
> [baidu](http://www.baidus.com/) Nest with invalid External Link 

> [!TIP] 
> Nest with External Link  [baidu](http://www.baidu.com/)

> [!IMPORTANT] 
> Nest with Internal Link [InternalLink](Link.md) Nest with Internal Link 

> [!CAUTION] 
> Nest with Internal Link [BookMark](#Alert without message)  

## Alert with Link only
> [!WARNING] 
> [baidu](http://www.baidus.com/) 

## Alert with Image
> [!NOTE] 
>  Text before Image 
> ![I am flower](./Images/flower.jpg "This is A/t text")
>  Text after Image

> [!WARNING] 
> ![I am flower](./Images/flower.jpg "This is A/t text")
>  Text after Image

> [!TIP] 
>  Text before Image 
> ![I am flower](./Images/flower.jpg "This is A/t text")

> [!IMPORTANT] 
> External image
> <img src="http://pic33.nipic.com/20130916/3420027_192919547000_2.jpg" width = "860" height ="645"/>

> [!CAUTION] 
> invalid Image source
> ![flower](./Images/flowers.jpg)

## Alert with image only
> [!TIP]
> ![](./Images/flower.jpg)

## Alert with image &code only
> [!TIP]
> ![](./Images/flower.jpg)
>```
> On July 2, an alien ship entered Earth's orbit and deployed several dozen saucer-shaped "destroyer" spacecraft, each 15 miles (24 km) wide.  
>  On July 3, the Black Knights, a squadron of Marine Corps F/A-18 Hornets, participated 
>```


> [!TIP]
> ![I am flower](./Images/flower.jpg "This is A/t text")

## Alert with code only
> [!NOTE] 
>```
> On July 2, an alien ship entered Earth's orbit and deployed several dozen saucer-shaped "destroyer" spacecraft, each 15 miles (24 km) wide.
>  On July 3, the Black Knights, a squadron of Marine Corps F/A-18 Hornets, participated 
>```

## Alert with code & text
>[!NOTE]  
>This part is text!
>On July 2, an alien ship entered Earth's orbit and deployed several dozen saucer-shaped "destroyer" spacecraft, each 15 miles (24 km) wide.
>On July 3, the Black Knights, a squadron of Marine Corps F/A-18 Hornets, participated 
>```

## Alert with List
> [!WARNING] 
> Bulleted list 
>* aaa
>* bbb
>* ccc

> [!TIP] 
>Bulleted list with empty item
>* aaa
>* bbb
>* 
>* ccc 

> [!IMPORTANT] 
>Order list with empty item
>1. aaa
>2. 
>3. bbb
>4. ccc


> [!CAUTION] 
> Order list with ,
>1. aaa
>2, bbb
>3. ccc

> [!CAUTION] 
>Orfer list with 3.7.9
>3. aaa
>7. bbb
>9. ccc

## Alert with table
> [!NOTE] 
> Normal table
>|1|2|3|
>|-------|------- |------- |
>|1|2|3|


> [!WARNING] 
>table with cell missing
> Cell missing  | Second Header
>------------- | -------------
>Content Cell  | Content Cell
> | 
>Content Cell  | Content Cell

> [!TIP] 
>table with empty row
> | empty row | Description          |
>| ------------- | ----------- |
>| Help      | Display the help window.|
>| Close     | _Closes_ a window     |
>|      |    |


> [!IMPORTANT] 
> table with empty row(by desighed)
>|  |           |
>| ------------- | ----------- |
>| The first row is empty      | Display the help window.|
>| Close     | _Closes_ a window    
>

> [!CAUTION] 
> Abnormal table
>| Normal table | Description          |
>| Help      | Display the help window.|
>| Close     | _Closes_ a window     |


## Alert with table, link, list
> [!CAUTION] 
> Normal table with invalid List
>| Normal table with invalid List | Description          |
>| ------------- | ----------- |
>| [bing.com](http://www.bing.com)      |  test|
>| Close     | 1. item1 <br> 2. Item2 <br> 3.    |

## Alert with table 
> [!CAUTION] 
>note
>> [!NOTE]
>>invalid Image source
>> ![flower](./Image/flower.jpg)

### Nested Alert
> [!NOTE] 
>>Note
>>[!WARNING] 
>>Sample Warning Message


> [!NOTE]
>  On July 2, an alien ship entered Earth's orbit and deployed several dozen saucer-shaped "destroyer" spacecraft, each 15 miles (24 km) wide.
  
>  On July 3, the Black Knights, a squadron of Marine Corps F/A-18 Hornets, participated 


> [!NOTE]
>>  On July 2, an alien ship entered Earth's orbit and deployed several dozen saucer-shaped "destroyer" spacecraft, each 15 miles (24 km) wide.
  
>>  On July 3, the Black Knights, a squadron of Marine Corps F/A-18 Hornets, participated 



> [!NOTE]
> Test link
> https://zhidao.baidu.com/search?ct=17&pn=0&tn=ikaslist&rn=10&word=win8%E5%A6%82%E4%BD%95%E6%89%93%E5%BC%80screen%20reader%E5%BF%AB%E6%8D%B7%E9%94%AE
