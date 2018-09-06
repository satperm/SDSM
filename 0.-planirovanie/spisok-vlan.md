# 4. Список VLAN

| № VLAN | VLAN name | Примечание |
| :--- | :--- | :--- |
| 1 | default | Не используется |
| 2 | Management | Для управления устройствами |
| 3 | Servers | Для серверной фермы |
| 4-100 |  | Зарезервировано |
| 101 | PTO | Для пользователей ПТО |
| 102 | FEO | Для пользователей ФЭО |
| 103 | Accounting | Для пользователей Бухгалтерии |
| 104 | Other | Для других пользователей |

Каждая группа будет выделена в отдельный влан. Таким образом мы ограничим широковещательные домены. Также введём специальный VLAN для управления устройствами.

Номера VLAN c 4 по 100 зарезервированы для будущих нужд.
