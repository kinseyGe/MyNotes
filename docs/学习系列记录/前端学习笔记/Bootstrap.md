- 如何使用
  1. 导入jquery.js文件
  2. 导入bootstrap.css文件
  3. 导入bootstrap.js文件
  4. 创建视口(非必须)
    ```
      //在移动设备上才是必须要创建的
      <meta name="viewport" content="width=device-width, initial-scale=1">
    ```
  5. 创建布局容器
    ```
    //第一种
      <div class="container">
      ...
      </div>
    //第二种
      <div class="container-fluid">
      ...
      </div>
    ```
---
- 栅格系统
  - **把每一行分成12份，超过12行自动换行**
    - 大屏幕 col-lg-n
    - 中等屏幕 col-md-n
    - 小屏幕 col-sm-6
    - 超小屏幕 col-xs-12
---
