# Step 1: Project Initialization & Configuration Files

## 1.1 Initialize Next.js Project

```bash
# Create new Next.js project with TypeScript
npx create-next-app@latest my-portfolio --typescript --eslint --app --src-dir --import-alias "@/*"

# Navigate to project directory
cd my-portfolio

# Install required dependencies
npm install framer-motion react-intersection-observer next-themes clsx class-variance-authority lucide-react

# Install development dependencies  
npm install -D @types/node @types/react @types/react-dom
```

## 1.2 Package.json

```json
{
  "name": "my-portfolio",
  "version": "0.1.0",
  "private": true,
  "scripts": {
    "dev": "next dev",
    "build": "next build",
    "start": "next start",
    "lint": "next lint",
    "type-check": "tsc --noEmit",
    "export": "next build"
  },
  "dependencies": {
    "react": "^18.2.0",
    "react-dom": "^18.2.0",
    "next": "^14.0.0",
    "typescript": "^5.0.0",
    "framer-motion": "^10.16.0",
    "react-intersection-observer": "^9.5.0",
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
    "eslint-config-next": "^14.0.0"
  }
}
```

## 1.3 Next.js Configuration (next.config.js)

```javascript
/** @type {import('next').NextConfig} */
const nextConfig = {
  experimental: {
    optimizeCss: true,
  },
  images: {
    domains: ['r2cdn.perplexity.ai'],
    formats: ['image/webp', 'image/avif'],
  },
  compiler: {
    removeConsole: process.env.NODE_ENV === 'production',
  },
  // Enable static export if deploying to GitHub Pages
  // output: 'export',
  // trailingSlash: true,
  // assetPrefix: process.env.NODE_ENV === 'production' ? '/your-repo-name' : '',
}

module.exports = nextConfig
```

## 1.4 TypeScript Configuration (tsconfig.json)

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