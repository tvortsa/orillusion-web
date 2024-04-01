# Transform

[Transform](/api/classes/Transform) это встроенный базовый [Component](/guide/core/component), который будет добавлен во все `Object3D` по-умолчанию, используется для управления `position`, `scale` и `rotation` параметров координат контейнера.

---
<Demo src="/demos/core/transform.ts"></Demo>

<<< @/public/demos/core/transform.ts

## Position
Положение узла относительно родительского контейнера
```ts
let obj = new Object3D();
// первый метод (рекомендуемый)
obj.x = 0;
obj.y = 0;
obj.z = 0;
// или
obj.transform.x = 0;
obj.transform.y = 0;
obj.transform.z = 0;
// второй метод
obj.transform.localPosition.set(0,0,0);
// третий метод
obj.transform.localPosition = new Vector3(0,0,0);
```

## Вращение 
Вращение узла относительно родительского контейнера
```ts
let obj = new Object3D();
// первый метод (рекомендуемый)
obj.rotationX = 0;
obj.rotationY = 0;
obj.rotationZ = 0;
// или
obj.transform.rotationX = 0;
obj.transform.rotationY = 0;
obj.transform.rotationZ = 0;
// второй метод
obj.transform.localRotation.set(0,0,0);
// третий метод
obj.transform.localRotation = new Vector3(0,0,0);
```

## Масштаб
Масштаб узла относительно родительского контейнера
```ts
let obj = new Object3D();
// The frist method
obj.scaleX = 1;
obj.scaleY = 1;
obj.scaleZ = 1;
// Or
obj.transform.scaleX = 1;
obj.transform.scaleY = 1;
obj.transform.scaleZ = 1;
// The second method
obj.transform.localScale.set(1,1,1);
// The third method
obj.transform.localScale = new Vector3(0,0,0);
```

Увидеть больше использования в [Transform](/api/classes/Transform) API