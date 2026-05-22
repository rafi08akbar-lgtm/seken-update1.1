# Seken Update 1.1

This is a modern web application built with **React**, **Vite**, **TanStack Router**, **Tailwind CSS**, and **Supabase**.

## 🚀 Technologies Used

- **Framework**: React 19
- **Build Tool**: Vite
- **Routing**: TanStack Router & TanStack Start
- **Styling**: Tailwind CSS & Radix UI (shadcn/ui components)
- **Backend / Database**: Supabase (Authentication & Database)
- **State Management**: Zustand
- **Data Fetching**: TanStack React Query

## 🏗️ Architecture & Design Rules

Proyek ini mengikuti prinsip-prinsip arsitektur dan desain berikut:

### 📂 FBA — Feature-Based Architecture

Kode diorganisasi berdasarkan **fitur/domain**, bukan berdasarkan tipe file. Setiap fitur memiliki folder sendiri yang berisi komponen, hooks, fungsi, dan tipe yang terkait.

```
src/
├── frontend/         # Komponen UI, hooks, assets
├── backend/          # Server logic, server functions, middleware, server-only Supabase
├── shared/           # Core domain (lib, integrations, lovable)
└── routes/           # Routing halaman (TanStack Router)
```

### 🧱 SOLID Principles

| Prinsip | Penerapan |
|---------|-----------|
| **S** — Single Responsibility | Setiap file/modul hanya bertanggung jawab atas satu hal. Contoh: `shared/lib/auth.ts` hanya menangani autentikasi, `backend/functions/products.functions.ts` hanya menangani logika produk. |
| **O** — Open/Closed | Komponen UI menggunakan `shadcn/ui` + `class-variance-authority` sehingga mudah di-extend tanpa mengubah source asli. |
| **L** — Liskov Substitution | Komponen Radix UI digunakan sebagai abstraksi yang bisa diganti dengan implementasi lain tanpa merusak sistem. |
| **I** — Interface Segregation | Server functions dipisah per domain (`backend/functions/products.functions.ts`, `backend/functions/transactions.functions.ts`) — client hanya mengimpor yang dibutuhkan. |
| **D** — Dependency Inversion | Akses database melalui abstraksi Supabase client (`shared/integrations/supabase/client.ts`, `backend/supabase/client.server.ts`), bukan query langsung. |

### 🎯 SSOT — Single Source of Truth

- **State Management**: Zustand store (`shared/lib/store.ts`) sebagai satu-satunya sumber kebenaran untuk state client-side (cart, UI state).
- **Database**: Supabase PostgreSQL sebagai satu-satunya sumber kebenaran untuk data persisten (produk, transaksi, user roles).
- **Routing**: `routeTree.gen.ts` di-generate otomatis dari file-based routing — satu sumber untuk semua definisi route.
- **Tipe Data**: `shared/integrations/supabase/types.ts` sebagai satu-satunya sumber tipe database.

### 🔷 Hexagonal Architecture (Ports & Adapters)

Arsitektur heksagonal memisahkan **core logic** dari **infrastruktur eksternal**:

```
┌─────────────────────────────────────────┐
│              CORE DOMAIN                │
│   shared/lib/products.ts,               │
│   shared/lib/auth.ts,                   │
│   shared/lib/store.ts, etc.             │
├─────────────────────────────────────────┤
│          PORTS (Interfaces)             │
│   Server Functions (createServerFn)     │
│   React Hooks (useSession, useMyRole)   │
├─────────────────────────────────────────┤
│        ADAPTERS (Infrastructure)        │
│   shared/integrations/supabase/*        │
│   frontend/components/*                 │
│   routes/*                              │
└─────────────────────────────────────────┘
```

- **Core Domain** → Logika bisnis murni, tidak bergantung pada framework tertentu.
- **Ports** → Interface yang menghubungkan core dengan dunia luar (server functions, hooks).
- **Adapters** → Implementasi konkret: Supabase sebagai DB adapter, React sebagai UI adapter.

### 🔶 Octagonal Architecture

Octagonal Architecture memperluas konsep Hexagonal dengan menambahkan lapisan keamanan, observabilitas, dan cross-cutting concerns:

```
        ┌──── Auth Middleware ────┐
        │                         │
   ┌────┴────┐             ┌──────┴─────┐
   │  Error  │             │    RLS     │
   │ Capture │             │ (Row Level │
   │         │             │  Security) │
   ├─────────┤             ├────────────┤
   │         │   CORE      │            │
   │ Logging │   DOMAIN    │ Validation │
   │         │             │   (Zod)    │
   ├─────────┤             ├────────────┤
   │  Error  │             │   RBAC     │
   │  Page   │             │ (isAdmin)  │
   └────┬────┘             └──────┬─────┘
        │                         │
        └──── SSR Middleware ─────┘
```

| Layer | File | Fungsi |
|-------|------|--------|
| Auth Middleware | `backend/middleware/auth-middleware.ts`, `backend/middleware/auth-attacher.ts` | Validasi token & injeksi Supabase context |
| Error Handling | `shared/lib/error-capture.ts`, `shared/lib/error-page.ts`, `backend/server.ts` | Tangkap error & render halaman error |
| Validation | `backend/functions/transactions.functions.ts` (Zod schemas) | Validasi input di sisi server |
| RBAC | `has_role()` SQL + `isAdmin()` TS | Kontrol akses berbasis peran |
| RLS | Supabase RLS Policies | Keamanan data di level database |

## 📦 Getting Started

### Prerequisites
Make sure you have [Node.js](https://nodejs.org/) (or [Bun](https://bun.sh/)) installed on your machine.

### Installation

1. Clone the repository:
   ```bash
   git clone https://github.com/rafi08akbar-lgtm/seken-update1.1.git
   ```
2. Navigate into the directory:
   ```bash
   cd seken-update1.1
   ```
3. Install dependencies (using npm or bun):
   ```bash
   npm install
   # or
   bun install
   ```

### Development Server

Run the local development server:

```bash
npm run dev
# or
bun run dev
```

Open [http://localhost:5173](http://localhost:5173) in your browser to see the result.

## 🛠️ Scripts

- `npm run dev` - Starts the development server
- `npm run build` - Builds the application for production
- `npm run preview` - Previews the production build locally
- `npm run lint` - Runs ESLint to check for code issues
- `npm run format` - Formats the codebase using Prettier

## 🌐 Environment Variables

Make sure to set up your `.env` file based on the required configurations (such as Supabase credentials).

```env
VITE_SUPABASE_URL=your_supabase_url
VITE_SUPABASE_ANON_KEY=your_supabase_anon_key
```

## 📝 License

This project is licensed under the MIT License.
