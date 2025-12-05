# Configuración de Variables de Entorno en Vercel

## Connection String de Neon

Para configurar la base de datos en Vercel, usa este connection string:

```
postgresql://neondb_owner:npg_V14aNIEDQjSp@ep-polished-bird-ah2ud3n6-pooler.c-3.us-east-1.aws.neon.tech/neondb?sslmode=require
```

## Pasos para Configurar en Vercel

### 1. Ir a Configuración del Proyecto

1. Ve a tu proyecto en [Vercel Dashboard](https://vercel.com/dashboard)
2. Click en **Settings** (Configuración)
3. Click en **Environment Variables** (Variables de Entorno)

### 2. Agregar Variables de Entorno

Agrega las siguientes variables:

#### Variable 1: DATABASE_URL
- **Key**: `DATABASE_URL`
- **Value**: 
  ```
  postgresql://neondb_owner:npg_V14aNIEDQjSp@ep-polished-bird-ah2ud3n6-pooler.c-3.us-east-1.aws.neon.tech/neondb?sslmode=require
  ```
- **Environments**: ✅ Production, ✅ Preview, ✅ Development

#### Variable 2: NEXTAUTH_SECRET
- **Key**: `NEXTAUTH_SECRET`
- **Value**: Genera una clave secreta (ver abajo)
- **Environments**: ✅ Production, ✅ Preview, ✅ Development

**Para generar NEXTAUTH_SECRET:**
```powershell
# En PowerShell
[Convert]::ToBase64String((1..32 | ForEach-Object { Get-Random -Maximum 256 }))
```

O usa un generador online: https://generate-secret.vercel.app/32

#### Variable 3: NODE_ENV
- **Key**: `NODE_ENV`
- **Value**: `production`
- **Environments**: ✅ Production solamente

### 3. Guardar y Redesplegar

1. Click en **Save** para guardar las variables
2. Ve a **Deployments**
3. Click en los tres puntos (...) del último deployment
4. Selecciona **Redeploy**

O simplemente haz un nuevo push a GitHub y Vercel desplegará automáticamente.

## Verificar que Funciona

Después del deploy:

1. Visita tu URL de Vercel: `https://tu-proyecto.vercel.app`
2. Verifica que la aplicación cargue correctamente
3. Prueba hacer login con:
   - Admin: `admin@maranatha.com` / `admin123`
   - Organizador: `organizador@maranatha.com` / `org2024`

## Aplicar Schema a la Base de Datos

Después del primer deploy exitoso, necesitas aplicar el schema de Prisma a Neon:

### Opción 1: Desde tu máquina local (Recomendado)

1. Asegúrate de tener `.env.local` con `DATABASE_URL`
2. Ejecuta:
   ```bash
   npm run db:push
   npm run db:seed
   ```

### Opción 2: Usar Vercel CLI

```bash
# Instalar Vercel CLI
npm i -g vercel

# Conectar con tu proyecto
vercel link

# Descargar variables de entorno
vercel env pull .env.local

# Aplicar schema
npm run db:push
npm run db:seed
```

## Troubleshooting

### Error: "Can't reach database server"

- Verifica que el connection string esté correcto en Vercel
- Asegúrate de usar la connection string "Pooled" (no "Direct")
- Verifica que Neon permita conexiones desde Vercel (debería ser automático)

### Error: "Environment variable not found"

- Verifica que todas las variables estén guardadas en Vercel
- Asegúrate de que estén marcadas para el entorno correcto (Production/Preview/Development)
- Haz un nuevo deploy después de agregar variables

### La aplicación funciona pero no hay datos

- Ejecuta `npm run db:push` para aplicar el schema
- Ejecuta `npm run db:seed` para poblar datos iniciales

