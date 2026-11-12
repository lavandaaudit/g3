# SEGA NETPLAY — ONLINE

Однострілкова сторінка для **GitHub Pages**: гра в класичні ігри Sega Mega Drive прямо у браузері з підтримкою онлайн-нетплею через WebRTC.

**Без встановлення. Без реєстрації.** ROMs у репозиторії — ігри стартують автоматично. Працює на **десктопі та мобільному**.

## Ігри (10 шт)

| Гра | Жанр | Рік | Розмір |
|-----|------|-----|--------|
| ⚽ FIFA 97 Gold | Sports | 1996 | 2 МБ |
| ⚽ FIFA 98 | Sports | 1997 | 2 МБ |
| 🏒 NHL '94 | Sports | 1993 | заглушка |
| 🏀 NBA Jam | Sports | 1993 | 2 МБ |
| 🏀 Bulls vs Blazers | Sports | 1992 | 1 МБ |
| 🕶️ Syndicate | Strategy | 1993 | заглушка |
| 🌍 Populous | Strategy | 1989 | заглушка |
| 🎖️ Commandos | Strategy | 1998 | заглушка |
| ⚓ Pirates! Gold | Adventure | 1993 | заглушка |
| 🌆 Shadowrun | RPG | 1994 | заглушка |

> **Заміни заглушки:** `nhl94.bin`, `syndicate.bin`, `populous.bin`, `commandos.bin`, `pirates_gold.bin`, `shadowrun.bin` — зараз це файли-заглушки без ігрових даних. Закинь свої ROM-дампи з **тим самими іменами** — і всі 10 ігор запрацюють.

**Як додати ще гру:**
1. Поклади ROM `.bin` файл у репозиторій
2. Додай запис у масив `GAMES` в `index.html`:
```js
{ id: 'sonic', name: 'Sonic', rom: './sonic.bin', emoji: '🦔', genre: 'Platformer', desc: '1991 · Sega' }
```
3. Гру автоматично з'явиться у виборі

**Або завантаж свій ROM:** кнопка «📁 Завантажити свій ROM» у виборі ігор — підтримує `.bin/.smd/.gen/.md/.zip`.

## Як вибрати гру

- Натисни **«📁 Ігри»** кнопку в header → модалка вибору
- Або відкрий `?game=<id>` (наприклад `?game=nhl94` для NHL '94)
- URL-параметр визначає яка гра завантажиться

## Десктоп

```
+------------------------------------+----------------+
| 🎮 SEGA NETPLAY    [📁 Ігри]  ☰   | РЕЖИМ ГРИ      |
+------------------------------------+ [Соло][Онлайн] |
|                                    | ОНЛАЙН КАБІНЕТ |
|         GAME CANVAS                | Нік: [______]  |
|         (великий, 4:3)            | [Створити]     |
|                                    | Код: [______]  |
|         WASD + J/K/L + Space       | [Приєднатись]  |
|                                    +----------------+
|                                    | ЧАТ            |
+------------------------------------+----------------+
```

## Мобільний

**Портрет:** гра розтягнута на весь екран, контроли overlay знизу.
**Ландшафт (рекомендований):** гра 4:3 в центрі, круглий D-pad зліва, A/B/C/START справа.

### Круглий D-pad (8 напрямків)
- Тиснеш по діагоналі → активуються 2 сторони одночасно (up+left для бігу наискоси)
- 4 основні (↑→↓←) + 4 діагоналі (↗↘↙↖)
- Центральний стік рухається до напрямку торху

### Керування (універсальне для всіх ігор)

| Клавіша (desktop) | Сенсорна кнопка (mobile) | Button ID | Дія |
|---|---|---|---|
| W/A/S/D | D-pad | 4/6/5/7 | рух |
| J | A | 9 | дія 1 (Пас / Attack) |
| K | B | 0 | дія 2 (Удар / Jump) |
| L | C | 8 | дія 3 (Through / Special) |
| Space | START | 3 | Start/Pause |

## Онлайн-нетплей

**Архітектура:** Host запускає гру локально, транслює canvas через WebRTC. Client отримує відео і відправляє свої інпути data-каналом. Host інжектує їх як P2 через `simulateInput(1, button, value)`.

**Флоу:**
1. Host: «Створити кімнату» → отримує 6-значний код
2. Client: вводить код → «Приєднатись»
3. Host керує P1 (меню, вибір команди)
4. Client керує P2 (своєю командою)
5. Відео стрім від хоста до клієнта

## Технічні деталі

- **EmulatorJS 4.2.3** (RetroArch + Genesis Plus GX WASM)
- **PeerJS 1.5.4** для сигналінгу WebRTC
- **10 ігор** у бібліотеці (4 реальні ROMs + 6 заглушок під заміну)
- **EJS_disableLocalStorage = true** — контролі не сбрасуються
- **EJS_defaultOptions = { 'virtual-gamepad': 'disabled' }** — кастомні сенсорні кнопки
- **CSS transform: scale(1, dynamic)** — розтягнення гри на портретному екрані
- **WebRTC**: SDP/ICE серіалізуються через `.toJSON()` (PeerJS не вміє native об'єкти)
- **Multi-game**: `?game=<id>` URL-параметр + GAMES config масив

## Деплой на GitHub Pages

1. Створи репозиторій
2. Додай всі файли:
```bash
git add index.html README.md fifa97.bin fifa98.bin nba_jam.bin bulls_vs_blazers.bin nhl94.bin syndicate.bin populous.bin commandos.bin pirates_gold.bin shadowrun.bin
git commit -m "SEGA Netplay — 10 games"
git push
```
3. Settings → Pages → Source: main
4. `https://<USER>.github.io/<repo>/`

## Файли

```
.
├── index.html                # ~65 КБ, єдиний файл
├── fifa97.bin                # 2 МБ — реальний ROM
├── fifa98.bin                # 2 МБ — реальний ROM
├── nba_jam.bin               # 2 МБ — реальний ROM
├── bulls_vs_blazers.bin      # 1 МБ — реальний ROM
├── nhl94.bin                 # ⚠️ заглушка → заміни на свій дамп
├── syndicate.bin             # ⚠️ заглушка → заміни на свій дамп
├── pirates_gold.bin          # ⚠️ заглушка → заміни на свій дамп
├── commandos.bin             # ⚠️ заглушка → заміни на свій дамп
├── shadowrun.bin             # ⚠️ заглушка → заміни на свій дамп
├── populous.bin              # ⚠️ заглушка → заміни на свій дамп
└── README.md
```

## Ліцензія

Код — публікодомен. ROMs — для збереження (abandonware).
