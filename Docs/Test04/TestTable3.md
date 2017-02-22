---
# required metadata

ms.prod: .net
ms.technology: test

---

## Table and alert testing
### MD alert
> [!WARNING]
> Sample Warning Message

### XML alert
<alert class="important"> <para>Important content</para> </alert>

## MD table nest alert
| 1_Normal table | Description          |
| ------------- | ----------- |
| 1_Help      | Display the help window.|
| MD table nest MD alert    | > [!WARNING] <br> > Sample Warning Message     |
| MD table nest Xml table     | <alert class="important"> <para>Important content in table</para> </alert>     |

## XML Table nest alert
<table>
<thead>
<tr>
<th>Normal table</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>XML table with MD alert</td>
<td> 
> [!WARNING]
> Sample Warning Message
</td>
</tr>
<tr>
<td>XML table with XML alert</td>
<td>
  <alert class="important"> <para>Important content</para> </alert>
</td>
</tr>
<tr>
<td>Other way</td>
<td>
<div class="alert">
<strong>Note</strong>  Right-click to display a context menu if selection or app bar commands are not appropriate UI behaviors. But we strongly recommend that you use the app bar for all command behaviors.
</div>
</td>
</tr>
</tbody>
</table>

## Alert test
### With **
> **Note**  You don’t specify the starting point in this URI scheme. The starting point is always assumed to be the current location. If you need to specify a starting point other than the current location, see [Display directions and traffic](#display-directions-and-traffic).

### With class
<div class="alert">
<strong>Warning</strong>  Right-click to display a context menu if selection or app bar commands are not appropriate UI behaviors. But we strongly recommend that you use the app bar for all command behaviors.
</div>

                       
                    

