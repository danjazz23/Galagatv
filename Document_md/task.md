# Checklist de Desarrollo - Android TV Space Game

## Fase 1: Estructura del Proyecto y Arquitectura Base
- [x] Crear estructura de directorios en `Assets/` (según `AndroidTVGame.md` y `Organización.md`).
- [x] Configurar los ajustes del proyecto para Android (Target SDK 36, Vulkan/GLES3, URP, ASTC) en `ProjectSettings`.
- [x] Implementar el sistema `ServiceLocator` para la gestión global de servicios.
- [x] Implementar el `EventBus` para comunicación basada en eventos.
- [x] Crear escenas básicas (`Boot`, `Loading`, `MainMenu`, `Gameplay`, `Pause`, `Settings`, `Credits`).
- [x] Configurar la escena de `Boot` y el script de inicialización (`Bootstrap`).

## Fase 2: Configuración del Input (Android TV y Mandos)
- [x] Configurar `InputSystem_Actions` para mapear controles de Android TV (D-pad/Joystick de mandos).
- [x] Implementar el script `InputManager` integrado con el `ServiceLocator`.
- [x] Verificar lecturas de Input para Jugador 1 y Jugador 2.

## Fase 3: Movimiento y Mecánicas del Jugador
- [x] Crear prefab e implementar `PlayerController` para movimiento horizontal restringido por la pantalla.
- [x] Soporte para inicializar Jugador 1 (rojo) y Jugador 2 (azul) en pantalla.
- [x] Implementar disparo de proyectiles (bombas/láseres del jugador) y sistema de límite de proyectiles en pantalla.
- [x] Implementar sistema de Vidas (comienza con 3) y Score.
- [x] Añadir evento para otorgar una vida extra cada 10,000 puntos sumados.

## Fase 4: Formación e Inteligencia Artificial de Enemigos
- [x] Diseñar sistema de "Entrada con Piruetas" usando curvas Bezier o Splines de Unity 6 para que los enemigos hagan piruetas antes de asentarse.
- [x] Crear el manager de formación de enemigos (`FormationManager`).
- [x] Implementar la IA de ataque: los enemigos salen individualmente o en pequeños grupos de la formación para atacar.
- [x] Definir la resistencia y puntuación de cada enemigo (`Enemy_Bee`, `Enemy_Wasp`, `Enemy_Fly`).

## Fase 5: Mecánica de Tractor Beam (Secuestro de Naves) y Disparo Dual
- [x] Diseñar el comportamiento especial del `Enemy_Wasp` con el rayo tractor.
- [x] Implementar la captura de la nave del jugador.
- [x] Implementar el rescate: al destruir al Wasp secuestrador, la nave se acopla al jugador rescatador cambiándose al color de este.
- [x] Habilitar el modo de disparo dual (disparar dos proyectiles simultáneamente).

## Fase 6: Sistema de Niveles y Challenging Stage
- [x] Crear el `LevelManager` que maneje el flujo de rondas de enemigos.
- [x] Implementar el "Challenging Stage" (Nivel 3, 6, 9...): 40 enemigos haciendo piruetas, bonus de 10,000 pts si se eliminan todos.
- [x] Añadir la habilidad de triplicación de `Enemy_Bee` a partir del Nivel 4.

## Fase 7: Interfaz de Usuario (HUD, Menú y Marcadores)
- [x] Diseñar el HUD superior para Jugador 1 y Jugador 2.
- [x] Diseñar la parte inferior del marcador (naves acumuladas, records "Hero 7" y "Hero A").
- [x] Crear las pantallas y escenas con soporte para navegación por mando (Android TV).

## Fase 8: Sistemas Secundarios (Audio, Guardado y Ajustes)
- [x] Implementar el `SaveSystem` local para almacenar las mejores puntuaciones.
- [x] Configurar el `AudioManager` para reproducir música y efectos de sonido en formato `.ogg`.
- [x] Integrar la carga de configuraciones.

## Fase 9: Testeo, Optimización y Compilación (Producción)
- [x] Pruebas finales a 60 FPS estables.
- [x] Compilación (APK / AAB) y optimización de texturas (ASTC).
