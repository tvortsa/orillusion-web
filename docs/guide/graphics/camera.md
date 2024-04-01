# Camera

Камера — это инструмент, позволяющий пользователям отображать или снимать виртуальный мир, точно так же, как глаза, которые наблюдают за происходящим в реальном мире. Все крутые изображения нужно визуализировать через камеру. В каждой сцене должна присутствовать хотя бы одна камера для просмотра объектов в сцене. `Orillusion` уже инкапсулировал наиболее часто используемые [типы камер](#camera-type) и [контроллеры](#camera-component) и пользователи также могут расширить функции камеры с помощью [кастомных компонентов](/guide/core/component).

## Основное использование
```ts
import { Object3D, Scene3D, Camera3D } from '@orillusion/core'
// Инициализировать сцену
let scene = new Scene3D();
// Инициализировать узел камеры
let cameraObj = new Object3D();
// Добавить компонент камеры в узел
let camera = cameraObj.addComponent(Camera3D);
// Добавить узел камеры в сцену
scene.addChild(cameraObj);

// Create 3D view
let view = new View3D();
// Заполнить сцену до 3D view
view.scene = scene;
// Заполнить камеру до 3D view
view.camera = camera;
// Start rendering
Engine3D.startRenderView(view);
```
Если в сцене несколько камер, вы можете вручную переключить целевую камеру с помощью `view.camera`:
```ts
// Если в сцене присутствует несколько камер
let cameraObj1 = new Object3D();
let camera1 = cameraObj.addComponent(Camera3D);
let cameraObj2 = new Object3D();
let camera2 = cameraObj.addComponent(Camera3D);

// Create 3D view
let view = new View3D();
// установка сцены для рендеринга
view.scene = scene;
// camera1 Set camera1
view.camera = camera1;
...
// Переключитесь на использование камеры2 для рендеринга
view.camera = camera2;

```

## Camera Position
Есть три способа изменить положение камеры:
1. С помощью преобразования `TransForm`: Положение и угол направления камеры можно задать вручную с помощью свойства [transForm](/guide/core/transform) свойство узла камеры `Object3D`:
```ts
// Create a node
let cameraObj = new Object3D();
// Добавить компонент камеры в узел
let camera = cameraObj.addComponent(Camera3D);
// задаем Position или Rotation объекта Object3D
cameraObj.x = 10;
cameraObj.rotateX = 90;
...
```

2. С помощью функции `lookAt` компонента: Функция [lookAt](/api/classes/Camera3D#lookat) компонента camera могут задавать position camera `Object3D` и position цели, которая будет наблюдаться в то же время:

```ts
// Создаем node
let cameraObj = new Object3D();
// Добавить компонент камеры в узел
let camera = cameraObj.addComponent(Camera3D);
// Используйте функцию LookAt компонента Camera3D, чтобы изменить положение и угол направления Object3D.
camera.lookAt(new Vector3(0,0,10), new Vector3(0,0,0), new Vector3(0,0,1));
```
| Параметр | Тип    | Описание                                                  | Пример          |
|-----------|---------|--------------------------------------------------------------|------------------|
| pos       | Vector3 | Положение самого объекта (глобальное)                   | Vector3(0, 0, 0) |
| target    | Vector3 | Положение цели (глобальное)                          | Vector3(0, 1, 0) |
| up        | Vector3 | Ось координат направления камеры вверх. | Vector3(0, 1, 0) |
3. Camera Controller: В движок встроено несколько общих [компонентов контроллера](#camera-component) которые могут автоматически регулировать свойства положения камеры в соответствии с вводом пользователя.


## Тип камеры
Движок в основном поддерживает ортогональные камеры и перспективные камеры, которые разработчики могут использовать в настоящее время.

### Ортографическая проекция

В орфографическом режиме камеры размер объекта не меняется независимо от того, насколько далеко он находится от камеры. Обычно мы используем ортогональные камеры в 2D-чертежах и устанавливаем координату `z` coordinate на `0.0` в нашей геометрической графике. Но ось `z` может быть расширена до любой желаемой длины. Используя ортогональную камеру для проецирования объекта отображения, результат масштабируется пропорционально без искажений.

![camera_orthoOffCenter](/images/camera_orthoOffCenter.webp)

Используйте  [camera.orthoOffCenter](/api/classes/Camera3D.html#orthooffcenter) API чтобы установить camera в orthographic camera:

| Параметр | Тип   | Описание                                                   | Пример                 |
|-----------|--------|---------------------------------------------------------------|-------------------------|
| left      | number | Минимальное значение оси x пирамиды вида        | -window.innerWidth / 2  |
| right     | number | Максимальное значение оси x пирамиды вида        | window.innerWidth / 2   |
| bottom    | number | The minimum значение оси y пирамиды вида        | -window.innerHeight / 2 |
| top       | number | The maximum значение оси y пирамиды вида        | window.innerHeight / 2  |
| near      | number | z значение ближней плоскости отсечения усеченной пирамиды обзора | 1                       |
| far       | number | z значение дальней плоскости отсечения усеченной пирамиды обзора  | 5000                    |

### Перспективная проекция
Перспективная проекция будет использовать разделение перспективы для укорочения и уменьшения объектов, находящихся далеко от наблюдателя. Объекты с одинаковым логическим размером кажутся больше в передней позиции, чем в задней в видимой области, что позволяет достичь эффекта наблюдения, близкого к человеческому глазу. Это наиболее часто используемый режим проецирования в 3D-сценах.

![camera_perspective](/images/camera_perspective.webp)

Используйте  [camera.perspective](/api/classes/Camera3D#perspective) API чтобы установить перспективную камеру:

| Параметр | Тип   | Описание                                                   | Пример                                |
|-----------|--------|---------------------------------------------------------------|----------------------------------------|
| fov       | number | Степень перспективы                                        | 60                                     |
| aspect    | number | Соотношение сторон окна просмотра                              | window.innerWidth / window.innerHeight |
| near      | number | Значение z ближней плоскости отсечения усеченной пирамиды обзора | 0.1                                    |
| far       | number | Значение z дальней плоскости отсечения усеченной пирамиды обзора  | 1000                                   |

<Demo :height="500" src="/demos/graphics/camera_type.ts"></Demo>

<<< @/public/demos/graphics/camera_type.ts{35-41}

## Компонент Camera
Компонент «Камера» обеспечивает гибкую поддержку расширений для камеры, которую можно использовать непосредственно с предопределенными компонентами или настроить для реализации более персонализированных требований. Компонент выполняет свою собственную логику `update` синхронно с основным циклом `Engine3D` через свою функцию обновления.

### [FlyCamera](/api/classes/FlyCameraController)
Этот контроллер реализует свободное перемещение камеры. Особенности его взаимодействия:
  - Двигайтесь вперед, назад, влево и вправо, нажимая W A S D
  - Управляйте направлением движения камеры, удерживая левую кнопку мыши.

<Demo :height="500" src="/demos/graphics/camera_fly.ts"></Demo>

<<< @/public/demos/graphics/camera_fly.ts

Основное использование:
```ts
import { Scene3D, Camera3D, FlyCameraController } from '@orillusion/core'
// Создать узел
let cameraObj = new Object3D();
// Добавить компонент камеры в node
let camera = cameraObj.addComponent(Camera3D);
// Добавить компонент контроллера в узел
let flyController = cameraObj.addComponent(FlyCameraController);
// Установите положение камеры через компонент setCamera
flyController.setCamera(new Vector3(0, 0, 15), new Vector3(0, 0, 0));
// Установите скорость движения мыши
flyController.moveSpeed = 10;
```
Летающую камеру можно настроить с помощью [setCamera](/api/classes/FlyCameraController#setcamera) чтобы установить собственное положение и ориентацию

| Параметр | Тип    | Описание                | Пример             |
|-----------|---------|----------------------------|---------------------|
| targetPos | Vector3 | Положение камеры | new Vector3(0,0,10) |
| lookAtPos | Vector3 | Положение цели | new Vector3(0,0,0)  |

Кроме того, вы можете изменить `moveSpeed` чтобы настроить скорость движения

| Параметр | Тип   | Описание           | Пример |
|-----------|--------|-----------------------|---------|
| moveSpeed | number | Скорость движения | 10      |

### [HoverCamera](/api/classes/HoverCameraController)

Этот контроллер камеры реализует перемещение камеры в плоскости `xz` и вращение вокруг текущей точки наблюдения. особенности его взаимодействия::
  - Нажмите левую кнопку мыши и перемещайте мышь, чтобы повернуть камеру вокруг текущей цели наблюдения.
  - Нажмите правую кнопку мыши и перемещайте мышь, чтобы плавно перемещать камеру в направлении и на расстоянии движения мыши в видимой области текущей сцены.
  - Прокрутите колесо мыши, чтобы контролировать расстояние обзора камеры.

<Demo :height="500" src="/demos/graphics/camera_hover.ts"></Demo>

<<< @/public/demos/graphics/camera_hover.ts

Основное использование:
```ts
import { Scene3D, Camera3D, HoverCameraController } from '@orillusion/core'
// Create a node
let cameraObj = new Object3D();
// Добавить компонент камеры в узел
let camera = cameraObj.addComponent(Camera3D);
// Добавить компонент контроллера в узел
let hoverCameraController = cameraObj.addComponent(HoverCameraController);
// Установите положение камеры через компонент SetCamera
hoverController.setCamera(15, -15, 15, new Vector3(0, 0, 0));
```
Камерой наведения (hover) можно управлять с помощью [setCamera](/api/classes/HoverCameraController#setcamera) чтобы установить ее собственное положение и ориентацию.

| Параметр | Тип    | Описание              | Пример            |
|-----------|---------|--------------------------|--------------------|
| roll      | number  | Вращение вокруг оси Y | 0                  |
| pitch     | number  | Вращение вокруг оси x | 0                  |
| distance  | number  | Расстояние до цели | 10                 |
| target    | Vector3 | координаты цели        | new Vector3(0,0,0) |


### [Orbit](/api/classes/OrbitController)
Этот контроллер камеры аналогичен камере при наведении, но он может напрямую задавать положение и ориентацию камеры `Object3D` для управления положением и ориентацией вида. Основные особенности заключаются в следующем:
  - Нажмите левую кнопку мыши и перемещайте мышь, чтобы повернуть камеру вокруг текущей цели наблюдения.
  - Нажмите правую кнопку мыши и перемещайте мышь, чтобы плавно перемещать камеру в направлении и на расстоянии движения мыши в видимой области текущей сцены..
  - Прокрутите колесо мыши, чтобы контролировать расстояние обзора камеры.
  - Вы можете настроить камеру на автоматический поворот
  - Вы можете установить скорость вращения, масштабирования и перевода.
  - Вы можете установить максимальный и минимальный углы подъема.

<Demo :height="500" src="/demos/graphics/camera_orbit.ts"></Demo>

<<< @/public/demos/graphics/camera_orbit.ts{12-17}


Основное использование:
```ts
import { Scene3D, Camera3D, OrbitController } from '@orillusion/core'
// Создать узел
let cameraObj = new Object3D();
// Добавить компонент камеры в узел
let camera = cameraObj.addComponent(Camera3D);
// Добавить компонент контроллера в узел
let orbit = cameraObj.addComponent(OrbitController);
// Установите положение камеры Object3D
cameraObj.localPosition.set(0, 10, 30);
// Включить автоматическое вращение
orbit.autoRotate = true
// Автоматическая скорость вращения
orbit.autoRotateSpeed = 0.1
// Коэффициент скорости увеличения
orbit.zoomFactor = 0.1
// Коэффициент скорости перевода вида
orbit.panFactor = 0.25
// Коэффициент сглаживания угла вида
orbit.smooth = 5
// минимальное расстояние масштабирования
orbit.minDistance = 1
// Максимальное расстояние масштабирования
orbit.maxDistance = 1000
// Минимальный угол высоты
orbit.minPolarAngle = -90
// Максимальный угол высоты
orbit.minPolarAngle = 90
```

### Пользовательский контроллер
Пользователи могут расширить дополнительные компоненты камеры через [custom components](/guide/core/script), См. [OrbitController](https://github.com/Orillusion/orillusion/blob/main/src/components/controller/OrbitController.ts) как пример.