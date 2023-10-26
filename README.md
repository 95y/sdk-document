
## H5开发流程 
### sdk暴露的方法
1) [getUrlParams](#geturlparams) (获取URL上参数)
2) [initData](#initdata) (剧本初始化，获取角色列表)
3) [getRole](#getrole) (获取角色自定义变量)
4) [updateRole](#updaterole) (更新角色自定义变量)
5) [getGlobalCustom](#getglobalcustom) (获取全局自定义变量)
6) [updateGlobalCustom](#updateglobalcutom) (更新全局自定义变量)
7) [getAllRole](#getallrole) (获取所有角色自定义变量List)
8) [getSdkTokenInfo](#getsdktokeninfo) (获取当前角色信息)
9) [getSdkPlayerList](#getsdkplayerlist) (获取剧本角色列表)
10) [appBack](#appback) (剧乐乐App 后退)
11) [appendNavigation](#appendnavigation) (H5插入导航 - 仅App生效，小程序默认导航无法去除)
12) [initSocket](#initsocket) (初始化连接socket)
13) [socket](#socket) (socket的属性)
14) [getRoomStoreInfo](#getroomstoreinfo) (获取房间店铺信息)
15) [clearGlobalAndRoleCustom](#clearglobalandrolecustom) (清除房间所有变量数据，包含全局和角色)

一.开发游戏阶段:  
1) 在[柒巧空间](https://cspace.you-drama.com/#/user/login)上传剧本，得到一个key。同时上传剧本所需的角色列表  
2) 通过柒巧空间得到的key调用SDK的初始化（initData）接口, 返回角色列表(<font color=#ffa657>roleId</font>  -- roleName -- roleToken)  
3) 开发者根据自己剧本的角色<font color=#ffa657>roleId</font>，角色<font color=#ffa657>token</font>, 操作自定义的变量,(调用SDK的updateRole, getRole等接口),展示效果。

注:此时不需要操作剧乐乐APP，平行书小程序等。

**二**. H5游戏开发完成之后，需要配合剧乐乐APP，平行书小程序来真正进行游戏。  
1) 剧本配置H5地址，提交审核  
2) 剧本审核通过之后，DM用剧乐乐APP开房间,玩家通过小程序来玩。  
3) 平行书进行到机制阶段的时候，平行书小程序会通过地址来告诉H5小游戏 <font color=#ffa657>roleId</font> 和 <font color=#ffa657>token</font> 的值。此时的<font color=#ffa657>roleId</font>就是玩家开房时候选择的角色，<font color=#ffa657>token</font>里面是包含房间Id等一系列业务逻辑的加密值。  
4) 此时H5游戏通过<font color=#ffa657><font color=#ffa657>roleId</font></font>和<font color=#ffa657><font color=#ffa657>token</font></font>来操作sdk的接口，此时效果应该是和开发阶段一致的。  

## 参数说明：

>**1、平行书&剧乐乐App打开H5的完整链接如下：**  
**https://h5机制地址.com?token=xx&roleId=xx&userId=xx&title=xx&env=prod**  
**2、开发人员可在url上添加自己的url参数，平行书&剧乐乐App 会检测url上是否带自定义url参数**  
     如：https://h5机制地址.com?自定义参数1=xx&自定义参数2=xx  <br><br>
**「自定义参数说明：如部分H5需要自己通过url携带参数用于判断当前场景等等」**  
     <font color=red>平行书&App会自动追加参数</font>
     **https://h5机制地址.com?自定义参数1=xx&自定义参数2=xx&token=xx&roleId=xx&title=xx**  
**「开发阶段建议把<font color=#ffa657>token</font>、<font color=#ffa657>roleID</font>自行拼接到url上，自行截取url参数，以保持与提测、生产阶段一致」**<br><br>
<font color=#ffa657></font>
```javascript
url参数字段说明：  
token: 接口请求凭证  
roleId: H5当前角色id  
userId: 小程序or剧乐乐App当前的用户id  
title: 用于设置title  
url参数预留关键字：env (自定义字段情况下，请勿使用env参数名)
```



```
Q & A：  
Q: H5中如何获取当前角色？  
A: 开发阶段可通过url上自己拼接roleId (角色ID)，进行对应角色的机制开发

Q: token哪里获取？  
A: 开发阶段: 可在柒巧空间的剧本编辑中，获取该剧本联调SDK的token  
提测&生产阶段：通过url截取token参数值
```

### 

## 引入SDK
### 在html文件中引入SDK
```
<script src="https://osslarp.oss-cn-shenzhen.aliyuncs.com/common/sdk/jllsdk.iife.min_v1.0.2.js"></script>
```
<span id="geturlparams"></span>
## SDK使用
### jll作为一个全局的变量，可以访问到SDK的所有方法
```javascript
<script>
  console.log(jll)
  // 截取url携带的参数，getUrlParams方法为sdk内置的截取url参数的方法
  const urlParams = jll.getUrlParams()
  console.log('getUrlParams...', urlParams)
</script>
```
![image.png](https://cdn.nlark.com/yuque/0/2023/png/20359859/1689906157347-2f32fc06-47bb-4051-8346-4ff4929e9830.png#averageHue=%2324272a&clientId=ud9fc5de7-3824-4&from=paste&height=237&id=ud05a5491&originHeight=237&originWidth=883&originalType=binary&ratio=1&rotation=0&showTitle=false&size=72053&status=done&style=none&taskId=u0b6ac7c4-1de5-41a9-af80-07f283e4e6b&title=&width=883)
```javascript

 sdk方法说明：
 
   appBack: 			      app的返回上一级页面方法(需要自行添加后退按钮)
   appendNavigation:    向App端插入导航 （后退按钮已内置appBack）方法
   getAllRole:          获取所有角色自定义变量
   getGlobalCustom:     获取全局自定义变量
   getRole:             获取当前角色变量
   getUrlParams:        获取Url上所有携带的参数
   initData:					  获取剧本角色列表和角色token
   initSocket:          初始化并连接websocket
   sendSocket:          向服务端发送消息（目前暂无，预留）
   socket:              socket的所有属性
   updateGlobalCustom: 	初始化/更新全局自定义变量
   updateRole:          初始化/更新角色自定义变量
   getSdkTokenInfo:     获取当前角色信息
   getSdkPlayerList:      获取剧本角色列表

```
![image.png](https://cdn.nlark.com/yuque/0/2023/png/20359859/1684995642469-92b0dbc1-136c-4069-94e9-5698c0f26227.png#averageHue=%2323292c&clientId=u05f9c401-2053-4&from=paste&height=70&id=u69e774cd&originHeight=70&originWidth=380&originalType=binary&ratio=1&rotation=0&showTitle=false&size=12003&status=done&style=none&taskId=u33ffa135-b5f7-4d5c-879d-9364897388b&title=&width=380)
### SDK的调用
```
const token = '123456'
jll.initData({
  data: {
    sKey: token
  },
  success (res) {
    console.log(res)
  },
  fail (e) {
    console.log(e)
  },
  complete () {
    console.log('完成')
  }
})
```
请求：
![image.png](https://cdn.nlark.com/yuque/0/2023/png/20359859/1689911119677-2e03a8fc-e5dd-4fbf-bcad-26b35d9e665b.png#averageHue=%2328292c&clientId=uc136910f-6c4d-4&from=paste&height=173&id=u2fe888d5&originHeight=173&originWidth=516&originalType=binary&ratio=1&rotation=0&showTitle=false&size=24786&status=done&style=none&taskId=udb5d981d-bc66-4cd6-b0a9-0cd2d243624&title=&width=516)
响应：
![image.png](https://cdn.nlark.com/yuque/0/2023/png/35114687/1684292831745-67036623-5a84-4831-bcf9-3d82e96425bb.png#averageHue=%23fdfbf7&clientId=u3e62b549-2100-4&from=paste&height=191&id=ub4abfc37&originHeight=191&originWidth=739&originalType=binary&ratio=1&rotation=0&showTitle=false&size=32483&status=done&style=none&taskId=ubc84e390-b79b-48b8-9e61-a5e42c6ada8&title=&width=739)

 💡 当errorCode为0时则调用成功，其他则失败

<span id="appback"></span>
## 剧乐乐App默认去除导航栏，自定义导航栏后退方法请调用appBack
jll.appBack()
### 剧乐乐App端添加导航栏
```json
// 直接调用就可以，webview环境判断已在内部做了判断
jll.appendNavigation('标题名称')
```
<span id="initdata"></span>
## initData初始化-获取剧本角色列表信息
### 请求参数
| 参数 | 是否必传 | 类型 |
| --- | --- | --- |
| sKey | 是 | String (token) |

### 请求示例
```json
const token = '123456'
jll.initData({
  data: {
    sKey: token
  },
  success (res) {
    console.log(res)
  },
  fail (e) {
    console.log(e)
  },
  complete () {
    console.log('完成')
  }
})
```
### 成功响应
**条件**：请求参数合法，并且用户身份校验通过。
##### 参数说明：
```
  avatarUrl:  角色头像
  id:         角色ID
  title:      角色名称
  角色token:   用于调用sdk其他接口（除initData）的凭证，请拿到token后，自行拼接到url上去调试

```
```json
{
    "constructor":1610711152,
    "errorCode":0,
    "roles":[
        {
            "avatarUrl":"https://larp-test.oss-cn-shenzhen.aliyuncs.com/4302/3DB2C19112078F131688628771105.jpg",
            "id":674,
            "title":"队友丙",
            "token":"jhTO/3sWRA4NvWKktqmKSGn5ymN9NgE1G3oyqO7U5Ca38AGUid+tNUuzuflsSpmJiby6//RqhI3jY5XzXzn9plCsxU8g7TrMLDOVFo+8RP6ZALlvFXjNJk29tlPMkAyRxOO3bgXEacvqUbe8SKrdVYMlhizwuVOWkTgcMnR80SaFoG6aqNjzBvbQpkLNSnVnZtSUM54Tg3F+eHU49ogG8Q=="
        },
        {
            "avatarUrl":"https://larp-test.oss-cn-shenzhen.aliyuncs.com/43004e/78086B3613DDA6DA1685067941327.jpg",
            "id":641,
            "title":"队友乙",
            "token":"jhTO/3sWRA4NvWKktqmKSGn5ymN9NgE1G3oyqO7U5Ca38AGUid+tNUuzuflsSpmJiby6//RqhI3jY5XzXzn9pkh6zJ8soUKlVj8CsMPVvYAzLLzkn1DrVPztM1FpzcntRiDiLPtMs3pELaoY1DntitcJeUaRqc1lJcJUsnFpN8wv/IUxarjdz0Awxp2w8MWAtKooT1vp+Z7cTOR0ePrbeQ=="
        },
        {
            "avatarUrl":"https://larp-test.oss-cn-shenzhen.aliyuncs.com/43004e/E5645D6A626D5FC31685067904941.png",
            "id":640,
            "title":"队友甲223",
            "token":"jhTO/3sWRA4NvWKktqmKSGn5ymN9NgE1G3oyqO7U5Ca38AGUid+tNUuzuflsSpmJiby6//RqhI3jY5XzXzn9pqsw0wF8l46bOFaTqmk4GU8Nv9lH8uLDFLZ2ET/e2I2o67GGTgTlagMpIJ+aYa+dNectdCwAJhPRibsFUNhdohIcJ2ZvV2z7Q3fPFmerfR3GZylN/6xFO7iz97mlfbtnQA=="
        }
    ]
}
```
<span id="getsdktokeninfo"></span>
## getSdkTokenInfo-获取当前角色信息
### 请求参数
| 参数 | 是否必传 | 类型 |
| --- | --- | --- |
| sKey | 是 | String (角色token) |

### 请求示例
```json
const token = '123456'
jll.getSdkTokenInfo({
  params: {
    sKey: token
  },
  success (res) {
    console.log(res)
  },
  fail (e) {
    console.log(e)
  },
  complete () {
    console.log('完成')
  }
})
```
### 成功响应
##### 参数说明：
```
dmFlag:           是否是DM身份
roleAvatarUrl:    角色头像
roleId:           角色ID
roleNickName:     角色名称
```
```json
{
	"constructor": 1610711163,
	"dmFlag": false,
	"errorCode": 0,
	"roleAvatarUrl": "https://larp-test.oss-cn-shenzhen.aliyuncs.com/4408/C5D5F0B518F32A501688634930435.jpg",
	"roleId": 669,
	"roleNickName": "胡克"
}
```

<span id="getsdkplayerlist"></span>
## getSdkPlayerList-获取剧本角色列表
### 请求参数
| 参数 | 是否必传 | 类型 |
| --- | --- | --- |
| sKey | 是 | String (角色token) |

### 请求示例
```json
const token = '123456'
jll.getSdkPlayerList({
  params: {
    sKey: token
  },
  success (res) {
    console.log(res)
  },
  fail (e) {
    console.log(e)
  },
  complete () {
    console.log('完成')
  }
})
```
### 成功响应
##### 参数说明：
```
dmFlag:           是否是DM身份
roleAvatarUrl:    角色头像
roleId:           角色ID
roleNickName:     角色名称
```
```json
{
    "constructor":1610711164,
    "errorCode":0,
    "sdkPlayerList":[
        {
            "dmFlag":false,
            "roleAvatarUrl":"https://larp-test.oss-cn-shenzhen.aliyuncs.com/4302/3DB2C19112078F131688628771105.jpg",
            "roleId":674,
            "roleNickName":"队友丙"
        },
        {
            "dmFlag":false,
            "roleAvatarUrl":"https://larp-test.oss-cn-shenzhen.aliyuncs.com/43004e/78086B3613DDA6DA1685067941327.jpg",
            "roleId":641,
            "roleNickName":"队友乙"
        },
        {
            "dmFlag":false,
            "roleAvatarUrl":"https://larp-test.oss-cn-shenzhen.aliyuncs.com/43004e/E5645D6A626D5FC31685067904941.png",
            "roleId":640,
            "roleNickName":"队友甲223"
        },
        {
            "dmFlag":true,
            "roleId":50,
            "roleNickName":"DM"
        }
    ]
}
```
<span id="getrole"></span>
## getRole 获取自定义角色变量
### 请求参数
| 参数 | 是否必传 | 类型 |  |
| --- | --- | --- | --- |
| sKey | 是 | String (token) |  角色token
| roleId | 是 | String |  发起人角色ID
| bizFields | 是 | String(length: 1024) | biz数据属性的描述列表,调用方的自己定义的,类似java里面对象的类描述信息,业务方可以用自定义bizId |
| bizDataVersion | 是 | String(length: 20) | biz的数据的版本号,调用方自己进行定义 |
| version | 是 | 1.0.2 | sdk版本号 |

### 请求示例
```json
const token = '123456'
const roleId = '638'
jll.getRole({
  data: {
    sKey: token
    roleId: roleId
  },
  success (res) {
    console.log(res)
  },
  fail (e) {
    console.log(e)
  },
  complete () {
    console.log('完成')
  }
})
```
### 成功响应
```
  customContent:  自定义角色变量  
  id:             角色ID
```
```json
{
  "constructor":1610711153,
  "errorCode":0,
  "role": {
    "customContent":"sdjfjdkjf",
    "id":638
  }
}
```
<span id="updaterole"></span>
## updateRole 初始化/更新/删除自定义角色变量
### 请求参数
| 参数 | 是否必传 | 类型 | 说明 |
| --- | --- | --- | --- |
| sKey | 是 | String (token) |  角色token |
| roleId | 是 | String |  发起人角色id |
| bizContent | 是 | String  | 业务数据 |
| noticeRoleIds | 是 | String  | 通知角色(id) 英文逗号分隔 DM的角色id默认50 |
| noticeContent | 是 | JSON String | socket广播的内容 |
| bizFields | 是 | String(length: 1024) | biz数据属性的描述列表,调用方的自己定义的,类似java里面对象的类描述信息,业务方可以用自定义bizId |
| bizDataVersion | 是 | String(length: 20) | biz的数据的版本号,调用方自己进行定义 |
| version | 是 | 1.0.2 | sdk版本号 |

### 请求示例
```json
const token = '123456'
const roleId = '638'
const customContent = 'test'
jll.updateRole({
  data: {
    sKey: token,
    roleId: roleId,
    customContent: customContent， // 该字段没有的时候是表示删除
    noticeRoleIds: '640,641,674', // 示例
    noticeContent: {type: 'updateRole'}  // 示例
  },
  success (res) {
    console.log(res)
  },
  fail (e) {
    console.log(e)
  },
  complete () {
    console.log('完成')
  }
})
```
**<font color=red>💡 customContent字段不传则表示删除</font>**
### 成功响应

**条件**：请求参数合法，并且用户身份校验通过。

```json
{"constructor":1610711154,"errorCode":0}
```

<span id="getglobalcustom"></span>
## getGlobalCustom 获取自定义全局变量
### 请求参数
| 参数 | 是否必传 | 类型 |  |
| --- | --- | --- | --- |
| sKey | 是 | String (token) |  角色token|
| roleId | 是 | String | 发起人角色id |
| bizFields | 是 | String(length: 1024) | biz数据属性的描述列表,调用方的自己定义的,类似java里面对象的类描述信息,业务方可以用自定义bizId |
| bizDataVersion | 是 | String(length: 20) | biz的数据的版本号,调用方自己进行定义 |
| version | 是 | 1.0.2 | sdk版本号 |

### 请求示例
```json
const token = '123456'
const roleId = '638'
jll.getGlobalCustom({
  params: {
    sKey: token,
    roleId: roleId
  },
  success (res) {
    console.log(res)
  },
  fail (e) {
    console.log(e)
  },
  complete () {
    console.log('完成')
  }
})
```
### 成功响应
**条件**：请求参数合法，并且用户身份校验通过。
##### globalContent: 全局自定义变量字段
```json
{
  "constructor":1610711158,
  "errorCode":0,
  "globalParamVo": {
    "globalContent":"我是全局自定义变量"
  }
}
```
<span id="updateglobalcustom"></span>
## updateGlobalCustom 更新自定义全局变量
### 请求参数
| 参数 | 是否必传 | 类型 | 说明 |
| --- | --- | --- | --- |
| sKey | 是 | String  | (token) |
| roleId | 是 | String（发起人角色id） | 当前角色ID |
| bizContent | 是 | String  | 业务数据 |
| noticeRoleIds | 是 | String  | 需要通知的角色 DM的角色id默认50 |
| noticeContent | 是 | JSON String | 广播的内容 |
| bizFields | 是 | String(length: 1024) | biz数据属性的描述列表,调用方的自己定义的,类似java里面对象的类描述信息,业务方可以用自定义bizId |
| bizDataVersion | 是 | String(length: 20) | biz的数据的版本号,调用方自己进行定义 |
| version | 是 | 1.0.2 | sdk版本号 |

### 请求示例
```json
const token = '123456'
const roleId = '638'
const globalContent = '我是新的'
jll.updateGlobalCustom({
  data: {
    sKey: token,
    roleId: roleId,
  	globalContent: globalContent,
  	noticeRoleIds: '640,641,674', // 示例
    noticeContent: {type: 'updateRole'}  // 示例
  },
  success (res) {
    console.log(res)
  },
  fail (e) {
    console.log(e)
  },
  complete () {
    console.log('完成')
  }
})
```
### 成功响应
**条件**：请求参数合法，并且用户身份校验通过。
```json
{
  "constructor":1610711159,
  "errorCode":0,
}
```
<span id="getroomstoreinfo"></span>
## getRoomStoreInfo 获取房间店铺信息
### 请求参数
| 参数 | 是否必传 | 类型 | 说明 |
| --- | --- | --- | --- |
| sKey | 是 | String  | (角色token) |

### 请求示例
```json
const token = '123456'
jll.getRoomStoreInfo({
  params: {
    sKey: token
  },
  success (res) {
    console.log(res)
  },
  fail (e) {
    console.log(e)
  },
  complete () {
    console.log('完成')
  }
})
```
### 成功响应
**条件**：请求参数合法，并且用户身份校验通过。
```json
{
  "address":"深大地铁站A口",
  "area":"南山区",
  "city":"深圳市",
  "constructor":1610711165,
  "errorCode":0,
  "province":"广东省",
  "roomCode":"123456", // 房间号
  "storeName":"张三的店铺", // 店铺名
  "storeOpenId":"LOcJd7XGXXXNttZ0Kray8A%3D%3D"
}
```
<span id="clearglobalandrolecustom"></span>
## clearGlobalAndRoleCustom 清除房间所有角色变量和全局数据
### 请求参数
| 参数 | 是否必传 | 类型 | 说明 |
| --- | --- | --- | --- |
| sKey | 是 | String  | (角色token) |

### 请求示例
```json
const token = '123456'
jll.clearGlobalAndRoleCustom({
  params: {
    sKey: token
  },
  success (res) {
    console.log(res)
  },
  fail (e) {
    console.log(e)
  },
  complete () {
    console.log('完成')
  }
})
```
### 成功响应
**条件**：请求参数合法，并且用户身份校验通过。
```json
{
  "constructor":1610711166,
  "errorCode":0,
}
```

<span id="getallrole"></span>
## getAllRole 获取所有角色自定义变量
### 请求参数
| 参数 | 是否必传 | 类型 |  |
| --- | --- | --- | --- |
| sKey | 是 | String (token) | 角色token |
| roleId | 是 | String | 发起人角色id |
| bizFields | 是 | String | biz数据属性的描述列表,调用方的自己定义的,类似java里面对象的类描述信息,业务方可以用自定义bizId |
| bizDataVersion | 是 | String | biz的数据的版本号,调用方自己进行定义 |
| version | 是 | String | sdk版本号 |

### 请求示例
```json
const token = '123456'
const roleId = '638'
jll.getAllRole({
  params: {
    sKey: token,
    roleId: roleId
  },
  success (res) {
    console.log(res)
  },
  fail (e) {
    console.log(e)
  },
  complete () {
    console.log('完成')
  }
})
```
### 成功响应
**条件**：请求参数合法，并且用户身份校验通过。
```json
{
  "constructor":1610711160,
  "errorCode":0,
  // 全局自定义变量
  "globalParamVo":{
    "globalContent":"我是新的"
  },
  // 角色自定义变量
  "roleList":[
    {"customContent":"我是新的角色自定义变量","id":641,"title":"队友乙"}
  ]
}
```
<span id="initsocket"></span>
## socket的初始化与接收

### 请求示例
```json
// 初始化连接socket  --- token为角色token
window.jll.initSocket(token)

// 接收消息
window.jll.socket.callback = res => {
  console.log(res)
  if (res.msgPreContent) {
  
  }
}
```
### 收到推送示例
![image.png](https://cdn.nlark.com/yuque/0/2023/png/20359859/1689908097238-a46c6816-991f-4972-ad9b-254ab098875c.png#averageHue=%231f2325&clientId=ud9fc5de7-3824-4&from=paste&height=255&id=u67b39245&originHeight=255&originWidth=1135&originalType=binary&ratio=1&rotation=0&showTitle=false&size=53440&status=done&style=none&taskId=u8360917a-617c-480f-9471-952eb3111a6&title=&width=1135)
