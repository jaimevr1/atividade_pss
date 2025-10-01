# US-M1: Módulo BateriaRápida

**Epic:** EPIC-001 - Sistema de Módulos Educacionais
**Prioridade:** Must Have (MVP)
**Estimativa:** 5 Story Points
**Tipo:** Educational Module

## User Story

**As a** 4th-6th grade student
**I want to** practice mathematical operations through rapid-fire exercises
**So that** I can develop fluency in basic math operations with immediate feedback

## Business Value

Atende à **habilidade BNCC EF05MA03** (Divisão com Resto) e **EF04MA07** (Problemas de Multiplicação). É o módulo central para desenvolvimento de **fluência matemática**, um dos objetivos primários do Dimidui.

## Acceptance Criteria

### AC-1: Problem Generation and Display
**Given** the module is configured for a specific operation type
**When** I start the BateriaRápida session
**Then** I should see:
- A clear math problem (e.g., "17 ÷ 5 = ?")
- Number input field with proper validation
- Progress indicator ("Problema 3 de 30")
- Timer if configured for time limits

### AC-2: Answer Processing
**Given** I submit an answer to a math problem
**When** the system validates my response
**Then** I should receive:
- Immediate visual feedback (✓ correct / ✗ incorrect)
- Correct answer shown if I was wrong
- Automatic progression to next problem after 2 seconds
- Score tracking throughout the session

### AC-3: Session Completion
**Given** I complete all problems or reach time limit
**When** the session ends
**Then** I should see:
- Final score display (e.g., "24/30 (80%)")
- Performance summary with encouraging message
- Progression to next module in activity
- Data saved for teacher analytics

## Technical Requirements

### Module Configuration Schema
```json
{
  "moduleType": "BateriaRápida",
  "config": {
    "operationType": "division", // "addition", "subtraction", "multiplication", "division"
    "numberRange": {
      "min": 10,
      "max": 100
    },
    "quantity": 30,
    "timeLimit": null, // seconds, null for no limit
    "allowCalculator": false,
    "showProgress": true,
    "difficultyCurve": "flat" // "flat", "ascending", "mixed"
  },
  "metadata": {
    "estimatedDuration": 15, // minutes
    "bnccCodes": ["EF05MA03", "EF04MA07"],
    "targetAge": [9, 10, 11, 12]
  }
}
```

### Problem Generation Logic
```javascript
// Problem generation examples
const generateProblem = (operationType, range) => {
  switch (operationType) {
    case 'division':
      // Ensure clean division or remainder handling
      return generateDivisionProblem(range);
    case 'multiplication':
      return generateMultiplicationProblem(range);
    // ... other operations
  }
}

// Division with remainder example
generateDivisionProblem({ min: 10, max: 50 }) => {
  dividend: 23,
  divisor: 5,
  expectedAnswer: "4 resto 3", // or decimal if configured
  explanation: "23 ÷ 5 = 4 com resto 3, porque 4 × 5 = 20, e 23 - 20 = 3"
}
```

## UI/UX Requirements

### Visual Design
- Problem text: 20px/700/Atkinson Hyperlegible (Question style from PRD)
- Input field: Large, clear, with focus indication
- Progress bar: Visual and accessible
- Feedback animations: Subtle glow (correct) or gentle shake (incorrect)

### Layout Structure
```
┌─────────────────────────────────────┐
│ Problema 15 de 30          [Clock]  │
├─────────────────────────────────────┤
│                                     │
│           23 ÷ 5 = ?                │
│                                     │
│     [________] [Confirmar]          │
│                                     │
│ ████████████████░░░░  (50%)         │
│                                     │
│ [Pausar]                            │
└─────────────────────────────────────┘
```

### Accessibility Features
- Keyboard navigation (Tab, Enter)
- Screen reader support for problem reading
- High contrast mode compatibility
- Font size preferences (OpenDyslexic option)

## Performance Requirements

- Problem generation: <50ms per problem
- Answer validation: <100ms
- Transition between problems: Smooth, no jarring jumps
- Memory usage: <20MB for 100 problems

## Educational Features

### Adaptive Difficulty (Future)
- Increase complexity based on performance
- Mix easier problems if student struggling
- Bonus challenges for high performers

### Error Pattern Analysis
- Track common mistake types
- Generate reports for teachers
- Adjust future problem selection

## Definition of Done

- [ ] Supports all 4 basic operations (+, -, ×, ÷)
- [ ] Configurable via JSON schema
- [ ] Handles division with remainders correctly
- [ ] Progress tracking functional
- [ ] Integration with feedback system
- [ ] Performance requirements met
- [ ] Responsive design (1024px+ screens)
- [ ] Accessibility compliance (WCAG AA)
- [ ] Unit tests 90%+ coverage
- [ ] Integration tests with various configurations
- [ ] User testing with 9-12 year olds
- [ ] Teacher preview functionality

## Integration Points

### Data Output
```javascript
// Data sent to next module and analytics
{
  moduleType: "BateriaRápida",
  sessionData: {
    totalProblems: 30,
    correctAnswers: 24,
    incorrectAnswers: 6,
    timeSpent: 893, // seconds
    averageTimePerProblem: 29.8,
    conceptsAttempted: ["division_basic", "division_remainder"],
    errorPatterns: ["remainder_ignored", "division_order"],
    finalScore: 0.80
  },
  bnccProgress: {
    "EF05MA03": { attempts: 15, correct: 12, mastery: 0.80 },
    "EF04MA07": { attempts: 15, correct: 12, mastery: 0.80 }
  }
}
```

## Testing Strategy

### Unit Tests
- Problem generation algorithms
- Answer validation logic
- Configuration parsing
- Score calculation

### Integration Tests
- Full session completion
- Data flow to next modules
- Error handling scenarios
- Performance under load

### User Acceptance Tests
- Age-appropriate problem difficulty
- Feedback clarity and helpfulness
- Interface usability
- Engagement maintenance

## Risks & Mitigations

| Risco | Prob | Impacto | Mitigação |
|-------|------|---------|-----------|
| Problems too easy/hard | Média | Alto | Teste extensivo com usuários, configurabilidade |
| Repetitive/boring experience | Alta | Médio | Variedade de problemas, micro-recompensas |
| Performance com muitos problemas | Baixa | Médio | Lazy loading, otimização algoritmos |

## Content Requirements

### Problem Variety
- Real-world contexts when appropriate
- Abstract number practice
- Visual problems (future: with diagrams)
- Word problems integrated with M5

### Brazilian Context
- Use Real currency for money problems
- Familiar objects and scenarios
- Appropriate cultural references