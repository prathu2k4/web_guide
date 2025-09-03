# Step 6: Layout Components - Navigation & Structure

## 6.1 Header Component (src/components/layout/Header.tsx)

```tsx
'use client'

import { useState, useEffect } from 'react'
import { motion } from 'framer-motion'
import Navigation from './Navigation'
import { useScrollAnimation } from '@/lib/hooks/useScrollAnimation'
import styles from './Header.module.css'

const Header = () => {
  const { isScrolled } = useScrollAnimation()

  return (
    <motion.header
      className={`${styles.header} ${isScrolled ? styles.scrolled : ''}`}
      initial={{ y: -100 }}
      animate={{ y: 0 }}
      transition={{ duration: 0.5 }}
    >
      <Navigation />
    </motion.header>
  )
}

export default Header
```

## 6.2 Header Styles (src/components/layout/Header.module.css)

```css
.header {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  background: rgba(26, 32, 44, 0.95);
  backdrop-filter: blur(20px);
  border-bottom: 1px solid rgba(0, 212, 255, 0.2);
  z-index: 1000;
  transition: all 0.3s ease;
}

.header.scrolled {
  background: rgba(26, 32, 44, 0.98);
  box-shadow: 0 4px 32px rgba(0, 0, 0, 0.3);
}

[data-theme="light"] .header {
  background: rgba(255, 255, 255, 0.95);
  border-bottom: 1px solid rgba(0, 0, 0, 0.1);
}

[data-theme="light"] .header.scrolled {
  background: rgba(255, 255, 255, 0.98);
  box-shadow: 0 4px 32px rgba(0, 0, 0, 0.1);
}
```

## 6.3 Navigation Component (src/components/layout/Navigation.tsx)

```tsx
'use client'

import { useState, useEffect } from 'react'
import { motion } from 'framer-motion'
import { Menu, X } from 'lucide-react'
import ThemeToggle from '@/components/ui/ThemeToggle'
import { useSmoothScroll } from '@/lib/hooks/useSmoothScroll'
import { navigationItems } from '@/lib/data'
import styles from './Navigation.module.css'

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

  const handleMobileMenuToggle = () => {
    setIsMobileMenuOpen(!isMobileMenuOpen)
  }

  // Close mobile menu on outside click
  useEffect(() => {
    const handleClickOutside = (event: MouseEvent) => {
      const target = event.target as HTMLElement
      if (isMobileMenuOpen && !target.closest(`.${styles.navMenu}`) && !target.closest(`.${styles.navToggle}`)) {
        setIsMobileMenuOpen(false)
      }
    }

    document.addEventListener('click', handleClickOutside)
    return () => document.removeEventListener('click', handleClickOutside)
  }, [isMobileMenuOpen])

  return (
    <nav className={styles.nav}>
      <div className="container">
        <div className={styles.navContent}>
          {/* Logo */}
          <motion.a
            href="#home"
            className={styles.navLogo}
            onClick={(e) => {
              e.preventDefault()
              handleNavClick('#home')
            }}
            whileHover={{ scale: 1.05 }}
            whileTap={{ scale: 0.95 }}
          >
            <span className="logo-gradient">Pratham</span>
          </motion.a>

          {/* Desktop Menu */}
          <div className={`${styles.navMenu} ${isMobileMenuOpen ? styles.active : ''}`}>
            {navigationItems.map((item) => (
              <motion.a
                key={item.href}
                href={item.href}
                className={`${styles.navLink} ${activeSection === item.href.substring(1) ? styles.active : ''}`}
                onClick={(e) => {
                  e.preventDefault()
                  handleNavClick(item.href)
                }}
                whileHover={{ y: -2 }}
                whileTap={{ y: 0 }}
              >
                {item.label}
              </motion.a>
            ))}
          </div>

          {/* Actions */}
          <div className={styles.navActions}>
            <ThemeToggle />
            
            {/* Mobile Menu Toggle */}
            <button
              className={`${styles.navToggle} ${isMobileMenuOpen ? styles.active : ''}`}
              onClick={handleMobileMenuToggle}
              aria-label="Toggle mobile menu"
              aria-expanded={isMobileMenuOpen}
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
      </div>
    </nav>
  )
}

export default Navigation
```

## 6.4 Navigation Styles (src/components/layout/Navigation.module.css)

```css
.nav {
  width: 100%;
}

.navContent {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: var(--space-16) 0;
}

.navLogo {
  font-size: var(--font-size-2xl);
  font-weight: var(--font-weight-bold);
  text-decoration: none;
  z-index: 10;
}

.navMenu {
  display: flex;
  gap: var(--space-32);
  align-items: center;
}

.navLink {
  color: var(--color-gray-300);
  font-weight: var(--font-weight-medium);
  transition: all 0.3s ease;
  position: relative;
  padding: var(--space-8) var(--space-16);
  border-radius: var(--radius-full);
  text-decoration: none;
}

.navLink:hover,
.navLink.active {
  color: #00d4ff;
  background: rgba(0, 212, 255, 0.1);
}

.navLink::before {
  content: '';
  position: absolute;
  bottom: -2px;
  left: 50%;
  width: 0;
  height: 2px;
  background: linear-gradient(90deg, #00d4ff, #10B981);
  transition: all 0.3s ease;
  transform: translateX(-50%);
}

.navLink.active::before,
.navLink:hover::before {
  width: 80%;
}

.navActions {
  display: flex;
  align-items: center;
  gap: var(--space-16);
  z-index: 10;
}

.navToggle {
  display: none;
  background: none;
  border: none;
  cursor: pointer;
  padding: var(--space-8);
  color: #00d4ff;
  border-radius: var(--radius-base);
  transition: all 0.3s ease;
}

.navToggle:hover {
  background: rgba(0, 212, 255, 0.1);
}

/* Theme Toggle Styles */
.navContent :global(.theme-toggle) {
  background: rgba(0, 212, 255, 0.1);
  border: 1px solid rgba(0, 212, 255, 0.3);
  border-radius: var(--radius-full);
  width: 44px;
  height: 44px;
  display: flex;
  align-items: center;
  justify-content: center;
  cursor: pointer;
  transition: all 0.3s ease;
  backdrop-filter: blur(10px);
  color: #00d4ff;
}

.navContent :global(.theme-toggle):hover {
  background: rgba(0, 212, 255, 0.2);
  transform: scale(1.1);
  box-shadow: 0 0 20px rgba(0, 212, 255, 0.3);
}

/* Mobile Styles */
@media (max-width: 768px) {
  .navToggle {
    display: flex;
    align-items: center;
    justify-content: center;
  }

  .navMenu {
    position: fixed;
    top: 100%;
    left: 0;
    right: 0;
    background: rgba(26, 32, 44, 0.98);
    backdrop-filter: blur(20px);
    border-top: 1px solid rgba(0, 212, 255, 0.2);
    flex-direction: column;
    padding: var(--space-24);
    gap: var(--space-16);
    transform: translateY(-100%);
    opacity: 0;
    visibility: hidden;
    transition: all 0.3s ease;
    z-index: 5;
  }

  .navMenu.active {
    transform: translateY(0);
    opacity: 1;
    visibility: visible;
  }

  .navLink {
    width: 100%;
    text-align: center;
    padding: var(--space-12) var(--space-20);
  }
}

/* Light theme mobile menu */
[data-theme="light"] .navMenu {
  background: rgba(255, 255, 255, 0.98);
  border-top: 1px solid rgba(0, 0, 0, 0.1);
}

/* Tablet Styles */
@media (max-width: 1024px) {
  .navMenu {
    gap: var(--space-24);
  }
}

/* Small mobile styles */
@media (max-width: 480px) {
  .navContent {
    padding: var(--space-12) 0;
  }
  
  .navLogo {
    font-size: var(--font-size-xl);
  }
  
  .navActions {
    gap: var(--space-12);
  }
  
  .navContent :global(.theme-toggle) {
    width: 40px;
    height: 40px;
  }
}
```

## 6.5 Footer Component (src/components/layout/Footer.tsx)

```tsx
'use client'

import { motion } from 'framer-motion'
import { Github, Linkedin, Mail, Heart } from 'lucide-react'
import { socialLinks } from '@/lib/data'
import styles from './Footer.module.css'

const Footer = () => {
  const currentYear = new Date().getFullYear()

  const getSocialIcon = (platform: string) => {
    switch (platform.toLowerCase()) {
      case 'github':
        return <Github size={20} />
      case 'linkedin':
        return <Linkedin size={20} />
      case 'email':
        return <Mail size={20} />
      default:
        return <span>{platform[0]}</span>
    }
  }

  return (
    <footer className={styles.footer}>
      <div className="container">
        <div className={styles.footerContent}>
          <div className={styles.footerInfo}>
            <motion.div
              initial={{ opacity: 0, y: 20 }}
              whileInView={{ opacity: 1, y: 0 }}
              transition={{ duration: 0.6 }}
              viewport={{ once: true }}
            >
              <p className={styles.footerText}>
                © {currentYear} Pratham Jain V S. Built with{' '}
                <motion.span
                  animate={{ scale: [1, 1.2, 1] }}
                  transition={{ duration: 1.5, repeat: Infinity }}
                  style={{ display: 'inline-block' }}
                >
                  <Heart size={16} fill="currentColor" />
                </motion.span>{' '}
                using Next.js & Framer Motion
              </p>
            </motion.div>
          </div>

          <div className={styles.footerSocial}>
            {socialLinks.map((link, index) => (
              <motion.a
                key={link.platform}
                href={link.url}
                target="_blank"
                rel="noopener noreferrer"
                className={styles.footerSocialLink}
                initial={{ opacity: 0, y: 20 }}
                whileInView={{ opacity: 1, y: 0 }}
                transition={{ duration: 0.6, delay: index * 0.1 }}
                viewport={{ once: true }}
                whileHover={{ y: -2, scale: 1.05 }}
                whileTap={{ scale: 0.95 }}
                aria-label={`Visit ${link.platform}`}
              >
                {getSocialIcon(link.platform)}
              </motion.a>
            ))}
          </div>
        </div>
        
        <motion.div
          className={styles.footerDivider}
          initial={{ scaleX: 0 }}
          whileInView={{ scaleX: 1 }}
          transition={{ duration: 0.8 }}
          viewport={{ once: true }}
        />
        
        <motion.div
          className={styles.footerBottom}
          initial={{ opacity: 0 }}
          whileInView={{ opacity: 1 }}
          transition={{ duration: 0.6, delay: 0.3 }}
          viewport={{ once: true }}
        >
          <p className={styles.footerBottomText}>
            Designed & Developed with passion for innovation
          </p>
        </motion.div>
      </div>
    </footer>
  )
}

export default Footer
```

## 6.6 Footer Styles (src/components/layout/Footer.module.css)

```css
.footer {
  background: linear-gradient(135deg, #1a202c 0%, #2d3748 100%);
  border-top: 1px solid rgba(0, 212, 255, 0.2);
  padding: var(--space-32) 0;
  margin-top: auto;
}

.footerContent {
  display: flex;
  justify-content: space-between;
  align-items: center;
  gap: var(--space-24);
}

.footerInfo {
  flex: 1;
}

.footerText {
  color: var(--color-gray-300);
  margin: 0;
  font-size: var(--font-size-base);
  display: flex;
  align-items: center;
  gap: var(--space-4);
}

.footerSocial {
  display: flex;
  gap: var(--space-16);
}

.footerSocialLink {
  display: flex;
  align-items: center;
  justify-content: center;
  width: 40px;
  height: 40px;
  background: rgba(0, 212, 255, 0.1);
  border: 1px solid rgba(0, 212, 255, 0.2);
  border-radius: var(--radius-base);
  color: #00d4ff;
  text-decoration: none;
  transition: all 0.3s ease;
  backdrop-filter: blur(10px);
}

.footerSocialLink:hover {
  background: rgba(0, 212, 255, 0.2);
  border-color: rgba(0, 212, 255, 0.4);
  box-shadow: 0 4px 20px rgba(0, 212, 255, 0.2);
}

.footerDivider {
  height: 1px;
  background: linear-gradient(90deg, transparent, #00d4ff, transparent);
  margin: var(--space-24) 0;
  transform-origin: center;
}

.footerBottom {
  text-align: center;
}

.footerBottomText {
  color: var(--color-gray-400);
  margin: 0;
  font-size: var(--font-size-sm);
  font-style: italic;
}

/* Light theme */
[data-theme="light"] .footer {
  background: linear-gradient(135deg, var(--color-cream-100) 0%, var(--color-cream-50) 100%);
  border-top: 1px solid var(--color-border);
}

[data-theme="light"] .footerText {
  color: var(--color-text-secondary);
}

[data-theme="light"] .footerBottomText {
  color: var(--color-text-secondary);
}

[data-theme="light"] .footerSocialLink {
  background: rgba(33, 128, 141, 0.1);
  border: 1px solid rgba(33, 128, 141, 0.2);
  color: var(--color-primary);
}

[data-theme="light"] .footerSocialLink:hover {
  background: rgba(33, 128, 141, 0.2);
  border-color: rgba(33, 128, 141, 0.4);
  box-shadow: 0 4px 20px rgba(33, 128, 141, 0.2);
}

/* Mobile Styles */
@media (max-width: 768px) {
  .footerContent {
    flex-direction: column;
    text-align: center;
    gap: var(--space-20);
  }
  
  .footerText {
    justify-content: center;
    flex-wrap: wrap;
  }
  
  .footerSocial {
    gap: var(--space-12);
  }
  
  .footerSocialLink {
    width: 36px;
    height: 36px;
  }
}

@media (max-width: 480px) {
  .footer {
    padding: var(--space-24) 0;
  }
  
  .footerText {
    font-size: var(--font-size-sm);
  }
  
  .footerBottomText {
    font-size: var(--font-size-xs);
  }
}
```

## 6.7 Back to Top Component (src/components/layout/BackToTop.tsx)

```tsx
'use client'

import { useState, useEffect } from 'react'
import { motion, AnimatePresence } from 'framer-motion'
import { ChevronUp } from 'lucide-react'
import { useSmoothScroll } from '@/lib/hooks/useSmoothScroll'
import styles from './BackToTop.module.css'

const BackToTop = () => {
  const [isVisible, setIsVisible] = useState(false)
  const { scrollToTop } = useSmoothScroll()

  useEffect(() => {
    const toggleVisibility = () => {
      if (window.pageYOffset > 300) {
        setIsVisible(true)
      } else {
        setIsVisible(false)
      }
    }

    window.addEventListener('scroll', toggleVisibility)
    
    return () => {
      window.removeEventListener('scroll', toggleVisibility)
    }
  }, [])

  return (
    <AnimatePresence>
      {isVisible && (
        <motion.button
          className={styles.backToTop}
          onClick={scrollToTop}
          initial={{ opacity: 0, y: 20 }}
          animate={{ opacity: 1, y: 0 }}
          exit={{ opacity: 0, y: 20 }}
          whileHover={{ 
            scale: 1.1, 
            y: -5,
            boxShadow: '0 12px 40px rgba(0, 212, 255, 0.4)'
          }}
          whileTap={{ scale: 0.9 }}
          transition={{ duration: 0.3 }}
          aria-label="Back to top"
        >
          <ChevronUp size={24} />
        </motion.button>
      )}
    </AnimatePresence>
  )
}

export default BackToTop
```

## 6.8 Back to Top Styles (src/components/layout/BackToTop.module.css)

```css
.backToTop {
  position: fixed;
  bottom: var(--space-32);
  right: var(--space-32);
  width: 56px;
  height: 56px;
  background: linear-gradient(135deg, #00d4ff, #10B981);
  color: var(--color-charcoal-800);
  border: none;
  border-radius: var(--radius-full);
  cursor: pointer;
  box-shadow: 0 8px 32px rgba(0, 212, 255, 0.3);
  z-index: 100;
  display: flex;
  align-items: center;
  justify-content: center;
  transition: all 0.4s ease;
}

.backToTop:hover {
  background: linear-gradient(135deg, #10B981, #8B5CF6);
}

/* Mobile styles */
@media (max-width: 768px) {
  .backToTop {
    bottom: var(--space-20);
    right: var(--space-20);
    width: 48px;
    height: 48px;
  }
}

/* Light theme */
[data-theme="light"] .backToTop {
  color: var(--color-white);
}
```