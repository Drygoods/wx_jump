# 一个规范且适合新手阅读的wx跳一跳辅助
> 原“100行以内实现自动玩微信跳一跳”

## FIRST
- 基于Python3/PIL/numpy编写，源码逻辑友好
- 因为修bug和优化算法，master分支上已经200行了，就改了名
- 建议以master分支上的版本为准，100行版本比较投机且不够规范
- 100行版本请见分支‘100’（不推荐）

## WHAT'S NEW ##
- 目前主流算法是使用opencv做图形识别进行定位
- 该项目**不使用**图像识别，也**不使用**opencv
- 直接通过矩阵计算来解决这个问题

## RELEASE NOTE
- 2018/1/3
    - 腾讯爸爸开始打击辅助了
    - 加入了防 反辅助工具 机制

- 2018/1/2
    - 看了一下github上的同类项目，top1那个已经开发得很成熟啦
    - 不过还是希望这个项目能给各位带来一点新的想法，共同学习~

## 思路

- 用adb获取手机截图并拉取到项目中
- 对图片进行二值化
- 根据棋子的RGB值获取当前位置
- 按行遍历这张图片，从300行后开始（避免干扰），如果检测到两行数据有差异，说明该位置为菱形顶端
- 获得菱形顶端之后可以计算出目标点的横坐标与菱形顶端的纵坐标
横坐标有两种判定方案：
    - 左右扫描：
        - 目标点在左边，则从画面左侧开始逐列扫描，第一个切点即阴影的左端
        - 目标点在右边，则从画面右侧开始逐列扫描，第一个切点即菱形的右端
        - 此时根据上述条件可以计算出目标点的坐标
    - 往下扫描（目前用这个）：
        - 继续从上往下扫描，以右侧为准（没有影子），如果线不再延伸说明图形结束，此时为目标y坐标。
- 两点间距求出距离，换算成adb点按时长，让手机执行

## HOW

- 目前只对android进行了适配，源码中的数据均为1080x1920下，可能需要微调参数
- 连接手机后开启USB调试模式，电脑需要安装adb
- 在命令行中输入adb devices查看设备状态以判断设备是否连接上且为device状态（必须）
- 打开微信-跳一跳小游戏，点击开始游戏并跳过教学阶段
- 运行wx_jump_py3.py即可

效果如图

 ![image](https://github.com/williamfzc/wx_jump/raw/master/demo.jpg)

## TODO

- 新定位算法对特殊图形仍有计算误差
- 整型计算感觉会有不小误差

## BUG

- 在目标点与起始点很近的情况下的判定可能有较明显误差
- 一些特殊图形有误差，无法combo