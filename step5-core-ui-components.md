# Step 5: Core Components - Theme & UI Elements

## 5.1 Theme Provider (src/components/providers/ThemeProvider.tsx)

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

## 5.2 Theme Toggle (src/components/ui/ThemeToggle.tsx)

```tsx
'use client'

import { useState, useEffect } from 'react'
import { useTheme } from 'next-themes'
import { motion } from 'framer-motion'
import { Sun, Moon } from 'lucide-react'

const ThemeToggle = () => {
  const [mounted, setMounted] = useState(false)
  const { theme, setTheme } = useTheme()

  useEffect(() => {
    setMounted(true)
  }, [])

  if (!mounted) {
    return (
      <div className="theme-toggle">
        <div className="theme-toggle__icon">🌙</div>
      </div>
    )
  }

  const toggleTheme = () => {
    setTheme(theme === 'dark' ? 'light' : 'dark')
  }

  return (
    <motion.button
      className="theme-toggle"
      onClick={toggleTheme}
      whileHover={{ scale: 1.1 }}
      whileTap={{ scale: 0.9 }}
      initial={{ rotate: 0 }}
      animate={{ rotate: theme === 'dark' ? 0 : 180 }}
      transition={{ duration: 0.3 }}
      aria-label="Toggle theme"
    >
      <motion.div
        className="theme-toggle__icon"
        initial={false}
        animate={{ rotate: theme === 'dark' ? 0 : 180 }}
        transition={{ duration: 0.3 }}
      >
        {theme === 'dark' ? <Moon size={20} /> : <Sun size={20} />}
      </motion.div>
    </motion.button>
  )
}

export default ThemeToggle
```

## 5.3 Loading Screen (src/components/layout/LoadingScreen.tsx)

```tsx
'use client'

import { motion, AnimatePresence } from 'framer-motion'
import styles from './LoadingScreen.module.css'

interface LoadingScreenProps {
  isVisible: boolean
}

const LoadingScreen = ({ isVisible }: LoadingScreenProps) => {
  return (
    <AnimatePresence>
      {isVisible && (
        <motion.div
          className={styles.loadingScreen}
          initial={{ opacity: 1 }}
          exit={{ opacity: 0 }}
          transition={{ duration: 0.5 }}
        >
          <div className={styles.loadingAnimation}>
            <motion.div
              className={styles.loadingCircle}
              animate={{ rotate: 360 }}
              transition={{
                duration: 1,
                repeat: Infinity,
                ease: 'linear'
              }}
            />
            <motion.div
              className={styles.loadingText}
              initial={{ opacity: 0 }}
              animate={{ opacity: 1 }}
              transition={{ delay: 0.5 }}
            >
              Loading...
            </motion.div>
          </div>
        </motion.div>
      )}
    </AnimatePresence>
  )
}

export default LoadingScreen
```

## 5.4 Loading Screen Styles (src/components/layout/LoadingScreen.module.css)

```css
.loadingScreen {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: linear-gradient(135deg, #1a202c 0%, #2d3748 100%);
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 9999;
}

.loadingAnimation {
  text-align: center;
}

.loadingCircle {
  width: 60px;
  height: 60px;
  border: 3px solid rgba(0, 212, 255, 0.2);
  border-top: 3px solid #00d4ff;
  border-radius: 50%;
  margin: 0 auto 20px;
}

.loadingText {
  color: #00d4ff;
  font-size: var(--font-size-lg);
  font-weight: var(--font-weight-medium);
}
```

## 5.5 Button Component (src/components/ui/Button.tsx)

```tsx
'use client'

import { ButtonHTMLAttributes, forwardRef } from 'react'
import { motion, HTMLMotionProps } from 'framer-motion'
import { cn } from '@/lib/utils'
import { cva, type VariantProps } from 'class-variance-authority'

const buttonVariants = cva(
  'inline-flex items-center justify-center rounded-md text-sm font-medium ring-offset-background transition-all duration-300 focus-visible:outline-none focus-visible:ring-2 focus-visible:ring-ring focus-visible:ring-offset-2 disabled:pointer-events-none disabled:opacity-50 relative overflow-hidden',
  {
    variants: {
      variant: {
        primary: 'btn btn--primary',
        outline: 'btn btn--outline',
        secondary: 'btn btn--secondary',
        ghost: 'hover:bg-accent hover:text-accent-foreground',
        link: 'text-primary underline-offset-4 hover:underline',
      },
      size: {
        sm: 'btn--sm',
        md: 'btn',
        lg: 'btn--lg',
      },
      glow: {
        true: 'btn--glow',
      }
    },
    defaultVariants: {
      variant: 'primary',
      size: 'md',
    },
  }
)

export interface ButtonProps
  extends Omit<HTMLMotionProps<'button'>, 'size'>,
    VariantProps<typeof buttonVariants> {
  asChild?: boolean
  children: React.ReactNode
}

const Button = forwardRef<HTMLButtonElement, ButtonProps>(
  ({ className, variant, size, glow, children, ...props }, ref) => {
    return (
      <motion.button
        className={cn(buttonVariants({ variant, size, glow, className }))}
        ref={ref}
        whileHover={{ 
          scale: variant === 'primary' || variant === 'outline' ? 1.05 : 1.02,
          y: variant === 'primary' || variant === 'outline' ? -3 : -1
        }}
        whileTap={{ scale: 0.95 }}
        transition={{ duration: 0.2 }}
        {...props}
      >
        {children}
      </motion.button>
    )
  }
)

Button.displayName = 'Button'

export { Button, buttonVariants }
```

## 5.6 Card Component (src/components/ui/Card.tsx)

```tsx
'use client'

import { HTMLAttributes, forwardRef } from 'react'
import { motion, HTMLMotionProps } from 'framer-motion'
import { cn } from '@/lib/utils'

interface CardProps extends HTMLMotionProps<'div'> {
  glass?: boolean
  hover?: boolean
  children: React.ReactNode
}

const Card = forwardRef<HTMLDivElement, CardProps>(
  ({ className, glass = false, hover = false, children, ...props }, ref) => {
    return (
      <motion.div
        ref={ref}
        className={cn(
          'card',
          glass && 'glass-card',
          className
        )}
        whileHover={hover ? { y: -5, scale: 1.02 } : undefined}
        transition={{ duration: 0.3 }}
        {...props}
      >
        {children}
      </motion.div>
    )
  }
)

Card.displayName = 'Card'

const CardHeader = forwardRef<HTMLDivElement, HTMLAttributes<HTMLDivElement>>(
  ({ className, ...props }, ref) => (
    <div
      ref={ref}
      className={cn('card__header', className)}
      {...props}
    />
  )
)

CardHeader.displayName = 'CardHeader'

const CardBody = forwardRef<HTMLDivElement, HTMLAttributes<HTMLDivElement>>(
  ({ className, ...props }, ref) => (
    <div
      ref={ref}
      className={cn('card__body', className)}
      {...props}
    />
  )
)

CardBody.displayName = 'CardBody'

const CardFooter = forwardRef<HTMLDivElement, HTMLAttributes<HTMLDivElement>>(
  ({ className, ...props }, ref) => (
    <div
      ref={ref}
      className={cn('card__footer', className)}
      {...props}
    />
  )
)

CardFooter.displayName = 'CardFooter'

export { Card, CardHeader, CardBody, CardFooter }
```

## 5.7 Modal Component (src/components/ui/Modal.tsx)

```tsx
'use client'

import { useEffect } from 'react'
import { motion, AnimatePresence } from 'framer-motion'
import { X } from 'lucide-react'
import { cn } from '@/lib/utils'
import styles from './Modal.module.css'

interface ModalProps {
  isOpen: boolean
  onClose: () => void
  children: React.ReactNode
  className?: string
  title?: string
}

const Modal = ({ isOpen, onClose, children, className, title }: ModalProps) => {
  useEffect(() => {
    const handleEscape = (event: KeyboardEvent) => {
      if (event.key === 'Escape' && isOpen) {
        onClose()
      }
    }

    document.addEventListener('keydown', handleEscape)
    
    if (isOpen) {
      document.body.style.overflow = 'hidden'
    } else {
      document.body.style.overflow = 'unset'
    }

    return () => {
      document.removeEventListener('keydown', handleEscape)
      document.body.style.overflow = 'unset'
    }
  }, [isOpen, onClose])

  return (
    <AnimatePresence>
      {isOpen && (
        <div className={styles.modal}>
          <motion.div
            className={styles.modalBackdrop}
            initial={{ opacity: 0 }}
            animate={{ opacity: 1 }}
            exit={{ opacity: 0 }}
            onClick={onClose}
          />
          <motion.div
            className={cn(styles.modalContent, 'glass-card', className)}
            initial={{ opacity: 0, scale: 0.9, y: 20 }}
            animate={{ opacity: 1, scale: 1, y: 0 }}
            exit={{ opacity: 0, scale: 0.9, y: 20 }}
            transition={{ type: 'spring', duration: 0.5 }}
          >
            <button
              className={styles.modalClose}
              onClick={onClose}
              aria-label="Close modal"
            >
              <X size={20} />
            </button>
            
            {title && (
              <div className={styles.modalHeader}>
                <h2>{title}</h2>
              </div>
            )}
            
            <div className={styles.modalBody}>
              {children}
            </div>
          </motion.div>
        </div>
      )}
    </AnimatePresence>
  )
}

export default Modal
```

## 5.8 Modal Styles (src/components/ui/Modal.module.css)

```css
.modal {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  z-index: 2000;
  display: flex;
  align-items: center;
  justify-content: center;
  padding: var(--space-20);
}

.modalBackdrop {
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: rgba(0, 0, 0, 0.8);
  backdrop-filter: blur(10px);
}

.modalContent {
  position: relative;
  max-width: 700px;
  width: 100%;
  max-height: 80vh;
  overflow-y: auto;
  z-index: 10;
}

.modalClose {
  position: absolute;
  top: var(--space-16);
  right: var(--space-16);
  width: 40px;
  height: 40px;
  background: rgba(255, 255, 255, 0.1);
  border: none;
  border-radius: var(--radius-base);
  cursor: pointer;
  color: var(--color-gray-200);
  z-index: 20;
  transition: all 0.3s ease;
  backdrop-filter: blur(10px);
  display: flex;
  align-items: center;
  justify-content: center;
}

.modalClose:hover {
  background: rgba(239, 68, 68, 0.2);
  color: #EF4444;
  transform: scale(1.1);
}

.modalHeader {
  padding: var(--space-24) var(--space-32) var(--space-16);
  border-bottom: 1px solid var(--color-border);
}

.modalHeader h2 {
  font-size: var(--font-size-2xl);
  font-weight: var(--font-weight-bold);
  margin: 0;
  color: #00d4ff;
}

.modalBody {
  padding: var(--space-32);
}

.modalBody p {
  color: var(--color-gray-300);
  line-height: 1.7;
  margin-bottom: var(--space-16);
}

.modalBody h3 {
  font-size: var(--font-size-xl);
  font-weight: var(--font-weight-semibold);
  color: var(--color-gray-200);
  margin: var(--space-20) 0 var(--space-12) 0;
}

/* Responsive */
@media (max-width: 768px) {
  .modal {
    padding: var(--space-16);
  }

  .modalBody {
    padding: var(--space-24);
  }
}
```

## 5.9 Progress Ring Component (src/components/ui/ProgressRing.tsx)

```tsx
'use client'

import { motion } from 'framer-motion'
import { useEffect, useState } from 'react'

interface ProgressRingProps {
  percentage: number
  size?: number
  strokeWidth?: number
  isAnimated?: boolean
  showText?: boolean
  className?: string
}

const ProgressRing = ({
  percentage,
  size = 80,
  strokeWidth = 4,
  isAnimated = false,
  showText = true,
  className = ''
}: ProgressRingProps) => {
  const radius = (size - strokeWidth) / 2
  const circumference = 2 * Math.PI * radius
  const strokeDashoffset = circumference - (percentage / 100) * circumference

  return (
    <div className={`relative inline-block ${className}`}>
      <svg width={size} height={size}>
        <defs>
          <linearGradient id="progressGradient" x1="0%" y1="0%" x2="100%" y2="100%">
            <stop offset="0%" stopColor="#00d4ff" />
            <stop offset="100%" stopColor="#10B981" />
          </linearGradient>
        </defs>
        
        {/* Background circle */}
        <circle
          cx={size / 2}
          cy={size / 2}
          r={radius}
          stroke="rgba(255,255,255,0.1)"
          strokeWidth={strokeWidth}
          fill="none"
        />
        
        {/* Progress circle */}
        <motion.circle
          cx={size / 2}
          cy={size / 2}
          r={radius}
          stroke="url(#progressGradient)"
          strokeWidth={strokeWidth}
          fill="none"
          strokeLinecap="round"
          strokeDasharray={circumference}
          initial={{ strokeDashoffset: circumference }}
          animate={isAnimated ? { strokeDashoffset } : { strokeDashoffset: circumference }}
          transition={{ duration: 1.5, ease: 'easeInOut', delay: 0.2 }}
          style={{ 
            transform: 'rotate(-90deg)', 
            transformOrigin: '50% 50%' 
          }}
        />
      </svg>
      
      {showText && (
        <motion.div
          className="absolute inset-0 flex items-center justify-center"
          initial={{ opacity: 0 }}
          animate={isAnimated ? { opacity: 1 } : { opacity: 0 }}
          transition={{ delay: 0.5 }}
        >
          <span className="text-lg font-bold" style={{ color: '#00d4ff' }}>
            {percentage}%
          </span>
        </motion.div>
      )}
    </div>
  )
}

export default ProgressRing
```