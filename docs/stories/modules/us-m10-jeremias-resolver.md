# US-M10: Módulo JeremiasResolver

**Epic:** EPIC-002 - Sistema do Personagem Jeremias
**Prioridade:** Must Have (MVP)
**Estimativa:** 8 Story Points
**Tipo:** Educational Module (Teaching)

## User Story

**As a** 4th-6th grade student
**I want to** help Jeremias identify and correct his conceptual errors
**So that** I can strengthen my own understanding by teaching (Feynman technique)

## Business Value

Este é o **módulo mais inovador** do Dimidui, implementando a técnica Feynman através de ensino peer-to-peer com personagem virtual. Diferencial competitivo único que transforma erros em oportunidades de aprendizado profundo.

## Acceptance Criteria

### AC-1: Problem Setup and Jeremias Reasoning
**Given** Jeremias is presented with a math problem
**When** he attempts to solve it step-by-step
**Then** I should see:
- Original problem clearly displayed
- Jeremias avatar in "thinking" mood (🤔)
- His reasoning shown step-by-step in distinct boxes
- One deliberate conceptual error embedded in his process
- Comic Sans font for all Jeremias dialogue

### AC-2: Error Identification Interface
**Given** Jeremias completes his (incorrect) solution
**When** I need to find his mistake
**Then** I should be able to:
- Click on the specific step where he made the error
- See visual feedback when clicking (highlight, border)
- Access clear instructions: "Clique onde Jeremias errou"
- Have multiple attempts if I click the wrong step initially

### AC-3: Error Explanation and Teaching
**Given** I correctly identify the error location
**When** I explain what type of mistake he made
**Then** I should:
- See 3-4 multiple choice explanations in student-friendly language
- Select the option that best describes his conceptual error
- Receive feedback on whether my teaching was accurate
- Watch Jeremias transition to "eureka" mood (😮) when correct

### AC-4: Jeremias Learning and Gratitude
**Given** I successfully teach Jeremias the correct approach
**When** he understands his mistake
**Then** he should:
- Show emotional transition: confused → eureka → grateful
- Express genuine gratitude: "Ah! Entendi! Obrigado por me ensinar!"
- Demonstrate understanding by explaining the correction
- Update his internal state (maturity progress)

### AC-5: Student Independent Practice
**Given** I have successfully taught Jeremias
**When** the teaching interaction completes
**Then** I should:
- Be presented with a similar problem to solve independently
- Demonstrate my own mastery without Jeremias' help
- Show that teaching strengthened my understanding
- Have my performance compared with pre-teaching baseline

## Technical Requirements

### Module Configuration Schema
```json
{
  "moduleType": "JeremiasResolver",
  "config": {
    "concept": "multiplicação_grupos",
    "difficulty": "level_2",
    "problemContext": {
      "text": "Maria comprou 3 pacotes de figurinhas. Cada pacote tem 8 figurinhas. Quantas figurinhas Maria tem?",
      "correctSolution": "3 × 8 = 24",
      "realWorldContext": true
    },
    "jeremiasErrorConfig": {
      "errorType": "confunde_multiplicacao_adicao",
      "errorStep": 3,
      "maturityLevel": 2
    },
    "teaching": {
      "allowMultipleAttempts": true,
      "provideCues": true,
      "independentPractice": true
    }
  }
}
```

### Jeremias Reasoning Structure
```javascript
const jeremiasReasoning = [
  {
    step: 1,
    text: "Maria comprou 3 pacotes de figurinhas",
    isCorrect: true,
    mood: "thinking",
    clickable: false
  },
  {
    step: 2,
    text: "Cada pacote tem 8 figurinhas",
    isCorrect: true,
    mood: "thinking",
    clickable: false
  },
  {
    step: 3,
    text: "Então eu preciso somar: 3 + 8 = 11 figurinhas",
    isCorrect: false,
    errorType: "confunde_multiplicacao_adicao",
    mood: "confused",
    clickable: true,
    explanation: "Quando temos grupos iguais, multiplicamos!"
  },
  {
    step: 4,
    text: "A resposta é 11 figurinhas",
    isCorrect: false,
    dependsOn: 3,
    mood: "confident", // falsely confident
    clickable: false
  }
]
```

### Error Catalog Integration
```javascript
const errorCatalog = {
  multiplicacao: {
    level_1: {
      errorType: "confunde_com_adicao",
      description: "Soma ao invés de multiplicar",
      studentExplanations: [
        "Confundiu multiplicação com adição", // CORRECT
        "Errou a conta de somar",
        "Não leu o problema direito",
        "Esqueceu dos números"
      ],
      feedback: "Exato! Quando temos grupos iguais, multiplicamos, não somamos."
    },
    level_2: {
      errorType: "esquece_reagrupamento",
      description: "Não reagrupa na multiplicação",
      // ... more complex errors
    }
  }
}
```

## UI/UX Requirements

### Visual Design
- **Problem display**: 20px/700 at top, clearly separated
- **Jeremias avatar**: 96x96px, positioned prominently
- **Reasoning steps**: Numbered boxes, clear visual hierarchy
- **Error highlighting**: Subtle red border when clickable step selected
- **Multiple choice**: Large buttons, easy to read options

### Step-by-Step Layout
```
┌─────────────────────────────────────┐
│ PROBLEMA:                           │
│ Maria comprou 3 pacotes de          │
│ figurinhas. Cada pacote tem 8       │
│ figurinhas. Quantas figurinhas ela  │
│ tem?                                │
├─────────────────────────────────────┤
│ 🤔 Jeremias                         │
│                                     │
│ ┌─ 1. Maria comprou 3 pacotes      │
│ ├─ 2. Cada pacote tem 8 figurinhas │
│ ├─ 3. [Então somo: 3 + 8 = 11] ← ❌│
│ └─ 4. A resposta é 11              │
│                                     │
│ 👆 Clique onde Jeremias errou       │
└─────────────────────────────────────┘
```

### Animation Sequence
1. **Problem presentation** (2s): Show problem, Jeremias in thinking mood
2. **Step-by-step reveal** (800ms per step): Each reasoning step appears
3. **Error indication** (1s): Jeremias transitions to confused mood
4. **Student interaction**: Clickable interface enabled
5. **Teaching moment** (2s): Correct selection → eureka → grateful
6. **Independent practice** (5-10min): Similar problem for student

## Educational Design

### Error Types by Maturity Level

#### Level 1: Basic Confusion
```javascript
{
  concept: "addition_vs_multiplication",
  error: "Uses addition when multiplication needed",
  difficulty: "obvious",
  explanation: "Simple operation confusion"
}
```

#### Level 2: Procedural Errors
```javascript
{
  concept: "multiplication_algorithm",
  error: "Forgets to regroup/carry in multi-digit",
  difficulty: "intermediate",
  explanation: "Algorithmic step missed"
}
```

#### Level 3: Conceptual Misunderstanding
```javascript
{
  concept: "word_problem_interpretation",
  error: "Misidentifies what operation is needed",
  difficulty: "complex",
  explanation: "Requires deep understanding"
}
```

### Teaching Validation Logic
```javascript
const validateTeaching = (selectedExplanation, errorType) => {
  const correctExplanations = errorCatalog[concept][level].studentExplanations;
  const isCorrect = correctExplanations[0] === selectedExplanation;

  return {
    isCorrect,
    feedback: isCorrect ?
      errorCatalog[concept][level].feedback :
      "Vamos pensar melhor. " + getHint(errorType),
    jeremiasResponse: isCorrect ?
      generateGratitudeResponse(concept, maturityLevel) :
      "Hmm, ainda não entendi..."
  };
}
```

## Definition of Done

- [ ] Jeremias displays step-by-step reasoning with embedded errors
- [ ] Clickable error identification system working
- [ ] Multiple choice explanation validation
- [ ] Mood transition animations (thinking → confused → eureka → grateful)
- [ ] Database integration for Jeremias state updates
- [ ] Independent practice problems generated correctly
- [ ] Error catalog covers all MVP mathematical concepts
- [ ] Comic Sans font loading and display
- [ ] Accessibility: keyboard navigation, screen readers
- [ ] Performance: smooth animations, no lag
- [ ] Unit tests for error generation and teaching validation
- [ ] Integration tests with character state system
- [ ] User testing shows learning improvement through teaching

## Jeremias Character Development

### Dialogue Examples by Maturity Level

#### Level 1 (Beginner)
```
Problem solving: "Vou tentar... mas é difícil para mim."
After error: "Acho que errei, né? Você pode me ajudar?"
Learning: "Ah! Agora entendi! Muito obrigado!"
```

#### Level 3 (Progressing)
```
Problem solving: "Deixa eu pensar... acho que sei como fazer."
After error: "Hmm, algo não está certo. Onde errei?"
Learning: "Claro! Faz sentido agora. Obrigado por me ensinar!"
```

#### Level 5 (Master)
```
Problem solving: "Vou resolver e você me corrige se eu errar."
After error: "Ops! Pegou meu erro? Onde foi que escorreguei?"
Learning: "Perfeito! Agora posso ensinar isso para outros também!"
```

## Integration Points

### Data Flow to Character State
```javascript
// Updates to jeremias_state table
const updateJeremiasAfterTeaching = {
  concepts_taught: [...previous, newConcept],
  corrections_made: previous + 1,
  times_taught: previous + 1,
  maturity_level: calculateNewLevel(corrections_made),
  last_dialogue: generateContextualDialogue(maturityLevel, concept),
  last_mood: "grateful",
  last_interaction: timestamp
}
```

### Analytics for Teachers
```javascript
// Teaching effectiveness data
{
  student: "Ana Silva",
  teaching_sessions: 12,
  concepts_taught: ["multiplicação", "divisão"],
  teaching_accuracy: 0.85, // 85% correct error identification
  jeremias_growth: "level_2_to_3",
  time_spent_teaching: 240, // minutes total
  learning_through_teaching: {
    pre_teaching_score: 0.65,
    post_teaching_score: 0.82,
    improvement: 0.17
  }
}
```

## Testing Strategy

### Learning Effectiveness Testing
- Pre/post assessment of concept understanding
- Compare students who teach vs those who don't
- Measure retention over time
- Track error identification accuracy improvement

### Character Engagement Testing
- Student emotional response to Jeremias
- Completion rates of teaching interactions
- Preference for teaching vs solo practice
- Long-term relationship development

## Risks & Mitigations

| Risco | Prob | Impacto | Mitigação |
|-------|------|---------|-----------|
| Students find errors too easy/hard | Média | Alto | Adaptive error selection, user testing |
| Teaching validation too strict | Média | Médio | Multiple valid explanations, flexible matching |
| Jeremias personality doesn't resonate | Baixa | Alto | Character testing, dialogue refinement |

## Performance Requirements

- Error generation: <200ms
- Step reveal animation: Smooth 60fps
- Mood transitions: <500ms
- Teaching validation: <100ms
- Independent problem generation: <300ms

## Content Creation Requirements

### Error Catalog Development
- **50+ error types** across MVP concepts
- **Age-appropriate explanations** for each error
- **Multiple student explanation options** per error
- **Brazilian Portuguese cultural context**
- **Pedagogical review** by education specialists