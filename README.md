Cordova Plugin KakaoTalk (with CustomURL scheme)
========================

This plugin is modified by combining the two other plguins

- https://github.com/taejaehan/KakaoTalkCordovaPlugin

Make sure you've registered your Facebook app with Kakao and have an KAKAO_APP_KEY

- https://developers.kakao.com



Cordova Install Note:
========================

cordova plugin add https://github.com/queenyjina/cordova-kakaotalk.git --variable KAKAO_APP_KEY=YOUR_KAKAO_APP_KEY

[Android]
* nothing to do ;-)
* But the Android app must register key hash(https://developers.kakao.com/docs/android#getting-started-launch-sample-app)

[iOS]
* Add following code to appDelegate

```
#import <KakaoOpenSDK/KakaoOpenSDK.h>
#import <Cordova/CDVPlugin.h>


- (BOOL)application:(UIApplication *)application openURL:(NSURL *)url
                                       sourceApplication:(NSString *)sourceApplication
                                              annotation:(id)annotation {
    ...
    if ([KOSession isKakaoAccountLoginCallback:url]){
        return [KOSession handleOpenURL:url];
    }else{
        if (!url) { return NO; }
        // all plugins will get the notification, and their handlers will be called
        [[NSNotificationCenter defaultCenter] postNotification:[NSNotification notificationWithName:CDVPluginHandleOpenURLNotification object:url]];
        return YES;
    }
    ...
}

- (void)applicationDidBecomeActive:(UIApplication *)application{[KOSession handleDidBecomeActive];}
```

* Ohter Linker Flags 

open platforms/ios/*.xcodeproj
        Build Settings > Linking > Other Linker Flags > add '-all_load'



How to use the plugin
========================

### Usage

This plugin adds an object to the window. Right now, you can only login and logout.

##### Login

Login using the `.login` method:
```
KakaoTalk.login(
    function (result) {
      console.log('Success login!');
      console.log(result);
    },
    function (message) {
      console.log('Error login!');
      console.log(message);
    }
);
```

The login reponse object is defined as:
```
{
  accessToken: '<KakaoTalk accessToken>',
  id: '<KakaoTalk User Id>',
  nickname: '<KakaoTalk User Nickname>',
  profile_image: '<KakaoTalk User ProfileImage>',
  thumbnail_image: '<KakaoTalk User thumbnailImage>'
}
```

##### Logout

Logout using the `.logout` method:
```
KakaoTalk.logout(
	function() {
		console.log('Success logout!');
	}, function() {
		console.log('Error logout!');
	}
);
```


##### Session

session check and newer infomation using the `.login` method:
```
KakaoTalk.session(
    function (result) {
      console.log('Success session!');
      console.log(result);
    },
    function (message) {
      console.log('Error session!');
      console.log(message);
    }
);
```

The session reponse object is defined as:
```
{
  accessToken: '<KakaoTalk accessToken>',
  id: '<KakaoTalk User Id>',
  nickname: '<KakaoTalk User Nickname>',
  profile_image: '<KakaoTalk User ProfileImage>',
  thumbnail_image: '<KakaoTalk User thumbnailImage>'
}
```


##### Share

Share using the `.share` method:
```
KakaoTalk.share({
    text : 'Share Message',
    image : {
      src : 'https://developers.kakao.com/assets/img/link_sample.jpg',
      width : 138, 
      height : 90,
    },
    weblink :{
      url : 'YOUR-WEBSITE URL',
      text : 'web사이트로 이동'
    },
    applink :{
      url : 'YOUR-WEBSITE URL', 
      text : '앱으로 이동',
    },
    params :{
      paramKey1 : 'paramVal',
      param1 : 'param1Value',
      cardId : '27',
      keyStr : 'hey'
    }
  },
  function (success) {
    console.log('kakao share success');
  },
  function (error) {
    console.log('kakao share error');
  });
```

- you can use text, image, weblink and applink(params) separately or together
- Min image width(80)xheight(80)
- weblink(text-link), applink(button-link)
- if you use applink, you can set any params(optional)

- https://developers.kakao.com/docs/ios#카카오링크
- https://developers.kakao.com/docs/android#카카오링크
