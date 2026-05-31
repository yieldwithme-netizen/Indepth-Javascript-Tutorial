# CSS in JS

CSS-in-JS refers to the pattern of writing CSS styles using JavaScript. It allows you to define styles dynamically, scope styles to components, and leverage JavaScript's power for styling.

## What is CSS-in-JS?

CSS-in-JS is a styling approach where CSS is written in JavaScript files rather than separate CSS files. It provides:

- **Component-scoped styles** - No class name conflicts
- **Dynamic styling** - Styles based on props/state
- **JavaScript variables** - Use JS variables in styles
- **Colocation** - Styles live with components
- **Dead code elimination** - Unused styles are removed

## Popular Libraries

- styled-components
- Emotion
- Glamorous
- JSS
- Linaria

## Styled Components Example

```javascript
import styled from 'styled-components';

// Basic component
const Button = styled.button`
  background-color: ${props => props.primary ? 'blue' : 'gray'};
  color: white;
  padding: 10px 20px;
  border: none;
  border-radius: 5px;
  cursor: pointer;

  &:hover {
    opacity: 0.8;
  }
`;

// Usage
function App() {
  return (
    <div>
      <Button primary>Primary Button</Button>
      <Button>Secondary Button</Button>
    </div>
  );
}
```

## Emotion Example

```javascript
import { css, jsx } from '@emotion/react';

const buttonStyle = css`
  background-color: #007bff;
  color: white;
  padding: 10px 20px;
  border: none;
  border-radius: 5px;

  &:hover {
    background-color: #0056b3;
  }
`;

function Button({ children }) {
  return <button css={buttonStyle}>{children}</button>;
}
```

## Dynamic Styles

```javascript
const StyledDiv = styled.div`
  background-color: ${props => props.bgColor || 'white'};
  font-size: ${props => props.large ? '24px' : '16px'};
  padding: ${props => props.padding || '10px'};
  margin: ${props => props.margin || '0'};
`;

// Usage
<StyledDiv bgColor="lightblue" large padding="20px">
  Dynamic content
</StyledDiv>
```

## Theming

```javascript
import { ThemeProvider } from 'styled-components';

const theme = {
  colors: {
    primary: '#007bff',
    secondary: '#6c757d',
    success: '#28a745',
    danger: '#dc3545'
  },
  fonts: {
    main: 'Arial, sans-serif',
    heading: 'Georgia, serif'
  }
};

const Button = styled.button`
  background-color: ${props => props.theme.colors.primary};
  font-family: ${props => props.theme.fonts.main};
`;

function App() {
  return (
    <ThemeProvider theme={theme}>
      <Button>Themed Button</Button>
    </ThemeProvider>
  );
}
```

## Global Styles

```javascript
import { createGlobalStyle } from 'styled-components';

const GlobalStyle = createGlobalStyle`
  * {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
  }

  body {
    font-family: 'Arial', sans-serif;
    line-height: 1.6;
  }
`;

function App() {
  return (
    <>
      <GlobalStyle />
      <div>Content</div>
    </>
  );
}
```

## Animations

```javascript
import styled, { keyframes } from 'styled-components';

const fadeIn = keyframes`
  from {
    opacity: 0;
    transform: translateY(-20px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
`;

const AnimatedDiv = styled.div`
  animation: ${fadeIn} 0.5s ease-in-out;
`;
```

## Common Use Cases

- Component libraries
- Design systems
- Dynamic theming
- Responsive design
- Animation systems

## Common Mistakes

1. **Over-styling** - Don't put every style in JS
2. **Performance issues** - Use proper memoization
3. **Bundle size** - Some libraries add significant weight
4. **Debugging difficulty** - Class names are dynamic
5. **Learning curve** - Team needs to adapt

## Benefits and Drawbacks

**Benefits:**
- No CSS conflicts
- Dynamic styles
- Colocation
- Better tooling
- Dead code elimination

**Drawbacks:**
- Runtime overhead
- Bundle size increase
- Learning curve
- Debugging complexity

## Related Topics

- [[CSS-Basics]]
- [[React-Components]]
- [[Theming]]
- [[Responsive-Design]]
- [[Component-Libraries]]

## Quick Revision

| Approach | Description |
|----------|-------------|
| styled-components | tagged template literals |
| Emotion | css prop or styled API |
| CSS Modules | Local scope CSS |
| Tailwind | Utility-first CSS |

CSS-in-JS is powerful for component-based architectures but requires careful consideration of trade-offs.