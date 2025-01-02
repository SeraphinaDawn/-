# Demo_Odoo_Tutorial:演示odoo教程

## model部分：

### AbstractModel

```
AbstractModel：抽象话model，并不会在
```

### Model

```
Model：是平时使用的model
```

### TransientModel

```
TransientModel：是暂时使用的model，比如平时的一些报表之类的东西可以暂存这个model里
```

### _name字段

```
`_name` 为 model 的名称， model 名称建议都使用单数， 然后不要使用 `_` 分隔名称

规范是用点例如：demo.odoo.tutorial
```



### _inherit

```
在odoo中继承是一个很重要的部分

_inherit 在 odoo 中不管是 model 还是 view，甚至是权限,都会使用继承 
```

### track_visibility参数选择：

**`track_visibility='always'`**：始终跟踪该字段的变化。

**`track_visibility='onchange'`**：仅在字段的值发生变化时跟踪。

**`track_visibility='never'`**：不跟踪该字段的变化。

### 勾选框：

> **在Odoo框架会自动将Boolean字段渲染为勾选框**
>
> **is_done_track_onchange = fields.Boolean(string="是否完成？",default=False,track_visibility='onchange')**

![image-20240715174553275](https://cdn.jsdelivr.net/gh/ThaiiOUJ/PicGo@main/image-20240715174553275.png)

## 安全文件security：

> **security目录下的文件能设置用户的读写权限**
>
> - **分别文件有：**
>   1. **ir.model.access.csv**
>   2. **security.xml**

### security

- ​	**xml**

  > ```xml
  >   <record id="module_demo_odoo_tutorial" model="ir.module.category">
  >     <field name="name">演示odoo教程类别</field>
  >   </record>
  > 
  > <!-- 用户-->
  >   <record id="demo_odoo_tutorial_group_user" model="res.groups">
  >   <!--demo_odoo_tutorial_group_user — 这是“User”用户组的唯一标识符-->
  >     <field name="name">User</field>
  >     <field name="category_id"  
  >            ref="module_demo_odoo_tutorial"/>
  > <!--category_id关联到上面定义的模块类别-->
  > <!-- ref="module_demo_odoo_tutorial"继承上面的id  -->
  >     <field name="implied_ids"
  >            eval="[(4, ref('base.group_user'))]"/>
  >       <!--'base.group_user'普通用户组权限-->
  >   </record>
  > 
  > <!-- 管理员-->
  >   <record id="demo_odoo_tutorial_group_manager" model="res.groups">
  >     <field name="name">Manager</field>
  >     <field name="category_id"
  >            ref="module_demo_odoo_tutorial"/>
  > <!--category_id 这里同样关联到第一行的模块类别，这意味着两个用户组user和manager都属于模块类别下，可以帮助组织和分类不同的用户组-->
  >     <field name="implied_ids"
  >            eval="[(4, ref('demo_odoo_tutorial_group_user'))]"/>
  > <!--表示当前用户组（Manager）继承了 demo_odoo_tutorial_group_user 用户组的所有权限。-->
  >     <field name="users"
  >            eval="[(4, ref('base.user_root')),
  >                   (4, ref('base.user_admin'))]"/>
  > <!--表示将 base.user_root（超级管理员）和 base.user_admin（管理员）这两个用户添加到该用户组（Manager）中。-->
  >   </record>
  > ```

  

  ### **ir.model.access.csv**

  - **csv**

    | id                       | name                           | model_id:py文件里的model把点换成下划线 | group_id                         | perm_read | perm_write | perm_create | perm_unlink |
    | ------------------------ | ------------------------------ | -------------------------------------- | -------------------------------- | --------- | ---------- | ----------- | ----------- |
    | access_demo_odoo_user    | Demo Odoo Tutorial User Access | model_demo_odoo_tutorial               | demo_odoo_tutorial_group_user    | 1         | 0          | 0           | 0           |
    | access_demo_odoo_manager | DemoOdooTutorialManager Access | model_demo_odoo_tutorial               | demo_odoo_tutorial_group_manager | 1         | 1          | 1           | 1           |





## 线性继承

> - ​	**线性继承基本都为下拉选项栏的**

![image-20240715164915959](https://cdn.jsdelivr.net/gh/ThaiiOUJ/PicGo@main/image-20240715164915959.png)



### view：视图

> - **from的意思是随便点一个进去后的表单**
>
>   ![image-20240715165437761](https://cdn.jsdelivr.net/gh/ThaiiOUJ/PicGo@main/image-20240715165437761.png)
>
> - **tree的意思是列表**
>
>   ![image-20240715165646404](https://cdn.jsdelivr.net/gh/ThaiiOUJ/PicGo@main/image-20240715165646404.png)
>
> - **action的意思是动作：没有这个在创建的模块odoo中是动不了的**
>
>   ![image-20240715165850008](https://cdn.jsdelivr.net/gh/ThaiiOUJ/PicGo@main/image-20240715165850008.png)
>
> - **menuitem的意思是菜单，**
>
>   > **其中的sequence=“2”是在菜单列别里拍第二位数字越高排的越上**
>
>   ![image-20240715170009684](https://cdn.jsdelivr.net/gh/ThaiiOUJ/PicGo@main/image-20240715170009684.png)

#### 返回按钮

> **在model下写一个名为action_return函数**
>
> **标准的返回方法ir.actions.act_window**

```py
name="%(action_demo_odoo_tutorial)d"：name 属性使用了 Odoo 的动作引用语法。它指向一个名为 action_demo_odoo_tutorial 的动作（通常是在 ir.actions.act_window 中定义的），这个动作可能是打开一个新的视图或页面。
```



#### 保存按钮

> **在model里写一个action_save方法**

> ```py
> def action_save(self):
>     # 这个方法将在点击“保存”按钮时被调用
>     self.ensure_one()  # 确保只处理一个记录
>     # 在保存时添加你想执行的任何额外逻辑
>     return {'type': 'ir.actions.act_window_close'}
> ```

## demo和data文件夹区别

**demo在建立一个新的数据库时勾选的demo data才是真的**

> **产生 demo 资料时, 你安装 addons 会自动产生 demo data** 

![image-20240715172332636](https://cdn.jsdelivr.net/gh/ThaiiOUJ/PicGo@main/image-20240715172332636.png)

**下面为新建的一个data文件目录**

> ### 1. 记录解释
>
> ```xml
> <record id="demo_odoo_1" model="demo.odoo.tutorial">
>     <field name="name">demo_odoo_1</field>
>     <field name="name_track_always">demo_name_track_always_1</field>
>     <field name="is_done_track_onchange">True</field>
> </record>
> 
> <record id="demo_odoo_2" model="demo.odoo.tutorial">
>     <field name="name">demo_odoo_2</field>
>     <field name="name_track_always">demo_name_track_always_2</field>
>     <field name="is_done_track_onchange">True</field>
> </record>
> ```
>
> ### 2. 字段解释
>
> - **id**: 每个记录的唯一标识符。
> - **model**: 指定记录所对应的模型，这里是 `demo.odoo.tutorial`。
> - **field name="name"**: 设置记录的名称。
> - **field name="name_track_always"**: 可能用于跟踪名称的变化。
> - **field name="is_done_track_onchange"**: 设置为 `True`，可能表示该记录在某些状态变化时会被跟踪或更新。
>
> ### 3. 安装模块后的效果
>
> - 当安装这个模块时，Odoo 会在 
>
>   ```
>   demo.odoo.tutorial
>   ```
>
>    模型中创建两条新记录：
>
>   - `demo_odoo_1`
>   - `demo_odoo_2`
>
> ### 4. 记录的生成
>
> - 安装模块后，这两条记录会被自动创建，成为数据库中的一部分。因此，用户（如 `root` 用户）在模块安装完成后会看到这两条信息。

![image-20240715171956298](https://cdn.jsdelivr.net/gh/ThaiiOUJ/PicGo@main/image-20240715171956298.png)

# 源码

### 演示odoo教程py文件              

```python
from odoo import models, fields, api
# 处理异常模块
from odoo.exceptions import ValidationError


class DemoOdooTutorial(models.Model):
    _name = 'demo.odoo.tutorial'
    _description = 'Dem Odoo Tutorial'
    # 下面是继承
    _inherit=['mail.thread','mail.activity.mixin']

    name = fields.Char(string='描述' )
    # track_visibility='onchange' 由true或false，改变时会进行跟踪，使用这个必须上面要有继承'mail.thread','mail.activity.mixin'不然会报错
    is_done_track_onchange = fields.Boolean(string="是否完成？",default=False,track_visibility='onchange')
    # 下面的是始终显示跟踪的意思
    name_track_always = fields.Char(string="曲目名称", track_visibility='always')
    # 下面开始时间结束时间.now:表示默认为当前时间
    start_datetime = fields.Datetime('开始日期', default=fields.Datetime.now())
    stop_datetime = fields.Datetime('结束日期', default=fields.Datetime.now())
    
    
    field_onchange_demo = fields.Char(string='允许用户改变示例')
    # 下面不可编辑，通过某个部分的api装饰器进行计算得出的
    field_onchange_demo_set = fields.Char('onchange_demo_set', readonly=True)
    
    input_number = fields.Float(string='输入数字', digits=(10, 3))
    # compute="_get_field_compute" 表示由下面链接上来的动态计算不能更改
    field_compute_demo = fields.Integer(compute="_get_field_compute", readonly=True)
    
    # 下面的是odoo用于定义数据库级别的约束
    # 约束名称：
    # 'name_uniq'：这是约束的名称，必须在数据库中唯一。通常采用 <字段名>_<约束类型> 的命名方式。
    # 约束类型：
    # 'unique(name)'：这表示 name 字段必须是唯一的。如果尝试插入或更新记录，使得 name 字段的值重复，数据库将会抛出错误。  
    # 错误消息：
    # 'Description must be unique'：当唯一约束被违反时，用户将看到的错误消息。
    _sql_constraints = [
        ('name_uniq', 'unique(name)', '描述必须是唯一的'),
    ]


    # 当api装饰器里的start_datetime或者stop_datetime，改变时，执行_check_date计算方法：当开始时间大于结束时间，就抛出异常：停止时间必须大于开始时间
    @api.constrains('start_datetime', 'stop_datetime')
    def _check_date(self):
        for data in self:
            if data.start_datetime > data.stop_datetime:
                raise ValidationError("停止时间必须大于开始时间")
            
    # 用于上面字段input_number输入的数字使输入的数字以除于1000的形式输出 
    @api.depends('input_number')
    def _get_field_compute(self):
        for data in self:
            data.field_compute_demo = data.input_number * 1000
```

### 演示odoo教程xml文件

```xml
<?xml version="1.0" encoding="utf-8"?>
<odoo>
    <data>
        <!-- 表单视图 -->
        <record id="view_demo_odoo_tutorial_form" model="ir.ui.view">
            <field name="name">演示odoo教程表单</field>
            <field name="model">demo.odoo.tutorial</field>
            <field name="arch" type="xml">
                <form string="Demo Odoo Tutorial">
                    <sheet>
                        <group>
                            <field name="name"/>
                            <field name="is_done_track_onchange"/>
                            <field name="name_track_always"/>
                            <field name="start_datetime"/>
                            <field name="stop_datetime"/>
                            <field name="field_onchange_demo"  />
                            <field name="field_onchange_demo_set" string = '字段更改演示集'/>
                            <field name="input_number"/>
                            <field name="field_compute_demo" string = '字段计算示例'/>
                        </group>
                    </sheet>
                    <div class="oe_chatter">
                        <field name="message_follower_ids" widget="mail_followers"/>
                        <field name="activity_ids" widget="mail_activity"/>
                        <field name="message_ids" widget="mail_thread"/>
                    </div>
                </form>
            </field>
        </record>

        <!-- 列表视图 -->
        <record id="view_demo_odoo_tutorial_tree" model="ir.ui.view">
            <field name="name">演示odoo教程列表</field>
            <field name="model">demo.odoo.tutorial</field>
            <field name="arch" type="xml">
                <tree string="Demo Odoo Tutorial">
                    <field name="name"/>
                    <field name="is_done_track_onchange"/>
                    <field name="name_track_always"/>
                    <field name="start_datetime"/>
                    <field name="stop_datetime"/>
                    <field name="input_number"/>
                    <field name="field_compute_demo"/>
                </tree>
            </field>
        </record>

        <!-- 动作 -->
        <record id="action_demo_odoo_tutorial" model="ir.actions.act_window">
            <field name="name">演示odoo教程动作</field>
            <field name="type">ir.actions.act_window</field>
            <field name="res_model">demo.odoo.tutorial</field>
            <field name="view_mode">tree,form</field>
        </record>

        <!-- 菜单 -->
        <menuitem id="menu_demo_odoo_tutorial"
                  name="Demo Odoo Tutorial"
                  action="action_demo_odoo_tutorial"
                  sequence="2"/>
    </data>
</odoo>

```



### xml继承代码分析

```xml
<div class="oe_chatter">
    <field name="message_follower_ids" widget="mail_followers"/>
    <field name="activity_ids" widget="mail_activity"/>
    <field name="message_ids" widget="mail_thread"/>
</div>
<div class="oe_chatter">：

这是一个包含聊天功能的容器。oe_chatter类用于标识这个部分，使其在界面上呈现为“聊天”区。
<field name="message_follower_ids" widget="mail_followers"/>：

该字段显示与当前记录相关的关注者。mail_followers小部件用于展示哪些用户正在关注此记录，允许他们接收相关消息。
<field name="activity_ids" widget="mail_activity"/>：

该字段用于显示与记录相关的活动（如待办事项）。mail_activity小部件提供一个接口，用户可以查看和管理这些活动。
<field name="message_ids" widget="mail_thread"/>：

这个字段显示所有与该记录相关的消息和讨论。mail_thread小部件允许用户查看历史消息并添加新消息。
```



## odoo带的class按钮颜色：

> **`btn`**: 基础按钮样式。
>
> **`btn-primary`**: 主要按钮样式，通常用于强调。
>
> **`btn-secondary`**: 次要按钮样式，通常用于辅助操作。
>
> **`btn-success`**: 成功状态的按钮样式（通常为绿色）。
>
> **`btn-danger`**: 警告或删除按钮样式（通常为红色）。
>
> **`btn-warning`**: 警告状态的按钮样式（通常为黄色）。
>
> **`btn-info`**: 信息状态的按钮样式（通常为蓝色）。
>
> **`btn-light`**: 浅色按钮样式。
>
> **`btn-dark`**: 深色按钮样式。
>
> **`oe_highlight`**: 突出显示按钮样式，通常用于强调操作。
