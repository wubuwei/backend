### 1. php.ini 与 FPM 配置信息有哪些
php.ini 文件
### 2. Nginx与 FPM 通信的数据格式
nginx

### 3. Go 和 PHP 在运行的时候有什么区别和优势？
https://learnku.com/articles/44432

IOC（依赖注入）和 AOP（面向切面）机制，

### 4. MVC

MVC模式（Model–view–controller）是软件工程中的一种软件架构模式，把软件系统分为三个基本部分：模型（Model）、视图（View）和控制器（Controller）。

- 模型（Model） - 程序员编写程序应有的功能（实现算法等等）、数据库专家进行数据管理和数据库设计(可以实现具体的功能)。
- 视图（View） - 界面设计人员进行图形界面设计。
- 控制器（Controller）- 负责转发请求，对请求进行处理。

Model 负责数据访问
Controller 负责处理消息
View 负责显示数据

Model（模型）是应用程序中用于处理应用程序数据逻辑的部分。
通常模型对象负责在数据库中存取数据。

View（视图）是应用程序中处理数据显示的部分。
通常视图是依据模型数据创建的。

Controller（控制器）是应用程序中处理用户交互的部分。
通常控制器负责从视图读取数据，控制用户输入，并向模型发送数据。