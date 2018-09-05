### 9.5xml格式

**reserved1**字段   
微信支付时必填，如：ios为type=ios&app_name=xxxxx&bundle_id=xxxx&商户自定义参数;
Android为type=android&app_name=王者荣耀&package_name=com.tencent.tmgp.sgame&商户自定义参数；
H5为type=h5&wap_url=xxxxx&wap_name=xxxx&商户自定义参数；

#### 正确写法：

```xml
<reserved1><![CDATA[type=android&app_name=王者荣耀&package_name=com.tencent.tmgp.sgame&112233]]>
</reserved1>
```

 **需要转义
