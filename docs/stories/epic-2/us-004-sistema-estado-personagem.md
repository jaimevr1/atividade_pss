# US-004: Sistema de Estado do Personagem Jeremias

**Epic:** EPIC-002 - Sistema do Personagem Jeremias
**Prioridade:** Must Have (MVP)
**Estimativa:** 4 Story Points
**Tipo:** Core Character System

## User Story

**As a** student
**I want to** see Jeremias' current emotional state and growth
**So that** I feel connected to our learning relationship and understand his progress

## Business Value

Implementa o **diferencial pedagógico único** do Dimidui: um personagem que evolui através do ensino do aluno. Cria **conexão emocional** e **narrativa continuada** que sustenta engajamento long-term.

## Acceptance Criteria

### AC-1: Emotional State Display
**Given** I interact with Jeremias in any context
**When** his state is displayed
**Then** I should see:
- Large emoji avatar (96x96px) matching his current mood
- His name "Jeremias" clearly displayed
- Dialogue appropriate to his emotional state
- Smooth transitions between moods (500ms bounce animation)

### AC-2: Mood System (6 States)
**Given** different learning contexts occur
**When** Jeremias needs to express emotion
**Then** he should display appropriate moods:
- 🙂 **neutral**: "Pronto para aprender"
- 🤔 **thinking**: "Tentando resolver"
- 😕 **confused**: "Não tenho certeza..."
- 😮 **eureka**: "Ah! Entendi!"
- 😊 **confident**: "Acho que sei fazer isso"
- 🙏 **grateful**: "Obrigado por me ensinar!"

### AC-3: Maturity Evolution (5 Levels)
**Given** I successfully teach Jeremias over time
**When** he reaches maturity milestones
**Then** his dialogue should reflect growth:
- **Level 1**: "Estou começando a aprender isso..."
- **Level 2**: "Já melhorei, mas ainda confundo algumas coisas"
- **Level 3**: "Estou ficando bom nisso!"
- **Level 4**: "Raramente erro agora. Valeu pela ajuda!"
- **Level 5**: "Agora eu ensino você! Cria um problema para EU resolver?"

### AC-4: State Persistence
**Given** I have multiple sessions with Jeremias
**When** I return to the platform
**Then** the system should:
- Remember his current maturity level
- Recall concepts I've taught him
- Reference previous learning experiences
- Maintain relationship continuity

## Technical Requirements

### Character State Schema
```javascript
// jeremias_state table structure
{
  student_id: "uuid",
  maturity_level: 3, // 1-5
  current_mood: "confident", // current emotional state
  last_dialogue: "Estou ficando bom nisso!",

  // Progress tracking
  concepts_taught: ["multiplicação", "divisão", "interpretação_texto"],
  times_taught: 15,
  corrections_made: 11,

  // Timestamps
  last_interaction: "2025-10-01T14:30:00Z",
  level_up_date: "2025-09-28T10:15:00Z",

  // Context awareness
  last_activity_type: "fluencia",
  session_count: 8,
  total_time_together: 7200 // seconds
}
```

### Mood Transition Rules
```javascript
const moodTransitions = {
  // Activity start
  activity_start: {
    from: "neutral",
    to: "thinking",
    trigger: "module_load",
    dialogue: contextualGreeting(maturityLevel, activityType)
  },

  // During problem solving
  problem_attempt: {
    from: "thinking",
    to: "confused",
    trigger: "makes_error",
    dialogue: "Hmm... não tenho certeza se está certo..."
  },

  // Being taught
  correction_received: {
    from: "confused",
    to: "eureka",
    trigger: "student_teaches_correctly",
    dialogue: "Ah! Entendi! Obrigado por me ensinar!"
  },

  // After learning
  teaching_complete: {
    from: "eureka",
    to: "grateful",
    trigger: "lesson_understood",
    dialogue: generateGratitudeMessage(conceptLearned)
  }
}
```

### Animation System
```javascript
// Framer Motion animation configs
const moodAnimations = {
  moodChange: {
    type: "spring",
    duration: 0.5,
    ease: "cubic-bezier(0.68, -0.55, 0.265, 1.55)", // bounce
    scale: [1, 1.1, 1], // subtle bounce
    transition: { duration: 0.5 }
  },

  dialogueAppear: {
    initial: { opacity: 0, y: 10 },
    animate: { opacity: 1, y: 0 },
    transition: { duration: 0.3, ease: "easeOut" }
  },

  avatarPulse: {
    scale: [1, 1.02, 1],
    transition: { duration: 2, repeat: Infinity, repeatType: "loop" }
  }
}
```

## Contextual Dialogue System

### Maturity-Based Responses
```javascript
const dialogueByMaturity = {
  1: { // Beginner
    greeting: "Oi! Você pode me ajudar a aprender?",
    struggle: "Isso é difícil para mim...",
    success: "Consegui! Você me ensinou bem!",
    gratitude: "Muito obrigado por ter paciência comigo!"
  },

  3: { // Progressing
    greeting: "E aí! Vamos aprender juntos?",
    struggle: "Acho que sei, mas não tenho certeza...",
    success: "Estou melhorando mesmo!",
    gratitude: "Aprendo muito quando você me ensina!"
  },

  5: { // Master
    greeting: "Hoje eu quero te desafiar! Preparado?",
    struggle: "Espera, deixa eu pensar melhor...",
    success: "Agora posso ajudar outros também!",
    gratitude: "Obrigado por me ensinar tanto! Agora somos parceiros!"
  }
}
```

### Context-Aware Messaging
- **First meeting**: Introduction and explanation of learning together
- **Returning student**: Reference to previous sessions and growth
- **After long absence**: Gentle re-engagement
- **Celebration milestones**: Level-up recognition
- **Struggling periods**: Extra encouragement and support

## UI/UX Requirements

### Visual Design
- **Avatar**: 96x96px emoji, centered prominently
- **Name**: 24px/700/Atkinson Hyperlegible below avatar
- **Dialogue**: 18px/400/Comic Sans MS (per PRD character specification)
- **Container**: Rounded card with soft shadow

### Layout Structure
```
┌─────────────────────────────────┐
│                                 │
│            😊                   │
│         Jeremias                │
│                                 │
│   "Estou ficando bom nisso!"    │
│                                 │
│      [Continuar] ──────►        │
│                                 │
└─────────────────────────────────┘
```

### Accessibility Features
- Alt text for emoji avatars
- Screen reader descriptions of mood changes
- Keyboard navigation support
- High contrast mode compatibility

## Definition of Done

- [ ] 6 emotional states fully implemented
- [ ] 5 maturity levels with appropriate dialogues
- [ ] Smooth mood transition animations
- [ ] Persistent state across sessions
- [ ] Context-aware dialogue generation
- [ ] Comic Sans font loading with fallbacks
- [ ] Database integration for state persistence
- [ ] Performance: state transitions <200ms
- [ ] Accessibility compliance (WCAG AA)
- [ ] Unit tests for state logic
- [ ] Integration tests with character interactions
- [ ] User testing shows positive emotional connection

## Performance Requirements

- State retrieval: <100ms
- Mood transitions: <200ms
- Dialogue generation: <50ms
- Animation smoothness: 60fps
- Memory usage: <10MB for character system

## Integration Points

### Module Integration
- **M1 BateriaRápida**: Encouragement during practice
- **M5 IdentificaOperação**: Thinking together through problems
- **M10 JeremiasResolver**: Core teaching interactions
- **M14 AutoAvaliação**: Shared reflection on learning

### Data Analytics
```javascript
// Character engagement metrics
{
  student_id: "uuid",
  jeremias_engagement: {
    interaction_frequency: "daily",
    emotional_response: "positive",
    teaching_success_rate: 0.85,
    relationship_duration: 28, // days
    favorite_interactions: ["teaching", "celebration"]
  }
}
```

## Testing Strategy

### Emotional Impact Testing
- Student interviews about relationship with Jeremias
- Observation of engagement during character interactions
- Long-term retention tracking
- Comparison with/without character system

### Technical Testing
- State persistence across sessions
- Mood transition reliability
- Performance under load
- Cross-browser emoji rendering

## Risks & Mitigations

| Risco | Prob | Impacto | Mitigação |
|-------|------|---------|-----------|
| Character feels fake/annoying | Média | Alto | User testing, authentic dialogue |
| Technical complexity too high | Baixa | Médio | Incremental development, MVP approach |
| Cultural appropriateness issues | Baixa | Alto | Brazilian educator review |

## Content Guidelines

### Personality Traits
- **Curious and eager to learn**
- **Humble about mistakes**
- **Genuinely grateful for help**
- **Uses age-appropriate Brazilian Portuguese**
- **Shows growth mindset consistently**

### Dialogue Principles
- Natural, conversational tone
- Avoid overly formal language
- Include subtle expressions ("tipo", "cara")
- Show vulnerability appropriately
- Express genuine emotion