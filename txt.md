## vue要做权限管理该怎么做？如果控制到按钮级别的权限怎么做？

- 路由方面，用户登录后只能看到自己有权访问的导航菜单，也只能访问自己有权访问的路由地址，否则将跳转 `4xx` 提示页
- 视图方面，用户只能看到自己有权浏览的内容和有权操作的控件
- 最后再加上请求控制作为最后一道防线，路由可能配置失误，按钮可能忘了加权限，这种时候请求控制可以用来兜底，越权请求将在前端被拦截

#### 如何做

前端权限控制可以分为四个方面：

- ==接口==权限
- ==按钮==权限
- ==菜单==权限
- ==路由==权限

#### 接口权限

采用`jwt`的形式来验证，没有通过的话一般返回`401`，跳转到登录页面重新进行登录

登录完拿到`token`，将`token`存起来，通过`axios`请求拦截器进行拦截，每次请求的时候头部携带`token`

#### 路由权限控制

##### 1.初始化即挂载全部路由，并且在路由上标记相应的权限信息，每次路由跳转前做校验

缺点：

- 加载所有的路由，如果路由很多，而用户并不是所有的路由都有权限访问，对性能会有影响。
- 全局路由守卫里，每次路由跳转都要做权限判断。
- 菜单信息写死在前端，要改个显示文字或权限信息，需要重新编译
- 菜单跟路由耦合在一起，定义路由的时候还有添加菜单显示标题，图标之类的信息，而且路由不一定作为菜单显示，还要多加字段进行标识



##### 初始化的时候先挂载不需要权限控制的路由，比如登录页，404等错误页。如果用户通过URL进行强制访问，则会直接进入404，相当于从源头上做了控制

登录后，获取用户的权限信息，然后筛选有权限访问的路由，在全局路由守卫里进行调用`addRoutes`添加路由

缺点：

- 全局路由守卫里，每次路由跳转都要做判断
- 菜单信息写死在前端，要改个显示文字或权限信息，需要重新编译
- 菜单跟路由耦合在一起，定义路由的时候还有添加菜单显示标题，图标之类的信息，而且路由不一定作为菜单显示，还要多加字段进行标识

#### [#](https://vue3js.cn/interview/vue/permission.html#菜单权限)菜单权限

菜单权限可以理解成将页面与理由进行解耦

菜单与路由分离，菜单由后端返回

#### 按钮权限

按钮权限也可以用`v-if`判断

通过自定义指令进行按钮权限的判断



权限需要前后端结合，前端尽可能的去控制，更多的需要后台判断