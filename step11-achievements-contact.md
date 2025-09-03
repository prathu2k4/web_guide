# Step 11: Achievements & Contact Sections

## 11.1 Achievements Component (src/components/sections/Achievements.tsx)

```tsx
'use client'

import { motion } from 'framer-motion'
import { useIntersectionObserver } from '@/lib/hooks/useIntersectionObserver'
import { Card, CardBody } from '@/components/ui/Card'
import { achievementsData } from '@/lib/data'
import { Zap, Award, Users, Briefcase, Trophy } from 'lucide-react'
import styles from './Achievements.module.css'

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
        return <Trophy size={32} />
      default:
        return <Award size={32} />
    }
  }

  return (
    <section 
      ref={achievementsRef as React.RefObject<HTMLElement>}
      className={`${styles.achievements} section-animate`}
      id="achievements"
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
            Achievements
          </motion.h2>
          <motion.p variants={itemVariants} className="section-subtitle">
            Milestones and accomplishments
          </motion.p>
        </motion.div>

        {/* Achievements Grid */}
        <motion.div
          className={styles.achievementsGrid}
          initial="hidden"
          animate={isVisible ? "visible" : "hidden"}
          variants={containerVariants}
        >
          {achievementsData.map((achievement, index) => (
            <motion.div
              key={achievement.id}
              variants={itemVariants}
              custom={index}
            >
              <AchievementCard 
                achievement={achievement} 
                index={index}
                isVisible={isVisible}
              />
            </motion.div>
          ))}
        </motion.div>
      </div>
    </section>
  )
}

// Achievement Card Component
interface AchievementCardProps {
  achievement: typeof achievementsData[0]
  index: number
  isVisible: boolean
}

const AchievementCard = ({ achievement, index, isVisible }: AchievementCardProps) => {
  const getAchievementIcon = () => {
    switch (achievement.icon) {
      case '🔬':
        return <Zap size={32} />
      case '🧠':
        return <Users size={32} />
      case '🏆':
        return <Trophy size={32} />
      default:
        return <Award size={32} />
    }
  }

  const getCardGradient = () => {
    const gradients = [
      'linear-gradient(135deg, rgba(0, 212, 255, 0.05), rgba(16, 185, 129, 0.05))',
      'linear-gradient(135deg, rgba(16, 185, 129, 0.05), rgba(139, 92, 246, 0.05))',
      'linear-gradient(135deg, rgba(139, 92, 246, 0.05), rgba(0, 212, 255, 0.05))'
    ]
    return gradients[index % gradients.length]
  }

  const getIconGradient = () => {
    const gradients = [
      'linear-gradient(135deg, rgba(0, 212, 255, 0.2), rgba(16, 185, 129, 0.2))',
      'linear-gradient(135deg, rgba(16, 185, 129, 0.2), rgba(139, 92, 246, 0.2))',
      'linear-gradient(135deg, rgba(139, 92, 246, 0.2), rgba(0, 212, 255, 0.2))'
    ]
    return gradients[index % gradients.length]
  }

  const getHoverIconGradient = () => {
    const gradients = [
      'linear-gradient(135deg, #00d4ff, #10B981)',
      'linear-gradient(135deg, #10B981, #8B5CF6)',
      'linear-gradient(135deg, #8B5CF6, #00d4ff)'
    ]
    return gradients[index % gradients.length]
  }

  return (
    <motion.div
      className={styles.achievementCard}
      whileHover={{ 
        y: -10, 
        scale: 1.02,
        rotateX: 2
      }}
      style={{ perspective: 1000 }}
    >
      <Card 
        className={styles.cardInner} 
        glass 
        style={{ background: getCardGradient() }}
      >
        <CardBody className={styles.achievementCardBody}>
          <motion.div 
            className={styles.achievementIcon}
            style={{ background: getIconGradient() }}
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
              background: getHoverIconGradient(),
              rotate: 360
            }}
          >
            {getAchievementIcon()}
          </motion.div>
          
          <div className={styles.achievementContent}>
            <h3 className={styles.achievementTitle}>
              {achievement.title}
            </h3>
            <div className={styles.achievementRole}>
              {achievement.role} • {achievement.period}
            </div>
            <p className={styles.achievementDescription}>
              {achievement.description}
            </p>
            
            {achievement.status === 'ongoing' && (
              <motion.div
                className={styles.ongoingBadge}
                animate={{ 
                  boxShadow: [
                    '0 0 10px rgba(0, 212, 255, 0.3)',
                    '0 0 20px rgba(0, 212, 255, 0.6)',
                    '0 0 10px rgba(0, 212, 255, 0.3)'
                  ]
                }}
                transition={{ duration: 2, repeat: Infinity }}
              >
                <span className="status status--ongoing">
                  Currently Active
                </span>
              </motion.div>
            )}
          </div>
        </CardBody>
      </Card>
    </motion.div>
  )
}

export default Achievements
```

## 11.2 Contact Component (src/components/sections/Contact.tsx)

```tsx
'use client'

import { motion } from 'framer-motion'
import { useIntersectionObserver } from '@/lib/hooks/useIntersectionObserver'
import { Card, CardBody } from '@/components/ui/Card'
import { Button } from '@/components/ui/Button'
import { contactData } from '@/lib/data'
import { Mail, MapPin, Linkedin, ExternalLink, Copy, Check } from 'lucide-react'
import { useState } from 'react'
import styles from './Contact.module.css'

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
        return <ExternalLink size={24} />
    }
  }

  return (
    <section 
      ref={contactRef as React.RefObject<HTMLElement>}
      className={`${styles.contact} section-animate`}
      id="contact"
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
            Let's Connect
          </motion.h2>
          <motion.p variants={itemVariants} className="section-subtitle">
            Ready to collaborate on innovative projects
          </motion.p>
        </motion.div>

        {/* Contact Cards */}
        <motion.div
          className={styles.contactCards}
          initial="hidden"
          animate={isVisible ? "visible" : "hidden"}
          variants={containerVariants}
        >
          {contactData.map((contact, index) => (
            <ContactCard
              key={contact.id}
              contact={contact}
              index={index}
              isVisible={isVisible}
              onCopyEmail={contact.type === 'email' ? handleCopyEmail : undefined}
              copiedEmail={copiedEmail}
            />
          ))}
        </motion.div>

        {/* Call to Action */}
        <motion.div
          className={styles.contactCTA}
          initial="hidden"
          animate={isVisible ? "visible" : "hidden"}
          variants={containerVariants}
        >
          <motion.div variants={itemVariants}>
            <Card className={styles.ctaCard} glass>
              <CardBody className={styles.ctaContent}>
                <h3 className={styles.ctaTitle}>
                  Ready to bring your ideas to life?
                </h3>
                <p className={styles.ctaDescription}>
                  Whether it's digital design, systems programming, or innovative automation solutions, 
                  I'm always excited to collaborate on projects that push the boundaries of technology.
                </p>
                <div className={styles.ctaButtons}>
                  <Button
                    variant="primary"
                    size="lg"
                    glow
                    onClick={() => window.open('mailto:prathu2k4@gmail.com', '_blank')}
                    className="flex items-center gap-2"
                  >
                    <Mail size={20} />
                    Start a Conversation
                  </Button>
                  <Button
                    variant="outline"
                    size="lg"
                    onClick={() => window.open('https://linkedin.com/in/pratham-jain', '_blank')}
                    className="flex items-center gap-2"
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

// Contact Card Component
interface ContactCardProps {
  contact: typeof contactData[0]
  index: number
  isVisible: boolean
  onCopyEmail?: () => void
  copiedEmail?: boolean
}

const ContactCard = ({ 
  contact, 
  index, 
  isVisible, 
  onCopyEmail, 
  copiedEmail 
}: ContactCardProps) => {
  const cardVariants = {
    hidden: { 
      opacity: 0, 
      y: 30,
      rotateY: -15
    },
    visible: { 
      opacity: 1, 
      y: 0,
      rotateY: 0,
      transition: {
        duration: 0.6,
        delay: 0.2 + index * 0.1,
        type: "spring",
        stiffness: 200
      }
    }
  }

  const iconVariants = {
    hidden: { scale: 0, rotate: -90 },
    visible: {
      scale: 1,
      rotate: 0,
      transition: {
        duration: 0.5,
        delay: 0.4 + index * 0.1,
        type: "spring",
        stiffness: 300
      }
    }
  }

  const getContactIcon = () => {
    switch (contact.type) {
      case 'email':
        return <Mail size={24} />
      case 'location':
        return <MapPin size={24} />
      case 'linkedin':
        return <Linkedin size={24} />
      default:
        return <ExternalLink size={24} />
    }
  }

  const handleClick = () => {
    if (contact.type === 'email' && onCopyEmail) {
      onCopyEmail()
    } else if (contact.href !== '#') {
      window.open(contact.href, '_blank', 'noopener,noreferrer')
    }
  }

  return (
    <motion.div
      className={styles.contactCard}
      variants={cardVariants}
      whileHover={{ 
        y: -8, 
        scale: 1.05,
        rotateX: 2
      }}
      style={{ perspective: 1000 }}
    >
      <Card className={styles.cardInner} glass onClick={handleClick}>
        <CardBody className={styles.contactCardBody}>
          <motion.div 
            className={styles.contactIcon}
            variants={iconVariants}
            whileHover={{ 
              scale: 1.1,
              rotate: contact.type === 'email' ? 0 : 15
            }}
          >
            {getContactIcon()}
          </motion.div>
          
          <h4 className={styles.contactTitle}>
            {contact.title}
          </h4>
          
          <p className={styles.contactValue}>
            {contact.value}
          </p>
          
          {contact.type === 'email' && (
            <Button
              variant="outline"
              size="sm"
              onClick={(e) => {
                e.stopPropagation()
                onCopyEmail?.()
              }}
              className={`${styles.copyButton} flex items-center gap-2`}
            >
              {copiedEmail ? (
                <>
                  <Check size={16} />
                  Copied!
                </>
              ) : (
                <>
                  <Copy size={16} />
                  Copy
                </>
              )}
            </Button>
          )}
          
          {contact.type === 'linkedin' && (
            <Button
              variant="primary"
              size="sm"
              onClick={handleClick}
              className="flex items-center gap-2"
            >
              <ExternalLink size={16} />
              Connect
            </Button>
          )}

          {/* Hover Effect Background */}
          <motion.div
            className={styles.contactHover}
            initial={{ opacity: 0, scale: 0.8 }}
            whileHover={{ opacity: 0.1, scale: 1 }}
            transition={{ duration: 0.3 }}
          />
        </CardBody>
      </Card>
    </motion.div>
  )
}

export default Achievements

// Also need to export Contact
export { Contact }
```

## 11.3 Achievements Styles (src/components/sections/Achievements.module.css)

```css
.achievements {
  background: radial-gradient(ellipse at center, rgba(16, 185, 129, 0.1) 0%, transparent 50%);
  position: relative;
  overflow: hidden;
}

.achievements::before {
  content: '';
  position: absolute;
  top: 20%;
  right: 10%;
  width: 400px;
  height: 400px;
  background: radial-gradient(circle, rgba(16, 185, 129, 0.05) 0%, transparent 70%);
  border-radius: 50%;
  pointer-events: none;
}

.achievements::after {
  content: '';
  position: absolute;
  bottom: 20%;
  left: 10%;
  width: 300px;
  height: 300px;
  background: radial-gradient(circle, rgba(139, 92, 246, 0.05) 0%, transparent 70%);
  border-radius: 50%;
  pointer-events: none;
}

.achievementsGrid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(350px, 1fr));
  gap: var(--space-32);
  max-width: 1200px;
  margin: 0 auto;
  position: relative;
  z-index: 2;
}

.achievementCard {
  height: 100%;
  cursor: pointer;
  transform-style: preserve-3d;
}

.cardInner {
  height: 100%;
  border: 1px solid rgba(255, 255, 255, 0.1);
  transition: all 0.4s ease;
  position: relative;
  overflow: hidden;
}

.achievementCard:hover .cardInner {
  border-color: rgba(16, 185, 129, 0.3);
  box-shadow: 0 20px 60px rgba(16, 185, 129, 0.2);
}

.achievementCardBody {
  display: flex;
  flex-direction: column;
  align-items: center;
  text-align: center;
  padding: var(--space-32);
  height: 100%;
  position: relative;
}

.achievementIcon {
  width: 80px;
  height: 80px;
  margin-bottom: var(--space-20);
  border-radius: var(--radius-full);
  display: flex;
  align-items: center;
  justify-content: center;
  color: #10B981;
  transition: all 0.4s ease;
  position: relative;
  z-index: 2;
}

.achievementCard:hover .achievementIcon {
  color: var(--color-charcoal-800);
}

.achievementContent {
  flex: 1;
  display: flex;
  flex-direction: column;
  justify-content: center;
  position: relative;
  z-index: 2;
}

.achievementTitle {
  font-size: var(--font-size-xl);
  font-weight: var(--font-weight-bold);
  color: var(--color-gray-200);
  margin-bottom: var(--space-8);
  line-height: 1.3;
}

.achievementRole {
  color: #10B981;
  font-weight: var(--font-weight-semibold);
  margin-bottom: var(--space-12);
  font-size: var(--font-size-lg);
  background: linear-gradient(135deg, #10B981, #00d4ff);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
}

.achievementDescription {
  color: var(--color-gray-300);
  line-height: 1.7;
  font-size: var(--font-size-base);
  margin-bottom: var(--space-16);
}

.ongoingBadge {
  align-self: center;
  margin-top: auto;
}

/* Light theme adjustments */
[data-theme="light"] .achievements {
  background: radial-gradient(ellipse at center, rgba(33, 128, 141, 0.08) 0%, transparent 50%);
}

[data-theme="light"] .cardInner {
  background: rgba(255, 255, 255, 0.8);
  border: 1px solid rgba(0, 0, 0, 0.1);
}

[data-theme="light"] .achievementCard:hover .cardInner {
  background: rgba(33, 128, 141, 0.05);
  border-color: rgba(33, 128, 141, 0.3);
  box-shadow: 0 20px 60px rgba(33, 128, 141, 0.15);
}

[data-theme="light"] .achievementTitle {
  color: var(--color-text);
}

[data-theme="light"] .achievementDescription {
  color: var(--color-text-secondary);
}

[data-theme="light"] .achievementIcon {
  color: var(--color-primary);
}

[data-theme="light"] .achievementCard:hover .achievementIcon {
  color: white;
}

/* Responsive Design */
@media (max-width: 768px) {
  .achievementsGrid {
    grid-template-columns: 1fr;
    gap: var(--space-24);
  }
  
  .achievementCardBody {
    padding: var(--space-24);
  }
  
  .achievementIcon {
    width: 60px;
    height: 60px;
  }
}

@media (max-width: 480px) {
  .achievementCardBody {
    padding: var(--space-20);
  }
  
  .achievementTitle {
    font-size: var(--font-size-lg);
  }
  
  .achievementRole {
    font-size: var(--font-size-base);
  }
}
```

## 11.4 Contact Styles (src/components/sections/Contact.module.css)

```css
.contact {
  background: linear-gradient(135deg, rgba(26, 32, 44, 0.95) 0%, rgba(45, 55, 72, 0.95) 100%);
  position: relative;
}

.contact::before {
  content: '';
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: 
    radial-gradient(ellipse at 30% 20%, rgba(0, 212, 255, 0.03) 0%, transparent 50%),
    radial-gradient(ellipse at 70% 80%, rgba(139, 92, 246, 0.03) 0%, transparent 50%);
  pointer-events: none;
}

.contactCards {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
  gap: var(--space-24);
  max-width: 1000px;
  margin: 0 auto var(--space-32);
  position: relative;
  z-index: 2;
}

.contactCard {
  height: 100%;
  cursor: pointer;
  transform-style: preserve-3d;
}

.cardInner {
  height: 100%;
  background: rgba(255, 255, 255, 0.02);
  border: 1px solid rgba(255, 255, 255, 0.1);
  transition: all 0.4s ease;
  position: relative;
  overflow: hidden;
}

.contactCard:hover .cardInner {
  background: rgba(0, 212, 255, 0.1);
  border-color: rgba(0, 212, 255, 0.3);
  box-shadow: 0 15px 50px rgba(0, 212, 255, 0.2);
}

.contactCardBody {
  display: flex;
  flex-direction: column;
  align-items: center;
  text-align: center;
  padding: var(--space-32);
  height: 100%;
  position: relative;
}

.contactIcon {
  width: 70px;
  height: 70px;
  margin-bottom: var(--space-16);
  background: linear-gradient(135deg, rgba(0, 212, 255, 0.2), rgba(16, 185, 129, 0.2));
  border-radius: var(--radius-full);
  display: flex;
  align-items: center;
  justify-content: center;
  color: #00d4ff;
  transition: all 0.4s ease;
  position: relative;
  z-index: 2;
}

.contactCard:hover .contactIcon {
  background: linear-gradient(135deg, #00d4ff, #10B981);
  color: var(--color-charcoal-800);
}

.contactTitle {
  font-size: var(--font-size-xl);
  font-weight: var(--font-weight-semibold);
  color: var(--color-gray-200);
  margin-bottom: var(--space-8);
  position: relative;
  z-index: 2;
}

.contactValue {
  color: var(--color-gray-300);
  margin-bottom: var(--space-16);
  line-height: 1.6;
  font-size: var(--font-size-base);
  position: relative;
  z-index: 2;
}

.copyButton {
  background: rgba(0, 212, 255, 0.1) !important;
  border-color: rgba(0, 212, 255, 0.3) !important;
  color: #00d4ff !important;
}

.copyButton:hover {
  background: rgba(0, 212, 255, 0.2) !important;
}

.contactHover {
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: radial-gradient(circle, rgba(0, 212, 255, 1) 0%, transparent 70%);
  border-radius: var(--radius-lg);
}

.contactCTA {
  max-width: 800px;
  margin: 0 auto;
  position: relative;
  z-index: 2;
}

.ctaCard {
  background: linear-gradient(135deg, 
    rgba(0, 212, 255, 0.05) 0%, 
    rgba(16, 185, 129, 0.05) 50%,
    rgba(139, 92, 246, 0.05) 100%);
  border: 1px solid rgba(0, 212, 255, 0.2);
}

.ctaContent {
  text-align: center;
  padding: var(--space-32);
}

.ctaTitle {
  font-size: var(--font-size-2xl);
  font-weight: var(--font-weight-bold);
  color: var(--color-gray-200);
  margin-bottom: var(--space-16);
  background: linear-gradient(135deg, #00d4ff, #10B981);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
}

.ctaDescription {
  font-size: var(--font-size-lg);
  color: var(--color-gray-300);
  line-height: 1.7;
  margin-bottom: var(--space-24);
  max-width: 600px;
  margin-left: auto;
  margin-right: auto;
}

.ctaButtons {
  display: flex;
  gap: var(--space-16);
  justify-content: center;
  flex-wrap: wrap;
}

/* Light theme adjustments */
[data-theme="light"] .contact {
  background: linear-gradient(135deg, var(--color-cream-100) 0%, var(--color-cream-50) 100%);
}

[data-theme="light"] .cardInner {
  background: rgba(255, 255, 255, 0.8);
  border: 1px solid rgba(0, 0, 0, 0.1);
}

[data-theme="light"] .contactCard:hover .cardInner {
  background: rgba(33, 128, 141, 0.05);
  border-color: rgba(33, 128, 141, 0.3);
  box-shadow: 0 15px 50px rgba(33, 128, 141, 0.15);
}

[data-theme="light"] .contactTitle {
  color: var(--color-text);
}

[data-theme="light"] .contactValue {
  color: var(--color-text-secondary);
}

[data-theme="light"] .ctaTitle {
  color: var(--color-text);
}

[data-theme="light"] .ctaDescription {
  color: var(--color-text-secondary);
}

[data-theme="light"] .contactIcon {
  background: linear-gradient(135deg, rgba(33, 128, 141, 0.2), rgba(16, 185, 129, 0.2));
  color: var(--color-primary);
}

[data-theme="light"] .contactCard:hover .contactIcon {
  background: linear-gradient(135deg, var(--color-primary), #10B981);
  color: white;
}

[data-theme="light"] .ctaCard {
  background: linear-gradient(135deg, 
    rgba(33, 128, 141, 0.05) 0%, 
    rgba(16, 185, 129, 0.05) 50%,
    rgba(139, 92, 246, 0.05) 100%);
  border: 1px solid rgba(33, 128, 141, 0.2);
}

/* Responsive Design */
@media (max-width: 768px) {
  .contactCards {
    grid-template-columns: 1fr;
    gap: var(--space-20);
  }
  
  .contactCardBody {
    padding: var(--space-24);
  }
  
  .contactIcon {
    width: 60px;
    height: 60px;
  }
  
  .ctaContent {
    padding: var(--space-24);
  }
  
  .ctaButtons {
    flex-direction: column;
    align-items: stretch;
  }
}

@media (max-width: 480px) {
  .contactCardBody {
    padding: var(--space-20);
  }
  
  .contactTitle {
    font-size: var(--font-size-lg);
  }
  
  .contactValue {
    font-size: var(--font-size-sm);
  }
  
  .ctaTitle {
    font-size: var(--font-size-xl);
  }
  
  .ctaDescription {
    font-size: var(--font-size-base);
  }
}
```