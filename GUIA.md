# Cervalina Comandas: instalación con impresión directa

Con esta versión, la app se abre en Chrome en tu tablet Android y **la cuenta se imprime sola al tocar "Cobrar"**. Las tablets siguen sincronizadas entre sí.

Necesitas una cuenta de Google y unos 20 minutos. Los pasos 1 a 4 se hacen una sola vez, de preferencia desde una computadora.

---

## 1. Crear el proyecto en Firebase

1. Entra a **console.firebase.google.com** con tu cuenta de Google.
2. Toca **Crear un proyecto** y ponle de nombre `cervalina-comandas`. Puedes desactivar Google Analytics porque no se necesita.
3. El plan gratuito (**Spark**) alcanza de sobra para el negocio y no pide tarjeta.

## 2. Activar la base de datos (Firestore)

1. En el menú izquierdo: **Compilación → Firestore Database → Crear base de datos**.
2. Ubicación: elige la más cercana a Oaxaca. Si aparece **northamerica-south1 (México)**, elígela; si no, **us-central1**.
3. Elige **Comenzar en modo de producción**.
4. Ve a la pestaña **Reglas**, borra lo que hay, pega esto y toca **Publicar**:

```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /{document=**} {
      allow read, write: if request.auth != null;
    }
  }
}
```

Con estas reglas, solo las cuentas que tú crees pueden ver y cambiar las ventas.

## 3. Crear las cuentas de acceso

1. Menú izquierdo: **Compilación → Authentication → Comenzar**.
2. En **Método de acceso**, activa **Correo electrónico/contraseña**.
3. En **Usuarios → Agregar usuario**, crea la cuenta del negocio. Por ejemplo, `caja@tunegocio.com` con una contraseña segura. Puedes crear una sola cuenta para todas las tablets o una por persona.

## 4. Registrar la app web y copiar la configuración

1. Toca el engrane ⚙ junto a "Descripción general del proyecto" y entra a **Configuración del proyecto**.
2. Abajo, en **Tus apps**, toca el ícono **</>** (Web), ponle de nombre `comandas` y toca **Registrar app**. No marques Firebase Hosting todavía.
3. Aparece un bloque con `const firebaseConfig = { apiKey: "...", ... }`. **Copia esos datos.**
4. Pégalos en el archivo `public/config.js` o mándamelos y yo los pongo. Estos datos **no son secretos**: solo identifican tu proyecto. La seguridad la dan las contraseñas y las reglas del paso 2.

## 5. Publicar la app (elige una opción)

### Opción A: Netlify Drop (sin instalar nada, recomendada)
1. Crea una cuenta gratuita en **netlify.com**.
2. Entra a **app.netlify.com/drop** y **arrastra la carpeta `public`** a la página.
3. Netlify te da una dirección como `https://cervalina-123.netlify.app`. En **Site configuration → Change site name** puedes cambiarla, por ejemplo a `cervalina-comandas`.
4. En Firebase: **Authentication → Configuración → Dominios autorizados → Agregar dominio**, y pega tu dirección de Netlify sin `https://`.

Para actualizar la app más adelante, arrastra de nuevo la carpeta en **Deploys** dentro de tu sitio de Netlify.

### Opción B: Firebase Hosting (necesita una computadora con Node.js)
```bash
npm install -g firebase-tools
firebase login
firebase use --add        # elige tu proyecto
firebase deploy
```
Te da la dirección `https://TU-PROYECTO.web.app`.

## 6. Preparar la tablet Android

1. Abre la dirección en **Chrome**. Tiene que ser Chrome: otros navegadores no permiten Bluetooth.
2. Menú ⋮ → **Agregar a la pantalla principal**. Queda como una app.
3. Inicia sesión con la cuenta del paso 3. La tablet recuerda la sesión.
4. La primera vez, la carta está vacía: toca **Cargar carta e insumos de Cervalina** (dos toques para confirmar).

## 7. Conectar la impresora (una vez por tablet)

1. Enciende la impresora y activa el Bluetooth de la tablet. **No hace falta vincularla en Ajustes de Android.**
2. En la app, toca **Conectar impresora** (arriba a la derecha) → **Buscar impresora**, elígela en la lista y toca **Vincular**.
3. Toca **Imprimir prueba**. Deben salir los acentos (á é í ó ú ñ ¿ ¡) y una línea de 32 números que ocupe todo el ancho.

Desde ahí, al tocar **Cobrar**, la cuenta se imprime sola. En el mismo menú puedes activar que también se imprima el **ticket de pago**.

Al cerrar y abrir la app, intenta reconectar sola. Si el botón dice "Conectar impresora", tócalo y elige la impresora otra vez.

### Si la impresora no aparece en la lista
Algunas impresoras solo tienen **Bluetooth clásico**, y Chrome no las ve. En ese caso:
1. Instala **RawBT** desde Play Store, ábrela, elige tu impresora y actívala como **servicio de impresión** de Android.
2. En la app, usa el botón **Menú de impresión** del ticket. Se abre el diálogo de Android; elige RawBT y toca imprimir. Son dos toques más.

---

## Bueno saber
- **Sin internet:** la app sigue tomando pedidos y cobrando. Los cambios se guardan en la tablet y se envían solos al volver la conexión. Para **abrir** la app la primera vez sí se necesita internet.
- **Varias tablets:** todas ven lo mismo al instante (mesero, cocina, caja).
- **Costo:** el plan gratuito de Firebase permite unas 50,000 lecturas y 20,000 escrituras al día, muy por encima de lo que usa un negocio de este tamaño.
- **Datos de prueba:** las existencias de insumos y las recetas son ejemplos. Ajústalas en **Insumos → Conteo** y en **Menú**.
