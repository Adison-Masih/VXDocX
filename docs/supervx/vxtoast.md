---
id: vxtoast
title: VxToast
---

### VelocityX gives you VxToast to show toast in your flutter app.

:::important

## Video Tutorial

<iframe width="300" height="185" src="https://www.youtube.com/embed/Peo5Wuos8CM" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
:::

## Using VxToast class

```dart
 VxToast.show(context, msg: "Hello from vx")
```

## Or Using Context

```dart
 context.showToast(msg: "Hello from vx")
```

---

<img src="https://i.imgur.com/Uk28KcB.png" alt="Toast sample 1" height="500"/>

:::note

Both of these works perfectly fine. So it is your call whatever you prefer.

:::

## Show Loading using VxToast

```dart
 VxToast.showLoading(context, msg: "Loading");

```

## Or Show Loading using Context

```dart
 final close = context.showLoading(msg: "Loading");
 Future.delayed(2.seconds, close); // Removes toast after 2 seconds

```

<img src="https://i.imgur.com/5Kgvk1G.png" alt="Toast sample 2" height="500"/>

## Properties

| Prop (Type)                |        Default         |                             Description |
| -------------------------- | :--------------------: | --------------------------------------: |
| showTime (int)             |          2000          |            To set duration of the toast |
| bgColor (Color)            |                        |      Sets background color of the toast |
| textColor (Color)          |                        |            Sets text color of the toast |
| textSize (int)             |           14           |        Sets text font size of the toast |
| position (VxToastPosition) | VxToastPosition.bottom | Available options - top, center, bottom |
| pdHorizontal (double)      |           20           |                 Sets horizontal padding |
| pdVertical (double)        |           10           |                   Sets vertical padding |
