# EngineSetting
Вы можете установить некоторые общие конфигурации движка через [EngineSetting](/api/types/EngineSetting), `EngineSetting` в основном состоит из нескольких конфигураций, включая pick mode, render pipeline, shadow settings, post-processing settings, skybox settings, etc.

## Основное использование
Перед инициализацией движка необходимо сначала установить конфигурацию движка, которая может быть установлена через свойство `setting` объекта `Engine3D`.

Например, установите максимальное количество источников света, поддерживаемых на сцене:
```ts
// Максимальное количество Источников света
Engine3D.setting.light.maxLight = 1024;
// Включить использование log depth
Engine3D.setting.render.useLogDepth = true;
// Сначала настроить, затем инициализируйте
await Engine3D.init();
```

## Pick Mode
Engine поддерживает два режима pick, первый `pixel` и второй `bound`.

По умолчанию стоит режим `bound`, который выбирает модель, вычисляя AABB габаритный бох модели. Точность не так хороша, как в режиме `pixel`, но расчет быстрее, а производительность лучше. Режим bounding box picking можно установить свойством `pick` конфигуратора движка.

```ts
Engine3D.setting.pick.enable = true;
Engine3D.setting.pick.mode = `bound`;
await Engine3D.init();
```

Кроме того, режим pixel picking также может быть установлен через свойство `pick`.

```ts
Engine3D.setting.pick.enable = true;
Engine3D.setting.pick.mode = `pixel`;
await Engine3D.init();
```

Узнайте больше о [Pick Event](/guide/interaction/pickfire)

## Настройки пост-процессинга
Engine поддерживает несколько эффектов пост-обработки, в том числе различные anti-aliasing, bloom, ambient occlusion, etc., который может быть установлен через свойство `postProcessing` конфигуратора `render`.

Например, установите `bloom` post-processing эффект:
```ts
// включаем bloom 
Engine3D.setting.render.postProcessing.bloom.enable = true;
// задаем интенсивность bloom
Engine3D.setting.render.postProcessing.bloom.intensity = 0.5;
```
Узнайте больше о [Post Processing](/guide/advanced/posteffect)

## Тени
Установка метода тени и атрибутов через свойство `shadow` конфигуратора движка.

```ts
Engine3D.setting.shadow.enable = true; // включить тени
Engine3D.setting.shadow.type = `SOFT`; // тип теней, SOFT
Engine3D.setting.shadow.shadowSize = 2048; // размер карты теней
Engine3D.setting.shadow.shadowBound = 20; // граница тени
```
Узнайте больше о [Shadow](/guide/graphics/shadow)

<!-- ## Global Illumination Settings
Настройка глобального освещения через свойство `gi` конфигурации.
```ts
Engine3D.setting.gi.enable = true;
```
Узнайте больше о [Global Illumination](/guide/advanced/gi) -->

