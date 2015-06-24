# AngularU Gestures

### Step 1

* Install Ionic and Cordova
    * ``npm install -g ionic cordova``

### Step 2

* Create new project
    * ``ionic start gestures https://github.com/thack/angularu-gestures``
    * ``cd gestures``
    * ``ionic platform add ios`` (Should be automatic on OSX)
    * ``ionic platform add android``

### Step 3

* Run using Ionic View
* Run on emulator or device
    * iOS recommended:
        * ``ionic emulate ios -l -c``
        * Live reload and console output.
    * Android recommended (Device or Genymotion):
        * ``ionic run android -l -c``
        * Live reload and console output.
    * ``ionic run ios``
        * Can be tricky because of certificate security.
    * ``ionic emulate android``
        * Slow without HAXM and HAXM is buggy on Mac.

### Step 4

* Add scroll=false to ion-content.
* Add div with square class.
* Add ion-pinch directive to div.
```
  <ion-content scroll="false">
    <div class="square" ion-pinch></div>
  </ion-content>
```

* Add CSS to style.css:
```
  .square {
    width: 184px;
    height: 200px;
    background: #DDD url('../img/shield.png') 0/100%;
  }
```

### Step 5

* Add code to app.js for onGesture() function.
```
  ionic.onGesture('touch drag transform dragend', function(e) {
    e.gesture.srcEvent.preventDefault();
    e.gesture.preventDefault();
    switch (e.type) {
    case 'touch':
      last_scale = scale;
      last_rotation = rotation;
      break;
    case 'drag':
      posX = e.gesture.deltaX + lastPosX;
      posY = e.gesture.deltaY + lastPosY;
      break;
    case 'transform':
      rotation = e.gesture.rotation + last_rotation;
      scale = e.gesture.scale * lastScale
      break;
    case 'dragend':
      lastPosX = posX;
      lastPosY = posY;
      lastScale = scale;
      break;
    }
    var transform =
      "translate3d(" + posX + "px," + posY + "px, 0) " +
      "scale(" + scale + ")" +
      "rotate(" + rotation + "deg) ";
    e.target.style.transform = transform;
    e.target.style.webkitTransform = transform;
  }, $element[0]);
```

### Step 6

* Run again using emulator, device, or Ionic View.

### Step 7

* Add Crosswalk
    * ``ionic browser add crosswalk``
* Run again on Android:
    * ``ionic run android``

### Extra Credit

* Shield PNG becomes blurry at high zoom.
* Replace PNG with SVG file for resolution independence.
