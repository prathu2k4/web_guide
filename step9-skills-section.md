# Step 9: Skills Section - Complete Implementation

## 9.1 Skills Component (src/components/sections/Skills.tsx)

```tsx
'use client'

import { useState } from 'react'
import { motion } from 'framer-motion'
import { useIntersectionObserver } from '@/lib/hooks/useIntersectionObserver'
import { Card, CardBody } from '@/components/ui/Card'
import ProgressRing from '@/components/ui/ProgressRing'
import { skillsData } from '@/lib/data'
import type { Skill } from '@/types'
import styles from './Skills.module.css'

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

  return (
    <section 
      ref={skillsRef as React.RefObject<HTMLElement>} 
      className={`${styles.skills} section-animate`} 
      id="skills"
    >
      <div className="container">
        {/* Section Header */}
        <motion.div
          className="section-header"
          initial="hidden"
          animate={isVisible ? "visible" : "hidden"}
          variants={containerVariants}
        >
          <motion.h2 variants={itemVariants} className="section-title">
            Skills & Technologies
          </motion.h2>
          <motion.p variants={itemVariants} className="section-subtitle">
            Cutting-edge tools and technologies
          </motion.p>
        </motion.div>

        <div className={styles.skillsContent}>
          {/* Programming Languages */}
          <SkillCategory
            title="Programming Languages"
            skills={programmingSkills}
            isVisible={isVisible}
            delay={0.5}
            type="programming"
          />

          {/* Tools & Software */}
          <SkillCategory
            title="Tools & Software"
            skills={toolsSkills}
            isVisible={isVisible}
            delay={0.8}
            type="tools"
          />

          {/* Domain Expertise */}
          <SkillCategory
            title="Domain Expertise"
            skills={domainSkills}
            isVisible={isVisible}
            delay={1.1}
            type="domains"
          />
        </div>
      </div>
    </section>
  )
}

// Skill Category Component
interface SkillCategoryProps {
  title: string
  skills: Skill[]
  isVisible: boolean
  delay: number
  type: 'programming' | 'tools' | 'domains'
}

const SkillCategory = ({ 
  title, 
  skills, 
  isVisible, 
  delay, 
  type 
}: SkillCategoryProps) => {
  const categoryVariants = {
    hidden: { opacity: 0, y: 50 },
    visible: {
      opacity: 1,
      y: 0,
      transition: {
        duration: 0.8,
        delay,
        staggerChildren: 0.1,
        delayChildren: delay + 0.2
      }
    }
  }

  const skillVariants = {
    hidden: { opacity: 0, y: 30, scale: 0.8 },
    visible: {
      opacity: 1,
      y: 0,
      scale: 1,
      transition: {
        duration: 0.6,
        type: "spring",
        stiffness: 200
      }
    }
  }

  return (
    <motion.div 
      className={styles.skillCategory}
      initial="hidden"
      animate={isVisible ? "visible" : "hidden"}
      variants={categoryVariants}
    >
      <motion.h3 variants={skillVariants}>
        {title}
      </motion.h3>

      <motion.div 
        className={`${styles.skillGrid} ${styles[`${type}Grid`]}`}
        variants={categoryVariants}
      >
        {skills.map((skill, index) => (
          <SkillCard
            key={skill.name}
            skill={skill}
            index={index}
            isVisible={isVisible}
            type={type}
          />
        ))}
      </motion.div>
    </motion.div>
  )
}

// Individual Skill Card Component
interface SkillCardProps {
  skill: Skill
  index: number
  isVisible: boolean
  type: 'programming' | 'tools' | 'domains'
}

const SkillCard = ({ skill, index, isVisible, type }: SkillCardProps) => {
  const [isHovered, setIsHovered] = useState(false)

  const cardVariants = {
    hidden: { opacity: 0, y: 30, rotateY: -15 },
    visible: {
      opacity: 1,
      y: 0,
      rotateY: 0,
      transition: {
        duration: 0.6,
        delay: 0.1 * index,
        type: "spring",
        stiffness: 200
      }
    }
  }

  const iconVariants = {
    hidden: { scale: 0, rotate: -180 },
    visible: {
      scale: 1,
      rotate: 0,
      transition: {
        duration: 0.5,
        delay: 0.3 + 0.1 * index,
        type: "spring",
        stiffness: 300
      }
    }
  }

  return (
    <motion.div
      variants={cardVariants}
      className={`${styles.skillCard} ${styles[`${type}Card`]}`}
      onHoverStart={() => setIsHovered(true)}
      onHoverEnd={() => setIsHovered(false)}
      whileHover={{ 
        y: -15, 
        scale: 1.02,
        rotateX: 5,
        rotateY: 5,
        transition: { duration: 0.3 }
      }}
    >
      <Card className={styles.cardInner} glass>
        <CardBody className={styles.skillCardBody}>
          <motion.div 
            className={styles.skillIcon}
            variants={iconVariants}
            whileHover={{ 
              rotate: type === 'programming' ? 360 : 0,
              scale: 1.1
            }}
            transition={{ duration: 0.4 }}
          >
            <span className={styles.iconEmoji}>{skill.icon}</span>
          </motion.div>
          
          <h4 className={styles.skillName}>{skill.name}</h4>
          <p className={styles.skillDescription}>{skill.description}</p>
          
          {type === 'programming' && (
            <div className={styles.skillProgress}>
              <ProgressRing 
                percentage={skill.level}
                size={70}
                isAnimated={isVisible}
                showText={true}
              />
            </div>
          )}
          
          {type !== 'programming' && (
            <motion.div 
              className={styles.skillLevel}
              initial={{ width: 0 }}
              animate={isVisible ? { width: `${skill.level}%` } : {}}
              transition={{ duration: 1.5, delay: 0.5 + index * 0.1 }}
            >
              <div className={styles.skillLevelBg} />
              <motion.div 
                className={styles.skillLevelFill}
                initial={{ scaleX: 0 }}
                animate={isVisible ? { scaleX: 1 } : {}}
                transition={{ duration: 1.2, delay: 0.7 + index * 0.1 }}
                style={{ originX: 0 }}
              />
              <span className={styles.skillPercentage}>{skill.level}%</span>
            </motion.div>
          )}

          {/* Hover overlay */}
          <motion.div
            className={styles.skillOverlay}
            initial={{ opacity: 0 }}
            animate={{ opacity: isHovered ? 1 : 0 }}
            transition={{ duration: 0.3 }}
          >
            <div className={styles.overlayContent}>
              <span className={styles.overlayIcon}>{skill.icon}</span>
              <span className={styles.overlayText}>Proficiency: {skill.level}%</span>
            </div>
          </motion.div>
        </CardBody>
      </Card>
    </motion.div>
  )
}

export default Skills
```

## 9.2 Skills Styles (src/components/sections/Skills.module.css)

```css
.skills {
  background: radial-gradient(ellipse at center, rgba(139, 92, 246, 0.1) 0%, transparent 50%);
  position: relative;
  overflow: hidden;
}

.skills::before {
  content: '';
  position: absolute;
  top: 20%;
  left: 10%;
  width: 300px;
  height: 300px;
  background: radial-gradient(circle, rgba(0, 212, 255, 0.05) 0%, transparent 70%);
  border-radius: 50%;
  pointer-events: none;
}

.skills::after {
  content: '';
  position: absolute;
  bottom: 10%;
  right: 10%;
  width: 400px;
  height: 400px;
  background: radial-gradient(circle, rgba(16, 185, 129, 0.05) 0%, transparent 70%);
  border-radius: 50%;
  pointer-events: none;
}

.skillsContent {
  max-width: 1400px;
  margin: 0 auto;
  position: relative;
  z-index: 2;
}

.skillCategory {
  margin-bottom: var(--space-32);
}

.skillCategory h3 {
  font-size: var(--font-size-2xl);
  font-weight: var(--font-weight-bold);
  margin-bottom: var(--space-24);
  text-align: center;
  color: #00d4ff;
  text-transform: uppercase;
  letter-spacing: 2px;
  position: relative;
}

.skillCategory h3::after {
  content: '';
  position: absolute;
  bottom: -8px;
  left: 50%;
  transform: translateX(-50%);
  width: 60px;
  height: 2px;
  background: linear-gradient(90deg, #00d4ff, #10B981);
}

.skillGrid {
  display: grid;
  gap: var(--space-24);
}

.programmingGrid {
  grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
}

.toolsGrid {
  grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
}

.domainsGrid {
  grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
}

.skillCard {
  position: relative;
  height: 100%;
  perspective: 1000px;
}

.cardInner {
  height: 100%;
  background: rgba(255, 255, 255, 0.02);
  border: 1px solid rgba(255, 255, 255, 0.1);
  transition: all 0.4s ease;
  position: relative;
  overflow: hidden;
}

.programmingCard .cardInner {
  background: linear-gradient(135deg, 
    rgba(0, 212, 255, 0.05) 0%, 
    rgba(16, 185, 129, 0.05) 50%, 
    rgba(139, 92, 246, 0.05) 100%);
}

.toolsCard .cardInner {
  background: linear-gradient(135deg, 
    rgba(16, 185, 129, 0.05) 0%, 
    rgba(0, 212, 255, 0.05) 100%);
}

.domainsCard .cardInner {
  background: linear-gradient(135deg, 
    rgba(139, 92, 246, 0.05) 0%, 
    rgba(0, 212, 255, 0.05) 100%);
}

.skillCard:hover .cardInner {
  background: rgba(0, 212, 255, 0.1);
  border-color: rgba(0, 212, 255, 0.3);
  box-shadow: 0 20px 60px rgba(0, 212, 255, 0.2);
}

.skillCardBody {
  display: flex;
  flex-direction: column;
  align-items: center;
  text-align: center;
  padding: var(--space-32);
  height: 100%;
  position: relative;
}

.skillIcon {
  width: 80px;
  height: 80px;
  margin-bottom: var(--space-16);
  background: linear-gradient(135deg, rgba(0, 212, 255, 0.2), rgba(16, 185, 129, 0.2));
  border-radius: var(--radius-full);
  display: flex;
  align-items: center;
  justify-content: center;
  position: relative;
  z-index: 2;
  transition: all 0.4s ease;
}

.skillCard:hover .skillIcon {
  background: linear-gradient(135deg, #00d4ff, #10B981);
}

.iconEmoji {
  font-size: 2rem;
  filter: drop-shadow(0 0 10px rgba(0, 212, 255, 0.3));
}

.skillName {
  font-size: var(--font-size-xl);
  font-weight: var(--font-weight-bold);
  color: var(--color-gray-200);
  margin-bottom: var(--space-8);
  position: relative;
  z-index: 2;
}

.skillDescription {
  color: var(--color-gray-400);
  margin-bottom: var(--space-16);
  position: relative;
  z-index: 2;
  font-size: var(--font-size-base);
  line-height: 1.5;
  flex-grow: 1;
}

.skillProgress {
  position: relative;
  z-index: 2;
  margin-top: auto;
}

.skillLevel {
  width: 100%;
  position: relative;
  height: 8px;
  background: rgba(255, 255, 255, 0.1);
  border-radius: var(--radius-full);
  overflow: hidden;
  margin-top: auto;
}

.skillLevelBg {
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: rgba(255, 255, 255, 0.1);
  border-radius: inherit;
}

.skillLevelFill {
  position: absolute;
  top: 0;
  left: 0;
  bottom: 0;
  background: linear-gradient(90deg, #00d4ff, #10B981);
  border-radius: inherit;
  width: 100%;
}

.skillPercentage {
  position: absolute;
  top: -24px;
  right: 0;
  font-size: var(--font-size-sm);
  font-weight: var(--font-weight-semibold);
  color: #00d4ff;
}

.skillOverlay {
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: linear-gradient(135deg, rgba(0, 212, 255, 0.9), rgba(16, 185, 129, 0.9));
  display: flex;
  align-items: center;
  justify-content: center;
  border-radius: var(--radius-lg);
  pointer-events: none;
}

.overlayContent {
  text-align: center;
  color: white;
}

.overlayIcon {
  font-size: 2.5rem;
  display: block;
  margin-bottom: var(--space-8);
  filter: drop-shadow(0 0 10px rgba(0, 0, 0, 0.3));
}

.overlayText {
  font-weight: var(--font-weight-semibold);
  text-shadow: 0 2px 4px rgba(0, 0, 0, 0.3);
}

/* Light theme adjustments */
[data-theme="light"] .skills {
  background: radial-gradient(ellipse at center, rgba(33, 128, 141, 0.08) 0%, transparent 50%);
}

[data-theme="light"] .skillCategory h3 {
  color: var(--color-primary);
}

[data-theme="light"] .skillName {
  color: var(--color-text);
}

[data-theme="light"] .skillDescription {
  color: var(--color-text-secondary);
}

[data-theme="light"] .skillPercentage {
  color: var(--color-primary);
}

[data-theme="light"] .cardInner {
  background: rgba(255, 255, 255, 0.8);
  border: 1px solid rgba(0, 0, 0, 0.1);
}

[data-theme="light"] .skillCard:hover .cardInner {
  background: rgba(33, 128, 141, 0.1);
  border-color: rgba(33, 128, 141, 0.3);
  box-shadow: 0 20px 60px rgba(33, 128, 141, 0.15);
}

[data-theme="light"] .skillIcon {
  background: linear-gradient(135deg, rgba(33, 128, 141, 0.2), rgba(16, 185, 129, 0.2));
}

[data-theme="light"] .skillCard:hover .skillIcon {
  background: linear-gradient(135deg, var(--color-primary), #10B981);
}

/* Responsive Design */
@media (max-width: 1024px) {
  .programmingGrid {
    grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
  }
  
  .domainsGrid {
    grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
  }
}

@media (max-width: 768px) {
  .programmingGrid,
  .toolsGrid,
  .domainsGrid {
    grid-template-columns: repeat(auto-fit, minmax(180px, 1fr));
  }
  
  .skillCardBody {
    padding: var(--space-24);
  }
  
  .skillIcon {
    width: 60px;
    height: 60px;
  }
  
  .iconEmoji {
    font-size: 1.5rem;
  }
  
  .skillName {
    font-size: var(--font-size-lg);
  }
  
  .skillDescription {
    font-size: var(--font-size-sm);
  }
}

@media (max-width: 640px) {
  .programmingGrid,
  .toolsGrid,
  .domainsGrid {
    grid-template-columns: 1fr;
    max-width: 300px;
    margin: 0 auto;
  }
  
  .skillCategory {
    margin-bottom: var(--space-24);
  }
  
  .skillCard {
    max-width: 280px;
    margin: 0 auto;
  }
}

@media (max-width: 480px) {
  .skillCardBody {
    padding: var(--space-20);
  }
  
  .skillCategory h3 {
    font-size: var(--font-size-xl);
  }
}

/* Accessibility */
@media (prefers-reduced-motion: reduce) {
  .skillCard {
    transition: none;
  }
  
  .skillIcon {
    transition: none;
  }
}

/* Print styles */
@media print {
  .skills {
    background: white;
  }
  
  .skillGrid {
    grid-template-columns: repeat(2, 1fr) !important;
  }
  
  .skillOverlay {
    display: none;
  }
}

/* High contrast mode */
@media (prefers-contrast: high) {
  .cardInner {
    border-width: 2px;
  }
  
  .skillIcon {
    border: 2px solid #00d4ff;
  }
}
```

## 9.3 Enhanced ProgressRing Component (Update to src/components/ui/ProgressRing.tsx)

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
  delay?: number
}

const ProgressRing = ({
  percentage,
  size = 80,
  strokeWidth = 4,
  isAnimated = false,
  showText = true,
  className = '',
  delay = 0
}: ProgressRingProps) => {
  const [animatedPercentage, setAnimatedPercentage] = useState(0)
  const radius = (size - strokeWidth) / 2
  const circumference = 2 * Math.PI * radius
  
  useEffect(() => {
    if (isAnimated) {
      const timer = setTimeout(() => {
        let start = 0
        const duration = 1500
        const startTime = Date.now()
        
        const animate = () => {
          const elapsed = Date.now() - startTime
          const progress = Math.min(elapsed / duration, 1)
          
          // Easing function
          const easeOutCubic = (t: number) => 1 - Math.pow(1 - t, 3)
          const easedProgress = easeOutCubic(progress)
          
          setAnimatedPercentage(easedProgress * percentage)
          
          if (progress < 1) {
            requestAnimationFrame(animate)
          }
        }
        
        requestAnimationFrame(animate)
      }, delay * 1000)
      
      return () => clearTimeout(timer)
    }
  }, [isAnimated, percentage, delay])

  const strokeDashoffset = circumference - (animatedPercentage / 100) * circumference

  return (
    <div className={`relative inline-block ${className}`}>
      <svg width={size} height={size} className="transform -rotate-90">
        <defs>
          <linearGradient id={`progressGradient-${size}`} x1="0%" y1="0%" x2="100%" y2="100%">
            <stop offset="0%" stopColor="#00d4ff" />
            <stop offset="50%" stopColor="#10B981" />
            <stop offset="100%" stopColor="#8B5CF6" />
          </linearGradient>
          
          <filter id={`glow-${size}`}>
            <feGaussianBlur stdDeviation="2" result="coloredBlur"/>
            <feMerge> 
              <feMergeNode in="coloredBlur"/>
              <feMergeNode in="SourceGraphic"/>
            </feMerge>
          </filter>
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
          stroke={`url(#progressGradient-${size})`}
          strokeWidth={strokeWidth}
          fill="none"
          strokeLinecap="round"
          strokeDasharray={circumference}
          strokeDashoffset={strokeDashoffset}
          filter={`url(#glow-${size})`}
          style={{ 
            transition: 'stroke-dashoffset 0.1s ease-out'
          }}
        />
      </svg>
      
      {showText && (
        <motion.div
          className="absolute inset-0 flex items-center justify-center"
          initial={{ opacity: 0, scale: 0.8 }}
          animate={isAnimated ? { 
            opacity: 1, 
            scale: 1 
          } : { 
            opacity: 0, 
            scale: 0.8 
          }}
          transition={{ delay: delay + 0.5, duration: 0.3 }}
        >
          <span 
            className="font-bold"
            style={{ 
              color: '#00d4ff',
              fontSize: `${size * 0.15}px`,
              textShadow: '0 0 10px rgba(0, 212, 255, 0.5)'
            }}
          >
            {Math.round(animatedPercentage)}%
          </span>
        </motion.div>
      )}
      
      {/* Pulsing effect for high percentages */}
      {percentage >= 80 && isAnimated && (
        <motion.div
          className="absolute inset-0 rounded-full"
          style={{
            background: 'radial-gradient(circle, rgba(0, 212, 255, 0.1) 0%, transparent 70%)',
            filter: 'blur(2px)'
          }}
          animate={{ 
            scale: [1, 1.2, 1],
            opacity: [0.5, 1, 0.5]
          }}
          transition={{
            duration: 2,
            repeat: Infinity,
            delay: delay + 1.5
          }}
        />
      )}
    </div>
  )
}

export default ProgressRing
```