# US-006: Dashboard do Aluno

**Epic:** EPIC-003 - Plataforma Base e Dashboard
**Prioridade:** Must Have (MVP)
**Estimativa:** 4 Story Points
**Tipo:** Core Interface

## User Story

**As a** 4th-6th grade student
**I want to** see my learning progress clearly and know what to do next
**So that** I feel motivated and understand my educational journey

## Business Value

Implementa o **Princípio P5** da PRD: "Transparência de Progresso". Essencial para **autonomia do aluno** e **motivação sustentada** no modelo de aprendizado autodirigido supervisionado.

## Acceptance Criteria

### AC-1: Progress Overview Display
**Given** I log into my dashboard
**When** I view my progress section
**Then** I should see:
- My overall concept mastery clearly displayed (e.g., "7/10 conceitos dominados")
- Visual progress indicators that are easy to understand
- Recent achievements and milestones
- Clear indication of areas that need more practice

### AC-2: Current Activity Prominence
**Given** there's an activity assigned for this week
**When** I view my dashboard
**Then** I should see:
- This week's activity prominently featured
- Activity type clearly labeled (Fluência/Integração)
- Estimated duration displayed
- Clear "Start" button to begin immediately
- Any prerequisites or preparation notes

### AC-3: Jeremias Relationship Status
**Given** I have been working with Jeremias
**When** I view my dashboard
**Then** I should see:
- Jeremias current level and mood
- Brief indication of concepts I've taught him
- Our learning streak or relationship milestones
- Encouraging message about our progress together

### AC-4: Learning History and Next Steps
**Given** I have completed previous activities
**When** I scroll through my dashboard
**Then** I should see:
- List of recently completed activities with scores
- Clear indication of what's coming next
- Suggestions for review or extra practice
- Sense of continuous learning journey

## Technical Requirements

### Dashboard Data Schema
```javascript
const dashboardData = {
  student: {
    id: "uuid",
    name: "Ana Beatriz Santos",
    grade: 5,
    preferences: {
      font: "atkinson",
      theme: "light"
    }
  },

  progress: {
    conceptsMastered: 7,
    totalConcepts: 10,
    currentStreak: 5, // days
    activitiesCompleted: 12,
    totalTimeSpent: 8400, // seconds
    bnccProgress: {
      "EF05MA03": { mastery: 0.85, status: "mastered" },
      "EF04MA07": { mastery: 0.65, status: "practicing" },
      // ... other concepts
    }
  },

  currentActivity: {
    id: "fluencia-divisao-semana-3",
    name: "Divisão com Resto - Prática",
    type: "fluencia",
    estimatedDuration: 20,
    status: "available", // "available", "in_progress", "completed"
    prerequisitesMet: true
  },

  jeremias: {
    maturityLevel: 3,
    currentMood: "confident",
    conceptsTaught: ["multiplicação", "divisão"],
    relationship: {
      daysTogther: 15,
      teachingSuccesses: 8,
      lastInteraction: "2025-10-01T14:30:00Z"
    }
  },

  recentActivities: [
    {
      id: "uuid",
      name: "Multiplicação - Grupos",
      completedAt: "2025-09-30T15:45:00Z",
      score: 0.82,
      timeSpent: 1080,
      type: "integracao"
    }
    // ... more activities
  ]
}
```

### Real-time Data Updates
```javascript
// Dashboard state management
const useDashboardData = () => {
  const [data, setData] = useState(null);
  const [loading, setLoading] = useState(true);

  useEffect(() => {
    // Subscribe to real-time updates
    const subscription = supabase
      .from('student_progress')
      .on('*', payload => {
        updateDashboardData(payload);
      })
      .subscribe();

    return () => subscription.unsubscribe();
  }, []);

  return { data, loading, refresh: fetchDashboardData };
}
```

## UI/UX Requirements

### Visual Design Layout
```
┌─────────────────────────────────────────┐
│ 👋 Olá, Ana Beatriz!        ⚙️ Config  │
├─────────────────────────────────────────┤
│                                         │
│ 📊 MEU PROGRESSO                        │
│ ┌─────────────────────────────────────┐ │
│ │ ████████████████░░░░  7/10          │ │
│ │ Conceitos dominados                 │ │
│ │                                     │ │
│ │ 🔥 5 dias seguidos                  │ │
│ │ 📚 12 atividades completadas        │ │
│ └─────────────────────────────────────┘ │
│                                         │
│ 🎯 ATIVIDADE DESTA SEMANA               │
│ ┌─────────────────────────────────────┐ │
│ │ Divisão com Resto - Prática         │ │
│ │ 📖 Tipo: Fluência | ⏱️ 20min        │ │
│ │                                     │ │
│ │ [▶️ Começar Agora] ────────────►     │ │
│ └─────────────────────────────────────┘ │
│                                         │
│ 🤝 JEREMIAS                             │
│ ┌─────────────────────────────────────┐ │
│ │ 😊 Nível 3 - "Em Progresso"         │ │
│ │ Conceitos ensinados: 3              │ │
│ │ "Estou ficando bom nisso!"          │ │
│ └─────────────────────────────────────┘ │
│                                         │
│ 📚 ATIVIDADES RECENTES                  │
│ ┌─────────────────────────────────────┐ │
│ │ ✅ Multiplicação - Grupos  82%      │ │
│ │ ✅ Interpretação Texto     75%      │ │
│ │ ✅ Adição Reagrupamento    90%      │ │
│ └─────────────────────────────────────┘ │
│                                         │
└─────────────────────────────────────────┘
```

### Progress Visualization
- **Concept mastery**: Horizontal progress bars with clear labeling
- **Activity completion**: Visual checkmarks and percentage scores
- **Jeremias relationship**: Avatar + level + encouraging quote
- **Streaks and achievements**: Icons with numbers (🔥 5 days)

### Interactive Elements
- **Large start button** for current activity (48px+ height)
- **Settings access** for font/theme preferences
- **Activity history** with quick access to review
- **Help/tutorial** easily accessible

## Responsive Design Requirements

### Desktop Layout (1024px+)
- **Three-column layout** maximizing screen space
- **Card-based design** with clear visual hierarchy
- **Adequate spacing** for comfortable reading
- **Consistent navigation** patterns

### Accessibility Features
- **Keyboard navigation** throughout interface
- **Screen reader support** with proper ARIA labels
- **High contrast mode** compatibility
- **Font scaling** support (Atkinson ↔ OpenDyslexic)

## Performance Requirements

- **Initial load**: <3 seconds from login to dashboard
- **Data refresh**: <1 second for progress updates
- **Smooth animations**: 60fps for transitions
- **Memory usage**: <50MB for dashboard session
- **Offline resilience**: Graceful degradation if network slow

## Definition of Done

- [ ] Dashboard loads student data reliably
- [ ] Progress visualization clear and motivating
- [ ] Current activity prominently displayed
- [ ] Jeremias relationship status integrated
- [ ] Activity history accessible and useful
- [ ] Real-time data updates working
- [ ] Performance requirements met (<3s load)
- [ ] Responsive design for desktop (1024px+)
- [ ] Accessibility compliance (WCAG AA)
- [ ] Integration with all core systems
- [ ] User testing shows positive engagement
- [ ] Error handling for network issues

## Educational Psychology Considerations

### Motivation Design
- **Progress made visible** to build sense of achievement
- **Next steps clear** to reduce anxiety and confusion
- **Challenges appropriate** to maintain engagement
- **Success celebrated** without over-reward

### Growth Mindset Language
- **"Conceitos em progresso"** instead of "failed"
- **"Continue praticando"** instead of "needs work"
- **Emphasis on effort** and improvement over perfection
- **Learning journey** framed as ongoing adventure

## Integration Points

### With Authentication System
```javascript
// Seamless transition from login
const postLoginFlow = {
  loadDashboard: async (studentId) => {
    const [student, progress, jeremias, activities] = await Promise.all([
      getStudentProfile(studentId),
      getProgressData(studentId),
      getJeremiasState(studentId),
      getRecentActivities(studentId)
    ]);

    return buildDashboard({ student, progress, jeremias, activities });
  }
}
```

### With Activity System
```javascript
// Starting activities from dashboard
const startActivity = async (activityId) => {
  const activityConfig = await getActivityConfig(activityId);
  const studentContext = await getStudentContext();

  // Transition to activity runtime
  navigateToActivity(activityConfig, studentContext);
}
```

### With Teacher Analytics
```javascript
// Anonymous data for teacher insights
const teacherData = {
  classProgress: aggregateProgress(allStudents),
  engagementMetrics: calculateEngagement(),
  strugglingConcepts: identifyDifficulties(),
  timeSpentAnalysis: analyzeTimePatterns()
}
```

## Testing Strategy

### User Experience Testing
- **9-12 year olds**: Can navigate independently
- **First-time users**: Understand interface immediately
- **Returning users**: Feel motivated to continue
- **Different performance levels**: All students feel supported

### Technical Testing
- **Load testing**: 30 students accessing simultaneously
- **Data accuracy**: Progress reflects actual performance
- **Real-time updates**: Changes appear immediately
- **Error scenarios**: Network failures, data corruption

### Accessibility Testing
- **Screen reader**: Full navigation possible
- **Keyboard only**: All functions accessible
- **High contrast**: Interface remains usable
- **Font preferences**: Both options work correctly

## Risks & Mitigations

| Risco | Prob | Impacto | Mitigação |
|-------|------|---------|-----------|
| Dashboard overwhelming for younger students | Média | Médio | Simplificação iterativa, user testing |
| Performance degrada com muitos dados | Baixa | Alto | Paginação, lazy loading, otimização |
| Progress não motiva estudantes | Média | Alto | A/B testing visualização, feedback |

## Analytics & Insights

### Dashboard Engagement Metrics
```javascript
const dashboardAnalytics = {
  timeSpent: "average_session_duration",
  interactionPatterns: "most_clicked_sections",
  motivationIndicators: "return_rate_after_progress_view",
  usabilityMetrics: "task_completion_success_rate"
}
```

### Student Behavior Insights
- **Most engaging sections** for design optimization
- **Drop-off points** for intervention
- **Time patterns** for activity scheduling
- **Progress correlation** with long-term retention

## Future Enhancements (V2+)

- **Personalized learning paths** based on performance
- **Social features** (anonymous peer comparison)
- **Goal setting** and achievement tracking
- **Parent/guardian view** (with permission)
- **Gamification elements** (badges, streaks)
- **AI-powered recommendations** for next activities