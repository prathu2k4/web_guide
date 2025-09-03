# Step 12: Main App Structure & Layout

## 12.1 Root Layout (src/app/layout.tsx)

```tsx
import './globals.css'
import type { Metadata } from 'next'
import { Inter } from 'next/font/google'
import { ThemeProvider } from '@/components/providers/ThemeProvider'

const inter = Inter({ 
  subsets: ['latin'],
  display: 'swap',
  variable: '--font-inter'
})

export const metadata: Metadata = {
  title: 'Pratham Jain V S - Digital Circuit Designer & Creative Developer',
  description: 'Passionate ECE student at UVCE Bengaluru, specializing in digital design, RTL development, and creative programming. Blending technical expertise with innovative problem-solving.',
  keywords: [
    'Pratham Jain',
    'Digital Circuit Design',
    'RTL Development', 
    'VLSI Design',
    'Verilog HDL',
    'FPGA',
    'ECE Student',
    'UVCE Bengaluru',
    'Hardware Design',
    'Creative Programming',
    'Electronics Engineering'
  ],
  authors: [{ name: 'Pratham Jain V S' }],
  creator: 'Pratham Jain V S',
  openGraph: {
    type: 'website',
    locale: 'en_US',
    url: 'https://your-domain.com',
    title: 'Pratham Jain V S - Digital Circuit Designer & Creative Developer',
    description: 'Passionate ECE student specializing in digital design, RTL development, and creative programming.',
    siteName: 'Pratham Jain Portfolio',
  },
  twitter: {
    card: 'summary_large_image',
    title: 'Pratham Jain V S - Digital Circuit Designer & Creative Developer',
    description: 'Passionate ECE student specializing in digital design, RTL development, and creative programming.',
  },
  robots: {
    index: true,
    follow: true,
    googleBot: {
      index: true,
      follow: true,
      'max-video-preview': -1,
      'max-image-preview': 'large',
      'max-snippet': -1,
    },
  },
  verification: {
    // Add your verification tokens here when deploying
    // google: 'your-google-verification-token',
  },
}

export default function RootLayout({
  children,
}: {
  children: React.ReactNode
}) {
  return (
    <html 
      lang="en" 
      className={inter.variable}
      suppressHydrationWarning
    >
      <head>
        <link rel="preconnect" href="https://fonts.googleapis.com" />
        <link rel="preconnect" href="https://fonts.gstatic.com" crossOrigin="anonymous" />
        <link 
          href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700;800&display=swap" 
          rel="stylesheet" 
        />
        <link rel="icon" href="/favicon.ico" sizes="any" />
        <link rel="apple-touch-icon" href="/apple-touch-icon.png" />
        <meta name="viewport" content="width=device-width, initial-scale=1" />
        <meta name="theme-color" content="#00d4ff" />
      </head>
      <body className={`${inter.className} antialiased`}>
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

## 12.2 Main Page (src/app/page.tsx)

```tsx
'use client'

import { useState, useEffect } from 'react'
import { motion, AnimatePresence } from 'framer-motion'
import dynamic from 'next/dynamic'

// Static imports for above-the-fold content
import Header from '@/components/layout/Header'
import Hero from '@/components/sections/Hero'
import LoadingScreen from '@/components/layout/LoadingScreen'

// Dynamic imports for below-the-fold content to improve performance
const About = dynamic(() => import('@/components/sections/About'), { 
  ssr: true,
  loading: () => <div className="min-h-screen flex items-center justify-center">Loading...</div>
})

const Skills = dynamic(() => import('@/components/sections/Skills'), { 
  ssr: true,
  loading: () => <div className="min-h-screen flex items-center justify-center">Loading...</div>
})

const Projects = dynamic(() => import('@/components/sections/Projects'), { 
  ssr: true,
  loading: () => <div className="min-h-screen flex items-center justify-center">Loading...</div>
})

const Achievements = dynamic(() => import('@/components/sections/Achievements'), { 
  ssr: true,
  loading: () => <div className="min-h-screen flex items-center justify-center">Loading...</div>
})

const Contact = dynamic(() => import('@/components/sections/Contact'), { 
  ssr: true,
  loading: () => <div className="min-h-screen flex items-center justify-center">Loading...</div>
})

const Footer = dynamic(() => import('@/components/layout/Footer'), { ssr: true })
const BackToTop = dynamic(() => import('@/components/layout/BackToTop'), { ssr: false })

export default function Home() {
  const [isLoading, setIsLoading] = useState(true)
  const [pageLoaded, setPageLoaded] = useState(false)

  useEffect(() => {
    // Simulate loading time and ensure smooth transitions
    const timer = setTimeout(() => {
      setIsLoading(false)
      setPageLoaded(true)
    }, 2000)

    // Preload critical resources
    const preloadFonts = () => {
      const link = document.createElement('link')
      link.rel = 'preload'
      link.href = 'https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700;800&display=swap'
      link.as = 'style'
      document.head.appendChild(link)
    }

    preloadFonts()

    return () => clearTimeout(timer)
  }, [])

  // Page transition variants
  const pageVariants = {
    hidden: { 
      opacity: 0,
      y: 20
    },
    visible: { 
      opacity: 1,
      y: 0,
      transition: {
        duration: 0.6,
        ease: "easeOut",
        staggerChildren: 0.1,
        delayChildren: 0.2
      }
    }
  }

  const sectionVariants = {
    hidden: { 
      opacity: 0,
      y: 30
    },
    visible: { 
      opacity: 1,
      y: 0,
      transition: {
        duration: 0.8,
        ease: "easeOut"
      }
    }
  }

  return (
    <>
      {/* Loading Screen */}
      <LoadingScreen isVisible={isLoading} />

      {/* Main Application */}
      <AnimatePresence>
        {pageLoaded && (
          <motion.main
            initial="hidden"
            animate="visible"
            variants={pageVariants}
            className="relative"
          >
            {/* Navigation */}
            <Header />

            {/* Page Sections */}
            <motion.div variants={sectionVariants}>
              <Hero />
            </motion.div>

            <motion.div variants={sectionVariants}>
              <About />
            </motion.div>

            <motion.div variants={sectionVariants}>
              <Skills />
            </motion.div>

            <motion.div variants={sectionVariants}>
              <Projects />
            </motion.div>

            <motion.div variants={sectionVariants}>
              <Achievements />
            </motion.div>

            <motion.div variants={sectionVariants}>
              <Contact />
            </motion.div>

            <motion.div variants={sectionVariants}>
              <Footer />
            </motion.div>

            {/* Back to Top Button */}
            <BackToTop />

            {/* Background Effects */}
            <PageBackgroundEffects />
          </motion.main>
        )}
      </AnimatePresence>
    </>
  )
}

// Background Effects Component
const PageBackgroundEffects = () => {
  return (
    <div className="fixed inset-0 pointer-events-none overflow-hidden" style={{ zIndex: -1 }}>
      {/* Gradient Orbs */}
      <motion.div
        className="absolute top-1/4 left-1/4 w-96 h-96 rounded-full"
        style={{
          background: 'radial-gradient(circle, rgba(0, 212, 255, 0.03) 0%, transparent 70%)',
          filter: 'blur(40px)'
        }}
        animate={{
          x: [0, 100, 0],
          y: [0, 50, 0],
        }}
        transition={{
          duration: 20,
          repeat: Infinity,
          ease: "easeInOut"
        }}
      />
      
      <motion.div
        className="absolute bottom-1/4 right-1/4 w-80 h-80 rounded-full"
        style={{
          background: 'radial-gradient(circle, rgba(16, 185, 129, 0.03) 0%, transparent 70%)',
          filter: 'blur(40px)'
        }}
        animate={{
          x: [0, -80, 0],
          y: [0, -60, 0],
        }}
        transition={{
          duration: 25,
          repeat: Infinity,
          ease: "easeInOut",
          delay: 10
        }}
      />
      
      <motion.div
        className="absolute top-3/4 left-1/2 w-64 h-64 rounded-full"
        style={{
          background: 'radial-gradient(circle, rgba(139, 92, 246, 0.03) 0%, transparent 70%)',
          filter: 'blur(30px)'
        }}
        animate={{
          x: [0, -60, 0],
          y: [0, 40, 0],
        }}
        transition={{
          duration: 18,
          repeat: Infinity,
          ease: "easeInOut",
          delay: 5
        }}
      />
    </div>
  )
}
```

## 12.3 Loading State (src/app/loading.tsx)

```tsx
'use client'

import { motion } from 'framer-motion'

export default function Loading() {
  return (
    <div className="fixed inset-0 bg-gradient-to-br from-slate-900 to-slate-800 flex items-center justify-content-center z-50">
      <div className="text-center">
        <motion.div
          className="w-16 h-16 border-4 border-blue-200 border-t-blue-500 rounded-full mx-auto mb-4"
          animate={{ rotate: 360 }}
          transition={{
            duration: 1,
            repeat: Infinity,
            ease: "linear"
          }}
        />
        <motion.p
          className="text-blue-400 text-lg font-medium"
          initial={{ opacity: 0 }}
          animate={{ opacity: 1 }}
          transition={{ delay: 0.5 }}
        >
          Loading Portfolio...
        </motion.p>
      </div>
    </div>
  )
}
```

## 12.4 Error Boundary (src/app/error.tsx)

```tsx
'use client'

import { useEffect } from 'react'
import { motion } from 'framer-motion'
import { AlertTriangle, RefreshCw, Home } from 'lucide-react'
import { Button } from '@/components/ui/Button'

export default function Error({
  error,
  reset,
}: {
  error: Error & { digest?: string }
  reset: () => void
}) {
  useEffect(() => {
    console.error('Application error:', error)
  }, [error])

  return (
    <div className="min-h-screen flex items-center justify-center bg-gradient-to-br from-slate-900 to-slate-800 px-4">
      <motion.div
        className="text-center max-w-md mx-auto"
        initial={{ opacity: 0, y: 20 }}
        animate={{ opacity: 1, y: 0 }}
        transition={{ duration: 0.6 }}
      >
        <motion.div
          className="text-red-400 mb-6"
          animate={{ rotate: [0, 10, -10, 0] }}
          transition={{ duration: 2, repeat: Infinity }}
        >
          <AlertTriangle size={64} />
        </motion.div>
        
        <h1 className="text-2xl font-bold text-white mb-4">
          Oops! Something went wrong
        </h1>
        
        <p className="text-gray-300 mb-8 leading-relaxed">
          We encountered an unexpected error while loading the portfolio. 
          Please try refreshing the page or go back to the homepage.
        </p>
        
        <div className="flex flex-col sm:flex-row gap-4 justify-center">
          <Button
            variant="primary"
            size="lg"
            onClick={reset}
            className="flex items-center gap-2"
          >
            <RefreshCw size={20} />
            Try Again
          </Button>
          
          <Button
            variant="outline"
            size="lg"
            onClick={() => window.location.href = '/'}
            className="flex items-center gap-2"
          >
            <Home size={20} />
            Go Home
          </Button>
        </div>
        
        {process.env.NODE_ENV === 'development' && (
          <motion.details
            className="mt-8 text-left"
            initial={{ opacity: 0 }}
            animate={{ opacity: 1 }}
            transition={{ delay: 1 }}
          >
            <summary className="text-gray-400 cursor-pointer hover:text-gray-300 transition-colors">
              Error Details (Development Only)
            </summary>
            <pre className="mt-4 p-4 bg-red-900/20 border border-red-500/30 rounded text-red-300 text-xs overflow-auto">
              {error.message}
            </pre>
          </motion.details>
        )}
      </motion.div>
    </div>
  )
}
```

## 12.5 Not Found Page (src/app/not-found.tsx)

```tsx
'use client'

import { motion } from 'framer-motion'
import { Home, ArrowLeft, Search } from 'lucide-react'
import { Button } from '@/components/ui/Button'
import Link from 'next/link'

export default function NotFound() {
  return (
    <div className="min-h-screen flex items-center justify-center bg-gradient-to-br from-slate-900 to-slate-800 px-4">
      <motion.div
        className="text-center max-w-lg mx-auto"
        initial={{ opacity: 0, y: 20 }}
        animate={{ opacity: 1, y: 0 }}
        transition={{ duration: 0.8 }}
      >
        {/* 404 Number */}
        <motion.div
          className="mb-8"
          initial={{ scale: 0 }}
          animate={{ scale: 1 }}
          transition={{ 
            duration: 0.8, 
            delay: 0.2,
            type: "spring",
            stiffness: 200
          }}
        >
          <h1 className="text-8xl sm:text-9xl font-bold text-transparent bg-clip-text bg-gradient-to-r from-blue-400 to-teal-400 leading-none">
            404
          </h1>
        </motion.div>

        {/* Floating Search Icon */}
        <motion.div
          className="text-blue-400 mb-6"
          animate={{ 
            y: [0, -10, 0],
            rotate: [0, 5, -5, 0]
          }}
          transition={{ 
            duration: 3, 
            repeat: Infinity,
            ease: "easeInOut"
          }}
        >
          <Search size={48} />
        </motion.div>

        <motion.h2
          className="text-2xl sm:text-3xl font-bold text-white mb-4"
          initial={{ opacity: 0 }}
          animate={{ opacity: 1 }}
          transition={{ delay: 0.4 }}
        >
          Page Not Found
        </motion.h2>

        <motion.p
          className="text-gray-300 mb-8 leading-relaxed text-lg"
          initial={{ opacity: 0 }}
          animate={{ opacity: 1 }}
          transition={{ delay: 0.6 }}
        >
          The page you're looking for doesn't exist or has been moved. 
          Let's get you back to exploring my portfolio!
        </motion.p>

        <motion.div
          className="flex flex-col sm:flex-row gap-4 justify-center"
          initial={{ opacity: 0, y: 20 }}
          animate={{ opacity: 1, y: 0 }}
          transition={{ delay: 0.8 }}
        >
          <Link href="/">
            <Button
              variant="primary"
              size="lg"
              glow
              className="flex items-center gap-2"
            >
              <Home size={20} />
              Back to Portfolio
            </Button>
          </Link>

          <Button
            variant="outline"
            size="lg"
            onClick={() => window.history.back()}
            className="flex items-center gap-2"
          >
            <ArrowLeft size={20} />
            Go Back
          </Button>
        </motion.div>

        {/* Decorative Elements */}
        <motion.div
          className="absolute top-1/4 left-1/4 w-32 h-32 rounded-full opacity-20"
          style={{
            background: 'radial-gradient(circle, rgba(0, 212, 255, 0.1) 0%, transparent 70%)',
            filter: 'blur(20px)'
          }}
          animate={{
            scale: [1, 1.2, 1],
            opacity: [0.1, 0.2, 0.1]
          }}
          transition={{
            duration: 4,
            repeat: Infinity,
            ease: "easeInOut"
          }}
        />

        <motion.div
          className="absolute bottom-1/4 right-1/4 w-40 h-40 rounded-full opacity-20"
          style={{
            background: 'radial-gradient(circle, rgba(16, 185, 129, 0.1) 0%, transparent 70%)',
            filter: 'blur(25px)'
          }}
          animate={{
            scale: [1, 1.3, 1],
            opacity: [0.1, 0.15, 0.1]
          }}
          transition={{
            duration: 5,
            repeat: Infinity,
            ease: "easeInOut",
            delay: 2
          }}
        />
      </motion.div>
    </div>
  )
}
```

## 12.6 Global Error Boundary (src/app/global-error.tsx)

```tsx
'use client'

import { useEffect } from 'react'
import { motion } from 'framer-motion'
import { AlertTriangle, RefreshCw } from 'lucide-react'

export default function GlobalError({
  error,
  reset,
}: {
  error: Error & { digest?: string }
  reset: () => void
}) {
  useEffect(() => {
    console.error('Global application error:', error)
  }, [error])

  return (
    <html>
      <body>
        <div className="min-h-screen flex items-center justify-center bg-gradient-to-br from-red-900 to-slate-900 px-4">
          <motion.div
            className="text-center max-w-md mx-auto"
            initial={{ opacity: 0, scale: 0.8 }}
            animate={{ opacity: 1, scale: 1 }}
            transition={{ duration: 0.6 }}
          >
            <motion.div
              className="text-red-400 mb-6"
              animate={{ 
                rotate: [0, 10, -10, 0],
                scale: [1, 1.1, 1]
              }}
              transition={{ 
                duration: 2, 
                repeat: Infinity,
                ease: "easeInOut"
              }}
            >
              <AlertTriangle size={64} />
            </motion.div>
            
            <h1 className="text-3xl font-bold text-white mb-4">
              Critical Error
            </h1>
            
            <p className="text-gray-300 mb-8 leading-relaxed">
              A critical error occurred that prevented the application from loading properly. 
              Please refresh the page to try again.
            </p>
            
            <motion.button
              onClick={reset}
              className="btn btn--primary flex items-center gap-2 mx-auto"
              whileHover={{ scale: 1.05 }}
              whileTap={{ scale: 0.95 }}
            >
              <RefreshCw size={20} />
              Restart Application
            </motion.button>
            
            {process.env.NODE_ENV === 'development' && (
              <details className="mt-6 text-left">
                <summary className="text-gray-400 cursor-pointer">
                  Technical Details
                </summary>
                <pre className="mt-2 p-3 bg-red-900/20 border border-red-500/30 rounded text-red-300 text-xs overflow-auto">
                  {error.message}
                </pre>
              </details>
            )}
          </motion.div>
        </div>
      </body>
    </html>
  )
}
```

## 12.7 Metadata Configuration (src/app/sitemap.ts)

```tsx
import { MetadataRoute } from 'next'
 
export default function sitemap(): MetadataRoute.Sitemap {
  const baseUrl = process.env.NEXT_PUBLIC_BASE_URL || 'https://your-domain.com'
  
  return [
    {
      url: baseUrl,
      lastModified: new Date(),
      changeFrequency: 'monthly',
      priority: 1,
    },
    {
      url: `${baseUrl}/#about`,
      lastModified: new Date(),
      changeFrequency: 'monthly',
      priority: 0.8,
    },
    {
      url: `${baseUrl}/#projects`,
      lastModified: new Date(),
      changeFrequency: 'weekly',
      priority: 0.9,
    },
    {
      url: `${baseUrl}/#skills`,
      lastModified: new Date(),
      changeFrequency: 'monthly',
      priority: 0.7,
    },
    {
      url: `${baseUrl}/#achievements`,
      lastModified: new Date(),
      changeFrequency: 'monthly',
      priority: 0.6,
    },
    {
      url: `${baseUrl}/#contact`,
      lastModified: new Date(),
      changeFrequency: 'yearly',
      priority: 0.5,
    },
  ]
}
```

## 12.8 Robots.txt (src/app/robots.ts)

```tsx
import { MetadataRoute } from 'next'
 
export default function robots(): MetadataRoute.Robots {
  const baseUrl = process.env.NEXT_PUBLIC_BASE_URL || 'https://your-domain.com'
  
  return {
    rules: {
      userAgent: '*',
      allow: '/',
      disallow: ['/api/', '/_next/', '/private/'],
    },
    sitemap: `${baseUrl}/sitemap.xml`,
  }
}
```