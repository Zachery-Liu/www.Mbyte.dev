---
title: 避开使用 Paypal 的恶心汇率的教程
date: 2024-08-05 12:22:32
tags: [Paypal,汇率,教程,currency]
cover: https://cdn.jsdelivr.net/gh/Zachery-Liu/blog-comments@main/pics/tech-daily-GdRvIi8mWzE-unsplash.jpg
banner: https://cdn.jsdelivr.net/gh/Zachery-Liu/blog-comments@main/pics/tech-daily-GdRvIi8mWzE-unsplash.jpg
topic: tutorials
---

一晚上越想越气，一气之下终于找到了绕开 Paypal 恶心汇率的终极秘籍。

<!-- more -->

本篇教程将介绍如何将 Paypal 支付外币时的转换从 Paypal 垃圾汇率到卡发行组织汇率。

本人研究不深，还请大佬多指正。

## 在支付时切换汇率

据 Paypal 自己所说：

>如需选择您发卡机构的汇率，请在核对付款时，选择“通过PayPal兑换币种”选项，并将其更改为“通过发卡机构兑换币种”。

{% image https://cdn.jsdelivr.net/gh/Zachery-Liu/blog-comments@main/pics/paypal/1.png Paypal帮助中的内容 fancybox:true %}

我没有试过，大家可以尝试一下。

## 自动支付时切换汇率

上网搜索之后据说找客服也可以，大家可以尝试一下。

### 修改结算方式

直接登录 Paypal 网页版，使用账号密码登录。

{% link https://www.paypal.com/myaccount/autopay/connect %}

在这里可以管理所有的自动支付。

{% image https://cdn.jsdelivr.net/gh/Zachery-Liu/blog-comments@main/pics/paypal/2.png 在这里可以管理所有的自动支付 fancybox:true %}

点击左侧栏需要修改的付款对象，然后修改右侧付款方式。

{% image https://cdn.jsdelivr.net/gh/Zachery-Liu/blog-comments@main/pics/paypal/3.png 点击左侧栏需要修改的付款对象，然后修改右侧付款方式 fancybox:true %}

选择```查看兑换选项```。

{% image https://cdn.jsdelivr.net/gh/Zachery-Liu/blog-comments@main/pics/paypal/4.png 支付方式 fancybox:true %}

选择使用银联进行兑换，然后点击同意即可。

{% image  https://cdn.jsdelivr.net/gh/Zachery-Liu/blog-comments@main/pics/paypal/5.png 选择银联进行兑换 fancybox:true %}

此时汇率修改完成，再也不用 Paypal 的垃圾汇率了。

## 汇率对比

{% note color:warning 注意：汇率请以当日为准，此处为二零二四年八月六日当日不同平台汇率，仅供参考。 %}

{% note color:cyan Paypal 1 USD=7.349949 CNY %}

{% frame iphone11 img:https://cdn.jsdelivr.net/gh/Zachery-Liu/blog-comments@main/pics/paypal/6.jpg %}

使用手机访问以下页面可以查询。

{% link https://www.paypal.com/sm/cshelp/article/%E5%9C%A8%E5%93%AA%E9%87%8C%E5%8F%AF%E4%BB%A5%E6%89%BE%E5%88%B0paypal%E7%9A%84%E5%B8%81%E7%A7%8D%E6%8D%A2%E7%AE%97%E5%99%A8%E5%92%8C%E6%B1%87%E7%8E%87%EF%BC%9F-help109?locale.x=zh_SM %}


{% note color:cyan 银联 1 USD = 7.176 CNY %}

{% image  https://cdn.jsdelivr.net/gh/Zachery-Liu/blog-comments@main/pics/paypal/7.png 银联官网汇率 fancybox:true %}

访问以下页面可以查询。

{% link https://www.unionpayintl.com/cn/rate/ %}