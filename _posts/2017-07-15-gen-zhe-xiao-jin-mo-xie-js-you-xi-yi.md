---
layout: post
title: "跟着萧井陌写js游戏"
date: 2017-07-15 23:52:23.000000000 +09:00
tags: javascript
---

萧井陌大神在B站开直播写游戏，果断跟进一波，学习一下高手敲代码的姿势。
[地址](http://www.bilibili.com/video/av12138532/)

## 先谈谈心得 

- **抽象**
- **面向对象**

讲道理，第一次听有开了一扇门的感觉——原来高手是这样思考的！

虽然他没有特别的提及，但还是感觉到了面向对象思想的重要性，从一开始写完一份完成功能的代码，到后面花了快一个小时将之梳理清楚，结构化，细思之下，所有的动作都围绕着“持续不断的抽象”、“坚决贯彻面向对象思想”两个中心来进行。

而代码的优雅性就在这不断的抽象中建立起来，看到最后成型的代码，真心觉得优美多了，从维护性和扩展性及复用性来说，都比一开始的代码要好了很多。

而一开始的代码，就是我们平时写出来的样子——根本没有把面向对象的思想根植心底是我很重大的问题（当然，代码量不足更是）

## 代码解析

下面对萧大的代码进行自我理解，先贴代码：

备注：此部分实现一个挡板左右滑动的功能，需要在当前目录下预先截取一个paddle.png图片作为挡板

1.0版本：
```javascript
<!DOCTYPE html>
<html>
<head>
	<meta charset="utf-8">
	<title>game 1</title>
	<style type="text/css">
		canvas {
			border: 1px black solid;
		}
	</style>
</head>
<body>
	<canvas id="id-canvas" width="400" height="300"></canvas>

<script type="text/javascript">
	// 初始化画布
	var log = console.log.bind(console)
	var canvas = document.querySelector('#id-canvas')
	var context = canvas.getContext('2d')

	// 初始化初值
	var x = 100
	var y = 200
	var speed = 5
	var img = new Image()
	img.src = 'paddle.png'
	log(img)
	// log(context)
	img.onload = function () {
		context.drawImage(img,x,y)		
	}

	var leftDown = false
	var rightDown = false
	//event
	window.addEventListener('keydown',function (event) {
		var k = event.key
		log('keydown')
		if (k == 'a') {
			leftDown = true
		}else if (k == 'd') {
			rightDown = true
		}
	})

	window.addEventListener('keyup',function (event) {
		var k = event.key
		log('keyup')
		if (k == 'a') {
			leftDown = false
		}else if (k == 'd') {
			rightDown = false
		}
	})	

	setInterval(function () {
		//update x y
		if (leftDown) {
			x -= 5
		} else if (rightDown) {
			x += 4
		}
		// draw
		context.clearRect(0, 0, canvas.width, canvas.height)
		context.drawImage(img, x, y)
	}, 1000/30)

</script>
</body>
</html>
```

1.1 版本：经过结构化处理
```javascript
<!DOCTYPE html>
<html>
<head>
	<meta charset="utf-8">
	<title>game 1</title>
	<style type="text/css">
		canvas {
			border: 1px black solid;
		}
	</style>
</head>
<body>
	<canvas id="id-canvas" width="400" height="300"></canvas>

<script type="text/javascript">
	// 初始化画布
	var log = console.log.bind(console)

	var imageFromPath = function (path) {
		var img = new Image()
		img.src = path 
		return img
	}

	var Paddle = function () {
		var image = imageFromPath('paddle.png')
		var o = {
			image: image,
			x: 100,
			y: 200,
			speed: 5,
		}

		o.moveLeft = function () {
			o.x -= o.speed
		}

		o.moveRight = function () {
			o.x += o.speed
		}

		return o
	}

	var GuaGame = function () {
		var g = {
			actions: {},
			keydowns: {},			
		}
		var canvas = document.querySelector('#id-canvas')
		var context = canvas.getContext('2d')
		g.canvas = canvas
		g.context = context
		// draw
		g.drawImage = function (guaImage) {
			g.context.drawImage(guaImage.image, guaImage.x, guaImage.y)			
		}
		// envents
		window.addEventListener('keydown', function (event) {
			g.keydowns[event.key] = true
		})
		window.addEventListener('keyup', function (event) {
			g.keydowns[event.key] = false
		})		
		// register
		g.registerAction = function (key, callback) {
			g.actions[key] = callback
		}
		// timer
		setInterval(function () {
			// envents
			var actions = Object.keys(g.actions)
			for (var i = 0; i < actions.length; i++) {
				var key = actions[i]
				if (g.keydowns[key]) {
					// 如果按键被按下，调用注册的action
					g.actions[key]()
				}
			}
			// update a and y
			g.update()
			// clear
			context.clearRect(0, 0, canvas.width, canvas.height)
			// draw
			g.draw()
		},1000/30)
		return g
	}

	var _main = function () {
		var game = GuaGame()
		// 初始化初值
		var paddle = Paddle()

		game.registerAction('a', function () {
			paddle.moveLeft()
		})

		game.registerAction('d', function () {
			paddle.moveRight()
		})

		game.update = function () {
			// update	
	
		}
		
		game.draw = function () {
			// draw	
			game.drawImage(paddle)			
		}
	}
	_main()
</script>

</body>
</html>
```
