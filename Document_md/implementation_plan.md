# Plan de Desarrollo Modular - Android TV Space Game

Este plan define las fases de desarrollo paso a paso para construir el videojuego tipo Galaga diseñado para Android TV (Unity 6.3 LTS). Dado el límite de tokens y la necesidad de mantener un flujo de desarrollo estructurado y sin errores, el proyecto se dividirá en **9 fases progresivas**.

---

## User Review Required

> [!IMPORTANT]
> **Enfoque Paso a Paso (Modular):**
> Para optimizar los tokens y evitar la sobrecarga de código, se recomienda encarecidamente realizar el desarrollo **fase por fase**. No avanzaremos a la siguiente fase hasta que la actual esté completada y verificada en el editor o dispositivo.

> [!NOTE]
> El proyecto se encuentra actualmente vacío (solo contiene el archivo de `InputSystem_Actions` y una escena vacía `SampleScene`). Empezaremos estructurando las carpetas del proyecto según las especificaciones técnicas.

---

## Open Questions

> [!IMPORTANT]
> **Preguntas de diseño para el usuario:**
> 1. **Rayo Tractor y Doble Nave en un Solo Jugador:** En el juego original Galaga (1 jugador), si tu nave es secuestrada y la rescatas en la siguiente vida, juegas con nave doble. En el modo cooperativo de 2 jugadores que describe tu documentación, ¿cómo funciona exactamente el secuestro? Si secuestran al Jugador 1, ¿el Jugador 2 puede disparar al Wasp para liberar la nave del Jugador 1 y acoplársela a sí mismo (Jugador 2), o se devuelve al Jugador 1?
> 2. **Assets y Modelos 3D:** ¿Cuentas ya con los archivos `.fbx` y texturas detallados en `Requisitos_modelos.md`, o utilizaremos formas geométricas simples (cubos/esferas/materiales planos) para prototipar inicialmente antes de importar el arte final?

---

## Proposed Changes

A continuación se detalla la checklist del plan de desarrollo estructurado. Cada fase se implementará de manera independiente.

### Fase 1: Estructura del Proyecto y Arquitectura Base
Establecer la estructura de carpetas y los sistemas esenciales de desacoplamiento de código (Service Locator y Event Bus).
*   [ ] Crear estructura de directorios en `Assets/` (`Art/`, `Audio/`, `Prefabs/`, `Scenes/`, `Scripts/`, etc.).
*   [ ] Configurar los ajustes del proyecto para Android (Target SDK 36, Vulkan/GLES3, URP, ASTC) como se indica en `Configuración_específica.md`.
*   [ ] Implementar el sistema `ServiceLocator` para la gestión global de servicios.
*   [ ] Implementar el `EventBus` para comunicación basada en eventos.
*   [ ] Configurar la escena de `Boot` y el script de inicialización (`Bootstrap`).

---

### Fase 2: Configuración del Input (Android TV y Mandos)
Configurar el control de la nave para que responda a mandos y mandos de Android TV usando el Input System moderno de Unity.
*   [ ] Configurar `InputSystem_Actions` para mapear controles de Android TV (D-pad/Joystick de mandos).
*   [ ] Implementar el script `InputManager` integrado con el `ServiceLocator`.
*   [ ] Verificar lecturas de Input para Jugador 1 y Jugador 2.

---

### Fase 3: Movimiento y Mecánicas del Jugador (Modo 1 y 2 Jugadores)
Crear los controladores del jugador y los proyectiles básicos.
*   [ ] Crear prefab e implementar `PlayerController` para movimiento horizontal restringido por la pantalla.
*   [ ] Soporte para inicializar Jugador 1 (rojo) y Jugador 2 (azul) en pantalla.
*   [ ] Implementar disparo de proyectiles (bombas/láseres del jugador) y sistema de límite de proyectiles en pantalla.
*   [ ] Implementar sistema de Vidas (comienza con 3) y Score.
*   [ ] Añadir evento para otorgar una vida extra cada 10,000 puntos sumados.

---

### Fase 4: Formación e Inteligencia Artificial de Enemigos
Diseñar el enjambre de alienígenas insectoides, sus piruetas de entrada y su posicionamiento en formación.
*   [ ] Diseñar sistema de "Entrada con Piruetas" usando curvas Bezier o Splines de Unity 6 para que los enemigos hagan piruetas antes de asentarse.
*   [ ] Crear el manager de formación de enemigos (`FormationManager`) para organizar filas de `Enemy_Fly` (arriba), `Enemy_Wasp` (medio) y `Enemy_Bee` (abajo).
*   [ ] Implementar la IA de ataque: los enemigos salen individualmente o en pequeños grupos de la formación para atacar disparando bombas y actuando como kamikazes.
*   [ ] Definir la resistencia de cada tipo de enemigo:
    *   `Enemy_Bee`: 1 disparo, otorga 100 puntos.
    *   `Enemy_Wasp`: 2 disparos, otorga 200 puntos.
    *   `Enemy_Fly`: 3 disparos, otorga 300 puntos.

---

### Fase 5: Mecánica de Tractor Beam (Secuestro de Naves) y Disparo Dual
Implementación de la mecánica clave del Wasp.
*   [ ] Diseñar el comportamiento especial del `Enemy_Wasp` cuando desciende a medio camino para proyectar el rayo tractor ("Tractor Beam").
*   [ ] Implementar la captura de la nave del jugador si entra en contacto con el rayo tractor (la nave se vuelve enemiga y se acopla detrás del Wasp).
*   [ ] Implementar el rescate: al destruir al Wasp secuestrador, la nave capturada se acopla al lado de la nave del jugador rescatador.
*   [ ] Habilitar el modo de disparo dual (disparar dos proyectiles simultáneamente desde las naves acopladas).

---

### Fase 6: Sistema de Niveles y Challenging Stage
Estructurar la dificultad y el nivel especial de bonus cada 3 rondas.
*   [ ] Crear un `LevelManager` que maneje el flujo de rondas de enemigos.
*   [ ] Implementar el "Challenging Stage" (Nivel 3, 6, 9...):
    *   Spawn de 40 enemigos en oleadas realizando piruetas predeterminadas sin disparar.
    *   Lógica de recompensas: +10,000 puntos si se destruyen todos, o 100 puntos por cada uno en caso contrario.
*   [ ] Añadir la habilidad de triplicación de `Enemy_Bee` a partir del Nivel 4.

---

### Fase 7: Interfaz de Usuario (HUD, Menú y Marcadores)
Construir todas las pantallas y el HUD del juego.
*   [ ] Diseñar el HUD superior:
    *   Esquina superior izquierda: Score y vidas del Jugador 1 (rojo).
    *   Esquina superior derecha: Score y vidas del Jugador 2 (azul).
*   [ ] Diseñar la parte inferior del marcador para mostrar naves acumuladas.
*   [ ] Programar la visualización especial de records millonarios:
    *   A los 7,000,000 puntos, limpiar naves inferiores y mostrar "Hero 7".
    *   A los 10,000,000 puntos, mostrar "Hero A".
*   [ ] Crear las pantallas y escenas de: `Boot`, `Loading`, `MainMenu`, `Pause`, `Settings` y `Credits`, asegurando soporte total para selección y navegación por mando (Android TV).

---

### Fase 8: Sistemas Secundarios (Audio, Guardado y Ajustes)
Integrar la persistencia de datos y pulido técnico.
*   [ ] Implementar el `SaveSystem` local para almacenar las mejores puntuaciones (High Score) usando PlayerPrefs o JSON.
*   [ ] Configurar el `AudioManager` para reproducir música de fondo y efectos de sonido en formato `.ogg`.
*   [ ] Integrar la carga de configuraciones (volumen, dificultad, idioma) desde el inicio.

---

### Fase 9: Testeo, Optimización y Compilación (Producción)
Verificaciones finales y optimizaciones de rendimiento para Android TV.
*   [ ] Probar rendimiento en el editor y simulación de hardware Android TV a 60 FPS estables.
*   [ ] Realizar compilaciones de prueba (APK / AAB) orientadas a ARM64.
*   [ ] Optimización de texturas (compresión ASTC) y shaders.

---

## Verification Plan

### Automated Tests
*   Pruebas unitarias/de integración simples si se requiere (usando Unity Test Framework) para verificar el cálculo de puntuaciones, ganancia de vidas a los 10k puntos, y lógica de acoplamiento de naves duales.

### Manual Verification
*   Simular el uso de gamepad en el Unity Editor para controlar los dos jugadores.
*   Validar la navegación del UI mediante teclado/gamepad simulando el control remoto de una Android TV.
