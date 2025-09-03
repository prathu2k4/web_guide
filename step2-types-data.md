# Step 2: TypeScript Types and Data

## 2.1 TypeScript Interfaces (src/types/index.ts)

```typescript
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

export interface NavigationItem {
  href: string
  label: string
}

export interface SocialLink {
  platform: string
  url: string
  icon: string
}
```

## 2.2 Data Configuration (src/lib/data.ts)

```typescript
import { Project, Skill, Achievement, ContactMethod, TimelineItem, StatItem, SocialLink } from '@/types'

export const projectsData: Project[] = [
  {
    id: 0,
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
    icon: "🔧"
  },
  {
    id: 1,
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
    icon: "🤖"
  },
  {
    id: 2,
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
    icon: "⚡"
  }
]

export const skillsData: Skill[] = [
  // Programming Languages
  {
    name: 'Verilog',
    level: 90,
    category: 'programming',
    icon: '⚡',
    description: 'Hardware Description Language'
  },
  {
    name: 'Python', 
    level: 85,
    category: 'programming',
    icon: '🐍',
    description: 'Automation & Development'
  },
  {
    name: 'C++',
    level: 80,
    category: 'programming', 
    icon: '⚙️',
    description: 'Systems Programming'
  },
  {
    name: 'JavaScript',
    level: 75,
    category: 'programming',
    icon: '💛',
    description: 'Web Development'
  },
  
  // Tools
  {
    name: 'ModelSim',
    level: 85,
    category: 'tools',
    icon: '📊',
    description: 'Analysis & Simulation'
  },
  {
    name: 'Vivado',
    level: 80,
    category: 'tools',
    icon: '🔌', 
    description: 'Circuit Simulation'
  },
  {
    name: 'Quartus',
    level: 75,
    category: 'tools',
    icon: '🎯',
    description: 'FPGA Development'
  },
  {
    name: 'QuestaSim',
    level: 70,
    category: 'tools',
    icon: '🔬',
    description: 'HDL Simulation'
  },
  {
    name: 'Altium',
    level: 65,
    category: 'tools',
    icon: '🖥️',
    description: 'PCB Design'
  },
  {
    name: 'Git',
    level: 85,
    category: 'tools',
    icon: '📝',
    description: 'Version Control'
  },
  
  // Domains
  {
    name: 'Digital Design',
    level: 90,
    category: 'domains',
    icon: '💾',
    description: 'Digital circuit design and verification'
  },
  {
    name: 'VLSI',
    level: 85,
    category: 'domains',
    icon: '🔧',
    description: 'Very Large Scale Integration design'
  },
  {
    name: 'Analog Design',
    level: 70,
    category: 'domains',
    icon: '📡',
    description: 'Analog and digital circuit design'
  },
  {
    name: 'DSP',
    level: 75,
    category: 'domains',
    icon: '📈',
    description: 'Digital signal processing algorithms'
  }
]

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

export const educationData: TimelineItem[] = [
  {
    id: 1,
    institution: "UVCE Bengaluru",
    degree: "Bachelor of Engineering - Electronics & Communication", 
    period: "2022 - 2026",
    focus: ["Digital System Design", "Signal Processing", "VLSI Technology"]
  }
]

export const statsData: StatItem[] = [
  {
    label: "Projects Completed",
    value: "15",
    suffix: "+"
  },
  {
    label: "Technologies Mastered", 
    value: "20",
    suffix: "+"
  },
  {
    label: "Years Experience",
    value: "3",
    suffix: "+"
  }
]

export const socialLinks: SocialLink[] = [
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

export const navigationItems = [
  { href: '#home', label: 'Home' },
  { href: '#about', label: 'About' },
  { href: '#skills', label: 'Skills' },
  { href: '#projects', label: 'Projects' },
  { href: '#achievements', label: 'Achievements' },
  { href: '#contact', label: 'Contact' }
]
```