- 权限可以看作就是一个url.

- 5张表实现简单权限认证

  - 权限表permission

    | id   | url             | title    |
    | ---- | --------------- | -------- |
    | 1    | /customer_list/ | 客户列表 |
    | 2    | /customer_add/  | 添加客户 |
    | 3    | /customer_edit  | 添加客户 |
    | 4    | /customer_del   | 添加客户 |
    | 5    | /consult_list/  | 添加客户 |

  - 角色表 role

    | id   | name   |
    | ---- | ------ |
    | 1    | BOSS   |
    | 2    | 二老板 |
    | 3    | 测试   |

  - 用户表 user

    | id   | username | pwd  |
    | ---- | -------- | ---- |
    | 1    | alex     | 123  |
    | 2    | peiqq    | 123  |
    | 3    | MJJ      | 123  |

  - 角色和权限的关系表(多对多)

    | id   | role_id | permission_id |
    | ---- | ------- | ------------- |
    | 1    | 1       | 1             |
    | 2    | 1       | 2             |
    | 3    | 1       | 3             |
    | 4    | 1       | 4             |
    | 5    | 1       | 5             |

  - 用户和角色关系表(多对多)

    | id   | user_id | role_id |
    | ---- | ------- | ------- |
    | 1    | 1       | 1       |
    | 2    | 2       | 2       |
    | 3    | 3       | 1       |