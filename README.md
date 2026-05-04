# Cuenta de Cobro SENA — Generador de informes de ejecución contractual

Aplicación web para automatizar la generación mensual de cuentas de cobro e informes de ejecución contractual del SENA, reduciendo el proceso manual de recolección de evidencias, redacción de reportes y organización de archivos.

## Problema que resuelve

Cada mes, los contratistas del SENA deben presentar una cuenta de cobro que incluye un informe principal de ejecución contractual (documento GC), documentos de evidencia por cada obligación con capturas de pantalla y descripciones técnicas, y una estructura de carpetas organizada por obligación. Este proceso es repetitivo, propenso a errores y consume varias horas mensuales en copiar formatos, recopilar commits, redactar descripciones y organizar archivos.

## Qué hace la aplicación

- **Extrae commits de GitHub** de uno o varios repositorios de la organización, filtrando por autor y rango de fechas, recorriendo todas las ramas y deduplicando por SHA.
- **Clasifica evidencias por obligación** mediante un tablero interactivo tipo kanban donde se asigna cada commit a la obligación contractual correspondiente.
- **Asiste en la redacción** con inteligencia artificial (Claude, Anthropic) para corregir ortografía, mejorar la formalidad del texto, mantener voz pasiva impersonal y generar descripciones técnicas a partir de mensajes de commit.
- **Genera documentos Word (.docx)** con el formato institucional del SENA: el informe principal con la tabla de obligaciones y los documentos de evidencia individuales con capturas incrustadas.
- **Empaqueta todo en un .zip** con la estructura de carpetas requerida (`Evidencias/Obligacion_1/` ... `Obligacion_12/`), listo para entregar.
- **Persiste la configuración del contrato** (datos del contratista, supervisora, obligaciones, valores de pago) para no tener que ingresarla cada mes.

## Tech stack

- **Next.js 15** (App Router) — Framework full-stack con API Routes como backend
- **React 19** — Interfaz de usuario
- **TypeScript** — Tipado estático
- **Prisma + SQLite** — ORM y base de datos local
- **Tailwind CSS** — Estilos
- **Octokit** (`@octokit/rest`) — Cliente oficial de GitHub API
- **Anthropic SDK** (`@anthropic-ai/sdk`) — Integración con Claude para asistencia de redacción
- **docx** — Generación de documentos Word
- **archiver** — Creación de archivos .zip

## Estructura del proyecto

```
src/
├── app/
│   ├── api/
│   │   ├── github/          # Endpoints para repos, ramas y commits
│   │   ├── reports/         # CRUD de informes mensuales
│   │   ├── evidence/        # Upload de capturas de pantalla
│   │   ├── ai/              # Corrección y mejora de texto con Claude
│   │   └── generate/        # Generación de .docx y .zip
│   ├── dashboard/           # Vista principal con historial de meses
│   └── reports/[id]/        # Editor de informe mensual
├── domain/
│   ├── github/              # GitHubClient, CommitCollector, CommitClassifier
│   ├── documents/           # GCDocumentBuilder, ObligationDocBuilder
│   ├── ai/                  # TextImprover (corrección con Claude)
│   └── packaging/           # ZipPackager
├── components/              # Componentes React reutilizables
└── prisma/
    └── schema.prisma        # Modelo de datos
```

## Variables de entorno

Crear un archivo `.env.local` en la raíz del proyecto:

```env
GITHUB_TOKEN=ghp_...
GITHUB_ORG=desarrolloSENAfv
GITHUB_AUTHOR=MauricioOrtizCano
ANTHROPIC_API_KEY=sk-ant-api03-...
```

## Instalación

```bash
git clone https://github.com/MauricioOrtizCano/cuenta-cobro-sena.git
cd cuenta-cobro-sena
npm install
npx prisma migrate dev
npm run dev
```

## Licencia

Uso privado.