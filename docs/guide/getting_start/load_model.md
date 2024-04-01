# Загрузка 3D моделей
Мы рекомендуем использовать `glTF` (Graphics Language Transmission Format) как формат загрузки модели.

Формат `glTF` , от `khronos`, обеспечивает эффективную передачу и загрузку трехмерных сцен и моделей.  `glTF` сжимает размер 3D -ресурсов для уменьшения размеров файлов приложений и трудностей с обработкой. Для получения дополнительной информации о `glTF`, пожалуйста, обратитесь к [glTF official website](https://www.khronos.org/gltf/).

## Основное использование
Простой модуль [управления ресурсами](/guide/resource/Readme) был включен в движок, мы можем использовать [loadGltf](/api/classes/Res#loadgltf) API просто для загрузки `gltf` или `glb` файлов:

```ts
let scene = new Scene3D();
// загружаем gltf файл
let data = await Engine3D.res.loadGltf('sample.gltf');
// добавляем его в сцену
scene.addChild(data);
```
Вы можете обратится к [GLTF Introduction](/guide/resource/gltf) за более подробной информацией.

## Example
Вот простой пример загрузки модели:

<Demo src="/demos/getting_start/load_model.ts"></Demo>

<<< @/public/demos/getting_start/load_model.ts{30}