# Scene3D

`Scene3D` унаследован от `Object3D`, который имеет те же свойства и методы, что и `Object3D`. Разница в том, что `Scene3D` является корневым узлом механизма рендеринга и самым высоким уровнем дерева сцены. Все узлы, которые необходимо визуализировать, необходимо добавить в `Scene3D` или в его дочерние узлы.

![Scene3D](/images/Scene3D.svg)  

Основные функции `Scene3D`:

1. `Scene3D` определяет скайбокс и отображение окружающего освещения сцены.
2. `Scene3D` может использоваться для контроля и управления узлами в дереве сцены, например: добавление, удаление, поиск узлов.

## Основное использование
```ts
await Engine3D.init();
// Создать сцену
let scene = new Scene3D();
// Добавить один узел объекта
let obj = new Object3D();
scene.addChild(obj);
// Добавить узел камеры
let cameraObj = new Object3D();
let camera = cameraObj.addComponent(Camera3D);
scene.addChild(cameraObj);
// Начать цикл рендеринга
let view = new View3D();
view.scene = scene;
view.camera = camera;
Engine3D.startRenderView(view);
// Удалить узел
scene.removeChild(obj);
```

## Atmospheric Skybox
Вы можете использовать [AtmosphericComponent](/api/classes/AtmosphericComponent.md) чтобы установить атмосферный бокс сцены.
```ts
// Добавим Atmospheric Skybox, автоматическое создание фона и света окружающей среды
let sky = scene3D.addComponent(AtmosphericComponent);
```
Подробности смотрите в следующем примере::
<Demo src="/demos/core/scene.ts"></Demo>

<<< @/public/demos/core/scene.ts

Использовать свойства `sunX`,`sunY`,`exposure` компонента `AtmosphericComponent` чтобы настраивать изменения окружающего света.

```ts
let sky = scene3D.addComponent(AtmosphericComponent);
sky.sunY = 0.54 // вертикальное положение солнца, вы можете отрегулировать яркость света окружающей среды
sky.exposure = 1.5; // экспозиция окружающего света, значение по умолчанию 1
sky.roughness = 0.5; // интенсивность размытия фона Sky box, диапазон [0, 1], по-умолчанию 0
```

### Auto Transform
В дополнение к ручному установлению значений `sunX` и `sunY`, движок также поддерживает автоматическую регулировку положения атмосферного окружающего света в соответствии с углом света в сцене.

```ts
// создаем Directional Light
let lightObj3D = new Object3D();
lightObj3D.rotationX = 46;
lightObj3D.rotationY = 62;
lightObj3D.rotationZ = 0;
let directLight = lightObj3D.addComponent(DirectLight);

let sky = scene3D.addComponent(AtmosphericComponent);
// auto change sunX/sunY на основе вращения directLight
sky.relativeTransform = directLight.transform
```

## Кастомный скайбокс
Если вы хотите настроить текстуру материала Skybox, вы можете добавить компонент `SkyRenderer` в `Scene3D` для отображения кастомного фона; в то же время вы можете установить окружающий свет через свойство `envMap` объекта `Scene3D`.


### 1. Solid Color Background и Ambient Light
Сплошной цветовой фон и окружающий свет могут быть созданы [SolidColorSky](/api/classes/SolidColorSky):
```ts
import {Scene3D, SolidColorSky, Color, SkyRenderer} from '@orillusion/core';

let scene = new Scene3D();
// Создать сплошную цветную карту
let colorSky = new SolidColorSky(new Color(0.5, 1.0, 0.8, 1))
// Добавить компонент Skyrenderer, затем установить текстуру карты
let sky = scene.addComponent(SkyRenderer);
sky.map = colorSky;

// Установите монохромный окружающий свет одновременно
scene.envMap = colorSky;
```

### 2. Cross Skybox
Вы можете установить Skybox, загрузив [cross texture cube](/guide/graphics/texture#cross-texture-cube):
```ts
// Загрузить всю cube texture
let textureCube = Engine3D.res.loadTextureCube('path/to/sky.png')
// Или загрузить 6 отдельных текстур куба
textureCube = Engine3D.res.loadTextureCube([
    'path/to/sky1.png',
    'path/to/sky2.png',
    'path/to/sky3.png',
    'path/to/sky4.png',
    'path/to/sky5.png',
    'path/to/sky6.png'
])
// Добавить компонент Skyrenderer, затем установить текстуру карты
let sky = scene.addComponent(SkyRenderer);
sky.map = textureCube;

// Установите окружающий свет
scene.envMap = textureCube;
```
> Cross Skybox в настоящее время поддерживает только формат изображений `LDR` normal.

### 3. Equirectangular Skybox
Движок также поддерживает установку [equirectangular](https://en.wikipedia.org/wiki/Equirectangular_projection) скайбокс. Мы можем быстро загрузить изображение формата normal `RGBA`  `LDR` images встроенным `res`, а также поддерживает загрузку изображений формата `RGBE` `HDR`:
```ts
// Normal equirectangular skybox
let skyTexture = Engine3D.res.loadLDRTextureCube('path/to/sky.png');
// HDR equirectangular skybox
skyTexture = Engine3D.res.loadHDRTextureCube('path/to/sky.hdr');

// Добавьте компонент SkyRenderer, затем установите текстуру карты
let sky = scene.addComponent(SkyRenderer);
sky.map = skyTexture;

// ambient light
scene.envMap = skyTexture;
```

### 4. Прозрачный фон
Если вы хотите отобразить прозрачный фон, вы можете скрыть его, отключив компонент скайбокса. Обратите внимание, что для вступления в силу обычно необходимо использовать прозрачный [Canvas](/guide/core/engine#config-canvas):

```ts
// инициализация движка
await Engine3D.init({
    canvasConfig:{
        alpha: true, //использование конфигурации прозрачного Canvas
        zIndex: 1
    }
});
let scene = new Scene3D();

// Сначала добавьте атмосферный Skybox, чтобы получить базовый окружающий свет
let sky = scene3D.addComponent(AtmosphericComponent);
// Затем скрыть атмосферный Skybox, окружающий свет не исчезнет
sky.enable = false
```
Конечно, вы также можете не добавлять атмосферный скайбокс или фон, а напрямую задать эмбиент:
```ts
// Установите простой белый рассеянный свет
scene.envMap = new SolidColorSky(new Color(0.75, 0.75, 0.75));
// Или загрузите карту окружения
scene.envMap = await Engine3D.res.loadHDRTextureCube('path/to/sky.hdr');
```


Дополнительные сведения об использовании см. в [Scene3D](/api/classes/Scene3D)