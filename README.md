# xlsconv
A Simple xls Conv Tool 一个简单execl转换工具  

### 支持功能：  
* to Json： 转换为json格式文件
* to xml:   转换为xml格式文件
* to be continue: 可以简单扩充功能为转换为其他格式

### 安装:
* 依赖: 需安装python基础应用。同时需要安装xlrd解析库
* 安装: 直接下载xlsconv.py到本地即可

### 命令选项:
* -h: 帮助命令
* -v: verbose输出详细信息
* -I: 【必须】输入的excel文件
* -O: 【可选】输出的文件名.如果不设置则默认输出到本地output.xx文件
* -F：【必须】转换格式。目前支持json xml
* example: `python xlsconv -I xls/xx.xlsx -O yy.json -F json`

### 输入文件:
* 输入文件为xls或者xlsx的excel文件，其转换的字段在xlsx内部定义
* 每个excel表格需要定义一个meta页签用于对每个其他数据页签进行说明
* meta页签按列区分，每一列定义一个数据页签  
  * 在meta列的首行定义各数据页签对应的输出表名。格式为页签名=table名  
  * 在meta每一列的其他行依次定义各数据页签表头字段  
  * 各数据页签的表头定义遵循列名=prefix+字段名的格式.prefix说明字段类型  
  * prefix:#表示数字 $表示字符串 ^表示原始文本  
  * 只有在meta中定义的数据页签才能被转出，未被定义的则不处理  
* 每一个数据页签对应一个单独的表，该表名在meta中定义  
  * 数据页签的首行定义表头 对各列名进行说明  
  * 表头对应的输出字段名在meta中定义 其中前缀表明不同的类型  
  * 数据页签其他行填入数据。如果是空白行则按照类型生成对应的默认值
  * 工具只转换在meta中定义的表头字段，如果数据页签有其他列未在meta中定义则不被转出  
  
### 输出文件:
 * JSON:
   * 生成的json默认形式为:  
   ```
   {
     "table_name":
     {
       "count":0
       "res":
       [
         {"col1":xx , "col2":xx , ...}
       ]
     }
   }
   ```
   * table_name 是数据页签的定义表名
   * count:是数据页签内数据的行数
   * res:包含所有行的数据 里面的每个{}表明每一行。每行的col字段在meta中定义
   
 * XML:
   * 生成的xml默认形式为:
   ```
   <xlsconv>
   <table_name>
     <count>0</count>
     <res>
       <entry>
         <col1>xx</col1>
         <col2>xx</col2>
       </entry>
     </res>
   </table_name>
   </xlsconv>
   ```
   * table_name 是数据页签的定义表名
   * count:是数据页签内数据的行数
   * res:包含所有行的数据 里面的每个<entry></entry>表明每一行
   * entry:是每一行的数据说明，里面的col字段名在meta中定义  

### 举例说明:
* 输入文件以xls/目录下的物品表.xlsx为例，里面包含了可能的各种情况  

#### json:
* 执行 `python xlsconv.py -F json  -I xls/物品表.xlsx`
  ```
  open input file succcess!
  parse meta finish!
  parse_sheet finish!
  parse_sheet finish!
  dump sheet 'item_timing_table'
  dump sheet 'item_table'
  ```
  
* 结果:
  ```
  cat output.json:
  {
  "item_timing_table":
  {
    "count": 3,
    "res":
    [
      {"id":10102, "expire_time":"2020-03-01 22:30:00", "show":0},
      {"id":10405, "expire_time":"2020-03-02 14:25:01", "show":0},
      {"id":10506, "expire_time":"2020-05-01 21:43:00", "show":1}
    ]
  },
  "item_table":
  {
    "count": 5,
    "res":
    [
      {"id":10102, "name":"头盔", "price":100, "type":101, "pos":[0 , 1 , 4], "can_sell":0},
      {"id":10203, "name":"帽子", "price":200, "type":102, "pos":[], "can_sell":1},
      {"id":10304, "name":"上衣", "price":300, "type":103, "pos":[2], "can_sell":0},
      {"id":0, "name":"", "price":0, "type":0, "pos":, "can_sell":0},
      {"id":10405, "name":"双手剑", "price":400, "type":104, "pos":[], "can_sell":0},
      {"id":10506, "name":"靴子", "price":500, "type":105, "pos":[3, 7], "can_sell":0}
    ]
  }
  }
  ```  
  
#### xml:
* 执行 `python xlsconv.py -F xml  -I xls/物品表.xlsx`
  ```
  open input file succcess!
  parse meta finish!
  parse_sheet finish!
  parse_sheet finish!
  dump sheet 'item_timing_table'
  dump sheet 'item_table'
  ```
  
* 结果:
  ```
  cat output.xml:
  <?xml version="1.0" encoding="UTF-8" ?>
  <xlsconv>
  <item_timing_table>
    <count>3</count>
    <res>
      <entry>
        <id>10102</id>
        <expire_time>"2020-03-01 22:30:00"</expire_time>
        <show>0</show>
      </entry>
      <entry>
        <id>10405</id>
        <expire_time>"2020-03-02 14:25:01"</expire_time>
        <show>0</show>
      </entry>
      <entry>
        <id>10506</id>
        <expire_time>"2020-05-01 21:43:00"</expire_time>
        <show>1</show>
      </entry>
    </res>
  </item_timing_table>

  <item_table>
    <count>5</count>
    <res>
      <entry>
        <id>10102</id>
        <name>"头盔"</name>
        <price>100</price>
        <type>101</type>
        <pos>[0 , 1 , 4]</pos>
        <can_sell>0</can_sell>
      </entry>
      <entry>
        <id>10203</id>
        <name>"帽子"</name>
        <price>200</price>
        <type>102</type>
        <pos>[]</pos>
        <can_sell>1</can_sell>
      </entry>
      <entry>
        <id>10304</id>
        <name>"上衣"</name>
        <price>300</price>
        <type>103</type>
        <pos>[2]</pos>
        <can_sell>0</can_sell>
      </entry>
      <entry>
        <id>0</id>
        <name>""</name>
        <price>0</price>
        <type>0</type>
        <pos></pos>
        <can_sell>0</can_sell>
      </entry>
      <entry>
        <id>10405</id>
        <name>"双手剑"</name>
        <price>400</price>
        <type>104</type>
        <pos>[]</pos>
        <can_sell>0</can_sell>
      </entry>
      <entry>
        <id>10506</id>
        <name>"靴子"</name>
        <price>500</price>
        <type>105</type>
        <pos>[3, 7]</pos>
        <can_sell>0</can_sell>
      </entry>
    </res>
  </item_table>

  </xlsconv>
  ```  
