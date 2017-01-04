# Image

### The image displayed normal with Alt text

> ![I am flower](./Images/flower.jpg "This is A/t text")

### The image displayed normal with no Alt text
  ![](./Images/flower.jpg)
  ![](./Images/flower.jpg)
  ![](./Images/flower.jpg)
    
### The image displayed normal with reference insert
  ![Flower][Flower]
  [Flower]: ./Images/flower.jpg
  
### invalid insert style
![flower][./Images/flower.jpg]


### invalid image source
![flower](./Images/flowers.jpg)


### The normal image with too greater width (External image)
<img src="http://pic33.nipic.com/20130916/3420027_192919547000_2.jpg" width = "860" height ="645"/>


### The normal image with nornal width (External image)
<img src="http://pic33.nipic.com/20130916/3420027_192919547000_2.jpg" width = "200" height ="100"/>


## Image in table

| ![smile](./Image/Flower.jpg) | Only images on Row/Column |
| ------------- | ----------- |
| ![smile](./Images/Flower.jpg)  | Display the help window. |
| ![smile](./Images/Flower.jpg)  | _Closes_ a window        |
| ![smile](./Image/Flower.jpg)    | ![smile](./Image/Flower.jpg)   |

## Image in alert
> [!NOTE] 
>  Text before Image 
> ![I am flower](./Images/flower.jpg "This is Alt text")
>  Text after Image

## Image in alert no alt message
> [!WARNING] 
> ![I am flower](./Images/flower.jpg)
>  Text after Image

## image in alert with no title
> [!TIP] 
>  Text before Image 
> ![](./Images/flower.jpg "This is Alt text")


## Image in alert no alt message
> [!WARNING] 
> ![I am flower][111]
> [111]:(./Images/flower.jpg)
>  Text after Image

## External image
> [!IMPORTANT] 
> External image
> <img src="http://pic33.nipic.com/20130916/3420027_192919547000_2.jpg" width = "860" height ="645"/>

## Invalid source
> [!CAUTION] 
> invalid Image source
> ![flower](./Images/flowers.jpg)

## Image in list
1. aaa
	* bbb
	* eee
	*  ![Flower](./Images/flower.jpg)
    
    
## image in list
1.  ![Flower](./Images/flower.jpg)
2. aaaa
4. cccc

### Error image in list
1. ![Flower](/Images/flower.jpg)
2. bbb


[comment]: <> (![INT access rules (inbound)](./Images/flower.png))
[comment]: <> (![INT access rules (outbound)](./Images/flowerssss.png))


