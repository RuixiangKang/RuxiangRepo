---
# required metadata

ms.prod: .net
ms.technology: test

---

## Table and alert testing
> [!NOTE]
> Not supported in Windows 8 and Windows 8.1 apps.

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
</tbody>
</table>

## Alert write by custumer
### With **
> **Note**  You don’t specify the starting point in this URI scheme. The starting point is always assumed to be the current location. If you need to specify a starting point other than the current location, see [Display directions and traffic](#display-directions-and-traffic).

### With class
<div class="alert">
<strong>Warning</strong>  Right-click to display a context menu if selection or app bar commands are not appropriate UI behaviors. But we strongly recommend that you use the app bar for all command behaviors.
</div>

### With class but nest in table
<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Term</th>
<th align="left">Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>Hover to learn</p></td>
<td align="left"><p>Hover over an element to display more detailed info or teaching visuals (such as a tooltip) without a commitment to an action.</p></td>
</tr>
<tr class="even">
<td align="left"><p>Left-click for primary action</p></td>
<td align="left"><p>Left-click an element to invoke its primary action (such as launching an app or executing a command).</p></td>
</tr>
<tr class="odd">
<td align="left"><p>Scroll to change view</p></td>
<td align="left"><p>Display scroll bars to move up, down, left, and right within a content area. Users can scroll by clicking scroll bars or rotating the mouse wheel. Scroll bars can indicate the location of the current view within the content area (panning with touch displays a similar UI).</p></td>
</tr>
<tr class="even">
<td align="left"><p>Right-click to select and command</p></td>
<td align="left"><p>Right-click to display the navigation bar (if available) and the app bar with global commands. Right-click an element to select it and display the app bar with contextual commands for the selected element.</p>
<div class="alert">
<strong>Note</strong>  Right-click to display a context menu if selection or app bar commands are not appropriate UI behaviors. But we strongly recommend that you use the app bar for all command behaviors.
</div>
<div>
 
</div></td>
</tr>
<tr class="odd">
<td align="left"><p>UI commands to zoom</p></td>
<td align="left"><p>Display UI commands in the app bar (such as + and -), or press Ctrl and rotate mouse wheel, to emulate pinch and stretch gestures for zooming.</p></td>
</tr>
<tr class="even">
<td align="left"><p>UI commands to rotate</p></td>
<td align="left"><p>Display UI commands in the app bar, or press Ctrl+Shift and rotate mouse wheel, to emulate the turn gesture for rotating. Rotate the device itself to rotate the entire screen.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>Left-click and drag to rearrange</p></td>
<td align="left"><p>Left-click and drag an element to move it.</p></td>
</tr>
<tr class="even">
<td align="left"><p>Left-click and drag to select text</p></td>
<td align="left"><p>Left-click within selectable text and drag to select it. Double-click to select a word.</p></td>
</tr>
</tbody>
</table>                                  
                    

