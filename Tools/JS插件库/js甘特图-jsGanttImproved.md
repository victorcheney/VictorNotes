## 用法 ##

### 建立最基本的甘特图 ###

引入JSGantt CSS 和 Javascript 文件

    <link href="jsgantt.css" rel="stylesheet" type="text/css"/>
    <script src="jsgantt.js" type="text/javascript"></script>

创建一个div元素来放置甘特图

    <div style="position:relative" class="gantt" id="GanttChartDIV"></div>

开始 `<script>` 块    

    <script type="text/javascript">


使用GanttChart()方法实例化JSGantt

    var g = new JSGantt.GanttChart(document.getElementById('GanttChartDIV'), 'day');

方法定义: `GanttChart(pDiv, pFormat)`

<style>
	table{border-collapse:collapse;}
	table th{background-color: #ddd}
	table td{text-align:left;font-size: 13px}
</style>
	
| 参数     |	描述|
| ------------- |:-------------:|
|pDiv:	        |（必需）这是一个在HTML中创建的DIV对象
|pFormat:		|（必填）给甘特图指定一种视图格式，如以“hour”，“day”，“week”，“month”或“quarter”格式绘制

### 使用配置方法自定义外观 ###

看下方配置选项

### 添加任务 ###

**a) 使用AddTaskItem()方法**

    g.AddTaskItem(new JSGantt.TaskItem(1, 'Define Chart API','',  '',  'ggroupblack','', 0, 'Brian', 0,  1,0,1,'','','Some Notes text',g));
    g.AddTaskItem(new JSGantt.TaskItem(11,'Chart Object','2014-02-20','2014-02-20','gmilestone', '', 1, 'Shlomy',100,0,1,1,'','','',g));

方法定义: TaskItem(**pID, pName, pStart, pEnd, pClass, pLink, pMile, pRes, pComp, pGroup, pParent, pOpen, pDepend, pCaption, pNotes, pGantt**)

|参数	|描述|
|-----------|:----------:|
|pID:	    |(必需)    用于标识每一行的唯一数字ID
|pName:	    |(必需)    任务标签名
|pStart:	|(必需)    任务开始日期，可以为组输入空日期（''），还可以输入具体时间（2016-02-20 12:00）以获得更多精度。
|pEnd:	    |(必需)    任务结束日期，可以为组输入空日期（''）
|pClass:	|(必需)    任务的css类(任务标签的样式类，如：'ggroupblack','gtaskblue','gtaskgreen'等)
|pLink:	    |(可选的)  任何http链接在工具提示中显示为“更多信息”链接。
|pMile:	    |(可选的)  指出这是否是一个里程碑式的任务 - 数字:1 表示 里程碑任务，0 表示 不是里程碑任务
|pRes:	    |(可选的)  资源名称
|pComp:	    |(必需)    完成百分比，数字
|pGroup:	|(可选的)  指示这是否是组任务（父） - 数字： 0 =正常任务，1 =标准组任务，2 =组合任务*
|pParent:	|(必需)    已存在的任务的pID，这表示此任务成为已识别任务的子任务。 数字顶级任务应将pParent设置为0
|pOpen:	    |(必需)    指示在绘制图表时是否打开标准组任务。 必须为所有项目设置值，但只能由标准组任务使用。 数字，1 =展开开，0 =收起
|pDepend:	|(可选的)  逗号分隔这个任务依赖的id列表。 将从每个列出的任务中画一条箭头到该项任务;每个id可以选择后跟一个依赖关系类型后缀。 有效值是:'FS' - 完成到开始（如果省略后缀，则为默认值）;'SF' - 开始到完成;'SS' - 开始启动;'FF' - 完成到完成； 如果存在，则后缀必须直接添加到id中。'123SS'
|pCaption:	|(可选的)  如果CaptionType设置为“Caption”，将在任务栏后面添加标题
|pNotes:	|(可选的)  将在此任务的工具提示中显示详细的任务信息
|pGantt:	|(必需)    javascript JSGantt.GanttChart对象从中进行设置。 默认为“g”用于向后兼容
* 联合组的任务显示在一个行中的所有子任务。任务列表和行标题中显示的信息取自父任务。根据自己的信息为每个子任务单独生成工具提示。**里程碑作为组合组任务的子任务无效，不会显示**。不执行检查子任务的开始和结束日期，因此这些任务栏可能重叠。依赖关系只能设置为子任务。

**b) 使用parseXML()与外部XML文件**

    JSGantt.parseXML("project.xml",g);
方法定义: JSGantt.parseXML(pFile, pGanttObj)

|参数	|描述|
|----------:|-----------|
|pFile:	    |（必需）XML的文件名
|pGanttObj:	|（必需）通过调用JSGantt.GanttChart() 返回的GanttChart对象
XML文件的结构：

    <project>
    <task>
    	<pID>25</pID>
    	<pName>WCF Changes</pName>
    	<pStart></pStart>
    	<pEnd></pEnd>
    	<pClass>gtaskred</pClass>
    	<pLink></pLink>
    	<pMile>0</pMile>
    	<pRes></pRes>
    	<pComp>0</pComp>
    	<pGroup>1</pGroup>
    	<pParent>2</pParent>
    	<pOpen>1</pOpen>
    	<pDepend>2,24</pDepend>
    	<pCaption>A caption</pCaption>
    	<pNotes>Text - can include limited HTML</pNotes>
    </task>
    </project>
字段定义如上面对TaskItem的参数所述。pClass元素在XML文件中是可选的，并且对于组任务将默认为“ggroupblack”，对于正常任务将默认为“gtaskblue”，以及用于里程碑的“gmilestone”。XML导入不需要pGantt元素。

JSGannt Improved还将测试提供的XML文件，看看它是否显示为Microsoft Project XML格式。如果是这样，将尝试加载项目。这个功能是实验性的，导入是尽力而不保证的。一旦加载，JSGantt Improved解释的项目可以使用提供的XML导出方法来提取。

**c) using parseXMLString() with XML held in a javascript string object**

    JSGantt.parseXMLString("<project><task>...</task></project>",g);
方法定义: JSGantt.parseXML(pStr, pGanttObj)

|参数	 |描述|
|-----------:|-----------|
|pStr:	|（必需）这是一个包含XML的JavaScript字符串
|pGanttObj:	|（必需）通过调用JSGantt.GanttChart() 返回的GanttChart对象
所提供的XML将以与外部XML文件的内容完全相同的方式进行解析，因此必须与上述parseXML所述的格式相匹配
### 调用Draw()方法 ###

    g.Draw();
###  script代码块`</script>`闭合标签 ### 

    </script>
### 更新已经存在的甘特图 ###

也可以使用RemoveTaskItem() 方法删除任务。

    g.RemoveTaskItem(11);
方法定义: RemoveTaskItem(pID)

|参数	|描述|
|-----------|:---------:|
|pID:	|（必需）要删除的项目的唯一数字ID
如果删除的任务是一个组项目，则所有子任务也将被删除。

添加或删除任务后，必须调用“g.Draw()”才能重绘图表。
## 选项 ##

### 开关方法 ###

jsGanttImproved的许多功能可以通过使用在调用JSGantt.GanttChart() 返回的GanttChart对象上可用的setter方法进行自定义。以下选项采用单个数字参数: 1表示启用描述功能，0表示禁用

|方法			   				|描述
|-------------------------------|:-----------------------------------------------------------:|
|setUseToolTip():				|控制**工具提示框**的显示，默认为1（已启用）
|setUseFade():					|控制在显示/隐藏**工具提示时使用淡入淡出效果**，默认为1（启用）
|setUseMove():					|控制在不同任务工具提示之间**切换**时使用滑动效果，默认为1（启用）
|setUseRowHlt():				|控制使用行**鼠标悬停**突出显示，默认为1（启用）
|setUseSort():					|控制任务列表是否被排序到父任务/开始时间顺序中，或者按照创建的顺序显示，默认为1（排序启用）
|setShowRes():					|控制**资源列**是否显示在任务列表中，默认为1（显示列）
|setShowDur():					|控制**“任务持续时间”**列是否显示在任务列表中，默认为1（显示列）
|setShowComp():					|控制“**完成百分比**”列是否显示在任务列表中，默认为1（显示列）
|setShowStartDate():			|控制**任务开始日期列**是否显示在任务列表中，默认为1（显示列）
|setShowEndDate():				|控制**任务结束日期列**是否显示在任务列表中，默认为1（显示列）
|setShowTaskInfoRes():			|控制**资源信息**是否显示在任务工具提示中，默认为1（显示信息）
|setShowTaskInfoDur():			|控制**任务持续时间信息**是否显示在任务工具提示中，默认为1（显示信息）
|setShowTaskInfoComp():			|控制**“完成百分比”信息**是否显示在任务工具提示中，默认为1（显示信息）
|setShowTaskInfoStartDate():	|控制**任务开始日期信息**是否显示在任务工具提示中，默认为1（显示信息）
|setShowTaskInfoEndDate():		|控制**任务结束日期信息**是否显示在任务工具提示中，默认为1（显示信息）
|setShowTaskInfoLink():			|控制**更多信息链接**是否显示在任务工具提示中，默认为0（请勿显示链接）
|setShowTaskInfoNotes():		|控制是否在任务工具提示中显示附加**备注数据**，默认为1（显示注释）
|setShowEndWeekDate():			|控制“日”视图中的主要标题是否以适当的格式显示周结束日期（见下文），默认为1（显示日期）
|setShowDeps():					|控制依赖行的显示，默认为1（显示依赖关系）
### 键值 ###

以下选项可使用一组特定键值启用功能

|方法			   				|描述
|-------------------------------|:-----------------------------------------------------------:|
|setShowSelector():	|控制显示格式选择器的位置，接受多个参数。有效参数值为“Top”，“Bottom”。默认为“top”。
|setFormatArr():	|控制格式选择器中显示的格式选项，可以接受多个参数。有效参数值为“Hour”，“Day”，“Week”，“Month”，“Quarter”。默认为所有有效值。
|setCaptionType():	|控制用于甘特图任务栏上的标题的任务字段，接受单个参数。有效参数值为“None”，“Caption”，“Resource”，“Duration”，“Complete”。默认为“None”
|setDateInputFormat():	|定义在任务创建中用于日期的输入格式，接受单个参数。有效参数值为“yyyy-mm-dd”，“dd / mm / yyyy”，“mm / dd / yyyy”。默认为“yyyy-mm-dd”
|setScrollTo():	|设置甘特图将滚动到的日期，以上面的setDateInputFormat（）设置的日期输入格式指定。也接受特殊值“today”。默认为最小显示日期
|setUseSingleCell():	|设置任务列表将使用每个行的单个表单元格而不是每个周期的一个单元格的单元格的阈值总数。有助于提高大图表的性能。数值为0，禁用此功能（始终使用多个单元格），默认为25000
|setLang():	|设置绘图时使用的语言。默认为“en”，插件中提供的唯一语言（有关如何添加更多语言的详细信息，请参阅下面的国际化）。
### 布局 ###

甘特图的大部分外观和感觉可以使用CSS进行控制，因为任务栏的长度由列宽决定，以下方法采用一个单一的数字参数来定义适当的列宽（以像素为单位）。请注意，任务栏大小调整代码假定使用1px宽的折叠表格边框。

|Method			   				|描述
|-------------------------------|:-----------------------------------------------------------:|
|setHourColWidth():	|以“小时”格式绘制时，甘特图的宽度以像素为单位。默认为18。
|setDayColWidth():	|当“日”格式绘制时，甘特图的宽度以像素为单位。默认为18。
|setWeekColWidth():	|当“周”格式绘制时，甘特图的宽度以像素为单位。默认为36。
|setMonthColWidth():	|以“月”格式绘制时，甘特图宽度以像素为单位。默认为36
|setQuarterColWidth():	|甘特图宽度以像素为单位，以“Quarter(季度)”格式绘制，虽然不是强制性的，但建议将其设置为可被3整除的值。默认值为18。
|setRowHeight():	|甘特图行高单位像素。用于在靠近端点的地方路由依赖线。默认为20。
|setMinGpLen():	|组任务的任务栏装端点，此值指定这些端点的宽度（以像素为单位）。一个简短的任务栏的长度将被四舍五入，以正确显示单个或两个端点。默认为8。
### 日期格式化 ###

日期显示格式可以单独控制。用于设置这些显示格式的方法各采用一个格式的字符串参数。格式字符串可以由以下组件组成（区分大小写）

|部件	|描述|
|-------------------------------|:-----------------------------------------------------------:|
|h	|小时 (1-12)
|hh	|小时 (01-12)
|pm	|am/pm 上午/下午指示符
|PM	|AM/PM 上午/下午指示符
|H	|24小时制 (0-23)
|HH	|24小时制 (01-23)
|mi	|分钟 (1-59)
|MI	|分钟 (01-59)
|d	|日 (1-31)
|dd	|日 (01-31)
|day	|英文星期的缩写
|DAY	|星期几
|m	|月 (1-12)
|mm	|月 (01-12)
|mon	|英文缩写的月份
|month	|月份全拼
|yy	|年，不包括世纪
|yyyy	|年
|q	|季度 (1-4)
|qq	|Quarter (Q1-Q4)
|w	|ISO周数 (1-53)
|ww	|ISO周数 (01-53)
|week	|完整的ISO周日期格式

由以下字符之一分隔：“/ - 。，' <space>：

使用不符合上述组件之一的分隔符之间的任何文本将使用有效的国际化字符串的不区分大小写匹配来检查（请参见下面的国际化）。如果仍然找不到该值，文本将不会被输出。

可用的日期显示方法是

|方法	|描述
|---------------------------------------|:-----------------------------------------------------------:|
|setDateTaskTableDisplayFormat():		|在**主任务列表中用于开始和结束日期的日期格式**。默认为“dd / mm / yyyy”。
|setDateTaskDisplayFormat():			|在**任务工具提示中用于开始和结束日期的日期格式**。默认为'dd month yyyy'。
|setHourMajorDateDisplayFormat()		|用于甘特图**主要日期标题**的日期格式以“**小时**”格式显示。默认为“日dd月yyyy”。
|setDayMajorDateDisplayFormat():		|用于甘特图**主要日期标题**的日期格式以“**日**”格式显示。默认为“dd / mm / yyyy”。
|setWeekMajorDateDisplayFormat():		|用于甘特图**主要日期标题**的日期格式以“**周**”格式显示。默认为'yyyy'。
|setMonthMajorDateDisplayFormat():		|用于甘特图**主要日期标题**的日期格式以“**月**”格式显示。默认为'yyyy'。
|setQuarterMajorDateDisplayFormat():	|用于甘特图**主要日期标题**的日期格式以“**年**”格式显示。默认为'yyyy'。
|setHourMinorDateDisplayFormat()		|甘特图使用的日期格式“**小时**”格式显示的**小日期标题**。默认为“HH”。
|setDayMinorDateDisplayFormat():		|用于甘特图的日期格式“**日**”格式显示的**小日期标题**。默认为'dd'。
|setWeekMinjorDateDisplayFormat():		|甘特图使用的日期格式以“**周**”格式显示的**小日期标题**。默认为“dd / mm”。
|setMonthMinorDateDisplayFormat():		|甘特图使用的日期格式以“**月**”格式显示。默认为“mon”。
|setQuarterMinorDateDisplayFormat():	|甘特图用于“**年**”格式显示的日期标题的日期格式。默认为'qq'。
### I18N 国际化 ###

jsGanttImproved仅提供英文文本，但是所有硬编码字符串都可以通过调用JSGantt.GanttChart（）返回的GanttChart对象上可用的addLang（）方法来替代，

`addLang（）`方法有两个参数。第一个是语言的字符串标识符，第二个是包含所有替换文本对的javascript对象，默认英文设置为：

|键值		|显示文本	|	对应中文	|键值		|显示文本 			|对应中文	|键值		|显示文本	| 对应中文	|
|-----------|-----------|-----------|:----------|-------------------|-----------|-----------|-----------|----------:|
|january	|January	|01			|sunday		|Sunday				|星期日		|format		|Format		|视图		|
|february	|February	|02			|monday		|Monday				|星期一		|hour		|Hour		|时			|
|march		|March		|03			|tuesday	|Tuesday			|星期二		|day		|Day		|日			|
|april		|April		|04			|wednesday	|Wednesday			|星期三		|week		|Week		|周			|
|maylong	|May		|05			|thursday	|Thursday			|星期四		|month		|Month		|月			|
|june		|June		|06			|friday		|Friday				|星期五		|quarter	|Quarter	|季			|
|july		|July		|07			|saturday	|Saturday			|星期六		|hours		|Hours		|小时		|
|august		|August		|08			|sun		|Sun				|星期日		|days		|Days		|天			|
|september	|September	|09			|mon		|Mon				|星期一		|weeks		|Weeks		|周			|
|october	|October	|10			|tue		|Tue				|星期二		|months		|Months		|月			|
|november	|November	|11			|wed		|Wed				|星期三		|quarters	|Quarters	|季度		|
|december	|December	|12			|thu		|Thu				|星期四		|hr			|Hr			|小时		|
|jan		|Jan		|1月			|fri		|Fri				|星期五		|dy			|Day		|天			|
|feb		|Feb		|2月			|sat		|Sat				|星期六		|wk			|Wk			|周			|
|mar		|Mar		|3月			|resource	|Resource			|资源		|mth		|Mth		|月			|
|apr		|Apr		|4月			|duration	|Duration			|耗时		|qtr		|Qtr		|季度		|
|may		|May		|5月			|comp		|%Comp.				|完成度		|hrs		|Hrs		|小时		|
|jun		|Jun		|6月			|completion	|Completion			|完成度		|dys		|Days		|天			|
|jul		|Jul		|7月			|startdate	|Start Date			|开始日期	|wks		|Wks		|周			|
|aug		|Aug		|8月			|enddateEnd |Date				|结束日期	|mths		|Mths		|月			|
|sep		|Sep		|9月			|moreinfo	|More Information	|更多信息	|qtrs		|Qtrs		|季度		|
|oct		|Oct		|10月		|notes		|Notes				|备注 		| 			| 			|			|
|nov		|Nov		|11月 		| 			| 					|			|			| 			| 				
|dec		|Dec		|12月 		| 			| 					|			|			| 			| 			
添加语言时，未提供的任何翻译将使用默认的英语语言值。这提供了一种覆盖默认字符串的简单方法，例如

    g.addLang('en2', {'format':'Select', 'comp':'Complete'});
将创建一个名为“en2”的语言，其中格式选择器中的文本为“Select”而不是“Format”，任务列表中“Percentage Complete”列的标题为“Complete”而不是“％Comp”。

一旦添加了一个另一种语言，在调用Draw()之前，必须使用适当的语言标识符对setLang()进行调用。

**中文国际化**

    g.addLang('zh',
        {'format':'视图','hour':'时','day':'日','week':'周','month':'月','quarter':'季','hours':'小时','days':'天',
        'weeks':'周','months':'月','quarters':'季度','hr':'小时','dy':'天','wk':'周','mth':'月','qtr':'季度','hrs':'小时',
        'dys':'天','wks':'周','mths':'月','qtrs':'季度','resource':'资源','duration':'耗时','comp':'完成度',
        'completion':'完成度','startdate':'开始日期','enddate':'结束日期','moreinfo':'更多信息','notes':'备注',
        'january':'01','february':'02','march':'03','april':'04','maylong':'05','june':'06','july':'07',
        'august':'08','september':'09','october':'10','november':'11','december':'12','jan':'1月',
        'feb':'2月','mar':'3月','apr':'4月','may':'5月','jun':'6月','jul':'7月','aug':'8月','sep':'9月','oct':'10月','nov':'11月',
        'dec':'12月','sunday':'星期日','monday':'星期一','tuesday':'星期二','wednesday':'星期三','thursday':'星期四',
        'friday':'星期五','saturday':'星期六','sun':'星期日','mon':'星期一','tue':'星期二','wed':'星期三','thu':'星期四','fri':'星期五','sat':'星期六'});
    g.setLang('zh');

### 选项示例 ###

提供的示例索引文件中使用的配置选项有：

    g.setCaptionType('Complete');  							 // 设置为显示标题（无，标题，资源，持续时间，完成）
    g.setQuarterColWidth(36);
    g.setDateTaskDisplayFormat('day dd month yyyy');		 // 显示在工具提示框中
    g.setDayMajorDateDisplayFormat('mon yyyy - Week ww'); 	 // 设置格式以在“日”视图的“主要”标题中显示日期
    g.setWeekMinorDateDisplayFormat('dd mon');				 // 设置格式以在“周”视图的“次要”标题中显示日期
    g.setShowTaskInfoLink(1); 								 //显示工具提示链接（0/1）
    g.setShowEndWeekDate(0); 								 // 显示/隐藏每日视图（1/0）
    g.setUseSingleCell(1); 									 // 每个表行使用一个单元格。有助于大型图表的呈现性能。
    g.setUseSingleCell(10000); 								 // 设置每个表行仅使用一个单元格的阈值（0个禁用）。 有助于大型图表的呈现性能。
    g.setFormatArr('Day', 'Week', 'Month', 'Quarter');    	 // 设置视图显示格式
将所有这些信息放在一起，产生示例索引文件中包含的图表的最终代码如下：

    <link href="jsgantt.css" rel="stylesheet" type="text/css"/>
    <script src="jsgantt.js" type="text/javascript"></script>
    
    <div style="position:relative" class="gantt" id="GanttChartDIV"></div>

    <script>
    
    var g = new JSGantt.GanttChart(document.getElementById('GanttChartDIV'), 'day');
    
    if( g.getDivId() != null ) {
	    g.setCaptionType('Complete');
	    g.setQuarterColWidth(36);
	    g.setDateTaskDisplayFormat('day dd month yyyy');
	    g.setDayMajorDateDisplayFormat('mon yyyy - Week ww');
	    g.setWeekMinorDateDisplayFormat('dd mon');
	    g.setShowTaskInfoLink(1);
	    g.setShowEndWeekDate(0);
	    g.setUseSingleCell(10000);
	    g.setFormatArr('Day', 'Week', 'Month', 'Quarter');
	    
	    g.AddTaskItem(new JSGantt.TaskItem(1,   'Define Chart API', '',   '',  'ggroupblack',  '',   0, 'Brian',0,   1, 0,  1, '',  '',  'Some Notes text', g));
	    g.AddTaskItem(new JSGantt.TaskItem(11,  'Chart Object', '2014-02-20','2014-02-20', 'gmilestone',   '',   1, 'Shlomy',   100, 0, 1,  1, '',  '',  '', g));
	    g.AddTaskItem(new JSGantt.TaskItem(12,  'Task Objects', '',   '',  'ggroupblack',  '',   0, 'Shlomy',   40,  1, 1,  1, '',  '',  '', g));
	    g.AddTaskItem(new JSGantt.TaskItem(121, 'Constructor Proc', '2014-02-21','2014-03-09', 'gtaskblue','',   0, 'Brian T.', 60,  0, 12, 1, '',  '',  '', g));
	    g.AddTaskItem(new JSGantt.TaskItem(122, 'Task Variables',   '2014-03-06','2014-03-11', 'gtaskred', '',   0, 'Brian',60,  0, 12, 1, 121, '',  '', g));
	    g.AddTaskItem(new JSGantt.TaskItem(123, 'Task by Minute/Hour',  '2014-03-09','2014-03-14 12:00', 'gtaskyellow', '',  0, 'Ilan', 60,  0, 12, 1, '',  '',  '', g));
	    g.AddTaskItem(new JSGantt.TaskItem(124, 'Task Functions',   '2014-03-09','2014-03-29', 'gtaskred', '',   0, 'Anyone',   60,  0, 12, 1, '123SS', 'This is a caption', null, g));
	    g.AddTaskItem(new JSGantt.TaskItem(2,   'Create HTML Shell','2014-03-24','2014-03-24', 'gtaskyellow',  '',   0, 'Brian',20,  0, 0,  1, 122, '',  '', g));
	    g.AddTaskItem(new JSGantt.TaskItem(3,   'Code Javascript',  '',   '',  'ggroupblack',  '',   0, 'Brian',0,   1, 0,  1, '',  '',  '', g));
	    g.AddTaskItem(new JSGantt.TaskItem(31,  'Define Variables', '2014-02-25','2014-03-17', 'gtaskpurple',  '',   0, 'Brian',30,  0, 3,  1, '',  'Caption 1', '', g));
	    g.AddTaskItem(new JSGantt.TaskItem(32,  'Calculate Chart Size', '2014-03-15','2014-03-24', 'gtaskgreen',   '',   0, 'Shlomy',   40,  0, 3,  1, '',  '',  '', g));
	    g.AddTaskItem(new JSGantt.TaskItem(33,  'Draw Task Items',  '',   '',  'ggroupblack',  '',   0, 'Someone',  40,  2, 3,  1, '',  '',  '', g));
	    g.AddTaskItem(new JSGantt.TaskItem(332, 'Task Label Table', '2014-03-06','2014-03-09', 'gtaskblue','',   0, 'Brian',60,  0, 33, 1, '',  '',  '', g));
	    g.AddTaskItem(new JSGantt.TaskItem(333, 'Task Scrolling Grid',  '2014-03-11','2014-03-20', 'gtaskblue','',   0, 'Brian',0,   0, 33, 1, '332',   '',  '', g));
	    g.AddTaskItem(new JSGantt.TaskItem(34,  'Draw Task Bars',   '',   '',  'ggroupblack',  '',   0, 'Anybody',  60,  1, 3,  0, '',  '',  '', g));
	    g.AddTaskItem(new JSGantt.TaskItem(341, 'Loop each Task',   '2014-03-26','2014-04-11', 'gtaskred', '',   0, 'Brian',60,  0, 34, 1, '',  '',  '', g));
	    g.AddTaskItem(new JSGantt.TaskItem(342, 'Calculate Start/Stop', '2014-04-12','2014-05-18', 'gtaskpink','',   0, 'Brian',60,  0, 34, 1, '',  '',  '', g));
	    g.AddTaskItem(new JSGantt.TaskItem(343, 'Draw Task Div','2014-05-13','2014-05-17', 'gtaskred', '',   0, 'Brian',60,  0, 34, 1, '',  '',  '', g));
	    g.AddTaskItem(new JSGantt.TaskItem(344, 'Draw Completion Div',  '2014-05-17','2014-06-04', 'gtaskred', '',   0, 'Brian',60,  0, 34, 1, "342,343",'', '', g));
	    g.AddTaskItem(new JSGantt.TaskItem(35,  'Make Updates', '2014-07-17','2015-09-04', 'gtaskpurple',  '',   0, 'Brian',30,  0, 3,  1, '333',   '',  '', g));
	    
	    g.Draw();
    }
    else
    {
    	alert("Error, unable to create Gantt Chart");
    }
    
    </script>
### XML Export ###

可以使用以下方法以XML格式提取项目中任务的详细信息

方法定义: **getXMLProject()**

返回包含JSGantt改进的XML格式的整个项目的字符串。日期将以当前定义的输入格式导出，由`setDateInputFormat()`设置。

定义方法: **getXMLTask(pID, pIdx)**

|参数  		|描述|
|-----------|:----------:|
|pID:		|（必需）标识要提取的任务的数字ID
|pIdx:		|（可选）Boolean - 如果存在并设置为“true”，则pID参数中传递的数字将被视为任务列表的数组索引，而不是ID
以JSGantt Improved XML格式返回包含指定任务项的字符串。日期将以当前定义的输入格式导出，由setDateInputFormat()设置。

