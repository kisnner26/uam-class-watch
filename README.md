# UAM Class Watch

app de **Apple Watch** para [UAM Class](https://github.com/kisnner26/uam-class):
tu horario de clases en la muñeca, sin sacar el teléfono.

## qué hace

- muestra la **próxima clase** con hora, sección y aula, y lo que queda del día debajo
- si hoy ya no queda nada, salta a la primera clase de los próximos días
- **avisos en el reloj** 1 hora y 15 minutos antes de cada clase, todas las semanas
- funciona **sin el iPhone cerca**: guarda el último horario recibido en el propio reloj
- se sincroniza sola: el iPhone manda el horario por `WatchConnectivity` y el reloj replanifica los avisos cada vez que llega uno nuevo

swiftui, sin dependencias externas. watchOS 10+.

## cómo se conecta con UAM Class

el horario se escribe una vez en la app de UAM Class (Mac / iPhone) y vive ahí. el iPhone lo empuja al reloj con `WCSession.updateApplicationContext`, y el reloj también puede pedirlo (`sendMessage`) si arranca antes de haber recibido algo.

```
UAM Class (iPhone)  ──WatchConnectivity──▶  UAM Class Watch
   horario                                    ContentView + avisos locales
```

`Shared/ScheduleModels.swift` es una copia del modelo `ClassSlot` / `Weekday` de la app principal. si cambias el modelo allá, actualiza este archivo también.

## estructura

```
UAMClassWatch/
  UAMClassWatchApp.swift            entrada de la app
  ContentView.swift                 próxima clase + lo que queda hoy
  WatchConnectivityManager.swift    puente iPhone → reloj y persistencia local
  NotificationPlanner.swift         avisos 1 h / 15 min antes
  Assets.xcassets/                  icono y color de acento
Shared/ScheduleModels.swift         modelo del horario
project.yml                         definición del proyecto (XcodeGen)
```

## compilar

requiere Xcode 16+ y [XcodeGen](https://github.com/yonaskolb/XcodeGen).

```bash
brew install xcodegen
xcodegen generate
open UAMClassWatch.xcodeproj
```

en `project.yml` pon tu `DEVELOPMENT_TEAM` (o elige tu equipo en Xcode → Signing) para instalarla en un reloj real. para el simulador no hace falta firma.

para instalar la versión completa (iPhone + reloj), usa el proyecto `iOS/` del repo principal, que embebe esta app dentro de la app del iPhone.

## licencia

pendiente de definir.
