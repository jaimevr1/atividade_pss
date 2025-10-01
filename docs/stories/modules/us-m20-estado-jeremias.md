# US-M20: Módulo EstadoJeremias

**Epic:** EPIC-002 - Sistema do Personagem Jeremias
**Prioridade:** Must Have (MVP)
**Estimativa:** 3 Story Points
**Tipo:** Character Interface Module

## User Story

**As a** 4th-6th grade student
**I want to** see Jeremias' current mood and have conversations with him
**So that** I feel connected to our learning journey and understand our relationship

## Business Value

Cria a **interface humana** do sistema de aprendizado, transformando atividades técnicas em **experiência social** de aprendizado. Essencial para **engajamento emocional** e **continuidade narrativa**.

## Acceptance Criteria

### AC-1: Character Display and Presentation
**Given** I encounter Jeremias in any learning context
**When** the EstadoJeremias module loads
**Then** I should see:
- Jeremias' large emoji avatar (96x96px) matching his current mood
- His name "Jeremias" clearly displayed below the avatar
- Contextual dialogue appropriate to the situation
- Smooth entrance animation when he appears

### AC-2: Mood-Appropriate Interactions
**Given** different learning situations occur
**When** Jeremias appears in various contexts
**Then** he should display appropriate moods and messages:
- **Activity start**: 🙂 "Olá! Vamos aprender juntos hoje?"
- **Transition**: 🤔 "O que vamos fazer agora?"
- **Encouragement**: 😊 "Você está indo muito bem!"
- **Gratitude**: 🙏 "Obrigado por me ensinar isso!"

### AC-3: Contextual Dialogue Generation
**Given** Jeremias appears at different points in activities
**When** he needs to communicate with me
**Then** his messages should:
- Reference our shared learning history
- Acknowledge my recent progress or struggles
- Set appropriate expectations for upcoming activities
- Maintain consistent personality and speaking style

### AC-4: Relationship Continuity
**Given** I have multiple sessions with Jeremias
**When** I return to learn with him
**Then** he should:
- Remember our previous interactions
- Reference concepts I've taught him
- Show his growth and development
- Create a sense of ongoing friendship

## Technical Requirements

### Module Configuration Schema
```json
{
  "moduleType": "EstadoJeremias",
  "config": {
    "context": "activity_start", // "activity_start", "transition", "encouragement", "completion"
    "duration": 2, // minutes for interaction
    "allowSkip": false,
    "moodOverride": null, // force specific mood if needed
    "dialogueStyle": "conversational", // "brief", "conversational", "detailed"
    "showProgress": true, // show Jeremias' learning progress
    "enableAnimation": true
  },
  "contextData": {
    "previousActivity": "bateria_rapida_division",
    "nextActivity": "auto_avaliacao",
    "recentTeaching": ["multiplicação"],
    "sessionNumber": 5
  }
}
```

### Dialogue Generation System
```javascript
const generateContextualDialogue = (context, maturityLevel, studentData) => {
  const templates = {
    activity_start: {
      1: "Oi! Estou pronto para aprender. Você vai me ajudar?",
      3: "E aí! Vamos continuar aprendendo juntos?",
      5: "Olá, parceiro! Hoje quero te fazer uma pergunta difícil!"
    },

    transition: {
      1: "O que vamos fazer agora? Estou curioso!",
      3: "Gostei da última atividade. Qual é a próxima?",
      5: "Que tal trocarmos de papel na próxima?"
    },

    encouragement: {
      1: "Você está me ajudando muito! Obrigado!",
      3: "Estou aprendendo tanto com você!",
      5: "Somos uma boa dupla, não acha?"
    },

    completion: {
      1: "Foi divertido! Quando vamos estudar de novo?",
      3: "Aprendi muito hoje. Até a próxima!",
      5: "Ótima sessão! Mal posso esperar para ensinar você também!"
    }
  };

  return templates[context][maturityLevel];
};
```

### Context-Aware Features
```javascript
const contextualEnhancements = {
  // References to recent activities
  recentActivity: {
    "bateria_rapida": "Gostei de praticar contas com você!",
    "jeremias_resolver": "Obrigado por me ensinar sobre {concept}!",
    "identifica_operacao": "Estou ficando melhor em entender problemas!"
  },

  // Time-based greetings
  timeContext: {
    morning: "Bom dia! Pronto para aprender?",
    afternoon: "Boa tarde! Como você está?",
    firstTime: "Muito prazer! Eu sou o Jeremias!",
    returning: "Que bom te ver de novo!"
  },

  // Progress celebrations
  milestones: {
    levelUp: "Você me ajudou tanto que estou ficando mais esperto!",
    conceptMastery: "Agora entendo {concept} graças a você!",
    longStreak: "Já estudamos juntos {days} dias seguidos!"
  }
}
```

## UI/UX Requirements

### Visual Design
- **Avatar size**: 96x96px emoji, centered
- **Name display**: 24px/700/Atkinson Hyperlegible
- **Dialogue**: 18px/400/Comic Sans MS (character specification)
- **Container**: Soft rounded card with subtle shadow
- **Spacing**: Generous padding for comfortable reading

### Layout Variations

#### Standard Interaction
```
┌─────────────────────────────────┐
│                                 │
│            😊                   │
│         Jeremias                │
│                                 │
│   "Olá! Vamos aprender juntos   │
│         hoje?"                  │
│                                 │
│      [Vamos lá!] ──────►        │
│                                 │
└─────────────────────────────────┘
```

#### Progress Celebration
```
┌─────────────────────────────────┐
│            🌟                   │
│         Jeremias                │
│                                 │
│   "Parabéns! Você me ensinou    │
│    tanto que cheguei ao nível   │
│             3!"                 │
│                                 │
│    [Que legal!] ──────►         │
│                                 │
└─────────────────────────────────┘
```

#### Brief Transition
```
┌─────────────────────────────────┐
│     😊 Jeremias diz:           │
│   "Agora vamos praticar!"       │
│                                 │
│      [Continuar] ──────►        │
└─────────────────────────────────┘
```

### Animation Requirements
- **Entrance**: Gentle slide-in from top (300ms)
- **Mood change**: Bounce effect on emoji (500ms)
- **Dialogue**: Typewriter effect for longer messages
- **Exit**: Fade out with slight scale (200ms)

## Character Personality Guidelines

### Core Personality Traits
- **Curious**: Always eager to learn and explore
- **Humble**: Acknowledges mistakes without shame
- **Grateful**: Genuinely appreciates help and teaching
- **Encouraging**: Supportive of student's efforts
- **Authentic**: Feels real, not scripted

### Language Style
- **Age-appropriate**: Uses vocabulary familiar to 9-12 year olds
- **Brazilian Portuguese**: Natural expressions and cultural context
- **Conversational**: Friendly, not formal or academic
- **Consistent**: Maintains same voice across all interactions
- **Positive**: Growth mindset, optimistic outlook

### Speaking Patterns
```javascript
const speechPatterns = {
  // Casual expressions
  casual: ["tipo assim", "cara", "nossa", "massa"],

  // Emotional expressions
  excitement: ["Nossa!", "Que legal!", "Incrível!"],
  confusion: ["Hmm...", "Não entendi bem...", "Deixa eu pensar..."],
  gratitude: ["Valeu!", "Muito obrigado!", "Você é demais!"],

  // Learning expressions
  understanding: ["Ah, entendi!", "Faz sentido!", "Agora sei!"],
  questioning: ["Será que...?", "E se...?", "Como é que...?"]
}
```

## Definition of Done

- [ ] Context-aware dialogue generation working
- [ ] All 6 mood states properly displayed
- [ ] Smooth animations and transitions
- [ ] Comic Sans font loading with fallbacks
- [ ] Integration with character state system
- [ ] Responsive design for desktop (1024px+)
- [ ] Accessibility: screen reader support, keyboard navigation
- [ ] Character personality consistency validated
- [ ] Performance: <200ms for dialogue generation
- [ ] User testing shows positive emotional connection
- [ ] Integration with all activity transition points
- [ ] Database queries optimized for character state

## Integration Points

### Activity Flow Integration
```javascript
const activityIntegration = {
  // Beginning of activities
  activityStart: {
    trigger: "activity_load",
    duration: "2_minutes",
    purpose: "set_context_and_mood"
  },

  // Between modules
  moduleTransition: {
    trigger: "module_complete",
    duration: "30_seconds",
    purpose: "maintain_continuity"
  },

  // End of sessions
  sessionComplete: {
    trigger: "activity_end",
    duration: "1_minute",
    purpose: "closure_and_encouragement"
  }
}
```

### Data Integration
```javascript
// Reads from multiple sources
const characterDataSources = {
  jeremias_state: "maturity_level, mood, dialogue_history",
  student_progress: "recent_activities, performance_trends",
  concept_mastery: "concepts_learned, teaching_successes",
  session_data: "time_of_day, session_count, duration"
}
```

## Testing Strategy

### Emotional Engagement Testing
- Student surveys about relationship with Jeremias
- Observation of facial expressions during interactions
- Completion rates when Jeremias is present vs absent
- Long-term retention of students in program

### Dialogue Quality Testing
- Age-appropriateness review with target users
- Cultural context validation with Brazilian educators
- Personality consistency across different contexts
- Response to various student learning scenarios

## Performance Requirements

- **Dialogue generation**: <100ms
- **Animation smoothness**: 60fps
- **Character state retrieval**: <200ms
- **Context analysis**: <50ms
- **Memory usage**: <5MB per character instance

## Risks & Mitigations

| Risco | Prob | Impacto | Mitigação |
|-------|------|---------|-----------|
| Character feels artificial | Média | Alto | Natural dialogue, user testing |
| Dialogue becomes repetitive | Alta | Médio | Large dialogue bank, variation |
| Cultural inappropriateness | Baixa | Alto | Brazilian educator review |
| Technical performance issues | Baixa | Médio | Optimization, caching |

## Content Requirements

### Dialogue Bank Development
- **200+ unique dialogue pieces** for various contexts
- **Cultural review** by Brazilian Portuguese speakers
- **Age-appropriateness validation** with 9-12 year olds
- **Personality consistency** across all interactions
- **Emotional range** covering all learning situations

### Context Variations
- **Different times of day** (morning, afternoon)
- **Different session types** (first time, returning, celebration)
- **Different student performance** (struggling, succeeding, mixed)
- **Different learning contexts** (practice, teaching, reflection)

## Accessibility Considerations

- **Screen reader compatibility**: Alt text for emojis
- **Keyboard navigation**: All interactions accessible
- **High contrast mode**: Text readable in all themes
- **Font preferences**: Works with both font options
- **Cognitive accessibility**: Clear, simple language