# US-M14: Módulo AutoAvaliação

**Epic:** EPIC-001 - Sistema de Módulos Educacionais
**Prioridade:** Must Have (MVP)
**Estimativa:** 2 Story Points
**Tipo:** Educational Module

## User Story

**As a** 4th-6th grade student
**I want to** reflect on my understanding of concepts I just practiced
**So that** I can develop metacognitive awareness and honest self-evaluation skills

## Business Value

Desenvolve **metacognição** e **autorregulação da aprendizagem**, competências fundamentais da BNCC. Fornece dados preciosos sobre **calibração** entre autoavaliação e performance real para professores.

## Acceptance Criteria

### AC-1: Self-Assessment Presentation
**Given** I complete a practice session
**When** I enter the AutoAvaliação module
**Then** I should see:
- Clear question about my understanding in friendly language
- 3-4 assessment options with emojis and simple text
- Encouraging message: "Não há resposta errada, seja honesto!"
- Visual scale easy to understand for my age

### AC-2: Response Collection
**Given** I'm presented with self-assessment options
**When** I select my level of understanding
**Then** I should:
- Receive immediate positive acknowledgment
- See encouraging message appropriate to my selection
- Feel safe to be honest about my struggles
- Progress smoothly to activity completion

### AC-3: Metacognitive Development
**Given** I use the module regularly over time
**When** I compare my self-assessments with actual performance
**Then** the system should:
- Track my calibration improvement (silent)
- Use data to suggest appropriate challenges
- Help me recognize my learning patterns
- Build my self-awareness gradually

## Technical Requirements

### Module Configuration Schema
```json
{
  "moduleType": "AutoAvaliação",
  "config": {
    "concepts": ["divisão", "interpretação_texto", "multiplicação"],
    "scaleType": "4-point", // "3-point", "4-point", "5-point"
    "questionTemplate": "Como você se sente sobre {concept} agora?",
    "showOverallQuestion": true,
    "collectConfidence": true,
    "options": [
      {
        "value": 1,
        "label": "Preciso praticar mais",
        "emoji": "😅",
        "color": "#FED7B8", // warm, non-judgmental
        "response": "Está tudo bem! Praticar mais vai te ajudar."
      },
      {
        "value": 2,
        "label": "Estou melhorando",
        "emoji": "🙂",
        "color": "#B7E4C7",
        "response": "Ótimo! Você está no caminho certo."
      },
      {
        "value": 3,
        "label": "Já domino",
        "emoji": "😊",
        "color": "#A8DADC",
        "response": "Parabéns! Você está indo muito bem."
      },
      {
        "value": 4,
        "label": "Posso ensinar outros",
        "emoji": "🌟",
        "color": "#F1FAEE",
        "response": "Incrível! Ensinar outros é uma ótima forma de aprender ainda mais."
      }
    ]
  }
}
```

### Data Collection Structure
```javascript
// Stored in student_progress.interactions
{
  type: "self_assessment",
  timestamp: "2025-10-01T14:30:00Z",
  moduleContext: "after_bateria_rapida_division",
  assessments: [
    {
      concept: "divisão",
      selfRating: 2, // "Estou melhorando"
      actualPerformance: 0.75, // 75% accuracy
      discrepancy: -0.25, // slightly overestimated
      confidence: 0.8 // how confident in self-rating
    }
  ],
  overallSession: {
    selfRating: 3,
    timeSpent: 180, // seconds on self-reflection
    emotionalState: "positive" // derived from responses
  }
}
```

## Psychological Design Principles

### Building Trust and Safety
- **No wrong answers** emphasized repeatedly
- Warm, non-judgmental color palette
- Growth mindset language throughout
- Private responses (not shared with peers)

### Encouraging Honesty
- "Seja honesto sobre como se sente"
- Validate all responses positively
- Show that struggling is normal and valuable
- Model self-reflection as strength

### Developing Calibration
- Track patterns over time (teacher view only)
- Gentle guidance through activity selection
- Help students notice their own accuracy
- Build confidence in self-knowledge

## UI/UX Requirements

### Visual Design
- Large, friendly buttons with emojis (48px+ height)
- Soft, warm colors (no harsh red/green judgments)
- Question text: 20px/700/Atkinson Hyperlegible
- Response buttons: Clear typography, adequate spacing

### Layout Example
```
┌─────────────────────────────────────┐
│ Como você se sente sobre divisão    │
│ agora?                              │
│                                     │
│ 😅 [Preciso praticar mais]          │
│                                     │
│ 🙂 [Estou melhorando]               │
│                                     │
│ 😊 [Já domino]                      │
│                                     │
│ 🌟 [Posso ensinar outros]           │
│                                     │
│ 💭 Não há resposta errada!          │
│    Seja honesto sobre como se sente │
└─────────────────────────────────────┘
```

### Response Animations
- Button hover: Gentle lift effect
- Selection: Soft glow around chosen option
- Feedback: Slide-in from bottom with encouraging message

## Educational Outcomes

### Primary Goals
- **Self-awareness**: Students understand their own learning
- **Honesty**: Safe space for authentic self-reflection
- **Growth mindset**: Struggle seen as opportunity
- **Metacognition**: Thinking about thinking

### Secondary Benefits
- Teacher insights into student confidence
- Data for adaptive instruction
- Reduced anxiety about assessment
- Increased ownership of learning

## Definition of Done

- [ ] Simple, age-appropriate interface
- [ ] Multiple concepts assessed in one session
- [ ] Positive responses for all selections
- [ ] Data collection for teacher analytics
- [ ] Integration with activity completion flow
- [ ] Accessibility compliance (screen readers, keyboard)
- [ ] Emoji rendering consistent across browsers
- [ ] Privacy protection (student responses confidential)
- [ ] Unit tests for response logic
- [ ] User testing shows positive emotional impact

## Integration Points

### Teacher Dashboard Data
```javascript
// Aggregated for teacher insights
{
  student: "Ana Silva",
  calibrationTrend: "improving", // overestimate -> accurate
  concepts: {
    "divisão": {
      selfAssessment: 2.8, // average over time
      actualPerformance: 0.82,
      calibration: "well_calibrated"
    }
  },
  recommendations: [
    "Ana is accurately assessing her division skills",
    "Consider challenging her with more complex problems"
  ]
}
```

### Activity Flow Integration
- Appears at end of practice sessions
- Takes 1-3 minutes maximum
- Seamless transition to completion
- Data immediately available for next session planning

## Testing Strategy

### Emotional Impact Testing
- Survey students: "How did self-assessment make you feel?"
- Observe engagement vs avoidance behaviors
- A/B test different response message styles
- Long-term tracking of honesty vs performance

### Calibration Development Testing
- Track accuracy of self-assessment over time
- Compare students with/without self-assessment
- Measure impact on learning outcomes
- Teacher feedback on data usefulness

## Risks & Mitigations

| Risco | Prob | Impacto | Mitigação |
|-------|------|---------|-----------|
| Students give "safe" answers | Alta | Médio | Build trust, emphasize honesty value |
| Self-assessment takes too long | Média | Baixo | Keep to 2-3 questions maximum |
| Teachers misuse calibration data | Baixa | Alto | Training, privacy guidelines |

## Content Guidelines

### Question Phrasing
- "Como você se sente sobre..."
- "O que você acha do seu progresso em..."
- "Quão confiante você está com..."

### Response Messages
- Always encouraging and supportive
- Acknowledge effort over ability
- Use growth mindset language
- Keep responses brief (1-2 sentences)

### Cultural Considerations
- Brazilian educational context
- Age-appropriate emotional vocabulary
- Respect for diverse learning styles
- Inclusive language choices

## Future Enhancements (V2+)
- Adaptive questioning based on performance
- Emotional state detection and response
- Peer comparison (anonymous, opt-in)
- Goal-setting integration
- Parent/guardian insights (with permission)