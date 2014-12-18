title: '初探Javascript的Device API'
id: 677
categories:
  - HTML
date: 2014-03-20 00:31:36
tags:
---

写这篇文时以下非常有用的device api并未被所有浏览器（特别是mobile browser）实现，期待各种新版本的新实现。

需要用时，在 caniuse.com 查阅就知道当前这个api是否可用。

&nbsp;

<!-- more -->

[javascript]

// # 获取地理位置
 if (navigator.geolocation.getCurrentPosition) {
 navigator.geolocation.getCurrentPosition(function (position) {
 // 获取经纬度
 var lat = position.coords.latitude;
 var log = position.coords.longitude;

alert('经纬度信息：', lat, ', ', log);
 });
 } else {
 console.log('啊噢~不支持 navigator.geolocation.getCurrentPosition 方法')
 }
 // # 监听设备旋转
 if (window.DeviceOrientationEvent) {
 // 添加监听
 window.addEventListener('deviceorientation', function (event) {
 // 获取从左到右的tilt
 var tiltLR = event.gamma;
 // 获取从前到后的tilt
 var tiltFB = event.beta;
 // 获取设备方向
 var direction = event.alpha;
 // console.log('设备方向信息：', tiltLR, tiltFB, direction);
 });
 } else {
 console.log('啊噢~ 不支持 window.DeviceOrientationEvent 方法');
 }
 // # 调用摄像头
 if (navigator.getUserMedia) {
 navigator.getUserMedia({
 video: true
 // 成功
 }, function (localMediaStream) {
 // 获取页面中的容器
 var vid = document.getElementById('camera-video');
 // 创建 URL对象
 vid.src = window.URL.createObjectURL(localMediaStream);
 // 失败
 }, function (err) {
 console.log('啊噢~调用摄像头失败：', err);
 });
 } else {
 console.log('啊噢~不支持 navigator.getUserMedia 方法')
 }

// # 设置振动
 if (navigator.vibrate) {
 // 振动一秒
 navigator.vibrate(1000);

// 振动几次
 navigator.vibrate([500, 250, 500]);
 } else {
 console.log('啊噢~ 不支持 navigator.vibrate 方法')
 }

// # 感应环境亮度
 window.addEventListener('devicelight', function (event) {
 // 单位是 lux
 var lightLevel = event.value;
 console.log('环境亮度值是：', lightLevel);
 });

// # 查看电源状态
 if (navigator.battery) {
 // 电量
 var level = navigator.battery.level;
 // 是否在充电
 var isCharging = navigator.battery.charging;
 // 还需多久才能充满电
 var chargingTime = navigator.battery.chargingTime;
 // 电源还能坚持多久
 var dischargingTime = navigator.battery.dischargingTime;

console.log('电池状态：', level, isCharging, chargingTime, dischargingTime);
 } else {
 console.log('啊噢~ 不支持 navigator.battery 方法')
 }

[/javascript]