# Mi Semana

App para organizar la semana: clases, entrenos y tareas. Se instala en el iPhone y en el ordenador, funciona sin conexión y se sincroniza entre dispositivos con Supabase.

## Instalar

- **iPhone (Safari):** abre la web, toca Compartir → *Añadir a pantalla de inicio*.
- **Ordenador (Chrome o Edge):** abre la web y pulsa el icono de instalar en la barra de direcciones (o menú → *Instalar Mi Semana*).
- **Mac con Safari:** Archivo → *Añadir al Dock*.

## Sincronización (Supabase, gratis)

1. Crea un proyecto en https://supabase.com.
2. En **SQL Editor**, ejecuta:

```sql
create table if not exists public.items (
  user_id uuid not null default auth.uid() references auth.users on delete cascade,
  id text not null,
  data jsonb,
  deleted boolean not null default false,
  updated_at timestamptz not null default now(),
  primary key (user_id, id)
);
alter table public.items enable row level security;
create policy "cada uno ve lo suyo" on public.items
  for all using (auth.uid() = user_id) with check (auth.uid() = user_id);
create index if not exists items_updated on public.items (user_id, updated_at);
```

3. En **Authentication → Sign In / Providers → Email**, desactiva *Confirm email* (así entras directamente desde la app).
4. Copia la *Project URL* y la clave *anon public* (Project Settings → API) en `config.js`.

Sin `config.js` rellenado, la app funciona igual pero guarda los datos solo en ese dispositivo.

## Archivos

- `index.html` — la app entera (HTML, CSS y JS).
- `config.js` — datos públicos de Supabase.
- `manifest.webmanifest`, `icons/` — instalación e icono.
- `sw.js` — funciona sin conexión.
