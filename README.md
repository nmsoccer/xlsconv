# xlsconv
A simple Xls Conv Tool 一个简单execl转换工具  

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

### 举例说明:
#### json:
* 执行 `python xlsconv.py -F json  -I xls/物品表.xlsx`
  ```
  open xls/▒▒Ʒ▒▒.xlsx succcess!
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
      {"id":10405, "name":"双手剑", "price":400, "type":104, "pos":[], "can_sell":0},
      {"id":10506, "name":"靴子", "price":500, "type":105, "pos":[3, 7], "can_sell":0}
    ]
  }
  }
  ```
  
#### xml:
* 执行 `python xlsconv.py -F xml  -I xls/物品表.xlsx`
  ```
  open xls/▒▒Ʒ▒▒.xlsx succcess!
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
