
京东云-wellcloud用户集成
1. 电话终端
京东云呼叫平台共支持三种类型的电话接入，sip/webrtc/TLS。
1.1. wellphone软电话
wellphone是一款sip软客户端， wellphone软终端用于电话呼叫。
sip服务器地址：jdcsip1.welljoint.com
UDP端口：18627
1.2. webphone软电话
webphone是一款支持webrtc协议的话机终端，无须坐席单独安装软件，集成于浏览器之中，webphone类似桌面话机或电脑安装软sip终端的功能。
Demo地址：https://rtc.welljoint.com/webrtc-1.3.2/
SIP服务器：rtc.welljoint.com/ws
注册成功状态条颜色会由黄色变为绿色，且会显示“注册成功”
《webphone集成API文档》地址：
https://www.yuque.com/wangdd/webphone
1.3. SIP电话
除了上述软终端以外，平台也支持标准SIP终端接入，支持的话机品牌及具体配置方式请咨询相关技术人员。
2. wellclient软电话集成
wellclient是软电话控制端，通常与业务系统集成，在业务系统上实现坐席的签入/签出、坐席状态控制、呼叫控制等功能。
《wellclient集成API文档》地址： 
https://www.yuque.com/books/share/6abbda7d-2580-405a-a3e1-d391187bd171
京东云平台上的JS-SDK地址：
https://jdc.welljoint.com/app/well-client.js
3. 获取平台后端接口访问token
如果需要集成本平台的后端服务，例如：调听平台录音、查询话单等，都要首先获取access token。
access token有效期为1天，过期后需要重新获取。
3.1. 申请token地址
https://jdc.welljoint.com:8090/accesstoken/apply
3.2. 上送参数：
appId
appSecret
apiKey
相关参数请联系运维人员获取
3.3. 获取示例：
4. 录音相关接口集成
4.1. 通过callId调听录音
如果存在多段录音，此接口会将录音合并成一个文件。
调听地址：https://jdc.welljoint.com:8090/api/cdr/2.0/recordings/open/call/{callid}/stream?isDownload=0&isWav=0
GET方法
4.1.1. 请求参数
参数名
类型
是否必须
位置
描述
举例
callId
string
是
path
通话ID
e617caaf-1fc6-4998-a8a8-c9313fefde38
isDownload
int
是
query
下载或调听（1:下载；0:调听；2:重定向）
1
isWav
int
是
query
录音类型(1：wav双轨道，0：mp3单轨道)
0
wellcloud_token
string
是
header
Token鉴权
pMXtiCp6QuY8Q01ASEO4bgv23Zre9dOp
4.1.2. 响应参数
文件流
4.1.3. 请求响应示例
https://jdc.welljoint.com:8090/api/cdr/2.0/recordings/open/call/e617caaf-1fc6-4998-a8a8-c9313fefde38/stream?isDownload=0&isWav=0
request headers
Content-Type: audio/x-wav
wellcloud_token: pMXtiCp6QuY8Q01ASEO4bgv23Zre9dOp
4.1.4. 异常示例
4.1.5. 业务错误码
错误码
描述
解决方案
14013
调听或下载录音时，录音不存在
检查录音文件是否存在
401
ssid无效或当前用户权限列表为空或当前用户没有权限
检查ssid是否正常
4.2.  根据callId查询录音记录
通过指定的callId查到所有相关的录音ID及录音信息，并可以使用返回的录音ID，调用下面的《4.3.  根据录音ID调听录音》
查询地址：https://jdc.welljoint.com:8090/api/cdr/2.0/recordings/open/call/{callid}
GET方法
4.2.1. 请求参数
参数名
类型
是否必须
位置
描述
举例
callId
string
是
path
通话ID
e617caaf-1fc6-4998-a8a8-c9313fefde38
wellcloud_token
string
是
header
Token鉴权
pMXtiCp6QuY8Q01ASEO4bgv23Zre9dOp
4.2.2. 响应参数
参数名
类型
是否必须
描述
备注
id
string
是
录音id
主键，uuid
callId
string
是
唯一的呼叫id
deviceId
string
否
设备标识
timeIn
datetime
是
分区时间
startTime
datetime
是
开始时间
endTime
datetime
是
结束时间
recordChannel
int
是
录音声道,1:单声道; 2:立体声
recordLength
int
是
录音时长，单位：秒 精确到毫秒
recordType
string
是
录音类型,1:振铃录音,2:通话录音
recordFormat
string
是
录音格式, WAV,MP3
filePath
string
是
录音文件存放路径
repoId
string
是
录音文件存放仓库名称
failReason
string
否
录音失败原因
recordingRepoNfsPath
string
否
fileService地址
recordingRepoRecordingPath
string
否
录音仓库路径
audioInMos
string
是
通话质量 
4.2.3. 响应示例
https://jdc.welljoint.com:8090/api/cdr/2.0/recordings/open/call/e617caaf-1fc6-4998-a8a8-c9313fefde38
request headers
Content-Type: application/json;charset=utf-8
wellcloud_token: pMXtiCp6QuY8Q01ASEO4bgv23Zre9dOp
request payload
response
4.2.4. 异常示例
4.2.5. 业务错误码
错误码
描述
解决方案
14011
callId不存在
检查callId是否存在
14012 
callId对应的录音记录不存在
通过callId检查对应的录音记录是否存在
4.3.  根据录音ID调听录音
通过指定的录音ID调听录音
查询地址：https://jdc.welljoint.com:8090/api/cdr/2.0/recordings/open/recording/{id}/stream
GET方法
4.3.1. 请求参数
参数名
类型
是否必须
位置
描述
举例
备注
id
string
是
path
录音ID
e617caaf-1fc6-4998-a8a8-c9313fefde38
isDownload 
int 
是 
query 
下载或调听（1:下载；0:调听；2:重定向）
1
isWav
int 
是 
query 
录音类型(1：wav双轨道，0：mp3单轨道)
0
如果调wav只能调最近X天的数据（X可配置）
wellcloud_token
string
是
header
Token鉴权
pMXtiCp6QuY8Q01ASEO4bgv23Zre9dOp
4.3.2. 响应参数
文件流
4.3.3. 响应示例
https://jdc.welljoint.com:8090/api/cdr/2.0/recordings/open/31a3f631-ed66-4713-86b2-c8d75ef4a4c7/stream?isDownload=0&isWav=0
request headers
Content-Type: audio/x-wav
wellcloud_token: pMXtiCp6QuY8Q01ASEO4bgv23Zre9dOp
request payload
response
4.3.4. 异常示例
4.3.5. 业务错误码
错误码
描述
解决方案
14013 
调听或下载录音时，录音文件不存在
检查录音文件是否存在
4.4. 话单查询接口
此接口可以根据 callId 查通话记录
查询地址：https://jdc.welljoint.com:8090/api/cdr/2.0/calls/open/call/{callid}
GET方法
4.4.1. 请求参数
参数名
类型
是否必须
位置
描述
举例
callId
string
是
path
CALL的唯一标识
000371b2-2522-4c13-b414-8cdc5fa0b159
wellcloud_token
string
是
header
Token鉴权
pMXtiCp6QuY8Q01ASEO4bgv23Zre9dOp
4.4.2. 响应参数
参数名
类型
是否必须
描述
备注
callId
string
是
唯一的通话ID
ani
string
是
主叫
dnis
string
是
被叫
callType
string
是
通话类型
callTypeName
string
是
通话类型名
trunkName
string
是
中继组
trunkMember
string
是
中继号
beginTime
string
是
开始时间
customerNo
string
客户号码
customerAreaCode
string
客户号码所属地区
customerRingLength
number
是
振铃时长
customerAnswerTime
string
是
应答时间
result
string
是
拨打结果
resultCode
int
是
拨打结果码
firstIvrNo
string 
是
首个IVR号码
firstIvrAnswerTime
string
是
首个IVR应答时间点
firstStationNo
string
是
首个振铃的分机号码
firstStationRingTime
string
是
首个分机振铃时间点
firstStationAnswerTime
string
是
首个分机应答时间点
firstAgentNo
string
是
首个坐席工号
lastQueueNo
string
是
技能组号
lastQueueLength
int
是
排队时长
lastRoutePoint
string
是
路由点
endTime
string
是
结束时间
endType
string
是
挂断类型
endTypeName
string
是
挂断类型名
hangupDevice
string
是
先挂断的设备
conferenced
int
是
是否是会议
transferred
int
是
是否转接
relateCall
string 
是
关联呼叫CallID
domain
string
是
域名
callLength
number
是
总时长
talkLength
number
是
通话时长
timeId
string
是
时间ID
displayNumber
string
是
外显号码
batchName
string
是
批次号
customerRosterId
string
是
客户名单ID
groupCampaignName
string
是
活动组
campaignName
string
是
活动
satisfaction
string
是
满意度
4.4.3. 请求响应示例
https://jdc.welljoint.com:8090/api/cdr/2.0/calls/open/call/000371b2-2522-4c13-b414-8cdc5fa0b159
request headers
Content-Type: application/json;charset=utf-8
wellcloud_token: pMXtiCp6QuY8Q01ASEO4bgv23Zre9dOp
response
4.4.4. 异常示例
返回的http状态码为400
4.4.5. 业务错误码
错误码
描述
解决方案
14011
callId不存在
检查callId是否存在
4.5. 话单查询接口(时间段)
GET https://jdc.welljoint.com:8090/api/cdr/2.0/calls/open
4.5.1 请求参数
参数名
类型
是否必须
位置
描述
举例
备注
beginTime
string
是
query
查询时间(格式:yyyy-MM-dd HH:mm:ss)
2018-12-01 00:00:00
endTime
string
是
query
查询时间(格式:yyyy-MM-dd HH:mm:ss)
2018-12-31 00:00:00
ani
string
否
query
主叫
8001@xiong.com
dnis
string
否
query
被叫
8002@xiong.com
isOrder
number
是
query
排序（1:升序；0:降序）
1
p
number
否
query
页数(从1开始，默认1)
1
s
number
否
query
每页的数量(默认10,<=100)
10
domain
string
是
query
域名
360.cc
wellcloud_token
string
是
header
Token鉴权
4.5.2  响应参数  
参数名
类型
是否必须
描述
备注
callId
string
是
唯一的通话ID
callTypeName
string
是
呼叫类型名
callType
int
是
呼叫类型名称 code
ani
string
是
主叫号码
dnis
string
是
被叫号码
displayNumber
string
是
外显号码
campaignName
string
是
活动名
groupCampaignName
string
是
活动组名
batchName
string
是
批次名
customerRosterId
string
是
客户名单ID
beginTime
string
是
开始时间
customerId
string
是
客户ID
result
string
是
呼叫结果
resultCode
int
是
拨打结果 code
talkLength
string
是
通话时长
endTypeName
string
是
挂断类型名（挂机原因）
endType
int
是
呼叫结束原因 code
c0
string
是
随路数据c0
c1
string
是
随路数据c1
c2
string
是
随路数据c2
4.5.3  请求响应示例  
request headers
response
 注意：当且仅当查第一页时会有total字段  
{
  "total": 1,
  "list": [
    {
      "result": "用户忙",
      "callTypeName": "自摘机",
      "endTypeName": "呼叫流转",
      "resultCode": 1,
      "callType": 2,
      "endType": 4,
      "callId": "7714f197-6784-4762-905b-0f0bd88e5c59",
      "ani": "8002@xiao.com",
      "dnis": "515021962027@xiao.com",
      "displayNumber": "1002",
      "beginTime": "2020-12-18 14:08:10",
      "campaignName": null,
      "groupCampaignName": null,
      "customerId": null,
      "batchName": null,
      "customerRosterId": null,
      "talkLength": 3.124,
      "c0": null,
      "c1": null,
      "c2": null
    }
  ]
}
4.5.4   异常示例  
{   "errorMessage": "没有权限",   "errorCode": 14001,   "instanceId": "query-xyzws" } 
4.5.5   业务错误码  
错误码
描述
解决方案
400
参数格式非法
401
权限名不合法，或ssid无效
14001
查询时间跨度过大
缩小查询的时间范围
14002
GET请求超时
检查调用的GET服务是否正常
14003
POST请求超时
检查调用的POST服务是否正常
14004
开始时间不得大于结束时间
检查beginTime是否大于endTime
14015
不得跨库查询，必须分成两部分查询
请以xx时间点分为两次查询
4.6 机器人话单查询接口
GET https://jdc.welljoint.com:8090/api/cdr/2.0/open/calls/robot
4.6.1 请求参数
参数名
类型
是否必须
位置
描述
举例
备注
resultCode
int
否
query
呼叫结果（见本文《呼叫结果编码》表）
2
beginTime
string
是
query
查询时间(格式:yyyy-MM-dd HH:mm:ss)
2021-10-31 00:00:00
endTime
string
是
query
查询时间(格式:yyyy-MM-dd HH:mm:ss)
2021-10-31 00:00:00
groupCampaignId
string
否
query
活动组ID
bbbbbbbbbbb
campaignId
string
是
query
子活动组ID
aaaaaaaaaa
batchId
string
否
query
批次ID
uuidxxxxxxxx
batchName
string
否
query
批次名
0928活动测试-20211011182343709
ani
string
否
query
主叫
6001@xiong.com
dnis
string
否
query
被叫
9018800000000@xiong.com
rosterId
string
否
query
名单ID
15
uuiKey
string
否
query
uui的键(结果类型)
currentCallLevel
uuiValue
string
否
query
uui的值(结果内容)；支持模糊
F
isOrder
number
是
query
排序（1:升序；0:降序）
1
isDownload
number
是
query
是否下载（1:下载；0:查询
0
isCount
Boolean
否
query
是否count（true:是；false:否） | 不传默认为true
p
number
否
query
页数(从1开始，默认1)
1
s
number
否
query
每页的数量(默认10,<=100)
10
domain
string
是
query
域名
xiong.com 
wellcloud_token
string
是
header
Token鉴权
4.6.2  响应参数  
参数名
类型
是否必须
描述
备注
callId
string
是
唯一的通话ID
callTypeName
string
是
呼叫类型名
ani
string
是
主叫号码
dnis
string
是
被叫号码
batchId
string
是
批次id
batchName
string
是
批次名
round
string
是
轮次
rosterId
string
是
名单Id
beginTime
string
是
开始时间
endTime
string
是
结束时间
customerAnswerTime
string
是
应答时间
result
string
是
呼叫结果
conclusion
string
是
业务结果
intent
string
是
通话内容
talkLength
string
是
通话时长
chatBotTemplate
string
是
话术模板
ivrNo
string
是
ivr流程号
campaignId
string
是
活动id
4.6.3  请求响应示例  
4.6.4. 异常示例
4.6.5   业务错误码  
错误码
描述
解决方案
400
参数格式非法
401
权限名不合法，或ssid无效
14001
查询时间跨度过大
缩小查询的时间范围
14002
GET请求超时
检查调用的GET服务是否正常
14003
POST请求超时
检查调用的POST服务是否正常
14004
开始时间不得大于结束时间
检查beginTime是否大于endTime
14015
不得跨库查询，必须分成两部分查询
请以xx时间点分为两次查询
5. 自动外呼接口集成
5.1. 即时自动外呼
调用此接口，立即进行自动外呼，外线接通后再转接到caller上
调用接口地址：
https://jdc.welljoint.com:8090/api/ocm/instant/smart
POST方法
5.1.1. 请求参数
Headers
参数名称
是否必须
示例
备注
Content-Type
是
application/json
wellcloud_token
是
pMXtiCp6QuY8Q01ASEO4bgv23Zre9dOp
Token鉴权
Body
名称
类型
是否必须
示例
备注
activityId
string
是
活动id
callee
string
是
被叫号码
caller
string
是
接通后待转接的号码
data
object
是
随路数据
domain
string
是
域名
id
string
否
rosterId
string
是
名单id
tenantId
string
是
租户id
5.1.2. 返回数据
名称
类型
是否必须
默认值
备注
其他信息
reason
string
必须
result
string
必须
5.2. 上传名单
调用此接口将原始名单文件上传到活动。
该接口每次只会接受一个文件，如果上传多个文件，只会处理第一个上传的名单文件。
每该接口调用一次，都会在活动中生成一个批次，批次代表一次上传的多个名单聚合。上传成功时，在响应体中会返回batchId，标识唯一的一个批次。
⚠️ 不会校验名单是否重复，无论是一个文件中多个名单是否重复还是多次调用重复的文件，都认为是不同的名单
调用接口地址：
https://jdc.welljoint.com:8090/api/ros/2.0/batches
POST方法
5.2.1. 名单格式说明
请求体是一个文本文件，可以使用txt、csv以及dat格式的文件
文件名不做要求，建议使用英文名
文件大小不要超过10MB
文件的每一行由名单Id(rosterId), 业务类型(prdType), 手机号(number)，客户id(customerId)和随路数据组成。五部分之间要用英文逗号隔开
随路数据是key-value形式的，key和value之间用英文冒号（“:”）间隔，每一对key-value之间用竖线（“|”）间隔
客户id和随路数据为可选字段，可以不用上传
名单示例：
5.2.2. 请求参数
Headers
参数名称
是否必须
示例
备注
Content-Type
是
multipart/form-data
wellcloud_token
是
pMXtiCp6QuY8Q01ASEO4bgv23Zre9dOp
Token鉴权
Query
参数名称
是否必须
示例
备注
alias
否
2021-11-名单
别名，逻辑上可以将多个批次划分到一个大的批次。比如别名为“催收包-2021”，可以分成多个名单文件上传到不同的活动，有助于业务管理名单
templateId
否
ca0f61c88e194ce5ad47d820ac40d1e0
活动id或活动组id，可以在租户控制台-> 自动外呼-> 活动管理 -> 查看id中获取，是活动的唯一标识
Body
参数名称
参数类型
是否必须
示例
备注
file
file
是
roster.txt
file
5.2.3. 返回数据
名称
类型
是否必须
默认值
备注
data
object
否
├─ alias
string
是
null
批次别名，如果上传时指定，则会返回
├─ batchId
string
是
这一批名单的唯一id
├─ batchName
string
是
自动生成的批次名，规则为产品Id + 时间戳
hasData
boolean
是
false
暂不使用，直接送false
5.2.4. 请求示例
curl示例
5.2.5. 返回示例
5.2.6. 响应码
状态码
说明
201
名单上传调用成功
400
请求参数错误
404
找不到服务或活动
500
内部服务错误
6. 双呼接口集成
“双呼”是指“主叫方”发出外呼请求，由平台后端向主叫方和被叫方分别发起的呼叫，从而实现双方通话。
通常是首先呼叫“主叫方”，“主叫方”接通以后，再呼叫“被叫方”。
调用接口地址：
https://jdc.welljoint.com:8090/api/ext/v2/calls/uc
POST方法
6.1. 请求参数
参数名
是否必须
类型
位置
示例
描述
wellcloud_token
是
String
Header
pMXtiCp6QuY8Q01ASEO4bgv23Zre9xOp
Token鉴权
deviceId
是
String
body
8001@test.cc
发起外呼的分机号码（双呼模式下不需要注册分机，通话录音会关联此分机号）
prefix
是
String
body
9
外呼前缀
customerPhone
是
String
body
13912345678
客户原始号码
userData
否
Map
body
{"Id":"142459"}
用户随路数据, json数据格式
options
否
Map
body
{"bindPhone":"918900000000"}
外呼可选参数，可选值
options参数说明
参数名
类型
示例
描述
bindPhone
String
918900000000
分机绑定的外线号码,应用于手机坐席或者双呼场景，需要带呼出前缀
displayOrigin
String
4008100001
外显主叫号码，需要线路支持
cpa
String
false
外呼被叫方回铃音识别，"true":启用,"false":不启用,默认不启用
displayDest
String
139*****678
SIP终端上显示的被叫号码，可以利用此字段脱敏（双呼场景下无SIP终端，此字段无效）
6.2. 响应参数
参数名
类型
是否必须
描述
callId
String
是
呼叫成功，返回当前呼叫的callId
6.3. 请求示例
6.4. 返回示例
6.5. 异常示例
当请求异常时，接口返回的数据，字段包括：
instanceId：服务实例名称
errorCode：错误码（int）
errorMessage：错误信息
exceptionName：异常名称
错误码说明
错误码
描述
解决方案
14001
deviceId为空
检查deviceId是否为空
14002
dest为空
customerPhone字段不能为空
14005
deviceId不合法
检查deviceId书写格式是否包含域名
12008
cms没有可用的服务实例
检查cms是否启动
7. 事件订阅说明
平台采用订阅/推送的机制，将平台产生的数据按照订阅地址推送给第三方系统，避免轮询造成的延时和系统压力。
目前主要提供了两类Topic
呼叫结束事件
坐席状态改变事件
7.1. 事件订阅
为了方便使用，事件订阅可以直接在“租户控制台”上操作
租户控制台地址：https://console.welljoint.com/
在租户控制台系统中的“联络中心管理“下的“第三方系统集成配置”页面中可以配置事件订阅，页面如下图所示：
URL中填写回调地址。
7.2. 呼叫结束事件（callEnd）
7.2.1. 事件说明
呼叫结束事件，是在通话结束以后，向订阅方主动发送数据的接口，如果推送失败，可以选择重传，重传模式是失败1次后过1分钟重传，第2次失败后过2分钟重传，第30次失败后过30分钟重传。
7.2.2. 字段说明
字段名
类型
格式(样例)
说明
eventName
String
callEnd
事件名称
eventSrc
String
a8307aa5-bd11-4216-a6e2-5e5d6161aec4
事件源
timestamp
Date
2018.06.12 11:33:56.368
事件时间戳
serial
Int
10
事件序列号,值递增
eventType
String
statEvent
事件类型
namespace
String
test.cc
租户命名空间
instanceId
String
cms_default
事件生成实例名称
mediaServer
String
172.16.43.20
媒体服务器地址
callId
String
a8307aa5-bd11-4216-a6e2-5e5d6161aec4
呼叫CallId
cid
String
a8307aa5-bd11-4216-a6e2-5e5d6161aec4
CMS内部CallID
ani
String
18612345678
原始主叫号码
dnis
String
33255500
原始被叫号码
callType
String
Inbound
deviceId
String
8001@test.cc
分机号码（参与呼叫的第一个分机）
agentId
String
5001@test.cc
坐席工号 （参与呼叫的第一个分机签入的坐席）
customerPhone
String
18012345678
客户方号码
startTime
Date
2018.06.12 11:32:01.578
呼叫开始时间戳
answerTime
Date
2018.06.12 11:32:32.071
呼叫应答时间戳
endTime
Date
2018.06.12 11:33:56.368
呼叫结束时间戳
skillId
String
7001@test.cc
排队的技能组号码（呼叫进入坐席前最后一个技能组）
queueLength
float
5.234
呼叫排队时长,单位：秒
talkLength
float
12.459
通话时长,单位：秒
endReason
String
QueueAbandon
customerRingLength
float
10.486
客户方振铃时长,单位：秒
deviceRingLength
float
0.134
坐席振铃时长,单位：秒
displayNumber
String
33255540
外呼外显号码
trunkName
String
SCM_021
客户方使用中继名称
customerAreaCode
String
410329
客户方所属行政区号
customerCityCode
String
0379
客户方所属市区号
resultCode
Int
0
campaignInfo
Map
{"campaignId":"test01"}
自动外呼活动相关信息,JSON格式
userData
Map
{"serviceId":"demo01"}
呼叫用户随路数据,JSON格式
systemData
Map
{"_mobilePhone":"true"}
呼叫系统随路数据,JSON格式
relatedCalls
Set
[]
关联呼叫ID,咨询和转接场景会出现关联呼叫
hangupDevice
String
8001@test.cc
先挂断方设备号
hangupCause
String
NORMAL_CLEARING
挂机信令原因
conferenced
Bool
false
该呼叫是否为会议呼叫
transferred
Bool
false
该呼叫是否为转接呼叫
firstRoutePoint
CallSegment
{...}
呼叫进入的第一个路由点片段信息
firstQueue
CallSegment
{...}
呼叫经过第一个技能组片段信息
firstIVR
CallSegment
{...}
呼叫进入第一个IVR片段信息
firstStation
CallSegment
{...}
呼叫进入的第一个分机片段信息
segmentList
CallSegment
[{...}]
所有片段信息
topics
Set
csta,stat
事件主题
originalCall
String
a8307aa5-bd11-4216-a6e2-5e5d6161aec4
上一个呼叫的callId
7.2.3. 事件返回结果样例
7.3. 坐席状态事件（agentStateEvent）
7.3.1. 事件说明
坐席状态变化事件，每次坐席状态变化，都会立刻向订阅方发送相关数据，包括座席的工作状态和通话状态，可以通过这个接口实时了解坐席状态
7.3.2. 字段说明
字段名
类型
格式(样例)
说明
eventName
String
agentStateEvent
事件名称
serial
Integer
事件的序列号,全局唯一
timestamp
String
发送事件的时间点
eventType
String
agent
事件类型为固定值"agent"
id
String
坐席工号
name
String
坐席的名称
acState
String
·  Aux("未就绪")
·  Available("就绪")
·  Acw("话后处理")
·  Allocate("预占")
·  LogOut("登出")
·  PreView("预览")
·  PreViewHold("预览处理中")
·  OffHook("摘机")
·  Hold("保持")
·  Conference("会议")
·  InboundRinging("呼入振铃")
·  InboundTalking("呼入通话")
·  ManualRinging("手动外呼振铃")
·  ManualTalking("动外呼通话")
·  InternalRinging("内线振铃")
·  InternalTalking("内线通话")
·  PreOccupiedRinging("预占外呼振铃")
·  PreOccupiedTalking("预占外呼通话")
·  PredictiveRinging("预测外呼振铃")
·  PredictiveTalking("预测外呼通话")
·  AgentAutoRinging("坐席自动外呼振铃")
·  AgentAutoTalking("坐席自动外呼通话")
·  PreViewOffHook("预览摘机")
·  PreViewRinging("预览振铃")
·  PreViewTalking("预览通话")
·  Unknown("未知")
坐席当前的状态
stateChangeTime
String
坐席状态改变的时间
lastACState
String
·  Aux("未就绪")
·  Available("就绪")
·  Acw("话后处理")
·  Allocate("预占")
·  LogOut("登出")
·  PreView("预览")
·  PreViewHold("预览处理中")
·  OffHook("摘机")
·  Hold("保持")
·  Conference("会议")
·  InboundRinging("呼入振铃")
·  InboundTalking("呼入通话")
·  ManualRinging("手动外呼振铃")
·  ManualTalking("动外呼通话")
·  InternalRinging("内线振铃")
·  InternalTalking("内线通话")
·  PreOccupiedRinging("预占外呼振铃")
·  PreOccupiedTalking("预占外呼通话")
·  PredictiveRinging("预测外呼振铃")
·  PredictiveTalking("预测外呼通话")
·  AgentAutoRinging("坐席自动外呼振铃")
·  AgentAutoTalking("坐席自动外呼通话")
·  PreViewOffHook("预览摘机")
·  PreViewRinging("预览振铃")
·  PreViewTalking("预览通话")
·  Unknown("未知")
instanceId
String
发送事件的实例名称
namespace
String
域名
lastStateLength
String
坐席上一状态持续的时长
lastStateChangeTime
String
坐席上一状态改变的时间
orgId
String
坐席部门的Id
orgName
String
坐席部门的名称
orgTreePos
String
坐席部门的treepos,用来标记部门的深度
reason
String
·  AgentOperation("坐席操作")
·  AllocateTimeout("预占超时")
· SessionTimeout("session超时")
·  MonitorLock("班长锁定坐席")
·  MonitorUnlock("班长解锁坐席")
AgentOperation
channelAddr
String
坐席登陆的分机号
skills
Array
坐席的技能组信息
callId
String
当前通话的callId
workSkill
String
当前通话是从哪个技能组分配的
7.3.3.  事件返回结果样例
示例1：
示例2：
8. 分享地址
https://welljoint.yuque.com/docs/share/d8ea5569-c8f0-4c79-a7ab-db1ae8395884?# 《京东云-wellcloud用户集成》
https://welljoint.yuque.com/docs/share/d1df3054-71ce-4c71-a170-0952ce682cb0?# 《京东云-wellcloud用户使用说明》
4

jeff、德军、已注销用户等02-10 11:3125174IP 属地上海
举报
所有评论（4）
高鹏
高鹏
2022-07-14 15:48
引用原文：[表格]
Id  为什么是大写的？ 
德军
德军
2022-07-18 09:42
这个可以自定义，按需求设置即可。
高鹏
高鹏
2022-07-14 15:49
引用原文：userData
userData的类型，实际接口类型为Object
德军
德军
2022-07-18 11:03

已修正

1
风月

4
关于语雀数据安全English
大纲
1. 电话终端
1.1. wellphone软电话
1.2. webphone软电话
1.3. SIP电话
2. wellclient软电话集成
3. 获取平台后端接口访问token
3.1. 申请token地址
3.2. 上送参数：
3.3. 获取示例：
4. 录音相关接口集成
4.1. 通过callId调听录音
4.1.1. 请求参数
4.1.2. 响应参数
4.1.3. 请求响应示例
4.1.4. 异常示例
4.1.5. 业务错误码
4.2. 根据callId查询录音记录
4.2.1. 请求参数
4.2.2. 响应参数
4.2.3. 响应示例
4.2.4. 异常示例
4.2.5. 业务错误码
4.3. 根据录音ID调听录音
4.3.1. 请求参数
4.3.2. 响应参数
4.3.3. 响应示例
4.3.4. 异常示例
4.3.5. 业务错误码
4.4. 话单查询接口
4.4.1. 请求参数
4.4.2. 响应参数
4.4.3. 请求响应示例
4.4.4. 异常示例
4.4.5. 业务错误码
4.5. 话单查询接口(时间段)
4.5.1 请求参数
4.5.2 响应参数
4.5.3 请求响应示例
4.5.4 异常示例
4.5.5 业务错误码
4.6 机器人话单查询接口
4.6.1 请求参数
4.6.2 响应参数
4.6.3 请求响应示例
4.6.4. 异常示例
4.6.5 业务错误码
5. 自动外呼接口集成
5.1. 即时自动外呼
5.1.1. 请求参数
5.1.2. 返回数据
5.2. 上传名单
5.2.1. 名单格式说明
5.2.2. 请求参数
5.2.3. 返回数据
5.2.4. 请求示例
5.2.5. 返回示例
5.2.6. 响应码
6. 双呼接口集成
6.1. 请求参数
6.2. 响应参数
6.3. 请求示例
6.4. 返回示例
6.5. 异常示例
7. 事件订阅说明
7.1. 事件订阅
7.2. 呼叫结束事件（callEnd）
7.2.1. 事件说明
7.2.2. 字段说明
7.2.3. 事件返回结果样例
7.3. 坐席状态事件（agentStateEvent）
7.3.1. 事件说明
7.3.2. 字段说明
7.3.3. 事件返回结果样例
8. 分享地址
Adblocker
