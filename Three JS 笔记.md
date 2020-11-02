# Three JS 笔记

## **Three.js**程序

> **一个典型的Three.js程序至少要包括渲染器（Renderer）、场景（Scene）、照相机（Camera），以及你在场景中创建的物体。**

## 渲染器

```javascript
var renderer = new THREE.WebGLRenderer({
    canvas: document.getElementById('mainCanvas')
});
```

### 设置背景颜色

```javascript
renderer.setClearColor(0x000000);
```

## 场景

```javascript
var scene = new THREE.Scene();
```

## 相机

### 透视投影相机

#### 构造函数

```javascript
THREE.PerspectiveCamera(fov, aspect, near, far)
```

> `aspect`等于`width / height`，是照相机水平方向和竖直方向长度的比值，通常设为Canvas的横纵比例。
>
> `near`和`far`分别是照相机到视景体最近、最远的距离，均为正值，且`far`应大于`near`。

#### 设置位置

```javascript
var camera = new THREE.PerspectiveCamera(45, 4 / 3, 1, 1000);
camera.position.set(0, 0, 5);
scene.add(camera);
```



### 正交投影相机

#### 构造函数

```javascript
THREE.OrthographicCamera(left, right, top, bottom, near, far)
```

> 为了保持照相机的横竖比例，需要保证`(right - left)`与`(top - bottom)`的比例与Canvas宽度与高度的比例一致。

设置相机位置

```javascript
var camera = new THREE.OrthographicCamera(-2, 2, 1.5, -1.5, 1, 10);
camera.position.set(0, 0, 5);
scene.add(camera);
```

设置相机角度

```javascript
camera.lookAt(new THREE.Vector3(0, 0, 0));
```



## 几何形状

### 长方体

#### 构造函数

```javascript
THREE.CubeGeometry(width, height, depth, widthSegments, heightSegments, depthSegments)
```

> `width`是x方向上的长度；`height`是y方向上的长度；`depth`是z方向上的长度
>
> 后三个参数分别是在三个方向上的分段数，如`widthSegments`为`3`的话，代表x方向上水平分为三份

```javascript
var cube = new THREE.Mesh(new THREE.CubeGeometry(1, 2, 3),
        new THREE.MeshBasicMaterial({
            color: 0xff0000
        })
);
scene.add(cube);
```

### 平面

#### 构造函数

```javascript
THREE.PlaneGeometry(width, height, widthSegments, heightSegments)
```

> `width`是x方向上的长度；`height`是y方向上的长度；后两个参数同样表示分段。

### 球体

#### 构造函数

```javascript
THREE.SphereGeometry(radius, segmentsWidth, segmentsHeight, phiStart, phiLength, thetaStart, thetaLength)
```

> `radius`是半径；`segmentsWidth`表示经度上的切片数；`segmentsHeight`表示纬度上的切片数；`phiStart`表示经度开始的弧度；`phiLength`表示经度跨过的弧度；`thetaStart`表示纬度开始的弧度；`thetaLength`表示纬度跨过的弧度。

### 圆形

#### 构造函数

```javascript
THREE.CircleGeometry(radius, segments, thetaStart, thetaLength)
```

### 圆柱体

#### 构造函数

```javascript
THREE.CylinderGeometry(radiusTop, radiusBottom, height, radiusSegments, heightSegments, openEnded)
```

> `radiusTop`与`radiusBottom`分别是顶面和底面的半径
>
> `height`是圆柱体的高度；`radiusSegments`与`heightSegments`可类比球体中的分段
>
> `openEnded`是一个布尔值，表示是否没有顶面和底面，缺省值为`false`，表示有顶面和底面

### 正四面体

```
THREE.TetrahedronGeometry(radius, detail)
```

### 正八面体

```
THREE.OctahedronGeometry(radius, detail)
```

### 正二十面体

```
THREE.IcosahedronGeometry(radius, detail)
```

> `radius`是半径；`detail`是细节层次（Level of Detail）的层数，对于大面片数模型，可以控制在视角靠近物体时，显示面片数多的精细模型，而在离物体较远时，显示面片数较少的粗略模型。

### 圆环面

#### 构造函数

```javascript
THREE.TorusGeometry(radius, tube, radialSegments, tubularSegments, arc)
```

> ，`radius`是圆环半径；`tube`是管道半径；`radialSegments`与`tubularSegments`分别是两个分段数，详见上图；`arc`是圆环面的弧度，缺省值为`Math.PI * 2`。

### 圆环结

```javascript
THREE.TorusKnotGeometry(radius, tube, radialSegments, tubularSegments, p, q, heightScale)
```

> `heightScale`是在z轴方向上的缩放

## 文字

```javascript
var loader = new THREE.FontLoader();
loader.load('../lib/helvetiker_regular.typeface.json', function(font) {
    var mesh = new THREE.Mesh(new THREE.TextGeometry('Hello', {
        font: font,
        size: 1,
        height: 1
    }), material);
    scene.add(mesh);

    // render
    renderer.render(scene, camera);
});
```

#### 构造函数

```
THREE.TextGeometry(text, parameters)
```

> 其中，`text`是文字字符串，`parameters`是以下参数组成的对象：
>
> - `size`：字号大小，一般为大写字母的高度
> - `height`：文字的厚度
> - `curveSegments`：弧线分段数，使得文字的曲线更加光滑
> - `font`：字体，默认是`'helvetiker'`，需对应引用的字体文件
> - `weight`：值为`'normal'`或`'bold'`，表示是否加粗
> - `style`：值为`'normal'`或`'italics'`，表示是否斜体
> - `bevelEnabled`：布尔值，是否使用倒角，意为在边缘处斜切
> - `bevelThickness`：倒角厚度
> - `bevelSize`：倒角宽度

## 自定义形状

#### 构造函数

```
THREE.Geometry()
```

示例

```javascript
// 初始化几何形状
var geometry = new THREE.Geometry();

// 设置顶点位置
// 顶部4顶点
geometry.vertices.push(new THREE.Vector3(-1, 2, -1));
geometry.vertices.push(new THREE.Vector3(1, 2, -1));
geometry.vertices.push(new THREE.Vector3(1, 2, 1));
geometry.vertices.push(new THREE.Vector3(-1, 2, 1));
// 底部4顶点
geometry.vertices.push(new THREE.Vector3(-2, 0, -2));
geometry.vertices.push(new THREE.Vector3(2, 0, -2));
geometry.vertices.push(new THREE.Vector3(2, 0, 2));
geometry.vertices.push(new THREE.Vector3(-2, 0, 2));

// 设置顶点连接情况
// 顶面
geometry.faces.push(new THREE.Face3(0, 1, 3));
geometry.faces.push(new THREE.Face3(1, 2, 3));
// 底面
geometry.faces.push(new THREE.Face3(4, 5, 6));
geometry.faces.push(new THREE.Face3(5, 6, 7));
// 四个侧面
geometry.faces.push(new THREE.Face3(1, 5, 6));
geometry.faces.push(new THREE.Face3(6, 2, 1));
geometry.faces.push(new THREE.Face3(2, 6, 7));
geometry.faces.push(new THREE.Face3(7, 3, 2));
geometry.faces.push(new THREE.Face3(3, 7, 0));
geometry.faces.push(new THREE.Face3(7, 4, 0));
geometry.faces.push(new THREE.Face3(0, 4, 5));
geometry.faces.push(new THREE.Face3(0, 5, 1));
```

## 材质

### 基本材质

```java
THREE.MeshBasicMaterial(opt)
```

> `opt`可以缺省，或者为包含各属性的值



> 常用的属性。
>
> - `visible`：是否可见，默认为`true`
> - `side`：渲染面片正面或是反面，默认为正面`THREE.FrontSide`，可设置为反面`THREE.BackSide`，或双面`THREE.DoubleSide`
> - `wireframe`：是否渲染线而非面，默认为`false`
> - `color`：十六进制RGB颜色，如红色表示为`0xff0000`
> - `map`：使用纹理贴图

### Lambert材质

> Lambert材质（MeshLambertMaterial）是符合Lambert光照模型的材质。Lambert光照模型的主要特点是只考虑漫反射而不考虑镜面反射的效果，因而对于金属、镜子等需要镜面反射效果的物体就不适应，对于其他大部分物体的漫反射效果都是适用的。

#### 构造函数

```
new THREE.MeshLambertMaterial({
    color: 0xffff00
})
```

> `color`是用来表现材质对散射光的反射能力，也是最常用来设置材质颜色的属性
>
> `ambient`表示对环境光的反射能力，只有当设置了`AmbientLight`后，该值才是有效的，材质对环境光的反射能力与环境光强相乘后得到材质实际表现的颜色。
>
> `emissive`是材质的自发光颜色，可以用来表现光源的颜色。

### Phong材质

> Phong材质（MeshPhongMaterial）是符合Phong光照模型的材质。和Lambert不同的是，Phong模型考虑了镜面反射的效果，因此对于金属、镜面的表现尤为适合。

#### 构造函数

```
new THREE.MeshPhongMaterial({
    color: 0xffff00
});
```

> `specular`值指定镜面反射系数
>
> 可以通过`shininess`属性控制光照模型中的`n`值，当`shininess`值越大时，高光的光斑越小，默认值为`30`

### 法向材质

#### 构造函数

```
new THREE.MeshNormalMaterial()
```

### 纹理贴图

```
var texture = THREE.ImageUtils.loadTexture('../img/0.png');
var material = new THREE.MeshLambertMaterial({
    map: texture
});

```

## 网格

创建网格

```
var material = new THREE.MeshLambertMaterial({
    color: 0xffff00
});
var geometry = new THREE.CubeGeometry(1, 2, 3);
var mesh = new THREE.Mesh(geometry, material);
scene.add(mesh);
```

属性

> 位置、缩放、旋转是物体三个常用属性。由于`THREE.Mesh`基础自`THREE.Object3D`，因此包含`scale`、`rotation`、`position`三个属性。
>
> 它们都是`THREE.Vector3`实例，因此修改其值的方法是相同的。
>
> 缩放对应的属性是`scale`，旋转对应的属性是`rotation`，具体方法与上例相同，分别表示沿x、y、z三轴缩放或旋转。



## 渲染

```javascript
renderer.render(scene, camera);
```

