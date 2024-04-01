# Object3D
`Object3D` во встроенной сущности в движке, которая обычно используется в качестве базового контейнера компонентов и может использоваться для реализации различных функций путем объединения разных компонентов.

![Object3D](/images/Object3D.svg)

По умолчанию, `Object3D` имеет встроенный компонент  [Transform](/guide/core/transform), который сохраняет базовую функцию контейнера только при отсутствии других компонентов и может использоваться в качестве родительского узла для добавления или объединения других `Object3D`.

`Object3D` предоставляет ряд методов, которые позволяют легко добавлять или находить дочерние объекты и прикрепленные к ним компоненты, а также устанавливать соединения с другими компонентами посредством кода.


## State of Node

Когда вы инициализируете объект `Object3D` он отображается по умолчанию. Вы можете изменить состояние узла с помощью атрибута `transform.enable` . Если установлено значение `false`, это отключит этот node и все объекты дочерних node этого node. Также все компоненты, подключенные к этим nodes также перестанут вызываться.

```ts
let obj = new Object3D();
obj.transform.enable = false; //Скрыть узел и все дочерние узлы
```

## Добавить и удалить узел

Чтобы добавить дочерний узел, вы можете использовать метод [addChild](/api/classes/Object3D#addchild).
Чтобы удалить дочерний узел, вы можете использовать метод [removeChild](/api/classes/Object3D#removeChild).
Что бы удалить дочерний узел в заданной позиции, используйте метод [removeChildByIndex](/api/classes/Object3D#removeChildByIndex).
Удалить сам дочерний узел из его родителя [removeFromParent](/api/classes/Object3D#removeFromParent) .
Удалить все дочерние узлы [removeAllChild](/api/classes/Object3D#removeAllChild) .

```ts
let parent = new Object3D();
let child = new Object3D();
//Добавить дочерний узел
parent.addChild(child);
//Удалить узел
parent.removeChild(child);
//Или удалить сам дочерний узел
child.removeFromParent();
//Или удалить все дочерние узлы
parent.removeAllChild();
```

## Добавить и удалить компоненты
Используя встроенные методы [addComponent](/api/classes/Object3D#addComponent) и [removeComponent](/api/classes/Object3D#removeComponent) объекта `Object3D`, вы можете легко добавлять и удалять компоненты.

```ts
let obj = new Object3D();
//Добавьте компонент прямого источника света в узле
let dl = obj.addComponent(DirectLight);
//удалите компонент
obj.removeComponent(DirectLight);
```

## Get Component Node
Все компоненты расширяются от `ComponentBase`, и вы можете получить текущий компонент через свойство `this.object3D` внутри компонента.
```ts
//Пользовательский компонент, чтобы увеличить положение оси x на 10 на 10
class CustomComponent extends ComponentBase {
    public start() {
        this.object3D.x += 10;
    } 
}
```

## Получить другие компоненты узла
Используя встроенный метод [getComponent](/api/classes/Object3D#getComponent) объекта `Object3D`, вы можете легко получить компоненты узла.
```ts
//Настройте компонент, чтобы изменить другой легкий компонент на узле, изменить цвет света
class CustomComponent extends ComponentBase {
    public start() {
        let light = this.object3D.getComponent(DirectLight)
        light.lightColor = new Color(1, 0, 0);
    } 
}
let obj = new Object3D();
obj.addComponent(DirectLight);
obj.addComponent(CustomComponent);
```

## Получите дочерний узел
Используя метод [getChildByIndex](/api/classes/Object3D#getChildByIndex) вы можете получить дочерний узел в порядке иерархии дочерних узлов. Используя метод [getChildByName](/api/classes/Object3D#getChildByName) вы можете получить дочерний узел по имени дочернего узла.

## Обход всех дочерних узлов
Используя метод [forChild](/api/classes/Object3D#forChild), вы можете пройти по всем дочерним узлам текущего узла, включая дочерние узлы дочерних узлов, и выполнить конкретную операцию с помощью функции обратного вызова.
```ts
// Пройти все дочерние узлы и выполнить обратный вызов
parent.forChild((child)=>{
    // Особая логика работы
})
```

## Release Object
Используя метод [destroy](/api/classes/Object3D#destroy), вы можете освободить занимаемые текущим узлом ресурсы, включая сам `Object3D` Однако по умолчанию ресурсы материалов, геометрии и текстур, необходимые в процессах рендеринга, не будут освобождены вместе с узлом. , поскольку несколько объектов могут использовать один и тот же материал и геометрию, или для будущих сцен могут потребоваться ресурсы. Если вы хотите освободить все ресурсы, вам обычно необходимо вручную освободить объекты рендеринга.

```ts
// Создать объект
let obj = new Object3D();
// Добавить компонент Meshrenderer
let mr = obj.addComponent(MeshRenderer)
let geometry = mr.geometry = new BoxGeometry()
let material = mr.material = new LitMaterial()

// уничтожить OBJ
obj.destroy() // но это НЕ освободит геометрию и объект материала
geometry.destroy() // вызвать destroy геометрии вручную
material.destroy() // вызвать destroy материала вручную
```
Вы можете использовать `Destroy(true)` с дополнительным параметром, чтобы принудительно уничтожить все связанные ресурсы узла.
> Note: Если объекты рендеринга являются общими, принудительное удаление может вызвать ошибки движка и привести к сбою рендеринга.
```ts
let obj1 = new Object3D();
let obj2 = new Object3D();

// создаем геометрию и материал
let metry = new BoxGeometry()
let material = new LitMaterial()

// установите MeshRenderer для объектов и делитесь геометрией и материальными объектами
let mr1 = obj1.addComponent(MeshRenderer)
let mr2 = obj2.addComponent(MeshRenderer)
mr1.geometry = mr2.geometry = geometry
mr2.material = mr2.material = material

// выдаст ошибки и остановит рендеринг, если уничтожит общий гео/мат
obj1.detroy(true) // это может привести к высвобождению геометрии и материала.
```

См. [Object3D](/api/classes/Object3D) API.


