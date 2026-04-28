# 鸿蒙学习笔记 （一）：一个简单的横屏名片展示
> **日期：** 2026-04-26
## 做了什么
 ### 用ArkTs做了一个简单的横屏页面展示
## 效果截图
![名片效果](../images/screenshot1.png)
## 核心代码
```ts

@Entry
@Component
struct  PersonTitle{
  build() {
    Stack({alignContent:Alignment.TopStart}) {
 //1.图片
       Image($r('app.media.photo4'))
         .width(150)
         .height(100)
         .offset({x:60,y:20})
 //2.红色方框
      Row() {}
      .height(30)
      .width(50)
      .backgroundColor(Color.Red)
      .offset({x:10,y:150})
 //3.文字
      Column({space:5}) {
          Text('表小可')
            .fontSize(30)
            .fontWeight(700)
          Text('客户经理')
            .fontSize(20)
    }
      .offset({x:80,y:150})
//4.基本信息
      Column(){
      Text('北京易创意科技有限公司')
        .fontSize(30)
        .fontWeight(700)
        .fontColor(Color.Red)
        Text('北京市东城区王府井大街74号100006\n' +
          '手机:186 8888 8888\n' +
          '电话:010-1234 4321\n' +
          '传真:010-1101 2202\n' +
          '邮箱:name@company.com\n' +
          '网址:www.brand.com')
          .fontSize(20)
          .lineHeight(25)
      }
      .offset({x:270,y:100})
 //5.二维码
      Image($r('app.media.qrcode'))
        .width(150)
        .height(150)
        .offset({x:550,y:170})
  }
  }

}
```
### 遇到的问题
- 相关命名
文件名：全小写 + 下划线
组件名：大驼峰（每个单词首字母大写）
- 图片展示 需保存在entry/src/main/resources/base/media  
资源文件命名不支持中文、特殊字符，只允许：英文字母、数字、下划线。
- Text()组件不能为空
- justifyContent 属性影响上下子组件        justifyContent 是 Row/Column 容器的属性，会影响容器内所有子组件的对齐方式
- 使用margin导致布局动来动去  
采用offest加stack代替magin做偏移 offset 是纯视觉偏移，不参与布局计算，不会影响其他组件的位置   
 margin	✅ 会影响，会改变父容器和兄弟组件的位置 	做组件之间的间距、外边距  
 offset	❌ 不影响，仅做视觉偏移	做重叠、偏移、悬浮效果
布局计算(margin)适合流态的app,而做固态的名片适合offset
- offest坐标轴规则  
元素直接放在 Stack 里	Stack 容器的左上角	整个画布的左上角是 (0,0)  
元素放在 Row/Column 里	这个 Row/Column 容器的左上角	父容器的左上角是 (0,0)  
可设置borderWidth(数字)边框线可视化帮助确定位置  
 注意：：Stack + offset 来定位元素  
 Stack 的 alignContent: Alignment.Center 导致元素默认居中
Stack 默认会把所有子组件居中摆放，再给每个组件加 offset，相当于 “先居中，再整体偏移”，就会和设计稿的位置错位  
设置alignContent: Alignment.TopStart比较好
- Stack 组件默认是 wrap_content（包裹内容）的大小，它的宽高是由里面最宽、最高的子组件决定的，不是全屏
### 学到了什么
- 换行操作  
 1. Column() {内容}  
 2. Blank() 自动撑开间距   
  3. lineHeight(数字)  
  注意：lineHeight 的数字 必须 ≥ 你的 fontSize  
lineHeight = 行高
fontSize = 文字本身高度  
如果 lineHeight 小于 fontSize → 文字会被截断、重叠、显示异常
必须 lineHeight ≥ fontSize
- Column({ space: 5 })space对column内的所有组件都起作用
- 使用stack{}+offest布局，margin和offset的适用场景
- column(),row(),offset(),Text(),Image()相关组件和属性的使用
### 下一步
- 内容动态 ：把写死的数据改成@state   
- 背景优化 : 加圆角和背景色  
- 响应化格局 ：适应竖屏，横屏竖屏自动转换  
- 图片适应 ：让图片在不同屏幕上显示
