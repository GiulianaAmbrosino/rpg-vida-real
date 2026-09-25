# RPG Vida Real

Esta es la versión del proyecto pensada para Webflow Cloud.

La idea es la misma: hábitos como misiones, XP, niveles, rachas y una bitácora. El cambio grande es de infraestructura: en vez de Express + SQLite local, ahora uso **Vite + Worker + D1** para que el backend pueda correr en el runtime de Webflow Cloud.

## Para correrlo local

Necesitás Node 22 o superior.

```bash
npm install
npm run db:local
npm run dev
```

Para probar el Worker y el D1 localmente de verdad:

```bash
npm install
npm run db:local
npx wrangler dev
```

## Para Webflow Cloud

1. Subí esta carpeta a tu repo de GitHub.
2. En Webflow Cloud conectá ese repo.
3. Elegí la rama que quieras usar.
4. El proyecto ya tiene `vite` en `package.json`, así que Webflow Cloud lo detecta como Vite.
5. `wrangler.json` declara la base D1 con el binding `DB`.
6. No hace falta agregar una `SESSION_SECRET`: las sesiones están guardadas en D1 y el token va en una cookie HttpOnly.

### Importante con el mount path

Los llamados del frontend usan `import.meta.env.BASE_URL`, así que no deberían romperse si Webflow monta la app, por ejemplo, en `/app`.

## Archivos importantes

- `src/main.js`: interfaz y lógica del RPG.
- `src/styles.css`: estilos.
- `src/worker.ts`: API, autenticación y reglas del juego.
- `migrations/0001_init.sql`: tablas de usuarios y sesiones.
- `wrangler.json`: binding de D1.

## Qué quedó sin Express

No está `server.js` a propósito. El backend ahora vive en el Worker y usa D1. Tampoco hace falta `better-sqlite3` ni `connect-sqlite3`.

## Nota sobre passwords

Las contraseñas se guardan como un hash PBKDF2 con salt aleatorio. La app nunca guarda la contraseña en texto plano.
