---
id: 07boipd8xwxk7415ed7qxhj
title: CSS
desc: ''
updated: 1725701119082
created: 1725676461111
---


### Box Model

The box model in CSS is what every HTML element uses for layout. It's made up of content, padding, border and margin. 

![box model](/assets/images/box-model.png)
[source](https://web.dev/learn/css/box-model) 

The default box sizing is content box which means the width and height of an element is the combination of the content, padding and border. For example in the css below for `.my-box`:

```css
.my-box {
  width: 200px;
  border: 10px solid;
  padding: 20px;
}
```

the width is 200px + 20px (border on left+right) + 40px (padding on left+right) = **260px**

If we change the `box-sizing` property to `border-box`, the width becomes 200px as the borders and paddings will fall into the content space.