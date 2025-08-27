
# PixelmonTournaments (1.12.2 / Pixelmon 8.4.3)
Документация по использованию пресетов, правил, зон, наград и тиров

---

## Содержание
1. [Общие сведения](#общие-сведения)
2. [Presets (пресеты турниров)](#presets-пресеты-турниров)
3. [Rules (правила)](#rules-правила)
4. [Tiers (типы / наборы покемонов)](#tiers-типы--наборы-покемонов)
   - [Формат `.tier.txt`](#формат-tierrtxt)
   - [Примеры](#примеры)
   - [Встроенные фильтры](#встроенные-фильтры)
5. [Zones (зоны проведения боёв)](#zones-зоны-проведения-боёв)
6. [Rewards (награды)](#rewards-награды)
7. [BattleRules (правила Pixelmon-боя)](#battlerules-правила-pixelmon-боя)
8. [Команды администратора](#команды-администратора)
9. [FAQ и советы](#faq-и-советы)

---

## Общие сведения
Мод добавляет систему турниров в Pixelmon.  
Администраторы создают турниры, определяют правила, зоны, награды и ограничения.  
Игроки регистрируются и проходят боевые стадии до победителя.

---

## Presets (пресеты турниров)
Пресет хранится в `config/tournaments/presets/*.txt` и содержит настройки турнира.

### Формат файла
```txt
BattleRules=...
Rules:
  key:value
  ...
Rewards:
  key:value
  ...
Zones:
  UID
  UID
```

### Пример
```txt
BattleRules=levelCap=50,fullHeal=true,clauses=Sleep,OHKO,Species

Rules:
  battletype:single2p
  partymin:3
  partymax:6
  levelmin:50
  levelmax:50
  helditems:true
  tiers:OU,NOLEGENDARIES

Rewards:
  cashmoney:50000
  pokemon:Ditto gender:any nature=Jolly ivs:31/31/31/31/31/31

Zones:
  1
  3
```

---

## Rules (правила)

### Общие
- `battletype:<composition>` — тип боя (single2p, double2p, team).
- `setparty:` / `setlevel:<N>` — фиксировать состав или уровень.
- `teamcap:<N>[,autostart=true]` — лимит команд и автостарт.

### На команду
- `partymin:<N>` / `partymax:<N>` — минимальное/максимальное число покемонов.

### На игрока
- `levelmin:<N>` / `levelmax:<N>`
- `helditems:true|false`
- `healing:true|false`
- `randompokemon:<args>`
- `tiers:<список>` — разрешённые тиры
- `disallowedpokemon:<список>`
- `disallowedmove:<список>`
- `disallowedability:<список>`

### Децайдеры (ничьи/краши)
- `partycount:<weight>` — победа по числу покемонов.
- `healthtotal:<weight>` — победа по суммарному HP.

---

## Tiers (типы / наборы покемонов)

Файлы: `config/tournaments/tiers/*.tier.txt`

### Формат `.tier.txt`
- `displayname=...` — отображаемое имя (опционально).
- Далее список покемонов/спецификаций построчно.
- Комментарии `//` и пустые строки игнорируются.
- Каждая строка создаёт `PokemonSpec`. Все строки объединяются через **ИЛИ**.

### Примеры
```txt
displayname=RandomBattle
Goodra
Darmanitan
Roserade
Gastrodon
Crobat
Flygon
```

```txt
displayname=KantoOnly
Pikachu
Charizard
Blastoise
Venusaur
Alakazam
Gengar
```

### Использование в пресете
```txt
Rules:
  tiers:randombattle,NOLEGENDARIES
```

### Встроенные фильтры
- `NOLEGENDARIES`
- `NOULTRABEASTS`
- `GEN1`...`GEN6`
- `FE`, `NFE`, `LC` и т.п.

---

## Zones (зоны проведения боёв)

Хранятся в `config/tournaments/zones.json`.  
Зона = 6 спотов (по 3 на каждую сторону).  
Игроки телепортируются в споты при старте боя, после — возвращаются в `leaveZone`.

### Команды
- `/tournaments zones create`
- `/tournaments zones remove <uid>`
- `/tournaments zones setspot <uid> <side> <slot>`
- `/tournaments zones leavezone set`
- `/tournaments zones leavezone remove`
- `/tournaments zones send <uid>`

В пресете указываются **UID зон**.

---

## Rewards (награды)

- `cashmoney:<amount>`
- `items:<nbt-json>`
- `pokemon:<PokemonSpec>`
- `command:<console command>` (поддержка `{player}`)

Пример:
```txt
Rewards:
  cashmoney:5000
  items:{"itemstack-0":{"id":264,"Count":5}}
  command:say {player} чемпион!
```

---

## BattleRules (правила Pixelmon-боя)

Указываются одной строкой после `BattleRules=`.  
Ключи — стандартные правила Pixelmon.

Пример:
```txt
BattleRules=levelCap=50,raiseToCap=false,fullHeal=true,clauses=Sleep,OHKO,Species
```

---

## Команды администратора

- `/tournaments create`
- `/tournaments start|open|close|join|leave|forfeit`
- `/tournaments rules add/remove ...`
- `/tournaments rewards add/remove ...`
- `/tournaments battlerules set ...`
- `/tournaments presets list|load|save|delete|rename`
- `/tournaments zones ...`

---

## FAQ и советы

**Q: Можно ли наследовать тиры?**  
A: Нет. `.tier.txt` — только список разрешённых покемонов. Для ограничений используйте встроенные фильтры или правило `disallowedpokemon:`.

**Q: Как работают несколько тиров в списке?**  
A: Все применяются одновременно (**И**).

**Q: Что если все строки `.tier.txt` неверные?**  
A: Тир не будет зарегистрирован, и его использование в `tiers:` вызовет ошибку.

**Q: Чем отличаются `Rules:` и `BattleRules=`?**  
A: `Rules:` — правила турнира (уровни, предметы, баны).  
   `BattleRules=` — базовые правила Pixelmon-боя (кап уровней, клаузы и т.п.).

---

# Конец документации
