# Code Snippet Test Cases

## Missing code samples

### Empty code sample 01
 ```

 ```

### Empty code sample 02
 ```c

 ```

### Empty code sample 03
I'm a empty code sample ` `

### Empty code sample 04
[!Code-c[snippetGetActions_empty](.\CodeSnippets\emptycode)]

### Empty azure code sample
```azurecli-interactive

``` 

## Missing language specification

### Empty language specification

#### Empty language specification 01
```
  
```

#### Empty language specification 02
[!Code-[snippetGetActions_fsharp](.\CodeSnippets\code_test_fsharp.fs)] 

#### Empty language specification 03
[!Code[snippetGetActions_fsharp](.\CodeSnippets\code_test_fsharp.fs)]

### Invalid language specification

#### Invalid language specification 01
[!Code-|[snippetGetActions_fsharp](.\CodeSnippets\code_test_fsharp.fs)]

#### Invalid language specification 02
```|
  hello world
```

## Normal code snippet

### Write code directly
  ```c#
  using system
  public class Demo
	{
	 public void Main()
	 {
	        Console.WriteLine("Hello World");
	 }
	}
  ```

### Code file reference
[!Code-fsharp[snippetGetActions_fsharp](.\CodeSnippets\code_test_fsharp.fs)] 

### Code sample as text
I am text with `code sample` example.

### Azurecli Code sample
```azurecli-interactive
az group create --name myResourceGroup --location westeurope
``` 

## Nested code snippet
### Nested into Alert
#### Nested into Alert 01
[!NOTE]
>```
>
>```
#### Nested into Alert 02
[!NOTE]
>I'm a  sample code ` ` nested into NOTE alert

### Nested into list
1. This is list.
2. 
>``` 
 
>```
3. I'm a  sample code ` ` nested into list

### Nested into table
| [!Code-[snippetGetActions_fsharp](./CodeSnippets/code_test_fsharp.fs)]  | [!Code-|[snippetGetActions_fsharp](./CodeSnippets/code_test_fsharp.fs)]       |
| ------------- | ----------- |
|  ``` ```   |  ` `|

### Empty code 01
 
 ```

 ```

### Normal code snippet with scrollbar
  ```
  On July 2, an alien ship entered Earth's orbit and deployed several dozen saucer-shaped "destroyer" spacecraft, each 15 miles (24 km) wide.
  
  On July 3, the Black Knights, a squadron of Marine Corps F/A-18 Hornets, participated 
  ```
  
  ### Normal code snippet with no scrollbar
  ```
  On July 2
  ```
  
  
### Normal code snippet in alert (only code)
>[!NOTE]
>``` 
>On July 2 
>```
 

>### Import internal code "code_test_f#" 
>[!NOTE]
>[!Codefsharp[snippetGetActions_fsharp](./CodeSnippets/code_test_fsharp.fs)] 
 
>### Import sample code 
>[!NOTE]
>1. 
>``` 
>On July 2 On July 2 On July 2 On July 2 On July 2 On July 2 On July 2 On July 2 On July 2 On July 2 On July 2 On July 2 On July 2 On July 2 On July 2 On July 2 On July 2 On July 2 On July 2 On July 2 On July 2 On July 2 On July 2 On July 2 On July 2 On July 2 On July 2 On July 2 
>``` 
 
>### Import internal code "code_test_f#" 
>1. [!Code-fsharp[snippetGetActions_fsharp](.\CodeSnippets\code_test_fsharp.fs)] 


>### Import internal code "code_test_f#" 
| [!Code-fsharp[snippetGetActions_fsharp](./CodeSnippets/code_test_fsharp.fs)]  | [!Code-fsharp[snippetGetActions_fsharp](./CodeSnippets/code_test_fsharp.fs)]       |
| ------------- | ----------- |
| [!Code-fsharp[snippetGetActions_fsharp](./CodeSnippets/code_test_fsharp.fs)]    | [!Code-fsharp[snippetGetActions_fsharp](./CodeSnippets/code_test_fsharp.fs)] |

>### Import sample code "code_test_f#" 
| [!Code-c[snippetGetActions_empty](.\CodeSnippets\emptycode)]  | [!Code-fsharp[snippetGetActions_fsharp](.\CodeSnippets\code_test_fsharp.fs)]       |
| ------------- | ----------- |
| [!Code-c[snippetGetActions_c02](./CodeSnippets/test_code_c-02.c)]      | [!Code-c[snippetGetActions_c01](.\CodeSnippets\test_code_c-01.c)] |

### Normal code snippet in list
>1. This is list.
>2. 
>``` 
>On July 2 
>```

### code snippet in list(long code)
>1. This is list.
>2. 
>``` 
>On July 2 On July 2 On July 2 On July 2 On July 2 On July 2 On July 2 On July 2 On July 2 On July 2 On July 2 On July 2 On July 2 On July 2 On July 2 On July 2 On July 2 On July 2 On July 2 On July 2 On July 2 On July 2 On July 2 On July 2 On July 2 On July 2 On July 2 On July 2 
>```

### Normal code snippet in list(Only code)
>1. 
>``` 
>On July 2 
>```

### Normal code snippet in list(Only code and long code)
>1. 
>``` 
>On July 2 On July 2On July 2On July 2On July 2On July 2On July 2On July 2On July 2On July 2On July 2On July 2On July 2On July 2On July 2On July 2On July 2On July 2On July 2On July 2On July 2On July 2On July 2On July 2
>```

### Normal code snippet in table(Only code)
>||
>|----|
>|```
>On July 2 
>```|

### Normal code snippet in table(Only code and long code)
>||
>|----|
>|```
>On July 2 On July 2 On July 2 On July 2 On July 2 On July 2 On July 2 On July 2 On July 2 On July 2 On July 2 On July 2 On July 2 On July 2 On July 2 On July 2 On July 2 On July 2 On July 2 On July 2 On July 2 On July 2 
>```|
  

### Normal code snippet with no scrollbar  
```  
On July 2 
```
  
  ### Normal code snippet with name
  ```c#
  using system
  public class Demo
	{
	 public void Main()
	 {
	        Console.WriteLine("Hello World");
	 }
	}
  ```
  
### Verify codesnippet width is not wider than article body(pending)
   ```
  On July 2, an alien ship entered Earth's orbit and deployed several dozen saucer-shaped "destroyer" spacecraft, each 15 miles (24 km) wide.
  
  On July 3, the Black Knights, a squadron of Marine Corps F/A-18 Hornets, participated 
  ```
 
 
### Verify codesnippet content is not missing01
   ```
  ```
  
### Verify codesnippet content is not missing02(contain \n)
  ```
  
  ```
 
### Verify codesnippet content is not missing03(contain \n \n)
  ```
  
  
  ```
  
### Verify codesnippet contain continual text 01
 ```
dfihsdiofnsdfoiweafsdnfisdofjdsiofnsdiofjsdiofndsfouisdhfndsjnfdsohfnsdijfndjnfdjfndjfdhnfjdbnjfuehbfdjs;dfjdnsdibn939knfoeifeknndsjkfneinf/////////////ndfdfsdfdfmk\\\\\\\\dkmnfddlmfkdfdmjfkddllddddddd//ssssssss\\dddddddddddddddddddddsssssssssssssssssssssssssssssssssssssssssssssss
 ```
 
### Verify codesnippet contain continual text 02
 ```
dfihsdiofnsdfoiweafsdnfisdofjdsiofnsdiofjsdiofndsfouisdhfndsjnfdsohfnsdijfndjnfdjfndjfdhnfjdbnjfuehbfdjsdfjdnsdibn939knfoeifeknndsjkfnssssspoinknjdnjdbhbsbjsbdjsadbfjfxzcnsdjskjdjfknfdjncndfjdnjdnjdncnnnnnnjsdfudhcvdvss
 ```
 
 >### Import internal code "code_test_f#"
 >[!Code-fsharp[snippetGetActions_fsharp](.\CodeSnippets\code_test_fsharp.fs)]
 
 >### Import internal code "code_test_java"
 >[!Code-java[snippetGetActions_java](.\CodeSnippets\code_test_java.java)]
 
 >### Import internal code "code_test_c-01"
 >[!Code-c[snippetGetActions_c01](.\CodeSnippets\test_code_c-01.c)]
  
 >### Import internal code "code_test_c-02"
 >[!Code-c[snippetGetActions_c02](./CodeSnippets/test_code_c-02.c)]  
 
 >### Import internal emptycode
 >[!Code-c[snippetGetActions_empty](.\CodeSnippets\emptycode)]
 

