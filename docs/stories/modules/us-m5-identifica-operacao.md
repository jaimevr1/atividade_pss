# US-M5: Módulo IdentificaOperação

**Epic:** EPIC-001 - Sistema de Módulos Educacionais
**Prioridade:** Must Have (MVP)
**Estimativa:** 3 Story Points
**Tipo:** Educational Module

## User Story

**As a** 4th-6th grade student
**I want to** read word problems and identify which mathematical operation to use
**So that** I can improve my text interpretation skills and problem-solving approach

## Business Value

Integra **matemática com português**, atendendo às habilidades BNCC **EF04MA07** (Problemas de Multiplicação) e **EF04LP15** (Localização de informações explícitas). Desenvolve competência crítica de **interpretação de problemas**.

## Acceptance Criteria

### AC-1: Problem Presentation
**Given** I start the IdentificaOperação module
**When** a word problem is displayed
**Then** I should see:
- Clear problem text with readable typography
- Highlighted keywords that suggest operations
- Four operation buttons: [Adição] [Subtração] [Multiplicação] [Divisão]
- Problem counter (e.g., "Problema 3 de 10")

### AC-2: Operation Selection
**Given** I read a word problem
**When** I click one of the operation buttons
**Then** I should receive:
- Immediate feedback (correct/incorrect)
- Brief explanation connecting keywords to operation
- Automatic progression to next problem
- No penalty for wrong choices (learning opportunity)

### AC-3: Educational Feedback
**Given** I make an incorrect selection
**When** the feedback is displayed
**Then** I should see:
- The correct operation highlighted
- Simple explanation (e.g., "João GANHOU maçãs, então somamos")
- Keywords emphasized to reinforce learning
- Encouragement to continue

## Technical Requirements

### Module Configuration Schema
```json
{
  "moduleType": "IdentificaOperação",
  "config": {
    "problemSet": "mixed_operations_level_1",
    "problemCount": 10,
    "showKeywords": true,
    "difficulty": "basic", // "basic", "intermediate", "advanced"
    "randomizeOrder": true,
    "feedbackLevel": "detailed", // "minimal", "detailed", "explanatory"
    "allowRetry": false
  },
  "problemBank": [
    {
      "id": "add_001",
      "text": "João tinha 5 maçãs e ganhou 3. Quantas maçãs ele tem agora?",
      "correctOperation": "addition",
      "keywords": ["ganhou", "agora", "total"],
      "explanation": "João GANHOU maçãs, então somamos as que ele tinha com as que ganhou.",
      "districtors": {
        "subtraction": "João não perdeu maçãs, ele ganhou",
        "multiplication": "Não estamos fazendo grupos iguais",
        "division": "Não estamos dividindo ou repartindo"
      }
    }
  ]
}
```

### Problem Categories and Keywords

#### Addition Keywords
- **Ganhou, recebeu, comprou, juntou, soma, total, junto**
- Exemplo: "Maria **comprou** 4 lápis e **ganhou** 2. Quantos lápis ela tem no **total**?"

#### Subtraction Keywords
- **Perdeu, gastou, deu, comeu, saiu, restou, sobrou**
- Exemplo: "Pedro tinha 12 figurinhas e **deu** 5 para Ana. Quantas **sobraram**?"

#### Multiplication Keywords
- **Cada, grupos, vezes, pacotes, fileiras, total de**
- Exemplo: "Ana comprou 3 **pacotes** de balas. **Cada** pacote tem 8 balas."

#### Division Keywords
- **Distribuir, dividir, repartir, cada um, grupos iguais**
- Exemplo: "Carlos vai **repartir** 20 chocolates entre 4 amigos **igualmente**."

## UI/UX Requirements

### Visual Design
- Problem text: 18px/400/Atkinson Hyperlegible
- Keywords: Subtle underline or bold (not distracting)
- Operation buttons: 48px height minimum, clear icons + text
- Feedback area: Below buttons, slide-in animation

### Button Layout
```
┌─────────────────────────────────────┐
│ João tinha 5 maçãs e ganhou 3.      │
│ Quantas maçãs ele tem agora?        │
│                                     │
│ [+ Adição]      [- Subtração]       │
│ [× Multiplicação] [÷ Divisão]       │
│                                     │
│ Problema 3 de 10                    │
└─────────────────────────────────────┘
```

### Feedback Animation
- **Correct**: Green glow around correct button (600ms)
- **Incorrect**: Red shake on incorrect + show correct (400ms)
- **Explanation**: Slides in below with connecting keywords highlighted

## Educational Design

### Problem Progression
1. **Level 1**: Single clear keyword (ganhou = adição)
2. **Level 2**: Multiple keywords, one dominant
3. **Level 3**: Ambiguous contexts requiring careful reading

### Keyword Teaching Strategy
- Gradually introduce keyword families
- Show connections between similar words
- Build vocabulary systematically
- Cultural context (Brazilian Portuguese)

## Definition of Done

- [ ] Renders problems from configurable bank
- [ ] Four-button operation selection interface
- [ ] Keyword highlighting system functional
- [ ] Immediate educational feedback
- [ ] Progress tracking per problem type
- [ ] Integration with activity flow
- [ ] Responsive design (1024px+ screens)
- [ ] Accessibility compliance (keyboard navigation, screen readers)
- [ ] Content reviewed by Brazilian educator
- [ ] Unit tests for problem validation logic
- [ ] Integration tests with various problem sets
- [ ] User testing shows comprehension improvement

## Content Requirements

### Minimum Problem Bank (MVP)
- **20 Addition problems** (various contexts)
- **20 Subtraction problems** (loss, consumption, giving)
- **15 Multiplication problems** (groups, arrays, repeated addition)
- **15 Division problems** (sharing, grouping, quotitive)

### Problem Quality Criteria
- Age-appropriate vocabulary (9-12 years)
- Single correct operation (no ambiguity)
- Realistic scenarios from student experience
- Gender-neutral names when possible
- Brazilian cultural references (Real currency, local contexts)

### Context Variety
- **School**: classroom supplies, lunch, sports
- **Home**: family activities, chores, cooking
- **Play**: toys, games, collections
- **Nature**: animals, plants, weather
- **Community**: shopping, transportation, events

## Integration Points

### Data Output for Analytics
```javascript
{
  moduleType: "IdentificaOperação",
  sessionData: {
    totalProblems: 10,
    correctSelections: 8,
    errorsByOperation: {
      "addition": { attempts: 3, correct: 3 },
      "subtraction": { attempts: 3, correct: 2 },
      "multiplication": { attempts: 2, correct: 2 },
      "division": { attempts: 2, correct: 1 }
    },
    keywordRecognition: {
      "ganhou": 100, // % recognition rate
      "perdeu": 100,
      "cada": 75,
      "repartir": 50
    },
    timeSpent: 420, // seconds
    conceptsStruggling: ["division_context"]
  }
}
```

## Testing Strategy

### Content Testing
- Problem clarity with target age group
- Cultural appropriateness review
- Keyword effectiveness validation
- Difficulty progression testing

### Technical Testing
- Problem rendering across devices
- Button interaction reliability
- Feedback timing and clarity
- Data collection accuracy

## Risks & Mitigations

| Risco | Prob | Impacto | Mitigação |
|-------|------|---------|-----------|
| Problems ambiguous/confusing | Média | Alto | Review pedagógico, teste usuário |
| Keywords não reconhecidos | Média | Médio | Validação vocabulário, adaptação regional |
| Interface muito complexa | Baixa | Médio | Design iterativo, simplicidade |

## Future Enhancements (V2+)
- Problem generation with templates
- Adaptive keyword emphasis
- Integration with text comprehension modules
- Teacher custom problem creation

---

## QA Results

### Review Date: 2025-10-01
### Reviewed By: Quinn (Test Architect)
### Review Stage: PLANNING

**Gate**: CONCERNS → docs/qa/gates/epic1-m5-identifica-operacao.yml
**Quality Score**: 68/100

### Key Findings

**INTEGRATES MATH + PORTUGUESE** - Pedagogically innovative but CONTENT-INTENSIVE

**Strengths**:
- ✓ Excellent educational concept (BNCC EF04MA07 math + EF04LP15 Portuguese)
- ✓ Clear keyword teaching strategy (ganhou, perdeu, cada, repartir)
- ✓ Good UI design (4-button operation selection)

**Critical Gaps**:
1. **70-problem content bank required but not created** (CRITICAL) - Only 1 example exists
2. **Keyword effectiveness not validated** (HIGH) - Regional variations (pt-BR dialects) not addressed
3. **Ambiguity elimination process undefined** (HIGH) - How to ensure single correct operation?
4. **Brazilian cultural context vague** (MEDIUM) - Need specific guidelines beyond Real currency

### Blockers - CONTENT-FIRST APPROACH REQUIRED
- [ ] Create 70-problem content bank (20 add, 20 sub, 15 mult, 15 div)
- [ ] Brazilian educator content review (ambiguity, age-appropriateness, cultural sensitivity)
- [ ] Validate keywords with Brazilian students (test regional recognition)
- [ ] Create cultural context guidelines (appropriate scenarios, name diversity, regional sensitivity)
- [ ] Define problem quality checklist (ambiguity elimination, age-appropriate vocabulary)

**Content Creation Timeline**:
1. Write 70 problems (~20 hours)
2. Educator review (~8 hours)
3. Keyword validation (~4 hours)
4. Cultural guidelines (~4 hours)
5. Ambiguity testing (~4 hours)
**Total: ~40 hours content creation BEFORE technical implementation**

**Recommended Status**: ⚠ Changes Required - Cannot implement without complete problem bank and validation. Content quality is paramount.