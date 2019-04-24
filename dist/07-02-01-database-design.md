# Dataset of Tinny Hippo


## 1 ER模型

![ER模型](https://github.com/rookies-sysu/Dashboard/blob/master/imgs/ER_model.png?raw=true)



## 2 关系模型

- 顾客 : 订单 = 1 : N

  顾客 ( <u>**顾客ID**</u>, 顾客NAME )

  订单 ( <u>**订单ID**</u>, 总价, 选菜信息, 订单状态, 备注 )

  下单 ( <u>**顾客ID**</u>,  订单ID, 时间, 桌号)

  - **主键**: 顾客ID
  - **外键**: 订单ID

- 顾客 : 菜单 = N : M

  顾客 ( <u>**顾客ID**</u>, 顾客NAME )

  菜单 ( <u>**菜ID**</u>, 菜名, 类别, 价格, 描述, 图片 )

  选择 (<u>**顾客ID**</u>, <u>**菜ID**</u>, 数量)

  - **复合主键**: 顾客ID, 菜ID
  - **外键**: 顾客ID, 菜ID

- 商家 : 订单 = 1 : N

  商家 ( <u>**商家ID**</u>, 商家NAME, 商家PASSWORD )

  订单 ( <u>**订单ID**</u>, 总价, 选菜信息, 订单状态, 备注 )

  管理2 ( <u>**商家ID**</u>,  订单ID)

  - **主键**: 商家ID
  - **外键**: 订单ID

- 商家 : 菜单 = 1 : N

  商家 ( <u>**商家ID**</u>, 商家NAME, 商家PASSWORD )

  菜单 ( <u>**菜ID**</u>, 菜名, 类别, 价格, 描述, 图片 )

  管理1 ( <u>**商家ID**</u>,  菜ID)

  - **主键**: 商家ID
  - **外键**: 菜ID

  ​

## 3 数据库物理模型

- **Customer**

  | Field         | Type        | Key  | Description          |
  | ------------- | ----------- | ---- | -------------------- |
  | customer_id   | int         | PRI  | The ID of customer   |
  | customer_name | varchar(20) |      | The name of customer |

- **Seller**

  | Field       | Type        | Key  | Description            |
  | ----------- | ----------- | ---- | ---------------------- |
  | seller_id   | int         | PRI  | The ID of seller       |
  | seller_name | varchar(20) |      | The name of seller     |
  | seller_pwd  | varchar(20) |      | The password of seller |

- **Order**

  | Field       | Type         | Key  | Description              |
  | ----------- | ------------ | ---- | ------------------------ |
  | order_id    | int          | PRI  | The ID of order          |
  | total       | float        |      | The total price of order |
  | order_info  | varchar(500) |      | The information of order |
  | order_state | varchar(10)  |      | The state of order       |
  | remark      | varchar(500) |      | The remark of order      |


- **Menu**

  | Field      | Type         | Key  | Description               |
  | ---------- | ------------ | ---- | ------------------------- |
  | food_id    | int          | PRI  | The ID of food            |
  | food_name  | varchar(20)  |      | The name of food          |
  | food_type  | varchar(20)  |      | The type of food          |
  | food_price | float        |      | The price of food         |
  | food_desc  | varchar(100) |      | The description of food   |
  | food_img   | varchar(100) |      | The address of food image |


- **Ordering**

  | Field         | Type | Key  | Description          |
  | ------------- | ---- | ---- | -------------------- |
  | customer_id   | int  | PRI  | The ID of customer   |
  | order_id      | int  |      | The ID of order      |
  | ordering_time | date |      | The time of ordering |
  | table_id      | int  |      | The ID of table      |


- **Select**

  | Field       | Type | Key  | Description                 |
  | ----------- | ---- | ---- | --------------------------- |
  | customer_id | int  | PRI  | The ID of customer          |
  | food_id     | int  | PRI  | The ID of food              |
  | food_num    | int  |      | The number of selected food |



- **Manager1**

  | Field     | Type | Key  | Description      |
  | --------- | ---- | ---- | ---------------- |
  | seller_id | int  | PRI  | The ID of seller |
  | food_id   | int  |      | The ID of food   |


- **Manager2**

  | Field     | Type | Key  | Description      |
  | --------- | ---- | ---- | ---------------- |
  | seller_id | int  | PRI  | The ID of seller |
  | order_id  | int  |      | The ID of order  |

