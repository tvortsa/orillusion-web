# Отрисовка куба

В этом разделе мы быстро рассмотрим использование этапов движка через пример рисования куба:

<Demo src="/demos/getting_start/cube.ts"></Demo>

<<< @/public/demos/getting_start/cube.ts

## Импорт модулей

Во -первых, нам нужно импортировать соответствующие модули:

```ts
import {
    Color,
    Engine3D,
    Scene3D,
    Object3D,
    Camera3D,
    View3D,
    LitMaterial,
    BoxGeometry,
    MeshRenderer,
    DirectLight,
    HoverCameraController,
    AtmosphericComponent
} from '@orillusion/core';
```

| Модули               | Описание                                                                                                                                             |
|-----------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------|
| Color                 | Дает определение цвета                                                                                                                         |
| Engine3D              | Класс Engine3D это основа движка, включая инициализацию движка, запуск рендеринга и другие основные методы                                  |
| Scene3D               | Создав новый класс Scene3D, вы можете создать экземпляр сцены, который обычно используется в качестве root node в программе                                 |
| Object3D              | Класс Object3D определяет объект для объекта (предмета), который содержит общие свойства объекта (предмета), такие как положение, вращение и другие параметры |
| Camera3D              | Создав новый класс Camera3D вы можете создать экземпляр камеры 3D компонент, который может быть добавлен в сцену как camera node                |
| View3D                | View3D, укажите целевую сцену и камеру наблюдения для engine rendering                                                                            |                                                                                                                                               |
| LitMaterial           | Класс LitMaterial позволяет создавать экземпляры материала и устанавливать параметры материала для достижения различных эффектов материала.                        |
| BoxGeometry           | Класс BoxGeometry позволяет создать прямоугольную(box) геометрию                                                                                  |
| MeshRenderer          | Класс MeshRenderer component предоставляет рендеринг геометрии объекта сетки для объектов                                                                          |
| DirectLight           | DirectLight component позволяет установить цвет, свойства интенсивности и угол света, чтобы получить подходящий световой эффект                   |
| HoverCameraController | HoverCamera component позволяет управлять движением камеры вокруг точки наблюдения                                                                    |
| AtmosphericComponent  | Встроенный компонент skybox                                                                                                                            |

## Инициализация движка

```ts
await Engine3D.init();
```

## Создание новой сцены (Scene Root-node)

```ts
let scene3D = new Scene3D();
```

## Добавляем скайбох
```ts
// Добавить компонент атмосферного рассеяния Skybox
let sky = scene3D.addComponent(AtmosphericComponent);
```
## Добавляем компонент Camera Controller

```ts
// Создать объект камеры
let cameraObj = new Object3D();
let camera = cameraObj.addComponent(Camera3D);
// Установите перспективу камеры в соответствии с размером окна
camera.perspective(60, window.innerWidth / window.innerHeight, 1, 5000.0);
// Установите контроллер камеры
let controller = camera.object3D.addComponent(HoverCameraController);
controller.setCamera(0, 0, 15);
// Добавить узел камеры в сцену
scene3D.addChild(cameraObj);
```

## Добавляем источник света в Scene

```ts
// Создаем объект light
let light = new Object3D();
// Добавляем компонент direct light
let component = light.addComponent(DirectLight);
// Настраиваем параметры источника света
light.rotationX = 45;
light.rotationY = 30;
component.intensity = 2;
// добавляем узел light к сцене
scene3D.addChild(light);
```

## Создаем New Object и Add MeshRenderer

После добавления Meshrenderer к объекту нам нужно прикрепить геометрию и материалы к Meshrenderer объекта.

```ts
// Создать новый объект
const obj = new Object3D();
// Добавляем MeshRenderer к object(obj)
let mr = obj.addComponent(MeshRenderer);
// настраиваем геометрию
mr.geometry = new BoxGeometry(5, 5, 5);
// задаем материал
mr.material = new LitMaterial();
```

## Добавляем объект к сцене

```ts
scene3D.addChild(obj);
```

## Рендер сцены

```ts
// Создаем объект View3D
let view = new View3D();
// Укажите сцену, для рендера
view.scene = scene3D;
// Задаем используемую камеру
view.camera = camera;
// Старт рендера
Engine3D.startRenderView(view);
```