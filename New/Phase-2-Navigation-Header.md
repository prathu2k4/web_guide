# Phase 2: Navigation & Header Components

In this phase, you'll add a professional navigation header with smooth scrolling and responsive design. After completing this phase, you'll have a working navigation system that smoothly scrolls to different sections.

## Prerequisites

Complete Phase 1 first. Your project should be running at `http://localhost:3000`.

## Step 1: Create Utility Functions & Hooks

### 1.1 Create Utility Functions

**File: `src/lib/utils.ts`**
```typescript
import { type ClassValue, clsx } from "clsx"

export function cn(...inputs: ClassValue[]) {
  return clsx(inputs)
}

export const throttle = (func: Function, delay: number) => {
  let timeoutId: NodeJS.Timeout | null = null
  let lastExecTime = 0
  
  return (...args: any[]) => {
    const currentTime = Date.now()
    
    if (currentTime - lastExecTime > delay) {
      func(...args)
      lastExecTime = currentTime
    } else {
      if (timeoutId) clearTimeout(timeoutId)
      timeoutId = setTimeout(() => {
        func(...args)
        lastExecTime = Date.now()
      }, delay - (currentTime - lastExecTime))
    }
  }
}
```

### 1.2 Create Smooth Scroll Hook

**File: `src/lib/hooks/useSmoothScroll.ts`**
```typescript
import { useCallback } from 'react'

export function useSmoothScroll() {
  const scrollToElement = useCallback((
    targetId: string,
    duration: number = 800,
    offset: number = 80
  ) => {
    const element = document.querySelector(targetId)
    if (!element) return

    const targetPosition = element.getBoundingClientRect().top + window.pageYOffset - offset
    const startPosition = window.pageYOffset
    const distance = targetPosition - startPosition
    let startTime: number | null = null

    const easeInOutCubic = (t: number): number => {
      return t < 0.5 ? 4 * t * t * t : (t - 1) * (2 * t - 2) * (2 * t - 2) + 1
    }

    const animation = (currentTime: number) => {
      if (startTime === null) startTime = currentTime
      const timeElapsed = currentTime - startTime
      const progress = Math.min(timeElapsed / duration, 1)
      const ease = easeInOutCubic(progress)

      window.scrollTo(0, startPosition + distance * ease)

      if (timeElapsed < duration) {
        requestAnimationFrame(animation)
      }
    }

    requestAnimationFrame(animation)
  }, [])

  return { scrollToElement }
}
```

### 1.3 Create Scroll Animation Hook

**File: `src/lib/hooks/useScrollAnimation.ts`**
```typescript
import { useEffect, useState } from 'react'
import { throttle } from '@/lib/utils'

export function useScrollAnimation() {
  const [scrollY, setScrollY] = useState(0)
  const [scrollDirection, setScrollDirection] = useState<'up' | 'down'>('down')
  const [isScrolled, setIsScrolled] = useState(false)

  useEffect(() => {
    let lastScrollY = window.pageYOffset

    const updateScrollDirection = throttle(() => {
      const scrollY = window.pageYOffset
      const direction = scrollY > lastScrollY ? 'down' : 'up'
      
      if (direction !== scrollDirection && Math.abs(scrollY - lastScrollY) > 5) {
        setScrollDirection(direction)
      }
      
      setScrollY(scrollY)
      setIsScrolled(scrollY > 100)
      lastScrollY = scrollY > 0 ? scrollY : 0
    }, 16)

    window.addEventListener('scroll', updateScrollDirection)
    
    return () => {
      window.removeEventListener('scroll', updateScrollDirection)
    }
  }, [scrollDirection])

  return {
    scrollY,
    scrollDirection,
    isScrolled
  }
}
```

## Step 2: Create Navigation Data

### 2.1 Create Navigation Data

**File: `src/lib/data/navigation.ts`**
```typescript
export const navigationItems = [
  { href: '#home', label: 'Home' },
  { href: '#about', label: 'About' },
  { href: '#skills', label: 'Skills' },
  { href: '#projects', label: 'Projects' },
  { href: '#achievements', label: 'Achievements' },
  { href: '#contact', label: 'Contact' }
]
```

## Step 3: Create Navigation Component

### 3.1 Create Navigation Component

**File: `src/components/layout/Navigation.tsx`**
```tsx
'use client'

import { useState, useEffect } from 'react'
import { motion } from 'framer-motion'
import { Menu, X } from 'lucide-react'
import ThemeToggle from '@/components/ui/ThemeToggle'
import { useSmoothScroll } from '@/lib/hooks/useSmoothScroll'
import { navigationItems } from '@/lib/data/navigation'

const Navigation = () => {
  const [isMobileMenuOpen, setIsMobileMenuOpen] = useState(false)
  const [activeSection, setActiveSection] = useState('home')
  const { scrollToElement } = useSmoothScroll()

  useEffect(() => {
    const handleSectionInView = () => {
      const sections = navigationItems.map(item => item.href.substring(1))
      const currentSection = sections.find(section => {
        const element = document.getElementById(section)
        if (element) {
          const rect = element.getBoundingClientRect()
          return rect.top <= 100 && rect.bottom >= 100
        }
        return false
      })
      
      if (currentSection) {
        setActiveSection(currentSection)
      }
    }

    window.addEventListener('scroll', handleSectionInView)
    
    return () => {
      window.removeEventListener('scroll', handleSectionInView)
    }
  }, [])

  const handleNavClick = (targetId: string) => {
    scrollToElement(targetId)
    setIsMobileMenuOpen(false)
  }

  return (
    <nav style={{
      position: 'fixed',
      top: 0,
      left: 0,
      right: 0,
      background: 'rgba(15, 23, 42, 0.95)',
      backdropFilter: 'blur(20px)',
      borderBottom: '1px solid rgba(56, 189, 248, 0.2)',
      zIndex: 1000,
      padding: 'var(--space-16) 0'
    }}>
      <div className="container">
        <div style={{ 
          display: 'flex', 
          alignItems: 'center', 
          justifyContent: 'space-between' 
        }}>
          {/* Logo */}
          <motion.a
            href="#home"
            onClick={(e) => {
              e.preventDefault()
              handleNavClick('#home')
            }}
            style={{
              fontSize: 'var(--font-size-2xl)',
              fontWeight: 'bold',
              textDecoration: 'none',
              background: 'linear-gradient(135deg, #38bdf8 0%, #10b981 100%)',
              WebkitBackgroundClip: 'text',
              WebkitTextFillColor: 'transparent',
              backgroundClip: 'text',
              cursor: 'pointer'
            }}
            whileHover={{ scale: 1.05 }}
            whileTap={{ scale: 0.95 }}
          >
            Pratham
          </motion.a>

          {/* Desktop Menu */}
          <div style={{ 
            display: 'flex', 
            gap: 'var(--space-32)', 
            alignItems: 'center',
            '@media (max-width: 768px)': {
              display: 'none'
            }
          }} className="desktop-menu">
            {navigationItems.map((item) => (
              <motion.a
                key={item.href}
                href={item.href}
                onClick={(e) => {
                  e.preventDefault()
                  handleNavClick(item.href)
                }}
                style={{
                  color: activeSection === item.href.substring(1) ? '#38bdf8' : '#94a3b8',
                  fontWeight: '500',
                  textDecoration: 'none',
                  padding: 'var(--space-8) var(--space-16)',
                  borderRadius: 'var(--radius-full)',
                  transition: 'all 0.3s ease',
                  position: 'relative',
                  background: activeSection === item.href.substring(1) 
                    ? 'rgba(56, 189, 248, 0.1)' : 'transparent'
                }}
                whileHover={{ y: -2 }}
                whileTap={{ y: 0 }}
              >
                {item.label}
              </motion.a>
            ))}
          </div>

          {/* Actions */}
          <div style={{ 
            display: 'flex', 
            alignItems: 'center', 
            gap: 'var(--space-16)' 
          }}>
            <ThemeToggle />
            
            {/* Mobile Menu Toggle */}
            <button
              onClick={() => setIsMobileMenuOpen(!isMobileMenuOpen)}
              style={{
                display: 'none',
                background: 'none',
                border: 'none',
                cursor: 'pointer',
                padding: 'var(--space-8)',
                color: '#38bdf8',
                borderRadius: 'var(--radius-base)'
              }}
              className="mobile-menu-toggle"
              aria-label="Toggle mobile menu"
            >
              <motion.div
                animate={{ rotate: isMobileMenuOpen ? 180 : 0 }}
                transition={{ duration: 0.3 }}
              >
                {isMobileMenuOpen ? <X size={24} /> : <Menu size={24} />}
              </motion.div>
            </button>
          </div>
        </div>

        {/* Mobile Menu */}
        {isMobileMenuOpen && (
          <motion.div
            initial={{ opacity: 0, y: -20 }}
            animate={{ opacity: 1, y: 0 }}
            exit={{ opacity: 0, y: -20 }}
            style={{
              position: 'absolute',
              top: '100%',
              left: 0,
              right: 0,
              background: 'rgba(15, 23, 42, 0.98)',
              backdropFilter: 'blur(20px)',
              borderTop: '1px solid rgba(56, 189, 248, 0.2)',
              padding: 'var(--space-24)',
              display: 'flex',
              flexDirection: 'column',
              gap: 'var(--space-16)'
            }}
            className="mobile-menu"
          >
            {navigationItems.map((item) => (
              <motion.a
                key={item.href}
                href={item.href}
                onClick={(e) => {
                  e.preventDefault()
                  handleNavClick(item.href)
                }}
                style={{
                  color: activeSection === item.href.substring(1) ? '#38bdf8' : '#94a3b8',
                  fontWeight: '500',
                  textDecoration: 'none',
                  padding: 'var(--space-12) var(--space-20)',
                  textAlign: 'center',
                  borderRadius: 'var(--radius-base)',
                  background: activeSection === item.href.substring(1) 
                    ? 'rgba(56, 189, 248, 0.1)' : 'transparent'
                }}
                whileHover={{ scale: 1.05 }}
                whileTap={{ scale: 0.95 }}
              >
                {item.label}
              </motion.a>
            ))}
          </motion.div>
        )}
      </div>
    </nav>
  )
}

export default Navigation
```

### 3.2 Add Mobile Styles to Global CSS

**Add to `src/app/globals.css`** (append to existing file):
```css
/* Mobile Navigation Styles */
@media (max-width: 768px) {
  .desktop-menu {
    display: none !important;
  }
  
  .mobile-menu-toggle {
    display: flex !important;
    align-items: center;
    justify-content: center;
  }
}

@media (min-width: 769px) {
  .mobile-menu {
    display: none !important;
  }
}
```

## Step 4: Create Header Component

### 4.1 Create Header Component

**File: `src/components/layout/Header.tsx`**
```tsx
'use client'

import { motion } from 'framer-motion'
import Navigation from './Navigation'
import { useScrollAnimation } from '@/lib/hooks/useScrollAnimation'

const Header = () => {
  const { isScrolled } = useScrollAnimation()

  return (
    <motion.header
      initial={{ y: -100 }}
      animate={{ y: 0 }}
      transition={{ duration: 0.5 }}
      style={{
        position: 'fixed',
        top: 0,
        left: 0,
        right: 0,
        zIndex: 1000,
        background: isScrolled 
          ? 'rgba(15, 23, 42, 0.98)' 
          : 'rgba(15, 23, 42, 0.95)',
        backdropFilter: 'blur(20px)',
        borderBottom: '1px solid rgba(56, 189, 248, 0.2)',
        boxShadow: isScrolled 
          ? '0 4px 32px rgba(0, 0, 0, 0.3)' 
          : 'none',
        transition: 'all 0.3s ease'
      }}
    >
      <Navigation />
    </motion.header>
  )
}

export default Header
```

## Step 5: Update Main Page with Sections

### 5.1 Update Main Page

**File: `src/app/page.tsx`**
```tsx
'use client'

import Header from '@/components/layout/Header'

export default function Home() {
  return (
    <>
      <Header />
      
      <main>
        {/* Hero Section */}
        <section 
          id="home" 
          style={{ 
            minHeight: '100vh',
            display: 'flex',
            alignItems: 'center',
            justifyContent: 'center',
            textAlign: 'center',
            padding: 'var(--space-24)',
            background: 'linear-gradient(135deg, rgba(15, 23, 42, 0.9) 0%, rgba(30, 41, 59, 0.9) 100%)'
          }}
        >
          <div className="container">
            <h1 style={{ 
              fontSize: 'clamp(2.5rem, 5vw, 4rem)',
              fontWeight: 'bold',
              marginBottom: 'var(--space-24)',
              background: 'linear-gradient(135deg, #38bdf8 0%, #10b981 50%, #8b5cf6 100%)',
              WebkitBackgroundClip: 'text',
              WebkitTextFillColor: 'transparent',
              backgroundClip: 'text'
            }}>
              Hi, I'm Pratham Jain V S
            </h1>
            <h2 style={{ 
              fontSize: 'var(--font-size-2xl)',
              marginBottom: 'var(--space-24)',
              color: 'var(--color-text)'
            }}>
              Digital Circuit Designer • Creative Developer
            </h2>
            <p style={{ 
              fontSize: 'var(--font-size-lg)',
              maxWidth: '600px',
              margin: '0 auto var(--space-32)',
              lineHeight: '1.7',
              color: 'var(--color-text-secondary)'
            }}>
              Blending technical expertise with innovative problem-solving to create 
              impactful digital design solutions. Specializing in RTL development, 
              VLSI design, and creative programming at UVCE Bengaluru.
            </p>
            <div style={{ 
              display: 'flex', 
              gap: 'var(--space-20)', 
              justifyContent: 'center',
              flexWrap: 'wrap'
            }}>
              <button className="btn btn--primary" style={{ padding: 'var(--space-16) var(--space-32)' }}>
                View My Work
              </button>
              <button 
                className="btn" 
                style={{ 
                  padding: 'var(--space-16) var(--space-32)',
                  background: 'transparent',
                  border: '2px solid var(--color-primary)',
                  color: 'var(--color-primary)'
                }}
              >
                Get In Touch
              </button>
            </div>
          </div>
        </section>

        {/* About Section */}
        <section 
          id="about" 
          style={{ 
            minHeight: '100vh',
            padding: '120px 0',
            background: 'var(--color-background)'
          }}
        >
          <div className="container">
            <div style={{ textAlign: 'center', marginBottom: 'var(--space-32)' }}>
              <h2 style={{ 
                fontSize: 'clamp(2.5rem, 5vw, 4rem)',
                fontWeight: 'bold',
                marginBottom: 'var(--space-16)',
                background: 'linear-gradient(135deg, #38bdf8 0%, #10b981 100%)',
                WebkitBackgroundClip: 'text',
                WebkitTextFillColor: 'transparent',
                backgroundClip: 'text'
              }}>
                About Me
              </h2>
              <p style={{ 
                fontSize: 'var(--font-size-xl)',
                color: 'var(--color-text-secondary)',
                maxWidth: '600px',
                margin: '0 auto'
              }}>
                Passionate engineer with a creative mindset
              </p>
            </div>
            
            <div style={{ 
              display: 'grid', 
              gridTemplateColumns: 'repeat(auto-fit, minmax(300px, 1fr))',
              gap: 'var(--space-32)',
              maxWidth: '1000px',
              margin: '0 auto'
            }}>
              <div style={{
                background: 'var(--color-surface)',
                padding: 'var(--space-32)',
                borderRadius: 'var(--radius-lg)',
                border: '1px solid var(--color-border)'
              }}>
                <h3 style={{ marginBottom: 'var(--space-16)', color: 'var(--color-primary)' }}>
                  My Journey
                </h3>
                <p style={{ lineHeight: '1.7', marginBottom: 'var(--space-16)' }}>
                  Passionate 5th-semester ECE student at UVCE Bengaluru, specializing in 
                  digital design, RTL development, and creative programming.
                </p>
                <p style={{ lineHeight: '1.7' }}>
                  My journey in electronics and communication has been driven by a deep fascination 
                  with how digital systems work at the lowest levels.
                </p>
              </div>
              
              <div style={{
                background: 'var(--color-surface)',
                padding: 'var(--space-32)',
                borderRadius: 'var(--radius-lg)',
                border: '1px solid var(--color-border)'
              }}>
                <h3 style={{ marginBottom: 'var(--space-16)', color: 'var(--color-primary)' }}>
                  Education
                </h3>
                <div style={{ 
                  borderLeft: '2px solid var(--color-primary)',
                  paddingLeft: 'var(--space-16)',
                  position: 'relative'
                }}>
                  <div style={{
                    position: 'absolute',
                    left: '-8px',
                    top: 0,
                    width: '14px',
                    height: '14px',
                    background: 'var(--color-primary)',
                    borderRadius: '50%'
                  }} />
                  <h4 style={{ color: 'var(--color-text)', marginBottom: 'var(--space-8)' }}>
                    Bachelor of Engineering - ECE
                  </h4>
                  <p style={{ color: 'var(--color-primary)', fontWeight: '500', marginBottom: 'var(--space-4)' }}>
                    UVCE Bengaluru
                  </p>
                  <p style={{ color: 'var(--color-text-secondary)', fontSize: '14px', marginBottom: 'var(--space-8)' }}>
                    2022 - 2026
                  </p>
                  <p style={{ color: 'var(--color-text-secondary)' }}>
                    <strong>Focus:</strong> Digital System Design, Signal Processing, VLSI Technology
                  </p>
                </div>
              </div>
            </div>
          </div>
        </section>

        {/* Skills Section */}
        <section 
          id="skills" 
          style={{ 
            minHeight: '100vh',
            padding: '120px 0',
            background: 'linear-gradient(135deg, rgba(30, 41, 59, 0.5) 0%, rgba(15, 23, 42, 0.5) 100%)'
          }}
        >
          <div className="container">
            <div style={{ textAlign: 'center', marginBottom: 'var(--space-32)' }}>
              <h2 style={{ 
                fontSize: 'clamp(2.5rem, 5vw, 4rem)',
                fontWeight: 'bold',
                marginBottom: 'var(--space-16)',
                background: 'linear-gradient(135deg, #38bdf8 0%, #10b981 100%)',
                WebkitBackgroundClip: 'text',
                WebkitTextFillColor: 'transparent',
                backgroundClip: 'text'
              }}>
                Skills & Technologies
              </h2>
              <p style={{ 
                fontSize: 'var(--font-size-xl)',
                color: 'var(--color-text-secondary)'
              }}>
                Cutting-edge tools and technologies
              </p>
            </div>
            
            <div style={{ 
              display: 'grid', 
              gridTemplateColumns: 'repeat(auto-fit, minmax(200px, 1fr))',
              gap: 'var(--space-24)',
              maxWidth: '1000px',
              margin: '0 auto'
            }}>
              {['Verilog HDL', 'Python', 'C++', 'MATLAB', 'Vivado', 'ModelSim'].map((skill) => (
                <div key={skill} style={{
                  background: 'rgba(56, 189, 248, 0.1)',
                  padding: 'var(--space-24)',
                  borderRadius: 'var(--radius-lg)',
                  textAlign: 'center',
                  border: '1px solid rgba(56, 189, 248, 0.2)'
                }}>
                  <div style={{
                    width: '60px',
                    height: '60px',
                    background: 'linear-gradient(135deg, #38bdf8, #10b981)',
                    borderRadius: '50%',
                    display: 'flex',
                    alignItems: 'center',
                    justifyContent: 'center',
                    margin: '0 auto var(--space-16)',
                    fontSize: '24px'
                  }}>
                    ⚡
                  </div>
                  <h4 style={{ color: 'var(--color-text)', marginBottom: 'var(--space-8)' }}>
                    {skill}
                  </h4>
                  <p style={{ color: 'var(--color-text-secondary)', fontSize: '14px' }}>
                    Advanced proficiency
                  </p>
                </div>
              ))}
            </div>
          </div>
        </section>

        {/* Projects Section */}
        <section 
          id="projects" 
          style={{ 
            minHeight: '100vh',
            padding: '120px 0',
            background: 'var(--color-background)'
          }}
        >
          <div className="container">
            <div style={{ textAlign: 'center', marginBottom: 'var(--space-32)' }}>
              <h2 style={{ 
                fontSize: 'clamp(2.5rem, 5vw, 4rem)',
                fontWeight: 'bold',
                marginBottom: 'var(--space-16)',
                background: 'linear-gradient(135deg, #38bdf8 0%, #10b981 100%)',
                WebkitBackgroundClip: 'text',
                WebkitTextFillColor: 'transparent',
                backgroundClip: 'text'
              }}>
                Featured Projects
              </h2>
              <p style={{ 
                fontSize: 'var(--font-size-xl)',
                color: 'var(--color-text-secondary)'
              }}>
                Innovative solutions and technical achievements
              </p>
            </div>
            <p style={{ textAlign: 'center', color: 'var(--color-text-secondary)' }}>
              Project details will be added in the next phase...
            </p>
          </div>
        </section>

        {/* Achievements Section */}
        <section 
          id="achievements" 
          style={{ 
            minHeight: '100vh',
            padding: '120px 0',
            background: 'linear-gradient(135deg, rgba(16, 185, 129, 0.1) 0%, transparent 50%)'
          }}
        >
          <div className="container">
            <div style={{ textAlign: 'center', marginBottom: 'var(--space-32)' }}>
              <h2 style={{ 
                fontSize: 'clamp(2.5rem, 5vw, 4rem)',
                fontWeight: 'bold',
                marginBottom: 'var(--space-16)',
                background: 'linear-gradient(135deg, #38bdf8 0%, #10b981 100%)',
                WebkitBackgroundClip: 'text',
                WebkitTextFillColor: 'transparent',
                backgroundClip: 'text'
              }}>
                Achievements
              </h2>
              <p style={{ 
                fontSize: 'var(--font-size-xl)',
                color: 'var(--color-text-secondary)'
              }}>
                Milestones and accomplishments
              </p>
            </div>
            <p style={{ textAlign: 'center', color: 'var(--color-text-secondary)' }}>
              Achievement details will be added in the next phase...
            </p>
          </div>
        </section>

        {/* Contact Section */}
        <section 
          id="contact" 
          style={{ 
            minHeight: '100vh',
            padding: '120px 0',
            background: 'linear-gradient(135deg, rgba(15, 23, 42, 0.95) 0%, rgba(30, 41, 59, 0.95) 100%)'
          }}
        >
          <div className="container">
            <div style={{ textAlign: 'center', marginBottom: 'var(--space-32)' }}>
              <h2 style={{ 
                fontSize: 'clamp(2.5rem, 5vw, 4rem)',
                fontWeight: 'bold',
                marginBottom: 'var(--space-16)',
                background: 'linear-gradient(135deg, #38bdf8 0%, #10b981 100%)',
                WebkitBackgroundClip: 'text',
                WebkitTextFillColor: 'transparent',
                backgroundClip: 'text'
              }}>
                Let's Connect
              </h2>
              <p style={{ 
                fontSize: 'var(--font-size-xl)',
                color: 'var(--color-text-secondary)'
              }}>
                Ready to collaborate on innovative projects
              </p>
            </div>
            <p style={{ textAlign: 'center', color: 'var(--color-text-secondary)' }}>
              Contact form will be added in the next phase...
            </p>
          </div>
        </section>
      </main>
    </>
  )
}
```

## Step 6: Test Your Navigation

### 6.1 Run and Test
```bash
npm run dev
```

### 6.2 Verify Navigation Works

1. Open `http://localhost:3000`
2. You should see:
   - Fixed navigation header with "Pratham" logo
   - Navigation menu with Home, About, Skills, Projects, Achievements, Contact
   - Smooth scrolling when clicking nav items
   - Active section highlighting in navigation
   - Responsive mobile menu (test by resizing browser)
   - Theme toggle still working
   - Professional-looking sections with proper spacing

### 6.3 Test Mobile Responsiveness

1. Resize browser to mobile width (< 768px)
2. Navigation should show hamburger menu
3. Click hamburger to see mobile menu
4. Click any nav item - it should scroll and close menu

## What You've Accomplished

✅ Created smooth scrolling navigation with active section detection  
✅ Added responsive mobile navigation with hamburger menu  
✅ Implemented scroll-based header effects  
✅ Created professional section layouts  
✅ Added proper spacing and typography  
✅ Enhanced theme switching integration  
✅ Added Framer Motion animations for smooth interactions  

## Next Phase Preview

In Phase 3, we'll add:
- Detailed project cards with data
- Skills with progress indicators
- Achievement cards with animations
- Contact form with validation
- Enhanced animations and micro-interactions

The navigation system is now complete and fully functional!