# Phase 3: Data Layer & Card Components

In this phase, you'll add structured data for projects, skills, and achievements, along with reusable card components. After completing this phase, you'll see actual project data displayed in professional cards.

## Prerequisites

Complete Phase 1 and Phase 2 first. Your navigation should be working properly.

## Step 1: Enhance Type Definitions

### 1.1 Update Type Definitions

**File: `src/types/index.ts`** (replace existing content)
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
  id: number
  name: string
  level: number
  category: 'programming' | 'tools' | 'domains'
  icon: string
  description: string
}

export interface Achievement {
  id: number
  title: string
  role: string
  period: string
  description: string
  icon: string
  status: 'ongoing' | 'completed'
}

export interface ContactMethod {
  id: number
  type: 'email' | 'location' | 'linkedin'
  title: string
  value: string
  href: string
  icon: string
}

export interface TimelineItem {
  id: number
  institution: string
  degree: string
  period: string
  focus: string[]
}

export interface StatItem {
  label: string
  value: string
  suffix?: string
}

export interface SocialLink {
  platform: string
  url: string
  icon: string
}
```

## Step 2: Create Data Files

### 2.1 Create Projects Data

**File: `src/lib/data/projects.ts`**
```typescript
import { Project } from '@/types'

export const projectsData: Project[] = [
  {
    id: 1,
    title: "RTL Design using Verilog HDL",
    status: "ongoing",
    description: "Comprehensive digital system design project implementing complex RTL architectures using Verilog HDL. Features advanced verification methodologies and FPGA optimization techniques.",
    technologies: ["Verilog", "SystemVerilog", "ModelSim", "Vivado", "FPGA"],
    category: "digital",
    highlights: [
      "Advanced RTL design patterns and methodologies",
      "Comprehensive testbench development and verification", 
      "FPGA synthesis optimization and timing analysis",
      "Industry-standard design flow implementation"
    ],
    githubUrl: "https://github.com/pratham-jain/rtl-design",
    icon: "⚡"
  },
  {
    id: 2,
    title: "Line Follower Robot Champion",
    status: "completed",
    award: "🏆 1st Place - Hack-a-Maze 2024",
    description: "Award-winning autonomous navigation system with advanced sensor fusion and real-time control algorithms. Showcased exceptional performance in competitive robotics environment.",
    technologies: ["C++", "Arduino", "PID Control", "Sensor Fusion"],
    category: "systems",
    highlights: [
      "Champion robotics solution with advanced algorithms",
      "Real-time PID control implementation and tuning",
      "Multi-sensor fusion for precise navigation",
      "Competition-winning autonomous decision making"
    ],
    githubUrl: "https://github.com/pratham-jain/line-follower",
    icon: "🏆"
  },
  {
    id: 3,
    title: "Python Automation Suite",
    status: "completed",
    company: "Samsung SIC India",
    description: "Industrial-grade automation solution developed during Samsung internship. Streamlined complex workflows and improved operational efficiency through intelligent scripting.",
    technologies: ["Python", "Automation", "API Integration", "Data Processing"],
    category: "software",
    highlights: [
      "Enterprise-level workflow automation solutions",
      "Intelligent task scheduling and optimization", 
      "Seamless API integration and data processing",
      "Performance monitoring and analytics systems"
    ],
    githubUrl: "https://github.com/pratham-jain/automation-suite",
    icon: "🚀"
  }
]
```

### 2.2 Create Skills Data

**File: `src/lib/data/skills.ts`**
```typescript
import { Skill } from '@/types'

export const skillsData: Skill[] = [
  // Programming Languages
  {
    id: 1,
    name: 'Verilog',
    level: 90,
    category: 'programming',
    icon: '⚡',
    description: 'Hardware Description Language'
  },
  {
    id: 2,
    name: 'Python', 
    level: 85,
    category: 'programming',
    icon: '🐍',
    description: 'Automation & Development'
  },
  {
    id: 3,
    name: 'C++',
    level: 80,
    category: 'programming', 
    icon: '⚙️',
    description: 'Systems Programming'
  },
  {
    id: 4,
    name: 'JavaScript',
    level: 75,
    category: 'programming',
    icon: '💛',
    description: 'Web Development'
  },
  
  // Tools
  {
    id: 5,
    name: 'ModelSim',
    level: 85,
    category: 'tools',
    icon: '📊',
    description: 'Analysis & Simulation'
  },
  {
    id: 6,
    name: 'Vivado',
    level: 80,
    category: 'tools',
    icon: '🔌', 
    description: 'Circuit Simulation'
  },
  {
    id: 7,
    name: 'Quartus',
    level: 75,
    category: 'tools',
    icon: '🎯',
    description: 'FPGA Development'
  },
  {
    id: 8,
    name: 'Git',
    level: 85,
    category: 'tools',
    icon: '📝',
    description: 'Version Control'
  },
  
  // Domains
  {
    id: 9,
    name: 'Digital Design',
    level: 90,
    category: 'domains',
    icon: '💾',
    description: 'Digital circuit design and verification'
  },
  {
    id: 10,
    name: 'VLSI',
    level: 85,
    category: 'domains',
    icon: '🔧',
    description: 'Very Large Scale Integration design'
  },
  {
    id: 11,
    name: 'Analog Design',
    level: 70,
    category: 'domains',
    icon: '📡',
    description: 'Analog and digital circuit design'
  }
]
```

### 2.3 Create Achievements Data

**File: `src/lib/data/achievements.ts`**
```typescript
import { Achievement } from '@/types'

export const achievementsData: Achievement[] = [
  {
    id: 1,
    title: "UVCE Research Lab",
    role: "Research Associate",
    period: "Ongoing",
    description: "Contributing to cutting-edge research in IoT and hardware systems at UVCE's premier research facility.",
    icon: "🔬",
    status: "ongoing"
  },
  {
    id: 2,
    title: "Smart India Hackathon",
    role: "Team Lead",
    period: "2024",
    description: "Led technical team to national finals, demonstrating leadership and innovation in problem-solving.",
    icon: "🧠",
    status: "completed"
  },
  {
    id: 3,
    title: "Hack-a-Maze Competition",
    role: "Technical Lead",
    period: "1st Place • 2024",
    description: "Designed and implemented winning line following system at Impetus, UVCE Tech Fest.",
    icon: "🏆",
    status: "completed"
  }
]
```

### 2.4 Create Central Data Export

**File: `src/lib/data/index.ts`**
```typescript
export { navigationItems } from './navigation'
export { projectsData } from './projects'
export { skillsData } from './skills'
export { achievementsData } from './achievements'
```

## Step 3: Create Reusable UI Components

### 3.1 Create Button Component

**File: `src/components/ui/Button.tsx`**
```tsx
'use client'

import { ButtonHTMLAttributes, forwardRef } from 'react'
import { motion, HTMLMotionProps } from 'framer-motion'
import { cn } from '@/lib/utils'

interface ButtonProps extends Omit<HTMLMotionProps<'button'>, 'size'> {
  variant?: 'primary' | 'outline' | 'secondary'
  size?: 'sm' | 'md' | 'lg'
  children: React.ReactNode
}

const Button = forwardRef<HTMLButtonElement, ButtonProps>(
  ({ className, variant = 'primary', size = 'md', children, ...props }, ref) => {
    const baseStyles = {
      display: 'inline-flex',
      alignItems: 'center',
      justifyContent: 'center',
      borderRadius: 'var(--radius-base)',
      fontWeight: '500',
      cursor: 'pointer',
      transition: 'all 0.2s ease',
      border: 'none',
      textDecoration: 'none',
      fontSize: size === 'sm' ? '14px' : size === 'lg' ? '18px' : '16px',
      padding: size === 'sm' ? 'var(--space-8) var(--space-16)' : 
               size === 'lg' ? 'var(--space-16) var(--space-32)' : 
               'var(--space-12) var(--space-24)'
    }

    const variantStyles = {
      primary: {
        background: 'linear-gradient(135deg, #38bdf8 0%, #10b981 100%)',
        color: 'white'
      },
      outline: {
        background: 'transparent',
        border: '2px solid var(--color-primary)',
        color: 'var(--color-primary)'
      },
      secondary: {
        background: 'var(--color-surface)',
        color: 'var(--color-text)',
        border: '1px solid var(--color-border)'
      }
    }

    return (
      <motion.button
        ref={ref}
        style={{
          ...baseStyles,
          ...variantStyles[variant]
        }}
        whileHover={{ 
          scale: 1.05,
          y: -2
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

export { Button }
```

### 3.2 Create Card Component

**File: `src/components/ui/Card.tsx`**
```tsx
'use client'

import { HTMLAttributes, forwardRef } from 'react'
import { motion, HTMLMotionProps } from 'framer-motion'
import { cn } from '@/lib/utils'

interface CardProps extends HTMLMotionProps<'div'> {
  hover?: boolean
  children: React.ReactNode
}

const Card = forwardRef<HTMLDivElement, CardProps>(
  ({ className, hover = false, children, style, ...props }, ref) => {
    const cardStyles = {
      background: 'rgba(255, 255, 255, 0.02)',
      backdropFilter: 'blur(20px)',
      border: '1px solid rgba(255, 255, 255, 0.1)',
      borderRadius: 'var(--radius-lg)',
      padding: 'var(--space-24)',
      ...style
    }

    return (
      <motion.div
        ref={ref}
        style={cardStyles}
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
      style={{ marginBottom: 'var(--space-16)' }}
      {...props}
    />
  )
)

CardHeader.displayName = 'CardHeader'

const CardBody = forwardRef<HTMLDivElement, HTMLAttributes<HTMLDivElement>>(
  ({ className, ...props }, ref) => (
    <div
      ref={ref}
      {...props}
    />
  )
)

CardBody.displayName = 'CardBody'

export { Card, CardHeader, CardBody }
```

### 3.3 Create Progress Ring Component

**File: `src/components/ui/ProgressRing.tsx`**
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
            <stop offset="0%" stopColor="#38bdf8" />
            <stop offset="100%" stopColor="#10b981" />
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
          style={{
            position: 'absolute',
            top: 0,
            left: 0,
            right: 0,
            bottom: 0,
            display: 'flex',
            alignItems: 'center',
            justifyContent: 'center'
          }}
        >
          <span 
            style={{ 
              fontSize: `${size * 0.15}px`,
              fontWeight: 'bold',
              color: '#38bdf8'
            }}
          >
            {percentage}%
          </span>
        </motion.div>
      )}
    </div>
  )
}

export default ProgressRing
```

## Step 4: Create Section Components

### 4.1 Create Intersection Observer Hook

**File: `src/lib/hooks/useIntersectionObserver.ts`**
```typescript
import { useEffect, useRef, useState } from 'react'

interface UseIntersectionObserverOptions {
  threshold?: number | number[]
  rootMargin?: string
  triggerOnce?: boolean
}

export function useIntersectionObserver(
  options: UseIntersectionObserverOptions = {}
) {
  const [isIntersecting, setIsIntersecting] = useState(false)
  const [hasTriggered, setHasTriggered] = useState(false)
  const elementRef = useRef<HTMLElement>(null)

  const {
    threshold = 0.1,
    rootMargin = '0px 0px -100px 0px',
    triggerOnce = true
  } = options

  useEffect(() => {
    const element = elementRef.current
    if (!element) return

    const observer = new IntersectionObserver(
      ([entry]) => {
        const isElementIntersecting = entry.isIntersecting
        
        if (isElementIntersecting && !hasTriggered) {
          setIsIntersecting(true)
          if (triggerOnce) {
            setHasTriggered(true)
          }
        } else if (!triggerOnce) {
          setIsIntersecting(isElementIntersecting)
        }
      },
      { threshold, rootMargin }
    )

    observer.observe(element)

    return () => {
      observer.unobserve(element)
    }
  }, [threshold, rootMargin, triggerOnce, hasTriggered])

  return { 
    ref: elementRef, 
    isIntersecting, 
    hasTriggered,
    isVisible: isIntersecting 
  }
}
```

### 4.2 Create Projects Section Component

**File: `src/components/sections/Projects.tsx`**
```tsx
'use client'

import { motion } from 'framer-motion'
import { useIntersectionObserver } from '@/lib/hooks/useIntersectionObserver'
import { Card, CardBody } from '@/components/ui/Card'
import { Button } from '@/components/ui/Button'
import { projectsData } from '@/lib/data'
import { ExternalLink, Github, Award, Building } from 'lucide-react'

const Projects = () => {
  const { ref: projectsRef, isVisible } = useIntersectionObserver({
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

  return (
    <section 
      ref={projectsRef as React.RefObject<HTMLElement>}
      id="projects"
      style={{ 
        minHeight: '100vh',
        padding: '120px 0',
        background: 'linear-gradient(135deg, rgba(30, 41, 59, 0.9) 0%, rgba(15, 23, 42, 0.9) 100%)'
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
            Featured Projects
          </motion.h2>
          <motion.p 
            variants={itemVariants}
            style={{ 
              fontSize: 'var(--font-size-xl)',
              color: 'var(--color-text-secondary)',
              lineHeight: '1.6'
            }}
          >
            Innovative solutions and technical achievements
          </motion.p>
        </motion.div>

        {/* Projects Grid */}
        <motion.div
          style={{
            display: 'grid',
            gridTemplateColumns: 'repeat(auto-fit, minmax(400px, 1fr))',
            gap: 'var(--space-32)',
            maxWidth: '1400px',
            margin: '0 auto'
          }}
          initial="hidden"
          animate={isVisible ? "visible" : "hidden"}
          variants={containerVariants}
        >
          {projectsData.map((project, index) => (
            <motion.div
              key={project.id}
              variants={itemVariants}
            >
              <Card
                hover
                style={{
                  height: '100%',
                  background: 'rgba(255, 255, 255, 0.02)',
                  border: '1px solid rgba(255, 255, 255, 0.1)'
                }}
              >
                <CardBody style={{ display: 'flex', flexDirection: 'column', height: '100%' }}>
                  {/* Project Header */}
                  <div style={{ 
                    display: 'flex', 
                    alignItems: 'flex-start', 
                    gap: 'var(--space-16)', 
                    marginBottom: 'var(--space-20)' 
                  }}>
                    <div style={{
                      width: '60px',
                      height: '60px',
                      background: 'linear-gradient(135deg, rgba(56, 189, 248, 0.2), rgba(16, 185, 129, 0.2))',
                      borderRadius: 'var(--radius-base)',
                      display: 'flex',
                      alignItems: 'center',
                      justifyContent: 'center',
                      fontSize: '1.5rem'
                    }}>
                      {project.icon}
                    </div>
                    
                    <div style={{ flex: 1 }}>
                      <div style={{
                        display: 'flex',
                        alignItems: 'center',
                        gap: 'var(--space-8)',
                        marginBottom: 'var(--space-8)'
                      }}>
                        <span style={{
                          background: project.status === 'ongoing' 
                            ? 'rgba(245, 158, 11, 0.2)' 
                            : 'rgba(16, 185, 129, 0.2)',
                          color: project.status === 'ongoing' ? '#F59E0B' : '#10B981',
                          padding: 'var(--space-4) var(--space-8)',
                          borderRadius: 'var(--radius-full)',
                          fontSize: '12px',
                          fontWeight: '500',
                          textTransform: 'uppercase',
                          border: `1px solid ${project.status === 'ongoing' 
                            ? 'rgba(245, 158, 11, 0.3)' 
                            : 'rgba(16, 185, 129, 0.3)'}`
                        }}>
                          {project.status}
                        </span>
                      </div>
                      
                      {project.award && (
                        <div style={{
                          background: 'linear-gradient(135deg, #F59E0B, #EF4444)',
                          color: 'white',
                          padding: 'var(--space-4) var(--space-8)',
                          borderRadius: 'var(--radius-base)',
                          fontSize: '12px',
                          fontWeight: 'bold',
                          marginBottom: 'var(--space-8)',
                          display: 'inline-block'
                        }}>
                          {project.award}
                        </div>
                      )}
                      
                      {project.company && (
                        <div style={{
                          background: 'rgba(139, 92, 246, 0.2)',
                          color: '#8B5CF6',
                          padding: 'var(--space-4) var(--space-8)',
                          borderRadius: 'var(--radius-base)',
                          fontSize: '12px',
                          fontWeight: '500',
                          border: '1px solid rgba(139, 92, 246, 0.3)',
                          display: 'flex',
                          alignItems: 'center',
                          gap: 'var(--space-4)',
                          width: 'fit-content'
                        }}>
                          <Building size={12} />
                          {project.company}
                        </div>
                      )}
                    </div>
                  </div>

                  {/* Project Content */}
                  <div style={{ flex: 1, display: 'flex', flexDirection: 'column' }}>
                    <h3 style={{
                      fontSize: 'var(--font-size-xl)',
                      fontWeight: 'bold',
                      color: 'var(--color-text)',
                      marginBottom: 'var(--space-12)'
                    }}>
                      {project.title}
                    </h3>
                    
                    <p style={{
                      color: 'var(--color-text-secondary)',
                      lineHeight: '1.7',
                      marginBottom: 'var(--space-20)',
                      flex: 1
                    }}>
                      {project.description}
                    </p>
                    
                    {/* Technologies */}
                    <div style={{
                      display: 'flex',
                      flexWrap: 'wrap',
                      gap: 'var(--space-8)',
                      marginBottom: 'var(--space-24)'
                    }}>
                      {project.technologies.map((tech) => (
                        <span
                          key={tech}
                          style={{
                            background: 'rgba(56, 189, 248, 0.1)',
                            color: '#38bdf8',
                            padding: 'var(--space-4) var(--space-8)',
                            borderRadius: 'var(--radius-base)',
                            fontSize: '12px',
                            fontWeight: '500',
                            border: '1px solid rgba(56, 189, 248, 0.2)'
                          }}
                        >
                          {tech}
                        </span>
                      ))}
                    </div>

                    {/* Actions */}
                    <div style={{
                      display: 'flex',
                      gap: 'var(--space-12)',
                      marginTop: 'auto'
                    }}>
                      <Button variant="primary" size="sm">
                        <ExternalLink size={16} style={{ marginRight: 'var(--space-4)' }} />
                        View Details
                      </Button>
                      
                      {project.githubUrl && (
                        <Button variant="outline" size="sm">
                          <Github size={16} style={{ marginRight: 'var(--space-4)' }} />
                          GitHub
                        </Button>
                      )}
                    </div>
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

export default Projects
```

### 4.3 Create Skills Section Component

**File: `src/components/sections/Skills.tsx`**
```tsx
'use client'

import { motion } from 'framer-motion'
import { useIntersectionObserver } from '@/lib/hooks/useIntersectionObserver'
import { Card, CardBody } from '@/components/ui/Card'
import ProgressRing from '@/components/ui/ProgressRing'
import { skillsData } from '@/lib/data'

const Skills = () => {
  const { ref: skillsRef, isVisible } = useIntersectionObserver({
    threshold: 0.2,
    triggerOnce: true
  })

  const programmingSkills = skillsData.filter(skill => skill.category === 'programming')
  const toolsSkills = skillsData.filter(skill => skill.category === 'tools')
  const domainSkills = skillsData.filter(skill => skill.category === 'domains')

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
    hidden: { opacity: 0, y: 30, scale: 0.8 },
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

  const SkillCategory = ({ title, skills, type }: { 
    title: string; 
    skills: typeof skillsData; 
    type: 'programming' | 'tools' | 'domains' 
  }) => (
    <div style={{ marginBottom: 'var(--space-32)' }}>
      <motion.h3 
        variants={itemVariants}
        style={{
          fontSize: 'var(--font-size-2xl)',
          fontWeight: 'bold',
          marginBottom: 'var(--space-24)',
          textAlign: 'center',
          color: '#38bdf8',
          textTransform: 'uppercase',
          letterSpacing: '2px'
        }}
      >
        {title}
      </motion.h3>

      <motion.div 
        style={{
          display: 'grid',
          gridTemplateColumns: type === 'programming' 
            ? 'repeat(auto-fit, minmax(250px, 1fr))'
            : 'repeat(auto-fit, minmax(200px, 1fr))',
          gap: 'var(--space-24)'
        }}
        variants={containerVariants}
      >
        {skills.map((skill, index) => (
          <motion.div key={skill.id} variants={itemVariants}>
            <Card hover style={{ height: '100%', textAlign: 'center' }}>
              <CardBody>
                <div style={{
                  width: '70px',
                  height: '70px',
                  background: 'linear-gradient(135deg, rgba(56, 189, 248, 0.2), rgba(16, 185, 129, 0.2))',
                  borderRadius: '50%',
                  display: 'flex',
                  alignItems: 'center',
                  justifyContent: 'center',
                  margin: '0 auto var(--space-16)',
                  fontSize: '2rem'
                }}>
                  {skill.icon}
                </div>
                
                <h4 style={{
                  fontSize: 'var(--font-size-xl)',
                  fontWeight: 'bold',
                  color: 'var(--color-text)',
                  marginBottom: 'var(--space-8)'
                }}>
                  {skill.name}
                </h4>
                
                <p style={{
                  color: 'var(--color-text-secondary)',
                  marginBottom: 'var(--space-16)',
                  fontSize: 'var(--font-size-base)'
                }}>
                  {skill.description}
                </p>
                
                {type === 'programming' && (
                  <ProgressRing 
                    percentage={skill.level}
                    size={70}
                    isAnimated={isVisible}
                    showText={true}
                  />
                )}
                
                {type !== 'programming' && (
                  <div style={{ 
                    width: '100%', 
                    background: 'rgba(255, 255, 255, 0.1)', 
                    borderRadius: 'var(--radius-full)', 
                    height: '8px',
                    position: 'relative',
                    overflow: 'hidden'
                  }}>
                    <motion.div 
                      style={{
                        height: '100%',
                        background: 'linear-gradient(90deg, #38bdf8, #10b981)',
                        borderRadius: 'inherit'
                      }}
                      initial={{ width: 0 }}
                      animate={isVisible ? { width: `${skill.level}%` } : {}}
                      transition={{ duration: 1.5, delay: 0.5 + index * 0.1 }}
                    />
                    <span style={{
                      position: 'absolute',
                      right: '8px',
                      top: '-24px',
                      fontSize: '12px',
                      fontWeight: 'bold',
                      color: '#38bdf8'
                    }}>
                      {skill.level}%
                    </span>
                  </div>
                )}
              </CardBody>
            </Card>
          </motion.div>
        ))}
      </motion.div>
    </div>
  )

  return (
    <section 
      ref={skillsRef as React.RefObject<HTMLElement>}
      id="skills"
      style={{ 
        minHeight: '100vh',
        padding: '120px 0',
        background: 'radial-gradient(ellipse at center, rgba(139, 92, 246, 0.1) 0%, transparent 50%)'
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
            Skills & Technologies
          </motion.h2>
          <motion.p 
            variants={itemVariants}
            style={{ 
              fontSize: 'var(--font-size-xl)',
              color: 'var(--color-text-secondary)'
            }}
          >
            Cutting-edge tools and technologies
          </motion.p>
        </motion.div>

        <motion.div
          initial="hidden"
          animate={isVisible ? "visible" : "hidden"}
          variants={containerVariants}
        >
          <SkillCategory
            title="Programming Languages"
            skills={programmingSkills}
            type="programming"
          />

          <SkillCategory
            title="Tools & Software"
            skills={toolsSkills}
            type="tools"
          />

          <SkillCategory
            title="Domain Expertise"
            skills={domainSkills}
            type="domains"
          />
        </motion.div>
      </div>
    </section>
  )
}

export default Skills
```

## Step 5: Update Main Page with New Components

### 5.1 Update Main Page

**File: `src/app/page.tsx`** (replace existing content)
```tsx
'use client'

import Header from '@/components/layout/Header'
import Projects from '@/components/sections/Projects'
import Skills from '@/components/sections/Skills'

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

        {/* Skills Section - Now using the new component */}
        <Skills />

        {/* Projects Section - Now using the new component */}
        <Projects />

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
              Achievement cards will be added in the next phase...
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
              Contact cards will be added in the next phase...
            </p>
          </div>
        </section>
      </main>
    </>
  )
}
```

## Step 6: Test Your Progress

### 6.1 Run and Test
```bash
npm run dev
```

### 6.2 Verify Everything Works

1. Open `http://localhost:3000`
2. You should see:
   - Navigation still working properly
   - Skills section with categorized cards and progress rings
   - Projects section with detailed project cards showing:
     - Status badges (ongoing/completed)
     - Award badges for winning projects
     - Company badges for internship projects
     - Technology tags
     - Action buttons (View Details, GitHub)
   - Smooth animations when scrolling into view
   - Responsive design on mobile

### 6.3 Test Interactions

1. Scroll to Skills section - progress rings should animate
2. Scroll to Projects section - cards should animate in
3. Hover over cards to see hover effects
4. Test on mobile - everything should be responsive

## What You've Accomplished

✅ Added structured TypeScript data for projects, skills, achievements  
✅ Created reusable UI components (Button, Card, ProgressRing)  
✅ Built Projects section with detailed project cards  
✅ Built Skills section with animated progress indicators  
✅ Added intersection observer for scroll-based animations  
✅ Implemented hover effects and micro-interactions  
✅ Maintained responsive design across all devices  

## Next Phase Preview

In Phase 4, we'll add:
- Achievements section with timeline cards
- Contact section with interactive contact methods
- Footer with social links
- Enhanced animations and transitions
- Loading states and error handling

The portfolio now displays real data and looks professional!