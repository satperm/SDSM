# Роль меток MPLS

Если мы вернёмся к изначальной схеме с [VRF-Lite](http://habrastorage.org/files/129/d0e/654/129d0e65435149f59d2e1748fb72a43f.gif), то проблема в том, что серый цвет IP-пакета \(индикатор принадлежности к VPN TARS' Robotics\) существует только в пределах узла, при передаче его на другой эта информация переносится в тегах VLAN. И если мы откажемся от сабинтерфейсов на промежуточных узлах, начнётся каша. А сделать это нужно во благо всего хорошего.  
Чтобы этого не произошло в сценарии с MPLS, Ingress LSR на пакет навешивает специальную метку MPLS — **Сервисную** — она идентификатор VPN. Egress LSR \(последний маршрутизатор — R3\) по этой метке понимает, что IP-пакет принадлежит VPN TARS's Robotics и просматривает соответствующий FIB.  
То есть очень похоже на VLAN, с той разницей, что только первый маршрутизатор должен об этом заботиться.

Но на основе сервисной метки пакет не может коммутироваться по MPLS-сети — если мы где-то её поменяем, то Egress LSR не сможет распознать, какому VPN она принадлежит.

И тут на выручку приходит стек меток, который мы так тщательно избегали в прошлом выпуске.  
Сервисная метка оказывается внутренней — первой в стеке, а сверху на неё ещё навешивается транспортная.  
То есть по сети MPLS пакет путешествует с двумя метками — верхней — транспортной и нижней — сервисной\).

Для чего нужно две метки, почему нельзя обойтись одной сервисной? Пусть бы, например, одна метка на Ingress LSR задавал один VPN, другая — другой. Соответственно дальше по пути они бы тоже коммутировались как обычно, и Egress LSR точно знал бы какому VRF нужно передать пакет.  
Вообще говоря, сделать так можно было бы, и это бы работало, но тогда в магистральной сети для каждого VPN был бы отдельный LSP. И если, например, у вас идёт пучок в 20 VPN с R1 на R3, то пришлось бы создавать 20 LSP. А это сложнее поддерживать, это перерасход меток, это лишняя нагрузка на транзитные LSR. Да и, строго говоря, это то, от чего мы тут пытаемся уйти.  
Кроме того, для разных префиксов одного VPN могут быть разные метки — это ещё значительно увеличивает количество LSP.  
Куда ведь проще создать один LSP, который будет туннелировать сразу все 20 VPN?

