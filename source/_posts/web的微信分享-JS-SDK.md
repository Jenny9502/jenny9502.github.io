---
title: react web的微信分享-JS-SDK
date: 2018-07-12 16:24:03
tags:
---

这是小姐姐我在做 **web页面** 需要在微信端正确分享的文章

何为 "正确" 说法,就是在微信端分享的时候,分享链接所带的小图,文章,链接是自定义的.

开发环境: 脚手架:react-boilerplate
          打包工具: webpack

## 一、了解 [微信JS-SDK说明文档](https://mp.weixin.qq.com/wiki?t=resource/res_main&id=mp1421141115)

web端微信的分享接口说明都在这个文档里,只要你慢慢研究,还是很好理解的.

先别着急着直接看微信分享接口,要一步一步来哦.

## 二、使用分享接口的步骤

### 1.绑定域名

>先登录微信公众平台进入“公众号设置”的“功能设置”里填写“JS接口安全域名”。
这个很好设置,本来要 po 步骤图的,但是无奈我不是管理员现在不能进账号 截图给你们分享了.but这一步还是很好操作的,相信大家都能很快做好!!

### 2.引入JS文件

有两种方式引入JS文件

#### (1) 在需要调用JS接口的页面引入如下JS文件,（支持https）:[http://res.wx.qq.com/open/js/jweixin-1.2.0.js](http://res.wx.qq.com/open/js/jweixin-1.2.0.js)

#### (2) 使用 AMD/CMD 标准模块加载方法 : **npm install weixin-js-sdk**

## 三、代码示例

### 1.创建分享文件 share.js

        // 微信分享
        import wx from 'weixin-js-sdk';
        import { getWxConfig } from './service';

        //设置不配置分享情况下的 title,描述,小图
        const baseTitle = 'XXXX';
        const baseDesc = 'XXXX';
        const baseImgUrl = 'XXXX';

        /**
         * 分享信息配置
         * @param shareMsg 分享信息对象
         */
        export const shareConfig = (shareMsg) => {
        //请求微信分享的固定配置函数,{url: 当前页面地址} 这是封装好的函数请求调用 Promise
          getWxConfig({ url: shareMsg.url }).then((data) => {
            wx.config({
              debug: false,
              appId: data.appid,
              timestamp: data.timestamp,
              nonceStr: data.noncestr,
              signature: data.signature,
              jsApiList: ['onMenuShareTimeline', 'onMenuShareAppMessage', 'onMenuShareQQ', 'onMenuShareQZone'],
            });
          });
          wx.ready(() => {
            // 分享到朋友圈
            wx.onMenuShareTimeline({
              title: shareMsg.title ? `${shareMsg.title}-${baseTitle}` : baseTitle, // 分享标题
              link: shareMsg.url, // 分享链接，该链接域名或路径必须与当前页面对应的公众号JS安全域名一致
              imgUrl: shareMsg.imgurl || baseImgUrl, // 分享图标
            });
            // 分享给朋友
            wx.onMenuShareAppMessage({
              title: shareMsg.title ? `${shareMsg.title}-${baseTitle}` : baseTitle,
              desc: shareMsg.desc ? `${shareMsg.title}-${baseDesc}` : '', // 分享描述
              link: shareMsg.url,
              imgUrl: shareMsg.imgurl || baseImgUrl,
              type: '', // 分享类型,music、video或link，不填默认为link
              dataUrl: '', // 如果type是music或video，则要提供数据链接，默认为空
            });
            // 分享到QQ
            wx.onMenuShareQQ({
              title: shareMsg.title ? `${shareMsg.title}-${baseTitle}` : baseTitle,
              desc: shareMsg.desc ? `${shareMsg.title}-${baseDesc}` : '', // 分享描述
              link: shareMsg.url,
              imgUrl: shareMsg.imgurl || baseImgUrl,
            });
            // 分享到QQ空间
            wx.onMenuShareQZone({
              title: shareMsg.title ? `${shareMsg.title}-${baseTitle}` : baseTitle,
              desc: shareMsg.desc ? `${shareMsg.title}-${baseDesc}` : '', // 分享描述
              link: shareMsg.url,
              imgUrl: shareMsg.imgurl || baseImgUrl,
            });
          });
        };
        /**
         * 设置调用微信分享函数
         * @param title 标题
         * @param desc  描述
         * @param img   小图
         */
        export const setWxShare = (title, desc, img) => {
          const url = img && (img.indexOf('http') ? img : `http:${img}`);
          const shareMsg = {
            url: location.href.split('#')[0],
            desc,
            title,
            imgurl: url,
          };
          shareConfig(shareMsg);
        };

### 2.在需要微信分享的页面调用 setWxShare() 函数就可以了

### 3.后台参数的生成

>以上就是前端需要做好的. But,最重要的就是 微信固定配置的 appId, timestamp, nonceStr, signature 这些值得获取.
>我们这里是通过请求后台直接编辑好把这些值返回给前端.

    wx.config({
        debug: true, // 开启调试模式,调用的所有api的返回值会在客户端alert出来，若要查看传入的参数，可以在pc端打开，参数信息会通过log打出，仅在pc端时才会打印。
        appId: '', // 必填，公众号的唯一标识
        timestamp: , // 必填，生成签名的时间戳
        nonceStr: '', // 必填，生成签名的随机串
        signature: '',// 必填，签名
        jsApiList: [] // 必填，需要使用的JS接口列表
    });

#### (1) appId

> appId 可以直接登录微信平台 微信公众平台-开发-基本配置 查看

#### (2) timestamp

> timestamp 生成签名的时间戳,后台返回的值,和生成签名时的时间戳保持一致.

#### (3) nonceStr

> nonceStr 生成签名的随机串 后台返回的值,和生成签名时的随机串保持一致.

    eg:
        const noncestr1 = () => {
          const data = ['0', '1', '2', '3', '4', '5', '6', '7', '8', '9', 'A', 'B', 'C', 'D', 'E', 'F', 'G', 'H', 'I', 'J', 'K', 'L', 'M', 'N', 'O', 'P', 'Q', 'R', 'S', 'T', 'U', 'V', 'W', 'X', 'Y', 'Z', 'a', 'b', 'c', 'd', 'e', 'f', 'g', 'h', 'i', 'j', 'k', 'l', 'm', 'n', 'o', 'p', 'q', 'r', 's', 't', 'u', 'v', 'w', 'x', 'y', 'z'];
          let str = '';
          for (let i = 0; i < 16; i++) {
            const r = Math.floor(Math.random() * 62);     // 取得0-62间的随机数，目的是以此当下标取数组data里的值！
            str += data[r];        // 输出20次随机数的同时，让rrr加20次，就是20位的随机字符串了。
          }
          return str;
        };

#### (4) signature

> signature 最重要的也是最难的,可以参考[微信的官方文档](https://mp.weixin.qq.com/wiki?t=resource/res_main&id=mp1421141115) 附录1-JS-SDK使用权限签名算法
>生成 signature 还需要了解 jsapi_ticket (是公众号用于调用微信JS接口的临时票据),然而 jsapi_ticket 是通过 **access_token** 获取的 可以参考 [微信的官方文档](https://mp.weixin.qq.com/wiki?t=resource/res_main&id=mp1421140183)

##### 生成 jsapi_ticket 的请求链接是: https://api.weixin.qq.com/cgi-bin/token?grant_type=client_credential&appid=APPID&secret=APPSECRET  https请求方式: GET

##### 其中 APPID, APPSECRET 的值可以在 微信公众平台-开发-基本配置 查看

##### 返回值的 access_token 就是需要的值

> Tips: 调用接口时，请登录“微信公众平台-开发-基本配置”提前将服务器IP地址添加到 **IP白名单** 中, 否则将无法调用成功。可以在 微信公众平台-开发-基本配置-IP白名单 配置

#### 步骤1. 按要求拼值 对所有待签名参数按照字段名的ASCII 码从小到大排序（字典序）后，使用URL键值对的格式（即key1=value1&key2=value2…）拼接成字符串string1。

    微信官方例子:
    noncestr=Wm3WZYTPz0wzccnW
    jsapi_ticket=sM4AOVdWfPE4DxkXGEs8VMCPGGVi4C3VM0P37wVUCFvkVAy_90u5h9nbSlYy3-Sl-HhTdfl2fzFy1AOcHKP7qg
    timestamp=1414587457
    url=http://mp.weixin.qq.com?params=value  // 需要分享的URL

    拼接的 string1 = jsapi_ticket=sM4AOVdWfPE4DxkXGEs8VMCPGGVi4C3VM0P37wVUCFvkVAy_90u5h9nbSlYy3-Sl-HhTdfl2fzFy1AOcHKP7qg&noncestr=Wm3WZYTPz0wzccnW&timestamp=1414587457&url=http://mp.weixin.qq.com?params=value;

#### 步骤2. 对string1进行sha1签名，得到signature：0f9de62fce790f9a083d5c99e95740ceb90c27ed

> sha1 可以通过 外部下载得到此方法.

    Tips:
        1.签名用的noncestr和timestamp必须与wx.config中的nonceStr和timestamp相同。

        2.签名用的url必须是调用JS接口页面的完整URL。

        3.出于安全考虑，开发者必须在服务器端实现签名的逻辑。

>如出现invalid signature 等错误详见 [微信的官方文档](https://mp.weixin.qq.com/wiki?t=resource/res_main&id=mp1421141115)附录5-常见错误及解决方法 常见错误及解决办法。

