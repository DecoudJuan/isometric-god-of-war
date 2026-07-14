# ⚔️ Isometric God of War — Survivors

Un juego estilo **Vampire Survivors** ambientado en el universo de **God of War**, con vista isométrica 2.5D y gráficos pixel-art generados 100% por código. Sobreviví a hordas infinitas de criaturas nórdicas y griegas entre las ruinas arenosas de Esparta y Atenas.

**Todo el juego vive en un único archivo `index.html`.** Sin dependencias, sin assets externos, sin build. Solo HTML5 Canvas y JavaScript puro.

🎮 **Jugar ahora:** https://decoudjuan.github.io/isometric-god-of-war

---

## 🕹️ Cómo jugar

| Acción | Control |
|---|---|
| Moverse | `WASD` o flechas |
| Atacar | **Automático** — las armas disparan solas |
| Elegir mejora | Click en la carta al subir de nivel |

Recogé los **orbes de XP** que sueltan los enemigos para subir de nivel. Cada nivel desbloquea una mejora, y en los niveles clave (2-3, 5, 8, 10 y 14) tu héroe despierta una nueva arma o habilidad.

---

## 🛡️ Héroes jugables

### Kratos
El Dios de la Guerra. Daño cuerpo a cuerpo devastador.
`Puños Espartanos` → `Hacha Leviatán` (melee o bumerán) → `Espadas del Caos` → `Escudo Guardián` → `Golpe de Escarcha` → `Ira de Esparta`

### Atreus
Ágil y a distancia, con compañeros animales.
`Arco de Garra` → `Flechas de Luz` → `Invocación del Lobo` → `Lluvia de Flechas` → `Tormenta de Pájaros` → `Colmillos de Fenrir`

### Freya
La Bruja de los Bosques. Magia Vanir y control de masas.
`Proyectil Vanir` → `Raíces de Vanaheim` → `Espada Mardöll` → `Alas de Valquiria` → `Tormenta Vanir` → `Bendición de Freya`

### Fantasma de Esparta
El Kratos de la trilogía griega, con los poderes de los dioses del Olimpo.
`Espadas de Atenea` → `Arco de Apolo` → `Furia de Poseidón` → `Vellocino Dorado` → `Ejército de Hades` → `Ira de los Dioses`

---

## 💀 Bestiario

| Enemigo | Origen | Peligro |
|---|---|---|
| Draugr | GoW 2018 | El clásico no-muerto nórdico |
| Pesadilla | GoW 2018 | Ojo volador, rápido y frágil |
| Legionario | Trilogía griega | Esqueleto con escudo de bronce |
| Gorgona | Trilogía griega | Te ralentiza al tocarte |
| Revenant | GoW 2018 | Se teletransporta hacia vos |
| Caminante de Hel | GoW 2018 | Tanque de hielo, te congela |
| Minotauro | Trilogía griega | Embiste a toda velocidad |
| Troll | GoW 2018 | Élite con garrote, mucha vida |
| **Cíclope** | Trilogía griega | **Jefe** — aparece cada 90 segundos |

Los enemigos escalan en vida y daño con el tiempo. Cuanto más sobrevivas, peor se pone.

---

## ✨ Mejoras (con tope)

Para mantener el juego balanceado, cada mejora tiene un máximo de niveles:

| Mejora | Efecto | Tope |
|---|---|---|
| ⚔️ Furia de Esparta | +18% de daño | 8 |
| 🌀 Alcance Rúnico | +15% de área | 6 |
| 🥾 Paso de Ciervo | +10% de velocidad | 5 |
| ⏳ Favor de las Nornas | -8% de enfriamiento | 5 |
| 🍖 Manzanas de Idunn | +25 vida máx. y cura 50% | **∞** |

Solo la vida es infinita. Elegí con sabiduría.

---

## 🏛️ El mundo

Mapa infinito de arena espartana con generación procedural determinista:

- **Columnas de mármol** (algunas rotas) que bloquean el paso — usalas para zigzaguear entre hordas.
- **Ruinas dispersas**: arcos, muros derruidos, bustos de guerreros y escombros con ánforas.
- **Dos monumentos fijos** para encontrar explorando: un **Panteón** con columnata y frontón al noreste, y la **estatua del Olimpo** al suroeste.

Como la generación es determinista, si volvés al mismo lugar la misma ruina sigue ahí.

---

## 🚀 Correr el juego

No hace falta instalar nada:

```bash
git clone https://github.com/DecoudJuan/isometric-god-of-war.git
cd isometric-god-of-war
# abrí index.html en el navegador, listo
```

O publicalo con **GitHub Pages**: en el repo, `Settings → Pages → Source: main / root`, y queda en `https://<tu-usuario>.github.io/isometric-god-of-war`.

---

## 🧱 Tecnología

- HTML5 Canvas + JavaScript vanilla (un solo archivo, ~2000 líneas)
- Proyección isométrica 2:1 con orden de profundidad (painter's algorithm)
- Sprites pixel-art definidos como matrices de caracteres, con vistas direccionales (frente / perfil / espalda) y animación procedural de piernas
- Mundo infinito por chunks con hashing determinista (sin seed almacenada)
- Loop a 60 FPS con `requestAnimationFrame` y delta time

---

*Proyecto fan sin fines de lucro. God of War es propiedad de Santa Monica Studio / Sony Interactive Entertainment.*


## License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.
