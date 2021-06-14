# Classes

ES6 classes are a simple sugar over the prototype-based OO pattern.

Classes support prototype-based inheritance, super calls, instance and static methods and constructors.

```javascript
class SkinnedMesh extends THREE.Mesh {
  constructor(geometry, materials) {
    super(geometry, materials);

    this.idMatrix = SkinnedMesh.defaultMatrix();
    this.bones = [];
    this.boneMatrices = [];
    //...
  }
  sayHi = () => 'hello, world'
  update(camera) {
    //...
    super.update();
  }
  get boneCount() {
    return this.bones.length;
  }
  set matrixType(matrixType) {
    this.idMatrix = SkinnedMesh[matrixType]();
  }
  static defaultMatrix() {
    return new THREE.Matrix4();
  }
}
```

## Mix-ins

```js
let calculatorMixin = Base => class extends Base {
  calc() {
    return 'calculatorMixin'
  }
};

let randomizerMixin = Base => class extends Base {
  calc() {
    return 'randomizerMixin'
  }
};

class Foo {}
class Bar extends randomizerMixin(calculatorMixin(Foo)) {}

let a = new Bar()

console.log(a.calc())
```