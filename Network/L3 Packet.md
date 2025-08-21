В заголовке IP-пакета есть поле, которое определяет протокол следующего уровня.
- В **IPv4** это поле называется **Protocol** (8 бит).
- В **IPv6** это поле называется **Next Header** (8 бит).

Заголовок содержит простое число, указывающее на протокол:
- `1` — [[ICMP & IGMP#^ICMP|ICMP]]
- `2` — [[ICMP & IGMP#^IGMP|IGMP]]
- `4` — [[IPv4]] (*IP-in-IP*, используется для [[Tunneling|туннелирования]] частных сетей через публичные, сейчас фактически вытеснен [[GRE]], [[L2TP]] + IPsec)
- `6` — [[TCP]]
- `17` — [[TCP & UDP#^UDP|UDP]]
- `41` — [[IPv6]] (*IPv6-in-IPv4*, [[Tunneling|туннелирование]] IPv6 внутри IPv4)
- `47` — [[GRE]]
- `115` — [[L2TP]]

Полный список ведёт [IANA: Assigned Internet Protocol Numbers](https://www.iana.org/assignments/protocol-numbers/protocol-numbers.xhtml).
