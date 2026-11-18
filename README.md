# Solo visibles — plugin para ONLYOFFICE

Plugin gratuito para **ONLYOFFICE Desktop Editors (Hojas de cálculo)** que aplica cambios **solo a las filas visibles** de una selección, es decir, a las que quedan después de aplicar un filtro.

ONLYOFFICE no tiene el equivalente a *Ir a → Especial → Solo celdas visibles* (`Alt+;`) de Excel, así que cualquier cambio masivo sobre un rango filtrado afecta también a las filas ocultas. Este plugin llena ese vacío.

## Qué hace

| Función | Descripción |
|---|---|
| **Escribir valor** | Escribe el mismo valor en todas las celdas visibles de la selección. |
| **Aplicar formato** | Negrita, color de relleno, color de letra y formato numérico, solo en las filas visibles. |
| **Eliminar filas visibles** | Borra las filas completas que están visibles (las filtradas), sin tocar las ocultas. Pide confirmación con un segundo clic. |
| **Probar detección** | Informa cuántas filas visibles y ocultas detecta en la selección. No modifica nada. |

## Instalación

1. Descarga `solo-visibles.plugin` desde la sección **Releases** de este repositorio.
2. Abre ONLYOFFICE Desktop Editors → pestaña **Plugins** → instala un plugin local y elige el archivo `.plugin`. El nombre exacto de la opción cambia según la versión.
3. Abre una hoja de cálculo. El panel **Solo visibles** aparece en la barra lateral.

> El plugin carga `plugins.js` desde `onlyoffice.github.io`, por lo que necesita conexión a internet.

### Instalación desde el código

Comprime el contenido de esta carpeta (`config.json`, `index.html`, `icon.png`) en un `.zip` y cámbiale la extensión a `.plugin`:

```bash
zip -r solo-visibles.plugin config.json index.html icon.png
```

## Uso

1. Aplica el filtro a tu tabla.
2. Selecciona el rango sobre el que quieres actuar (por ejemplo, toda la columna de datos). Si seleccionas una columna entera, se usa el rango usado de la hoja.
3. Abre el panel **Solo visibles** y pulsa la acción que necesites.

Para eliminar filas:
1. Pulsa **Eliminar filas visibles**. El botón pide confirmación.
2. Pulsa de nuevo en los 4 segundos siguientes para confirmar.

## Advertencias

- **Prueba siempre en una copia del archivo**, sobre todo antes de eliminar filas. Después de ejecutar un plugin, el deshacer (`Ctrl+Z`) puede no funcionar.
- Usa **Probar detección** primero para comprobar que cuenta bien las filas visibles y ocultas en tu versión.
- El plugin usa la API pública de ONLYOFFICE, que no permite crear una selección real de "solo visibles". Por eso aplica el cambio fila por fila, en lugar de seleccionar las celdas.
- Está probado con una versión concreta de ONLYOFFICE Desktop Editors. Algunos métodos de la API pueden comportarse distinto en otras versiones. Si algo falla, abre un *issue* con el mensaje que muestra el panel.

## Cómo funciona

1. Lee la dirección de la selección y la convierte en un rango de filas y columnas.
2. Recorre las filas y descarta las que están ocultas (`GetHidden()`).
3. Aplica la acción elegida a cada fila visible.
4. Para eliminar, recorre las filas **de abajo hacia arriba**, para no descuadrar los índices al borrar. Prueba varios métodos de borrado (`Delete("up")` sobre la fila completa) hasta encontrar el que funciona en la versión instalada.

## Estructura

```
solo-visibles/
├── config.json   # Metadatos y configuración del plugin
├── index.html    # Interfaz y lógica
└── icon.png      # Icono
```

Documentación de la API: [api.onlyoffice.com](https://api.onlyoffice.com/docs/plugin-and-macros/get-started/overview/)

## Contribuir

Los *issues* y *pull requests* son bienvenidos. Ideas pendientes:
- Más acciones (copiar valores visibles, ordenar, formatos de fecha).
- Soporte para selecciones de varias áreas.
- Funcionamiento sin conexión (incluir `plugins.js` localmente).

## Licencia

[MIT](LICENSE): libre para usar, copiar, modificar y distribuir, incluso con fines comerciales, manteniendo el aviso de copyright.

---

Proyecto independiente, no afiliado ni respaldado por Ascensio System SIA / ONLYOFFICE.
