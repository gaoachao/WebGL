# playcanvas

> PlayCanvas 是一款使用 HTML5 和 WebGL 技术运行游戏以及其他 3D 内容的开源游戏引擎，PlayCanvas 以其独特的性能实现了在任何手机移动端和桌面浏览器端均可以流畅运行。
>
> 对于开发者来说，可以在[playcanvas的平台](https://playcanvas.com/)进行开发，也可以在本地开发。
>
> [官方文档](https://developer.playcanvas.com/en/user-manual) |  [官方实例](https://playcanvas.github.io/#/physics/compound-collision) | [GitHub仓库](https://github.com/playcanvas/engine) 

## 起步

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8" />
    <title>PlayCanvas Hello Cube</title>
    <meta
      name="viewport"
      content="width=device-width, initial-scale=1, maximum-scale=1, minimum-scale=1, user-scalable=no"
    />
    <style>
      body {
        margin: 0;
        overflow: hidden;
      }
    </style>
    <script src="https://code.playcanvas.com/playcanvas-stable.min.js"></script>
  </head>
  <body>
    <canvas id="application"></canvas>
    <script>
      // 创建一个PlayCanvas应用
      const canvas = document.getElementById("application");
      const app = new pc.Application(canvas);

      // 在全屏模式下填满可用空间
      app.setCanvasFillMode(pc.FILLMODE_FILL_WINDOW);
      app.setCanvasResolution(pc.RESOLUTION_AUTO);

      // 确保在窗口尺寸变化同时画布也同时改变其大小
      window.addEventListener("resize", () => app.resizeCanvas());

      // 创建一个立方体
      const box = new pc.Entity("cube");
      box.addComponent("model", {
        type: "box",
      });
      app.root.addChild(box);

      // 创建一个摄像头
      const camera = new pc.Entity("camera");
      camera.addComponent("camera", {
        clearColor: new pc.Color(0.1, 0.1, 0.1),
      });
      app.root.addChild(camera);
      camera.setPosition(0, 0, 3);

      // 创建一个指向灯
      const light = new pc.Entity("light");
      light.addComponent("light");
      app.root.addChild(light);
      light.setEulerAngles(45, 0, 0);

      // 旋转相机
	  let x,z,count = 0;
      app.on("update", (dt) => {
		count += dt;
		x = 3 * Math.sin(count);
		z = 3 * Math.cos(count);
		camera.setPosition(x, 0, z);
		camera.rotate(0,57.265*dt,0);
	  });
        
      app.start();
    </script>
  </body>
</html>
```

## 2D平面

> 在三维空间中一般放置立体模型，而传统web前端开发中的按钮、盒子等都会放在三维空间中创建的2D平面。例如我们在浏览器中玩3D游戏时，人物、场景只是模型，而我们操作的是在2D平面上的按钮等。

### 创建2D平面

```javascript
	// 创建2D屏幕
	const screen = new pc.Entity();
	screen.addComponent("screen", {
		referenceResolution: new pc.Vec2(1280, 720),
		scaleBlend: 0.5,
		scaleMode: pc.SCALEMODE_BLEND,
		screenSpace: true,
	});
	app.root.addChild(screen);
```

### 2D平面的定位问题

[文档地址](https://developer.playcanvas.com/zh/user-manual/user-interface/elements/)

## 物理性质

> 在三维空间中引入的模型默认不具有物理性质，需要引入[ammo.js](https://github.com/kripken/ammo.js)才能添加重力等物理性质。主要是`rigidbody`和`collision`两个组件

### 添加物理性质

```javascript
	// 创建地板
	const floor = new pc.Entity();
	floor.addComponent("render", {
		type: "box",
		material: gray,
	});

	floor.setLocalScale(12, 0.1, 16);
	// add a rigidbody component
	floor.addComponent("rigidbody", {
		type: "static",
		restitution: 0.5,
	});
	// add a collision component
	floor.addComponent("collision", {
		type: "box",
		halfExtents: new pc.Vec3(10, 0.1, 16),
	});
	// add the floor to the hierarchy
	app.root.addChild(floor);
```

