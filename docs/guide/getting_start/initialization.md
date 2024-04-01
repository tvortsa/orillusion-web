# Инициализация
## Создание экземпляра Engine3D 
Перед использованием движка он должен быть инициализирован методом `Engine3D.init()`, и движок автоматически создаст экземпляр `Engine3d` для следующих операций
```ts
import { Engine3D } from '@orillusion/core';

Engine3D.init().then(()=>{
  // for following operations
})l
```
Обратите внимание, что `Engine3D.init()` это асинхронное API, рекомендуется использовать с `async/await`

```ts
import { Engine3D } from '@orillusion/core';

async function demo(){
  await Engine3D.init();
  // for following operations
}
demo();
```

## Создайте холст вручную
По умолчанию экземпляр `Engine3D.Init ()` автоматически генерирует `холст` с размером экрана (ширина и высота). Если вы не хотите использовать холст, автоматически созданный движком, вы также можете создать холст вручную.
Например, пользователь может вставить метку `<canvas>` в HTML и задатm ей id:
```html
<canvas id="canvas" width="800" height="500" />
```

затем использовать `document.getElementById`  в Typescript чтобы получить canvas:
```ts
let canvas = document.getElementById('canvas');
```

и использовать `canvasConfig` для передачи параметров `canvas` в метод `init()` для инициализации:
```ts
import { Engine3D } from '@orillusion/core';

let canvas = document.getElementById('canvas');
await Engine3D.init({
  canvasConfig: { canvas }
});
```

Получите больше информации о конфигурации от [Engine3D](/guide/core/engine)