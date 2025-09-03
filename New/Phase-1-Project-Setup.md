# Phase 1: Project Setup & Basic Structure

This phase sets up the foundational Next.js project with TypeScript and basic configuration. After completing this phase, you'll have a working Next.js app that displays "Hello World" and basic routing.

## Step 1: Initialize Next.js Project

### 1.1 Create Next.js Project
```bash
# Create new Next.js project with TypeScript
npx create-next-app@latest pratham-portfolio --typescript --eslint --app --src-dir --import-alias "@/*"

# Navigate to project directory
cd pratham-portfolio

# Install required dependencies for this phase
npm install framer-motion next-themes clsx class-variance-authority lucide-react

# Install development dependencies  
npm install -D @types/node @types/react @types/react-dom prettier eslint-config-prettier
```

### 1.2 Update Package.json Scripts

**File: `package.json`**
```json
{
  "name": "pratham-portfolio",
  "version": "0.1.0",
  "private": true,
  "scripts": {
    "dev": "next dev --turbo",
    "build": "next build",
    "start": "next start",
    "lint": "next lint",
    "lint:fix": "next lint --fix",
    "type-check": "tsc --noEmit",
    "format": "prettier --write .",
    "format:check": "prettier --check ."
  },
  "dependencies": {
    "react": "^18.2.0",
    "react-dom": "^18.2.0",
    "next": "^14.0.0",
    "typescript": "^5.0.0",
    "framer-motion": "^10.16.0",
    "next-themes": "^0.2.1",
    "clsx": "^2.0.0",
    "class-variance-authority": "^0.7.0",
    "lucide-react": "^0.294.0"
  },
  "devDependencies": {
    "@types/node": "^20.0.0",
    "@types/react": "^18.2.0",
    "@types/react-dom": "^18.2.0",
    "eslint": "^8.0.0",
    "eslint-config-next": "^14.0.0",
    "prettier": "^3.1.0",
    "eslint-config-prettier": "^9.0.0"
  }
}
```

### 1.3 Configure TypeScript

**File: `tsconfig.json`**
```json
{
  "compilerOptions": {
    "target": "es5",
    "lib": ["dom", "dom.iterable", "es6"],
    "allowJs": true,
    "skipLibCheck": true,
    "strict": true,
    "noEmit": true,
    "esModuleInterop": true,
    "module": "esnext",
    "moduleResolution": "bundler",
    "resolveJsonModule": true,
    "isolatedModules": true,
    "jsx": "preserve",
    "incremental": true,
    "plugins": [
      {
        "name": "next"
      }
    ],
    "baseUrl": ".",
    "paths": {
      "@/*": ["./src/*"],
      "@/components/*": ["./src/components/*"],
      "@/lib/*": ["./src/lib/*"],
      "@/styles/*": ["./src/styles/*"],
      "@/types/*": ["./src/types/*"]
    }
  },
  "include": ["next-env.d.ts", "**/*.ts", "**/*.tsx", ".next/types/**/*.ts"],
  "exclude": ["node_modules"]
}
```

### 1.4 Configure Prettier

**File: `.prettierrc.json`**
```json
{
  "printWidth": 80,
  "tabWidth": 2,
  "useTabs": false,
  "semi": false,
  "singleQuote": true,
  "quoteProps": "as-needed",
  "trailingComma": "es5",
  "bracketSpacing": true,
  "bracketSameLine": false,
  "arrowParens": "avoid",
  "endOfLine": "lf"
}
```

### 1.5 Configure ESLint

**File: `.eslintrc.json`**
```json
{
  "extends": [
    "next/core-web-vitals",
    "prettier"
  ],
  "rules": {
    "prefer-const": "error",
    "no-var": "error",
    "react-hooks/exhaustive-deps": "warn",
    "react/no-unescaped-entities": "off"
  }
}
```

### 1.6 Create Directory Structure

Create these directories in your project:
```bash
mkdir -p src/components/ui
mkdir -p src/components/layout  
mkdir -p src/components/sections
mkdir -p src/lib/hooks
mkdir -p src/lib/data
mkdir -p src/types
mkdir -p public/images
mkdir -p public/icons
```

### 1.7 Create Basic Type Definitions

**File: `src/types/index.ts`**
```typescript
export interface NavigationItem {
  href: string
  label: string
}

export interface Project {
  id: number
  title: string
  status: 'ongoing' | 'completed'
  description: string
  technologies: string[]
  category: 'digital' | 'systems' | 'software'
  highlights: string[]
  githubUrl?: string
  award?: string
  company?: string
  icon?: string
}

export interface Skill {
  name: string
  level: number
  category: 'programming' | 'tools' | 'domains'
  icon: string
  description: string
}
```

## Step 2: Basic Layout & Theme Setup

### 2.1 Create Theme Provider

**File: `src/components/providers/ThemeProvider.tsx`**
```tsx
'use client'

import { ThemeProvider as NextThemesProvider } from 'next-themes'
import { type ThemeProviderProps } from 'next-themes/dist/types'
import { useEffect, useState } from 'react'

export function ThemeProvider({ children, ...props }: ThemeProviderProps) {
  const [mounted, setMounted] = useState(false)

  useEffect(() => {
    setMounted(true)
  }, [])

  if (!mounted) {
    return <>{children}</>
  }

  return <NextThemesProvider {...props}>{children}</NextThemesProvider>
}
```

### 2.2 Create Basic Root Layout

**File: `src/app/layout.tsx`**
```tsx
import './globals.css'
import type { Metadata } from 'next'
import { Inter } from 'next/font/google'
import { ThemeProvider } from '@/components/providers/ThemeProvider'

const inter = Inter({ 
  subsets: ['latin'],
  display: 'swap',
})

export const metadata: Metadata = {
  title: 'Pratham Jain V S - Portfolio',
  description: 'Digital Circuit Designer & Creative Developer',
}

export default function RootLayout({
  children,
}: {
  children: React.ReactNode
}) {
  return (
    <html lang="en" suppressHydrationWarning>
      <body className={inter.className}>
        <ThemeProvider
          attribute="data-theme"
          defaultTheme="dark"
          enableSystem={false}
          storageKey="portfolio-theme"
        >
          {children}
        </ThemeProvider>
      </body>
    </html>
  )
}
```

### 2.3 Create Basic Global Styles

**File: `src/app/globals.css`**
```css
@import url('https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700;800&display=swap');

:root {
  /* Colors */
  --color-background: #f8fafc;
  --color-surface: #ffffff;
  --color-text: #1e293b;
  --color-text-secondary: #64748b;
  --color-primary: #0ea5e9;
  --color-primary-hover: #0284c7;
  --color-border: #e2e8f0;

  /* Typography */
  --font-family-base: "Inter", -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, sans-serif;
  --font-size-base: 16px;
  --font-size-lg: 18px;
  --font-size-xl: 20px;
  --font-size-2xl: 24px;
  --font-size-3xl: 30px;
  --font-size-4xl: 36px;

  /* Spacing */
  --space-4: 4px;
  --space-8: 8px;
  --space-12: 12px;
  --space-16: 16px;
  --space-20: 20px;
  --space-24: 24px;
  --space-32: 32px;

  /* Border Radius */
  --radius-sm: 4px;
  --radius-base: 8px;
  --radius-lg: 12px;
  --radius-full: 9999px;
}

/* Dark theme colors */
[data-theme="dark"] {
  --color-background: #0f172a;
  --color-surface: #1e293b;
  --color-text: #f1f5f9;
  --color-text-secondary: #94a3b8;
  --color-primary: #38bdf8;
  --color-primary-hover: #0ea5e9;
  --color-border: #334155;
}

/* Base styles */
* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}

html {
  font-size: var(--font-size-base);
  font-family: var(--font-family-base);
  scroll-behavior: smooth;
}

body {
  background-color: var(--color-background);
  color: var(--color-text);
  transition: background-color 0.3s ease, color 0.3s ease;
  overflow-x: hidden;
}

/* Typography */
h1, h2, h3, h4, h5, h6 {
  font-weight: 600;
  line-height: 1.2;
  color: var(--color-text);
}

h1 { font-size: var(--font-size-4xl); }
h2 { font-size: var(--font-size-3xl); }
h3 { font-size: var(--font-size-2xl); }
h4 { font-size: var(--font-size-xl); }

p {
  line-height: 1.6;
  color: var(--color-text-secondary);
}

/* Container */
.container {
  width: 100%;
  max-width: 1200px;
  margin: 0 auto;
  padding: 0 var(--space-16);
}

/* Button base */
.btn {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  padding: var(--space-8) var(--space-16);
  border-radius: var(--radius-base);
  font-size: var(--font-size-base);
  font-weight: 500;
  cursor: pointer;
  transition: all 0.2s ease;
  border: none;
  text-decoration: none;
}

.btn--primary {
  background-color: var(--color-primary);
  color: white;
}

.btn--primary:hover {
  background-color: var(--color-primary-hover);
}
```

### 2.4 Create Simple Theme Toggle

**File: `src/components/ui/ThemeToggle.tsx`**
```tsx
'use client'

import { useState, useEffect } from 'react'
import { useTheme } from 'next-themes'
import { Sun, Moon } from 'lucide-react'

const ThemeToggle = () => {
  const [mounted, setMounted] = useState(false)
  const { theme, setTheme } = useTheme()

  useEffect(() => {
    setMounted(true)
  }, [])

  if (!mounted) {
    return <div className="w-9 h-9" />
  }

  const toggleTheme = () => {
    setTheme(theme === 'dark' ? 'light' : 'dark')
  }

  return (
    <button
      onClick={toggleTheme}
      className="btn"
      style={{
        background: 'var(--color-surface)',
        border: '1px solid var(--color-border)',
        width: '36px',
        height: '36px',
        borderRadius: 'var(--radius-full)',
      }}
      aria-label="Toggle theme"
    >
      {theme === 'dark' ? <Moon size={16} /> : <Sun size={16} />}
    </button>
  )
}

export default ThemeToggle
```

### 2.5 Create Basic Main Page

**File: `src/app/page.tsx`**
```tsx
'use client'

import ThemeToggle from '@/components/ui/ThemeToggle'

export default function Home() {
  return (
    <main style={{ minHeight: '100vh', padding: 'var(--space-24)' }}>
      <div className="container">
        {/* Header with theme toggle */}
        <header style={{ 
          display: 'flex', 
          justifyContent: 'space-between', 
          alignItems: 'center',
          marginBottom: 'var(--space-32)',
          padding: 'var(--space-16) 0'
        }}>
          <h1 style={{ color: 'var(--color-primary)' }}>Pratham Jain</h1>
          <ThemeToggle />
        </header>

        {/* Hero Section */}
        <section style={{ 
          textAlign: 'center', 
          padding: 'var(--space-32) 0' 
        }}>
          <h2 style={{ marginBottom: 'var(--space-16)' }}>
            Digital Circuit Designer & Creative Developer
          </h2>
          <p style={{ 
            maxWidth: '600px', 
            margin: '0 auto var(--space-24)',
            fontSize: 'var(--font-size-lg)'
          }}>
            Passionate ECE student at UVCE Bengaluru, specializing in digital design, 
            RTL development, and creative programming.
          </p>
          <button className="btn btn--primary">
            View My Work
          </button>
        </section>

        {/* Placeholder sections */}
        <section style={{ padding: 'var(--space-32) 0' }}>
          <h3 style={{ marginBottom: 'var(--space-16)' }}>About Me</h3>
          <p>This section will contain the about content...</p>
        </section>

        <section style={{ padding: 'var(--space-32) 0' }}>
          <h3 style={{ marginBottom: 'var(--space-16)' }}>Skills</h3>
          <p>This section will contain the skills content...</p>
        </section>

        <section style={{ padding: 'var(--space-32) 0' }}>
          <h3 style={{ marginBottom: 'var(--space-16)' }}>Projects</h3>
          <p>This section will contain the projects content...</p>
        </section>
      </div>
    </main>
  )
}
```

## Step 3: Test Your Setup

### 3.1 Run the Development Server
```bash
npm run dev
```

### 3.2 Verify Everything Works

1. Open `http://localhost:3000` in your browser
2. You should see:
   - "Pratham Jain" header with theme toggle
   - Hero section with title and description
   - Placeholder sections for About, Skills, Projects
   - Theme toggle working (dark/light mode switch)

### 3.3 Test Commands
```bash
# Test TypeScript
npm run type-check

# Test linting
npm run lint

# Test formatting
npm run format:check
```

## What You've Accomplished

✅ Set up Next.js 14 with TypeScript and App Router  
✅ Configured development tools (ESLint, Prettier)  
✅ Created basic project structure and folders  
✅ Implemented theme switching (dark/light mode)  
✅ Created a basic responsive layout  
✅ Added proper TypeScript types  
✅ Verified everything works locally  

## Next Phase Preview

In Phase 2, we'll add:
- Navigation header with smooth scrolling
- Basic animations with Framer Motion
- Structured data for projects and skills
- Card components for better UI

The portfolio is now ready for incremental development where you can see each change immediately!