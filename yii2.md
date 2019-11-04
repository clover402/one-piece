### 登录
提供一个AR类的用户model继承IdentityInterface（it is the interface that should be implemented by a class providing identity information.
This interface can typically be implemented by a user model class.）
实现该接口的类一般为一个AR类，主要提供用户信息相关的查询功能。
一般包含5个方法：
1. findIdentity($id)  通过用户id获取用户信息
2. findIdentityByAccessToken($token, $type = null) 通过accessToken获取用户信息
3. getId() 获取用户id
4. getAuthKey() 获取用户认证key
5. validateAuthKey($authKey)  验证用户认证key
*如果要实现RESTFul API认证还要加一个方法：*
6. loginByAccessToken($accessToken, $type)  通过accessToken进行登录

### API认证
1. 提供认证方法
可以使用yii框架自带的三种：
* QueryParamAuth 通过access-token认证
* HttpBasicAuth 通过用户名密码认证
* HttpBearerAuth HEAD的Authorization字段认证
* 也可以继承\yii\filters\auth\AuthMethod来自定义，继承该类需要实现至少1个方法
authenticate($user, $request, $response)  必须，进行认证,参考user组件
challenge($response)  可选，认证失败时的提示，可加在header
handleFailure($response) 无需，AuthMethod已有，认证失败的操作一般抛出认证失败异常
2. 在控制器中添加认证方法
以QueryParamAuth为例
```
public function behaviors()
{
   $behaviors = parent::behaviors();
   $behaviors['authenticator'] = [
       'class' => \yii\filters\auth\QueryParamAuth::className(),
       'tokenParam' => 'sso_ticket',
   ];
   return $behaviors;
}
```
3. 在用户的IdentityInterface实现类中添加loginByAccessToken($accessToken, $type)方法，如下为示例：
```
public function loginByAccessToken($token, $type)
{
    switch($type){
        case 'QueryParamAuth'://token参数验证
            $identity = static::findIdentityByAccessToken($token, $type);
            if($identity && $identity->login()) {
                return $identity;
            }else{
                return null;
            }
            break;
        case 'HttpBasicAuth'://用户名密码验证
            return null;
            break;
        case 'HttpBearerAuth'://header bearer验证
            return null;
            break;
        case 'AjaxAuth'://自定义api验证
            return null;
        default:
            return null;
            break;
    }
}
```

* [login](http://blog.csdn.net/a553181867/article/details/50987388)
* [RESTFul API](http://blog.csdn.net/u012979009/article/details/52136672)

### ActiveRecord
1. 联表查询
在model中增加方法
```
public function getComUniCode()
{
    return $this->hasOne(ComBasicInfo::className(), ['com_uni_code' => 'com_uni_code']);
}
```
调用时在where方法后加 with('comUniCode') 即可
2. 联表时增加被关联表的条件
```
$customers = Customer::find()->limit(100)->with([
    'orders' => function($query) {
        $query->andWhere('subtotal>100');
    },
])->all();
```
3. 多表联表查询
在model中增加方法
```
public function getAcquisition()
{
    return $this->hasOne(ComAssetAcquisition::className(), ['seq'=>'trade_related_id'])
       ->viaTable(
            ComAssetTraderelated::tableName(),
            ['trade_main_id'=>'qhqm_seq'],
            function($query){
              $query->onCondition(['trade_related_name'=>ComAssetAcquisition::tableName()]);
            });
}
```

### extension
1. 安装扩展
在vendor\yiisoft\extensions.php文件最后添加如下内容
```
'miloschuman/yii2-highcharts-widget' =>
        array (
          'name' => 'miloschuman/yii2-highcharts-widget',
          'version' => '5.0.2',
          'alias' => array (
             '@miloschuman/highcharts' => $vendorDir . '/miloschuman/yii2-highcharts-widget/src',
          ),
    ),
```

###
