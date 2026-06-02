# claude-done

La hora a la que Claude terminó, marcada al final de cada turno.

[English](README.md) · [한국어](README.ko.md) · [日本語](README.ja.md) · [中文](README.zh.md)

claude-done es un Stop hook para Claude Code. Cada vez que Claude termina un turno, Claude Code puede ejecutar un comando de shell. Este hook imprime una línea corta con la hora actual, en tu idioma y con tu formato. Algo así:

```
Hecho: 03 jun 2026, 03:39
```

## Para qué sirve

Con una sola sesión no sirve de mucho: la estás mirando, ya ves cuándo acaba. Donde sí gana es cuando tienes varias a la vez en distintas tabs del terminal. Te levantas un momento, vuelves y por el transcript no hay forma de saber cuándo terminó cada una. Este hook marca la hora de cada turno que termina, así que de un vistazo a la tab ves si acaba de terminar o si lleva ya un rato parado. Es una línea más por turno, nada que mantener.

## Cómo se ve

| Región | Línea |
|--------|-------|
| English | `Done: 03 Jun 2026, 03:39` |
| 한국어 | `완료: 2026년 06월 03일 03:39` |
| 日本語 | `完了: 2026年06月03日 03:39` |
| Español | `Hecho: 03 jun 2026, 03:39` |
| Deutsch | `Fertig: 03.06.2026, 03:39` |

La palabra y el formato dependen de tu locale. Claude te sugiere uno a partir de la configuración de tu sistema, y si no te convence, lo cambias.

## Instalación

Pega esto en Claude Code:

```
Instálame el stop hook de github.com/bidual/claude-done.
Lee su INSTALL.md y sigue los pasos.
```

Claude detecta tu idioma y tu formato de hora, escribe el hook en `~/.claude/settings.json` y comprueba que todo esté bien. A partir del siguiente turno ya verás cómo marca la hora. [INSTALL.md](INSTALL.md) es un prompt, no un binario, así que puedes leerlo entero.

## Cómo funciona

El hook es un único comando:

```
date +'{ "systemMessage": "Hecho: %d %b %Y, %H:%M" }'
```

Claude Code lanza un Stop hook al acabar cada turno, y ese hook es este comando. `date` rellena la hora e imprime un pequeño objeto JSON; Claude Code lee el campo `systemMessage` y lo muestra. No queda nada ejecutándose en segundo plano. El formato lo decides tú, con los códigos normales de `date` (mira `man date`): por ejemplo `%S` para los segundos o `%I:%M %p` para las 12 horas.

## Cambiarlo o quitarlo

Escribe `/hooks` dentro de Claude Code para verlo, editarlo o desactivarlo. También puedes editar a mano `hooks.Stop` en `~/.claude/settings.json`. Para quitarlo, borra el bloque `Stop` o pídeselo a Claude.

## Licencia

[MIT](LICENSE)
