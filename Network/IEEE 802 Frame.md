Перед каждым фреймом идет **Preamble + SFD** (7 + 1 байт), чтобы принимающая сторона успела «поймать ритм» и понять, где начало пакета.
- **Preamble (преамбула)** — 7 байт (56 бит), чередующиеся нули и единицы (`10101010...`) - нужна для синхронизации тактовой частоты приёмника с передатчиком. Физический уровень работает на электрических импульсах, и чтобы приёмник «подстроил генератор», ему нужно несколько бит ритма.
- **SFD (Start Frame Delimiter)** — 1 байт: `10101011`.  Последние два бита `11` говорят: «всё, преамбула закончилась, дальше начинается сам фрэйм.
Всё это нужно потому, что канал может простаивать, могут быть помехи и коллизии.

### Frame structure

- **HEADER**: 14 bytes
	- Destination MAC-address: 6 bytes
	- Source MAC-address: 6 bytes
	- Ether type: 2 bytes; defines a protocol of the network layer:
		- `0x0800` — IPv4
		- `0x86DD` — IPv6
		- `0x0806` — ARP
		- `0x8847` — MPLS
- **PAYLOAD**: 46 - 1500 bytes; if less than 46, filled with padding to get 64 byte frame.
- **CRC CHECKSUM**: 4 bytes

Headers + checksum = 18 bytes.
MTU (Maximum Transmission Unit) = 1500 bytes.
Total: 1518 bytes.

[frame structure](https://ru.wikipedia.org/wiki/%D0%9A%D0%B0%D0%B4%D1%80_(%D1%82%D0%B5%D0%BB%D0%B5%D0%BA%D0%BE%D0%BC%D0%BC%D1%83%D0%BD%D0%B8%D0%BA%D0%B0%D1%86%D0%B8%D0%B8))
