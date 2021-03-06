# HiPermission
A simple and beautiful runtime permission library on Android.[中文文档](/README-CN.md)


# Features
* One line of code to solve
* Various logics are omitted
* Support change permission Ui

`ACCESS_FINE_LOCATION`,`WRITE_EXTERNAL_STORAGE` and `CAMERA`,these almost every app requires permissions ,so we can request these permissions when app launch.But it's not very friendly to request so many permissions at once.So before the request permission, we need to tell the user why we did this.The inspiration comes from [eleme](https://play.google.com/store/apps/details?id=me.ele)

# Screenshot

![](/screenshot/screenshot1.gif)

# Demo

[Download](/screenshot/app-debug.apk)

# Usage
Use Gradle:

	compile 'me.weyye.hipermission:library:1.0.4'

Or Maven:

	<dependency>
	  <groupId>me.weyye.hipermission</groupId>
	  <artifactId>library</artifactId>
	  <version>1.0.4</version>
	  <type>pom</type>
	</dependency>
  
  In your Activity or anywhere:
  
  They will request three necessary permissions：`CAMERA`,`ACCESS_FINE_LOCATION` and `WRITE_EXTERNAL_STORAGE`
  ``` java
HiPermission.create(context)
	.checkMutiPermission(new PermissionCallback() {
		@Override
		public void onClose() {
			Log.i(TAG, "onClose");
			showToast("They cancelled our request");
		}

		@Override
		public void onFinish() {
			showToast("All permissions requested completed");
		}

		@Override
		public void onDeny(String permisson, int position) {
			Log.i(TAG, "onDeny");
		}

		@Override
		public void onGuarantee(String permisson, int position) {
			Log.i(TAG, "onGuarantee");
		}
	});
```

Sometimes you don't need to request for these permissions,you can add permissions that you want to request like this

``` java
List<PermissonItem> permissonItems = new ArrayList<PermissonItem>();
permissonItems.add(new PermissonItem(Manifest.permission.CAMERA, "Camera", R.drawable.permission_ic_memory));
permissonItems.add(new PermissonItem(Manifest.permission.ACCESS_FINE_LOCATION, "Location", R.drawable.permission_ic_location));
HiPermission.create(MainActivity.this)
			.permissions(permissonItems)
			.checkMutiPermission(...);
```

## Custom Style

Can I change the color of the interface? Or change hint information? Yes!

``` java
HiPermission.create(MainActivity.this)
			.title("Dear God")
			.permissions(permissonItems)
			.filterColor(ResourcesCompat.getColor(getResources(), R.color.colorPrimary, getTheme()))//permission icon color
			.msg("To protect the peace of the world, open these permissions! You and I together save the world!")
			.style(R.style.PermissionBlueStyle)
			.checkMutiPermission(...);
```

> After you have set the theme, you must called `filterColor ()` to set the color of the icon,otherwise the default is black


styles.xml

``` xml
    <style name="PermissionBlueStyle">
        <item name="PermissionTitleColor">@color/colorPrimaryDark</item>
        <item name="PermissionMsgColor">@color/colorPrimary</item>
        <item name="PermissionItemTextColor">@color/colorPrimary</item>
        <item name="PermissionButtonBackground">@drawable/shape_btn</item>
        <item name="PermissionBackround">@drawable/shape_bg_white</item>
        <item name="PermissionButtonTextColor">@android:color/white</item>
    </style>
```

![](/screenshot/screenshot2.jpg)

## Default Icon

If you need to request other permissions, but no icon? `HiPermission` has been prepared for you

| |Calendar|Camera|Contacts|Location|
|:-:|:-:|:-:|:-:|:-:|
| |![](http://upload-images.jianshu.io/upload_images/1643415-f64d7048c37dd8e2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)|![](http://upload-images.jianshu.io/upload_images/1643415-1697d58118fce639.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)|![](http://upload-images.jianshu.io/upload_images/1643415-e897ebfaa200ad34.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)|![](http://upload-images.jianshu.io/upload_images/1643415-ee31c852a07475df.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)|
|drawableId|permission_ic_calendar|permission_ic_camera|permission_ic_contacts|permission_ic_location|


| |Micro Phone|Phone|Sms|Storage|Sensors|
|:-:|:-:|:-:|:-:|:-:|:-:|
| |![](http://upload-images.jianshu.io/upload_images/1643415-42be4b1f4d72c177.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)|![](http://upload-images.jianshu.io/upload_images/1643415-7dd3e979f0448ad5.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)|![](http://upload-images.jianshu.io/upload_images/1643415-af7115c6855019f7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)|![](http://upload-images.jianshu.io/upload_images/1643415-c21d7061a286192c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)|![](http://upload-images.jianshu.io/upload_images/1643415-9905653ae13b86e1.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)|
|drawableId|permission_ic_micro_phone|permission_ic_phone|permission_ic_sms|permission_ic_storage|permission_ic_sensors|

Use icon 

``` java
List<PermissonItem> permissons = new ArrayList<PermissonItem>();
//use default icon
permissons.add(new PermissonItem(Manifest.permission.CALL_PHONE, getString(R.string.permission_cus_item_phone), R.drawable.permission_ic_phone));
HiPermission.create(MainActivity.this)
		.permissions(permissons)
		.style(R.style.PermissionDefaultGreenStyle)
		.checkMutiPermission(...);
```

>The icon is black by default, and you need to called `filterColor ()` to change the icon color

## Default Style

Of course, there are three default style and animations

| |Screenshot| Screenshot|Screenshot|
|:-:|:-:|:-:| :-:|
| |![](http://upload-images.jianshu.io/upload_images/1643415-9778484edc5c78a7.gif?imageMogr2/auto-orient/strip) |![](http://upload-images.jianshu.io/upload_images/1643415-f2eb04a9b1421357.gif?imageMogr2/auto-orient/strip)|![](http://upload-images.jianshu.io/upload_images/1643415-0a8990f176c5bb8f.gif?imageMogr2/auto-orient/strip)|
|styleId|PermissionDefaultNormalStyle|PermissionDefaultGreenStyle|PermissionDefaultBlueStyle|
|AnimId|PermissionAnimFade|PermissionAnimModal|PermissionAnimScale|

Theme by default without animation, you need to call animStyle () like this:

``` java
HiPermission.create(MainActivity.this)
                        .title(getString(R.string.permission_cus_title))
                        .permissions(permissons)
                        .msg(getString(R.string.permission_cus_msg))
                        .animStyle(R.style.PermissionAnimModal)//set dialog animation
                        .style(R.style.PermissionDefaultGreenStyle)//set dialog style 
                        .checkMutiPermission(...);
```

If you want to change the style of a attribute, you can extends a style inside your style override a attribute, like this

``` xml
    <style name="CusStyle" parent="PermissionDefaultGreenStyle">
        <item name="PermissionBgFilterColor">#75D175</item>
    </style>
```

![](http://upload-images.jianshu.io/upload_images/1643415-4dc09678be25a146.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


The following are all attributes


| attributes name       | type 
| ------------- |:-------------:|
| PermissionTitleColor     | int | 
| PermissionMsgColor| int |
| PermissionItemTextColor| int |
| PermissionButtonTextColor| int |
| PermissionButtonBackground| drawable|
| PermissionBackround| drawable|
| PermissionBgFilterColor|int|
| PermissionIconFilterColor|int|

# End
If you like,please give a star as an encouragement to me

# About me
* Email:[hiweyye@gmail.com](mailto:hiweyye@gmail.com)
* My Blog:[http://weyye.me](http://weyye.me)
* 简书:[http://www.jianshu.com/u/c5da2f9c87fb](http://www.jianshu.com/u/c5da2f9c87fb)
* CSDN:[http://blog.csdn.net/yewei02538](http://blog.csdn.net/yewei02538)

# License
    Copyright (C) 2017 WeyYe

    Licensed under the Apache License, Version 2.0 (the "License");
    you may not use this file except in compliance with the License.
    You may obtain a copy of the License at

    http://www.apache.org/licenses/LICENSE-2.0

    Unless required by applicable law or agreed to in writing, software
    distributed under the License is distributed on an "AS IS" BASIS,
    WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
    See the License for the specific language governing permissions and
    limitations under the License.
