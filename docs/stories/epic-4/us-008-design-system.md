# US-008: Sistema de Design e Tokens

**Epic:** EPIC-004 - Sistema Visual e Experiência
**Prioridade:** Must Have (MVP)
**Estimativa:** 3 Story Points
**Tipo:** Visual Infrastructure

## User Story

**As a** developer and designer
**I want to** have a consistent, accessible design system implemented
**So that** all interfaces follow the Dimidui visual identity and accessibility standards

## Business Value

Implementa os **Princípios P3 e P4** da PRD: "Respeito Cognitivo" e "Economia de Atenção". Garante **consistência visual** e **acessibilidade** em toda a plataforma, criando experiência coesa que respeita a cognição dos estudantes de 9-12 anos.

## Acceptance Criteria

### AC-1: Design Tokens Implementation
**Given** the Dimidui design requirements from the PRD
**When** I implement the design system
**Then** it should include:
- Complete color palette (light and dark modes)
- Typography hierarchy with appropriate font loading
- Spacing scale for consistent layout
- Animation tokens for micro-interactions
- Component tokens for reusable elements

### AC-2: Component Library
**Given** common UI patterns across the platform
**When** I create reusable components
**Then** they should include:
- Base components (Button, Input, Card, etc.)
- Educational components (ProgressBar, FeedbackMessage, etc.)
- Layout components (Container, Grid, Stack)
- All components accessible (WCAG AA)
- Proper TypeScript/PropTypes definitions

### AC-3: Theme System
**Given** users need light/dark mode options
**When** theme switching is implemented
**Then** it should:
- Smoothly transition between themes
- Maintain proper contrast ratios in both modes
- Preserve user preference across sessions
- Work correctly with all components

### AC-4: Responsive System
**Given** the platform targets desktop usage (1024px+)
**When** implementing responsive design
**Then** it should:
- Scale appropriately for different desktop sizes
- Maintain usability on larger screens (1440px+)
- Provide consistent spacing and typography
- Ensure touch targets remain accessible (48px+)

## Technical Requirements

### Design Tokens (Tailwind CSS Configuration)
```javascript
// tailwind.config.js
module.exports = {
  theme: {
    extend: {
      colors: {
        // Light mode (primary palette)
        background: '#E8EDF2',
        surface: '#FFFFFF',

        primary: {
          50: '#F3F0FF',
          500: '#6B46C1', // Primary purple
          600: '#553C9A',
          700: '#4C1D95'
        },

        // Semantic colors
        semantic: {
          correct: '#48BB78',
          incorrect: '#F56565',
          warning: '#ED8936',
          info: '#4299E1'
        },

        // Action colors
        action: {
          primary: '#F6AD55',
          secondary: '#A0AEC0',
          danger: '#E53E3E'
        },

        // Text colors
        text: {
          primary: '#2D3748',
          secondary: '#4A5568',
          onDark: '#FFFFFF'
        }
      },

      fontFamily: {
        'atkinson': ['Atkinson Hyperlegible', 'sans-serif'],
        'opendyslexic': ['OpenDyslexic', 'sans-serif'],
        'comic': ['Comic Sans MS', 'cursive', 'sans-serif']
      },

      fontSize: {
        'h1': ['32px', { lineHeight: '1.2', fontWeight: '700' }],
        'h2': ['24px', { lineHeight: '1.3', fontWeight: '700' }],
        'question': ['20px', { lineHeight: '1.4', fontWeight: '700' }],
        'body': ['18px', { lineHeight: '1.6', fontWeight: '400' }],
        'dialogue': ['18px', { lineHeight: '1.5', fontWeight: '400' }],
        'feedback': ['16px', { lineHeight: '1.5', fontWeight: '400' }],
        'meta': ['14px', { lineHeight: '1.4', fontWeight: '400' }]
      },

      spacing: {
        'xs': '4px',
        'sm': '8px',
        'md': '16px',
        'lg': '24px',
        'xl': '32px',
        'xxl': '48px',
        'xxxl': '64px'
      },

      borderRadius: {
        'module': '12px'
      },

      animation: {
        'gentle-bounce': 'bounce 0.5s cubic-bezier(0.68, -0.55, 0.265, 1.55)',
        'fade-in': 'fadeIn 0.3s ease-out',
        'slide-up': 'slideUp 0.4s ease-out'
      }
    }
  },

  // Dark mode configuration
  darkMode: 'class'
}
```

### Typography System
```javascript
// Typography hierarchy implementation
const TypographyScale = {
  h1: {
    fontSize: '32px',
    lineHeight: 1.2,
    fontWeight: 700,
    fontFamily: 'Atkinson Hyperlegible',
    usage: 'Activity titles'
  },

  h2: {
    fontSize: '24px',
    lineHeight: 1.3,
    fontWeight: 700,
    fontFamily: 'Atkinson Hyperlegible',
    usage: 'Section headers'
  },

  question: {
    fontSize: '20px',
    lineHeight: 1.4,
    fontWeight: 700,
    fontFamily: 'Atkinson Hyperlegible',
    usage: 'Problem statements, questions'
  },

  body: {
    fontSize: '18px',
    lineHeight: 1.6,
    fontWeight: 400,
    fontFamily: 'Atkinson Hyperlegible',
    usage: 'Regular text content'
  },

  dialogue: {
    fontSize: '18px',
    lineHeight: 1.5,
    fontWeight: 400,
    fontFamily: 'Comic Sans MS',
    usage: 'Jeremias speech only'
  },

  feedback: {
    fontSize: '16px',
    lineHeight: 1.5,
    fontWeight: 400,
    fontFamily: 'Atkinson Hyperlegible',
    usage: 'Inline feedback messages'
  },

  meta: {
    fontSize: '14px',
    lineHeight: 1.4,
    fontWeight: 400,
    fontFamily: 'Atkinson Hyperlegible',
    usage: 'Secondary information, labels'
  }
}
```

### Font Loading System
```javascript
// Font loading with proper fallbacks
const FontLoader = {
  async loadFonts() {
    try {
      // Load Atkinson Hyperlegible from Google Fonts
      await this.loadGoogleFont('Atkinson Hyperlegible');

      // Load OpenDyslexic from local assets
      await this.loadLocalFont('OpenDyslexic', '/assets/fonts/opendyslexic.woff2');

      // Comic Sans MS is system font (fallback to cursive)
      this.validateSystemFont('Comic Sans MS');

    } catch (error) {
      console.warn('Font loading failed, using fallbacks:', error);
      this.enableFallbackMode();
    }
  },

  async loadGoogleFont(fontName) {
    const link = document.createElement('link');
    link.href = `https://fonts.googleapis.com/css2?family=${fontName.replace(' ', '+')}:wght@400;700&display=swap`;
    link.rel = 'stylesheet';
    document.head.appendChild(link);

    return new Promise((resolve, reject) => {
      link.onload = resolve;
      link.onerror = reject;
      setTimeout(reject, 3000); // Timeout after 3s
    });
  }
}
```

## Component Library Structure

### Base Components
```jsx
// Button component with design system integration
const Button = ({
  variant = 'primary',
  size = 'md',
  children,
  disabled = false,
  ...props
}) => {
  const baseClasses = 'font-body rounded-module transition-all duration-200 focus:outline-none focus:ring-2';

  const variants = {
    primary: 'bg-action-primary text-text-primary hover:bg-action-primary/90',
    secondary: 'bg-action-secondary text-text-primary hover:bg-action-secondary/90',
    danger: 'bg-action-danger text-white hover:bg-action-danger/90'
  };

  const sizes = {
    sm: 'px-md py-sm text-feedback',
    md: 'px-lg py-md text-body',
    lg: 'px-xl py-lg text-question'
  };

  return (
    <button
      className={`${baseClasses} ${variants[variant]} ${sizes[size]} ${disabled ? 'opacity-50 cursor-not-allowed' : ''}`}
      disabled={disabled}
      {...props}
    >
      {children}
    </button>
  );
};

// Input component with accessibility
const Input = ({
  label,
  error,
  type = 'text',
  required = false,
  ...props
}) => (
  <div className="space-y-sm">
    {label && (
      <label className="block text-meta font-medium text-text-secondary">
        {label} {required && <span className="text-semantic-incorrect">*</span>}
      </label>
    )}
    <input
      type={type}
      className={`
        w-full px-md py-md text-body font-atkinson
        border-2 rounded-module
        transition-colors duration-200
        focus:outline-none focus:ring-2 focus:ring-primary-500
        ${error ? 'border-semantic-incorrect' : 'border-action-secondary'}
        ${error ? 'focus:border-semantic-incorrect' : 'focus:border-primary-500'}
      `}
      aria-invalid={error ? 'true' : 'false'}
      aria-describedby={error ? `${props.id}-error` : undefined}
      {...props}
    />
    {error && (
      <p id={`${props.id}-error`} className="text-feedback text-semantic-incorrect">
        {error}
      </p>
    )}
  </div>
);
```

### Educational Components
```jsx
// Progress bar with educational context
const ProgressBar = ({
  current,
  total,
  threshold = 0.8,
  concept,
  showThreshold = true
}) => {
  const percentage = (current / total) * 100;
  const thresholdPercentage = threshold * 100;
  const isMastered = percentage >= thresholdPercentage;

  return (
    <div className="space-y-sm">
      <div className="flex justify-between text-meta">
        <span>{concept}</span>
        <span>{current}/{total} ({Math.round(percentage)}%)</span>
      </div>

      <div className="relative w-full h-md bg-action-secondary/20 rounded-full overflow-hidden">
        <div
          className={`h-full transition-all duration-500 ${
            isMastered ? 'bg-semantic-correct' : 'bg-primary-500'
          }`}
          style={{ width: `${Math.min(percentage, 100)}%` }}
        />

        {showThreshold && (
          <div
            className="absolute top-0 w-0.5 h-full bg-text-secondary/50"
            style={{ left: `${thresholdPercentage}%` }}
          />
        )}
      </div>

      {isMastered && (
        <div className="flex items-center space-x-xs text-semantic-correct text-feedback">
          <span>✓</span>
          <span>Conceito dominado!</span>
        </div>
      )}
    </div>
  );
};

// Feedback message component
const FeedbackMessage = ({
  type = 'correct',
  message,
  explanation,
  onContinue
}) => {
  const types = {
    correct: {
      icon: '✓',
      bgColor: 'bg-semantic-correct/10',
      textColor: 'text-semantic-correct',
      borderColor: 'border-semantic-correct/30'
    },
    incorrect: {
      icon: '✗',
      bgColor: 'bg-semantic-incorrect/10',
      textColor: 'text-semantic-incorrect',
      borderColor: 'border-semantic-incorrect/30'
    },
    info: {
      icon: 'ℹ',
      bgColor: 'bg-semantic-info/10',
      textColor: 'text-semantic-info',
      borderColor: 'border-semantic-info/30'
    }
  };

  const config = types[type];

  return (
    <div className={`
      p-lg rounded-module border-2 space-y-md
      ${config.bgColor} ${config.borderColor}
      animate-fade-in
    `}>
      <div className="flex items-start space-x-md">
        <span className={`text-question ${config.textColor}`}>
          {config.icon}
        </span>
        <div className="flex-1 space-y-sm">
          <p className={`text-body font-medium ${config.textColor}`}>
            {message}
          </p>
          {explanation && (
            <p className="text-feedback text-text-secondary">
              {explanation}
            </p>
          )}
        </div>
      </div>

      {onContinue && (
        <div className="flex justify-end">
          <Button
            variant={type === 'correct' ? 'primary' : 'secondary'}
            onClick={onContinue}
          >
            Continuar
          </Button>
        </div>
      )}
    </div>
  );
};
```

## Theme System Implementation

### Theme Provider
```jsx
const ThemeProvider = ({ children }) => {
  const [theme, setTheme] = useState(() => {
    return localStorage.getItem('dimidui-theme') || 'light';
  });

  const toggleTheme = () => {
    const newTheme = theme === 'light' ? 'dark' : 'light';
    setTheme(newTheme);
    localStorage.setItem('dimidui-theme', newTheme);

    // Apply theme to document
    document.documentElement.className = newTheme;
  };

  useEffect(() => {
    document.documentElement.className = theme;
  }, [theme]);

  return (
    <ThemeContext.Provider value={{ theme, toggleTheme }}>
      {children}
    </ThemeContext.Provider>
  );
};
```

### Dark Mode Color Adjustments
```css
/* Dark mode overrides */
.dark {
  --background: #1A202C;
  --surface: #2D3748;
  --primary-500: #9F7AEA;
  --text-primary: #E2E8F0;
  --text-secondary: #A0AEC0;

  /* Semantic colors adjusted for dark mode */
  --semantic-correct: #68D391;
  --semantic-incorrect: #FC8181;
  --semantic-warning: #F6AD55;
  --semantic-info: #63B3ED;
}
```

## Accessibility Implementation

### Focus Management
```css
/* Focus styles for keyboard navigation */
.focus-visible {
  @apply outline-none ring-2 ring-primary-500 ring-offset-2;
}

/* Ensure adequate contrast */
.text-contrast-aa {
  color: contrast(var(--background), #000000, #FFFFFF, 4.5);
}

/* Screen reader only content */
.sr-only {
  @apply absolute w-px h-px p-0 -m-px overflow-hidden whitespace-nowrap border-0;
}
```

### Animation Preferences
```css
/* Respect prefers-reduced-motion */
@media (prefers-reduced-motion: reduce) {
  * {
    animation-duration: 0.01ms !important;
    animation-iteration-count: 1 !important;
    transition-duration: 0.01ms !important;
  }
}
```

## Definition of Done

- [ ] Complete design token system implemented in Tailwind
- [ ] Typography hierarchy working with proper font loading
- [ ] Light/dark theme system functional
- [ ] Base component library created and documented
- [ ] Educational components implemented
- [ ] Accessibility features working (WCAG AA)
- [ ] Font preferences toggle (Atkinson ↔ OpenDyslexic)
- [ ] Animation system respects motion preferences
- [ ] Responsive behavior tested on various desktop sizes
- [ ] Performance optimization (CSS bundle <100KB)
- [ ] Storybook documentation for all components
- [ ] Design system usage guidelines documented

## Performance Requirements

- **CSS bundle size**: <100KB minified + gzipped
- **Font loading**: <2 seconds for all fonts
- **Theme switching**: <200ms transition
- **Component rendering**: <16ms for 60fps animations
- **Memory usage**: <10MB for design system

## Testing Strategy

### Visual Regression Testing
- Component appearance consistency
- Theme switching visual integrity
- Font loading fallback behavior
- Responsive layout validation

### Accessibility Testing
- Keyboard navigation throughout all components
- Screen reader compatibility
- Color contrast validation (WCAG AA)
- Focus management and visible indicators

### Performance Testing
- CSS load time measurement
- Font loading optimization
- Animation frame rate monitoring
- Memory usage profiling

## Integration Points

### With Module Components
```jsx
// Modules use design system components
const ModuleWrapper = ({ children, title }) => (
  <Card className="module-container">
    <Typography variant="h2" className="module-title">
      {title}
    </Typography>
    <div className="module-content">
      {children}
    </div>
  </Card>
);
```

### With Character System
```jsx
// Jeremias uses consistent design tokens
const JeremiasAvatar = ({ mood, size = 'lg' }) => (
  <div className={`
    jeremias-avatar
    ${sizes[size]}
    transition-all duration-500
    animate-gentle-bounce
  `}>
    <span className="emoji-avatar" role="img" aria-label={`Jeremias está ${mood}`}>
      {moodEmojis[mood]}
    </span>
  </div>
);
```

## Risks & Mitigations

| Risco | Prob | Impacto | Mitigação |
|-------|------|---------|-----------|
| Font loading failures | Média | Baixo | Robust fallback system |
| Performance com muitos componentes | Baixa | Médio | Code splitting, lazy loading |
| Inconsistência visual | Média | Médio | Automated testing, guidelines |
| Acessibilidade gaps | Baixa | Alto | Comprehensive testing, audits |

## Future Enhancements (V2+)

- **Animation library expansion** with educational micro-interactions
- **Component variants** for different age groups
- **Advanced theming** with custom color schemes
- **Motion design system** for educational feedback
- **High contrast mode** for visual impairments