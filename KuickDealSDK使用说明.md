#KuickDealSDK接入指南(iOS)

#目录
###1.导入SDK
###2.添加初始化参数
###3.用户实名登录
###4.用户注销登录
###5.手动添加事件
###6.提高数据准确度
###7.常见错误

##1.导入SDK
###方法1.文件导入
1.从官网下载SDK文件.

2.将`KuickDealSDK.framework `文件导入工程中.

3.在`Build Phases >Link Binary With Libraries`中添加framwork.

4.在`Build Phases > Embed Framework`中 添加framwork.
###方法2.执行pod安装
-暂不支持pod!!!


##2.添加初始化参数
1. 在 AppDelegate 中引入`#import "KuickDealSDK.h"`
如:

```obj-c
- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
   
   //启动Kuick
    [[KDSKuickDeal shareClient] init:@{@"appKey":@"76be292b-a733-4ff4-981e-58e3eb88e6b1",
                                          @"appUserId":@"123"}];
    ....
    return YES;
}
```
* 初始化参数说明:

| 参数 |必填 | 说明  |
| ------------- |:-------------:| -----|
| appKey  |是 | 应用Key, 从Deal管理后台获取 | 
| appUserId | 是 | 应用中用户ID，由第三方应用提供，用来唯一标识一个用户 |
| appSecret |是 |app秘钥|


2.实现`KuickDealClientDelegate`协议
如:

```obj-c
- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
   
    ...
    [KDSKuickDeal shareClient].delegate = self;
    ...
    return YES;
}

- (void)kuickDealClient:(KDSKuickDeal *)client didComplete:(NSError *)error{
    if (!error) {
        NSLog(@"初始化成功!");
    }
}
```

##3.用户实名登录
```obj-c
  [[KDSKuickDeal shareClient] userNamedWithName:@"姓名"
                                          phone:@"手机"
                                        company:@"公司"
                                          email:@"邮箱"
                                          title:@"职位"
                                      appUserID:@"appUserID"
                                        succeed:^
    {
        [self showAlert:@"实名化成功"];
    } failure:^(NSError *error) {
        [self showAlert:@"实名化失败"];
    }];
```
* 实名化登录参数说明:

| 参数 |必填 | 说明  |
| ------------- |:-------------:| -----|
| name  |是 | 用户名 | 
| phone | 是 | 电话|
| company |否 |公司|
| email |否 |邮箱|
| title |否 |职位|
| appUserID |是 |应用用户id|

##4.用户注销登录
```obj-c
  [[KDSKuickDeal shareClient] destruction]
```
##5.手动添加事件
###5.1 播放音视频
* 开始播放

 ```obj-c
 //接受返回的事件ID,用于创建停止播放事件
 __playID = [[KDSKuickDeal shareClient] actionInVeiw:self.view 
 												  acitonTyep:KuickDealAcitonTypePlayMedia 
 												 description:@"播放视频"];
 												 
 ```
* 结束播放
 
 ```obj-c
 //使用一个 播放事件的 ID 创建结束播放时间
  [[KDSKuickDeal shareClient] endActionWithID:__playID];
    __playID = nil;
    
 ```
 **注意:开始播放和结束播放必须是成对出现!!!**
 
###5.2 提交表单

 ```obj-c
 [[KDSKuickDeal shareClient] actionInVeiw:self.view 
 									 acitonTyep:KuickDealAcitonTypeSubmit 
 									description:@"提交表单"];
 ```
 
###5.3 下载文件
```obj-c
    [[KDSKuickDeal shareClient] actionInVeiw:self.view
                                  acitonTyep:KuickDealAcitonTypeDownloadFile
                                 description:@"下载文件"];
```

##6.提高数据准确度
1.尽量为每个页面添加Title, 如果这个viewController有复用,做多个不同功能的页面使用,请在`viewWilAppear:`方法中变更title的值.
如:

```obj-c
- (void)viewWillAppear:(BOOL)animated{
   ...
   
   if(type == KKViewTypeSetName){
      self.navigationItem.title = @"设置昵称";
   }else{
      self.navigationItem.title = @"设置邮箱";
   }
   
   ...
}
```
2.给每个viewController 添加唯一ID(默认取这个viewController的类名做唯一标识),如果这个viewController做多个不同功能的页面复用,请在为改页面添加`@property (weak, nonatomic) NSString * kuickBehaviorViewIdentity`;`viewWilAppear:`方法中设置唯一标识.
如:

```obj-c

@interface KKTestVC()
...
@property (nonatomic,copy)NSString * kuickBehaviorViewIdentity
...
@end

@implementation KKTestVC
- (void)viewWillAppear:(BOOL)animated{
   ...
   
   self.kuickBehaviorViewIdentity = @"KKSomeIdentity";
   
   ...
}
```
##7.常见错误
1.提示`'KuickDealSDK.h' file not found` 或相关头文件找不到.


* **解决方法1:**

尝试以`<KuickDealSDK/KuickDealSDK.h>`的方式导入.


* **解决方法2:**

在`Build Setting > Heather Search Paths`中添加framwork头文件目录.
<p>
 如:

    $(PROJECT_DIR)/{中间目录}/KuickDealSDK.framework/Headers
 </p>

2.提示` Reason: image not found`
请在`Build Phases > Embed Framework` 添加KuickDealSDK.framwork









