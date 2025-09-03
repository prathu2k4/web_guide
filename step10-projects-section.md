# Step 10: Projects Section - Complete Implementation

## 10.1 Projects Component (src/components/sections/Projects.tsx)

```tsx
'use client'

import { useState } from 'react'
import { motion } from 'framer-motion'
import { useIntersectionObserver } from '@/lib/hooks/useIntersectionObserver'
import { Button } from '@/components/ui/Button'
import Modal from '@/components/ui/Modal'
import ProjectCard from '@/components/ui/ProjectCard'
import { projectsData } from '@/lib/data'
import type { Project } from '@/types'
import styles from './Projects.module.css'

const Projects = () => {
  const [activeFilter, setActiveFilter] = useState('all')
  const [selectedProject, setSelectedProject] = useState<Project | null>(null)
  const [isModalOpen, setIsModalOpen] = useState(false)
  
  const { ref: projectsRef, isVisible } = useIntersectionObserver({
    threshold: 0.1,
    triggerOnce: true
  })

  const filters = [
    { id: 'all', label: 'All Projects' },
    { id: 'digital', label: 'Digital Design' },
    { id: 'systems', label: 'Systems' },
    { id: 'software', label: 'Software' }
  ]

  const filteredProjects = activeFilter === 'all' 
    ? projectsData 
    : projectsData.filter(project => project.category === activeFilter)

  const handleFilterChange = (filterId: string) => {
    setActiveFilter(filterId)
  }

  const handleProjectDetails = (project: Project) => {
    setSelectedProject(project)
    setIsModalOpen(true)
  }

  const handleModalClose = () => {
    setIsModalOpen(false)
    setSelectedProject(null)
  }

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

  const projectVariants = {
    hidden: { opacity: 0, scale: 0.8, rotateX: -15 },
    visible: (index: number) => ({
      opacity: 1,
      scale: 1,
      rotateX: 0,
      transition: {
        duration: 0.6,
        delay: index * 0.1,
        type: "spring",
        stiffness: 200
      }
    }),
    exit: {
      opacity: 0,
      scale: 0.8,
      transition: { duration: 0.3 }
    }
  }

  return (
    <section 
      ref={projectsRef as React.RefObject<HTMLElement>}
      className={`${styles.projects} section-animate`}
      id="projects"
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
            Featured Projects
          </motion.h2>
          <motion.p variants={itemVariants} className="section-subtitle">
            Innovative solutions and technical achievements
          </motion.p>
        </motion.div>

        {/* Project Filters */}
        <motion.div
          className={styles.projectFilters}
          initial="hidden"
          animate={isVisible ? "visible" : "hidden"}
          variants={containerVariants}
        >
          {filters.map((filter, index) => (
            <motion.button
              key={filter.id}
              className={`${styles.filterBtn} ${
                activeFilter === filter.id ? styles.active : ''
              }`}
              onClick={() => handleFilterChange(filter.id)}
              variants={itemVariants}
              whileHover={{ y: -2, scale: 1.05 }}
              whileTap={{ scale: 0.95 }}
              custom={index}
            >
              {filter.label}
            </motion.button>
          ))}
        </motion.div>

        {/* Projects Grid */}
        <motion.div
          className={styles.projectsGrid}
          layout
        >
          {filteredProjects.map((project, index) => (
            <motion.div
              key={project.id}
              variants={projectVariants}
              initial="hidden"
              animate="visible"
              exit="exit"
              layout
              custom={index}
            >
              <ProjectCard
                project={project}
                onDetailsClick={() => handleProjectDetails(project)}
                index={index}
                isVisible={isVisible}
              />
            </motion.div>
          ))}
        </motion.div>
      </div>

      {/* Project Details Modal */}
      <Modal
        isOpen={isModalOpen}
        onClose={handleModalClose}
        title={selectedProject?.title}
      >
        {selectedProject && (
          <ProjectModal project={selectedProject} />
        )}
      </Modal>
    </section>
  )
}

// Project Modal Content Component
interface ProjectModalProps {
  project: Project
}

const ProjectModal = ({ project }: ProjectModalProps) => {
  return (
    <div className={styles.modalContent}>
      {/* Project Header */}
      <div className={styles.modalHeader}>
        <div className={styles.modalTags}>
          <span className={`status ${project.status === 'ongoing' ? 'status--ongoing' : 'status--completed'}`}>
            {project.status}
          </span>
          {project.award && (
            <motion.span 
              className={styles.modalAward}
              animate={{ 
                boxShadow: [
                  '0 0 10px rgba(245, 158, 11, 0.5)',
                  '0 0 20px rgba(245, 158, 11, 0.8)',
                  '0 0 10px rgba(245, 158, 11, 0.5)'
                ]
              }}
              transition={{ duration: 2, repeat: Infinity }}
            >
              {project.award}
            </motion.span>
          )}
          {project.company && (
            <span className={styles.modalCompany}>
              {project.company}
            </span>
          )}
        </div>
      </div>

      {/* Project Description */}
      <div className={styles.modalBody}>
        <p className={styles.modalDescription}>
          {project.description}
        </p>

        {/* Key Highlights */}
        <div className={styles.modalSection}>
          <h3>Key Features & Highlights</h3>
          <ul className={styles.modalHighlights}>
            {project.highlights.map((highlight, index) => (
              <motion.li
                key={index}
                initial={{ opacity: 0, x: -20 }}
                animate={{ opacity: 1, x: 0 }}
                transition={{ duration: 0.4, delay: 0.1 * index }}
              >
                {highlight}
              </motion.li>
            ))}
          </ul>
        </div>

        {/* Technologies */}
        <div className={styles.modalSection}>
          <h3>Technologies Used</h3>
          <div className={styles.modalTech}>
            {project.technologies.map((tech, index) => (
              <motion.span
                key={tech}
                className="tech-tag"
                initial={{ opacity: 0, scale: 0 }}
                animate={{ opacity: 1, scale: 1 }}
                transition={{ 
                  duration: 0.3, 
                  delay: 0.05 * index,
                  type: "spring",
                  stiffness: 200 
                }}
                whileHover={{ scale: 1.05 }}
              >
                {tech}
              </motion.span>
            ))}
          </div>
        </div>

        {/* Action Buttons */}
        <div className={styles.modalActions}>
          {project.githubUrl && (
            <Button
              variant="primary"
              size="lg"
              onClick={() => window.open(project.githubUrl, '_blank')}
              className="flex items-center gap-2"
            >
              <span>View on GitHub</span>
              <svg width="16" height="16" viewBox="0 0 24 24" fill="currentColor">
                <path d="M12 0c-6.626 0-12 5.373-12 12 0 5.302 3.438 9.8 8.207 11.387.599.111.793-.261.793-.577v-2.234c-3.338.726-4.033-1.416-4.033-1.416-.546-1.387-1.333-1.756-1.333-1.756-1.089-.745.083-.729.083-.729 1.205.084 1.839 1.237 1.839 1.237 1.07 1.834 2.807 1.304 3.492.997.107-.775.418-1.305.762-1.604-2.665-.305-5.467-1.334-5.467-5.931 0-1.311.469-2.381 1.236-3.221-.124-.303-.535-1.524.117-3.176 0 0 1.008-.322 3.301 1.23.957-.266 1.983-.399 3.003-.404 1.02.005 2.047.138 3.006.404 2.291-1.552 3.297-1.23 3.297-1.23.653 1.653.242 2.874.118 3.176.77.84 1.235 1.911 1.235 3.221 0 4.609-2.807 5.624-5.479 5.921.43.372.823 1.102.823 2.222v3.293c0 .319.192.694.801.576 4.765-1.589 8.199-6.086 8.199-11.386 0-6.627-5.373-12-12-12z"/>
              </svg>
            </Button>
          )}
        </div>
      </div>
    </div>
  )
}

export default Projects
```

## 10.2 ProjectCard Component (src/components/ui/ProjectCard.tsx)

```tsx
'use client'

import { motion } from 'framer-motion'
import { ExternalLink, Github, Award, Building } from 'lucide-react'
import { Button } from '@/components/ui/Button'
import { Card, CardBody } from '@/components/ui/Card'
import type { Project } from '@/types'
import styles from './ProjectCard.module.css'

interface ProjectCardProps {
  project: Project
  onDetailsClick: () => void
  index: number
  isVisible: boolean
}

const ProjectCard = ({ project, onDetailsClick, index, isVisible }: ProjectCardProps) => {
  const cardVariants = {
    hidden: { 
      opacity: 0, 
      y: 50,
      rotateX: -15,
      scale: 0.9
    },
    visible: { 
      opacity: 1, 
      y: 0,
      rotateX: 0,
      scale: 1,
      transition: {
        duration: 0.6,
        delay: index * 0.1,
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
        delay: 0.3 + index * 0.1,
        type: "spring",
        stiffness: 300
      }
    }
  }

  const getProjectIcon = () => {
    return project.icon || '🚀'
  }

  const getStatusIcon = () => {
    if (project.award) return <Award size={16} />
    if (project.company) return <Building size={16} />
    return null
  }

  return (
    <motion.div
      className={styles.projectCard}
      variants={cardVariants}
      initial="hidden"
      animate={isVisible ? "visible" : "hidden"}
      whileHover={{ 
        y: -15, 
        scale: 1.02,
        rotateX: 2,
        rotateY: 2
      }}
      style={{ perspective: 1000 }}
    >
      <Card className={styles.cardInner} glass>
        <CardBody className={styles.projectCardBody}>
          {/* Project Header */}
          <div className={styles.projectHeader}>
            <motion.div 
              className={styles.projectIcon}
              variants={iconVariants}
              whileHover={{ 
                rotate: 360,
                scale: 1.1
              }}
              transition={{ duration: 0.4 }}
            >
              <span className={styles.iconEmoji}>
                {getProjectIcon()}
              </span>
            </motion.div>
            
            <div className={styles.projectMeta}>
              <div className={styles.projectStatus}>
                <span className={`status status--${project.status}`}>
                  {getStatusIcon()}
                  {project.status}
                </span>
              </div>
              
              {project.award && (
                <motion.div 
                  className={styles.projectAward}
                  animate={{ 
                    boxShadow: [
                      '0 0 10px rgba(245, 158, 11, 0.5)',
                      '0 0 20px rgba(245, 158, 11, 0.8)',
                      '0 0 10px rgba(245, 158, 11, 0.5)'
                    ]
                  }}
                  transition={{ duration: 2, repeat: Infinity }}
                >
                  {project.award}
                </motion.div>
              )}
              
              {project.company && (
                <div className={styles.projectCompany}>
                  <Building size={14} />
                  {project.company}
                </div>
              )}
            </div>
          </div>

          {/* Project Content */}
          <div className={styles.projectContent}>
            <h3 className={styles.projectTitle}>{project.title}</h3>
            <p className={styles.projectDescription}>{project.description}</p>
            
            {/* Technologies */}
            <div className={styles.projectTech}>
              {project.technologies.map((tech, techIndex) => (
                <motion.span
                  key={tech}
                  className="tech-tag"
                  initial={{ opacity: 0, scale: 0 }}
                  animate={isVisible ? { opacity: 1, scale: 1 } : {}}
                  transition={{ 
                    duration: 0.3, 
                    delay: 0.5 + index * 0.1 + techIndex * 0.05,
                    type: "spring",
                    stiffness: 200
                  }}
                  whileHover={{ scale: 1.05 }}
                >
                  {tech}
                </motion.span>
              ))}
            </div>
          </div>

          {/* Project Actions */}
          <div className={styles.projectActions}>
            <Button
              variant="primary"
              size="sm"
              onClick={onDetailsClick}
              className="flex items-center gap-2"
            >
              <ExternalLink size={16} />
              View Details
            </Button>
            
            {project.githubUrl && (
              <Button
                variant="outline"
                size="sm"
                onClick={() => window.open(project.githubUrl, '_blank')}
                className="flex items-center gap-2"
              >
                <Github size={16} />
                GitHub
              </Button>
            )}
          </div>

          {/* Hover Overlay Effect */}
          <motion.div
            className={styles.projectOverlay}
            initial={{ opacity: 0 }}
            whileHover={{ opacity: 1 }}
            transition={{ duration: 0.3 }}
          >
            <div className={styles.overlayContent}>
              <motion.div
                initial={{ scale: 0 }}
                whileHover={{ scale: 1 }}
                transition={{ duration: 0.2, delay: 0.1 }}
              >
                <ExternalLink size={24} />
              </motion.div>
              <span>Click to explore</span>
            </div>
          </motion.div>
        </CardBody>
      </Card>
    </motion.div>
  )
}

export default Projects
```

## 10.3 ProjectCard Styles (src/components/ui/ProjectCard.module.css)

```css
.projectCard {
  position: relative;
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

.projectCard:hover .cardInner {
  background: rgba(0, 212, 255, 0.05);
  border-color: rgba(0, 212, 255, 0.3);
  box-shadow: 0 25px 80px rgba(0, 212, 255, 0.25);
}

.projectCardBody {
  display: flex;
  flex-direction: column;
  height: 100%;
  padding: var(--space-32);
  position: relative;
}

.projectHeader {
  display: flex;
  align-items: flex-start;
  gap: var(--space-16);
  margin-bottom: var(--space-20);
}

.projectIcon {
  width: 60px;
  height: 60px;
  background: linear-gradient(135deg, rgba(0, 212, 255, 0.2), rgba(16, 185, 129, 0.2));
  border-radius: var(--radius-base);
  display: flex;
  align-items: center;
  justify-content: center;
  flex-shrink: 0;
  transition: all 0.4s ease;
  position: relative;
  z-index: 2;
}

.projectCard:hover .projectIcon {
  background: linear-gradient(135deg, #00d4ff, #10B981);
  transform: scale(1.1);
}

.iconEmoji {
  font-size: 1.5rem;
  filter: drop-shadow(0 0 10px rgba(0, 212, 255, 0.3));
}

.projectMeta {
  flex: 1;
  display: flex;
  flex-direction: column;
  gap: var(--space-8);
}

.projectStatus {
  display: flex;
  align-items: center;
  gap: var(--space-8);
}

.projectAward {
  background: linear-gradient(135deg, #F59E0B, #EF4444);
  color: white;
  padding: var(--space-4) var(--space-8);
  border-radius: var(--radius-base);
  font-size: var(--font-size-xs);
  font-weight: var(--font-weight-bold);
  text-transform: uppercase;
  letter-spacing: 0.5px;
  display: flex;
  align-items: center;
  gap: var(--space-4);
  width: fit-content;
}

.projectCompany {
  background: rgba(139, 92, 246, 0.2);
  color: #8B5CF6;
  padding: var(--space-4) var(--space-8);
  border-radius: var(--radius-base);
  font-size: var(--font-size-xs);
  font-weight: var(--font-weight-semibold);
  border: 1px solid rgba(139, 92, 246, 0.3);
  display: flex;
  align-items: center;
  gap: var(--space-4);
  width: fit-content;
}

.projectContent {
  flex: 1;
  display: flex;
  flex-direction: column;
  position: relative;
  z-index: 2;
}

.projectTitle {
  font-size: var(--font-size-xl);
  font-weight: var(--font-weight-bold);
  color: var(--color-gray-200);
  margin-bottom: var(--space-12);
  line-height: 1.3;
  transition: color 0.3s ease;
}

.projectCard:hover .projectTitle {
  color: #00d4ff;
}

.projectDescription {
  color: var(--color-gray-300);
  line-height: 1.7;
  margin-bottom: var(--space-20);
  font-size: var(--font-size-base);
  flex-grow: 1;
}

.projectTech {
  display: flex;
  flex-wrap: wrap;
  gap: var(--space-8);
  margin-bottom: var(--space-24);
}

.projectActions {
  display: flex;
  gap: var(--space-12);
  margin-top: auto;
  position: relative;
  z-index: 2;
}

.projectOverlay {
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: linear-gradient(135deg, 
    rgba(0, 212, 255, 0.9) 0%, 
    rgba(16, 185, 129, 0.9) 50%,
    rgba(139, 92, 246, 0.9) 100%);
  display: flex;
  align-items: center;
  justify-content: center;
  border-radius: var(--radius-lg);
  pointer-events: none;
  z-index: 1;
}

.overlayContent {
  text-align: center;
  color: white;
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: var(--space-8);
}

.overlayContent span {
  font-weight: var(--font-weight-semibold);
  text-shadow: 0 2px 4px rgba(0, 0, 0, 0.3);
}

/* Light theme adjustments */
[data-theme="light"] .cardInner {
  background: rgba(255, 255, 255, 0.9);
  border: 1px solid rgba(0, 0, 0, 0.1);
}

[data-theme="light"] .projectCard:hover .cardInner {
  background: rgba(33, 128, 141, 0.05);
  border-color: rgba(33, 128, 141, 0.3);
  box-shadow: 0 25px 80px rgba(33, 128, 141, 0.15);
}

[data-theme="light"] .projectTitle {
  color: var(--color-text);
}

[data-theme="light"] .projectCard:hover .projectTitle {
  color: var(--color-primary);
}

[data-theme="light"] .projectDescription {
  color: var(--color-text-secondary);
}

[data-theme="light"] .projectIcon {
  background: linear-gradient(135deg, rgba(33, 128, 141, 0.2), rgba(16, 185, 129, 0.2));
}

[data-theme="light"] .projectCard:hover .projectIcon {
  background: linear-gradient(135deg, var(--color-primary), #10B981);
}

/* Responsive Design */
@media (max-width: 768px) {
  .projectCardBody {
    padding: var(--space-24);
  }
  
  .projectHeader {
    flex-direction: column;
    gap: var(--space-12);
  }
  
  .projectIcon {
    width: 50px;
    height: 50px;
    margin: 0 auto;
  }
  
  .iconEmoji {
    font-size: 1.25rem;
  }
  
  .projectMeta {
    text-align: center;
  }
  
  .projectActions {
    flex-direction: column;
    gap: var(--space-8);
  }
  
  .projectActions button {
    width: 100%;
  }
}

@media (max-width: 480px) {
  .projectCardBody {
    padding: var(--space-20);
  }
  
  .projectTitle {
    font-size: var(--font-size-lg);
  }
  
  .projectDescription {
    font-size: var(--font-size-sm);
  }
}
```

## 10.4 Projects Styles (src/components/sections/Projects.module.css)

```css
.projects {
  background: linear-gradient(135deg, rgba(45, 55, 72, 0.9) 0%, rgba(26, 32, 44, 0.9) 100%);
  position: relative;
  overflow: hidden;
}

.projects::before {
  content: '';
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: 
    radial-gradient(ellipse at 20% 30%, rgba(0, 212, 255, 0.03) 0%, transparent 50%),
    radial-gradient(ellipse at 80% 70%, rgba(16, 185, 129, 0.03) 0%, transparent 50%);
  pointer-events: none;
}

.projectFilters {
  display: flex;
  justify-content: center;
  gap: var(--space-12);
  margin-bottom: var(--space-32);
  flex-wrap: wrap;
  position: relative;
  z-index: 2;
}

.filterBtn {
  background: rgba(0, 212, 255, 0.1);
  border: 1px solid rgba(0, 212, 255, 0.3);
  color: #00d4ff;
  padding: var(--space-12) var(--space-24);
  border-radius: var(--radius-full);
  font-weight: var(--font-weight-medium);
  cursor: pointer;
  transition: all 0.3s ease;
  backdrop-filter: blur(10px);
  font-size: var(--font-size-base);
  white-space: nowrap;
}

.filterBtn:hover,
.filterBtn.active {
  background: linear-gradient(135deg, #00d4ff, #10B981);
  color: var(--color-charcoal-800);
  border-color: transparent;
  transform: translateY(-2px);
  box-shadow: 0 8px 25px rgba(0, 212, 255, 0.3);
}

.projectsGrid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(400px, 1fr));
  gap: var(--space-32);
  max-width: 1400px;
  margin: 0 auto;
  position: relative;
  z-index: 2;
}

/* Modal Styles */
.modalContent {
  width: 100%;
}

.modalHeader {
  margin-bottom: var(--space-20);
}

.modalTags {
  display: flex;
  flex-wrap: wrap;
  gap: var(--space-8);
  margin-bottom: var(--space-16);
}

.modalAward {
  background: linear-gradient(135deg, #F59E0B, #EF4444);
  color: white;
  padding: var(--space-6) var(--space-12);
  border-radius: var(--radius-base);
  font-size: var(--font-size-xs);
  font-weight: var(--font-weight-bold);
  text-transform: uppercase;
  letter-spacing: 0.5px;
}

.modalCompany {
  background: rgba(139, 92, 246, 0.2);
  color: #8B5CF6;
  padding: var(--space-6) var(--space-12);
  border-radius: var(--radius-base);
  font-size: var(--font-size-xs);
  font-weight: var(--font-weight-semibold);
  border: 1px solid rgba(139, 92, 246, 0.3);
}

.modalBody {
  line-height: 1.7;
}

.modalDescription {
  font-size: var(--font-size-lg);
  color: var(--color-gray-300);
  margin-bottom: var(--space-24);
  line-height: 1.8;
}

.modalSection {
  margin-bottom: var(--space-24);
}

.modalSection h3 {
  font-size: var(--font-size-xl);
  font-weight: var(--font-weight-semibold);
  color: var(--color-gray-200);
  margin-bottom: var(--space-12);
  display: flex;
  align-items: center;
  gap: var(--space-8);
}

.modalSection h3::before {
  content: '';
  width: 4px;
  height: 20px;
  background: linear-gradient(135deg, #00d4ff, #10B981);
  border-radius: 2px;
}

.modalHighlights {
  list-style: none;
  padding: 0;
  margin: 0;
}

.modalHighlights li {
  padding: var(--space-8) 0;
  color: var(--color-gray-300);
  position: relative;
  padding-left: var(--space-24);
  line-height: 1.6;
}

.modalHighlights li::before {
  content: '✓';
  position: absolute;
  left: 0;
  color: #10B981;
  font-weight: bold;
  font-size: var(--font-size-lg);
}

.modalTech {
  display: flex;
  flex-wrap: wrap;
  gap: var(--space-8);
}

.modalActions {
  display: flex;
  gap: var(--space-12);
  margin-top: var(--space-24);
  padding-top: var(--space-20);
  border-top: 1px solid rgba(255, 255, 255, 0.1);
}

/* Light theme modal adjustments */
[data-theme="light"] .modalDescription {
  color: var(--color-text-secondary);
}

[data-theme="light"] .modalSection h3 {
  color: var(--color-text);
}

[data-theme="light"] .modalHighlights li {
  color: var(--color-text-secondary);
}

[data-theme="light"] .modalActions {
  border-top: 1px solid var(--color-border);
}

/* Responsive Design */
@media (max-width: 1024px) {
  .projectsGrid {
    grid-template-columns: repeat(auto-fit, minmax(350px, 1fr));
  }
}

@media (max-width: 768px) {
  .projectsGrid {
    grid-template-columns: 1fr;
    gap: var(--space-24);
  }
  
  .projectFilters {
    gap: var(--space-8);
    margin-bottom: var(--space-24);
  }
  
  .filterBtn {
    padding: var(--space-8) var(--space-16);
    font-size: var(--font-size-sm);
  }
  
  .modalActions {
    flex-direction: column;
  }
  
  .modalActions button {
    width: 100%;
  }
}

@media (max-width: 640px) {
  .modalContent {
    padding: var(--space-20);
  }
}

/* Print styles */
@media print {
  .projects {
    background: white;
  }
  
  .projectsGrid {
    grid-template-columns: 1fr;
  }
  
  .projectFilters {
    display: none;
  }
  
  .projectOverlay {
    display: none;
  }
}
```