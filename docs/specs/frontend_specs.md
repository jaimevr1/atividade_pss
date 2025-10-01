# Dimidui - Especificação Completa Frontend

**Stack:** React 18 + Vite + Tailwind CSS + Supabase Client  
**Target:** Desktop (1024px+)  
**Browsers:** Chrome, Firefox, Edge (últimas 2 versões)

---

## 1. SETUP INICIAL

### 1.1 Criar Projeto

```bash
npm create vite@latest dimidui -- --template react
cd dimidui
npm install
```

### 1.2 Dependências

```bash
# Core
npm install @supabase/supabase-js react-router-dom

# UI & Styling
npm install tailwindcss postcss autoprefixer
npm install framer-motion lucide-react

# Audio
npm install tone

# Utilities
npm install clsx
```

### 1.3 Configurar Tailwind

```bash
npx tailwindcss init -p
```

**tailwind.config.js:**
```javascript
/** @type {import('tailwindcss').Config} */
export default {
  content: [
    "./index.html",
    "./src/**/*.{js,ts,jsx,tsx}",
  ],
  darkMode: 'class',
  theme: {
    extend: {
      colors: {
        // Light Mode
        'bg-light': '#E8EDF2',
        'surface-light': '#FFFFFF',
        'text-light': '#2D3748',
        'text-light-secondary': '#4A5568',
        
        // Dark Mode
        'bg-dark': '#1A202C',
        'surface-dark': '#2D3748',
        'text-dark': '#E2E8F0',
        'text-dark-secondary': '#A0AEC0',
        
        // Primary (Purple)
        'primary': {
          light: '#9F7AEA',
          DEFAULT: '#6B46C1',
          dark: '#553C9A',
        },
        
        // Semantic
        'correct': {
          light: '#68D391',
          DEFAULT: '#48BB78',
        },
        'incorrect': {
          light: '#FC8181',
          DEFAULT: '#F56565',
        },
        'warning': {
          light: '#F6AD55',
          DEFAULT: '#ED8936',
        },
        'info': {
          light: '#63B3ED',
          DEFAULT: '#4299E1',
        },
        
        // Actions
        'action-primary': {
          light: '#FBD38D',
          DEFAULT: '#F6AD55',
        },
        'action-secondary': {
          light: '#718096',
          DEFAULT: '#A0AEC0',
        },
        'action-danger': {
          light: '#FC8181',
          DEFAULT: '#E53E3E',
        },
      },
      fontFamily: {
        'primary': ['Atkinson Hyperlegible', 'sans-serif'],
        'dyslexic': ['OpenDyslexic', 'sans-serif'],
        'character': ['Comic Sans MS', 'Comic Neue', 'cursive'],
      },
      fontSize: {
        'h1': ['32px', { lineHeight: '1.6', fontWeight: '700' }],
        'h2': ['24px', { lineHeight: '1.6', fontWeight: '700' }],
        'question': ['20px', { lineHeight: '1.6', fontWeight: '700' }],
        'body': ['18px', { lineHeight: '1.6', fontWeight: '400' }],
        'dialogue': ['18px', { lineHeight: '1.6', fontWeight: '400' }],
        'feedback': ['16px', { lineHeight: '1.6', fontWeight: '400' }],
        'meta': ['14px', { lineHeight: '1.6', fontWeight: '400' }],
      },
      spacing: {
        'xs': '4px',
        'sm': '8px',
        'md': '16px',
        'lg': '24px',
        'xl': '32px',
        'xxl': '48px',
        'xxxl': '64px',
      },
      borderRadius: {
        'card': '12px',
      },
      transitionDuration: {
        'instant': '100ms',
        'fast': '200ms',
        'normal': '350ms',
        'slow': '600ms',
      },
    },
  },
  plugins: [],
}
```

### 1.4 Configurar Supabase Client

**src/lib/supabase.js:**
```javascript
import { createClient } from '@supabase/supabase-js'

const supabaseUrl = import.meta.env.VITE_SUPABASE_URL
const supabaseAnonKey = import.meta.env.VITE_SUPABASE_ANON_KEY

export const supabase = createClient(supabaseUrl, supabaseAnonKey)
```

**.env:**
```
VITE_SUPABASE_URL=your_supabase_url
VITE_SUPABASE_ANON_KEY=your_anon_key
```

---

## 2. ESTRUTURA DE PASTAS

```
src/
├── App.jsx
├── main.jsx
│
├── assets/
│   ├── fonts/
│   │   └── OpenDyslexic-Regular.woff2
│   └── sounds/
│       └── (Tone.js gera em runtime)
│
├── components/
│   ├── ui/                      # Componentes base reutilizáveis
│   │   ├── Button.jsx
│   │   ├── Card.jsx
│   │   ├── Input.jsx
│   │   └── Modal.jsx
│   │
│   └── layout/
│       ├── Header.jsx
│       ├── Sidebar.jsx
│       └── Container.jsx
│
├── modules/                     # 20 Módulos de atividades
│   ├── BaseModule.jsx           # Componente abstrato
│   ├── M01_BateriaRapida/
│   │   ├── BateriaRapida.jsx
│   │   └── BateriaRapida.config.js
│   ├── M05_IdentificaOperacao/
│   ├── M10_JeremiasResolver/
│   ├── M14_AutoAvaliacao/
│   ├── M20_EstadoJeremias/
│   └── ...
│
├── features/                    # Features complexas
│   ├── activity/
│   │   ├── ActivityRuntime.jsx  # Engine que renderiza módulos
│   │   ├── ActivityProgress.jsx
│   │   └── ModuleRegistry.js    # Registro de módulos disponíveis
│   │
│   ├── jeremias/
│   │   ├── JeremiasAvatar.jsx
│   │   ├── JeremiasDialogue.jsx
│   │   ├── JeremiasState.js     # Lógica de estado
│   │   └── errorCatalog.js      # Catálogo de erros
│   │
│   ├── progress/
│   │   ├── ConceptMap.jsx
│   │   ├── ProgressDashboard.jsx
│   │   └── progressUtils.js
│   │
│   └── feedback/
│       ├── FeedbackSystem.jsx
│       ├── InlineFeedback.jsx
│       └── soundSystem.js       # Tone.js wrapper
│
├── pages/
│   ├── LoginPage.jsx
│   ├── StudentDashboard.jsx
│   ├── ActivityPage.jsx
│   └── TeacherDashboard.jsx
│
├── context/
│   ├── AuthContext.jsx          # Contexto do aluno logado
│   ├── ThemeContext.jsx         # Light/Dark mode
│   └── ActivityContext.jsx      # Estado da atividade em andamento
│
├── hooks/
│   ├── useSupabase.js
│   ├── useActivity.js
│   ├── useJeremias.js
│   └── useSound.js
│
├── utils/
│   ├── scoring.js               # Lógica de pontuação
│   ├── validation.js            # Validação de respostas
│   └── dateUtils.js
│
└── lib/
    ├── supabase.js
    └── constants.js             # Constantes globais
```

---

## 3. DESIGN TOKENS (CSS Variables)

**src/index.css:**
```css
@import url('https://fonts.googleapis.com/css2?family=Atkinson+Hyperlegible:wght@400;700&display=swap');

@font-face {
  font-family: 'OpenDyslexic';
  src: url('./assets/fonts/OpenDyslexic-Regular.woff2') format('woff2');
  font-weight: 400;
  font-style: normal;
}

@tailwind base;
@tailwind components;
@tailwind utilities;

@layer base {
  :root {
    --letter-spacing: 0.01em;
  }
  
  body {
    @apply font-primary text-body bg-bg-light text-text-light;
    letter-spacing: var(--letter-spacing);
  }
  
  body.dark {
    @apply bg-bg-dark text-text-dark;
  }
  
  body.font-dyslexic {
    @apply font-dyslexic;
  }
  
  /* Garantir tamanho mínimo de clique (48x48px) */
  button, a {
    min-width: 48px;
    min-height: 48px;
  }
}

@layer components {
  .card {
    @apply bg-surface-light dark:bg-surface-dark rounded-card shadow-md p-lg;
  }
  
  .btn-primary {
    @apply bg-action-primary hover:bg-action-primary-light 
           text-white font-bold py-sm px-lg rounded-lg
           transition-fast;
  }
  
  .btn-secondary {
    @apply bg-action-secondary hover:bg-action-secondary-light 
           text-white font-bold py-sm px-lg rounded-lg
           transition-fast;
  }
  
  .btn-danger {
    @apply bg-action-danger hover:bg-action-danger-light 
           text-white font-bold py-sm px-lg rounded-lg
           transition-fast;
  }
  
  .feedback-correct {
    @apply text-correct border-2 border-correct bg-correct/10 
           rounded-lg p-md animate-pulse;
  }
  
  .feedback-incorrect {
    @apply text-incorrect border-2 border-incorrect bg-incorrect/10 
           rounded-lg p-md animate-shake;
  }
}

@layer utilities {
  .animate-shake {
    animation: shake 400ms cubic-bezier(0.68, -0.55, 0.265, 1.55);
  }
  
  @keyframes shake {
    0%, 100% { transform: translateX(0); }
    25% { transform: translateX(-8px); }
    75% { transform: translateX(8px); }
  }
  
  .animate-glow {
    animation: glow 600ms ease-in-out;
  }
  
  @keyframes glow {
    0%, 100% { box-shadow: 0 0 0 rgba(72, 187, 120, 0); }
    50% { box-shadow: 0 0 20px rgba(72, 187, 120, 0.6); }
  }
}
```

---

## 4. CONTEXTOS GLOBAIS

### 4.1 AuthContext

**src/context/AuthContext.jsx:**
```javascript
import { createContext, useContext, useState, useEffect } from 'react'
import { supabase } from '../lib/supabase'

const AuthContext = createContext({})

export const AuthProvider = ({ children }) => {
  const [student, setStudent] = useState(null)
  const [loading, setLoading] = useState(true)
  
  useEffect(() => {
    // Carregar aluno do localStorage se existir
    const savedStudentId = localStorage.getItem('dimidui_student_id')
    if (savedStudentId) {
      loadStudent(savedStudentId)
    } else {
      setLoading(false)
    }
  }, [])
  
  const loadStudent = async (studentId) => {
    try {
      const { data, error } = await supabase
        .from('students')
        .select('*')
        .eq('id', studentId)
        .single()
      
      if (error) throw error
      setStudent(data)
    } catch (error) {
      console.error('Error loading student:', error)
    } finally {
      setLoading(false)
    }
  }
  
  const login = async (studentId) => {
    await loadStudent(studentId)
    localStorage.setItem('dimidui_student_id', studentId)
  }
  
  const logout = () => {
    setStudent(null)
    localStorage.removeItem('dimidui_student_id')
  }
  
  return (
    <AuthContext.Provider value={{ student, loading, login, logout }}>
      {children}
    </AuthContext.Provider>
  )
}

export const useAuth = () => useContext(AuthContext)
```

### 4.2 ThemeContext

**src/context/ThemeContext.jsx:**
```javascript
import { createContext, useContext, useState, useEffect } from 'react'

const ThemeContext = createContext({})

export const ThemeProvider = ({ children }) => {
  const [theme, setTheme] = useState('light') // 'light' | 'dark'
  const [font, setFont] = useState('atkinson') // 'atkinson' | 'opendyslexic'
  
  useEffect(() => {
    // Aplicar tema ao body
    document.body.classList.toggle('dark', theme === 'dark')
  }, [theme])
  
  useEffect(() => {
    // Aplicar fonte ao body
    document.body.classList.toggle('font-dyslexic', font === 'opendyslexic')
  }, [font])
  
  const toggleTheme = () => {
    setTheme(prev => prev === 'light' ? 'dark' : 'light')
  }
  
  const toggleFont = () => {
    setFont(prev => prev === 'atkinson' ? 'opendyslexic' : 'atkinson')
  }
  
  return (
    <ThemeContext.Provider value={{ theme, font, toggleTheme, toggleFont }}>
      {children}
    </ThemeContext.Provider>
  )
}

export const useTheme = () => useContext(ThemeContext)
```

### 4.3 ActivityContext

**src/context/ActivityContext.jsx:**
```javascript
import { createContext, useContext, useState } from 'react'

const ActivityContext = createContext({})

export const ActivityProvider = ({ children }) => {
  const [currentActivity, setCurrentActivity] = useState(null)
  const [currentModuleIndex, setCurrentModuleIndex] = useState(0)
  const [interactions, setInteractions] = useState([])
  const [startTime, setStartTime] = useState(null)
  
  const startActivity = (activity) => {
    setCurrentActivity(activity)
    setCurrentModuleIndex(0)
    setInteractions([])
    setStartTime(Date.now())
  }
  
  const nextModule = () => {
    if (currentModuleIndex < currentActivity.modules.length - 1) {
      setCurrentModuleIndex(prev => prev + 1)
    }
  }
  
  const recordInteraction = (interaction) => {
    setInteractions(prev => [...prev, {
      ...interaction,
      timestamp: Date.now(),
      moduleIndex: currentModuleIndex
    }])
  }
  
  const calculateScore = () => {
    const correctAnswers = interactions.filter(i => i.isCorrect).length
    const totalAnswers = interactions.length
    return totalAnswers > 0 ? correctAnswers / totalAnswers : 0
  }
  
  return (
    <ActivityContext.Provider value={{
      currentActivity,
      currentModuleIndex,
      interactions,
      startActivity,
      nextModule,
      recordInteraction,
      calculateScore,
      timeElapsed: startTime ? Date.now() - startTime : 0
    }}>
      {children}
    </ActivityContext.Provider>
  )
}

export const useActivity = () => useContext(ActivityContext)
```

---

## 5. COMPONENTES BASE (UI)

### 5.1 Button

**src/components/ui/Button.jsx:**
```javascript
import clsx from 'clsx'

export const Button = ({ 
  variant = 'primary', // 'primary' | 'secondary' | 'danger'
  size = 'md', // 'sm' | 'md' | 'lg'
  children, 
  className,
  ...props 
}) => {
  const baseClasses = 'font-bold rounded-lg transition-fast disabled:opacity-50 disabled:cursor-not-allowed'
  
  const variantClasses = {
    primary: 'btn-primary',
    secondary: 'btn-secondary',
    danger: 'btn-danger'
  }
  
  const sizeClasses = {
    sm: 'py-xs px-md text-feedback',
    md: 'py-sm px-lg text-body',
    lg: 'py-md px-xl text-question'
  }
  
  return (
    <button
      className={clsx(
        baseClasses,
        variantClasses[variant],
        sizeClasses[size],
        className
      )}
      {...props}
    >
      {children}
    </button>
  )
}
```

### 5.2 Card

**src/components/ui/Card.jsx:**
```javascript
import clsx from 'clsx'

export const Card = ({ children, className, ...props }) => {
  return (
    <div className={clsx('card', className)} {...props}>
      {children}
    </div>
  )
}
```

### 5.3 Input

**src/components/ui/Input.jsx:**
```javascript
import clsx from 'clsx'

export const Input = ({ 
  type = 'text',
  label,
  error,
  className,
  ...props 
}) => {
  return (
    <div className="space-y-xs">
      {label && (
        <label className="block text-feedback font-bold">
          {label}
        </label>
      )}
      <input
        type={type}
        className={clsx(
          'w-full px-md py-sm rounded-lg border-2',
          'focus:outline-none focus:ring-2 focus:ring-primary',
          'bg-surface-light dark:bg-surface-dark',
          'text-text-light dark:text-text-dark',
          error ? 'border-incorrect' : 'border-action-secondary',
          className
        )}
        {...props}
      />
      {error && (
        <p className="text-incorrect text-meta">{error}</p>
      )}
    </div>
  )
}
```

---

## 6. MÓDULOS DE ATIVIDADES (MVP)

### 6.1 BaseModule (Interface)

**src/modules/BaseModule.jsx:**
```javascript
/**
 * Todos os módulos devem estender este componente base
 * ou implementar esta interface
 */
export const BaseModule = ({ config, onComplete }) => {
  throw new Error('BaseModule não deve ser renderizado diretamente')
}

// Interface que todo módulo deve seguir:
/*
Props:
  - config: Object (configuração específica do módulo)
  - onComplete: Function (callback quando módulo é concluído)
  
Callbacks obrigatórios:
  - onComplete({ 
      moduleId: string,
      interactions: Array,
      timeSpent: number,
      data: Object (dados específicos do módulo)
    })
*/
```

### 6.2 M01 - BateriaRápida

**src/modules/M01_BateriaRapida/BateriaRapida.jsx:**
```javascript
import { useState, useEffect } from 'react'
import { Card } from '../../components/ui/Card'
import { Button } from '../../components/ui/Button'
import { Input } from '../../components/ui/Input'

export const BateriaRapida = ({ config, onComplete }) => {
  const [problems, setProblems] = useState([])
  const [currentIndex, setCurrentIndex] = useState(0)
  const [answer, setAnswer] = useState('')
  const [results, setResults] = useState([])
  const [startTime] = useState(Date.now())
  const [feedback, setFeedback] = useState(null)
  
  useEffect(() => {
    // Gerar problemas baseado na configuração
    const generated = generateProblems(config)
    setProblems(generated)
  }, [config])
  
  const generateProblems = ({ concept, quantity, operations }) => {
    const probs = []
    for (let i = 0; i < quantity; i++) {
      const a = randomInt(operations.min[0], operations.max[0])
      const b = randomInt(operations.min[1], operations.max[1])
      
      let problem, correctAnswer
      
      switch(concept) {
        case 'multiplicação':
          problem = `${a} × ${b}`
          correctAnswer = a * b
          break
        case 'divisão':
          problem = `${a * b} ÷ ${a}`
          correctAnswer = b
          break
        case 'adição':
          problem = `${a} + ${b}`
          correctAnswer = a + b
          break
        case 'subtração':
          problem = `${a} - ${b}`
          correctAnswer = a - b
          break
        default:
          problem = `${a} + ${b}`
          correctAnswer = a + b
      }
      
      probs.push({ problem, correctAnswer, id: i })
    }
    return probs
  }
  
  const randomInt = (min, max) => {
    return Math.floor(Math.random() * (max - min + 1)) + min
  }
  
  const handleSubmit = (e) => {
    e.preventDefault()
    
    const currentProblem = problems[currentIndex]
    const userAnswer = parseInt(answer)
    const isCorrect = userAnswer === currentProblem.correctAnswer
    
    // Registrar resultado
    const result = {
      problem: currentProblem.problem,
      userAnswer,
      correctAnswer: currentProblem.correctAnswer,
      isCorrect,
      timestamp: Date.now()
    }
    
    setResults(prev => [...prev, result])
    
    // Mostrar feedback
    setFeedback({
      isCorrect,
      correctAnswer: currentProblem.correctAnswer
    })
    
    // Limpar input
    setAnswer('')
    
    // Próximo problema após 1s
    setTimeout(() => {
      setFeedback(null)
      if (currentIndex < problems.length - 1) {
        setCurrentIndex(prev => prev + 1)
      } else {
        // Completou todos
        onComplete({
          moduleId: 'bateria_rapida',
          interactions: results,
          timeSpent: Date.now() - startTime,
          data: {
            totalProblems: problems.length,
            correctAnswers: results.filter(r => r.isCorrect).length,
            score: results.filter(r => r.isCorrect).length / problems.length
          }
        })
      }
    }, 1000)
  }
  
  if (problems.length === 0) return <div>Carregando...</div>
  
  const currentProblem = problems[currentIndex]
  
  return (
    <Card className="max-w-2xl mx-auto">
      <div className="space-y-lg">
        {/* Progresso */}
        <div className="flex justify-between items-center">
          <span className="text-meta">
            Questão {currentIndex + 1} de {problems.length}
          </span>
          <div className="w-1/2 bg-action-secondary/30 rounded-full h-2">
            <div 
              className="bg-primary h-2 rounded-full transition-normal"
              style={{ width: `${((currentIndex + 1) / problems.length) * 100}%` }}
            />
          </div>
        </div>
        
        {/* Problema */}
        <div className="text-center">
          <p className="text-h1 font-bold">{currentProblem.problem} = ?</p>
        </div>
        
        {/* Input */}
        <form onSubmit={handleSubmit} className="space-y-md">
          <Input
            type="number"
            value={answer}
            onChange={(e) => setAnswer(e.target.value)}
            placeholder="Sua resposta"
            autoFocus
            disabled={feedback !== null}
            className="text-center text-h2"
          />
          
          <Button 
            type="submit" 
            disabled={!answer || feedback !== null}
            className="w-full"
          >
            Verificar
          </Button>
        </form>
        
        {/* Feedback */}
        {feedback && (
          <div className={feedback.isCorrect ? 'feedback-correct' : 'feedback-incorrect'}>
            {feedback.isCorrect ? (
              <p className="text-center font-bold">✓ Correto!</p>
            ) : (
              <p className="text-center font-bold">
                ✗ Incorreto. A resposta era {feedback.correctAnswer}
              </p>
            )}
          </div>
        )}
      </div>
    </Card>
  )
}
```

### 6.3 M20 - EstadoJeremias

**src/modules/M20_EstadoJeremias/EstadoJeremias.jsx:**
```javascript
import { Card } from '../../components/ui/Card'
import { Button } from '../../components/ui/Button'

export const EstadoJeremias = ({ config, onComplete }) => {
  const { mood, dialogue } = config
  
  const moodEmojis = {
    neutral: '🙂',
    thinking: '🤔',
    confused: '😕',
    eureka: '😮',
    confident: '😊',
    grateful: '🙏'
  }
  
  return (
    <Card className="max-w-2xl mx-auto text-center">
      <div className="space-y-lg">
        {/* Avatar do Jeremias */}
        <div className="flex justify-center">
          <div className="text-[96px] animate-bounce">
            {moodEmojis[mood] || '🙂'}
          </div>
        </div>
        
        {/* Nome */}
        <h2 className="text-h2 font-bold">Jeremias</h2>
        
        {/* Diálogo */}
        <div className="bg-primary/10 rounded-lg p-lg">
          <p className="text-dialogue font-character">
            {dialogue}
          </p>
        </div>
        
        {/* Botão */}
        <Button 
          onClick={() => onComplete({ moduleId: 'estado_jeremias', data: {} })}
          className="w-full"
        >
          Continuar
        </Button>
      </div>
    </Card>
  )
}
```

### 6.4 M10 - JeremiasResolver

**src/modules/M10_JeremiasResolver/JeremiasResolver.jsx:**
```javascript
import { useState } from 'react'
import { Card } from '../../components/ui/Card'
import { Button } from '../../components/ui/Button'

export const JeremiasResolver = ({ config, onComplete }) => {
  const { problem, alexAttempt, correctionPrompts } = config
  const [stage, setStage] = useState('showing') // 'showing' | 'identifying' | 'correcting' | 'complete'
  const [selectedStep, setSelectedStep] = useState(null)
  const [selectedErrorType, setSelectedErrorType] = useState(null)
  
  const handleIdentifyError = (stepIndex) => {
    setSelectedStep(stepIndex)
    
    if (stepIndex === alexAttempt.errorStep) {
      // Correto!
      setStage('correcting')
    } else {
      // Errado, tenta novamente
      alert('Não é nesse passo. Tente outro!')
    }
  }
  
  const handleSelectErrorType = (errorType) => {
    setSelectedErrorType(errorType)
    
    if (errorType === alexAttempt.errorType) {
      setStage('complete')
    } else {
      alert('Não é esse tipo de erro. Tente outro!')
    }
  }
  
  const handleComplete = () => {
    onComplete({
      moduleId: 'jeremias_resolver',
      interactions: [
        { action: 'identified_error_step', value: selectedStep },
        { action: 'identified_error_type', value: selectedErrorType }
      ],
      timeSpent: 0, // TODO: rastrear tempo
      data: {
        correctIdentification: true,
        errorType: alexAttempt.errorType
      }
    })
  }
  
  return (
    <Card className="max-w-3xl mx-auto">
      <div className="space-y-lg">
        {/* Problema */}
        <div className="bg-info/10 rounded-lg p-md">
          <p className="text-question font-bold">Problema:</p>
          <p className="text-body">{problem.text}</p>
        </div>
        
        {/* Tentativa do Jeremias */}
        {stage === 'showing' && (
          <div className="space-y-md">
            <div className="flex items-center gap-md">
              <span className="text-[64px]">🤔</span>
              <div>
                <p className="text-h2 font-bold">Jeremias tentou resolver:</p>
                <p className="text-body font-character">Vou mostrar meu raciocínio...</p>
              </div>
            </div>
            
            <div className="space-y-sm">
              {alexAttempt.reasoning.map((step, index) => (
                <div 
                  key={index}
                  className="bg-surface-light dark:bg-surface-dark p-md rounded-lg border-2 border-action-secondary/30"
                >
                  <p className="text-body">
                    <span className="font-bold">Passo {index + 1}:</span> {step}
                  </p>
                </div>
              ))}
            </div>
            
            <Button onClick={() => setStage('identifying')} className="w-full">
              {correctionPrompts[0] || "Em qual passo Jeremias errou?"}
            </Button>
          </div>
        )}
        
        {/* Identificar passo do erro */}
        {stage === 'identifying' && (
          <div className="space-y-md">
            <p className="text-question font-bold">Clique no passo onde Jeremias errou:</p>
            <div className="space-y-sm">
              {alexAttempt.reasoning.map((step, index) => (
                <button
                  key={index}
                  onClick={() => handleIdentifyError(index)}
                  className={`w-full text-left p-md rounded-lg border-2 transition-fast hover:border-primary ${
                    selectedStep === index ? 'border-primary bg-primary/10' : 'border-action-secondary/30'
                  }`}
                >
                  <p className="text-body">
                    <span className="font-bold">Passo {index + 1}:</span> {step}
                  </p>
                </button>
              ))}
            </div>
          </div>
        )}
        
        {/* Identificar tipo de erro */}
        {stage === 'correcting' && (
          <div className="space-y-md">
            <p className="text-question font-bold">
              {correctionPrompts[1] || "Qual foi o erro do Jeremias?"}
            </p>
            
            <div className="space-y-sm">
              {['Confundiu operação', 'Erro de cálculo', 'Não leu direito', 'Confundiu unidades'].map((errorOption) => (
                <button
                  key={errorOption}
                  onClick={() => handleSelectErrorType(errorOption.toLowerCase().replace(/ /g, '_'))}
                  className="w-full text-left p-md rounded-lg border-2 border-action-secondary/30 hover:border-primary transition-fast"
                >
                  <p className="text-body">{errorOption}</p>
                </button>
              ))}
            </div>
          </div>
        )}
        
        {/* Jeremias agradece */}
        {stage === 'complete' && (
          <div className="space-y-md text-center">
            <span className="text-[96px]">😮</span>
            <p className="text-h2 font-bold">Jeremias entendeu!</p>
            <div className="bg-correct/10 rounded-lg p-lg">
              <p className="text-dialogue font-character">
                Ah! Agora eu entendi! Obrigado por me ensinar! Vou lembrar disso na próxima vez.
              </p>
            </div>
            <Button onClick={handleComplete} className="w-full">
              Continuar
            </Button>
          </div>
        )}
      </div>
    </Card>
  )
}
```

### 6.5 M14 - AutoAvaliação

**src/modules/M14_AutoAvaliacao/AutoAvaliacao.jsx:**
```javascript
import { useState } from 'react'
import { Card } from '../../components/ui/Card'
import { Button } from '../../components/ui/Button'

export const AutoAvaliacao = ({ config, onComplete }) => {
  const { concepts, scale } = config
  const [responses, setResponses] = useState({})
  
  const handleSelect = (conceptIndex, scaleValue) => {
    setResponses(prev => ({
      ...prev,
      [conceptIndex]: scaleValue
    }))
  }
  
  const handleSubmit = () => {
    onComplete({
      moduleId: 'auto_avaliacao',
      interactions: Object.entries(responses).map(([index, value]) => ({
        concept: concepts[index],
        selfAssessment: value
      })),
      data: { responses }
    })
  }
  
  const allAnswered = concepts.every((_, i) => responses[i] !== undefined)
  
  return (
    <Card className="max-w-2xl mx-auto">
      <div className="space-y-lg">
        <h2 className="text-h2 font-bold text-center">Como você se sente sobre...</h2>
        
        {concepts.map((concept, conceptIndex) => (
          <div key={conceptIndex} className="space-y-sm">
            <p className="text-question font-bold">{concept}</p>
            <div className="grid grid-cols-1 gap-sm">
              {scale.map((option, scaleIndex) => (
                <button
                  key={scaleIndex}
                  onClick={() => handleSelect(conceptIndex, scaleIndex)}
                  className={`p-md rounded-lg border-2 transition-fast text-left ${
                    responses[conceptIndex] === scaleIndex
                      ? 'border-primary bg-primary/10'
                      : 'border-action-secondary/30 hover:border-primary/50'
                  }`}
                >
                  <p className="text-body">{option}</p>
                </button>
              ))}
            </div>
          </div>
        ))}
        
        <Button 
          onClick={handleSubmit}
          disabled={!allAnswered}
          className="w-full"
        >
          Concluir
        </Button>
      </div>
    </Card>
  )
}
```

---

## 7. MODULE REGISTRY

**src/features/activity/ModuleRegistry.js:**
```javascript
import { BateriaRapida } from '../../modules/M01_BateriaRapida/BateriaRapida'
import { EstadoJeremias } from '../../modules/M20_EstadoJeremias/EstadoJeremias'
import { JeremiasResolver } from '../../modules/M10_JeremiasResolver/JeremiasResolver'
import { AutoAvaliacao } from '../../modules/M14_AutoAvaliacao/AutoAvaliacao'

export const MODULE_REGISTRY = {
  'bateria_rapida': BateriaRapida,
  'estado_jeremias': EstadoJeremias,
  'jeremias_resolver': JeremiasResolver,
  'auto_avaliacao': AutoAvaliacao,
  // Adicionar outros módulos conforme implementados
}

export const getModuleComponent = (moduleType) => {
  const Component = MODULE_REGISTRY[moduleType]
  if (!Component) {
    console.error(`Módulo ${moduleType} não encontrado no registro`)
    return null
  }
  return Component
}
```

---

## 8. ACTIVITY RUNTIME ENGINE

**src/features/activity/ActivityRuntime.jsx:**
```javascript
import { useState, useEffect } from 'react'
import { useActivity } from '../../context/ActivityContext'
import { getModuleComponent } from './ModuleRegistry'
import { Button } from '../../components/ui/Button'
import { supabase } from '../../lib/supabase'
import { useAuth } from '../../context/AuthContext'

export const ActivityRuntime = ({ activityId }) => {
  const { student } = useAuth()
  const { 
    currentActivity, 
    currentModuleIndex, 
    startActivity, 
    nextModule, 
    recordInteraction,
    calculateScore,
    interactions 
  } = useActivity()
  
  const [loading, setLoading] = useState(true)
  
  useEffect(() => {
    loadActivity()
  }, [activityId])
  
  const loadActivity = async () => {
    try {
      const { data, error } = await supabase
        .from('activities')
        .select('*')
        .eq('id', activityId)
        .single()
      
      if (error) throw error
      
      startActivity(data)
      
      // Criar registro de progresso
      await supabase
        .from('student_progress')
        .upsert({
          student_id: student.id,
          activity_id: activityId,
          status: 'in_progress',
          started_at: new Date().toISOString()
        })
      
    } catch (error) {
      console.error('Error loading activity:', error)
    } finally {
      setLoading(false)
    }
  }
  
  const handleModuleComplete = async (moduleResult) => {
    // Registrar interações
    recordInteraction(moduleResult)
    
    // Salvar progresso
    await saveProgress()
    
    // Próximo módulo ou finalizar
    if (currentModuleIndex < currentActivity.modules.length - 1) {
      nextModule()
    } else {
      await completeActivity()
    }
  }
  
  const saveProgress = async () => {
    try {
      await supabase
        .from('student_progress')
        .update({
          interactions: interactions,
          time_spent: Math.floor((Date.now() - startTime) / 1000)
        })
        .eq('student_id', student.id)
        .eq('activity_id', activityId)
    } catch (error) {
      console.error('Error saving progress:', error)
    }
  }
  
  const completeActivity = async () => {
    try {
      const score = calculateScore()
      
      await supabase
        .from('student_progress')
        .update({
          status: 'completed',
          completed_at: new Date().toISOString(),
          final_score: score,
          interactions: interactions
        })
        .eq('student_id', student.id)
        .eq('activity_id', activityId)
      
      // Atualizar concept_mastery
      await updateConceptMastery()
      
      // Redirecionar para dashboard
      window.location.href = '/dashboard'
      
    } catch (error) {
      console.error('Error completing activity:', error)
    }
  }
  
  const updateConceptMastery = async () => {
    // TODO: Implementar lógica de atualização de domínio conceitual
    // baseado nas interações e BNCC codes da atividade
  }
  
  if (loading) {
    return <div className="text-center py-xxxl">Carregando atividade...</div>
  }
  
  if (!currentActivity) {
    return <div className="text-center py-xxxl">Atividade não encontrada</div>
  }
  
  const currentModule = currentActivity.modules[currentModuleIndex]
  const ModuleComponent = getModuleComponent(currentModule.type)
  
  if (!ModuleComponent) {
    return <div className="text-center py-xxxl">Módulo não implementado: {currentModule.type}</div>
  }
  
  return (
    <div className="min-h-screen py-xl">
      {/* Barra de progresso global */}
      <div className="max-w-4xl mx-auto mb-lg px-md">
        <div className="flex justify-between items-center mb-sm">
          <span className="text-meta">
            Módulo {currentModuleIndex + 1} de {currentActivity.modules.length}
          </span>
          <span className="text-meta">{currentActivity.name}</span>
        </div>
        <div className="w-full bg-action-secondary/30 rounded-full h-2">
          <div 
            className="bg-primary h-2 rounded-full transition-normal"
            style={{ 
              width: `${((currentModuleIndex + 1) / currentActivity.modules.length) * 100}%` 
            }}
          />
        </div>
      </div>
      
      {/* Módulo atual */}
      <ModuleComponent
        config={currentModule.config}
        onComplete={handleModuleComplete}
      />
    </div>
  )
}
```

---

## 9. PÁGINAS PRINCIPAIS

### 9.1 LoginPage

**src/pages/LoginPage.jsx:**
```javascript
import { useState, useEffect } from 'react'
import { useNavigate } from 'react-router-dom'
import { useAuth } from '../context/AuthContext'
import { Card } from '../components/ui/Card'
import { supabase } from '../lib/supabase'

export const LoginPage = () => {
  const [students, setStudents] = useState([])
  const { login } = useAuth()
  const navigate = useNavigate()
  
  useEffect(() => {
    loadStudents()
  }, [])
  
  const loadStudents = async () => {
    const { data, error } = await supabase
      .from('students')
      .select('*')
      .order('name')
    
    if (error) {
      console.error('Error loading students:', error)
    } else {
      setStudents(data)
    }
  }
  
  const handleSelectStudent = async (studentId) => {
    await login(studentId)
    navigate('/dashboard')
  }
  
  return (
    <div className="min-h-screen flex items-center justify-center p-md">
      <Card className="max-w-2xl w-full">
        <div className="space-y-lg">
          <div className="text-center">
            <h1 className="text-h1 font-bold text-primary">Dimidui</h1>
            <p className="text-body text-text-light-secondary dark:text-text-dark-secondary">
              Aprenda Fazendo, Ensine Crescendo
            </p>
          </div>
          
          <div className="space-y-sm">
            <p className="text-question font-bold text-center">Quem é você?</p>
            <div className="grid grid-cols-2 gap-md">
              {students.map((student) => (
                <button
                  key={student.id}
                  onClick={() => handleSelectStudent(student.id)}
                  className="p-lg rounded-lg border-2 border-action-secondary/30 hover:border-primary hover:bg-primary/10 transition-fast"
                >
                  <p className="text-body font-bold">{student.name}</p>
                  <p className="text-meta">{student.grade_level}º ano</p>
                </button>
              ))}
            </div>
          </div>
        </div>
      </Card>
    </div>
  )
}
```

### 9.2 StudentDashboard

**src/pages/StudentDashboard.jsx:**
```javascript
import { useState, useEffect } from 'react'
import { useNavigate } from 'react-router-dom'
import { useAuth } from '../context/AuthContext'
import { Card } from '../components/ui/Card'
import { Button } from '../components/ui/Button'
import { supabase } from '../lib/supabase'

export const StudentDashboard = () => {
  const { student, logout } = useAuth()
  const navigate = useNavigate()
  const [weeklyActivity, setWeeklyActivity] = useState(null)
  const [jeremiasState, setJeremiasState] = useState(null)
  const [stats, setStats] = useState({ completed: 0, conceptsMastered: 0 })
  
  useEffect(() => {
    if (student) {
      loadDashboardData()
    }
  }, [student])
  
  const loadDashboardData = async () => {
    // Carregar atividade da semana
    const { data: activity } = await supabase
      .from('activities')
      .select('*')
      .eq('published', true)
      .order('week_number', { ascending: false })
      .limit(1)
      .single()
    
    setWeeklyActivity(activity)
    
    // Carregar estado do Jeremias
    const { data: jeremias } = await supabase
      .from('jeremias_state')
      .select('*')
      .eq('student_id', student.id)
      .single()
    
    setJeremiasState(jeremias)
    
    // Carregar estatísticas
    const { data: progress } = await supabase
      .from('student_progress')
      .select('*')
      .eq('student_id', student.id)
      .eq('status', 'completed')
    
    const { data: concepts } = await supabase
      .from('concept_mastery')
      .select('*')
      .eq('student_id', student.id)
      .gte('mastery_level', 0.75)
    
    setStats({
      completed: progress?.length || 0,
      conceptsMastered: concepts?.length || 0
    })
  }
  
  return (
    <div className="min-h-screen py-xl px-md">
      <div className="max-w-4xl mx-auto space-y-lg">
        {/* Header */}
        <div className="flex justify-between items-center">
          <h1 className="text-h1 font-bold">Olá, {student?.name}! 👋</h1>
          <Button variant="secondary" onClick={logout}>
            Sair
          </Button>
        </div>
        
        {/* Progresso */}
        <Card>
          <h2 className="text-h2 font-bold mb-md">📊 Meu Progresso</h2>
          <div className="grid grid-cols-2 gap-md">
            <div className="text-center p-md bg-primary/10 rounded-lg">
              <p className="text-h1 font-bold text-primary">{stats.completed}</p>
              <p className="text-body">Atividades Completas</p>
            </div>
            <div className="text-center p-md bg-correct/10 rounded-lg">
              <p className="text-h1 font-bold text-correct">{stats.conceptsMastered}</p>
              <p className="text-body">Conceitos Dominados</p>
            </div>
          </div>
        </Card>
        
        {/* Atividade da Semana */}
        {weeklyActivity && (
          <Card>
            <h2 className="text-h2 font-bold mb-md">🎯 Atividade desta Semana</h2>
            <div className="bg-primary/5 rounded-lg p-lg space-y-md">
              <div>
                <h3 className="text-question font-bold">{weeklyActivity.name}</h3>
                <p className="text-meta">
                  Tipo: {weeklyActivity.type === 'fluencia' ? 'Fluência' : 'Integração'} | 
                  Duração: ~{weeklyActivity.estimated_duration} min
                </p>
              </div>
              <Button onClick={() => navigate(`/activity/${weeklyActivity.id}`)}>
                Começar →
              </Button>
            </div>
          </Card>
        )}
        
        {/* Estado do Jeremias */}
        {jeremiasState && (
          <Card>
            <h2 className="text-h2 font-bold mb-md">🤝 Jeremias</h2>
            <div className="flex items-start gap-md">
              <span className="text-[64px]">😊</span>
              <div className="flex-1">
                <div className="flex justify-between mb-sm">
                  <span className="text-body font-bold">Nível {jeremiasState.maturity_level}</span>
                  <span className="text-meta">
                    {jeremiasState.concepts_taught?.length || 0} conceitos aprendidos
                  </span>
                </div>
                <div className="bg-primary/10 rounded-lg p-md">
                  <p className="text-dialogue font-character">
                    {jeremiasState.last_dialogue || "Estou ansioso para aprender mais com você!"}
                  </p>
                </div>
              </div>
            </div>
          </Card>
        )}
      </div>
    </div>
  )
}
```

---

## 10. ROUTING

**src/App.jsx:**
```javascript
import { BrowserRouter, Routes, Route, Navigate } from 'react-router-dom'
import { AuthProvider, useAuth } from './context/AuthContext'
import { ThemeProvider } from './context/ThemeContext'
import { ActivityProvider } from './context/ActivityContext'

import { LoginPage } from './pages/LoginPage'
import { StudentDashboard } from './pages/StudentDashboard'
import { ActivityPage } from './pages/ActivityPage'
import { TeacherDashboard } from './pages/TeacherDashboard'

const ProtectedRoute = ({ children }) => {
  const { student, loading } = useAuth()
  
  if (loading) return <div>Carregando...</div>
  
  return student ? children : <Navigate to="/login" />
}

function App() {
  return (
    <BrowserRouter>
      <ThemeProvider>
        <AuthProvider>
          <ActivityProvider>
            <Routes>
              <Route path="/login" element={<LoginPage />} />
              
              <Route 
                path="/dashboard" 
                element={
                  <ProtectedRoute>
                    <StudentDashboard />
                  </ProtectedRoute>
                } 
              />
              
              <Route 
                path="/activity/:activityId" 
                element={
                  <ProtectedRoute>
                    <ActivityPage />
                  </ProtectedRoute>
                } 
              />
              
              <Route path="/teacher" element={<TeacherDashboard />} />
              
              <Route path="/" element={<Navigate to="/login" />} />
            </Routes>
          </ActivityProvider>
        </AuthProvider>
      </ThemeProvider>
    </BrowserRouter>
  )
}

export default App
```

**src/pages/ActivityPage.jsx:**
```javascript
import { useParams } from 'react-router-dom'
import { ActivityRuntime } from '../features/activity/ActivityRuntime'

export const ActivityPage = () => {
  const { activityId } = useParams()
  
  return <ActivityRuntime activityId={activityId} />
}
```

---

## 11. CHECKLIST DE IMPLEMENTAÇÃO (MVP)

### Fase 1: Setup (Dia 1)
- [ ] Criar projeto Vite
- [ ] Instalar todas dependências
- [ ] Configurar Tailwind com design tokens
- [ ] Configurar Supabase client
- [ ] Adicionar fonte OpenDyslexic aos assets
- [ ] Criar arquivo .env com credenciais

### Fase 2: Componentes Base (Dia 2-3)
- [ ] Implementar Button
- [ ] Implementar Card
- [ ] Implementar Input
- [ ] Implementar Modal (se necessário)
- [ ] Criar contextos (Auth, Theme, Activity)
- [ ] Testar componentes isoladamente

### Fase 3: Módulos MVP (Dia 4-6)
- [ ] Implementar M20 - EstadoJeremias
- [ ] Implementar M01 - BateriaRápida
- [ ] Implementar M14 - AutoAvaliação
- [ ] Implementar M10 - JeremiasResolver
- [ ] Implementar M05 - IdentificaOperação
- [ ] Criar ModuleRegistry
- [ ] Testar cada módulo isoladamente

### Fase 4: Runtime & Pages (Dia 7-9)
- [ ] Implementar ActivityRuntime
- [ ] Implementar LoginPage
- [ ] Implementar StudentDashboard
- [ ] Implementar ActivityPage
- [ ] Configurar routing
- [ ] Integrar com Supabase (CRUD básico)

### Fase 5: Features Avançadas (Dia 10-12)
- [ ] Sistema de sons (Tone.js)
- [ ] Animações (Framer Motion)
- [ ] Salvar/carregar progresso
- [ ] Atualizar concept_mastery
- [ ] Atualizar jeremias_state
- [ ] Toggle de tema (Light/Dark)
- [ ] Toggle de fonte (Atkinson/OpenDyslexic)

### Fase 6: Testing & Polish (Dia 13-14)
- [ ] Testar fluxo completo (Login → Atividade → Conclusão)
- [ ] Testar responsividade (1024px+)
- [ ] Testar acessibilidade (contraste, tamanho clique)
- [ ] Corrigir bugs encontrados
- [ ] Deploy no Vercel

---

## 12. COMANDOS ÚTEIS

```bash
# Desenvolvimento
npm run dev

# Build para produção
npm run build

# Preview do build
npm run preview

# Deploy no Vercel (após instalar vercel-cli)
vercel --prod
```

---

## 13. NOTAS IMPORTANTES

1. **Todos os módulos devem chamar `onComplete`** quando finalizados
2. **Sempre salvar progresso** após cada módulo (não perder dados)
3. **Feedback imediato** em atividades de fluência (não esperar)
4. **Animações sutis** - não distrair do conteúdo pedagógico
5. **Acessibilidade** - manter contraste WCAG AAA, tamanhos de clique 48px+
6. **Performance** - otimizar para 5 usuários simultâneos
7. **Estados de loading** - sempre mostrar feedback visual ao usuário

---

**FIM DA ESPECIFICAÇÃO FRONTEND**