# Step 13: Data Layer & TypeScript Definitions

## 13.1 TypeScript Type Definitions (src/types/index.ts)

```typescript
// Core types for the portfolio application

export interface NavigationItem {
  href: string
  label: string
  external?: boolean
}

export interface SocialLink {
  platform: string
  url: string
  icon: string
}

export interface Skill {
  id: number
  name: string
  category: 'programming' | 'tools' | 'domains'
  level: number
  description: string
  icon: string
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
  liveUrl?: string
  award?: string
  company?: string
  icon?: string
  period?: string
  images?: string[]
}

export interface Achievement {
  id: number
  title: string
  role: string
  period: string
  description: string
  status: 'ongoing' | 'completed'
  icon: string
  organization: string
}

export interface EducationItem {
  id: number
  degree: string
  institution: string
  period: string
  focus: string[]
  gpa?: string
  status: 'ongoing' | 'completed'
}

export interface ContactItem {
  id: number
  type: 'email' | 'location' | 'linkedin'
  title: string
  value: string
  href: string
  icon: string
  description?: string
}

export interface StatItem {
  label: string
  value: string
  suffix?: string
}

// Component Props Types
export interface SectionProps {
  className?: string
  children?: React.ReactNode
}

export interface AnimatedSectionProps extends SectionProps {
  delay?: number
  stagger?: number
}

// Animation Types
export interface AnimationConfig {
  duration?: number
  delay?: number
  ease?: string
  stagger?: number
}

// Theme Types
export type ThemeMode = 'light' | 'dark'

// Hook Return Types
export interface UseScrollAnimationReturn {
  scrollY: number
  scrollDirection: 'up' | 'down'
  isScrolled: boolean
}

export interface UseIntersectionObserverReturn {
  ref: React.RefObject<HTMLElement>
  isIntersecting: boolean
  hasTriggered: boolean
  isVisible: boolean
}

export interface UseTypingAnimationReturn {
  displayText: string
  isComplete: boolean
  showCursor: boolean
  cursor: string
}

// Utility Types
export type Optional<T, K extends keyof T> = Omit<T, K> & Partial<Pick<T, K>>

export type RequiredBy<T, K extends keyof T> = T & Required<Pick<T, K>>
```

## 13.2 Navigation Data (src/lib/data/navigation.ts)

```typescript
import type { NavigationItem, SocialLink } from '@/types'

export const navigationItems: NavigationItem[] = [
  {
    href: '#home',
    label: 'Home'
  },
  {
    href: '#about',
    label: 'About'
  },
  {
    href: '#skills',
    label: 'Skills'
  },
  {
    href: '#projects',
    label: 'Projects'
  },
  {
    href: '#achievements',
    label: 'Achievements'
  },
  {
    href: '#contact',
    label: 'Contact'
  }
]

export const socialLinks: SocialLink[] = [
  {
    platform: 'GitHub',
    url: 'https://github.com/pratham-jain',
    icon: 'github'
  },
  {
    platform: 'LinkedIn',
    url: 'https://linkedin.com/in/pratham-jain',
    icon: 'linkedin'
  },
  {
    platform: 'Email',
    url: 'mailto:prathu2k4@gmail.com',
    icon: 'email'
  }
]
```

## 13.3 Skills Data (src/lib/data/skills.ts)

```typescript
import type { Skill } from '@/types'

export const skillsData: Skill[] = [
  // Programming Languages
  {
    id: 1,
    name: "Verilog HDL",
    category: "programming",
    level: 90,
    description: "Hardware Description Language for digital design",
    icon: "⚡"
  },
  {
    id: 2,
    name: "Python",
    category: "programming", 
    level: 85,
    description: "Automation & Development",
    icon: "🐍"
  },
  {
    id: 3,
    name: "C++",
    category: "programming",
    level: 80,
    description: "Systems Programming",
    icon: "⚙️"
  },
  {
    id: 4,
    name: "MATLAB",
    category: "programming",
    level: 75,
    description: "Analysis & Simulation",
    icon: "📊"
  },

  // Tools & Software
  {
    id: 5,
    name: "LTspice",
    category: "tools",
    level: 85,
    description: "Circuit Simulation",
    icon: "🔌"
  },
  {
    id: 6,
    name: "Vivado",
    category: "tools",
    level: 80,
    description: "FPGA Development",
    icon: "🔧"
  },
  {
    id: 7,
    name: "ModelSim",
    category: "tools",
    level: 82,
    description: "HDL Simulation",
    icon: "📡"
  },
  {
    id: 8,
    name: "KiCad",
    category: "tools",
    level: 70,
    description: "PCB Design",
    icon: "🔲"
  },
  {
    id: 9,
    name: "Git",
    category: "tools",
    level: 78,
    description: "Version Control",
    icon: "📝"
  },
  {
    id: 10,
    name: "Multisim",
    category: "tools",
    level: 75,
    description: "Circuit Analysis",
    icon: "⚡"
  },

  // Domain Expertise
  {
    id: 11,
    name: "RTL Design",
    category: "domains",
    level: 88,
    description: "Digital circuit design and verification",
    icon: "🔬"
  },
  {
    id: 12,
    name: "VLSI Design",
    category: "domains",
    level: 82,
    description: "Very Large Scale Integration design",
    icon: "💻"
  },
  {
    id: 13,
    name: "Analog Design",
    category: "domains",
    level: 75,
    description: "Analog and digital circuit design",
    icon: "📻"
  },
  {
    id: 14,
    name: "DSP",
    category: "domains",
    level: 78,
    description: "Digital signal processing algorithms",
    icon: "📊"
  }
]
```

## 13.4 Projects Data (src/lib/data/projects.ts)

```typescript
import type { Project } from '@/types'

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
    icon: "⚡",
    period: "2024 - Present"
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
    icon: "🏆",
    period: "2024"
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
    icon: "🚀",
    period: "2023"
  }
]
```

## 13.5 Education & Achievements Data (src/lib/data/personal.ts)

```typescript
import type { EducationItem, Achievement, StatItem, ContactItem } from '@/types'

export const educationData: EducationItem[] = [
  {
    id: 1,
    degree: "Bachelor of Engineering - Electronics and Communication",
    institution: "UVCE Bengaluru",
    period: "2022 - 2026",
    focus: ["Digital System Design", "Signal Processing", "VLSI Technology"],
    status: "ongoing"
  }
]

export const achievementsData: Achievement[] = [
  {
    id: 1,
    title: "IoT Systems Research",
    role: "Research Associate",
    period: "Ongoing",
    description: "Contributing to cutting-edge research in IoT and hardware systems at UVCE's premier research facility.",
    status: "ongoing",
    icon: "🔬",
    organization: "UVCE Research Lab"
  },
  {
    id: 2,
    title: "Smart India Hackathon",
    role: "Team Lead",
    period: "2024",
    description: "Led technical team to national finals, demonstrating leadership and innovation in problem-solving.",
    status: "completed",
    icon: "🧠",
    organization: "Government of India"
  },
  {
    id: 3,
    title: "Hack-a-Maze Competition",
    role: "Technical Lead",
    period: "1st Place • 2024",
    description: "Designed and implemented winning line following system at Impetus, UVCE Tech Fest.",
    status: "completed",
    icon: "🏆",
    organization: "UVCE Impetus"
  }
]

export const statsData: StatItem[] = [
  {
    label: "Projects",
    value: "15",
    suffix: "+"
  },
  {
    label: "Competitions",
    value: "8",
    suffix: "+"
  },
  {
    label: "Technologies",
    value: "12",
    suffix: "+"
  }
]

export const contactData: ContactItem[] = [
  {
    id: 1,
    type: 'email',
    title: 'Email',
    value: 'prathu2k4@gmail.com',
    href: 'mailto:prathu2k4@gmail.com',
    icon: 'mail',
    description: 'Send me an email for project inquiries'
  },
  {
    id: 2,
    type: 'location',
    title: 'Location',
    value: 'Bengaluru, India',
    href: '#',
    icon: 'map-pin',
    description: 'Based in the Silicon Valley of India'
  },
  {
    id: 3,
    type: 'linkedin',
    title: 'LinkedIn',
    value: 'Connect professionally',
    href: 'https://linkedin.com/in/pratham-jain',
    icon: 'linkedin',
    description: 'Let\'s connect and network'
  }
]
```

## 13.6 Centralized Data Export (src/lib/data/index.ts)

```typescript
// Central export file for all data
export { navigationItems, socialLinks } from './navigation'
export { skillsData } from './skills'
export { projectsData } from './projects'
export { 
  educationData, 
  achievementsData, 
  statsData, 
  contactData 
} from './personal'

// Constants
export const SITE_CONFIG = {
  name: 'Pratham Jain V S',
  title: 'Digital Circuit Designer & Creative Developer',
  description: 'Passionate ECE student at UVCE Bengaluru, specializing in digital design, RTL development, and creative programming.',
  url: process.env.NEXT_PUBLIC_BASE_URL || 'https://your-domain.com',
  email: 'prathu2k4@gmail.com',
  social: {
    github: 'https://github.com/pratham-jain',
    linkedin: 'https://linkedin.com/in/pratham-jain'
  }
} as const

export const ANIMATION_CONFIG = {
  duration: {
    fast: 0.2,
    normal: 0.4,
    slow: 0.8
  },
  ease: {
    standard: [0.16, 1, 0.3, 1],
    bounce: [0.68, -0.55, 0.265, 1.55]
  },
  stagger: {
    items: 0.1,
    sections: 0.2
  }
} as const

export const BREAKPOINTS = {
  sm: 640,
  md: 768,
  lg: 1024,
  xl: 1280,
  '2xl': 1536
} as const
```

## 13.7 Constants (src/lib/constants.ts)

```typescript
export const LOADING_DURATION = 2000
export const TYPING_SPEED = 120
export const SCROLL_OFFSET = 80

export const THEME_STORAGE_KEY = 'portfolio-theme'
export const DEFAULT_THEME = 'dark'

export const INTERSECTION_OPTIONS = {
  threshold: 0.1,
  rootMargin: '0px 0px -100px 0px'
} as const

export const SCROLL_ANIMATION_OPTIONS = {
  duration: 800,
  offset: 80,
  ease: 'easeInOut'
} as const

export const SKILLS_ANIMATION_DELAY = 150
export const PROJECTS_STAGGER_DELAY = 100

// SEO Constants
export const SEO_DEFAULTS = {
  title: 'Pratham Jain V S - Digital Circuit Designer & Creative Developer',
  description: 'Passionate ECE student at UVCE Bengaluru, specializing in digital design, RTL development, and creative programming.',
  keywords: [
    'Pratham Jain',
    'Digital Circuit Design',
    'RTL Development',
    'VLSI Design', 
    'Verilog HDL',
    'FPGA',
    'ECE Student',
    'UVCE Bengaluru'
  ]
} as const

// Animation Presets
export const FADE_UP_ANIMATION = {
  initial: { opacity: 0, y: 50 },
  animate: { opacity: 1, y: 0 },
  transition: { duration: 0.6, ease: "easeOut" }
} as const

export const SCALE_IN_ANIMATION = {
  initial: { opacity: 0, scale: 0.8 },
  animate: { opacity: 1, scale: 1 },
  transition: { duration: 0.5, ease: "easeOut" }
} as const

export const SLIDE_IN_LEFT = {
  initial: { opacity: 0, x: -30 },
  animate: { opacity: 1, x: 0 },
  transition: { duration: 0.6, ease: "easeOut" }
} as const

export const SLIDE_IN_RIGHT = {
  initial: { opacity: 0, x: 30 },
  animate: { opacity: 1, x: 0 },
  transition: { duration: 0.6, ease: "easeOut" }
} as const

// Error Messages
export const ERROR_MESSAGES = {
  LOAD_FAILED: 'Failed to load content',
  NETWORK_ERROR: 'Network connection error',
  UNKNOWN_ERROR: 'An unexpected error occurred',
  COPY_FAILED: 'Failed to copy to clipboard',
  COPY_SUCCESS: 'Copied to clipboard!'
} as const

// Validation Rules
export const VALIDATION_RULES = {
  EMAIL_PATTERN: /^[^\s@]+@[^\s@]+\.[^\s@]+$/,
  MIN_MESSAGE_LENGTH: 10,
  MAX_MESSAGE_LENGTH: 1000
} as const
```

## 13.8 Environment Variables (.env.local)

```env
# Site Configuration
NEXT_PUBLIC_BASE_URL=http://localhost:3000
NEXT_PUBLIC_SITE_NAME="Pratham Jain Portfolio"

# Analytics (Optional)
# NEXT_PUBLIC_GA_ID=G-XXXXXXXXXX
# NEXT_PUBLIC_GTM_ID=GTM-XXXXXXX

# Contact Form (Optional - if you add a contact form later)
# EMAILJS_SERVICE_ID=your_service_id
# EMAILJS_TEMPLATE_ID=your_template_id
# EMAILJS_USER_ID=your_user_id

# Development
NODE_ENV=development

# Security
# NEXTAUTH_SECRET=your-secret-key (if adding authentication later)
# NEXTAUTH_URL=http://localhost:3000
```

## 13.9 Next.js Configuration (next.config.js)

```javascript
/** @type {import('next').NextConfig} */
const nextConfig = {
  reactStrictMode: true,
  swcMinify: true,
  
  // Experimental features for better performance
  experimental: {
    optimizeCss: true,
  },

  // Image optimization
  images: {
    domains: [
      'images.unsplash.com',
      'avatars.githubusercontent.com'
    ],
    formats: ['image/webp', 'image/avif'],
    minimumCacheTTL: 60,
  },

  // Headers for security and performance
  async headers() {
    return [
      {
        source: '/(.*)',
        headers: [
          {
            key: 'X-Frame-Options',
            value: 'DENY',
          },
          {
            key: 'X-Content-Type-Options',
            value: 'nosniff',
          },
          {
            key: 'Referrer-Policy',
            value: 'origin-when-cross-origin',
          },
          {
            key: 'X-DNS-Prefetch-Control',
            value: 'on',
          },
        ],
      },
      {
        source: '/static/(.*)',
        headers: [
          {
            key: 'Cache-Control',
            value: 'public, max-age=31536000, immutable',
          },
        ],
      },
    ]
  },

  // Webpack configuration for better bundle optimization
  webpack: (config, { buildId, dev, isServer, defaultLoaders, webpack }) => {
    // Optimize bundle splitting
    if (!dev && !isServer) {
      config.optimization.splitChunks.chunks = 'all'
      config.optimization.splitChunks.cacheGroups = {
        ...config.optimization.splitChunks.cacheGroups,
        vendor: {
          test: /[\\/]node_modules[\\/]/,
          name: 'vendors',
          chunks: 'all',
          priority: 10,
        },
        framer: {
          test: /[\\/]node_modules[\\/](framer-motion)[\\/]/,
          name: 'framer',
          chunks: 'all',
          priority: 20,
        },
      }
    }

    return config
  },

  // Compress responses
  compress: true,

  // Power-only domain redirects (configure when deploying)
  // async redirects() {
  //   return [
  //     {
  //       source: '/home',
  //       destination: '/',
  //       permanent: true,
  //     },
  //   ]
  // },

  // Generate static paths for better SEO
  trailingSlash: false,

  // Environment variables available on client-side
  env: {
    CUSTOM_KEY: process.env.CUSTOM_KEY,
  },
}

module.exports = nextConfig
```

## 13.10 TypeScript Configuration (tsconfig.json)

```json
{
  "compilerOptions": {
    "lib": ["dom", "dom.iterable", "es6", "es2017"],
    "allowJs": true,
    "skipLibCheck": true,
    "strict": true,
    "noEmit": true,
    "esModuleInterop": true,
    "module": "esnext",
    "moduleResolution": "bundler",
    "resolveJsonModule": true,
    "isolatedModules": true,
    "jsx": "preserve",
    "incremental": true,
    "plugins": [
      {
        "name": "next"
      }
    ],
    "baseUrl": ".",
    "paths": {
      "@/*": ["./src/*"],
      "@/components/*": ["./src/components/*"],
      "@/lib/*": ["./src/lib/*"],
      "@/types/*": ["./src/types/*"],
      "@/app/*": ["./src/app/*"],
      "@/styles/*": ["./src/styles/*"],
      "@/public/*": ["./public/*"]
    },
    "target": "es5",
    "forceConsistentCasingInFileNames": true
  },
  "include": [
    "next-env.d.ts",
    "**/*.ts",
    "**/*.tsx",
    ".next/types/**/*.ts"
  ],
  "exclude": [
    "node_modules",
    ".next",
    "out",
    "dist"
  ]
}
```