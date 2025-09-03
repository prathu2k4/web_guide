# Step 7: Hero Section - Complete Implementation

## 7.1 Hero Component (src/components/sections/Hero.tsx)

```tsx
'use client'

import { useEffect, useState } from 'react'
import { motion } from 'framer-motion'
import { ChevronDown, Github, Linkedin, Mail } from 'lucide-react'
import { Button } from '@/components/ui/Button'
import { useTypingAnimation } from '@/lib/hooks/useTypingAnimation'
import { useSmoothScroll } from '@/lib/hooks/useSmoothScroll'
import { useMousePosition } from '@/lib/hooks/useMousePosition'
import { socialLinks } from '@/lib/data'
import styles from './Hero.module.css'

const Hero = () => {
  const { displayText, showCursor } = useTypingAnimation("Pratham Jain V S", 120, 1500)
  const { scrollToElement } = useSmoothScroll()
  const mousePosition = useMousePosition()

  const handleScrollToProjects = () => {
    scrollToElement('#projects')
  }

  const handleScrollToContact = () => {
    scrollToElement('#contact')
  }

  const getSocialIcon = (platform: string) => {
    switch (platform.toLowerCase()) {
      case 'github':
        return <Github size={24} />
      case 'linkedin':
        return <Linkedin size={24} />
      case 'email':
        return <Mail size={24} />
      default:
        return <span>{platform}</span>
    }
  }

  return (
    <section className={styles.hero} id="home">
      {/* Background with animated gradient */}
      <div className={styles.heroBackground}>
        <motion.div 
          className={styles.heroGradient}
          animate={{ 
            backgroundPosition: ['0% 50%', '100% 50%', '0% 50%']
          }}
          transition={{
            duration: 8,
            repeat: Infinity,
            ease: 'easeInOut'
          }}
        />
        <FloatingShapes mousePosition={mousePosition} />
      </div>
      
      <div className="container">
        <div className={styles.heroContent}>
          <motion.div 
            className={styles.heroText}
            initial={{ opacity: 0, y: 50 }}
            animate={{ opacity: 1, y: 0 }}
            transition={{ duration: 1, ease: "easeOut" }}
          >
            {/* Main Title with Typing Animation */}
            <h1 className={styles.heroTitle}>
              <motion.span 
                className={styles.titleLine}
                initial={{ opacity: 0, x: -50 }}
                animate={{ opacity: 1, x: 0 }}
                transition={{ duration: 0.8, delay: 0.5 }}
              >
                Hi, I'm
              </motion.span>
              <span className={`${styles.titleLine} ${styles.gradientText}`}>
                {displayText}
                <motion.span
                  className={styles.cursor}
                  animate={{ opacity: showCursor ? [1, 0, 1] : 1 }}
                  transition={{ 
                    duration: 1, 
                    repeat: Infinity,
                    repeatType: 'loop'
                  }}
                >
                  |
                </motion.span>
              </span>
            </h1>
            
            {/* Subtitle */}
            <motion.h2 
              className={styles.heroSubtitle}
              initial={{ opacity: 0, y: 30 }}
              animate={{ opacity: 1, y: 0 }}
              transition={{ duration: 0.8, delay: 1.5 }}
            >
              Digital Circuit Designer • Creative Developer
            </motion.h2>
            
            {/* Description */}
            <motion.p 
              className={styles.heroDescription}
              initial={{ opacity: 0, y: 30 }}
              animate={{ opacity: 1, y: 0 }}
              transition={{ duration: 0.8, delay: 2 }}
            >
              Blending technical expertise with innovative problem-solving to create 
              impactful digital design solutions. Specializing in RTL development, 
              VLSI design, and creative programming at UVCE Bengaluru.
            </motion.p>
            
            {/* CTA Buttons */}
            <motion.div 
              className={styles.heroButtons}
              initial={{ opacity: 0, y: 30 }}
              animate={{ opacity: 1, y: 0 }}
              transition={{ duration: 0.8, delay: 2.5 }}
            >
              <Button
                variant="primary"
                size="lg"
                glow
                onClick={handleScrollToProjects}
                className={styles.heroBtn}
              >
                View My Work
              </Button>
              <Button
                variant="outline"
                size="lg"
                onClick={handleScrollToContact}
                className={styles.heroBtn}
              >
                Get In Touch
              </Button>
            </motion.div>

            {/* Social Links */}
            <motion.div
              className={styles.heroSocial}
              initial={{ opacity: 0, y: 30 }}
              animate={{ opacity: 1, y: 0 }}
              transition={{ duration: 0.8, delay: 3 }}
            >
              {socialLinks.map((link, index) => (
                <motion.a
                  key={link.platform}
                  href={link.url}
                  target="_blank"
                  rel="noopener noreferrer"
                  className={styles.socialLink}
                  initial={{ opacity: 0, scale: 0 }}
                  animate={{ opacity: 1, scale: 1 }}
                  transition={{ 
                    duration: 0.5, 
                    delay: 3.2 + (index * 0.1),
                    type: 'spring',
                    stiffness: 200
                  }}
                  whileHover={{ 
                    y: -5, 
                    scale: 1.1,
                    boxShadow: '0 10px 30px rgba(0, 212, 255, 0.3)'
                  }}
                  whileTap={{ scale: 0.9 }}
                  aria-label={`Visit ${link.platform}`}
                >
                  {getSocialIcon(link.platform)}
                </motion.a>
              ))}
            </motion.div>
          </motion.div>
        </div>
      </div>
      
      {/* Scroll Indicator */}
      <ScrollIndicator />
    </section>
  )
}

// Floating Shapes Component
interface FloatingShapesProps {
  mousePosition: { x: number; y: number }
}

const FloatingShapes = ({ mousePosition }: FloatingShapesProps) => {
  const shapes = [
    { size: 80, top: '20%', left: '10%', delay: 0 },
    { size: 120, top: '60%', right: '15%', delay: 2 },
    { size: 60, top: '10%', right: '30%', delay: 4 },
    { size: 100, bottom: '20%', left: '20%', delay: 1 }
  ]

  return (
    <div className={styles.floatingShapes}>
      {shapes.map((shape, index) => (
        <motion.div
          key={index}
          className={`${styles.shape} ${styles[`shape${index + 1}`]}`}
          animate={{
            y: [0, -20, 20, 0],
            rotate: [0, 120, 240, 360],
            x: mousePosition.x * (0.01 + index * 0.002),
          }}
          transition={{
            y: {
              duration: 6,
              repeat: Infinity,
              ease: "easeInOut",
              delay: shape.delay,
            },
            rotate: {
              duration: 8,
              repeat: Infinity,
              ease: "linear",
              delay: shape.delay,
            },
            x: {
              duration: 0.3,
              ease: "easeOut"
            }
          }}
          style={{
            width: shape.size,
            height: shape.size,
            position: 'absolute',
            ...shape
          }}
        />
      ))}
    </div>
  )
}

// Scroll Indicator Component
const ScrollIndicator = () => {
  return (
    <motion.div 
      className={styles.scrollIndicator}
      initial={{ opacity: 0, y: 20 }}
      animate={{ opacity: 1, y: 0 }}
      transition={{ duration: 0.8, delay: 3.5 }}
    >
      <motion.div 
        className={styles.mouse}
        animate={{ y: [0, -10, 0] }}
        transition={{ 
          duration: 2, 
          repeat: Infinity,
          ease: 'easeInOut'
        }}
      >
        <motion.div 
          className={styles.wheel}
          animate={{
            y: [0, 12, 0],
            opacity: [1, 0.5, 1]
          }}
          transition={{
            duration: 2,
            repeat: Infinity,
            ease: 'easeInOut'
          }}
        />
      </motion.div>
      <motion.span
        initial={{ opacity: 0 }}
        animate={{ opacity: 1 }}
        transition={{ delay: 4 }}
      >
        Scroll Down
      </motion.span>
      <motion.div
        animate={{ y: [0, 5, 0] }}
        transition={{ duration: 1.5, repeat: Infinity }}
      >
        <ChevronDown size={16} />
      </motion.div>
    </motion.div>
  )
}

export default Hero
```

## 7.2 Hero Styles (src/components/sections/Hero.module.css)

```css
.hero {
  min-height: 100vh;
  position: relative;
  display: flex;
  align-items: center;
  overflow: hidden;
}

.heroBackground {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  z-index: 1;
}

.heroGradient {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: linear-gradient(135deg,
    #1a202c 0%,
    #2d3748 25%,
    #1a202c 50%,
    #2d3748 75%,
    #1a202c 100%);
  background-size: 400% 400%;
}

.floatingShapes {
  position: absolute;
  width: 100%;
  height: 100%;
  overflow: hidden;
  pointer-events: none;
}

.shape {
  border-radius: 50%;
  filter: blur(1px);
}

.shape1 {
  background: linear-gradient(135deg, rgba(0, 212, 255, 0.1), rgba(16, 185, 129, 0.1));
}

.shape2 {
  background: linear-gradient(135deg, rgba(139, 92, 246, 0.1), rgba(0, 212, 255, 0.1));
}

.shape3 {
  background: linear-gradient(135deg, rgba(16, 185, 129, 0.1), rgba(139, 92, 246, 0.1));
}

.shape4 {
  background: linear-gradient(135deg, rgba(0, 212, 255, 0.1), rgba(139, 92, 246, 0.1));
}

.heroContent {
  position: relative;
  z-index: 2;
  max-width: 800px;
  width: 100%;
}

.heroText {
  text-align: left;
}

.heroTitle {
  font-size: clamp(3rem, 8vw, 6rem);
  font-weight: var(--font-weight-bold);
  margin-bottom: var(--space-24);
  line-height: 1.1;
}

.titleLine {
  display: block;
  margin-bottom: var(--space-8);
}

.gradientText {
  background: linear-gradient(135deg, #00d4ff 0%, #10B981 50%, #8B5CF6 100%);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
  background-size: 200% 200%;
  position: relative;
}

.cursor {
  color: #00d4ff;
  font-weight: normal;
  margin-left: 2px;
}

.heroSubtitle {
  font-size: var(--font-size-2xl);
  margin-bottom: var(--space-24);
  font-weight: var(--font-weight-medium);
  color: var(--color-gray-200);
}

.heroDescription {
  font-size: var(--font-size-lg);
  line-height: 1.7;
  margin-bottom: var(--space-32);
  color: var(--color-gray-300);
  max-width: 600px;
}

.heroButtons {
  display: flex;
  gap: var(--space-20);
  margin-bottom: var(--space-32);
  flex-wrap: wrap;
}

.heroBtn {
  min-width: 180px;
}

.heroSocial {
  display: flex;
  gap: var(--space-20);
  margin-bottom: var(--space-32);
}

.socialLink {
  display: flex;
  align-items: center;
  justify-content: center;
  width: 56px;
  height: 56px;
  border-radius: var(--radius-full);
  background: rgba(0, 212, 255, 0.1);
  border: 1px solid rgba(0, 212, 255, 0.3);
  color: #00d4ff;
  text-decoration: none;
  transition: all 0.4s ease;
  backdrop-filter: blur(10px);
}

.socialLink:hover {
  background: rgba(0, 212, 255, 0.2);
  color: #00d4ff;
}

.scrollIndicator {
  position: absolute;
  bottom: var(--space-32);
  left: 50%;
  transform: translateX(-50%);
  text-align: center;
  color: var(--color-gray-300);
  font-size: var(--font-size-sm);
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: var(--space-8);
}

.mouse {
  width: 28px;
  height: 48px;
  border: 2px solid #00d4ff;
  border-radius: 14px;
  position: relative;
  margin-bottom: var(--space-8);
}

.wheel {
  width: 4px;
  height: 8px;
  background: #00d4ff;
  border-radius: 2px;
  position: absolute;
  top: 8px;
  left: 50%;
  transform: translateX(-50%);
}

/* Light theme adjustments */
[data-theme="light"] .heroGradient {
  background: linear-gradient(135deg,
    var(--color-cream-50) 0%,
    var(--color-cream-100) 25%,
    var(--color-cream-50) 50%,
    var(--color-cream-100) 75%,
    var(--color-cream-50) 100%);
}

[data-theme="light"] .heroSubtitle {
  color: var(--color-text-secondary);
}

[data-theme="light"] .heroDescription {
  color: var(--color-text-secondary);
}

[data-theme="light"] .socialLink {
  background: rgba(33, 128, 141, 0.1);
  border: 1px solid rgba(33, 128, 141, 0.3);
  color: var(--color-primary);
}

[data-theme="light"] .socialLink:hover {
  background: rgba(33, 128, 141, 0.2);
  color: var(--color-primary);
}

[data-theme="light"] .mouse {
  border-color: var(--color-primary);
}

[data-theme="light"] .wheel {
  background: var(--color-primary);
}

/* Responsive Design */
@media (max-width: 768px) {
  .heroTitle {
    font-size: clamp(2.5rem, 6vw, 4rem);
    text-align: center;
  }

  .heroText {
    text-align: center;
  }

  .heroButtons {
    flex-direction: column;
    align-items: center;
    width: 100%;
  }

  .heroBtn {
    width: 100%;
    max-width: 300px;
  }

  .heroSocial {
    justify-content: center;
  }

  .heroDescription {
    text-align: center;
  }
}

@media (max-width: 640px) {
  .heroButtons {
    gap: var(--space-16);
  }

  .heroSocial {
    gap: var(--space-16);
  }

  .socialLink {
    width: 48px;
    height: 48px;
  }

  .scrollIndicator {
    bottom: var(--space-24);
  }
}

/* Hover effects */
@media (hover: hover) {
  .heroContent:hover .shape {
    animation-play-state: paused;
  }
}

/* High performance mode */
@media (prefers-reduced-motion: reduce) {
  .shape {
    animation: none;
  }
  
  .heroGradient {
    animation: none;
  }
  
  .cursor {
    animation: none;
    opacity: 1;
  }
}
```

## 7.3 Hero Floating Shapes Animation

```tsx
// Additional component for the Hero.tsx file (add this to the Hero component)

interface FloatingShapesProps {
  mousePosition: { x: number; y: number }
}

const FloatingShapes = ({ mousePosition }: FloatingShapesProps) => {
  const shapes = [
    { 
      size: 80, 
      initialPosition: { top: '20%', left: '10%' },
      colors: 'linear-gradient(135deg, rgba(0, 212, 255, 0.1), rgba(16, 185, 129, 0.1))',
      delay: 0 
    },
    { 
      size: 120, 
      initialPosition: { top: '60%', right: '15%' },
      colors: 'linear-gradient(135deg, rgba(139, 92, 246, 0.1), rgba(0, 212, 255, 0.1))',
      delay: 2 
    },
    { 
      size: 60, 
      initialPosition: { top: '10%', right: '30%' },
      colors: 'linear-gradient(135deg, rgba(16, 185, 129, 0.1), rgba(139, 92, 246, 0.1))',
      delay: 4 
    },
    { 
      size: 100, 
      initialPosition: { bottom: '20%', left: '20%' },
      colors: 'linear-gradient(135deg, rgba(0, 212, 255, 0.1), rgba(139, 92, 246, 0.1))',
      delay: 1 
    }
  ]

  return (
    <div className={styles.floatingShapes}>
      {shapes.map((shape, index) => (
        <motion.div
          key={index}
          className={`${styles.shape} ${styles[`shape${index + 1}`]}`}
          animate={{
            y: [0, -20, 20, 0],
            rotate: [0, 120, 240, 360],
            x: mousePosition.x * (0.005 + index * 0.002) - (shape.size / 2),
          }}
          transition={{
            y: {
              duration: 6,
              repeat: Infinity,
              ease: "easeInOut",
              delay: shape.delay,
            },
            rotate: {
              duration: 8,
              repeat: Infinity,
              ease: "linear",
              delay: shape.delay,
            },
            x: {
              duration: 0.3,
              ease: "easeOut"
            }
          }}
          style={{
            width: shape.size,
            height: shape.size,
            position: 'absolute',
            borderRadius: '50%',
            background: shape.colors,
            filter: 'blur(1px)',
            ...shape.initialPosition
          }}
        />
      ))}
    </div>
  )
}

// Button Ripple Effect Hook (add to Hero.tsx)
export const useRipple = () => {
  const createRipple = (event: React.MouseEvent<HTMLButtonElement>) => {
    const button = event.currentTarget
    const rect = button.getBoundingClientRect()
    const size = Math.max(rect.width, rect.height)
    const x = event.clientX - rect.left - size / 2
    const y = event.clientY - rect.top - size / 2
    
    const ripple = document.createElement('span')
    ripple.className = 'btn__ripple'
    ripple.style.width = ripple.style.height = size + 'px'
    ripple.style.left = x + 'px'
    ripple.style.top = y + 'px'
    
    button.appendChild(ripple)
    
    setTimeout(() => {
      button.removeChild(ripple)
    }, 600)
  }
  
  return createRipple
}
```