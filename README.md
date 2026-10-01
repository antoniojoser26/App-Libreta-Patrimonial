# App Libreta Patrimonial

App de finanzas personales que funciona como un **artifact de Claude** (claude.ai). Lleva tus gastos, tu cartera de fondos de MyInvestor, tus compras a plazos y tus objetivos, y trae un **gestor patrimonial** con el que puedes hablar sobre tu dinero.

Esta versión viene **vacía**: no contiene ningún dato. Todo lo que metas se guarda en la base de datos privada del artifact y solo lo ve la persona que lo publica.

---

## Qué hace

| Sección | Para qué sirve |
|---|---|
| **Inicio** | Patrimonio total con el reparto efectivo / inversión, saldo de la cuenta, gastos del mes, colchón líquido (meses de gasto que cubre la cuenta), dinero reservado (fondo monetario), cuota de compras a plazos, gastos por categoría, ingresos vs. gastos, combustible, evolución del patrimonio y objetivos. |
| **Movimientos** | Lista de gastos e ingresos con buscador, filtros y categoría editable. Se rellena con *Sincronizar*, importando extractos o a mano. |
| **Cartera** | Fondos con participaciones, valor liquidativo, ganancia y peso. El botón *Actualizar con MyInvestor* revaloriza los fondos con el último valor liquidativo. |
| **Fraccionados** | Compras a plazos: precio, cuota, interés (TIN) y fecha. Calcula nº de cuotas, coste total, intereses, calendario de pagos y fecha de fin. Botones − / + para marcar pagos. |
| **Gestor** | Chat con Claude que responde usando tus datos reales (cartera, movimientos, plazos, objetivos) y puede consultar el catálogo de fondos de MyInvestor. Orientativo, no es asesoramiento financiero regulado. |
| **Perfil** | Tus datos (edad, nómina, vivienda, riesgo…), objetivos de ahorro y la sección **Tus datos** para exportarlo todo a Excel, TXT o Google Drive. |

---

## Requisitos

- Una cuenta de **claude.ai** con artifacts.
- **Conectores** en *claude.ai › Ajustes › Conectores* (todos opcionales; la app funciona sin ellos, solo se desactiva lo que los necesita):
  - **Gmail**: leer los avisos de movimientos que envía MyInvestor.
  - **MyInvestor**: valor liquidativo de tus fondos y catálogo para el gestor.
  - **Google Drive**: guardar copias de tus datos.
- **App de escritorio de Claude** (opcional): necesaria solo para que *Sincronizar* lea los movimientos directamente de la web de MyInvestor con su navegador integrado.

---

## Instalación

1. Descarga `index.html` y `capabilities.json` de este repositorio.
2. Abre una conversación en claude.ai, adjunta los dos archivos y pide:

   > Publica `index.html` como artifact con las capabilities de `capabilities.json`.

3. Abre el artifact. La primera vez te pedirá permiso para usar Claude, Gmail, MyInvestor, Drive o descargas según lo que uses.

Las reglas de `capabilities.json` hacen que **solo el propietario** del artifact pueda leer y escribir los datos, aunque compartas el enlace.

---

## Primeros pasos

1. **Perfil**: rellena edad, nómina neta, pagas, vivienda, aportación mensual y tolerancia al riesgo. El gestor usa estos datos en cada respuesta.
2. **Cartera**: en *Movimientos › Importar*, elige «Posición de cartera» y sube una **captura de pantalla** de tus fondos en MyInvestor. Claude la lee y te enseña el resultado antes de guardar. También puedes usar *Cartera › Editar valores*.
3. **Movimientos**: pulsa **Sincronizar** o importa el Excel de *Resumen de movimientos* que descargas de MyInvestor.
4. **Objetivos**: en *Perfil*, añade metas (colchón, entrada de piso…) y con qué se miden.
5. **Compras a plazos**: en *Fraccionados › Añadir compra a plazos*.

---

## Cómo entran los movimientos

| Vía | Qué trae | Dónde funciona |
|---|---|---|
| **Sincronizar → Gmail** | Recibos, transferencias, nóminas y operaciones con fondos que MyInvestor notifica por correo. | Web, móvil y escritorio |
| **Sincronizar → web de MyInvestor** | Todos los movimientos del mes, también pagos con tarjeta y el saldo. Claude navega en modo solo lectura. | Solo app de escritorio, con la sesión de MyInvestor iniciada en su navegador integrado |
| **Importar** | Excel/CSV del banco, texto pegado o captura de pantalla. Marca posibles duplicados. | Todas |
| **Añadir** | Un movimiento a mano. | Todas |

**Truco para los pagos con el móvil:** MyInvestor no envía un correo por cada pago con tarjeta. Con una app de automatización de Android (por ejemplo MacroDroid) puedes reenviarte por email cada notificación de Google Wallet o de la app de MyInvestor con el asunto `[LIBRETA]`. La búsqueda de Gmail por defecto ya incluye ese asunto.

La búsqueda de Gmail se cambia en *Perfil › Búsqueda de Gmail para sincronizar*.

---

## Categorías y cálculos

- **Inversión** y **Traspasos** no cuentan como ingreso ni gasto, para que aportar a fondos no infle tus gastos.
- **Colchón líquido** = saldo de la cuenta ÷ gasto medio de los últimos meses completos.
- **Dinero reservado** = lo que tienes en fondos de tipo *Monetario*.
- **Combustible**: las gasolineras se clasifican solas al importar o sincronizar.
- **Fraccionados**: amortización francesa con el interés anual (TIN) que indiques. Si das el nº de cuotas del contrato, el coste total es nº × cuota y los intereses salen de la diferencia con el precio.

---

## Exportar tus datos

En *Perfil › Tus datos*:

- **Descargar Excel**: hojas Resumen, Movimientos, Cartera, Plazos, Evolución, Objetivos, Perfil y Gestor.
- **Descargar TXT**: lo mismo en texto plano.
- **Guardar copia en Google Drive**: sube el Excel y el TXT con la fecha. Por defecto van a la raíz de tu Drive; puedes pedirle a Claude que guarde en la base de datos del artifact el documento `state/drive` con el id de tu carpeta (`{"exportaciones": "<id de carpeta>"}`) para que vayan siempre ahí.

---

## Automatizaciones opcionales (tareas programadas)

Desde una conversación de Claude puedes crear tareas programadas que trabajen con la base de datos del artifact (pásales la URL de tu artifact):

- **Diaria**: leer Gmail, crear los movimientos nuevos y revalorizar la cartera con MyInvestor.
- **Mensual (día 1)**: generar el cierre del mes anterior en Excel y TXT y guardarlo en Google Drive.

---

## Privacidad

- Los datos viven en la base de datos del artifact, con lectura y escritura solo para el propietario.
- El código no contiene datos personales ni claves.
- Las llamadas a Claude (gestor, importar, sincronizar) gastan el uso de **tu** cuenta de Claude.
- La lectura de la web del banco es de solo lectura: la app bloquea escribir en formularios, transferir, firmar o cerrar sesión.

---

## Limitaciones conocidas

- MyInvestor no ofrece una API pública de cuentas; el conector de MyInvestor solo da datos de catálogo (fondos, valor liquidativo), no movimientos.
- La lectura directa de la web de MyInvestor depende de su diseño y de que la sesión siga iniciada; si pide SMS o confirmación en el móvil, la app te avisa.
- El conector de Google Drive crea archivos nuevos; no sobrescribe los existentes.

---

## Estructura

```
index.html          La app completa (HTML, CSS y JS en un archivo)
capabilities.json   Permisos del artifact (base de datos, Claude, conectores, descargas)
README.md           Esta guía
```

Licencia: MIT.
