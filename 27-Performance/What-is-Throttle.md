# What is Throttle

Throttle is a technique that limits how often a function can execute within a specified time period. Unlike debounce which waits for inactivity, throttle ensures a function runs at most once every X milliseconds, regardless of how many times it's called.

## How Throttle Works

```javascript
// Without throttle: function called on every scroll event
function handleScroll() {
  console.log('Scroll position:', window.scrollY);
}

window.addEventListener('scroll', handleScroll); // Fires hundreds of times

// With throttle: function called at most once every 200ms
const throttledScroll = throttle(handleScroll, 200);

window.addEventListener('scroll', throttledScroll); // Fires at most 5 times/second
```

## Basic Throttle Implementation

```javascript
function throttle(func, limit) {
  let inThrottle = false;

  return function (...args) {
    if (!inThrottle) {
      func.apply(this, args);
      inThrottle = true;
      setTimeout(() => {
        inThrottle = false;
      }, limit);
    }
  };
}

// Usage
const handleMouseMove = throttle((e) => {
  console.log('Mouse position:', e.clientX, e.clientY);
}, 100);

document.addEventListener('mousemove', handleMouseMove);
```

## Throttle with Leading and Trailing Options

```javascript
function throttle(func, limit, options = {}) {
  let timeoutId = null;
  let lastArgs = null;
  const { leading = true, trailing = true } = options;

  return function (...args) {
    const callNow = leading && !timeoutId;

    if (trailing) {
      lastArgs = args;
    }

    if (!timeoutId) {
      if (callNow) {
        func.apply(this, args);
      }

      timeoutId = setTimeout(() => {
        if (trailing && lastArgs) {
          func.apply(this, lastArgs);
          lastArgs = null;
        }
        timeoutId = null;
      }, limit);
    }
  };
}

// Leading only: execute immediately, ignore subsequent calls
const leadingOnly = throttle(fn, 300, { leading: true, trailing: false });

// Trailing only: execute after delay, skip immediate
const trailingOnly = throttle(fn, 300, { leading: false, trailing: true });
```

## Throttle with Cancel and Flush

```javascript
function throttle(func, limit) {
  let timeoutId = null;
  let lastArgs = null;
  let lastThis = null;

  function throttled(...args) {
    lastArgs = args;
    lastThis = this;

    if (!timeoutId) {
      func.apply(this, args);
      lastArgs = null;
      lastThis = null;

      timeoutId = setTimeout(() => {
        if (lastArgs) {
          func.apply(lastThis, lastArgs);
          lastArgs = null;
          lastThis = null;
        }
        timeoutId = null;
      }, limit);
    }
  }

  throttled.cancel = function () {
    clearTimeout(timeoutId);
    timeoutId = null;
    lastArgs = null;
    lastThis = null;
  };

  throttled.flush = function () {
    if (lastArgs && timeoutId) {
      func.apply(lastThis, lastArgs);
      throttled.cancel();
    }
  };

  return throttled;
}

// Usage
const throttledFn = throttle(console.log, 1000);

throttledFn('a'); // Executes immediately
throttledFn('b'); // Ignored
throttledFn('c'); // Ignored
throttledFn.cancel(); // Cancel pending
```

## Scroll Event Handler

```javascript
// Efficient scroll handling
class ScrollHandler {
  constructor() {
    this.ticking = false;
    this.throttledScroll = throttle(this.onScroll.bind(this), 100);
    window.addEventListener('scroll', this.throttledScroll);
  }

  onScroll() {
    // Direct DOM updates (not batched)
    this.updateProgressBar();
    this.loadMoreContent();
  }

  updateProgressBar() {
    const scrollHeight = document.documentElement.scrollHeight - window.innerHeight;
    const scrolled = (window.scrollY / scrollHeight) * 100;
    document.getElementById('progress').style.width = `${scrolled}%`;
  }

  loadMoreContent() {
    const nearBottom = window.innerHeight + window.scrollY >= document.body.offsetHeight - 100;
    if (nearBottom) {
      this.fetchMorePosts();
    }
  }

  destroy() {
    this.throttledScroll.cancel();
  }
}
```

## React Throttle Hook

```javascript
import { useState, useEffect, useRef, useCallback } from 'react';

function useThrottle(callback, delay) {
  const lastCall = useRef(0);
  const timeoutRef = useRef(null);

  return useCallback((...args) => {
    const now = Date.now();

    if (now - lastCall.current >= delay) {
      lastCall.current = now;
      callback(...args);
    } else {
      clearTimeout(timeoutRef.current);
      timeoutRef.current = setTimeout(() => {
        lastCall.current = Date.now();
        callback(...args);
      }, delay - (now - lastCall.current));
    }
  }, [callback, delay]);
}

// Usage in component
function ScrollTracker() {
  const [scrollY, setScrollY] = useState(0);

  const throttledScroll = useThrottle(() => {
    setScrollY(window.scrollY);
  }, 100);

  useEffect(() => {
    window.addEventListener('scroll', throttledScroll);
    return () => window.removeEventListener('scroll', throttledScroll);
  }, [throttledScroll]);

  return <div>Scroll position: {scrollY}</div>;
}
```

## RequestAnimationFrame Throttle

```javascript
// For smooth animations tied to frame rate
function rafThrottle(func) {
  let rafId = null;
  let lastArgs = null;

  return function (...args) {
    lastArgs = args;

    if (rafId === null) {
      rafId = requestAnimationFrame(() => {
        func.apply(this, lastArgs);
        rafId = null;
        lastArgs = null;
      });
    }
  };
}

// Usage for smooth mouse tracking
const smoothMouseMove = rafThrottle((e) => {
  // Runs once per animation frame (~60fps)
  element.style.transform = `translate(${e.clientX}px, ${e.clientY}px)`;
});

document.addEventListener('mousemove', smoothMouseMove);
```

## Common Use Cases

- **Scroll events**: Processing scroll position at regular intervals
- **Mouse movement**: Tracking cursor position
- **Resize events**: Recalculating layout
- **Button clicks**: Preventing rapid submissions
- **API calls**: Limiting request frequency
- **Animation frames**: Smooth animations

## Debounce vs Throttle

| Feature | Debounce | Throttle |
|---------|----------|----------|
| When fires | After inactivity | At regular intervals |
| First call | Usually delayed | Usually immediate |
| Best for | Search, validation | Scroll, resize, mouse |
| Frequency | Once per pause | Once per interval |

## Common Mistakes

1. **Using debounce for scroll events** - Throttle is better for continuous events
2. **Not handling trailing calls** - Missing final event
3. **Wrong limit value** - Too frequent or too infrequent
4. **Not cancelling on cleanup** - Memory leaks
5. **Forgetting about this context** - Wrong function context

## Related Topics

- [[What-is-Debounce]]
- [[What-is-MemoryLeak]]
- [[Prevent-MemoryLeaks]]
- [[Implement-Auth]]

## Quick Revision

| Concept | Description |
|---------|-------------|
| Throttle | Limits execution frequency |
| Limit | Minimum time between calls |
| Leading | Execute immediately on first call |
| Trailing | Execute after limit period |
| Cancel | Stop pending execution |
| RAF Throttle | Tied to animation frames |

**When to use:**
- Use **debounce** for: search input, form validation, auto-save
- Use **throttle** for: scroll events, mouse movement, resize handling
