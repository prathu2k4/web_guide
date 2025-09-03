# Step 4: Core Hooks Implementation

## 4.1 Utility Functions (src/lib/utils.ts)

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

export const debounce = (func: Function, delay: number) => {
  let timeoutId: NodeJS.Timeout
  
  return (...args: any[]) => {
    clearTimeout(timeoutId)
    timeoutId = setTimeout(() => func(...args), delay)
  }
}
```

## 4.2 Intersection Observer Hook (src/lib/hooks/useIntersectionObserver.ts)

```typescript
import { useEffect, useRef, useState } from 'react'

interface UseIntersectionObserverOptions {
  threshold?: number | number[]
  rootMargin?: string
  triggerOnce?: boolean
  skip?: boolean
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
    triggerOnce = true,
    skip = false
  } = options

  useEffect(() => {
    if (skip) return

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
  }, [threshold, rootMargin, triggerOnce, hasTriggered, skip])

  return { 
    ref: elementRef, 
    isIntersecting, 
    hasTriggered,
    isVisible: isIntersecting 
  }
}
```

## 4.3 Typing Animation Hook (src/lib/hooks/useTypingAnimation.ts)

```typescript
import { useState, useEffect } from 'react'

export function useTypingAnimation(
  text: string,
  speed: number = 120,
  startDelay: number = 1500
) {
  const [displayText, setDisplayText] = useState('')
  const [currentIndex, setCurrentIndex] = useState(0)
  const [isComplete, setIsComplete] = useState(false)
  const [showCursor, setShowCursor] = useState(true)

  useEffect(() => {
    // Start typing after initial delay
    const startTimeout = setTimeout(() => {
      if (currentIndex < text.length) {
        const timeout = setTimeout(() => {
          setDisplayText(prev => prev + text[currentIndex])
          setCurrentIndex(prev => prev + 1)
        }, speed + Math.random() * 80) // Variable typing speed

        return () => clearTimeout(timeout)
      } else if (!isComplete) {
        setIsComplete(true)
        
        // Start cursor blinking after typing is complete
        setTimeout(() => {
          setShowCursor(true)
        }, 500)
      }
    }, startDelay)

    return () => clearTimeout(startTimeout)
  }, [currentIndex, text, speed, startDelay, isComplete])

  // Cursor blinking effect
  useEffect(() => {
    if (isComplete) {
      const cursorInterval = setInterval(() => {
        setShowCursor(prev => !prev)
      }, 1000)

      return () => clearInterval(cursorInterval)
    }
  }, [isComplete])

  return {
    displayText,
    isComplete,
    showCursor,
    cursor: showCursor ? '|' : ''
  }
}
```

## 4.4 Scroll Animation Hook (src/lib/hooks/useScrollAnimation.ts)

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

## 4.5 Smooth Scroll Hook (src/lib/hooks/useSmoothScroll.ts)

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

  const scrollToTop = useCallback((duration: number = 600) => {
    scrollToElement('body', duration, 0)
  }, [scrollToElement])

  return {
    scrollToElement,
    scrollToTop
  }
}
```

## 4.6 Throttle Hook (src/lib/hooks/useThrottle.ts)

```typescript
import { useCallback, useRef } from 'react'

export function useThrottle<T extends (...args: any[]) => any>(
  callback: T,
  delay: number
): T {
  const timeoutRef = useRef<NodeJS.Timeout | null>(null)
  const lastCallTimeRef = useRef<number>(0)

  return useCallback(
    ((...args: Parameters<T>) => {
      const now = Date.now()
      
      if (now - lastCallTimeRef.current >= delay) {
        callback(...args)
        lastCallTimeRef.current = now
      } else {
        if (timeoutRef.current) {
          clearTimeout(timeoutRef.current)
        }
        
        timeoutRef.current = setTimeout(() => {
          callback(...args)
          lastCallTimeRef.current = Date.now()
        }, delay - (now - lastCallTimeRef.current))
      }
    }) as T,
    [callback, delay]
  )
}
```

## 4.7 Window Size Hook (src/lib/hooks/useWindowSize.ts)

```typescript
import { useState, useEffect } from 'react'

interface WindowSize {
  width: number
  height: number
}

export function useWindowSize(): WindowSize {
  const [windowSize, setWindowSize] = useState<WindowSize>({
    width: 0,
    height: 0,
  })

  useEffect(() => {
    function handleResize() {
      setWindowSize({
        width: window.innerWidth,
        height: window.innerHeight,
      })
    }

    window.addEventListener('resize', handleResize)
    handleResize()

    return () => window.removeEventListener('resize', handleResize)
  }, [])

  return windowSize
}
```

## 4.8 Local Storage Hook (src/lib/hooks/useLocalStorage.ts)

```typescript
import { useState, useEffect } from 'react'

export function useLocalStorage<T>(
  key: string,
  initialValue: T
): [T, (value: T | ((val: T) => T)) => void] {
  const [storedValue, setStoredValue] = useState<T>(() => {
    try {
      if (typeof window !== 'undefined') {
        const item = window.localStorage.getItem(key)
        return item ? JSON.parse(item) : initialValue
      }
      return initialValue
    } catch (error) {
      console.error(`Error reading localStorage key "${key}":`, error)
      return initialValue
    }
  })

  const setValue = (value: T | ((val: T) => T)) => {
    try {
      const valueToStore = value instanceof Function ? value(storedValue) : value
      setStoredValue(valueToStore)
      
      if (typeof window !== 'undefined') {
        window.localStorage.setItem(key, JSON.stringify(valueToStore))
      }
    } catch (error) {
      console.error(`Error setting localStorage key "${key}":`, error)
    }
  }

  return [storedValue, setValue]
}
```

## 4.9 Mouse Position Hook (src/lib/hooks/useMousePosition.ts)

```typescript
import { useState, useEffect } from 'react'

interface MousePosition {
  x: number
  y: number
}

export function useMousePosition(): MousePosition {
  const [mousePosition, setMousePosition] = useState<MousePosition>({
    x: 0,
    y: 0
  })

  useEffect(() => {
    const updateMousePosition = (event: MouseEvent) => {
      setMousePosition({
        x: event.clientX,
        y: event.clientY
      })
    }

    window.addEventListener('mousemove', updateMousePosition)

    return () => {
      window.removeEventListener('mousemove', updateMousePosition)
    }
  }, [])

  return mousePosition
}
```

## 4.10 Page Visibility Hook (src/lib/hooks/usePageVisibility.ts)

```typescript
import { useState, useEffect } from 'react'

export function usePageVisibility(): boolean {
  const [isVisible, setIsVisible] = useState(true)

  useEffect(() => {
    const handleVisibilityChange = () => {
      setIsVisible(!document.hidden)
    }

    document.addEventListener('visibilitychange', handleVisibilityChange)

    return () => {
      document.removeEventListener('visibilitychange', handleVisibilityChange)
    }
  }, [])

  return isVisible
}
```