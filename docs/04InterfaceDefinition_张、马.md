# 0. 开放接口

## 0.1 /oauth/token 登录
- HTTP方式：POST
- 版本：v0.0.11
- 发布版本：v0.0.11
- 输入参数：

		header: Authorization=xxxxx 
		// 请联系管理员获取
		
1
	|参数名			|参数类型|是否必须|	参数描述		|
	|---- 			| --- 	| ------| ---- 			|
	|username		|string	|yes	|用户的登录用户名	|
	|password		|string	|yes	|用户密码，md5加密后传输|
	|grant_type		|string	|yes	|授权方式，password或refresh_token	|

- 返回参数

		{
			"access_token" = "5fab58c8-6465-4af6-95a1-75cb66550a33",
			"token_type" = "bearer",
			"refresh_token" = "0ba1cde7-3cee-4518-8b33-305684cc1bdc",
			"expires_in" = 14591,
			"scope" = "read write"
		}

## 0.2 /organization/list 获取企业列表
- HTTP方式：GET
- 版本：v0.0.15
- 发布版本：v0.0.15
- 输入参数

	|参数名			|参数类型|是否必须|	参数描述		|
	|---- 			| --- 	| ------| ---- 			|
	|nameMatch		|string	|no		|名字匹配		|
	|pageStart		|int	|否		|起始页，默认1	|
	|pageCount		|int	|否		|每页条数，默认50	|

- 返回参数

	<pre>
	{
		"total":"20",
		"rows":[{
			"orgName":"皖通科技有限公司",
			"orgShortName":"皖通科技",
			"orgCode":"WT001"
			"orgLogo":"http://234.232.123.123/wt.png"		//小logo，50x50
			"orgBand":"http://234.232.123.123/wt_big.png" 	//较大的图标
			},
			{
			"orgName":"皖通科技有限公司",
			"orgShortName":"皖通科技",
			"orgCode":"WT001"
			"orgLogo":"http://234.232.123.123/wt.png"		//小logo，50x50
			"orgBand":"http://234.232.123.123/wt_big.png" 	//较大的图标
			},
			{
			"orgName":"皖通科技有限公司",
			"orgShortName":"皖通科技",
			"orgCode":"WT001"
			"orgLogo":"http://234.232.123.123/wt.png"		//小logo，50x50
			"orgBand":"http://234.232.123.123/wt_big.png" 	//较大的图标
			}
		]
	}
	</pre>

## 0.3 /organization/{orgCode} 获取企业信息
- HTTP方式：GET
- 版本：v0.0.16
- 发布版本：v0.0.16
- 权限：无
- 输入参数

	|参数名			|参数类型|是否必须|	参数描述		|
	|---- 			| --- 	| ------| ---- 			|
	|orgCode		|string	|yes	|企业号			|

- 返回参数

	<pre>
	{
		"orgName":"皖通科技有限公司",
		"orgShortName":"皖通科技",
		"orgCode":"WT001"
		"orgLogo":"http://234.232.123.123/wt.png"		//小logo，50x50
		"orgBand":"http://234.232.123.123/wt_big.png" 	//较大的图标
	}
	</pre>

## 0.4 /organization/tree 获取组织架构树
- HTTP方式：GET
- 版本：
- 发布版本：
- 权限：平台管理员
- 输入参数

-返回参数
- 返回参数

	<pre>
	[{
		"orgUid":"345642acedf32a8ce7394263cafe2938"
		"orgCode":"345642acedf32a8ce7394263cafe2938",
		"orgName":"车辆管理研发部",
		"orgSname":"车管部"
		"logoUrl":"233.233.233.233/....."
		"biglogo":"233.233.233.233/....."
		"porgUid":"345642acedf32a8ce7394263cafe2938" //父级Uid
		"createTime":"2012-01-01"
		"type":"1"
		"children": [
			{
				"orgUid":"345642acedf32a8ce7394263cafe2938"
				"orgCode":"345642acedf32a8ce7394263cafe2938",
				"orgName":"车辆管理研发部",
				"orgSname":"车管部"
				"logoUrl":"233.233.233.233/....."
				"biglogo":"233.233.233.233/....."
				"createTime":"2012-01-01"
				"type":"2"
				"porgUid":"345642acedf32a8ce7394263cafe2938"
			},
			{
				"deptUid":"345642acedf32a8ce7394263cafe2938",
				"deptName":"车辆管理研发部",
				"deptShortName":"研发部"
				"orderNo":"1",
				"pdeptUid":"345642acedf32a8ce7394263cafe2938"
				"children":[]
			}
		]
	},
	{
		"orgUid":"345642acedf32a8ce7394263cafe2938"
		"orgCode":"345642acedf32a8ce7394263cafe2938",
		"orgName":"车辆管理研发部",
		"orgSname":"车管部"
		"logoUrl":"233.233.233.233/....."
		"biglogo":"233.233.233.233/....."
		"createTime":"2012-01-01"
		"type":"1"
		"porgUid":"345642acedf32a8ce7394263cafe2938"
	}]
	</pre>


# 1. 用户管理

## 1.1 /user 用户注册
- HTTP方式：POST
- 版本：v 0.0.43
- 发布版本：v 0.0.24
- 权限：机构管理员
- 输入参数

	|参数名			|参数类型|是否必须|	参数描述		|
	|---- 			| --- 	| ------| ---- 			|
	|loginId		|string	|yes	|用户的登录名		|
	|deptUid		|string	|yes	|用户所属部门		|
	|realName		|string	|yes	|姓名			|
	|password		|string	|yes	|初始账户密码		|
	|phone			|string	|yes	|用户手机号		|
	|email			|string	|no		|用户邮箱		|
	|roles			|array	|yes	|默认注册用户均有普通用户权限，若需要其他权限需指定 <br /> leader：部门领导，负责审批订单 <br /> vehicle_manager：负责管理部门下的车辆	|

- 返回参数

	<pre>
	{
		"userUid":"8329004fadcce8934"
	}
	</pre>

## 1.2 /user/profile
- HTTP方式：GET
- 版本：v0.0.43
- 发布版本：v0.0.15
- 权限：用户以上
- 输入参数

	|参数名			|参数类型|是否必须|	参数描述		|
	|---- 			| --- 	| ------| ---- 			|
	|loginId		|string	|no 	|用户的登录名，当为空时，查询自己，不为空根据loginId获取用户信息		|

- 输出参数

	<pre>
	{	
		"uid":"34234d-c334234-3dcabef",		// 用户在系统中的唯一标识
		"realName":"李四",
		"phone":"13854685847",
		"email":"aaa@1234",
		"loginId":"manager",
		"dept":{
			"deptUid":"347829374dcabc",
			"deptName":"车辆管理服务平台开发部",
			"deptShortName":"车管开发部"
		}
		"roles":["user", "leader"]
	}
	</pre>

## 1.3 /user/profile 用户信息修改
- HTTP方式：POST
- 版本：0.0.43
- 发布版本：0.0.39
- 发布版本：无
- 权限：用户以上
- 输入参数

	|参数名			|参数类型|是否必须|	参数描述		|
	|---- 			| --- 	| ------| ---- 			|
	|cmd			|string	|yes	|set：设置参数<br>delete:删除，伪删除，此操作需要机构管理员权限，只能删除其他账户，cmd为该参数时，loginId必须发送	|
	|loginId		|String	|no		|不发送时，表示修改自己的数据，发送该参数时，表示修改loginId对应用户信息|
	|realName		|string	|no		|姓名			|
	|oldPassword	|string	|no		|当修改密码时必须附带该参数	|
	|password		|string	|no		|密码			|
	|phone			|string	|no		|用户手机号		|
	|email			|string	|no		|用户邮箱		|
	|*以下需要 机构管理员 权限*|	|
	|deptUid		|String	|no		|修改用户部门		|
	|roles			|array	|no		|默认注册用户均有普通用户权限，若需要其他权限需指定 <br /> leader：部门领导，负责审批订单 <br /> vehicle_manager：负责管理部门下的车辆	|

- 返回参数

	<pre>
	{
		"ret":"0",
		"message":"操作成功"
	}
	</pre>



## 1.4 /user/passenger 添加乘客信息
- HTTP方式：POST
- 版本：v0.0.17
- 发布版本：v0.0.17
- 输入参数

	|参数名			|参数类型|是否必须|	参数描述		|
	|---- 			| --- 	| ------| ---- 			|
	|passengers		|array	|yes	|乘客信息列表		|

	<pre>
	{
		"passengers":[
			{
				"name":"宋江",
				"phone":"13234532345"
			}
		]
	}
	</pre>

- 返回参数

	<pre>
	{
		"ret":"0",
		"message":"操作成功"
	}
	</pre>

## 1.5 /user/passenger/list 获取用户的乘车人列表
- HTTP方式：GET
- 版本：v0.0.17
- 发布版本：v0.0.14
- 输入参数

	无
	
- 返回参数

	<pre>
		{
			"total":"3"
			"rows":[
				{
					"name":"张三",
					"phone":"13783920843"
				},
				{
					"name":"张三",
					"phone":"13783920843"
				},
				{
					"name":"张三",
					"phone":"13783920843"
				}
			]
		}
	</pre>


# 2. 订单管理

## 2.1 /order 建立订单
- HTTP方式：POST
- 版本：v0.0.24
- 发布版本：v0.0.24
- 输入参数

	|参数名			|参数类型|是否必须|	参数描述		|
	|---- 			| --- 	| ------| ---- 			|
	|planStartPlace	|string	|	是	|上车地点		|
	|startLocLa		|string	|	否	|上车地点纬度		|
	|startLocLo		|string	|	否	|上车地点经度		|
	|planEndPlace 	|string	| 	是 	|出行目的地 		|
	|endLocLa		|string	|	否	|出行目的地纬度	|
	|endLocLo		|string	|	否	|出行目的地经度	|
	|planStartTime	|string	|	是	|用车起始时间		|
	|planEndTime	|string	|	是	|用车结束时间		|
	|vehicleCount	|int	|	是	|用车数量		|
	|vehicleInfo	|array	|	否	|申请的车辆uid	|
	|passengers		|array	|	否	|用车人			|
	|description	|string	|	否	|说明			|

- 示例输入
	<pre>
	{
		"planEndPlace": "目的地",
		"startTime": "2015-06-18 23:59:59",
		"planStartPlace": "起始地点",
		"vehicleCount": "1",
		"vehicleInfo": ["324238985498acd", "34234234789234ace"],
		"passsengers": [
			{
				"name": "张三",
				"phone": "13548568756"
			},
			{
				"name": "张三",
				"phone": "13548568756"
			}
		]
	}
	</pre>
- 返回参数

    新建订单的id：

		{"serialNum":"345642acedf32a8ce7394263cafe2938"}

## 2.2 /order/{serialNum}   订单修改
- HTTP方式：POST
- 版本：v0.0.24
- 发布版本：v0.0.24
- 输入参数

	|参数名		|参数类型|是否必须|参数描述	|
	| ---- 		| --- 	| ------| ---- 		|
	|cmd		|string	|	否	|get或为空：获取该订单详细信息 <br /> approve：批准该订单 <br/> reject：不同意该订单 <br /> issue：分配车辆司机 <br /> begin：司机发车 <br /> finish：完成该订单 |
	|comment	|string	|	否	|审批意见	|
	|issueInfo	|array	|	否	|分配车辆信息，当为issue时不为空|
- 输入示例
	<pre>
	{
		"cmd":"issue",
		"issueInfo":[{
			"orderUid": "34234823948723ad",
			"driverName": "张三",
			"driverUid":"345642acedf32a8ce7394263cafe2938", 
			"userUid":"345642acedf32a8ce7394263cafe2938",   //司机的userUid
			"phone":"13809872312",
			"vehicleUid": "345642acedf32a8ce7394263cafe2938",
			"vehicleLic":"皖A56XCV",
			"vehicleType":"别克商务车",
			"seatNum":"4"},
			{
			"orderUid": "34234823948723ad",
			"driverName": "张三",
			"driverUid":"345642acedf32a8ce7394263cafe2938",
			"userUid":"345642acedf32a8ce7394263cafe2938",
			"phone":"13809872312",
			"vehicleUid": "345642acedf32a8ce7394263cafe2938",
			"vehicleLic":"皖A56XCV",
			"vehicleType":"别克商务车",
			"seatNum":"4"},
			{
			"orderUid": "34234823948723ad",
			"driverName": "张三",
			"driverUid":"345642acedf32a8ce7394263cafe2938",
			"userUid":"345642acedf32a8ce7394263cafe2938",
			"phone":"13809872312",
			"vehicleUid": "345642acedf32a8ce7394263cafe2938",
			"vehicleLic":"皖A56XCV",
			"vehicleType":"别克商务车",
			"seatNum":"4"}
		]}
	</pre>

- 返回参数
	- 当操作为get时返回：
	<pre>
	{"serialNum":"345642acedf32a8ce7394263cafe2938",
	  "planStartTime":"2015-06-08 12:00:00",
	  "planEndTime":"2015-06-18 12:00:00",
	  "driverAndVehicle":[
			{"orderUid": "345642acedf32a8ce7394263cafe2938",
			 "vehicleLic":"皖A56XCV",
             "vehicleType":"别克商务车",
			 "driver": "张三",
			 "phone":"18789831234"},
	        {"orderUid": "345642acedf32a8ce7394263cafe2938",
			 "vehicleLic":"皖A56XCV",
             "vehicleType":"别克商务车",
			 "driver": "张三",
			 "phone":"18789831234"},
	        {"orderUid": "345642acedf32a8ce7394263cafe2938",
			 "vehicleLic":"皖A56XCV",
             "vehicleType":"别克商务车",
			 "driver": "张三",
			 "phone":"18789831234"}
		]，
	  "planStartPlace":"皖水路皖通科技",
	  "planEndPlace":"合肥新桥机场"，
	  "vehicleCount":"3"，
	  "status":"1",
	  "comment":"测试",
	  "applyTime":"2015-06-01",
	  "applyDept":"技术中心",
	  "passengers":[{
			"name":"张三",
			"phone":"23212342345"
		},
		{
			"name":"张三",
			"phone":"23212342345"
		}
		],
	  "applicant":"张三",
      "phone":"18932393456",
	  "approveStatus":"0",
	  "isFeedback":"0",
      "isSubmitLog":"0"
	}
	</pre>
	- 当其他操作是返回：
	<pre>
	{"ret":"0",
     "message":"操作成功"
	}
	</pre>

## 2.3 /order/list 订单列表
- HTTP方式：GET
- 版本：v0.0.26
- 发布版本：v0.0.24
- 权限：用户以上
- 输入参数

	|参数名			|参数类型|是否必须|参数描述|
	| ---- 			| --- 	| -----	| ---- 	|
	|status			|array	|no		|状态<br/>不发送该参数代表全部 <br/> 1：审批中 <br/>2：审批中(待派车)<br/> 3：已配车 <br /> 4：在途 <br/> 5：完成 <br /> *该参数可输入多个状态值，为以逗号分隔的字符串，格式为：1,2* |
	|self			|boolean|	no	|true：只查询用户自己的订单 <br /> false：查询所管理部门所有订单 <br />*该参数对用户无效*|
	|pageStart		|int	|	否	|起始页，默认1	|
	|pageCount		|int	|	否	|每页条数，默认50	|
	|startTime		|date	|	否	|订单起止时间范围的开始时间|
	|endTime		|date	|	否	|订单起止时间范围的结束时间|
	|function		|int	|	否	|对startTime与endTime给定的时间范围进行功能选择，此参数应与startTime、endTime结合使用 <br/> 1：在该时间范围内创建的订单 <br/>2：在该时间范围内开始的订单 <br/>3：在该时间范围内结束的订单|
	|vehicleLic		|String	|	否	|订单配车的车牌号	|
	|vehicleNick	|String	|	否	|订单配车的别名	|
	|applicant		|String	|	否	|订单申请者的姓名	|
	|orderBy		|String	|	否	|订单列表排序依据。<br/>不发送默认按照申请时间排序<br/>createTime:申请时间排序<br/>startTime:用车时间排序<br/>useTime:用车时长排序|
	|sequence		|String	|	否	|排序方式<br/>不发送默认按照升序排序<br/>asc:降序<br/>desc:升序|
	

- 返回参数
	<pre>
	{
	"total":"100",
	"rows":[
		{"serialNum":"345642acedf32a8ce7394263cafe2938",
		"planStartTime":"2015-06-08 12:00:00",
		"planEndTime":"2015-06-18 12:00:00",
		"driverAndVehicle":[
			{"orderUid": "345642acedf32a8ce7394263cafe2938",
			"vehicleLic":"皖A56XCV",
			"vehicleType":"别克商务车",
			"driver": "张三",
			"phone":"18789831234"},
			{"orderUid": "345642acedf32a8ce7394263cafe2938",
			"vehicleLic":"皖A56XCV",
			"vehicleType":"别克商务车",
			"driver": "张三",
			"phone":"18789831234"},
			{"orderUid": "345642acedf32a8ce7394263cafe2938",
			"vehicleLic":"皖A56XCV",
			"vehicleType":"别克商务车",
			"driver": "张三",
			"phone":"18789831234"}
			]，
		"planStartPlace":"皖水路皖通科技",
		"planEndPlace":"合肥新桥机场"，
		"vehicleCount":"3"，
		"status":"1",
		"comment":"测试",
		"applyTime":"2015-06-01",
		"applyDept":"技术中心",
		"passengers":[张三,李四,王五],
		"applicant":"张三",
		"phone":"18932393456",
		"approveStatus":"0",
		"isFeedback":"0",
		"isSubmitLog":"0"
		}，
		{"serialNum":"345642acedf32a8ce7394263cafe2938",
		"planStartTime":"2015-06-08 12:00:00",
		"planEndTime":"2015-06-18 12:00:00",
		"driverAndVehicle":[
			{"orderUid": "345642acedf32a8ce7394263cafe2938",
			"vehicleLic":"皖A56XCV",
			"vehicleType":"别克商务车",
			"driver": "张三",
			"phone":"18789831234"},
			{"orderUid": "345642acedf32a8ce7394263cafe2938",
			"vehicleLic":"皖A56XCV",
			"vehicleType":"别克商务车",
			"driver": "张三",
			"phone":"18789831234"},
			{"orderUid": "345642acedf32a8ce7394263cafe2938",
			"vehicleLic":"皖A56XCV",
			"vehicleType":"别克商务车",
			"driver": "张三",
			"phone":"18789831234"}
			]，
		"planStartPlace":"皖水路皖通科技",
		"planEndPlace":"合肥新桥机场"，
		"vehicleCount":"3"，
		"status":"1",
		"comment":"测试",
		"applyTime":"2015-06-01",
		"applyDept":"技术中心",
		"passengers":[张三,李四,王五],
		"applicant":"张三",
		"phone":"18932393456",
		"approveStatus":"0",
		"isFeedback":"0",
		"isSubmitLog":"0"
		}
		]
	}
	</pre>

## 2.4 /order/{orderUid}/fund 税费添加
-  HTTP方式：POST
-  版本：v0.0.14
-  发布版本：v0.0.14
-  输入参数

	|参数名			|参数类型|是否必须|参数描述	|
	| -----------	| --- 	| ------| ---- 		|
	|costType		|int	|	是	|1：高速公路收费 <br /> 2：加油费 <br /> 3：车辆维修费	|
	|costAmount		|double	|	是	|费用值，单位元		|
	|costLoc		|string	|	否	|缴费地点，描述地点的字符串	|
	|costTime		|time	|	否	|缴费时间	|
	|comment		|string	|	否	|说明		|


- 返回参数

		{"ret"："0"，
	 	"message":"保存成功"}

## 2.4 /fund/fundUid 税费信息修改/获取
-  HTTP方式：POST/GET
-  版本：v0.0.14
-  发布版本：v0.0.14
-  输入参数

	|参数名			|参数类型|是否必须|参数描述	|
	| -----------	| --- 	| ------| ---- 		|
	|costUid		|string	|yes	|税费的uuid	|
	|costType		|int	|	是	|1：高速公路收费 <br /> 2：加油费 <br /> 3：车辆维修费	|
	|costAmount		|double	|	是	|费用值，单位元		|
	|costLoc		|string	|	否	|缴费地点，描述地点的字符串	|
	|costTime		|time	|	否	|缴费时间	|
	|comment		|string	|	否	|说明		|


- 返回参数

		{"ret"："0"，
	 	"message":"保存成功"}


## 2.5 /order/{orderUid}/fund/list 税费查询
- HTTP方式：GET
- 版本：v0.0.14
- 发布版本：v0.0.14
- 输入参数
	
	|参数名			|参数类型|是否必须|参数描述		|
	| -----------	| --- 	| ------| ---- 			|
	|startTime		|date	|no		|查询的时间段		|
	|endTime		|date	|no		|查询的时间段		|
	|pageStart		|int	|no		|起始页码， 默认1	|
	|pageCount		|int	|no		|每页条数，默认50	|

- 返回参数

	<pre>
	{
		"orderUid":"345642acedf32a8ce7394263cafe2938",
		"driverName":"张三",
		"createDate":"2015-06-14",
		"passengers":"李四，王五",
		"passengersPhone":"18955179867",
		"startTime":"2015-06-14 9:30",
		"endTime":"2015-06-14 12:30",
		"startPlace":"集团公司",
		"endPlace":"合肥市政府",
		"distance":"30公里",
		"total":"2",
		"rows":
			[{
				"costUid":"3423432",
				"costType":"1",
				"costAmount":"50.00", //以元为单位的货币数量
				"costLoc":"肥西收费站",
				"costTime":"2015-06-21 11:08:00"
				"comment":""},
			{
				"costUid":"3423432",
				"costType:"2",
				"costAmount":"35.00",
				"costLoc":"加油站",
				"costTime":"2015-06-21 12:00:00",
				"comment":"加油5L"},
			{
				"costUid":"3423432",
				"costType":3,
				"costAmount":"300.00"
				"costLoc":"肥西修理厂",
				"costTime":"2015-06-21 13:30:00",
				"comment":"更换电池"
			}]
	}
	</pre>

# 3. 车辆管理
## 3.1 /vehicle 注册新车
- HTTP方式：POST
- 版本:0.0.51
- 发布版本：0.0.39
- 输入参数

	|参数名			|参数类型|是否必须|参数描述	|
	| -----------	| --- 	| ------| ---- 		|
	|deptUid		|String	|是		|车辆所属部门	|
	|vehicleLic		|string	|	是	|车辆车牌号	|
	|vehicleType	|string	|	是	|车辆类型	|
	|vehicleNickName|string	|	否	|车辆别名	|
	|vehicleSeatNum	|int	|	yes	|车辆座位数	|
	|fuelType		|string	|yes	|车辆油耗类型id，从油费管理接口获取	|
	|vehicleBrand	|string	|yes	|车辆品牌	|
	|isShow			|int	|	no	|车辆是否显示，1显示，0不显示，所有涉及到该功能的字段均为isShow	|

- 输入参数示例

		{
			"vehicleLic":"皖A56XCV",
			"vehicleType","别克商务车",
			"vehicleLogo","http://220.178.67.250:8080/images/vechicle0190122.png",
			"vehicleSeatNum": 5,
			"isShow":1
		}

- 返回参数


		{
			"ret":0,
			"message":"操作成功"
		}

### 3.1.1 /vehicle/brands 获取车辆品牌
- HTTP方式：GET
- 版本：
- 发布版本：
- 说明：无
- 输入参数：无
- 返回参数：
	<pre>
	{
		"total":3
		"rows":[{"brand":"奥迪A6",
				 "picUrl":"http://123.123.123.1"
				},
				{"brand":"奥迪A6",
				 "picUrl":"http://123.123.123.1"
				}}
	}
	<pre>

### 3.1.2 /vehicle/series 获取对应品牌车系
- HTTP方式：GET
- 版本：
- 发布版本：
- 说明：无
- 输入参数：

	|参数名			|参数类型|是否必须|参数描述	|
	|---------------|-------|-------|-----------|
	|brand			|String	|是		|车辆品牌	|

- 返回参数：
	<pre>
	{
		"total":3,
		"rows":["奥迪"，"大众"，"本田"]
	}
	<pre>

### 3.1.3 /vehicle/fuelType/list 获取油耗类型列表
- HTTP方式：GET
- 版本：无
- 发布版本：
- 输入参数：
- 返回参数：

	<pre>
	{
		"typeUid":"232143243232",
		"typeName":"93#",
		"typePrice":"2.3"
	}
	</pre>

## 3.2 /vehicle/list 根据条件获取车辆列表
- HTTP方式：GET
- 版本：v0.0.39
- 发布版本：v0.0.24
- 说明：无
- 输入参数

	|参数名			|参数类型|是否必须|参数描述	|
	|---------------|----	|----	|----		|
	|startTime		|date	|no		|开始时间	|
	|endTime		|date	|no		|结束时间，该参数用户查询某个时间段内的车辆状态，与5，6状态有关	|
	|status			|int	|no		|4：执行任务中 <br /> 5：空闲（区别于静止和运动中）|
	|movingStatus	|int	|no		|1：运动中 <br /> 2：静止 <br /> 3：离线 <br /> |
	|matchLic		|string	|no		|车牌号匹配字符串	|
	|matchNick		|string	|no		|别名匹配		|
	|matchType		|string	|no		|车辆类型字段匹配	|
	|pageStart		|int	|no		|起始页码， 默认1	|
	|pageCount		|int	|no		|每页条数，默认50	|
	|driverUid		|String	|no		|如果发送该参数，则返回列表中的前几条为该司机最近使用车辆，最多为前三条|
	|driverName		|String	|no		|根据当前出车的司机姓名匹配车辆（模糊匹配）	|
	|deptUid		|String	|no		|获取该部门的车辆	|
	
- 返回参数
	<pre>
	{
		"total":"2",
		"rows":[
		{
			"vehicleUid": "30928340932840afed",
			"vehicleNickName": "商务部2",
			"vehicleLic":"皖A44532",
			"vehicleAvatar":"http://343.213.432.534/car.png",
			"vehicleType":"别克商务车",
			"status":"4",
			"seatNum":"4",
			"buUid":"32402938420398fdc",			// 车辆所属部门
			"buName": "部门名称",
			"buShortName":"部门短名",
			"driverUid":"30928340932840afed",		//当前驾驶该车的司机，若没有则为null。
			"driverName":"曹操",
			"driverPhone":"12534582531",
			"location": {
				"la":"4.564234",	//经度
				"lo":"5.324234"		//纬度
				"dir":"23.24"		//以正北为0度，顺时针方向的旋转角
				"speed":"23.234"	//时速，km/h
				"status":"1",		//行驶状态：1：运动中；2：静止；3：离线
				"updateTime":"2015-07-03"			// 位置更新时间
			}
			"fuelType":"90#"
		},
		{
			"vehicleUid": "30928340932840afed",
			"vehicleNickName": "商务部2",
			"vehicleLic":"皖A44532",
			"vehicleAvatar":"http://343.213.432.534/car.png",
			"vehicleType":"别克商务车",
			"seatNum":"4",
			"status":"2",
			"driverUid":"30928340932840afed",//当前驾驶该车的司机，若没有则为null。
			"buName": "部门名称",
			"buShortName":"部门短名",
			"driverName":"曹操",
			"driverPhone":"12534582531",
			"location": {
				"la":"4.564234",	//经度
				"lo":"5.324234"		//纬度
				"dir":"23.24"		//以正北为0度，顺时针方向的旋转角
				"speed":"23.234"	//时速，km/h
				"status":"1",		//行驶状态：1：运动中；2：静止；3：离线
				"updateTime":"2015-07-03"			// 位置更新时间
			}
			"fuelType":"90#"
		}
	]}
</pre>

## 3.3 /vehicle/{vehicleUid} 修改车辆信息
- HTTP方式：POST
- 版本：v0.0.33
- 发布版本：0.0.39
- 输入参数

	|参数名			|参数类型|是否必须|参数描述	|
	|---------------|----	|----	|----		|
	|cmd			|string	|no		|get:可以为空，查询vehicle_id对应的车辆信息 <br /> modify:修改车辆信息 <br /> delete:删除车辆信息
	|isShow			|int	|no		|设置该车辆是否显示|

- 返回参数
	
	<pre>
	{
		"vehicleUid": "30928340932840afed",   //车辆uid 	
		"vehicleNickName": "商务部2",		//车辆别名
		"vehicleLic":"皖A44532",				//车牌号
		"vehicleAvatar":"http://343.213.432.534/car.png",	//车辆logo地址
		"vehicleType":"别克商务车",			//车辆类型
		"vehicleBrand":"别克",
		"deptName":"车辆管理部门",				//车辆所属部门
		"deptUid":"12313432524542",
		"displayDistance":"15000.12",		//车辆当前行驶里程
		"seatNum":"4",						//车辆座位数
		"driverUid":"30928340932840afed",//当前驾驶该车的司机，若没有则为null。
		"driverName":"曹操",
		"driverPhone":"21323454534"
		"location": {
			"la":"4.564234",	//经度
			"lo":"5.324234"		//纬度
			"dir":"23.24"		//以正北为0度，顺时针方向的旋转角
			"speed":"23.234"	//时速，km/h
			"status":"1",		//行驶状态：1：运动中；2：静止；3：离线
			"updateTime":"2015-07-03"			// 位置更新时间
		}
		"fuelType":"90#"
	}
	</pre>

## 3.3.1 /vehicle/{vehicleUid} 获取车辆详细信息
- HTTP方式： GET
- 版本：v0.0.52
- 发布版本：0.0.39
- 输入参数

	无

- 返回参数
	
	<pre>
	{
		"vehicleUid": "30928340932840afed",   //车辆	uid
		"vehicleNickName": "商务部2",		//车辆别名
		"vehicleLic":"皖A44532",				//车牌号
		"vehicleAvatar":"http://343.213.432.534/car.png",	//车辆照片地址
		"vehicleType":"别克商务车",			//车辆类型
		"buName":"车辆管理部门",				//车辆所属部门
		"displayDistance":"15000.12",		//车辆当前行驶里程
		"vehicleAuditTime":"2015-12-01",		//年检时间		
		"seatNum":"4",						//车辆座位数
		"driverUid":"30928340932840afed",	//当前驾驶该车的司机，若没有则为null。
		"driverName":"曹操",					//
		"driverPhone":"123456"				// 电话号码
		"location": {						//位置信息
			"la":"4.564234",	//经度
			"lo":"5.324234",	//纬度
			"dir":"23.24",		//以正北为0度，顺时针方向的旋转角
			"speed":"23.234",	//时速，km/h
			"status":"1",		//行驶状态：1：运动中；2：静止；3：离线
			"rpm":"2342",		// 发动机转速rpm
			"voltage":"13.9",	// 电瓶电压V
			"avgFuel":"6",		// 平均油耗L
			"mile":"10143.2",	// 行驶里程km
			"fuel":"50",		// 当前油耗L
			"allFuel":"298",	// 总油耗L
			"degree":"23",		// 环境温度（℃）
			"updateTime":"2015-07-04 23:22:10"	// 信息更新时间
		}
		"fuelType":"90#"
	}
	</pre>

## 3.4 /vehicle/{vehicleUid}/currentLocation
- HTTP方式：GET
- 版本：0.0.39
- 发布版本：0.0.39
- 输入参数

	无

- 返回参数

	<pre>
	{
		"lo":"4.564234",	//经度
		"la":"5.324234",	//纬度
		"dir":"23.24",		//以正北为0度，顺时针方向的旋转角
		"speed":"23.234",	//时速，km/h
		"status":"1",		//行驶状态：1：运动中；2：静止；3：离线
		"rpm":"2342",		// 发动机转速rpm 1-1000
		"voltage":"13.9",	// 电瓶电压V 9-36
		"avgFuel":"6",		// 平均油耗L
		"mile":"10143.2",	// 行驶里程km
		"fuel":"50",		// 当前油耗L
		"allFuel":"298",	// 总油耗L
		"degree":"23",		// 环境温度（℃）
		"updateTime":"2015-07-04 23:22:10"	// 信息更新时间
		"waterDegree":"90"	// 当前水温 -40-200
	}
	</pre>

## 3.5 /vehicle/{vehicleUid}/journals 获取车辆行车记录
- HTTP方式：GET
- 版本：0.0.43
- 发布版本：0.0.43
- 输入参数:

	|参数名			|参数类型|是否必须|参数描述	|
	|---------------|----	|----	|----		|
	|day			|string	|yes	|查询的日期	|

- 返回参数：

	<pre>	
	{
		"total":10,
		"rows":[
			{
				"startLoc": {
					"time":2282736129382,	// 该点的timestamp
					"desc":"三里庵"
					"locLo":"23.23123",		// 该点的经度
					"locLa":"43.23232",		// 该点的纬度
					"speed":"100",			// 车辆时速（km/h）
					"angle":"300",			// 车辆的行驶方向，以正北为0度，顺时针方向
					"events":[
						{
							"eventLogo":"http://232...",		// 事件的图标
							"eventDesc":"发动机点火"				// 事件描述
						}
					]
				},
				"endLoc": {
					"time":2282736129382,	// 该点的timestamp
					"desc":"之心城"
					"locLo":"23.23123",		// 该点的经度
					"locLa":"43.23232",		// 该点的纬度
					"speed":"100",			// 车辆时速（km/h）
					"angle":"300",			// 车辆的行驶方向，以正北为0度，顺时针方向
					"events":[
						{
							"eventLogo":"http://232...",		// 事件的图标
							"eventDesc":"发动机点火"				// 事件描述
						}
					]
				},
				"miles":"123",				// 行驶距离km
				"stand":"234",				// 停留时间（结束点） 
				"fuel":"3.4"				// 油耗L
			},
			{
				"startLoc": {
					"time":2282736129382,	// 该点的timestamp
					"locLa":"23.23123",		// 该点的经度
					"locLo":"43.23232",		// 该点的纬度
					"speed":"100",			// 车辆时速（km/h）
					"angle":"300",			// 车辆的行驶方向，以正北为0度，顺时针方向
					"events":[
						{
							"eventLogo":"http://232...",		// 事件的图标
							"eventDesc":"发动机点火"				// 事件描述
						}
					]
				},
				"endLoc": {
					"time":2282736129382,	// 该点的timestamp
					"locLa":"23.23123",		// 该点的经度
					"locLo":"43.23232",		// 该点的纬度
					"speed":"100",			// 车辆时速（km/h）
					"angle":"300",			// 车辆的行驶方向，以正北为0度，顺时针方向
					"events":[
						{
							"eventLogo":"http://232...",		// 事件的图标
							"eventDesc":"发动机点火"				// 事件描述
						}
					]
				},
				"miles":"123",				// 行驶距离km
				"stand":"234",				// 停留时间（结束点）
				"fuel":"3.4"				// 油耗L
			}
		]
	}
	</pre>
	

## 3.6 /vehicle/{vehicleUid}/path 获取车辆历史轨迹接口
- HTTP方式：GET
- 版本：0.0.51
- 发布版本：0.0.43
- 输入参数

	|参数名			|参数类型|是否必须|参数描述	|
	|---------------|----	|----	|----		|
	|startTime		|date	|yes	|查询的时间段	|
	|endTime		|date	|yes	|查询的时间段	|
	|accuracy		|String	|no		|查询的时间精度，以秒为单位，默认30s	|
	|eventVisible	|boolean|yes	|是否显示行驶路径上的警告	|
	|eventType		|string	|no		|事件的警告类型id，以逗号分隔。	|
	|
	|eventUid		|string	|no		|当该参数存在时，按该警告出现的时间查找行驶路线。以上参数均失效|
	
- 返回参数
	
	<pre>
	{
		"total":100,
		"rows":[
			{
				"time":"2282736129382",	// 该点的timestamp
				"locLa":"23.23123",		// 该点的经度
				"locLo":"43.23232",		// 该点的纬度
				"speed":"100",			// 当前车辆时速（km/h）
				"angle":"300",			// 当前车辆的行驶方向，以正北为0度，顺时针方向
				"events":[
					{
						"eventUid":"34234234"				// 事件的uid
						"eventType":"3"						// 事件类型ID
						"eventLogo":"http://232...",		// 事件的图标
						"eventDesc":"发动机点火"				// 事件描述
						"eventTime":"2015-07-16 23:00:00"	// 事件发生时间
					}
				]
			}
		]
	}
	</pre>

## 3.7 /vehicle/{vehicleUid}/consumption
- HTTP方式：GET
- 版本：0.0.43
- 发布版本：0.0.43
- 输入参数：

	|参数名			|参数类型|是否必须|参数描述	|
	|---------------|----	|----	|----		|
	|startTime		|date	|yes	|查询的时间段	|
	|endTime		|date	|yes	|查询的时间段	|

- 输出参数：

	<pre>
	{
		"fuel":"0.4",	// 油耗L
		"miles":"23.2"	// 里程km
	}
	</pre>

## 3.8 /vehicle/{vehicleUid}/parking 历史停靠接口
- HTTP方式：GET
- 版本：0.0.43
- 发布版本：0.0.43
- 输入参数：

	|参数名			|参数类型|是否必须|参数描述	|
	|---------------|----	|----	|----		|
	|startTime		|date	|yes	|查询的时间段	|
	|endTime		|date	|yes	|查询的时间段，若不输入时间段，则默认显示最近3次停车位置	|

- 输出参数

	<pre>
	{
		"total":"4",
		"rows":[{
			"lo":"34.2342",
			"la":"23.2423",
			"duration":"23",		// 停靠时间min
			"startTime":"2015-12-12 12:12",
			"endTime":"2015-12-12 12:45"
		}]
	}
	</pre>

## 3.9 /vehicle/parts/list 车辆配件查询
- HTTP方式：GET
- 版本：无
- 发布版本：无
- 输入参数：无
- 返回参数

	返回的参数包含三层分类，第三层为具体的类型

	<pre>
	[{
		"categoryName":"车身及附件",
		"rows":[{
			"categoryName":"保险杠",
			"parts":[{
				"parstName":"左后装饰条",
				"partUid":"342394832-fcdac"
			},
			{
				"partsName":"右后装饰条",
				"partUid":"342394832-fcdac"
			}]
		},
		{
			"categoryName":"发动机"
			"parts":[{
				"partsName":"左后装饰条",
				"partUid":"342394832-fcdac"
			},
			{
				"partsName":"右后装饰条",
				"partUid":"342394832-fcdac"
			}]
		}]
	},
	{
		"categoryName":"车身及附件",
		"rows":[{
			"categoryName":"保险杠"
			"parts":[{
				"partsName":"左后装饰条",
				"partUid":"342394832-fcdac"
			},
			{
				"partsName":"右后装饰条",
				"partUid":"342394832-fcdac"
			}]
		},
		{
			"categoryName":"发动机"
			"parts":[{
				"partsName":"左后装饰条",
				"partUid":"342394832-fcdac"
			},
			{
				"partsName":"右后装饰条",
				"partUid":"342394832-fcdac"
			}]
		}]
	}
	]
	</pre>


## 3.10 /vehicle/repair/{vehicleUid} 新增车辆维修信息
- HTTP方式：POST
- 版本：v0.0.51
- 发布版本：0.0.39
- 输入参数

	
	|参数名			|参数类型|是否必须|参数描述			|
	|---------------|----	|----	|----				|
	|repairTime		|date	|yes	|维修时间,yyyy-MM-dd	|
	|repairDesc		|String	|yes	|维修描述			|
	|repairPart		|Array	|yes	|配件维修明细	列表		|
	|partName		|String	|yes	|维修配件名称			|
	|count			|int	|yes	|维修配件数量			|
	|partCost		|double	|yes	|配件价格			|
	|laborCost		|double	|yes	|配件维修人工费		|
	|laborCostSum	|double	|yes	|人工费总费用			|
	|partCostSum	|double	|yes	|配件总费用			|
	|repairCostSum	|double	|yes	|维修总费用，单位元	|

	{
		"repairTime":"2015-07-01",
		"repairDesc":"车辆损坏",
		"repairPart":[{
						"partName":"一级分类/二级分类/挡风玻璃",
						"count":"4",
						"partCost":"456",
						"laborCost":"3254"
					  },
					  {
						"partName":"一级分类/二级分类/挡风玻璃",
						"count":"4",
						"partCost":"456",
						"laborCost":"3254"
					  }}
		"laborCostSum":"345"
		"partCostSum":"3245"
		"repairCostSum":"34565"

	}

- 返回参数
	<pre>
	
		{
			"ret":"0",
     		"message":"添加成功"
		}
	</pre>

## 3.10.1 /vehicle/{repairSerial}/repair 维修单修改、删除
- HTTP方式：POST
- 版本：v0.0.51
- 发布版本：0.0.39
- 输入参数

	|参数名			|参数类型|是否必须|参数描述			|
	|---------------|----	|----	|----				|
	|cmd			|String	|yes	|modify:修改<br/>delete：删除|
	|repairTime		|date	|yes	|维修时间,yyyy-MM-dd	|
	|repairDesc		|String	|yes	|维修描述			|
	|repairPart		|Array	|yes	|配件维修明细	列表		|
	|partName		|String	|yes	|维修配件名称			|
	|count			|int	|yes	|维修配件数量			|
	|partCost		|double	|yes	|配件价格			|
	|laborCost		|double	|yes	|配件维修人工费		|
	|laborCostSum	|double	|yes	|人工费总费用			|
	|partCostSum	|double	|yes	|配件总费用			|
	|repairCostSum	|double	|yes	|维修总费用，单位元	|

	注：返回全部信息，包括未修改信息

-返回参数

	<pre>
	
		{
			"ret":"0",
     		"message":"添加成功"
		}
	</pre>

### 3.10.2 /vehicle/{repairSerial}/repair维修单详情获取
- HTTP方式：POST
- 版本：v0.0.51
- 发布版本：0.0.39
-返回参数

	<pre>
	
		{	"vehicleUid":"13435675789798"
			"vehicleLic":"晥AWT668"
			"repairSerial":"2143245664564"
			"repairTime":"2015-07-01",
			"repairDesc":"车辆损坏",
			"repairPart":[{
					"repairUid":"2143253456546"
					"partName":"一级分类/二级分类/挡风玻璃",
					"count":"4",
					"partCost":"456",
					"laborCost":"3254"
					 },
					 {
					"repairUid":"2143253456546"
					"partName":"一级分类/二级分类/挡风玻璃",
					"count":"4",
					"partCost":"456",
					"laborCost":"3254"
					 }}
			"laborCostSum":"345"
			"partCostSum":"3245"
			"repairCostSum":"34565"}
		}
	</pre>
## 3.11 /vehicle/repair/list 车辆维修列表
- HTTP方式：GET
- 版本：v0.0.14
- 发布版本：0.0.39
- 输入参数

	|参数名			|参数类型|是否必须|参数描述	|
	|---------------|----	|----	|----		|
	|vehicleNick	|String	|no		|车辆别名	|
	|vehicleLic		|Stirng	|no		|车牌号|
	|startTime		|date	|no		|查询的时间段，yyyy-MM-dd	|
	|endTime		|date	|no		|查询的时间段	,yyyy-MM-dd|
	|pageStart		|int	|no		|起始页码， 默认1	|
	|pageCount		|int	|no		|每页条数，默认50	|

- 返回参数
	<pre>
	{
		"total":"2",
		"rows":[{
				"vehicleUid":"13435675789798"
				"vehicleLic":"晥AWT668"
				"repairSerial":"2143245664564"
				"repairTime":"2015-07-01",
				"repairDesc":"车辆损坏",
				"repairPart":[{
						"repairUid":"2143253456546"
						"partName":"一级分类/二级分类/挡风玻璃",
						"count":"4",
						"partCost":"456",
						"laborCost":"3254"
					  },
					  {
						"repairUid":"2143253456546"
						"partName":"一级分类/二级分类/挡风玻璃",
						"count":"4",
						"partCost":"456",
						"laborCost":"3254"
					  }}
				"laborCostSum":"345"
				"partCostSum":"3245"
				"repairCostSum":"34565"},
			{"vehicleUid":"13435675789798"
			"vehicleLic":"晥AWT668"
			"repairSerial":"2143245664564"
			"repairTime":"2015-07-01",
			"repairDesc":"车辆损坏",
			"repairPart":[{
						"repairUid":"2143253456546"
						"partName":"一级分类/二级分类/挡风玻璃",
						"count":"4",
						"partCost":"456",
						"laborCost":"3254"
					  },
					  {
						"repairUid":"2143253456546"
						"partName":"一级分类/二级分类/挡风玻璃",
						"count":"4",
						"partCost":"456",
						"laborCost":"3254"
					  }}
		"laborCostSum":"345"
		"partCostSum":"3245"
		"repairCostSum":"34565"
		}]
	}
	</pre>


## 3.12 /vehicle/maintain/{vehicleUid} 车辆保养处理添加
- HTTP方式：POST
- 版本：
- 发布版本
- 输入参数

	|参数名			|参数类型|是否必须|参数描述			|
	|---------------|----	|----	|----				|
	|maintainTime	|date	|yes	|保养时间,yyyy-MM-dd	|
	|maintainDesc	|String	|yes	|维修描述			|
	|maintainPart		|Array	|yes	|配件保养明细列表		|
	|partName		|String	|yes	|保养配件名称			|
	|count			|int	|yes	|保养配件数量			|
	|partCost		|double	|yes	|配件价格			|
	|laborCost		|double	|yes	|配件保养人工费		|
	|laborCostSum	|double	|yes	|人工费总费用			|
	|partCostSum	|double	|yes	|配件总费用			|
	|maintainCostSum|double	|yes	|保养总费用，单位元	|
	|nextMile		|double	|yes	|下次保养里程	|
	|nextTime		|date	|yes	|下次保养时间	,yyyy-MM-dd|


	{
		"maintainTime":"2015-07-01",
		"maintainDesc":"车辆损坏",
		"maintainPart":[{
						"partName":"一级分类/二级分类/挡风玻璃",
						"count":"4",
						"partCost":"456",
						"laborCost":"3254"
					  },
					  {
						"partName":"一级分类/二级分类/挡风玻璃",
						"count":"4",
						"partCost":"456",
						"laborCost":"3254"
					  }}
		"laborCostSum":"345"
		"partCostSum":"3245"
		"maintainCostSum":"34565"
		"nextMile":"1235.6"
		"nextTime":"2015-01-01"

	}

- 返回参数

	<pre>
	{
		"ret":0
		"message":"操作成功"
	}
	</pre>

### 3.12.1 /vehicle/{maintainSerial}/maintain 保养信息删除修改
- HTTP方式：POST
- 版本：
- 发布版本
- 输入参数

	|参数名			|参数类型|是否必须|参数描述			|
	|---------------|----	|----	|----				|
	|cmd			|String	|yes	|操作类型<br/>modify:修改<br/>delete:删除|
	|maintainTime	|date	|yes	|保养时间,yyyy-MM-dd	|
	|maintainDesc	|String	|yes	|维修描述			|
	|maintainPart		|Array	|yes	|配件保养明细列表		|
	|partName		|String	|yes	|保养配件名称			|
	|count			|int	|yes	|保养配件数量			|
	|partCost		|double	|yes	|配件价格			|
	|laborCost		|double	|yes	|配件保养人工费		|
	|laborCostSum	|double	|yes	|人工费总费用			|
	|partCostSum	|double	|yes	|配件总费用			|
	|maintainCostSum|double	|yes	|保养总费用，单位元	|
	|nextMile		|double	|yes	|下次保养里程	|
	|nextTime		|date	|yes	|下次保养时间	,yyyy-MM-dd|
	注：发送全部参数，包括未修改信息，发送格式与创建信息时相同

-返回参数
	{
		"ret":0
		"message":"操作成功"
	}

### 3.12.2 /vehicle/{maintainSerial}/maintain 保养信息获取
- HTTP方式：GET
- 版本：
- 发布版本
- 输入参数
- 返回参数
				{"vehicleUid":"13435675789798"
				"vehicleLic":"晥AWT668"
				"maintainSerial":"2143245664564"
				"maintainTime":"2015-07-01",
				"maintainDesc":"车辆损坏",
				"maintainPart":[{
						"maintainUid":"2143253456546"
						"partName":"一级分类/二级分类/挡风玻璃",
						"count":"4",
						"partCost":"456",
						"laborCost":"3254"
					  },
					  {
						"maintainUid":"2143253456546"
						"partName":"一级分类/二级分类/挡风玻璃",
						"count":"4",
						"partCost":"456",
						"laborCost":"3254"
					  }}
				"laborCostSum":"345"
				"partCostSum":"3245"
				"maintainCostSum":"34565"
				"nextTime":"2015-07-07",
				"nextMile":"234243"}

## 3.13 /vehicle/maintain/list 车辆保养列表
- HTTP方式：GET
- 版本：
- 发布版本：
- 输入参数：
	
	|参数名			|参数类型|是否必须|参数描述		|
	|---------------|----	|----	|----			|
	|vehicleUid		|string	|no		|查询的车辆Uid	|
	|vehicleNick	|String	|no		|车辆别名，若为空则查询所有车辆	|
	|vehicleLic		|Stirng	|no		|车牌号，若为空则查询所有车辆	|
	|startTime		|date	|no		|查询的时间段	|
	|endTime		|date	|no		|查询的时间段	|
	|isHistory		|String	|no		|1：返回车辆的所有信息<br/>0：返回车辆的活动保养信息<br/>默认为1|
	|pageStart		|int	|no		|起始页码， 默认1	|
	|pageCount		|int	|no		|每页条数，默认50	|

- 返回参数

	<pre>
	{
		"total":"2",
		"rows":[{
				"vehicleUid":"13435675789798"
				"vehicleLic":"晥AWT668"
				"maintainSerial":"2143245664564"
				"maintainTime":"2015-07-01",
				"maintainDesc":"车辆损坏",
				"maintainPart":[{
						"maintainUid":"2143253456546"
						"partName":"一级分类/二级分类/挡风玻璃",
						"count":"4",
						"partCost":"456",
						"laborCost":"3254"
					  },
					  {
						"maintainUid":"2143253456546"
						"partName":"一级分类/二级分类/挡风玻璃",
						"count":"4",
						"partCost":"456",
						"laborCost":"3254"
					  }}
				"laborCostSum":"345"
				"partCostSum":"3245"
				"maintainCostSum":"34565"
				"nextTime":"2015-07-07",
				"nextMile":"234243"},
			{"vehicleUid":"13435675789798"
			"vehicleLic":"晥AWT668"
			"maintainSerial":"2143245664564"
			"maintianTime":"2015-07-01",
			"maintainDesc":"车辆损坏",
			"maintainPart":[{
						"maintainUid":"2143253456546"
						"partName":"一级分类/二级分类/挡风玻璃",
						"count":"4",
						"partCost":"456",
						"laborCost":"3254"
					  },
					  {
						"maintainUid":"2143253456546"
						"partName":"一级分类/二级分类/挡风玻璃",
						"count":"4",
						"partCost":"456",
						"laborCost":"3254"
					  }}
			"laborCostSum":"345"
			"partCostSum":"3245"
			"maintainCostSum":"34565"
			"nextTime":"2015-07-07",
			"nextMile":"234243"
			}]
		}
		</pre>

## 3.14 /vehicle/annual/{vehicleUid} 年审处理
- HTTP方式：POST
- 版本：
- 发布版本：
- 输入参数
	
	|参数名			|参数类型|是否必须|参数描述	|
	|---------------|----	|----	|----		|
	|annualTime		|Date	|yes	|年审处理时间，yyyy-MM-dd|
	|result			|string	|yes	|年审结果	|
	|nextTime		|date	|yes	|下次年审到期时间,yyyy-MM-dd|
	|annualFee		|double	|yes	|年审费用，元	|

- 返回参数

	<pre>
	{
		"ret":0
		"message":"操作成功"		
	}
	</pre>

## 3.15 /vehicle/annual/list 年审列表
- HTTP方式：GET
- 版本：无
- 发布版本：无
- 输入参数：

	|参数名			|参数类型|是否必须|参数描述		|
	|---------------|----	|----	|----			|
	|vehicleUid		|string	|no		|查询的车辆Uid	|
	|vehicleNick	|String	|no		|车辆别名，若为空则查询所有车辆	|
	|vehicleLic		|Stirng	|no		|车牌号，若为空则查询所有车辆	|
	|isHistory		|String	|no		|0：显示当前活动年审信息<br/>1：显示历史年审信息<br/>默认为显示所有历史记录|
	|startTime		|date	|no		|查询的时间段		|
	|endTime		|date	|no		|查询的时间段		|
	|pageStart		|int	|no		|起始页码， 默认1	|
	|pageCount		|int	|no		|每页条数，默认50	|

- 返回参数：

	<pre>
	{
		"total":"23",
		"rows:[{
			"annualUid":"21442533"
			"vehicleLic":"皖A3423f"			// 车牌号
			"vehicleUid":"2343facd",		// 车辆UID
			"vehicleType":"奥迪——奥迪A6"			// 车辆类型
			"currentMile":"2323.23"			// 车辆总里程
			"nextTime":"2015-04-23"		// 年审到期时间
			"result":"年审结果"
			"annualTime":"2014-04-23"		// 年审处置时间（显示历史时为当次处理时间）
			"nextTime":"年审到期时间"
			"annualFee":"3435646"
		}]
	}
	</pre>

### 3.15.1 /vehicle/{annualUid}/annual 年审信息修改删除
- HTTP方式：POST
- 版本：无
- 发布版本：无
- 输入参数：
	|参数名			|参数类型|是否必须|参数描述		|
	|---------------|----	|----	|----			|
	|cmd			|string	|yes	|操作类型<br/>modify:删除<br/>delete:删除	|
	|annualTime		|Date	|no		|年审处理时间，yyyy-MM-dd|
	|result			|string	|no		|年审结果	|
	|nextTime		|date	|no		|下次年审到期时间,yyyy-MM-dd|
	|annualFee		|double	|no		|年审费用，元	|
-返回参数：
	{
		"ret":0
		"message":"操作成功"
	}

### 3.15.2 /vehicle/{annualUid}/annual 年审信息获取
- HTTP方式：GET
- 版本：无
- 发布版本：无
- 输入参数：
- 返回参数：
	
	<pre>
	{
			"annualUid":"21442533"
			"vehicleLic":"皖A3423f"			// 车牌号
			"vehicleUid":"2343facd",		// 车辆UID
			"vehicleType":"大众"			// 车辆品牌
			"currentMile":"2323.23"			// 车辆总里程
			"nextTime":"2015-04-23"		// 年审到期时间
			"result":"年审结果"
			"annualTime":"2014-04-23"		// 年审处置时间（显示历史时为当次处理时间）
			"annualFee"："3424"
	}
	</pre>

## 3.16 /vehicle/insu/{vehicleUid} 车辆保险处置新建
- HTTP方式：POST
- 版本：
- 发布版本：
- 输入参数
	
	|参数名			|参数类型|是否必须|参数描述	|
	|---------------|----	|----	|----		|
	|insuTime		|Date	|yes	|保险处理时间	|
	|nextTime			|date	|yes	|下次保险到期时间	|
	|insuFee		|float	|yes	|保险费用	|

- 返回参数

	<pre>
	{
		"ret":0
		"message":"操作成功"		
	}
	</pre>

## 3.17 /vehicle/insu/list 车辆保险查询
- HTTP方式：GET
- 版本：无
- 发布版本：无
- 输入参数：

	|参数名			|参数类型|是否必须|参数描述		|
	|---------------|----	|----	|----			|
	|vehicleUid		|string	|no		|查询的车辆Uid（当没有改参数时，显示所有车辆的保险信息。当指定该参数时，显示该车的历史保险信息）	|
	|startTime		|date	|no		|查询的时间段	|
	|isHistory		|String	|no		|0:显示当前活动信息<br/>2:显示历史信息|
	|endTime		|date	|no		|查询的时间段	|
	|pageStart		|int	|no		|起始页码， 默认1	|
	|pageCount		|int	|no		|每页条数，默认50	|

- 返回参数：

	<pre>
	{
		"total":"23",
		"rows:[{
			"insuUid":"21331425"
			"vehicleLic":"皖A3423f"			// 车牌号
			"vehicleUid":"2343facd",		// 车辆UID
			"vehicleType":"大众"			    // 车辆品牌
			"currentMile":"2323.23"			// 车辆总里程
			"insuTime":"2015-04-01"			//当次处置时间
			"nextTime":"2015-04-23"			// 保险到期时间
			"insuFee":"124"
		}]
	}
	</pre>

### 3.18.1 /vehicle/{insuUid}/insu 保险信息修改删除
- HTTP方式：POST
- 版本：无
- 发布版本：无
- 输入参数：
	|参数名			|参数类型|是否必须|参数描述		|
	|---------------|----	|----	|----			|
	|cmd			|string	|yes	|操作类型<br/>modify:删除<br/>delete:删除	|
	|insuTime		|Date	|no		|保险处理时间，yyyy-MM-dd|
	|nextTime		|date	|no		|下次保险到期时间,yyyy-MM-dd|
	|insuFee		|double	|no		|保险费用，元	|
-返回参数：
	{
		"ret":0
		"message":"操作成功"
	}

### 3.18.2 /vehicle/{insuUid}/insu 保险信息获取
- HTTP方式：GET
- 版本：无
- 发布版本：无
- 输入参数：
- 返回参数：
	
	<pre>
	{
			"insuUid":"21442533"
			"vehicleLic":"皖A3423f"			// 车牌号
			"vehicleUid":"2343facd",		// 车辆UID
			"vehicleBrand":"大众"			// 车辆品牌
			"vehicleModel":"速腾"			// 车辆型号
			"currentMile":"2323.23"			// 车辆总里程
			"nextTime":"2015-04-23"		// 保险到期时间
			"insuTime":"2014-04-23"		// 保险处置时间（显示历史时为当次处理时间）
			"insuFee"："3424"
	}
	</pre>


## 3.19 /vehicle/seal/{vehicleUid} 封存登记
- HTTP方式：POST
- 版本：无
- 发布版本：无
- 输入参数：
	
	|参数名			|参数类型|是否必须|参数描述	|
	|---------------|----	|----	|----		|
    |sealTime		|date   |yes	|封存日期    |
    |approveDept    |string |yes    |批准单位    |
	|operator       |string |yes	|经办人     	|
	|reason         |string |yes	|原因       	|
- 返回参数

	<pre>
	{
		"ret":0
		"message":"操作成功"
	}
	</pre>

### 3.20 /vehicle/seal/list
- HTTP方式：POST
- 版本：无
- 发布版本：无
- 输入参数：
	
	|参数名			|参数类型|是否必须|参数描述	|
	|---------------|----	|----	|----		|
	|vehicleNick	|String	|no		|车辆别名，若为空则查询所有车辆	|
	|vehicleLic		|Stirng	|no		|车牌号，若为空则查询所有车辆	|
	|startTime		|date	|no		|查询的时间段		|
	|endTime		|date	|no		|查询的时间段		|
	|pageStart		|int	|no		|起始页码， 默认1	|
	|pageCount		|int	|no		|每页条数，默认50	|
- 返回参数

	<pre>
	{"total":12,
	"rows":[{ "sealUid":"2343facd",		// 封存UID
			"vehicleUid":"2343facd",		// 车辆UID
			"vehicleLic":"皖A3423f"			// 车牌号
			"vehicleNick":"车辆别名"
			"vehicleType":"速腾"			    // 车辆型号
			"currentMile":"2323.23"			// 车辆总里程
			"approveDept":"综合管理部"		//
			"operator":"王强"			    // 
			"sealStatus":"0"					// 封存启用状态，0：启用，1：封存
			"sealDate":"2015-07-16"	    // 封存日期
     		"unSealDate":"2015-07-22"	    // 启用日期},
			{ "sealUid":"2343facd",		// 封存UID
			"vehicleUid":"2343facd",		// 车辆UID
			"vehicleLic":"皖A3423f"			// 车牌号
			"vehicleNick":"车辆别名"
			"vehicleType":"速腾"			    // 车辆型号
			"currentMile":"2323.23"			// 车辆总里程
			"approveDept":"综合管理部"		//
			"operator":"王强"			    // 
			"sealStatus":"0"					// 封存启用状态，0：启用，1：封存
			"reserveDate":"2015-07-16"	    // 封存日期
     		"unsealDate":"2015-07-22"}]}	    // 启用日期
			
	</pre>

### 3.21.1 /vehicle/{sealUid}/seal 封存启用操作
- HTTP方式：POST
- 版本：无
- 发布版本：无
- 输入参数：
	
	|参数名			|参数类型|是否必须|参数描述	|
	|---------------|----	|----	|----		|
    |cmd        	|string |yes	|enable:启用	<br/>delete删除|
    
- 返回参数

	<pre>
	{
		"ret":0
		"message":"操作成功"
	}
	</pre>



### 3.21.2 /vehicle/{sealUid}/seal 获取封存启用详情
- HTTP方式：GET
- 版本：无
- 发布版本：无
- 输入参数
- 返回参数

	<pre>
	{
	        "sealUid":"2343facd",		// 封存UID
			"vehicleUid":"2343facd",		// 车辆UID
			"vehicleLic":"皖A3423f"			// 车牌号
			"vehicleNick":"车辆别名"
			"vehicleType":"速腾"			    // 车辆型号
			"currentMile":"2323.23"			// 车辆总里程
			"approveDept":"综合管理部"		//
			"operator":"王强"			    // 
			"sealStatus":"0"					// 封存启用状态，0：启用，1：封存
			"sealDate":"2015-07-16"	    // 封存日期
     		"unsealDate":"2015-07-22"	    // 启用日期
			"reason":"afsfsfgf"
	}
	</pre>

## 3.22 /vehicle/dispose/{vehicleUid} 处置新增
- HTTP方式：POST
- 版本：无
- 发布版本：无
- 输入参数
	
	|参数名			|参数类型|是否必须|参数描述	|
	|---------------|----	|----	|----		|
	|disposeTime	|string	|yes	|处置日期    |
	|disposeType	|string	|yes	|处置类型：1:出售，2:报废，3:划拨|
	|disposeCost	|string	|yes	|处置金额     |
	|receiveDept	|string	|yes	|接收单位     |
	|receivePerson	|string	|yes	|接收人       |
	|approveDept	|string	|yes	|处置单位     |
	|operator       |string |yes    |经办人       |
	|reason			|string	|yes	|原因         |
- 返回参数
	
	<pre>
	{
		"ret":0
		"message":"操作成功"
	}
	</pre>

## 3.23 /vehicle/dispose/list 获取处置信息列表
- HTTP方式：POST
- 版本：无
- 发布版本：无
- 输入参数：
	
	|参数名			|参数类型|是否必须|参数描述	|
	|---------------|----	|----	|----		|
	|vehicleNick	|String	|no		|车辆别名，若为空则查询所有车辆	|
	|vehicleLic		|Stirng	|no		|车牌号，若为空则查询所有车辆	|
	|startTime		|date	|no		|查询的时间段		|
	|endTime		|date	|no		|查询的时间段		|
	|pageStart		|int	|no		|起始页码， 默认1	|
	|pageCount		|int	|no		|每页条数，默认50	|
- 返回参数

	<pre>
	{"total":12,
	"rows":[{ "disposeUid":"2343facd",		// 处置UID
			"vehicleUid":"2343facd",		// 车辆UID
			"vehicleLic":"皖A3423f"			// 车牌号
			"vehicleNick":"车辆别名"
			"vehicleType":"速腾"			    // 车辆型号
			"currentMile":"2323.23"			// 车辆总里程
			"disposeTime":"2015-01-01"
			"disposeType":"3",
			"disposeCost":"32324"
			"receiveDept":"车管部"
			"receivePerson":"张三"
			"approveDept":"综合管理部"		//
			"operator":"王强"			    // 
			{"disposeUid":"2343facd",		// 处置UID
			"vehicleUid":"2343facd",		// 车辆UID
			"vehicleLic":"皖A3423f"			// 车牌号
			"vehicleNick":"车辆别名"
			"vehicleType":"速腾"			    // 车辆型号
			"currentMile":"2323.23"			// 车辆总里程
			"disposeTime":"2015-01-01"
			"disposeType":"3",
			"disposeCost":"32324"
			"receiveDept":"车管部"
			"receivePerson":"张三"
			"approveDept":"综合管理部"		//
			"operator":"王强"			    // 
			
	</pre>

### 3.19.1 /vehicle/dispose/{disposeUid} 处置修改、删除
- HTTP方式：POST
- 版本：无
- 发布版本：无
- 输入参数
	
	|参数名			|参数类型|是否必须|参数描述	|
	|---------------|----	|----	|----		|
	|cmd            |string |yes    |modify:修改，delete：删除(当为delete时，不需要传以下参数)|
	|currentMile	|string	|yes	|累计里程    |
	|disposeTime	|string	|yes	|处置日期    |
	|disposeType	|string	|yes	|处置类型：出售，报废，划拨|
	|disposeCost	|string	|yes	|处置金额     |
	|receiveDept	|string	|yes	|接收单位     |
	|receivePerson	|string	|yes	|接收人       |
	|approveDept	|string	|yes	|处置单位     |
	|operator       |string |yes    |经办人       |
	|reason			|string	|yes	|原因         |
- 返回参数

	<pre>
	{
		"ret":0
		"message":"操作成功"
	}
	</pre>

### 3.19.2 /vehicle/dispose/{disposeUid} 查询单条处置信息
- HTTP方式：GET
- 版本：无
- 发布版本：无
- 输入参数：无
- 返回参数

	<pre>
	{
		"disposeUid":"80e2ef0-30e1-40a2-9d5f-b7083e08ccfb",
		"currentMile":"234",
		"disposeDate":"2015-07-15",
		"disposeType":"报废",
		"disposeCost":"2000",
		"receiveDept":"蜀山维修厂",
		"receivePerson":"王五",
		"approveDept":"综合管理部",
		"operator":"王强",
		"reason":"超过20年"
	}
	</pre>

## 3.19.3 /vehicle/dispose/list
- HTTP方式：GET
- 版本：无
- 发布版本：无
- 输入参数
	
	|参数名			|参数类型|是否必须|参数描述		|
	|---------------|----	|----	|----			|
	|vehicleLic		|string	|no		|查询车牌号	|
	|vehicleNick	|string	|no		|查询车辆别名	|
	|startTime		|date	|no		|查询的时间段	|
	|endTime		|date	|no		|查询的时间段	|
	|pageStart		|int	|no		|起始页码， 默认1	|
	|pageCount		|int	|no		|每页条数，默认50	|

- 返回参数

	<pre>
	{
		"total":"23",
		"rows:[{
			"disposeUid":"80e2ef0-30e1-40a2-9d5f-b7083e08ccfb",
			"currentMile":"234",
			"disposeDate":"2015-07-15",
			"disposeType":"报废",
			"disposeCost":"2000",
			"receiveDept":"蜀山维修厂",
			"receivePerson":"王五",
			"approveDept":"综合管理部",
			"operator":"王强",
			"reason":"超过20年"
		}，{
			"disposeUid":"80e2ef0-30e1-40a2-9d5f-b7083e08ccfb",
			"currentMile":"234",
			"disposeDate":"2015-07-15",
			"disposeType":"报废",
			"disposeCost":"2000",
			"receiveDept":"蜀山维修厂",
			"receivePerson":"王五",
			"approveDept":"综合管理部",
			"operator":"王强",
			"reason":"超过20年"
		}]
	}
	</pre>

## 3.20 /vehicle/{vehicleUid}/accident 
- HTTP方式：POST
- 版本：无
- 发布版本：无
- 输入参数
	
	|参数名			|参数类型|是否必须|参数描述	|
	|---------------|----	|----	|----		|
	|accidentType	|string	|yes	|事故类型    |
	|accidentAddr	|string	|yes	|事故地点    |
	|accidentEvent	|string	|yes	|事故事件    |
	|accidentType	|string	|no 	|事故描述    |
	|driverName 	|string	|no 	|驾驶员      |
	|accidentCost	|string	|no 	|费用        |
	|operator		|string	|no 	|经办人      |
- 返回参数

	<pre>
	{
		"ret":0
		"message":"操作成功"
	}
	</pre>
事故类型，事故地点，事故事件，事故描述

## 3.21 /vehicle/{vehicleUid}/match 人车绑定
- HTTP方式：POST
- 版本：无
- 发布版本：无
- 输入参数
	
	|参数名			|参数类型|是否必须|参数描述	|
	|---------------|----	|----	|----		|
	|driverUid		|string	|yes	|司机UID    	|
	|vehicleUid		|String	|yes	|车辆uid		|

- 返回参数

	<pre>
	{
		"ret":0
		"message":"操作成功"
	}
	</pre>
## 3.22 /vehicle/match/list 人车绑定列表
- HTTP方式：POST
- 版本：无
- 发布版本：无
- 输入参数
	
	|参数名			|参数类型|是否必须|参数描述	|
	|---------------|----	|----	|----		|
	|driverName		|String	|no		|司机姓名	|
	|vehicleLic		|String	|no		|车辆车牌号	|
	|vehicleNick	|String	|no		|车辆别名	|
	|startTime		|Date	|no		|开始时间，yyyy-MM-dd	|
	|endTime		|Date	|no		|结束时间。yyyy-MM-dd	|
	|pageStart		|String	|no		|起始页，默认为1|
	|pageCount		|String	|no		|单页数量，默认为50|

- 返回参数

	<pre>
	{
		"total":12
		"rows":[{"matchUid":"12343245667889",
				"vehicleLic":"晥A12345",
				"vhehicleNick":"公车001",
				"driverName":"张三",
				"loginId":"WT0023"，
				"bindTime":"2014-01-01"},
				{"matchUid":"12343245667889",
				"vehicleLic":"晥A12345",
				"vhehicleNick":"公车001",
				"driverName":"张三",
				"loginId":"WT0023"，
				"bindTime":"2014-01-01"}]
	}
	</pre>

## 3.22 /vehicle/match/{matchUid} 人车绑定解绑
- HTTP方式：POST
- 版本：无
- 发布版本：无
- 输入参数
- 返回参数
	{
		"ret":1;
		"message":"操作成功"
	
	}

## 3.23 /vehicle/rescue 车辆救援
- HTTP方式：POST
- 版本：无
- 发布版本：无
- 输入参数
	
	|参数名			|参数类型|是否必须|参数描述	|
	|---------------|----	|----	|----		|
	|vehicleLic		|String	|no		|司机姓名	|
	|vehicleLic		|String	|no		|车辆车牌号	|
	|vehicleNick	|String	|no		|车辆别名	|
	|startTime		|Date	|no		|开始时间，yyyy-MM-dd	|
- 返回参数
	{
		"ret":1;
		"message":"操作成功"
	
	}

# 4 驾驶员管理

## 4.1 /driver 注册驾驶员
- HTTP方式：POST
- 版本：v 0.0.44
- 发布版本：0.0.39
- 输入参数 

	|参数名			|参数类型|是否必须|参数描述	|
	|---------------|----	|----	|----		|
	|deptUid		|string	|yes	|驾驶员所属部门|
	|staffCode		|string	|yes	|职工号		|
	|loginId		|string	|yes	|驾驶员登录账号名|
	|password		|string	|yes	|初始密码	|
	|realName		|string	|yes	|驾驶员真实姓名	|
	|idNumber		|string	|yes	|身份证号	|
	|phone       	|string	|yes	|手机号		|
	|licFirst		|string	|yes	|初次领证日期|
	|approvalType	|string	|yes	|准驾车型	|
	|licNumber		|string	|yes	|驾照号码	|
	|gender			|int	|yes	|性别：1：男<br/>2：女|
	|picUrl			|string	|no		|照片		|
	|nameUsed		|string	|no		|曾用名		|

- 返回参数

		{
			"driverUid":"393843945aafcd34324"
		}

## 4.2 /driver/list 驾驶员查询
- HTTP方式：GET
- 版本：v 0.0.40
- 发布版本：0.0.39
- 输入参数

	|参数名			|参数类型|是否必须|参数描述	|
	|---------------|----	|----	|----		|
	|startTime		|date	|no		|开始时间	|
	|endTime		|date	|no		|结束时间	|
	|status			|int	|no		|1：空闲 <br /> 2：工作中 |
	|matchName		|string	|no		|名字匹配	|
	|matchStaffCode	|string	|no		|工号匹配	|
	|pageStart		|int	|no		|起始页码， 默认1	|
	|pageCount		|int	|no		|每页条数，默认50	|
	|vehicleUid		|String	|no		|给定此字段时，返回结果的前几条为该车最近匹配司机结果，最多为前三条|

- 返回参数

	<pre>
	[{
		"driverUid":"345642acedf32a8ce7394263cafe2938",
		"userUid":"345642acedf32a8ce7394263cafe2938",
		"driverName":"张三",
		"phone":"18966186789",
		"approvalType":"C1",
		"status":"1" //1:空闲；2：工作中
	},{
		"driverUid":"345642acedf32a8ce7394263cafe2938",
		"userUid":"345642acedf32a8ce7394263cafe2938",
		"driverName":"张四",
		"phone":"18966186789",
		"approvalType":"C1", // 驾驶证准驾车型（v0.0.40修改）
		"status":"1" //1:空闲；2：工作中
	}]
	</pre>

## 4.3 /driver/{driverUid} 驾驶员修改、删除接口定义
- HTTP方式：POST
- 版本：无
- 发布版本：
- 输入参数

	|参数名			|参数类型|是否必须|参数描述	|
	|---------------|----	|----	|----		|
	|cmd			|String	|yes	|modify:修改信息<br/>delete：删除
	|staffCode		|string	|no		|职工号		|
	|realName		|string	|no		|驾驶员真实姓名	|
	|phone       	|string	|no		|手机号		|
	|licFirst		|date	|no		|初次领证日期,yyyy-MM-dd|
	|approvalType	|string	|no		|准驾车型	|
	|licNumber		|string	|no		|驾照号码	|
	|idNumber		|string	|no		|身份证号	|
	|deptUid		|String	|no		|需要修改的部门uid|

- 返回参数
	{
		"ret":0,
		"message":"操作	成功"
	}

### 4.3.1 /driver/{driverUid} 驾驶员信息查询接口
- HTTP方式：GET
- 版本：无
- 输入参数:无
- 返回参数
	{
		"driverUid":"123454567",
		"deptUid":"13aa3385-2838-11e5-8788-005056ac2c83",
		"deptName":"信息化部",
		"loginId":"WT0010",
		"realName":"张三",
		"phone":"15012341234",
		"idNumber":"身份证号",
		"gender":"1",
		"photo":"http://123.123.123.123",
		"staffCode":"1234",
		"licNumber":"12321321312",
		"licFirst":"2012-01-01",
		"approvalType":"A1"
	}

## 4.4 /driver/{driverUid}/annual 驾驶证年审处理
- HTTP方式：POST
- 版本：无
- 发布版本：无
- 输入参数

	|参数名			|参数类型|是否必须|参数描述	|
	|---------------|----	|----	|----		|
	|result			|string	|yes	|年审结果	|
	|next			|date	|yes	|下次年审到期时间	|

- 返回参数

	
	{
		"ret":0,
		"message":"操作成功"
	}

## 4.5 /driver/lesson 驾驶员安全学习

- HTTP方式：POST
- 版本：无
- 发布版本：无
- 输入参数
名称，时间，参加人数，地点，主持单位，主持人，记录人，学习内容，
参加人员，学习记录

# 5 部门管理

## 5.1 /dept 新增部门接口定义
- HTTP方式：POST
- 版本：v0.0.43
- 发布版本：无
- 输入参数

	|参数名			|参数类型|是否必须|	参数描述		|
	|---- 			| --- 	| ------| ---- 			|
	|pdeptUid		|string	|	是	|父部门id，如果是根节点，约定它的父节点id为0 |
	|deptName		|string	|	是	|部门名称		|
	|deptShortName	|string	|no		|部门简称		|
	|orgCode	    |string	|no		|企业编码，在新增企业节点时不能为空|
	|orderNo		|int	|	是	|部门排序号		|

- 返回参数

		{
			"ret":"0",
     		"deptUid":"345642acedf32a8ce7394263cafe2938"
		}

## 5.2 /dept/{deptUid} 查询、修改、删除部门接口定义
- HTTP方式：POST
- 版本：v0.0.43
- 发布版本：0.0.39
- 输入参数

	|参数名			|参数类型|是否必须|	参数描述		|
	|---- 			| --- 	| ------| ---- 			|
	|cmd			|string	|no		|get:可为空，获取部门信息 <br /> modify：不可为空，修改部门信息 <br/> delete: 不可为空，删除部门及子部门
	|deptName		|string	|否		|部门名称，当为modify时不能为空|
	|orderNo		|int	|否		|部门排序号，当为modify时不能为空|
	
- 返回参数
	- 当为get时返回：
	<pre>
	{
		"deptUid":"345642acedf32a8ce7394263cafe2938",
		"deptName":"研发部",
		"orgCode":"ahwtkj",
		"deptShortName":"研发部",
		"orderNo":"1",
		"pdeptUid":"345642acedf32a8ce7394263cafe2938"
	}
	</pre>
	- 当为modify和delete时返回：
	<pre>
	{
		"ret":"0",
		"message":"操作成功"
	}
	</pre>

## 5.3 /dept/{deptUid}/children 获取子部门列表
- HTTP方式：GET
- 说明：当deptUid为0时返回顶层列表
- 版本：v0.0.14
- 发布版本：0.0.39
- 输入参数

	无

- 返回参数

	<pre>
	{
		"total":"3”,
		"rows":
		[{
			"deptUid":"345642acedf32a8ce7394263cafe2938",
			"deptName":"车辆管理研发部",
			"deptShortName":"研发部"
			"orderNo":"1",
			"pdeptUid":"345642acedf32a8ce7394263cafe2938"
		 },{
			"deptUid":"345642acedf32a8ce7394263cafe2938",
			"deptName":"车辆管理研发部",
			"deptShortName":"研发部"
			"orderNo":"1",
			"pdeptUid":"345642acedf32a8ce7394263cafe2938"
		 },{
			"deptUid":"345642acedf32a8ce7394263cafe2938",
			"deptName":"车辆管理研发部",
			"deptShortName":"研发部"
			"orderNo":"1",
			"pdeptUid":"345642acedf32a8ce7394263cafe2938"
		 }
		]
	</pre>

# 5.4 /dept/tree 获取组织架构树
- HTTP方式：GET
- 版本：v0.0.43
- 发布版本：0.0.39
- 输入参数：

	|参数名			|参数类型|是否必须|	参数描述		|
	|---- 			| --- 	| ------| ---- 			|
	|type			|string	|yes	| vehicle：车辆架构树 <br /> driver：司机架构树 <br/> dept:部门架构表<br/>user:用户架构树

- 返回参数

	<pre>
	[{
		"deptUid":"345642acedf32a8ce7394263cafe2938",
		"deptName":"车辆管理研发部",
		"deptShortName":"研发部"
		"orderNo":"1",
		"pdeptUid":"345642acedf32a8ce7394263cafe2938" //父级Uid
		"children": [
			{
				"deptUid":"345642acedf32a8ce7394263cafe2938",
				"deptName":"车辆管理研发部",
				"deptShortName":"研发部"
				"orderNo":"1",
				"pdeptUid":"345642acedf32a8ce7394263cafe2938"
			},
			{
				"vehicleUid": "30928340932840afed",
				"vehicleNickName": "商务部2",
				"vehicleLic":"皖A44532",
				"vehicleAvatar":"http://343.213.432.534/car.png",
				"vehicleType":"别克商务车",
				"status":"2",	// 1：运动中；2：静止；3：离线
				"seatNum":"4",
				"isShow":1
			}
		]
	},
	{
		"deptUid":"345642acedf32a8ce7394263cafe2938",
		"deptName":"车辆管理研发部",
		"deptShortName":"研发部"
		"orderNo":"1",
		"pdeptUid":"345642acedf32a8ce7394263cafe2938"
	}]
	</pre>

	type=driver时，返回参数

	<pre>
	[{
		"deptUid":"345642acedf32a8ce7394263cafe2938",
		"deptName":"车辆管理研发部",
		"deptShortName":"研发部"
		"orderNo":"1",
		"pdeptUid":"345642acedf32a8ce7394263cafe2938" //父级Uid
		"children": [
			{
				"deptUid":"345642acedf32a8ce7394263cafe2938",
				"deptName":"车辆管理研发部",
				"deptShortName":"研发部"
				"orderNo":"1",
				"pdeptUid":"345642acedf32a8ce7394263cafe2938"
			},
			{
				"driverUid":"345642acedf32a8ce7394263cafe2938",
				"userUid":"345642acedf32a8ce7394263cafe2938",
				"driverName":"张三",
				"phone":"18966186789",
				"approvalType":"C1",
				"status":"1" //1:空闲；2：工作中
			}
		]
	},
	{
		"deptUid":"345642acedf32a8ce7394263cafe2938",
		"deptName":"车辆管理研发部",
		"deptShortName":"研发部"
		"orderNo":"1",
		"pdeptUid":"345642acedf32a8ce7394263cafe2938"
	}]
	</pre>

	type=user时，返回参数

	<pre>
	[{
		"deptUid":"345642acedf32a8ce7394263cafe2938",
		"deptName":"车辆管理研发部",
		"deptShortName":"研发部"
		"orderNo":"1",
		"pdeptUid":"345642acedf32a8ce7394263cafe2938" //父级Uid
		"children": [
			{
				"deptUid":"345642acedf32a8ce7394263cafe2938",
				"deptName":"车辆管理研发部",
				"deptShortName":"研发部"
				"orderNo":"1",
				"pdeptUid":"345642acedf32a8ce7394263cafe2938"
			},
			{
				"deptUid":"345642acedf32a8ce7394263cafe2938",
				"userName":"张三",
				"loginId":"user"
				"phone":"18966186789",
			}
		]
	},
	{
		"deptUid":"345642acedf32a8ce7394263cafe2938",
		"deptName":"车辆管理研发部",
		"deptShortName":"研发部"
		"orderNo":"1",
		"pdeptUid":"345642acedf32a8ce7394263cafe2938"
	}]
	</pre>

# 6.提醒系统

## 6.1 /reminder/list 提醒设置列表获取
- HTTP方式：GET
- 版本：0.0.39
- 发布版本：0.0.39
- 输入参数

	无

- 返回参数

	<pre>
	{
		"total":"10",
		"rows":[
			{
				"remindUid":"7329874923402938acd",
				"text":"时空报警",
				"checked":"false"
			},
			{
				"remindUid":"7329874923402938acd",
				"text":"超速告警",
				"checked":"false"
			},
			{
				"remindUid":"7329874923402938acd",
				"text":"低电压",
				"checked":"false"
			},
			{
				"remindUid":"7329874923402938acd",
				"text":"水温告警",
				"checked":"false"
			},
			{
				"remindUid":"7329874923402938acd",
				"text":"OBD告警",   // 插入，拔出
				"checked":"false"
			},
			{
				"remindUid":"7329874923402938acd",
				"text":"疲劳驾驶",
				"checked":"false"
			},
			{
				"remindUid":"7329874923402938acd",
				"text":"非调度用车",
				"checked":"false"
			},
			{
				"remindUid":"7329874923402938acd",
				"text":"绕道提醒",
				"checked":"false"
			},
			{
				"remindUid":"7329874923402938acd",
				"text":"事故提醒",
				"checked":"false"
			},
			{
				"remindUid":"7329874923402938acd",
				"text":"费用异常提醒",
				"checked":"false"
			},
			{
				"remindUid":"7329874923402938acd",
				"text":"年审提醒",
				"checked":"false"
			},
			{
				"remindUid":"7329874923402938acd",
				"text":"行驶证到期",
				"checked":"false"
			},
			{
				"remindUid":"7329874923402938acd",
				"text":"驾驶证到期",
				"checked":"false"
			},
			{
				"remindUid":"7329874923402938acd",
				"text":"保险提醒",
				"checked":"false"
			},
			{
				"remindUid":"7329874923402938acd", 
				"text":"年限到期提醒",
				"checked":"false"
			},
			{
				"remindUid":"7329874923402938acd",
				"text":"故障提醒",
				"checked":"false"
			},
			{
				"remindUid":"7329874923402938acd",
				"text":"违章",
				"checked":"false"
			}
		]
	}
	</pre>

## 6.2 /reminder 提醒设置
- HTTP方式：POST
- 版本：0.0.39
- 发布版本：0.0.39
- 输入参数

	|参数名			|参数类型|是否必须|	参数描述		|
	|---- 			| --- 	| ------| ---- 			|
	|remindUid		|string	|yes	|该设置的Uid		|
	|checked		|boolean|yes	|提醒值			|

	<pre>
	{
		"remindUid":"8723823948afedc",
		"checked":false
	}
	</pre>

- 返回参数

	<pre>
	{
		"ret":"0",
		"message":"操作成功"
	}
	</pre>

## 6.3 /message/count 获取用户未读消息数量
- HTTP方式：GET
- 版本：无
- 发布版本：无
- 输入参数

	无

- 输出参数

	<pre>
	{
		"1":"234"		异常用车	
		"2":"32"		故障维修
		"3":"34"		维修保险年审
		"4":"123"		待派车   对用orderlist接口status=2
		"5":"12"		已派车	status=3
		"6":"123"		在途中	status=4
	
	}
	</pre>

## 6.4 /message/list 获取用户所有消息列表
- HTTP方式：GET
- 版本：无
- 发布版本：无
- 输入参数
	|参数名			|参数类型|是否必须|	参数描述		|
	|---- 			| --- 	| ------| ---- 			|
	|messageType	|string	|no		|需要获取消息种类<br/>1:异常用车<br/>2:故障维修<br/>3:维修保险年审<br/>不发送该参数表示获取所有信息	|
	|pageStart		|String	|no		|开始页数，默认为1|
	|pageCount		|String	|no		|单页数量，默认50	|
	

- 输出参数

	<pre>												
			{	"total":2434，
				"rows"[
						{"messageUid"："23435364",
						"remindType":"超速",			
						"messageType":"异常用车".
						"vehicleLic":"皖A12455"，
						"isRead":"1"},					是否已读，0，未读，1，已读
						{"messageUid"："23435364",
						"remindType":"超速",
						"messageType":"异常用车".
						"vehicleLic":"皖A12455"，
						"isRead":"1"},
						{"messageUid"："23435364",
						"remindType":"超速",
						"messageType":"异常用车".
						"vehicleLic":"皖A12455"，
						"isRead":"1"}
					  ]
			}
	</pre>

## 6.5 /message/{messageUid} 获取用户消息详细信息
- HTTP方式：GET
- 版本：无
- 发布版本：无
- 输入参数、
- 输出参数

	<pre>
	{
		"messageUid":"312343676898",
		"vehicleLic":"皖A12345",
		"remindType":"年审到期",
		"text":"年审到期，请尽快更新"	
		
	}
	</pre>

## 6.6 /area 添加区域管理
- HTTP方式：POST
- 版本：0.0.43
- 发布版本：无
- 输入参数

	|参数名			|参数类型|是否必须|	参数描述		|
	|---- 			| --- 	| ------| ---- 			|
	|name			|string	|yes	|区域名称		|
	|startTime		|time	|no		|每天的起始时间 HH:mm		|
	|endTime		|time	|no		|监控的每日结束时间	HH:mm	|
	|trigger		|int	|yes	|0:驶出触发<br/> 1:驶入触发 <br /> 2:驶入驶出触发
	|type			|int	|yes	|区域类型，0: 矩形<br /> 1:圆形，圆心和半径表示<br />2:多边形
	|points			|array	|yes	|区域表示点，GPS坐标点数组	|
	|radius			|int	|no		|当类型为圆时，该参数必须,无则发送null	|
	|vehicles		|array	|no		|监控车辆的Uid列表，若为空则该监控区域不监视任何车辆 	|
	|startDate		|date	|no		|开始日期，yyyy-MM-dd|
	|endDate		|date	|no		|结束日期，yyyy-MM-dd|
	|monitorType	|String	|no		|监控类型|

- 返回参数
	
	<pre>
	{
		"areaUid":"fd3422-342324"	// 所创建区域的uid
	}
	</pre>

## 6.7 /area/list 获取监控区域的列表
- HTTP方式：GET
- 版本：0.0.39
- 发布版本：无
- 权限：车辆管理员
- 输入参数

	无

- 返回参数

	<pre>
	{
		"total":"4",
		"rows":[
			{
				"areaUid":"30928340932840afed",
				"name":"合肥区域"
				"startTime":"23:20",
				"endTime":"23:50"
				"trigger":"0"
				"type":"0"
				"startDate":"2015-07-07",
				"endDate":"2015-07-09",
				"monitorType":"2",
				"points":[{
					"la":"23.123"
					"lo":"12.242"
				},{
					"la":"23.123"
					"lo":"12.242"
				}],
				"vehicles":[{
					"vehicleUid": "30928340932840afed",   //车辆	uid
					"vehicleNickName": "商务部2",		//车辆别名
					"vehicleLic":"皖A44532",				//车牌号
					"vehicleAvatar":"http://343.213.432.534/car.png",	//车辆照片地址
					"vehicleType":"别克商务车",			//车辆类型
				},{
					"vehicleUid": "30928340932840afed",   //车辆	uid
					"vehicleNickName": "商务部2",		//车辆别名
					"vehicleLic":"皖A44532",				//车牌号
					"vehicleAvatar":"http://343.213.432.534/car.png",	//车辆照片地址
					"vehicleType":"别克商务车",			//车辆类型
				}]
			}
		]
	}
	</pre>

## 6.8 /area/{areaUid} 修改区域列表信息
- HTTP方式：POST
- 版本：0.0.39
- 发布版本：无
- 权限：车辆管理员
- 输入参数
	
	|参数名			|参数类型|是否必须|	参数描述					|
	|---- 			| --- 	| ------| ---- 						|
	|startTime		|time	|no		|每天的起始时间 HH:mm			|
	|endTime		|time	|no		|监控的每日结束时间	HH:mm	|
	|trigger		|int	|no		|0:驶出触发<br/> 1:驶入触发 <br /> 2:驶入驶出触发
	|type			|int	|no		|区域类型，0: 矩形，多边形 <br /> 1:圆形，以中心点和在该圆上的任意一点表示
	|points			|array	|no		|区域表示点，GPS坐标点数组	|
	|name			|String |no		|名字					|
	|radius			|float	|no		|当类型为圆时，该参数必须	|
	|vehicles		|array	|no		|监控车辆的Uid列表，若为空则该监控区域不监视任何车辆
	|startDate		|date	|no		|开始日期，yyyy-MM-dd|
	|endDate		|date	|no		|结束日期，yyyy-MM-dd|
	|monitorType	|String	|no		|监控类型|

- 返回参数

	<pre>
	{
		"ret":"0",
		"message":"操作成功"
	}
	</pre>

### 6.8.1 /area/{areaUid} 获取区域信息
- HTTP方式：GET
- 版本：0.0.39
- 发布版本：无
- 权限：车辆管理员
- 输入参数：无

- 返回参数

	<pre>
	{"areaUid":"30928340932840afed",
	"name":"合肥区域"
	"startTime":"23:20",
	"endTime":"23:50"
	"trigger":"0"
	"type":"0"
	"startDate":"2015-07-07",
	"endDate":"2015-07-09",
	"monitorType":"2",
	"points":[{
				"la":"23.123"
				"lo":"12.242"
			},{
				"la":"23.123"
				"lo":"12.242"
			}],
	"vehicles":[{
			"vehicleUid": "30928340932840afed",   //车辆	uid
			"vehicleNickName": "商务部2",		//车辆别名
			"vehicleLic":"皖A44532",				//车牌号
			"vehicleAvatar":"http://343.213.432.534/car.png",	//车辆照片地址
			"vehicleType":"别克商务车",			//车辆类型
		},{
			"vehicleUid": "30928340932840afed",   //车辆	uid
			"vehicleNickName": "商务部2",		//车辆别名
			"vehicleLic":"皖A44532",				//车牌号
			"vehicleAvatar":"http://343.213.432.534/car.png",	//车辆照片地址
			"vehicleType":"别克商务车",			//车辆类型
		}
	}
	</pre>

# 7. 系统管理

## 7.1 /odb 终端设备管理添加与绑定、解绑
- HTTP方式：POST
- 版本：0.0.52
- 发布版本：0.0.52
- 输入参数
	
	|参数名			|参数类型|是否必须|	参数描述				|
	|---- 			| --- 	| ------| ---- 					|
	|obdUid			|String	|no		|obdUid	,添加时无需该参数，当存在该参数时，按照绑定与解绑操作处理	|
	|cmd			|String	|no		|bind：绑定<br/>unbind:解绑<br>当obdUid存在时，该参数必须存在|
	|obdId			|string	|yes	|obd的设备ID				|
	|obdSerial		|string	|yes	|obd序列号				|
	|obdType		|string	|yes	|obd类型uid				|
	|simCard		|string	|yes	|sim卡号					|
	|deptUid		|String |no		|组织ID					|
	|vechileUid		|string	|no		|车辆UID	，当该参数存在时，优先使用该参数|
	|vehicleLic		|String	|no		|车牌号					|

## 7.2 /obd/{obdUid}
- HTTP方式：POST
- 版本：无
- 发布版本：
- 输入参数
	
	|参数名			|参数类型|是否必须|	参数描述				|
	|---- 			| --- 	| ------| ---- 					|
	|cmd			|String	|yes	|modify:修改信息<br/>:删除|
	|obdId			|string	|no		|obdId					|
	|simCard		|string	|no		|SIM卡号					|
	|obdSerial		|String	|no		|obd序列号				|

- 返回参数
	
	obd未绑定时，相关车辆信息为空
	
	<pre>
	{
		"vehicleLic":"皖A44532",				//车牌号
		"vehicleUid":"214324553"
		"deptName":"车辆管理部门",				//车辆所属部门
		"obdId":"23213234"					//obdId
		"obdSerial":"FIEFJFDS"				// OBD序列号
		"obdType":"航天-1234",
		"bindStatus":"3"						//obd状态：1：空闲 2：注册 3：绑定
		"bindTime":"2015-07-17 23:23:23"		// obd绑定时间
	}
	</pre>

## 7.3 /obd/list 获取obd列表
- HTTP方式：GET
- 版本：无
- 发布版本：
- 输入参数
	
	|参数名			|参数类型|是否必须|参数描述				|
	|---------------|-------|-------|-------------------	----	|
	|deptUid		|String	|no		|所属机构的UID			|
	|obdId			|String	|no		|设备ID					|
	|obdSerial		|String	|no		|设备序列号				|
	|simCard		|String	|no		|sim卡号					|
	|status			|String	|no		|设备状态<br/>1:空闲<br/>2:注册<br/>3：绑定|
	|vehicleLic		|String	|no		|绑定车牌号				|
	|pageStart		|String	|no		|起始页，默认为1			|
	|pageCount		|String	|no		|单页显示数量				|
	
- 返回参数

	obd未绑定时，相关车辆信息为空
	
	<pre>
	{
	 "total":3,
	 "rows":[
			{
				"obdUid":"21ererw32435",
				"vehicleLic":"皖A44532",				//车牌号
				"vehicleUid":"214324553"
				"deptName":"车辆管理部门",				//车辆所属部门
				"obdId":"23213234"					//obdId
				"obdSerial":"FIEFJFDS"				// OBD序列号
				"obdType":"航天-1234",
				"bindStatus":"3"						//obd状态：1：空闲 2：注册 3：绑定
				"bindTime":"2015-07-17 23:23:23"		// obd绑定时间
				"simCard":"123456"
				"location":{}
			}]
	}
	</pre>

## 7.3.1 /obd/type/list 查询obd类型
- HTTP方式：GET
- 版本：
- 发布版本：
- 输入参数：无
- 返回参数：

	<pre>
	{
		"total":"10",
		"rows":[{
			"obdTypeUid":"342342332",
			"obdBrand":"航天",
			"obdTypeName":"G213"
		},{
			"obdTypeUid":"342342332",
			"obdBrand":"航天",
			"obdTypeName":"G212"
		}]
	}
	</pre>

# 8. 统计接口

## 8.1 /statistic/fuel 油耗统计
	注：该接口停止维护，请使用接口8.2代替
- HTTP方式：GET
- 版本：
- 发布版本：
- 输入参数：

	|参数名			|参数类型|是否必须|	参数描述				|
	|---- 			| --- 	| ------| ---- 					|
	|startTime		|string	|yes	|统计的开始日期（格式为'yyyy-MM-dd'	|
	|endTime		|string	|yes	|统计的结束时间，格式同上	|
	|vehicleUid		|string	|no		|待统计的车辆Uid			|
	|deptUid		|string	|no		|待统计的部门Uid，vehicleUid和deptUid两个参数必选其一	|

- 返回参数：
	
	<pre>
	{
		"total":32
		rows: [{
			date: "2015-07-09 00:00:00",		//统计日期
			fuel: 4.22, 					//油耗 L
			mile: 72.2, 					//里程（km）
			avgFuel: 5.84					//平均油耗，（L/100km）
		},
		{
			date: "2015-07-10 00:00:00",
			fuel: 4.22, 
			mile: 72.2, 
			avgFuel: 5.84
		}]
	}

## 8.2 图表展示 /statistic/chart/single
- HTTP方式：GET
- 版本：
- 发布版本：
- 输入参数：

	|参数名			|参数类型|是否必须|	参数描述				|
	|---- 			| --- 	| ------| ---- 					|
	|code			|string	|yes	|编码，
		c001：里程
		c002：油耗
		c003：总耗油费
		c004：平均油耗
		c005：用车时间
		c006：机车拆除
		c007：怠速超长
		c008：越界
		c009：非调度用车
		c010：车辆速率
		c011：超速
			|
	|unitId			|string	|yes	|车辆所在单位ID			|
	|startDate		|string	|yes	|统计的开始日期（格式为'yyyyMMdd'，手机端取当前月份第一天，如20150701	|
	|endDate		|string	|yes	|统计的结束日期，格式同上，手机端取当前月份最后一天，如20150731	|

- 返回参数：
	
	<pre>
	{
		"total":32
		rows: [{
			CREATE_DATE: "20150701",		//统计日期
			Y1: 72							//值
		},
		{
			CREATE_DATE: "20150702", 
			Y2: 92
		}]
	}

# 9. 其他接口

## 9.1 /pic 图片上传
- HTTP方式：POST
- 版本：无
- 输入参数

	
- 返回参数

	<pre>
	{
		"url":"http://343.233.231.321/images/a.png"
	}
	</pre>

## 9.2 /push/reminder 消息推送接口
- HTTP方式：POST
- 版本：无
- 发布版本：无
- 输入参数
	
	|参数名			|参数类型|是否必须|	参数描述					|
	|---- 			| --- 	| ------| ---- 						|
	|principle		|string	|yes	|关联目标的Id，即vehicleUid，为0时表示关联所有车辆	|
	|title			|string	|yes	|消息标题					|
	|text			|string	|no		|消息内容					|
	|time			|date	|no		|消息发生的时间				|
	|source			|string	|yes	|消息来源，1:OBD;2:三方接口3:驾驶员主动发起	|
	|data			|string	|no		|该消息相关的数据			|
	|reminderType   |int    |yes    |消息类型，来自Reminder表|
	|obdId			|string	|no		|			|
	|obdUid			|string	|no		|			|
	|vehicleLic		|string	|no		|车牌			|

- 返回参数

	<pre>
	{
		"ret":"0",
		"message":"推送成功"
	}
	</pre>

## 9.3 /weather 天气接口
- HTTP方式：GET
- 版本：无
- 发布版本：无
- 输入参数


	|参数名			|参数类型|是否必须|	参数描述					|
	|---- 			| --- 	| ------| ---- 						|
	|city			|string	|yes	|想获取天气的城市	|

- 返回参数
	
	<pre>
	{
		"currentCity":"合肥",
		"today": {
			"date":"今天",		// 天气预报的时间
			"pictureUrl":"http://api.abc.com/good.png",		// 天气图标路径
			"weather":"晴,多云",		// 天气状况描述，以逗号分隔多种天气
			"wind":"23",				// 风力
			"temp":"23",				// 温度（℃）
		}
	}
	</pre>


### 4.3.9 /vehicle/match 人车匹配接口定义
#### 4.3.9.1 HTTP方式：POST
#### 4.3.9.2 输入参数
 - 
 - 
 - 
 - Lic|string|是|车牌号
 - vehicleUid|string|是|车辆id
 - 
 - Name|string|是|驾驶员姓名
 - driverUid|string|是|驾驶员id
#### 4.3.9.3 返回参数
	{"ret":"0",
     "message":"匹配成功"}

	注意：匹配保存时需要判断，该车与驾驶员是否已经匹配过。如果是，给出错误提示。

### 4.3.10 /vehicle/match/list 人车匹配列表接口定义
#### 4.3.10.1 HTTP方式：GET
#### 4.3.10.2 输入参数
 - deptUid|部门id|否|部门id，为空则查询所有
 - vehicleLic|string|否|车牌号
 - vehicleNickName|string|否|车辆别名
 - driverName|string|否|司机姓名
 - startTime|string|否|起始日期|2015-06-20
 - endTime|string|否|结束日期|2015-06-23
  - 所有的查询条件都是模糊匹配 
#### 4.3.10.3 返回参数
	{"total":"25",
	 "rows":[{"matchUid":"345642acedf32a8ce7394263cafe2938",
	  "vehicleLic":"皖A56XCV",
	  "vehicleNickName":"客服6448",
	  "driverName":"张三",
	  "phone":"18966789653",
	  "matchTime":"2015-06-17 18:21",
	  "reMatchTime":"2015-06-17 18:21",
	  "status":"0"
	 },{"matchUid":"345642acedf32a8ce7394263cafe2938",
	  "vehicleLic":"皖A56XCV",
	  "vehicleNickName":"客服6448",
	  "driverName":"张三",
	  "phone":"18966789653",
	  "matchTime":"2015-06-17 18:21",
	  "reMatchTime":"2015-06-17 18:21",
	  "status":"0"
	 },{"matchUid":"345642acedf32a8ce7394263cafe2938",
	  "vehicleLic":"皖A56XCV",
	  "vehicleNickName":"客服6448",
	  "driverName":"张三",
	  "phone":"18966789653",
	  "matchTime":"2015-06-17 18:21",
	  "reMatchTime":"2015-06-17 18:21",
	  "status":"0"
	 }]
	}
	status:0表示未绑定，1表示已绑定。




### 4.3.10 /vehicle/rematch 解除人车匹配接口定义
#### 4.3.10.1 HTTP方式：POST
#### 4.3.10.2 输入参数
 - matchUid|string|是|人车绑定id
#### 4.3.10.3 返回参数
	{"ret":"0",
     "message","解绑成功"}