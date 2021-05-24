---
id: vxanimator
title: VxAnimator
---

> VelocityX gives you <b>VxAnimator</b> that makes animation easier.

:::important
VxAnimator uses [animator](https://pub.dev/packages/animator) package to support animations. You can use animator directly without VelocityX.
:::

:::note
VxAnimator as of now doesn't work with null safety.
:::

<img src="https://i.imgur.com/X0n8Eq6.gif" alt="animator sample gif" height="500"/>

> Above is an animated VxBox using the VxAnimator Widget

## Using VxAnimator

```dart
VxAnimator({
  Key key,
  Tween<dynamic> tween, // (1) // Default tween: Tween<double>(begin:0 end: 1)
  Map<String, Tween<dynamic>> tweenMap, // (1)
  Duration duration: const Duration(milliseconds: 500),  // (2)
  Curve curve: Curves.linear, // (3)
  int cycles, // (4)
  int repeats, // (5)
  (BuildContext, AnimatorState, child) → Widget builder, // (6)
  Widget child, // (7)
  () → void endAnimationListener, // (8)
  dynamic customListener,  // (9)
  (AnimationStatus, AnimationSetup) → dynamic statusListener //(10)
  bool triggerOnInit: true, () // (11)
  bool resetAnimationOnRebuild: false, // (12)
  TickerMixin tickerMixin, // (13)
  AnimatorKey animatorKey, // (14)
}).{curvetype}.{tweentype}.{duration}.infinite.make()


Eg:
VxAnimator<double>(
          builder: (context, animState, child) {
            return VxBox()
                .rounded
                .alignCenter
                .pink400
                .square(animState.value)
                .makeCentered();
          },
        ).easeInCubic.doubleTween(10.0, 200.0).seconds(sec: 10).infinite.make(),

```

Animator:

- makes animation as simple as the simplest widget in Flutter with the help of Animator widget,
- Allows you to control (forward, stop, inverse), reconfigure, reset and restart animation from buttons, or events callbacks.

# Animator:

With one widget, `Animator`, you can do all the available animation in Flutter.

Actually, Animator is a facade that hides all the complexity of the animation setting in Flutter.
In the design pattern the facade pattern is :

> Facade is a structural design pattern that provides a simplified interface to a library, a framework, or any other complex set of classes.

```dart
VxAnimator({
  Key key,
  Tween<dynamic> tween, // (1) // Default tween: Tween<double>(begin:0 end: 1)
  Map<String, Tween<dynamic>> tweenMap, // (1)
  Duration duration: const Duration(milliseconds: 500),  // (2)
  Curve curve: Curves.linear, // (3)
  int cycles, // (4)
  int repeats, // (5)
  (BuildContext, AnimatorState, child) → Widget builder, // (6)
  Widget child, // (7)
  () → void endAnimationListener, // (8)
  dynamic customListener,  // (9)
  (AnimationStatus, AnimationSetup) → dynamic statusListener //(10)
  bool triggerOnInit: true, () // (11)
  bool resetAnimationOnRebuild: false, // (12)
  TickerMixin tickerMixin, // (13)
  AnimatorKey animatorKey, // (14)
}).make()
```

- To implement any type of animation with animator you have to define a `Tween` (1), `Duration` (2), and `Curve` (3). `

- With `cycles` argument (4) you define the number of the forward and backward periods you want your animation to perform before stopping.

- With `repeats` argument (5) you define the number of forward periods you want your animation to perform before stopping.

- In the `builder` argument (6) you put your widgets to be animated.

  ```dart
      VxAnimator<T>(
        builder : (BuildContext context, AnimatorState animatorState, Widget child) {
            //to get animation value:
            final T value =  animatorState.value;
            //to get Animation object:
            final Animation<T> animation = animatorState.animation;
            //to get AnimationController object:
            final AnimationController animation = animatorState.controller;
            //to get animation value form tweenMap
            final R value = animatorState.getValue<R>('animName');
            //To get Animation object from tweenMap
            final Animation<R> value =animatorState.getAnimation<R>('animName');
        },
        child : MayWidget(), //widget to not rebuild with animation
    ).make()
  ```

- If you want to animate many Tween, use `tweenMap` argument (2). It is a Map of String type keys and Tween type values.
- With `endAnimationListener` (8) argument you can define a VoidCallback to be executed when the animation is finished. For example, it can be used to trigger another animation.

- With `customListener` (9) argument you can define a function to be called every time the animation value changes. The customListener is provided with an Animation object.

- With `statusListener` (10) argument, you can define a function to be called every time the status of the animation change. The customListener is provided with an AnimationStatus, AnimationSetup objects.

- `triggerOnInit` (11) controls whether the animation is automatically started when the Animator widget is initialized. The default value is true.

- If you want to reset your animation, such as changing your Tween or duration, and want the new setting to be reconsidered when the Animator widget is rebuilt, set the `resetAnimationOnRebuild` (12) argument to true. The default value is false. (See `AnimatorKey`)

- The right `TickerProvider` is chosen by animator:(13)

  - If animation is simple the `singleTickerProviderStateMixin`.
  - If you define the animatorKey parameter or set `resetAnimationOnRebuild`, animator use `tickerProviderStateMixin`.

- With `animationKey` (14), you can associate an `Animator` widget to an `AnimatorKey`. Doing so, you can control (forward, start, stop, reverse) animation from callbacks outside the Animator widget, you can even reset the animation parameter (tween, duration curve, repeats, cycles) on the fly. (See example below)

## Example of a single Tween animation:

<img src="https://i.imgur.com/bpenmfz.gif" alt="animator sample gif" height="500"/>

>Above is the output of the code below

```dart

class AnimatedLogo extends StatelessWidget {
  Widget build(BuildContext context) {
    return VxAnimator<double>(
      tween: Tween<double>(begin: 0, end: 300),
      cycles: 0,
      builder: (context, animatorState, child ) => Center(
            child: Container(
              margin: EdgeInsets.symmetric(vertical: 10),
              height: animatorState.value,
              width: animatorState.value,
              child: FlutterLogo(),
            ),
          ),
    ).make();
  }
}
```

## Example of a multi-tween animation:

<img src="https://i.imgur.com/Fz4Nfcz.gif" alt="animator sample gif" height="500"/>

> Above is the output of the code below

```dart
class AnimatedLogo extends StatelessWidget {
  Widget build(BuildContext context) {
    return VxAnimator(
      tweenMap: {
        "scaleAnim": Tween<double>(begin: 0, end: 300),
        "translateAnim": Tween<Offset>(begin: Offset(-2,0), end: Offset(2, 0)),
      },
      cycles: 0,
      builder: (context, animatorState, child ) => Center(
            child: FractionalTranslation(
              translation: animatorState.getAnimation<Offset>("translateAnim").value,
              child: Container(
                margin: EdgeInsets.symmetric(vertical: 10),
                height: animatorState.getAnimation<double>("scaleAnim").value,
                width:  animatorState.getAnimation<double>("scaleAnim").value,
                child: FlutterLogo(),
              ),
            ),
          ),
    ).seconds(sec: 3).make();
  }
}
```

You can nest many animators with a different setting (tween, duration, curve, repeats, and cycles) to do a complex animation:

This is a simple example of how to animate the scale and rotation independently.

```dart

class AnimatedLogo extends StatelessWidget {
  Widget build(BuildContext context) {
    return VxAnimator<double>(
      tween: Tween<double>(begin: 0, end: 300),
      repeats: 0,
      duration: Duration(seconds: 2),
      builder: (context, anim1, child ) => Animator<double>(
        tween: Tween<double>(begin: -1, end: 1),
        cycles: 0,
         builder: (context, anim2, child ) => Center(
          child: Transform.rotate(
            angle: anim2.value,
            child: Container(
              margin: EdgeInsets.symmetric(vertical: 10),
              height: anim1.value,
              width: anim1.value,
              child: FlutterLogo(),
            ),
          ),
        ),
      ),
    ).make();
  }
}
```

Use nested Animators with CustomPainter and CustomClipper and draw the animation you want.

# AnimatorKey

Similarly to Flutter global key. AnimatorKey allows controlling the animation from outside the Animator widget it is associated with.

Example:

```dart
final animatorKey = AnimatorKey();

//..
//..
VxAnimator(
    animatorKey: animatorKey,
    builder: (context, anim, child) {
        //....
    }
).make()

//By default, the animation will not start when the Animator is inserted in the widget tree.
//To start the animation from some Button in the widget tree outside the builder of the Animator.

onPressed: (){
    animatorKey.triggerAnimation();

    //You can  forward, start, reverse animation
    animatorKey.controller.stop,


    //You can configure the animation online and reset the setting and restart the animation
    animatorKey.refreshAnimation(
        tween: Tween(...),//new tween
        duration : Duration(...),
        curve : Curve(...),
        repeats : ....,
        cycles : ...
    );

}
```

# AnimatorRebuilder widget

The AnimatorRebuilder widget can subscribe to an “AnimatorKey”. It will be animated in synchronization with the Animator widget with which `animatorKey` is associated.
Example:

```dart
//In AnimatorKey you can provide the initial values of the animation.
final animatorKey = AnimatorKey<T>(initialValue: 10);

//..
//..
VxAnimator(
    animatorKey: animatorKey,
    builder: (context, anim, child) {
        //....
    }
).make();
//

//In another place in the widget tree :

AnimatorRebuilder(
    observe:()=> animatorKey,
    builder: (context, anim, child) {
        //....
        //this widget receives the same animation object of the Animator above.
    }
);
```

The order of Animator and AnimatorRebuilder in the widget tree is irrelevant, except in the case, AnimatorRebuilder is first inserted in the widget tree, you have to provide initial values in AnimatorKey to avoid null exceptions.

## [Credits](https://github.com/GIfatahTH/animator)
