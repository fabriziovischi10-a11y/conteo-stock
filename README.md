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

1. **Escritorio > Armar lista:** filtrá (rubro, stock bajo, búsqueda), ordená (por ejemplo stock de mayor a menor), marcá productos y tocá **Agregar a la lista de Hugo**.
2. **Celular > Conteo:** Hugo toca **Escanear código de barras**, apunta al código, carga lo que contó y guarda. La app le muestra si falta, sobra o coincide.
3. **Celular > Roturas / bajas:** Hugo escanea cada producto roto, elige el motivo, carga las unidades y guarda. Puede cargar el mismo producto varias veces.
4. **Escritorio > Resultados** y **Roturas / bajas:** se actualizan en vivo. Se pueden copiar para Excel o descargar en CSV.

Si la cámara en vivo no lee un código, está el botón **Sacar foto del código**, o se puede escribir el código o la descripción.
