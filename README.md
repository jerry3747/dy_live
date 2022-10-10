

## 前言
strData,sessionid,x_bogus,_signature,_ac_signature,q,xgplayer_user_id,s_v_web_id,rcft。


## 步骤一，抓包分析
打开chrome浏览器，新开无痕窗口，随便进入一个用户主页，同时用charles进行抓包。
![在这里插入图片描述](https://img-blog.csdnimg.cn/c885b3c743934f0d8e848a181931a7ea.png)

![在这里插入图片描述](https://img-blog.csdnimg.cn/42d65c50943e4f3f8e5632a4f1ed5846.png)
	通过抓包得知，主页会请求两次，第一次请求返回__ac_nonce，并且返回一段js。
	![在这里插入图片描述](https://img-blog.csdnimg.cn/130ba347b3004402b7e2b37ad4834f49.png)
	这段js里面可以清楚看到有__ac_nonce和__ac_signature的生成方法。



## 步骤二，扣代码
步骤一里面的js是分别被两个script标签包裹的，把它们都抠出来，格式化一下，稍作修改，然后复制到浏览器的控制台运行一下。![在这里插入图片描述](https://img-blog.csdnimg.cn/5e2644a0e06d4c6d91809b7856bb4261.png)
![在这里插入图片描述](https://img-blog.csdnimg.cn/cf638a36ccc344a895cd778161a3ba35.png)
能够正常获取到__ac_signature，如果之前有了解过_signature就一眼能看出来，__ac_signature和_signature是通过同一个方法生成的，只是传参不一样，__ac_signature传入的__ac_nonce，而_signature传入的是url。
![在这里插入图片描述](https://img-blog.csdnimg.cn/9470587265614edb8bf40ab90e20964d.png)



## 步骤三，补环境
将修改后的js放vscode跑一下，会报错，根据错误信息补上相应的环境。

```javascript
var window = global;
// window = this;
window.location = {
    href: "https://*******/",
    protocol: "https:",
};

window.navigator = {
    userAgent: "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/100.0.4896.60 Safari/537.36"
};

window.document = {
    referrer:"https:/******/"
};
```

补上这几个环境，最后把window.byted_acrawler改为exports就可以了。
![在这里插入图片描述](https://img-blog.csdnimg.cn/9deb3d021818438bb4c1d99e74c165e1.png)
![在这里插入图片描述](https://img-blog.csdnimg.cn/ec0b72f5f9834c02b233a5af24dcc679.png)

某音点赞、关注、人气都有研究，详情可以加群交流，
以上内容仅供交流学习，如有疑问或者侵权请私信联系删除，
交流学习q群940447889
