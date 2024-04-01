# Script

Мы узнали о жизненном цикле [компонента](/guide/core/component) в [Component Lifecycle](/guide/core/component#life-cycle), пользователи могут расширять класс [ComponentBase](/api/classes/ComponentBase) для разработки кастомных компонентов. Пользователи могут настроить логику работы, перезаписав функции жизненного цикла базового класса компонента:
 - `Initialize/stop`: как `init` и `destroy`
 - `State change`: как `start`, `onEnable` и `onDisable`
 - `Update logic`: как `onUpdate`, `onLateUpdate` и `onBeforeUpdate`


## Основное использование
Добавить пользовательский сценарий в сущность
```ts
class Script extends ComponentBase {
    //Перезапись init
    public init() {
        //Эта функция вызывается при создании компонента и может использоваться для инициализации внутренних переменных.
        //Обратите внимание, что на данный момент компонент не подключен к Object3D, поэтому доступ к нему с помощью this.object3D невозможен.
    }
    // Перезапись start
    public start() {
        // Эта функция вызывается до того, как компонент начнет рендеринг,
        // В настоящее время вы можете получить доступ к this.object3D, чтобы получить атрибуты узла или других компонентов.
    }
    // Перезапись onUpdate
    public onUpdate() {
        // Эта функция называется циклом рендеринга каждого кадра и обычно определяет логику цикла узла.
        // Например, обновляйте угол поворота объекта каждый кадр.
        this.object3D.rotationY += 1;
    }
}

let ball: Object3D = new Object3D();
ball.addComponent(Script);
```
В пользовательском скрипте вы можете получить объект `object3D` текущий компонент которого смонтирован через `this.object3D`, а затем измените состояние объекта.  
Ключевым моментом разработки игры или анимации является обновление поведения, состояния и ориентации объекта перед рендерингом каждого кадра. Эти операции обновления обычно могут быть определены в обратном вызове `onUpdate` самого компонента. Движок автоматически зарегистрирует коллбэк `onUpdate` в главном цикле чтобы обновлять логику в каждом кадре.

## Пример
Вот три различных примера анимации сценариев, чтобы показать вам более сложное использование компонентов сценария.

### 1. Скрипт компонента анимации источника света
---
<Demo src="/demos/core/script_light.ts"></Demo>

<<< @/public/demos/core/script_light.ts

```ts
class LightAnimation extends ComponentBase {
    private light: DirectLight;
    private color: Color;

    // Перезаписать начало инициализации переменных start
    protected start() {
        this.light = this.object3D.getComponent(DirectLight);
        this.color = this.light.lightColor;
    }

    onUpdate() {
        // обновляем lightColor
        this.color.r = Math.pow(Math.sin(Time.time * 0.001), 10);
        this.light.lightColor = this.color;
    }
}
```
Здесь мы меняем красный компонент цвета света каждый кадр, чтобы он менялся со временем и, наконец, создавал эффект световой анимации.

### 2. Скрипт компонента анимации материала
---
<Demo src="/demos/core/script_mat.ts"></Demo>

<<< @/public/demos/core/script_mat.ts

```ts
class MaterialAnimation extends ComponentBase {
    private material: LitMaterial;

    // Overwrite start initialize variables of start
    protected start() {
        let mr = this.object3D.getComponent(MeshRenderer);
        this.material = mr.material;
    }

    onUpdate() {
        // Update baseColor
        let delta = Time.time * 0.001
        this.material.baseColor = new Color(Math.sin(delta), Math.cos(delta), Math.sin(delta));
    }
}
```
Как и выше, мы можем изменить материальный объект объекта, например, изменив цвет материала в зависимости от времени, чтобы добиться соответствующего эффекта анимации.

### 3. Path Animation Script Component
---
<Demo src="/demos/core/script_path.ts"></Demo>

<<< @/public/demos/core/script_path.ts

```ts
class PathAnimation extends ComponentBase {
    onUpdate() {
    // Update Position
    this.object3D.x = Math.sin(Time.time * 0.001) * 2;
    this.object3D.y = Math.cos(Time.time * 0.001) * 2;
  }
}
```
В этом случае мы изменяем свойство `Position` объекта `Object3D`, чтобы объект со временем перемещался по кругу в плоскости `xy`.
