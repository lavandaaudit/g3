# SEGA NETPLAY — ONLINE

Однострілкова сторінка для **GitHub Pages**: гра в класичні ігри Sega Mega Drive прямо у браузері з підтримкою онлайн-нетплею через WebRTC.

**Без встановлення. Без реєстрації.** ROMs у репозиторії — ігри стартують автоматично. Працює на **десктопі та мобільному**.

## Ігри (21 шт)

| Гра | Жанр | Рік | Розмір |
|-----|------|-----|--------|
| ⚽ FIFA 97 Gold | Sports | 1996 | 2 МБ |
| ⚽ FIFA 98 | Sports | 1997 | 2 МБ |
| ⚽ FIFA International Soccer | Sports | 1993 | 2 МБ |
| 🏒 NHL '94 | Sports | 1993 | 1 МБ |
| 🏀 NBA Jam | Sports | 1994 | 2 МБ |
| 🏀 Bulls vs Blazers | Sports | 1993 | 1 МБ |
| 🥇 Olympic Summer Games | Sports | 1996 | 2 МБ |
| 🥋 Mortal Kombat 3 | Fighting | 1995 | 4 МБ |
| 🏎️ Formula One | Racing | 1993 | 1 МБ |
| 🏎️ F-1 Grand Prix (JPN) | Racing | 1991 | 1 МБ |
| 🏁 Super Skidmarks | Racing | 1995 | 2 МБ |
| 💥 Comix Zone | Action | 1995 | 2 МБ |
| 🐢 TMNT: Hyperstone Heist | Action | 1992 | 1 МБ |
| 🔫 Soldiers of Fortune | Action | 1993 | 1.5 МБ |
| 🕶️ Syndicate | Strategy | 1994 | 2 МБ |
| 🌍 Populous II | Strategy | 1993 | 1 МБ |
| 👑 Power Monger | Strategy | 1993 | 1 МБ |
| 🎖️ Commandos | Strategy | 2002 | 2 МБ |
| ⚓ Pirates! Gold | Adventure | 1993 | 1 МБ |
| 🌆 Shadowrun | RPG | 1994 | 2 МБ |
| 🐿️ Spirou | Platformer | 1995 | 1 МБ |

> Усі 21 ROM у репозиторії — реальні дампи (перевірено по заголовках SEGA MEGA DRIVE / GENESIS). Файл `Pirates! Gold (USA).gen` — дублікат `pirates_gold.bin`, його можна видалити.

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
- **21 гра** у бібліотеці (усі — реальні ROMs); кімнати нетплею ізольовані по грі: префікс `g3-<game-id>-`, а посилання-запрошення несе `?game=<id>#<КОД>`
- **EJS_disableLocalStorage = true** — контролі не сбрасуються
- **EJS_defaultOptions = { 'virtual-gamepad': 'disabled' }** — кастомні сенсорні кнопки
- **CSS transform: scale(1, dynamic)** — розтягнення гри на портретному екрані
- **WebRTC**: SDP/ICE серіалізуються через `.toJSON()` (PeerJS не вміє native об'єкти)
- **Multi-game**: `?game=<id>` URL-параметр + GAMES config масив

## Деплой на GitHub Pages

1. Створи репозиторій
2. Додай всі файли:
```bash
git add index.html README.md *.bin
git commit -m "SEGA Netplay — 21 games"
git push
```
3. Settings → Pages → Source: main
4. `https://<USER>.github.io/<repo>/`

## Файли

```
.
├── index.html                        # ~70 КБ, єдиний файл + каталог на 21 гру
├── README.md
└── 21 ROM-файл (.bin/.gen)           # усі реальні дампи
    ├── fifa97.bin, fifa98.bin, FIFA International Soccer.bin, nhl94.bin
    ├── nba_jam.bin, bulls_vs_blazers.bin, Olympic Summer Games.bin
    ├── Mortal Kombat 3.bin
    ├── Formula One (USA).bin, Nakajima Satoru Kanshuu - F-1 Grand Prix.bin, Super Skidmarks.bin
    ├── Comix Zone (USA).bin, Teenage Mutant Ninja Turtles - The Hyperstone Heist (USA).bin
    ├── Soldiers of Fortune (USA).bin
    ├── syndicate.bin, populous.bin, Power Monger.bin, commandos.bin
    ├── pirates_gold.bin              # (поруч лежить дублікат Pirates! Gold (USA).gen — можна видалити)
    ├── shadowrun.bin
    └── Spirou.bin
```

## Ліцензія

Код — публікодомен. ROMs — для збереження (abandonware).
