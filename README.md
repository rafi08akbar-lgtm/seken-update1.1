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
