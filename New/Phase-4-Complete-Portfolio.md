# Phase 4: Achievements, Contact & Footer

In this phase, you'll complete the portfolio by adding the achievements section, contact section, and footer. After completing this phase, you'll have a fully functional portfolio website.

## Prerequisites

Complete Phases 1, 2, and 3 first. Your skills and projects sections should be working properly.

## Step 1: Create Achievements Section

### 1.1 Create Achievements Section Component

**File: `src/components/sections/Achievements.tsx`**
```tsx
'use client'

import { motion } from 'framer-motion'
import { useIntersectionObserver } from '@/lib/hooks/useIntersectionObserver'
import { Card, CardBody } from '@/components/ui/Card'
import { achievementsData } from '@/lib/data'
import { Zap, Award, Users } from 'lucide-react'

const Achievements = () => {
  const { ref: achievementsRef, isVisible } = useIntersectionObserver({
    threshold: 0.1,
    triggerOnce: true
  })

  const containerVariants = {
    hidden: { opacity: 0 },
    visible: {
      opacity: 1,
      transition: {
        delayChildren: 0.3,
        staggerChildren: 0.2
      }
    }
  }

  const itemVariants = {
    hidden: { opacity: 0, y: 50, scale: 0.8 },
    visible: { 
      opacity: 1, 
      y: 0,
      scale: 1,
      transition: {
        duration: 0.6,
        ease: "easeOut"
      }
    }
  }

  const getAchievementIcon = (iconString: string) => {
    switch (iconString) {
      case '🔬':
        return <Zap size={32} />
      case '🧠':
        return <Users size={32} />
      case '🏆':
        return <Award size={32} />
      default:
        return <Award size={32} />
    }
  }

  return (
    <section 
      ref={achievementsRef as React.RefObject<HTMLElement>}
      id="achievements"
      style={{ 
        minHeight: '100vh',
        padding: '120px 0',
        background: 'radial-gradient(ellipse at center, rgba(16, 185, 129, 0.1) 0%, transparent 50%)',
        position: 'relative'
      }}
    >
      <div className="container">
        {/* Section Header */}
        <motion.div
          style={{ 
            textAlign: 'center', 
            marginBottom: 'var(--space-32)',
            maxWidth: '600px',
            margin: '0 auto var(--space-32)'
          }}
          initial="hidden"
          animate={isVisible ? "visible" : "hidden"}
          variants={containerVariants}
        >
          <motion.h2 
            variants={itemVariants}
            style={{ 
              fontSize: 'clamp(2.5rem, 5vw, 4rem)',
              fontWeight: 'bold',
              marginBottom: 'var(--space-16)',
              background: 'linear-gradient(135deg, #38bdf8 0%, #10b981 100%)',
              WebkitBackgroundClip: 'text',
              WebkitTextFillColor: 'transparent',
              backgroundClip: 'text'
            }}
          >
            Achievements
          </motion.h2>
          <motion.p 
            variants={itemVariants}
            style={{ 
              fontSize: 'var(--font-size-xl)',
              color: 'var(--color-text-secondary)'
            }}
          >
            Milestones and accomplishments
          </motion.p>
        </motion.div>

        {/* Achievements Grid */}
        <motion.div
          style={{
            display: 'grid',
            gridTemplateColumns: 'repeat(auto-fit, minmax(350px, 1fr))',
            gap: 'var(--space-32)',
            maxWidth: '1200px',
            margin: '0 auto'
          }}
          initial="hidden"
          animate={isVisible ? "visible" : "hidden"}
          variants={containerVariants}
        >
          {achievementsData.map((achievement, index) => (
            <motion.div
              key={achievement.id}
              variants={itemVariants}
            >
              <Card
                hover
                style={{
                  height: '100%',
                  background: `linear-gradient(135deg, rgba(${index === 0 ? '56, 189, 248' : index === 1 ? '16, 185, 129' : '139, 92, 246'}, 0.05), rgba(${index === 0 ? '16, 185, 129' : index === 1 ? '139, 92, 246' : '56, 189, 248'}, 0.05))`,
                  border: '1px solid rgba(255, 255, 255, 0.1)'
                }}
              >
                <CardBody style={{ 
                  display: 'flex', 
                  flexDirection: 'column', 
                  alignItems: 'center',
                  textAlign: 'center',
                  height: '100%' 
                }}>
                  <motion.div 
                    style={{
                      width: '80px',
                      height: '80px',
                      background: `linear-gradient(135deg, rgba(${index === 0 ? '56, 189, 248' : index === 1 ? '16, 185, 129' : '139, 92, 246'}, 0.2), rgba(${index === 0 ? '16, 185, 129' : index === 1 ? '139, 92, 246' : '56, 189, 248'}, 0.2))`,
                      borderRadius: '50%',
                      display: 'flex',
                      alignItems: 'center',
                      justifyContent: 'center',
                      marginBottom: 'var(--space-20)',
                      color: index === 0 ? '#38bdf8' : index === 1 ? '#10b981' : '#8b5cf6'
                    }}
                    initial={{ scale: 0, rotate: -180 }}
                    animate={isVisible ? { scale: 1, rotate: 0 } : {}}
                    transition={{ 
                      duration: 0.6, 
                      delay: 0.3 + index * 0.1,
                      type: "spring",
                      stiffness: 300
                    }}
                    whileHover={{ 
                      scale: 1.2,
                      rotate: 360
                    }}
                  >
                    {getAchievementIcon(achievement.icon)}
                  </motion.div>
                  
                  <div style={{ flex: 1, display: 'flex', flexDirection: 'column', justifyContent: 'center' }}>
                    <h3 style={{
                      fontSize: 'var(--font-size-xl)',
                      fontWeight: 'bold',
                      color: 'var(--color-text)',
                      marginBottom: 'var(--space-8)'
                    }}>
                      {achievement.title}
                    </h3>
                    
                    <div style={{
                      color: '#10b981',
                      fontWeight: '600',
                      marginBottom: 'var(--space-12)',
                      fontSize: 'var(--font-size-lg)',
                      background: 'linear-gradient(135deg, #10b981, #38bdf8)',
                      WebkitBackgroundClip: 'text',
                      WebkitTextFillColor: 'transparent',
                      backgroundClip: 'text'
                    }}>
                      {achievement.role} • {achievement.period}
                    </div>
                    
                    <p style={{
                      color: 'var(--color-text-secondary)',
                      lineHeight: '1.7',
                      marginBottom: 'var(--space-16)'
                    }}>
                      {achievement.description}
                    </p>
                    
                    {achievement.status === 'ongoing' && (
                      <motion.div
                        style={{
                          background: 'rgba(56, 189, 248, 0.2)',
                          color: '#38bdf8',
                          padding: 'var(--space-6) var(--space-12)',
                          borderRadius: 'var(--radius-full)',
                          fontSize: '12px',
                          fontWeight: '600',
                          textTransform: 'uppercase',
                          letterSpacing: '1px',
                          border: '1px solid rgba(56, 189, 248, 0.3)',
                          alignSelf: 'center'
                        }}
                        animate={{ 
                          boxShadow: [
                            '0 0 10px rgba(56, 189, 248, 0.3)',
                            '0 0 20px rgba(56, 189, 248, 0.6)',
                            '0 0 10px rgba(56, 189, 248, 0.3)'
                          ]
                        }}
                        transition={{ duration: 2, repeat: Infinity }}
                      >
                        Currently Active
                      </motion.div>
                    )}
                  </div>
                </CardBody>
              </Card>
            </motion.div>
          ))}
        </motion.div>
      </div>
    </section>
  )
}

export default Achievements
```

## Step 2: Create Contact Section

### 2.1 Create Contact Data

**File: `src/lib/data/contact.ts`**
```typescript
import { ContactMethod } from '@/types'

export const contactData: ContactMethod[] = [
  {
    id: 1,
    type: 'email',
    title: 'Send Email',
    value: 'prathu2k4@gmail.com',
    href: 'mailto:prathu2k4@gmail.com',
    icon: '📧'
  },
  {
    id: 2,
    type: 'location',
    title: 'Location',
    value: 'Bengaluru, India',
    href: '#',
    icon: '📍'
  },
  {
    id: 3,
    type: 'linkedin',
    title: 'Connect professionally',
    value: 'LinkedIn',
    href: 'https://linkedin.com/in/pratham-jain',
    icon: '💼'
  }
]

export const socialLinks = [
  {
    platform: 'GitHub',
    url: 'https://github.com/pratham-jain',
    icon: '💻'
  },
  {
    platform: 'LinkedIn',
    url: 'https://linkedin.com/in/pratham-jain',
    icon: '💼'
  },
  {
    platform: 'Email',
    url: 'mailto:prathu2k4@gmail.com', 
    icon: '📧'
  }
]
```

### 2.2 Update Data Index File

**File: `src/lib/data/index.ts`** (update existing file)
```typescript
export { navigationItems } from './navigation'
export { projectsData } from './projects'
export { skillsData } from './skills'
export { achievementsData } from './achievements'
export { contactData, socialLinks } from './contact'
```

### 2.3 Create Contact Section Component

**File: `src/components/sections/Contact.tsx`**
```tsx
'use client'

import { motion } from 'framer-motion'
import { useState } from 'react'
import { useIntersectionObserver } from '@/lib/hooks/useIntersectionObserver'
import { Card, CardBody } from '@/components/ui/Card'
import { Button } from '@/components/ui/Button'
import { contactData } from '@/lib/data'
import { Mail, MapPin, Linkedin, Copy, Check } from 'lucide-react'

const Contact = () => {
  const [copiedEmail, setCopiedEmail] = useState(false)
  
  const { ref: contactRef, isVisible } = useIntersectionObserver({
    threshold: 0.1,
    triggerOnce: true
  })

  const containerVariants = {
    hidden: { opacity: 0 },
    visible: {
      opacity: 1,
      transition: {
        delayChildren: 0.3,
        staggerChildren: 0.1
      }
    }
  }

  const itemVariants = {
    hidden: { opacity: 0, y: 50 },
    visible: { 
      opacity: 1, 
      y: 0,
      transition: {
        duration: 0.6,
        ease: "easeOut"
      }
    }
  }

  const handleCopyEmail = async () => {
    try {
      await navigator.clipboard.writeText('prathu2k4@gmail.com')
      setCopiedEmail(true)
      setTimeout(() => setCopiedEmail(false), 2000)
    } catch (err) {
      console.error('Failed to copy email:', err)
    }
  }

  const getContactIcon = (type: string) => {
    switch (type) {
      case 'email':
        return <Mail size={24} />
      case 'location':
        return <MapPin size={24} />
      case 'linkedin':
        return <Linkedin size={24} />
      default:
        return <Mail size={24} />
    }
  }

  return (
    <section 
      ref={contactRef as React.RefObject<HTMLElement>}
      id="contact"
      style={{ 
        minHeight: '100vh',
        padding: '120px 0',
        background: 'linear-gradient(135deg, rgba(15, 23, 42, 0.95) 0%, rgba(30, 41, 59, 0.95) 100%)'
      }}
    >
      <div className="container">
        {/* Section Header */}
        <motion.div
          style={{ 
            textAlign: 'center', 
            marginBottom: 'var(--space-32)',
            maxWidth: '600px',
            margin: '0 auto var(--space-32)'
          }}
          initial="hidden"
          animate={isVisible ? "visible" : "hidden"}
          variants={containerVariants}
        >
          <motion.h2 
            variants={itemVariants}
            style={{ 
              fontSize: 'clamp(2.5rem, 5vw, 4rem)',
              fontWeight: 'bold',
              marginBottom: 'var(--space-16)',
              background: 'linear-gradient(135deg, #38bdf8 0%, #10b981 100%)',
              WebkitBackgroundClip: 'text',
              WebkitTextFillColor: 'transparent',
              backgroundClip: 'text'
            }}
          >
            Let's Connect
          </motion.h2>
          <motion.p 
            variants={itemVariants}
            style={{ 
              fontSize: 'var(--font-size-xl)',
              color: 'var(--color-text-secondary)'
            }}
          >
            Ready to collaborate on innovative projects
          </motion.p>
        </motion.div>

        {/* Contact Cards */}
        <motion.div
          style={{
            display: 'grid',
            gridTemplateColumns: 'repeat(auto-fit, minmax(300px, 1fr))',
            gap: 'var(--space-24)',
            maxWidth: '1000px',
            margin: '0 auto var(--space-32)'
          }}
          initial="hidden"
          animate={isVisible ? "visible" : "hidden"}
          variants={containerVariants}
        >
          {contactData.map((contact, index) => (
            <motion.div
              key={contact.id}
              variants={itemVariants}
              whileHover={{ 
                y: -8, 
                scale: 1.05
              }}
            >
              <Card
                hover
                style={{
                  height: '100%',
                  background: 'rgba(255, 255, 255, 0.02)',
                  border: '1px solid rgba(255, 255, 255, 0.1)',
                  cursor: contact.type === 'location' ? 'default' : 'pointer'
                }}
                onClick={() => {
                  if (contact.type === 'email') {
                    handleCopyEmail()
                  } else if (contact.type === 'linkedin') {
                    window.open(contact.href, '_blank')
                  }
                }}
              >
                <CardBody style={{ 
                  display: 'flex', 
                  flexDirection: 'column', 
                  alignItems: 'center',
                  textAlign: 'center',
                  height: '100%' 
                }}>
                  <motion.div 
                    style={{
                      width: '70px',
                      height: '70px',
                      background: 'linear-gradient(135deg, rgba(56, 189, 248, 0.2), rgba(16, 185, 129, 0.2))',
                      borderRadius: '50%',
                      display: 'flex',
                      alignItems: 'center',
                      justifyContent: 'center',
                      marginBottom: 'var(--space-16)',
                      color: '#38bdf8'
                    }}
                    initial={{ scale: 0, rotate: -90 }}
                    animate={isVisible ? { scale: 1, rotate: 0 } : {}}
                    transition={{
                      duration: 0.5,
                      delay: 0.4 + index * 0.1,
                      type: "spring",
                      stiffness: 300
                    }}
                    whileHover={{ 
                      scale: 1.1,
                      rotate: contact.type === 'email' ? 0 : 15
                    }}
                  >
                    {getContactIcon(contact.type)}
                  </motion.div>
                  
                  <h4 style={{
                    fontSize: 'var(--font-size-xl)',
                    fontWeight: '600',
                    color: 'var(--color-text)',
                    marginBottom: 'var(--space-8)'
                  }}>
                    {contact.title}
                  </h4>
                  
                  <p style={{
                    color: 'var(--color-text-secondary)',
                    marginBottom: 'var(--space-16)',
                    lineHeight: '1.6'
                  }}>
                    {contact.value}
                  </p>
                  
                  {contact.type === 'email' && (
                    <Button
                      variant="outline"
                      size="sm"
                      onClick={(e) => {
                        e.stopPropagation()
                        handleCopyEmail()
                      }}
                      style={{
                        background: 'rgba(56, 189, 248, 0.1)',
                        borderColor: 'rgba(56, 189, 248, 0.3)',
                        color: '#38bdf8',
                        display: 'flex',
                        alignItems: 'center',
                        gap: 'var(--space-8)'
                      }}
                    >
                      {copiedEmail ? (
                        <>
                          <Check size={16} />
                          Copied!
                        </>
                      ) : (
                        <>
                          <Copy size={16} />
                          Copy Email
                        </>
                      )}
                    </Button>
                  )}
                  
                  {contact.type === 'linkedin' && (
                    <Button
                      variant="primary"
                      size="sm"
                      style={{
                        display: 'flex',
                        alignItems: 'center',
                        gap: 'var(--space-8)'
                      }}
                    >
                      <Linkedin size={16} />
                      Connect
                    </Button>
                  )}
                </CardBody>
              </Card>
            </motion.div>
          ))}
        </motion.div>

        {/* Call to Action */}
        <motion.div
          initial="hidden"
          animate={isVisible ? "visible" : "hidden"}
          variants={containerVariants}
        >
          <motion.div variants={itemVariants}>
            <Card 
              style={{
                background: 'linear-gradient(135deg, rgba(56, 189, 248, 0.05) 0%, rgba(16, 185, 129, 0.05) 50%, rgba(139, 92, 246, 0.05) 100%)',
                border: '1px solid rgba(56, 189, 248, 0.2)',
                maxWidth: '800px',
                margin: '0 auto'
              }}
            >
              <CardBody style={{ textAlign: 'center', padding: 'var(--space-32)' }}>
                <h3 style={{
                  fontSize: 'var(--font-size-2xl)',
                  fontWeight: 'bold',
                  color: 'var(--color-text)',
                  marginBottom: 'var(--space-16)',
                  background: 'linear-gradient(135deg, #38bdf8, #10b981)',
                  WebkitBackgroundClip: 'text',
                  WebkitTextFillColor: 'transparent',
                  backgroundClip: 'text'
                }}>
                  Ready to bring your ideas to life?
                </h3>
                <p style={{
                  fontSize: 'var(--font-size-lg)',
                  color: 'var(--color-text-secondary)',
                  lineHeight: '1.7',
                  marginBottom: 'var(--space-24)',
                  maxWidth: '600px',
                  margin: '0 auto var(--space-24)'
                }}>
                  Whether it's digital design, systems programming, or innovative automation solutions, 
                  I'm always excited to collaborate on projects that push the boundaries of technology.
                </p>
                <div style={{
                  display: 'flex',
                  gap: 'var(--space-16)',
                  justifyContent: 'center',
                  flexWrap: 'wrap'
                }}>
                  <Button
                    variant="primary"
                    size="lg"
                    onClick={() => window.open('mailto:prathu2k4@gmail.com', '_blank')}
                    style={{
                      display: 'flex',
                      alignItems: 'center',
                      gap: 'var(--space-8)',
                      boxShadow: '0 8px 32px rgba(56, 189, 248, 0.3)'
                    }}
                  >
                    <Mail size={20} />
                    Start a Conversation
                  </Button>
                  <Button
                    variant="outline"
                    size="lg"
                    onClick={() => window.open('https://linkedin.com/in/pratham-jain', '_blank')}
                    style={{
                      display: 'flex',
                      alignItems: 'center',
                      gap: 'var(--space-8)'
                    }}
                  >
                    <Linkedin size={20} />
                    Connect on LinkedIn
                  </Button>
                </div>
              </CardBody>
            </Card>
          </motion.div>
        </motion.div>
      </div>
    </section>
  )
}

export default Contact
```

## Step 3: Create Footer Component

### 3.1 Create Footer Component

**File: `src/components/layout/Footer.tsx`**
```tsx
'use client'

import { motion } from 'framer-motion'
import { Github, Linkedin, Mail, Heart } from 'lucide-react'
import { socialLinks } from '@/lib/data'

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
    <footer style={{
      background: 'linear-gradient(135deg, rgba(15, 23, 42, 0.98) 0%, rgba(30, 41, 59, 0.98) 100%)',
      borderTop: '1px solid rgba(56, 189, 248, 0.2)',
      padding: 'var(--space-32) 0',
      marginTop: 'auto'
    }}>
      <div className="container">
        <div style={{
          display: 'flex',
          justifyContent: 'space-between',
          alignItems: 'center',
          gap: 'var(--space-24)',
          flexWrap: 'wrap'
        }}>
          <div style={{ flex: 1 }}>
            <motion.div
              initial={{ opacity: 0, y: 20 }}
              whileInView={{ opacity: 1, y: 0 }}
              transition={{ duration: 0.6 }}
              viewport={{ once: true }}
            >
              <p style={{
                color: 'var(--color-text-secondary)',
                margin: 0,
                fontSize: 'var(--font-size-base)',
                display: 'flex',
                alignItems: 'center',
                gap: 'var(--space-4)',
                flexWrap: 'wrap'
              }}>
                © {currentYear} Pratham Jain V S. Built with{' '}
                <motion.span
                  animate={{ scale: [1, 1.2, 1] }}
                  transition={{ duration: 1.5, repeat: Infinity }}
                  style={{ display: 'inline-block', color: '#ef4444' }}
                >
                  <Heart size={16} fill="currentColor" />
                </motion.span>{' '}
                using Next.js & Framer Motion
              </p>
            </motion.div>
          </div>

          <div style={{ display: 'flex', gap: 'var(--space-16)' }}>
            {socialLinks.map((link, index) => (
              <motion.a
                key={link.platform}
                href={link.url}
                target="_blank"
                rel="noopener noreferrer"
                style={{
                  display: 'flex',
                  alignItems: 'center',
                  justifyContent: 'center',
                  width: '40px',
                  height: '40px',
                  background: 'rgba(56, 189, 248, 0.1)',
                  border: '1px solid rgba(56, 189, 248, 0.2)',
                  borderRadius: 'var(--radius-base)',
                  color: '#38bdf8',
                  textDecoration: 'none',
                  transition: 'all 0.3s ease'
                }}
                initial={{ opacity: 0, y: 20 }}
                whileInView={{ opacity: 1, y: 0 }}
                transition={{ duration: 0.6, delay: index * 0.1 }}
                viewport={{ once: true }}
                whileHover={{ 
                  y: -2, 
                  scale: 1.05,
                  background: 'rgba(56, 189, 248, 0.2)',
                  borderColor: 'rgba(56, 189, 248, 0.4)',
                  boxShadow: '0 4px 20px rgba(56, 189, 248, 0.2)'
                }}
                whileTap={{ scale: 0.95 }}
                aria-label={`Visit ${link.platform}`}
              >
                {getSocialIcon(link.platform)}
              </motion.a>
            ))}
          </div>
        </div>
        
        <motion.div
          style={{
            height: '1px',
            background: 'linear-gradient(90deg, transparent, #38bdf8, transparent)',
            margin: 'var(--space-24) 0',
            transformOrigin: 'center'
          }}
          initial={{ scaleX: 0 }}
          whileInView={{ scaleX: 1 }}
          transition={{ duration: 0.8 }}
          viewport={{ once: true }}
        />
        
        <motion.div
          style={{ textAlign: 'center' }}
          initial={{ opacity: 0 }}
          whileInView={{ opacity: 1 }}
          transition={{ duration: 0.6, delay: 0.3 }}
          viewport={{ once: true }}
        >
          <p style={{
            color: 'var(--color-text-secondary)',
            margin: 0,
            fontSize: 'var(--font-size-sm)',
            fontStyle: 'italic'
          }}>
            Designed & Developed with passion for innovation
          </p>
        </motion.div>
      </div>
    </footer>
  )
}

export default Footer
```

## Step 4: Create Back to Top Component

### 4.1 Create Back to Top Component

**File: `src/components/layout/BackToTop.tsx`**
```tsx
'use client'

import { useState, useEffect } from 'react'
import { motion, AnimatePresence } from 'framer-motion'
import { ChevronUp } from 'lucide-react'
import { useSmoothScroll } from '@/lib/hooks/useSmoothScroll'

const BackToTop = () => {
  const [isVisible, setIsVisible] = useState(false)
  const { scrollToElement } = useSmoothScroll()

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

  const scrollToTop = () => {
    window.scrollTo({
      top: 0,
      behavior: 'smooth'
    })
  }

  return (
    <AnimatePresence>
      {isVisible && (
        <motion.button
          onClick={scrollToTop}
          style={{
            position: 'fixed',
            bottom: 'var(--space-32)',
            right: 'var(--space-32)',
            width: '56px',
            height: '56px',
            background: 'linear-gradient(135deg, #38bdf8, #10b981)',
            color: 'var(--color-charcoal-800)',
            border: 'none',
            borderRadius: '50%',
            cursor: 'pointer',
            boxShadow: '0 8px 32px rgba(56, 189, 248, 0.3)',
            zIndex: 100,
            display: 'flex',
            alignItems: 'center',
            justifyContent: 'center'
          }}
          initial={{ opacity: 0, y: 20 }}
          animate={{ opacity: 1, y: 0 }}
          exit={{ opacity: 0, y: 20 }}
          whileHover={{ 
            scale: 1.1, 
            y: -5,
            boxShadow: '0 12px 40px rgba(56, 189, 248, 0.4)'
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

## Step 5: Update Main Page with All Components

### 5.1 Update Main Page

**File: `src/app/page.tsx`** (replace existing content)
```tsx
'use client'

import Header from '@/components/layout/Header'
import Footer from '@/components/layout/Footer'
import BackToTop from '@/components/layout/BackToTop'
import Projects from '@/components/sections/Projects'
import Skills from '@/components/sections/Skills'
import Achievements from '@/components/sections/Achievements'
import Contact from '@/components/sections/Contact'

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
                <p style={{ lineHeight: '1.7', marginBottom: 'var(--space-16)' }}>
                  My journey in electronics and communication has been driven by a deep fascination 
                  with how digital systems work at the lowest levels.
                </p>
                <p style={{ lineHeight: '1.7' }}>
                  Currently focused on advancing my expertise in VLSI design, digital signal 
                  processing, and IoT systems while contributing to cutting-edge research projects.
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
                
                {/* Stats */}
                <div style={{
                  display: 'grid',
                  gridTemplateColumns: 'repeat(3, 1fr)',
                  gap: 'var(--space-16)',
                  marginTop: 'var(--space-24)'
                }}>
                  {[
                    { label: 'Projects', value: '15+' },
                    { label: 'Technologies', value: '20+' },
                    { label: 'Years Experience', value: '3+' }
                  ].map((stat) => (
                    <div key={stat.label} style={{
                      textAlign: 'center',
                      padding: 'var(--space-16)',
                      background: 'rgba(56, 189, 248, 0.1)',
                      borderRadius: 'var(--radius-base)',
                      border: '1px solid rgba(56, 189, 248, 0.2)'
                    }}>
                      <div style={{
                        fontSize: 'var(--font-size-2xl)',
                        fontWeight: 'bold',
                        color: '#38bdf8',
                        marginBottom: 'var(--space-4)'
                      }}>
                        {stat.value}
                      </div>
                      <div style={{
                        fontSize: '12px',
                        color: 'var(--color-text-secondary)',
                        fontWeight: '500',
                        textTransform: 'uppercase',
                        letterSpacing: '0.5px'
                      }}>
                        {stat.label}
                      </div>
                    </div>
                  ))}
                </div>
              </div>
            </div>
          </div>
        </section>

        {/* Skills Section */}
        <Skills />

        {/* Projects Section */}
        <Projects />

        {/* Achievements Section */}
        <Achievements />

        {/* Contact Section */}
        <Contact />
      </main>

      <Footer />
      <BackToTop />
    </>
  )
}
```

## Step 6: Test Complete Portfolio

### 6.1 Run and Test
```bash
npm run dev
```

### 6.2 Verify Everything Works

1. Open `http://localhost:3000`
2. You should see:
   - Complete hero section with improved about section and stats
   - Working skills section with animations
   - Professional projects section with detailed cards
   - Achievements section with animated icons
   - Contact section with interactive cards and email copy functionality
   - Footer with social links and animations
   - Back to top button that appears when scrolling
   - All navigation links working smoothly
   - Responsive design on all screen sizes

### 6.3 Test All Interactions

1. **Navigation**: Click all nav items - should smoothly scroll to sections
2. **Skills**: Scroll to skills - progress rings should animate
3. **Projects**: Hover over project cards - should have hover effects
4. **Achievements**: Scroll to achievements - icons should animate in
5. **Contact**: 
   - Click email card - should copy email to clipboard
   - Click LinkedIn card - should open LinkedIn in new tab
   - Click CTA buttons - should open email/LinkedIn
6. **Footer**: Click social links - should open in new tabs
7. **Back to Top**: Scroll down - button should appear and work
8. **Mobile**: Test on mobile - hamburger menu should work

## What You've Accomplished

✅ Built complete portfolio with all sections  
✅ Added professional achievements section with animations  
✅ Created interactive contact section with copy-to-clipboard  
✅ Built footer with social links and animations  
✅ Added back-to-top functionality  
✅ Enhanced about section with statistics  
✅ Implemented scroll-based animations throughout  
✅ Ensured responsive design on all devices  
✅ Added professional micro-interactions and hover effects  

## Next Steps (Optional Enhancements)

For further enhancements, you could add:
- Project detail modals
- Contact form with validation
- Blog section
- Loading states
- Error boundaries
- SEO optimization
- Performance improvements

Your portfolio is now complete and ready for deployment!