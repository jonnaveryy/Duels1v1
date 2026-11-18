# Duels1v1 · Crystal vs Pot

Plugin de duelos 1v1 para **Paper** con dos modos: **Crystal PvP** (el mapa se rompe y se regenera) y **Pot PvP**. Tiene menús, reto por chat con botones, guardado del inventario, espectadores y placeholders para hologramas.

## Contenido

- [Instalar](#instalar)
- [Configurar los kits](#configurar-los-kits)
- [Crear una arena](#crear-una-arena)
- [Marcar spawns y verlos](#marcar-spawns-y-verlos)
- [Espectadores](#espectadores)
- [Placeholders y hologramas](#placeholders-y-hologramas)
- [Comandos de jugador](#comandos-de-jugador)
- [Comandos de admin](#comandos-de-admin)
- [Configuración](#configuración)
- [Permisos](#permisos)
- [Problemas comunes](#problemas-comunes)
- [Compilar](#compilar)

---

## Instalar

1. **Servidor Paper 1.21.7 o más nuevo.** Con esa versión los menús salen con el estilo nuevo de Minecraft (título, ítems con descripción y botones abajo). Desde **1.21.9**, cada botón de jugador muestra también su cabeza al lado del nombre. En Paper 1.21 a 1.21.6 el plugin funciona igual, pero con menús de cofre. No funciona en Spigot.
2. Copia `Duels1v1.jar` a la carpeta `plugins/`. Si vas a usar hologramas, agrega también [PlaceholderAPI](https://www.spigotmc.org/resources/placeholderapi.6245/) y un plugin de hologramas como DecentHolograms.
3. **Reinicia** el servidor. Se crea `plugins/Duels1v1/`:
   - `config.yml`: opciones y mensajes.
   - `arenas.yml`: las arenas.
   - `kits.yml`: los kits.
   - `regiones/`: copia de los bloques de cada arena de crystal.
   - `inventarios/`: inventarios guardados mientras alguien está en duelo.

> Usa reinicio, no `/reload`.

## Configurar los kits

Cada modo tiene un kit, que se toma de tu inventario tal cual: armadura, mano izquierda, barra rápida y el resto.

1. Ponte el kit de Crystal (armadura, totems, obsidiana, cristales, anclas, perlas…).
2. `/duel admin setkit crystal`
3. Ponte el kit de Pot y usa `/duel admin setkit pot`

Puedes repetir el comando cuando quieras para cambiar un kit.

## Crear una arena

Usa nombres simples y en orden: `crystal1` … `crystal10` y `pot1` … `pot10`.

### Arena de Crystal PvP (se rompe y se regenera)

1. **Construye la arena completa primero.** La regeneración la deja tal como esté cuando la guardes en el paso 5.
2. `/duel admin create crystal1 crystal`
3. Párate donde va cada jugador, mirando hacia el rival:
   - `/duel admin setspawn1 crystal1`
   - `/duel admin setspawn2 crystal1`
4. `/duel admin wand crystal1`: con esta varita, click izquierdo a un bloque de una esquina de abajo y click derecho a la esquina opuesta de arriba. La caja tiene que cubrir todo lo que se pueda romper, más un poco de aire por encima.
5. `/duel admin saveregion crystal1`: guarda los bloques. Si después cambias algo de la arena, vuelve a usarlo.
6. `/duel admin show crystal1` para revisarla. Cuando `/duel admin list` dice **libre**, ya está lista.

### Arena de Pot PvP (no se rompe)

1. `/duel admin create pot1 pot`
2. `/duel admin setspawn1 pot1` y `/duel admin setspawn2 pot1`
3. **Marca las esquinas** con `/duel admin wand pot1`. Así la arena queda protegida y los espectadores no pueden salir de ella. Las arenas de pot no necesitan `saveregion`.

## Marcar spawns y verlos

`setspawn1` y `setspawn2` guardan tu posición **y hacia dónde miras**.

`/duel admin show <arena>` dibuja la arena con partículas durante 15 segundos (solo las ves tú):

| Color | Qué es |
|---|---|
| 🟩 Verde | spawn1 |
| 🟥 Rojo | spawn2 |
| 🟦 Celeste | espectadores |
| 🟨 Amarillo | borde (pos1/pos2) |

| Qué marcas | Comando | Cómo |
|---|---|---|
| Jugador 1 | `/duel admin setspawn1 <arena>` | Donde estás parado |
| Jugador 2 | `/duel admin setspawn2 <arena>` | Donde estás parado |
| Espectadores | `/duel admin setspectator <arena>` | Opcional. Sin él, aparecen entre los dos spawns, dentro de la arena |
| Esquina 1 | `/duel admin wand <arena>` | Click izquierdo a un bloque |
| Esquina 2 | (misma varita) | Click derecho a un bloque |
| Esquinas sin varita | `/duel admin pos1\|pos2 <arena>` | Donde estás parado |

## Espectadores

Para mirar un duelo, el jugador usa `/duel` → **ESPECTAR**, o `/duel spectate <nick>`.

- El plugin le guarda el inventario, lo manda a la arena y lo pone **automáticamente en modo espectador**.
- Puede hacer click a uno de los que pelean para ver desde su cámara. Solo puede hacerlo con los jugadores de ese duelo.
- **No puede salir de la arena:** la caja de pos1/pos2 funciona como una pared invisible, incluso con el menú de teletransporte.
- Para salir: click en **[SALIR]** en el chat, o `/duel leave`. Vuelve a donde estaba con sus cosas.
- Cuando el duelo termina, todos los espectadores salen solos.

```yaml
spectators:
  enabled: true
  max-per-arena: 10      # 0 = sin límite
  notify-fighters: true  # avisa a los que pelean
  gamemode: SPECTATOR    # o ADVENTURE (volando con una cama para salir)
  border-padding: 0      # 0 = no pueden salir de la arena
```

El permiso `duels.spectate.bypass` deja entrar aunque la arena esté llena.

## Placeholders y hologramas

Requiere PlaceholderAPI. Cambia `crystal1` por el nombre de cada arena.

| Placeholder | Muestra |
|---|---|
| `%duels_crystal1_player1%` | Jugador 1 de esa arena, o «Esperando...» |
| `%duels_crystal1_player2%` | Jugador 2 de esa arena |
| `%duels_crystal1_status%` | Libre / En duelo / Regenerando |
| `%duels_crystal1_spectators%` | Cuántos están espectando |
| `%duels_crystal1_mode%` | Crystal PvP / Pot PvP |
| `%duels_free_crystal%` / `%duels_free_pot%` | Arenas libres de cada modo |
| `%duels_playing_crystal%` / `%duels_playing_pot%` | Jugadores peleando en cada modo |
| `%duels_opponent%` | Tu rival (para scoreboard) |
| `%duels_player1%` / `%duels_player2%` | Los jugadores de tu duelo |
| `%duels_arena%` | Arena donde estás peleando |
| `%duels_in_duel%` | `true` / `false` |

Ejemplo con DecentHolograms:

```
/dh create crystal1 &d&lCRYSTAL #1
/dh line add crystal1 1 &e%duels_crystal1_player1% &7vs &e%duels_crystal1_player2%
/dh line add crystal1 1 &7Estado: &f%duels_crystal1_status%
```

## Comandos de jugador

`/1v1` y `/duels` funcionan igual que `/duel`.

| Comando | Qué hace |
|---|---|
| `/duel` | Menú principal: **RETAR** o **ESPECTAR** |
| `/duel <nick>` | Elegir el modo y retar (TAB sugiere nombres) |
| `/duel <nick> crystal\|pot` | Retar directo a un modo |
| `/duel accept <nick>` | Aceptar (o click en **[ACEPTAR]** en el chat) |
| `/duel deny <nick>` | Rechazar |
| `/duel spectate [nick]` | Espectar el duelo de ese jugador. Sin nick abre la lista |
| `/duel leave` | Salir del modo espectador |

En **RETAR** hay una barra **Buscar jugador**. Escribe parte del nombre y toca **⌕ Buscar**: solo quedan los que tienen esas letras. Si queda uno solo, pasa directo a elegir el modo.

## Comandos de admin

| Comando | Qué hace |
|---|---|
| `/duel admin create <arena> crystal\|pot` | Crear arena |
| `/duel admin setspawn1\|setspawn2 <arena>` | Spawns de los jugadores |
| `/duel admin setspectator <arena>` | Spawn de espectadores |
| `/duel admin wand <arena>` | Varita para marcar esquinas |
| `/duel admin pos1\|pos2 <arena>` | Esquinas donde estás parado |
| `/duel admin show <arena>` | Ver spawns y borde con partículas |
| `/duel admin saveregion <arena>` | Guardar los bloques (crystal) |
| `/duel admin regen <arena>` | Regenerar a mano |
| `/duel admin setkit crystal\|pot` | Guardar tu inventario como kit |
| `/duel admin list` | Ver todas las arenas y su estado |
| `/duel admin info <arena>` | Detalles y qué le falta |
| `/duel admin tp <arena>` | Ir a la arena |
| `/duel admin delete <arena>` | Borrar arena |
| `/duel admin reload` | Recargar config y kits |

## Configuración

| Opción | Por defecto | Para qué |
|---|---|---|
| `request-expire-seconds` | 60 | Tiempo para aceptar un duelo |
| `countdown-seconds` | 5 | Cuenta regresiva (los jugadores quedan congelados) |
| `end-delay-seconds` | 3 | Tiempo que el ganador se queda en la arena |
| `max-duration-seconds` | 600 | Si se acaba el tiempo, es empate. 0 = sin límite |
| `regen-blocks-per-tick` | 30000 | Velocidad de regeneración. Bájalo si da lag |
| `broadcast-start` | false | Anunciar «X vs Y» a todo el servidor |
| `broadcast-winner` | true | Anunciar «Duels: X ganó el duelo contra Y» a todo el servidor |
| `allowed-commands-in-duel` | /msg /r /tell | Comandos permitidos dentro del duelo |
| `menus.style` | dialog | `dialog` = menús nuevos, `chest` = menús de cofre |

Los mensajes están en `messages:` con formato [MiniMessage](https://docs.advntr.dev/minimessage/format.html). Variables: `{jugador1}` `{jugador2}` `{ganador}` `{perdedor}` `{modo}` `{arena}`. Los textos de los menús están en `dialogs:`.

```yaml
duel-start: "<yellow>{jugador1} <gray>vs</gray> {jugador2}"
duel-winner: "<gold>Duels:</gold> <green>{ganador}</green> <gray>gano el duelo contra</gray> <red>{perdedor}</red>"
```

## Permisos

| Permiso | Por defecto | Para qué |
|---|---|---|
| `duels.use` | Todos | Usar /duel, retar y espectar |
| `duels.spectate.bypass` | OP | Espectar aunque la arena esté llena |
| `duels.admin` | OP | Comandos de admin, editar arenas, usar comandos en duelo |

## Problemas comunes

<details><summary>«No hay arenas libres»</summary>

Todas las arenas de ese modo están ocupadas o incompletas. Revisa con `/duel admin list`: a las que dicen *incompleta* les faltan los spawns, y si son de crystal también pos1, pos2 y `saveregion`.
</details>

<details><summary>«El kit todavía no está configurado»</summary>

Falta `/duel admin setkit crystal` o `/duel admin setkit pot`.
</details>

<details><summary>La arena de crystal no queda igual después de regenerar</summary>

La caja de pos1/pos2 no cubre todo. Revísala con `/duel admin show`, agrándala con la varita y vuelve a usar `saveregion` con la arena en buen estado.
</details>

<details><summary>Un espectador no queda en modo espectador</summary>

Otro plugin (por ejemplo Multiverse) cambia el modo de juego del mundo. Duels1v1 intenta evitarlo; si sigue pasando, revisa la opción de forzar modo de juego de ese plugin.
</details>

<details><summary>La regeneración da lag</summary>

Baja `regen-blocks-per-tick` a 10000 o 5000. La arena tardará un poco más en quedar libre.
</details>

## Compilar

Requiere Java 21+ y Maven:

```bash
mvn package
```

El plugin queda en `target/Duels1v1.jar`.
