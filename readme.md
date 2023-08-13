最终效果：

![](https://upload-images.jianshu.io/upload_images/130752-efbf84e4509121cb.gif?imageMogr2/auto-orient/strip)


源码：
```
import SwiftUI

struct SpinnerView: View {
    
  // 定义 Leaf 结构体
  struct Leaf: View {
    let rotation: Angle
    let isCurrent: Bool

    var body: some View {
        /*
         逐行解释以下代码：

         1. `Capsule()`：创建一个椭圆形状的视图。

         2. `.stroke(isCurrent ? Color.white : .gray, lineWidth: 8)`：给椭圆形状视图添加一个描边，同时根据`isCurrent`的值来决定描边的颜色是白色还是灰色，描边的线宽为8。

         3. `.frame(width: 20, height: 50)`：设置视图的大小，将其宽度设置为20，高度设置为50。

         4. `.offset(isCurrent ? .init(width: 10, height: 0) : .init(width: 40, height: 70))`：根据`isCurrent`的值来设置视图的偏移量。如果`isCurrent`为`true`，则将视图在X轴上向右偏移10，Y轴上不偏移；如果`isCurrent`为`false`，则将视图在X轴上向右偏移40，Y轴上向下偏移70。

         5. `.scaleEffect(isCurrent ? 0.5 : 1)`：根据`isCurrent`的值来设置视图的缩放比例。如果`isCurrent`为`true`，则将视图缩放为原来的一半；如果`isCurrent`为`false`，则保持原始大小。

         6. `.rotationEffect(rotation)`：根据给定的角度`rotation`对视图进行旋转。这里可以根据需要传入一个`Angle`类型的值来控制旋转角度。

         7. `.animation(.easeInOut(duration: 1.5), value: isCurrent)`：为视图添加动画效果。`.animation`修饰符用于指定动画效果的类型和持续时间。在这里，使用了`easeInOut`类型的动画，持续时间为1.5秒。通过`value`参数，动画将在`isCurrent`的值发生变化时触发。
         */
      Capsule()
        .stroke(isCurrent ? Color.white : .gray, lineWidth: 8)
        .frame(width: 20, height: 50)
        .offset(
          isCurrent
            ? .init(width: 10, height: 0)
            : .init(width: 40, height: 70)
        )
        .scaleEffect(isCurrent ? 0.5 : 1)
        .rotationEffect(rotation)
      // MARK: - Update deprecated `animation` modifier
      //        .animation(.easeIn(duration: 1.5))
      // Animate when `isCurrent` or `isCompleting` change
        .animation(.easeInOut(duration: 1.5), value: isCurrent)
    }
  }

  // 叶子的数量
  let leavesCount = 12
  //初始值-1的@State属性currentIndex，用于跟踪当前显示的叶子索引
  @State var currentIndex = -1
  
  var body: some View {
    VStack {
      ZStack {
        ForEach(0..<leavesCount) { index in
          Leaf(
            rotation: .init(degrees: .init(index) / .init(leavesCount) * 360),
            isCurrent: index == currentIndex
          )
        }
      }
      .onAppear(perform: animate)
    }
  }
  
  func animate() {
    Timer.scheduledTimer(withTimeInterval: 0.15, repeats: true) { timer in
      currentIndex = (currentIndex + 1) % leavesCount
    }
  }
}

struct SpinnerView_Previews : PreviewProvider {
  static var previews: some View {
    SpinnerView()
  }
}

```