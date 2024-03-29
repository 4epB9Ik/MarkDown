Чтобы реализовать модификацию для Minecraft Forge 1.12.2, используя API мода Pixelmon Reforged 8.4.3, которая обеспечивает защиту легендарному покемону от взаимодействий со стороны других игроков, нам нужно выполнить несколько шагов:

1. **Слушатель События Появления Легендарного Покемона**: В первую очередь, нам нужно создать слушатель события `LegendarySpawnEvent.DoSpawn`, чтобы определить момент появления легендарного покемона.

2. **Запоминание Игрока**: В момент срабатывания события, мы должны запомнить игрока, связанного с этим событием (например, того, кто находится ближе всего к точке спавна покемона или кто выполнил действие, приведшее к спавну).

3. **Отмена Событий Взаимодействия**: После этого, нужно реализовать логику для отмены событий `PokeballImpactEvent`, если они не относятся к запомненному игроку, на протяжении заданного временного интервала.

4. **Таймер Защиты**: Защита должна действовать в течение заданного временного интервала (в нашем случае, 4 минуты). Это можно реализовать, используя таймер или планировщик задач внутри модификации.

Примерный код может выглядеть следующим образом:

```java
@SubscribeEvent
public void onLegendarySpawn(LegendarySpawnEvent.DoSpawn event) {
    // Запоминаем игрока, связанного с событием
    EntityPlayerMP targetPlayer = event.player;

    // Запоминаем покемона для защиты
    EntityPixelmon legendaryPokemon = event.entity;

    // Устанавливаем таймер защиты
    long protectionEndTime = System.currentTimeMillis() + TimeUnit.MINUTES.toMillis(4);

    // Добавляем обработчик события взаимодействия с покеболом
    MinecraftForge.EVENT_BUS.register(new Object() {
        @SubscribeEvent
        public void onPokeballImpact(PokeballImpactEvent event) {
            // Проверяем, принадлежит ли покемон к защищаемому и текущее время в рамках защиты
            if (event.getHitEntity() == legendaryPokemon && System.currentTimeMillis() <= protectionEndTime) {
                // Проверяем, является ли игрок тем, кто имеет право взаимодействовать
                if (!event.player.equals(targetPlayer)) {
                    event.setCanceled(true); // Отменяем событие, если игрок не тот
                }
            }
        }
    });

    // Удаление обработчика события после окончания времени защиты
    new Timer().schedule(new TimerTask() {
        @Override
        public void run() {
            MinecraftForge.EVENT_BUS.unregister(this);
        }
    }, TimeUnit.MINUTES.toMillis(4));
}
```

Этот код является примерным и может потребовать адаптации под вашу конкретную ситуацию и требования. Убедитесь, что у вас есть соответствующие импорты для всех используемых классов. Кроме того, важно тестировать модификацию в различных условиях, чтобы убедиться, что она работает корректно и не влияет на игровой процесс негативно.
