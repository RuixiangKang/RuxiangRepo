
<head>
<script type="text/javascript">

function MM_jumpMenu(targ,selObj,restore){ //v3.0
  eval(targ+".location='"+selObj.options[selObj.selectedIndex].value+"'");
  if (restore) selObj.selectedIndex=0;
}
</script>
</head>

<body>
<select>
<option value="">-- Select Value --</option>
<option value="http://www.w3school.com.cn/tags/tag_option.asp">ATA Deployment Guide </option>
</select>



<form action="" method="get">
    <label>1、普通下拉列表菜单</label>
    <select name="">
        <option value="0">divcss5</option>
        <option value="1">DIVCSS5</option>
    </select><br />

    <label>2、跳转的下拉列表菜单</label>
    <select name="jumpMenu" id="jumpMenu" onchange="MM_jumpMenu('parent',this,0)">
        <option value="http://www.divcss5.com/">divcss5</option>
        <option value="http://www.divcss5.com/">DIVCSS5</option>
    </select>
</form>


    <label>1、普通下拉列表菜单</label>
    <select name="">
        <option value="0">divcss5</option>
        <option value="1">DIVCSS5</option>
    </select><br />

    <label>2、跳转的下拉列表菜单</label>
    <select name="jumpMenu" id="jumpMenu" onchange="MM_jumpMenu('parent',this,0)">
        <option value="http://www.divcss5.com/">divcss5</option>
        <option value="http://www.divcss5.com/">DIVCSS5</option>
    </select>
    </body>
