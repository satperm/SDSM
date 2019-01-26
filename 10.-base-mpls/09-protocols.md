## Протоколы распространения меток

Их не так много — три: LDP, RSVP-TE, MBGP.  
Есть две глобальные цели — распространение траспортных меток и распространение меток сервисных.  
Поясним: _**транспортные метки**_ используются **для передачи трафика** по сети MPLS. Это как раз те, о которых мы говорим весь выпуск. Для них используются LDP и RSVP-TE.

_**Сервисные метки**_ служат **для разделения** различных сервисов. Тут на арену выходят MBGP и отросток LDP — [tLDP](http://lookmeup.linkmeup.ru/#term511).  
В частности MBGP позволяет, например, пометить, что вот такой-то маршрут принадлежит такому-то VPN. Потом он этот маршрут передаёт, как vpn-ipv4 family своему соседу с меткой, чтобы тот смог потом отделить мух от котлет.  
Так вот, чтобы он мог отделить, ему и нужно сообщить о соответствии метка-FEC.  
Но это действие другой пьесы, которую мы сыграем ещё через полгода-год.

Обязательным условием работы всех протоколов динамического распределения меток является базовая настройка IP-связности. То есть на сети должны быть запущены IGP.

Ну, вот теперь, когда я вас окончательно запутал, можно начинать распутывать.  
Итак, как проще всего распределить метки? Ответ — включить LDP.