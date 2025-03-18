# game-pixijs

## github
https://github.com/xiaozhu188/pixi-game-match3

## pixijs example article
https://juejin.cn/post/7265995969689452603

## 关于页面开发
https://juejin.cn/post/7265995969689452603

### 游戏首页的开发
上文已经提及过，每一个Screen本质是一个pixi的Container的容器。由于游戏首页主要以绘制及布局元素为主，关于所有页面的布局会在下篇统一讲解，因此，这里我贴一下主要代码并聊聊需要注意的几个点。

```js
/** The first screen that shows up after loading */
export class HomeScreen extends Container {
    /** Assets bundles required by this screen */
    public static assetBundles = ['home', 'common'];
    
    constructor() {
        super();
        // draw materials of screen
    }
    
    /** Resize the screen, fired whenever window size changes  */
    public resize(width: number, height: number) { ... }
    
    /** Show screen with animations */
    public async show() { 
        // play bgm
        bgm.play('common/bgm-main.mp3', { volume: 0.7 });
        
        // others..
    }
    
    /** Hide screen with animations */
    public async hide() { ... }
    
    /** and so on */
}
```

所有页面都遵循的开发思路
1. 在HomeScreen上我们定义了一个assetBundles静态属性，表示HomeScreen所需的bundle列表。其中，common指所有游戏页所通用的bundle，而home指当前页需要的bundle。从上文实现的路由管理器Navigation得知，这些bundle列表会在页面显示前被加载完毕。

2. 一个页面的生命周期大概会经历以下阶段：constructor->prepare?->resize?->update?->show?->hide?。?表示Screen是否定义了该方法。

3. constructor：实例化一系列所需的UI元素，本质是一些pixi的Graphic，Sprite，Text元素，并将其添加到画布。

4. prepare：主要实现一些即将显示的准备工作。

5. resize：UI元素的布局与大小设置。show之前会被调用一次，在窗口大小改变时依然同步调用resize，以确保元素拥有正确的位置与大小。

6. update：该方法会被作为回调函数参数，传递app.ticker.add。

7. show：通过gsap将UI元素以动画的形式显现。

8. hide：隐藏当前页。伴随app.ticker.remove等动作。

不管是哪个页面甚至是一个UI元素，都遵循上面的思路。下面附上首页最终效果图及主要标注的预览：


