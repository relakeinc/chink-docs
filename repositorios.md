# Repositorios

El proyecto está dividido en dos repositorios:

* **Código de la aplicación**: `github.com/relakeinc/chink`. Es privado porque tiene la configuración del servidor. Ahí están el frontend, el backend y la base de datos.
* **Documentación**: este repositorio, que es público y está conectado a GitBook. Cada vez que se sube un cambio a la rama principal, GitBook actualiza la documentación.

## Qué se necesita para correr el proyecto

* Node.js 22 o más nuevo.
* pnpm (se activa con `corepack enable`).
* PostgreSQL 17 con una base de datos llamada `chink_saas`.

## Cómo correrlo en local

```bash
git clone https://github.com/relakeinc/chink.git
cd chink
cp .env.example .env
pnpm install
pnpm --filter @chink-saas/db migrate:deploy
pnpm db:seed
pnpm dev
```

Ojo: en la versión actual el seed tiene un error de orden que lo hace fallar (está en el reporte de defectos). Mientras se corrige en el repositorio, se usa la versión corregida del archivo `packages/db/prisma/seed.ts`.

El seed crea datos de ejemplo y seis usuarios con distintos roles. Todos tienen la contraseña `chink2026`, por ejemplo `lucia@chink.es` (Administradora), `ana@chink.es` (Operador) y `marta@chink.es` (Consulta).

## Cómo correr las pruebas

Las pruebas unitarias están hechas con Vitest y están en `apps/api/test/matriz.test.ts`. Cada prueba tiene el mismo código del caso en la matriz de pruebas (CP001, CP002, etc.).

```bash
pnpm db:seed
cd apps/api
npx dotenv -e ../../.env -- npx vitest run --reporter=verbose
```

## Ramas

Trabajamos sobre la rama `main`. Los cambios grandes se hacen en una rama aparte y después se unen a `main` cuando ya están probados.
