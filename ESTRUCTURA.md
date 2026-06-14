# Estructura del código de index.html

El juego es un solo archivo HTML con tres bloques: el CSS dentro de la etiqueta style, el HUD y las pantallas en el body, y todo el motor del juego dentro de una sola etiqueta script. Para encontrar cada parte rápido conviene buscar los comentarios de sección en mayúsculas, porque el código ya viene dividido con banners del tipo "ESTADO GLOBAL", "ARMAS", "UPDATE PRINCIPAL" y "RENDER". Abajo está el mapa de las partes importantes y qué hace cada una.

## CSS y HUD (cabecera y body)

En la etiqueta style están las variables de color en :root y los estilos del HUD. Las clases nuevas que conviene conocer son hptext, que es el texto de vida total y porcentaje sobreimpreso en la barra de vida, lvlleft, que es el nivel mostrado arriba a la izquierda, y el bloque upbar con uprow, dots y dot, que es la tira de mejoras elegidas con icono y puntitos. La clase dot.on es el puntito pintado (lleno) y dot a secas es el vacío. También está el estilo de nextlevel, que es el cartel grande de "SIGUIENTE NIVEL" que aparece al derrotar a Zeus.

En el body están el canvas, el contenedor hud con la barra de vida, la barra de experiencia, el nivel a la izquierda y la tira upbar, el panel stats arriba a la derecha con nivel, derrotados y tiempo, los avisos weaponlog y bosslog, el cartel nextlevel, y las tres pantallas: menu de selección de personaje, levelup para elegir mejora y gameover.

## Utilidades y proyección isométrica

Arriba del script están las funciones chicas rand, dist2, clamp, hash2 y mulberry, que sirven para azar y ruido determinista del escenario. Justo después está la proyección isométrica con isoX e isoY. Son la base de todo el dibujo y del movimiento. La regla es que el mundo se guarda en coordenadas planas wx y wy, y estas dos funciones las convierten a pantalla. La inversa de esta proyección es la que se usa en el movimiento para que caminar se sienta uniforme en todas las direcciones.

## Sprites

La constante PAL es la paleta de letras a colores. SPR guarda los sprites de torso de cada héroe en filas de texto, y SPR_ENEMY es el enemigo humanoide base. drawSprite pinta una grilla de letras y drawActor arma un actor completo sumando torso, piernas animadas y rebote al caminar. Los enemigos gigantes y especiales no usan estos sprites sino dibujo directo con rectángulos dentro de drawEnemy.

## Personajes y armas

CHARS define los cuatro héroes: Kratos, Atreus, Freya y el Fantasma de Esparta. Cada uno tiene nombre, color, sprite, descripción y una lista de armas con el nivel en que se desbloquea cada una. Si se quiere cambiar qué arma sale a cada nivel, se edita acá.

WEAPONS es el registro de todas las armas. Cada arma es un objeto con un campo cd de enfriamiento y una función fire que se ejecuta cuando toca disparar. Algunas tienen además passive, persistent o tick para efectos que corren siempre. La función totalDmg centraliza el daño y aplica el multiplicador de daño del jugador y el bonus de furia. Los ajustes de balance de daño de los kits flojos se hicieron cambiando los números que se pasan a totalDmg dentro de cada arma, sin tocar los cd para no alterar la cadencia.

updateWeapons recorre las armas del héroe según su nivel y las dispara respetando cooldown y el multiplicador cdMul.

## Enemigos

ETYPES es el bestiario con vida, velocidad, daño, radio y comportamiento de cada enemigo. Acá está agregado zeus como jefe con vida fija. spawnTable y pickType deciden qué enemigo aparece según el tiempo transcurrido. spawnEnemy crea el enemigo fuera de pantalla y aplica el escalado de vida y daño por tiempo, que se suavizó para que los números no crezcan tanto. Zeus es la excepción y usa vida fija. killEnemy maneja la muerte, las partículas y los orbes de experiencia. damageEnemy aplica daño y genera el texto de daño.

## Compañeros y proyectiles

updateCompanions mueve el lobo de Atreus y los pájaros que orbitan. updateProjectiles mueve y resuelve las colisiones de flechas, hacha bumerán, rayos guiados, almas y demás disparos.

## Efectos de área

updateEffects procesa los efectos con vida limitada como las ondas de escarcha, el barrido de espadas o la lluvia. Acá también se resuelve el impacto del rayo de Zeus: el aviso zeuswarn, cuando se le acaba la vida, revisa si el jugador sigue parado en la marca y recién ahí aplica el daño, lo que hace que el rayo sea esquivable.

## Mejoras

UPGRADES es la lista de mejoras con tope. Cada una tiene clave, icono, nombre, tope max y la función apply que sube el nivel y recalcula el multiplicador. Entre ellas están las Botas de Prometeo, que suben la velocidad, y el Favor de Hermes, que amplía el radio para recoger orbes a través de pickupMul. Los topes se subieron en dos a cada mejora y la vida sigue con tope infinito. La función allCappedMaxed indica si el jugador ya maximizó todas las mejoras con tope, lo que dispara la aparición de Zeus. gainXP y checkLevel manejan la subida de nivel, y showLevelUp arma las tres cartas para elegir. renderUpgradeStrip dibuja la tira de mejoras elegidas en el HUD con el icono y los puntitos. El puntito lleno se pinta dorado y brillante y el vacío queda oscuro, para que se note el progreso.

## Escenario

CHUNK, pillarCache y chunkPillars generan las columnas de forma procedural por sectores. chunkPillars tiene una rama para el Olimpo que genera columnas de mármol densas y estatuas. pillarsNear junta las columnas cercanas y collidePillars resuelve que el jugador y los enemigos no las atraviesen. TEMPLE y STATUE son los monumentos fijos de Grecia. drawColumn dibuja una columna y acepta un parámetro de mármol para el Olimpo. drawStatueSmall dibuja una estatua de mármol chica. drawTemple y drawStatue dibujan los monumentos grandes de Grecia.

## Jefe Zeus y transición al Olimpo

Esta sección agrupa todo lo del jefe final del primer nivel. showNextLevel y hideNextLevel manejan el cartel grande. triggerZeus se llama cuando el jugador maximiza todas las mejoras con tope o a los diez minutos, hace desaparecer a los enemigos normales e invoca a Zeus. spawnZeus crea al jefe con sus temporizadores de rayos y de fase. updateZeus es la inteligencia de Zeus y tiene dos fases. En la fase normal mantiene distancia, lanza rayos solo si el jugador no está demasiado cerca y deja pegarle de cerca a melee. Al cruzar un umbral de vida entra en la segunda fase, la tormenta: se vuelve invulnerable, se cura un veinte por ciento y hace caer rayos que siguen la posición del jugador, así que hay que mantenerse en movimiento para esquivarlos. Cuando termina de recuperar ese veinte por ciento deja de ser invulnerable y vuelve a atacar normalmente. La invulnerabilidad se respeta en damageEnemy, que ignora el daño mientras Zeus está invulnerable. onZeusDefeated arranca la transición. enterOlympus cambia el escenario a mármol blanco, reubica al jugador, le devuelve la vida, fija el inicio del escalado del Olimpo y reanuda las hordas.

## Enemigos del Olimpo

El Olimpo usa enemigos propios y distintos a los de Grecia, todos con aspecto sagrado. El sprite SPR.robed es una figura con túnica que se recolorea por enemigo con su paleta. En ETYPES están el acolito, una figura con túnica que persigue de cerca, el centinela, una versión más resistente con detalles dorados, y el alado, que es el enemigo especial del nivel. El alado vuela manteniendo distancia y tira lanzas que se pueden esquivar moviéndose, parecidas a las flechas de Atreus. Las lanzas son proyectiles de tipo espear que se procesan en updateProjectiles y dañan al jugador, y se dibujan en drawProjectiles. La función spawnTable devuelve la tabla de enemigos del Olimpo cuando el realm es olimpo, y el escalado de vida y daño usa un tiempo relativo que arranca al entrar al Olimpo, guardado en olympusStart, para que los enemigos no aparezcan inflados por el tiempo global.

## Items desparramados

La constante G.items guarda los objetos que aparecen tirados por el mapa y G.itemTimer controla cada cuánto sale uno. spawnItem crea un item en una posición desparramada alrededor del jugador y elige al azar entre vida y Cuerno de Amaltea. Hay dos tipos. El de vida es una bolita verde que cura el cincuenta por ciento de la vida máxima. El Cuerno de Amaltea es el imán: al agarrarlo activa G.vacuum por un tiempo corto, y mientras está activo todas las bolitas de experiencia del mapa son atraídas hacia el jugador sin importar la distancia, así se recogen las que quedaron sueltas. La recogida de ambos items está en el update, justo después del bucle de orbes. El dibujo está en render: la bolita verde con una cruz y el cuerno dorado, que usa la función drawHorn. Las bolitas de experiencia que sueltan los enemigos ahora son rojas en vez de azules, y se dibujan en el mismo bloque de orbes del render.

## Update principal

La función update es el corazón de la lógica por cuadro. Adentro está, en orden, el movimiento uniforme del jugador con la inversa de la proyección, el disparador del jefe a los diez minutos, el control de aparición de hordas que se pausa durante la pelea con Zeus, el temporizador del cartel de nivel, el bucle de enemigos con sus comportamientos especiales y la rama de Zeus, las armas y compañeros, los orbes de experiencia, las partículas, los textos de daño, la muerte del jugador y por último la actualización del HUD con la vida, el porcentaje, el nivel y la tira de mejoras.

## Render

La función render dibuja todo en orden de profundidad. Primero el fondo, que cambia entre el cielo de Grecia y el del Olimpo. Después el suelo con baldosas, que son arena en Grecia y mármol blanco en el Olimpo. Luego los orbes, las partículas, y la lista de dibujables ordenada por profundidad que incluye enemigos, jugador, lobo, pájaros, columnas, estatuas y monumentos. drawEnemy tiene una rama por cada tipo especial, incluida la de Zeus. drawWolf, drawProjectiles y drawEffects completan el dibujo. drawEffects incluye el dibujo del aviso del rayo y del rayo de Zeus cayendo. Al final están la viñeta y la alerta roja de vida baja.

## Game loop

Al final del script está loop, que mide el delta de tiempo, llama a update solo si el estado es playing y a render salvo en el menú, y se vuelve a programar con requestAnimationFrame para correr a sesenta cuadros por segundo.

## Dónde tocar para cada cosa

Para cambiar el balance de daño se editan los números dentro de WEAPONS. Para cambiar los topes de mejora se edita UPGRADES. Para cambiar cuándo aparece Zeus se edita el disparador dentro de update que compara el tiempo con seiscientos segundos. Para cambiar el aspecto del Olimpo se editan la rama de olimpo en chunkPillars, los colores del suelo en render y drawColumn con drawStatueSmall. Para cambiar la velocidad base de los personajes se edita speed dentro de newPlayer.
