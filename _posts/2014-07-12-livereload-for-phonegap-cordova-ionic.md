---
layout: post
title: Livereload for Phonegap/Cordova/Ionic
---

I love the fast feedback loop of webdevelopment and want to have the same experience for developing mobile applications. I finally found [a way](http://app.phonegap.com), which is ultra fast and dead simple to use.

--

I'm currently writing my bachelor thesis at [KIT](http://www.kit.edu) about iBeacons and have developed [this plugin](https://github.com/mobilion/cordova-ibeacon-plugin), which allows you to use iBeacons in your Phonegap/Cordova app.

While working on applications using this plugin I was really annoyed by the compile time. I'm working with web technologies and want to see the changes immediately, like I'm used to.

After a lot of research I found the new [PhoneGap Developer App](http://app.phonegap.com). All you have to do, is to install the mobile app on your target device ([iOS](https://itunes.apple.com/app/id843536693)/[Android](https://play.google.com/store/apps/details?id=com.adobe.phonegap.app)) and run the following command in your terminal:

```sh
$ phonegap serve
```

### For Cordova / Ionic users

If you are using Cordova or Ionic and you don't have `phonegap` installed, never mind: you can use your existing project. Just create a `.cordova` folder in your project's root folder (this is a Phonegap relic and will hopefully be removed in the future) and install Phonegap by running: 

```sh
$ npm install -g phonegap
```

### Try it

You should now see an ip address in your terminal. Run the "PhoneGap" app on your mobile device, enter the ip and click on "Connect". If everything went well you'll be seeing your project application, deployed over the air. If it didn't work, make sure that both your host system (Notebook/Desktop) and your mobile device are on the same (wifi) network. It even works with multiple devices at the same time.

### Livereload

`phonegap serve` supports livereload out of the box. Just make a change to a file inside your `www` folder and your application should refresh automagically. Awesome!

At first I had some errors like this:

```
node[31020] (CarbonCore.framework) FSEventStreamStart: register_with_server: ERROR: f2d_register_rpc() => (null) (-21)
```

I figured out this occured because there were too many files living in my `www` folder. I solved this problem by using [browserify](http://browserify.org) to auto build my javascript sources into one single file.

### Custom plugin support

PhoneGap Developer App doesn't support custom plugins yet, however I managed to use my iBeacon plugin. I forked [their repository](https://github.com/phonegap/phonegap-app-developer) and added my plugin via `$ phonegap plugin add`. I deployed my fork of the PhoneGap Developer App on my mobile device but unfortunately it didn't work yet.

I had to come up with another workaround (which hopefully won't be necessary in the future): You have to add a customized `corodva_plugins.js` and the corresponding `plugins` directory to your `www` folder. But at least it worked now.

### Conclusion

PhoneGap Developer App is a total lifesaver and makes development with Cordova, Phonegap or Ionic much more fun, since you see your changes immediately. It's dead simple to use and saves you a ton of time.





