# Conteo de stock

Página web para contar stock y cargar roturas/bajas con el celular, leyendo códigos de barras con la cámara.

- **Escritorio:** importar el Excel del sistema, armar la lista de productos a contar (filtros por rubro, stock bajo, orden por stock), ver resultados (contado contra sistema y diferencia) y ver las roturas/bajas cargadas.
- **Celular:** escanear el código de barras, ver descripción, código interno y stock del sistema, y cargar lo contado o las unidades rotas.

Los datos se guardan en **Firebase Firestore** y la página se publica gratis en **GitHub Pages**. Para entrar se usa una cuenta de Google, y solo las cuentas autorizadas en las reglas pueden leer o guardar.

## Puesta en marcha (una sola vez)

### 1. Crear el proyecto de Firebase
1. Entrá a <https://console.firebase.google.com> y creá un proyecto (por ejemplo `conteo-stock`). Google Analytics no hace falta.
2. **Firestore Database** > *Crear base de datos* > modo producción > ubicación `southamerica-east1` (San Pablo).
3. **Authentication** > *Comenzar* > *Método de acceso* > activá **Google**.
4. **Authentication** > *Configuración* > *Dominios autorizados* > agregá `fabriziovischi10-a11y.github.io` (el dominio de GitHub Pages).

La página publicada: <https://fabriziovischi10-a11y.github.io/conteo-stock/>

### 2. Conectar la página con Firebase
1. En Firebase: ⚙️ *Configuración del proyecto* > *Tus apps* > ícono `</>` (Web) > registrá la app.
2. Copiá los valores de `firebaseConfig` en el archivo `firebase-config.js` de este repositorio.

### 3. Autorizar las cuentas
1. Abrí `firestore.rules`, reemplazá los dos mails de ejemplo por tu mail y el de Hugo (cuentas de Google).
2. En Firebase: **Firestore Database** > *Reglas*, pegá el contenido y tocá **Publicar**.

Para sumar a otra persona, agregás su mail a la lista y volvés a publicar las reglas.

### 4. Cargar el stock
Entrá a la página desde la computadora, pestaña **Stock del sistema**, elegí el `.xls` exportado del sistema y tocá **Importar**. Se usan las columnas `codigo`, `codigof` (código de barras), `descripcio`, `stock`, `ptocri`, `min`, `nombre_a`, `nombre_b` y `equi`.

## Uso diario

1. **Escritorio > Armar lista:** filtrá (rubro, stock bajo, búsqueda), ordená (por ejemplo stock de mayor a menor), marcá productos y tocá **Agregar a la lista de conteo**.
   - **Carga masiva:** en el recuadro "Cargar lista desde Excel" subí un .xls/.xlsx/.csv con las columnas *código interno, código de barras, descripción y stock actual* y tocá **Cargar a la lista**. Cada fila entra a la lista con ese stock (aunque el producto no esté en el stock del sistema). Si el producto ya estaba en la lista sin contar, se actualiza; si ya se contó, no se toca. Con **Descargar planilla modelo** bajás un ejemplo.
2. **Celular > Conteo:** Hugo toca **Escanear código de barras**, apunta al código y carga lo que contó en dos casilleros: **Apto** y **No apto**. El total (apto + no apto) se compara con el sistema y la app muestra si falta, sobra o coincide. El **no apto se carga solo en Roturas / bajas** (motivo "No apto (conteo)"); si después se corrige o se borra el conteo, esa rotura se ajusta sola.
3. **Celular > Roturas / bajas:** Hugo escanea cada producto roto, elige el motivo, carga las unidades y guarda. Puede cargar el mismo producto varias veces.
4. **Celular > Suelto:** igual que roturas, pero para mercadería que apareció y no figuraba en el stock (por ejemplo, con stock 0). Las unidades se suman.
5. **Escritorio > Resultados**, **Roturas / bajas** y **Suelto:** se actualizan en vivo. Se pueden copiar para Excel o descargar en CSV. Con "Sumar por producto" se ve el stock del sistema, las unidades cargadas y el stock resultante.

### Negativos
Lista aparte para los productos que figuran con stock negativo.
- **Escritorio > Negativos:** tocá **Agregar todos los negativos** (toma los del stock del sistema) o subí un Excel con código interno, código de barras, descripción y stock actual. Ahí se ve el stock del sistema, lo contado, la diferencia, **qué pasó** y quién lo contó. Botón **Editar** para corregir.
- **Celular > Negativos:** escaneá o elegí el producto, cargá cuánto hay y elegí qué pasó (venta sin facturar, error de carga, no ingresó la compra, rotura no registrada u otro), con una nota opcional.

### Varios usuarios en el celular
Todos los que entran en la vista **Celular** (por ejemplo Hugo y Sergio) comparten la misma lista de conteo y cargan roturas y sueltos por igual. Cada carga guarda quién la hizo (el primer nombre de su cuenta de Google): se ve en el historial del celular, en la columna **Contó** de Resultados y en la columna **Cargó** de Roturas / Suelto, donde además se puede filtrar por persona.

### Historial
En el celular, debajo de Roturas / bajas y de Suelto, está el **Historial** con el filtro Hoy / 7 días / 30 días / Todo. En el escritorio, esas pestañas muestran por defecto **todo el historial**, con filtros por período y por quién cargó.

### Cerrar el día
En **Escritorio > Resultados**, cada producto contado con diferencia tiene la columna **Motivo del ajuste** (un motivo de la lista y un detalle opcional), que se guarda apenas se elige. El filtro **Sin motivo de ajuste** muestra los que faltan.
Al terminar, tocá **Cerrar día**: si falta algún motivo, la app te lo marca; si está todo, guarda la planilla completa (stock, contado, apto/no apto, diferencia, motivo y detalle, quién contó). Con **Cerrar día** se quitan de la lista los productos ya contados y **los pendientes quedan para contar**, marcados "Pendiente del dd/mm" (filtro **De días anteriores**). Si después cargás una lista nueva, los pendientes se suman a ella. También podés **cerrar y mantener toda la lista**. Las planillas cerradas quedan en **Cierres anteriores**, para copiar a Excel o descargar en CSV.

### Corregir errores
- **Roturas y sueltos:** botón **Editar** en cada carga (en el celular, en "Cargadas hoy"; en el escritorio, en la tabla). Se puede cambiar la cantidad, el motivo, la nota y el producto: en el celular, mientras se corrige, escaneá o buscá el producto correcto; en el escritorio, escribí su código. **Borrar** pide un segundo toque.
- **Conteos:** en el celular, tocá el producto en "Ya contados" (o escanealo de nuevo), cambiá la cantidad y tocá **Guardar corrección**, o **Borrar este conteo** si era otro producto. En el escritorio, botón **Editar** en Resultados.

Si la cámara en vivo no lee un código, está el botón **Sacar foto del código**, o se puede escribir el código o la descripción.
