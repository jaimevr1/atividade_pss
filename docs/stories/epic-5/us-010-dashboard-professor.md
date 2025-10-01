# US-010: Dashboard do Professor

**Epic:** EPIC-005 - Editor de Atividades (Professor)
**Prioridade:** Should Have (MVP+)
**Estimativa:** 4 Story Points
**Tipo:** Teacher Analytics

## User Story

**As a** teacher
**I want to** monitor my class's learning progress and identify students who need support
**So that** I can make informed instructional decisions and provide targeted help

## Business Value

Fornece **insights pedagógicos acionáveis** para o professor, permitindo **intervenção preventiva** e **personalização do ensino**. Transforma dados de aprendizado em **inteligência educacional**.

## Acceptance Criteria

### AC-1: Class Overview Display
**Given** I access my teacher dashboard
**When** I view the class summary
**Then** I should see:
- Total students in my class and how many are active
- Percentage of students who completed this week's activity
- Average time spent on activities
- Overall class performance trends

### AC-2: Individual Student Profiles
**Given** I want to understand individual student progress
**When** I view student details
**Then** I should see:
- Each student's concept mastery progress
- Recent activity completion rates and scores
- Time spent learning vs class average
- Areas where the student is struggling or excelling
- Jeremias relationship progress (level, concepts taught)

### AC-3: Concept Difficulty Analysis
**Given** I need to identify learning challenges
**When** I view concept analytics
**Then** I should see:
- Which BNCC concepts have highest error rates
- Common mistake patterns across the class
- Concepts where students are progressing slowly
- Suggestions for targeted instruction

### AC-4: Engagement and Intervention Alerts
**Given** students may need additional support
**When** the system detects concerning patterns
**Then** I should receive:
- Alerts for students who haven't engaged recently
- Notifications for students showing declining performance
- Suggestions for specific interventions
- Recommendations for activity adjustments

## Technical Requirements

### Dashboard Data Schema
```javascript
const teacherDashboardData = {
  teacher: {
    id: "prof-maria-silva",
    name: "Maria Silva",
    classes: ["5A_manha", "5B_tarde"],
    currentClass: "5A_manha"
  },

  classOverview: {
    totalStudents: 28,
    activeStudents: 26, // logged in this week
    weeklyCompletion: {
      completed: 22,
      inProgress: 4,
      notStarted: 2,
      percentage: 78.6
    },
    averageTimeSpent: 18.5, // minutes
    classPerformance: {
      overall: 0.73, // 73% average across all concepts
      trend: "improving", // "improving", "stable", "declining"
      weekOverWeek: 0.05 // +5% from last week
    }
  },

  studentProfiles: [
    {
      id: "ana-beatriz",
      name: "Ana Beatriz Santos",
      grade: 5,
      engagement: {
        lastActive: "2025-10-01T15:30:00Z",
        totalTimeThisWeek: 45, // minutes
        activitiesCompleted: 3,
        loginStreak: 5 // days
      },
      performance: {
        overallMastery: 0.85,
        conceptProgress: {
          "EF05MA03": { mastery: 0.90, status: "mastered" },
          "EF04MA07": { mastery: 0.80, status: "mastered" },
          "EF05MA08": { mastery: 0.60, status: "practicing" }
        },
        recentScores: [0.82, 0.88, 0.75, 0.90],
        timeToMastery: "below_average" // compared to class
      },
      jeremias: {
        level: 3,
        conceptsTaught: ["multiplicação", "divisão"],
        teachingAccuracy: 0.85
      },
      alerts: [] // no current concerns
    }
    // ... more students
  ],

  conceptAnalytics: {
    "EF05MA03": { // Division with remainder
      classAverage: 0.65,
      studentsStruggling: 8,
      commonErrors: [
        { type: "ignora_resto", frequency: 0.42 },
        { type: "confunde_dividendo_divisor", frequency: 0.28 }
      ],
      averageTimeToMastery: 12.5, // attempts
      recommendation: "Revisar conceito de resto com manipulativos"
    },
    "EF04MA07": { // Multiplication problems
      classAverage: 0.78,
      studentsStruggling: 3,
      commonErrors: [
        { type: "confunde_multiplicacao_adicao", frequency: 0.35 }
      ],
      averageTimeToMastery: 8.2,
      recommendation: "Continuar prática, bom progresso geral"
    }
  },

  alerts: [
    {
      type: "low_engagement",
      student: "carlos-eduardo",
      message: "Carlos não acessa há 4 dias",
      priority: "high",
      suggestedAction: "Conversar individualmente"
    },
    {
      type: "declining_performance",
      student: "daniela-ferreira",
      message: "Performance caiu 15% nas últimas 2 semanas",
      priority: "medium",
      suggestedAction: "Revisar conceitos básicos"
    }
  ]
}
```

### Real-time Analytics Queries
```javascript
const teacherAnalytics = {
  async getClassSummary(teacherId, classId, timeRange = 'week') {
    const students = await getClassStudents(classId);
    const activities = await getStudentActivities(students, timeRange);
    const progress = await getProgressData(students);

    return {
      engagement: calculateEngagementMetrics(activities),
      performance: calculatePerformanceMetrics(progress),
      trends: calculateTrends(progress, timeRange),
      alerts: generateAlerts(students, activities, progress)
    };
  },

  async getConceptDifficulty(classId) {
    const conceptData = await getConceptMasteryByClass(classId);

    return Object.entries(conceptData).map(([concept, data]) => ({
      concept,
      difficulty: calculateDifficultyScore(data),
      commonErrors: identifyCommonErrors(data),
      recommendations: generateRecommendations(data)
    }));
  }
}
```

## UI/UX Requirements

### Dashboard Layout
```
┌─────────────────────────────────────────────────┐
│ 👩‍🏫 Prof. Maria Silva | Turma 5A Manhã          │
├─────────────────────────────────────────────────┤
│                                                 │
│ 📊 VISÃO GERAL DA TURMA                         │
│ ┌─────────────────────────────────────────────┐ │
│ │ 👥 26/28 alunos ativos  📈 73% médio        │ │
│ │ ✅ 22 completaram      ⏱️ 18min médio       │ │
│ │ 🔄 4 em progresso      📊 +5% vs sem. ant.  │ │
│ └─────────────────────────────────────────────┘ │
│                                                 │
│ 🎯 CONCEITOS COM MAIS DIFICULDADE               │
│ ┌─────────────────────────────────────────────┐ │
│ │ 🔢 Divisão com resto     65% • 8 estudantes │ │
│ │ 📐 Interpretação texto   70% • 5 estudantes │ │
│ │ ➕ Frações equivalentes  72% • 4 estudantes │ │
│ └─────────────────────────────────────────────┘ │
│                                                 │
│ 👥 DESEMPENHO POR ALUNO                         │
│ ┌─────────────────────────────────────────────┐ │
│ │ Ana Beatriz    ⭐⭐⭐⭐⭐ 85%  🟢          │ │
│ │ Bruno Costa    ⭐⭐⭐    73%  🟡          │ │
│ │ Carlos Eduardo ⭐⭐      45%  🔴 4 dias   │ │
│ │ Daniela Lima   ⭐⭐⭐⭐   80%  🟡 ↓15%    │ │
│ │ [Ver todos...] ─────────────────────────►   │ │
│ └─────────────────────────────────────────────┘ │
│                                                 │
│ 🚨 ALERTAS E INTERVENÇÕES                       │
│ ┌─────────────────────────────────────────────┐ │
│ │ ⚠️ Carlos não acessa há 4 dias              │ │
│ │ 📉 Daniela com queda de performance         │ │
│ │ 💡 Sugestão: Revisão de resto com turma    │ │
│ └─────────────────────────────────────────────┘ │
│                                                 │
│ ➕ [Nova Atividade] 📊 [Exportar Dados]         │
│                                                 │
└─────────────────────────────────────────────────┘
```

### Individual Student Detail View
```
┌─────────────────────────────────────────────────┐
│ ← Voltar     👤 Ana Beatriz Santos              │
├─────────────────────────────────────────────────┤
│                                                 │
│ 📈 PROGRESSO GERAL                              │
│ Domínio: 85% │ Atividades: 12 │ Tempo: 3.2h    │
│                                                 │
│ 🎯 CONCEITOS BNCC                               │
│ ┌─────────────────────────────────────────────┐ │
│ │ ✅ EF05MA03 - Divisão resto      90% ████   │ │
│ │ ✅ EF04MA07 - Multiplicação      80% ███    │ │
│ │ 🔄 EF05MA08 - Frações            60% ██     │ │
│ │ 🔒 EF06MA24 - Gráficos          Bloqueado   │ │
│ └─────────────────────────────────────────────┘ │
│                                                 │
│ 🤝 RELACIONAMENTO COM JEREMIAS                  │
│ Nível 3 │ Ensinou 5 conceitos │ 85% precisão   │
│                                                 │
│ 📊 ATIVIDADES RECENTES                          │
│ ┌─────────────────────────────────────────────┐ │
│ │ 01/10 Divisão Prática       18min  82% ✅   │ │
│ │ 30/09 Multiplicação Grupos  22min  88% ✅   │ │
│ │ 28/09 Interpretação Texto   15min  75% ✅   │ │
│ └─────────────────────────────────────────────┘ │
│                                                 │
│ 💡 RECOMENDAÇÕES                                │
│ • Continuar com frações - bom progresso        │
│ • Pronta para desbloqueio de gráficos          │
│                                                 │
└─────────────────────────────────────────────────┘
```

## Analytics and Insights Engine

### Performance Calculation Algorithms
```javascript
const calculateClassMetrics = (studentsData) => {
  const metrics = {
    engagement: {
      activeRate: studentsData.filter(s => s.lastActive > sevenDaysAgo).length / studentsData.length,
      averageTimeSpent: studentsData.reduce((sum, s) => sum + s.weeklyTime, 0) / studentsData.length,
      completionRate: studentsData.filter(s => s.weeklyCompletion > 0).length / studentsData.length
    },

    performance: {
      classMastery: studentsData.reduce((sum, s) => sum + s.overallMastery, 0) / studentsData.length,
      conceptSpread: calculateConceptDifficultySpread(studentsData),
      progressTrend: calculateProgressTrend(studentsData)
    },

    alerts: generateClassAlerts(studentsData)
  };

  return metrics;
};

const generateAlerts = (studentsData) => {
  const alerts = [];

  studentsData.forEach(student => {
    // Low engagement alert
    if (daysSinceLastActive(student) > 3) {
      alerts.push({
        type: 'low_engagement',
        student: student.id,
        priority: daysSinceLastActive(student) > 7 ? 'high' : 'medium',
        message: `${student.name} não acessa há ${daysSinceLastActive(student)} dias`
      });
    }

    // Declining performance alert
    const trend = calculatePerformanceTrend(student.recentScores);
    if (trend < -0.1) { // 10% decline
      alerts.push({
        type: 'declining_performance',
        student: student.id,
        priority: 'medium',
        message: `Performance de ${student.name} caiu ${Math.abs(trend * 100)}%`
      });
    }

    // Struggling with concept
    Object.entries(student.conceptProgress).forEach(([concept, progress]) => {
      if (progress.attempts > 20 && progress.mastery < 0.5) {
        alerts.push({
          type: 'concept_struggle',
          student: student.id,
          concept: concept,
          priority: 'high',
          message: `${student.name} tem dificuldade com ${concept}`
        });
      }
    });
  });

  return alerts.sort((a, b) => priorityOrder[a.priority] - priorityOrder[b.priority]);
};
```

### Recommendation Engine
```javascript
const generateRecommendations = (classData, conceptAnalytics) => {
  const recommendations = [];

  // Class-wide concept difficulty
  Object.entries(conceptAnalytics).forEach(([concept, data]) => {
    if (data.studentsStruggling > classData.length * 0.3) { // 30% struggling
      recommendations.push({
        type: 'class_instruction',
        priority: 'high',
        message: `Revisar ${concept} com toda a turma`,
        action: `${data.studentsStruggling} estudantes com dificuldade`,
        suggestion: data.recommendation
      });
    }
  });

  // Individual interventions needed
  const strugglingStudents = classData.filter(s => s.overallMastery < 0.6);
  if (strugglingStudents.length > 0) {
    recommendations.push({
      type: 'individual_support',
      priority: 'medium',
      message: `${strugglingStudents.length} estudantes precisam apoio individual`,
      students: strugglingStudents.map(s => s.name)
    });
  }

  return recommendations;
};
```

## Data Export Functionality

### CSV Export for External Analysis
```javascript
const exportClassData = (classData, format = 'csv') => {
  const exportData = classData.map(student => ({
    'Nome': student.name,
    'Série': student.grade,
    'Último Acesso': formatDate(student.lastActive),
    'Tempo Total (min)': student.totalTime,
    'Atividades Completadas': student.activitiesCompleted,
    'Domínio Geral (%)': Math.round(student.overallMastery * 100),
    'Nível Jeremias': student.jeremias.level,
    'Conceitos Ensinados': student.jeremias.conceptsTaught.length,

    // BNCC concepts
    'EF05MA03 - Divisão (%)': Math.round(student.conceptProgress['EF05MA03']?.mastery * 100 || 0),
    'EF04MA07 - Multiplicação (%)': Math.round(student.conceptProgress['EF04MA07']?.mastery * 100 || 0),
    'EF05MA08 - Frações (%)': Math.round(student.conceptProgress['EF05MA08']?.mastery * 100 || 0),

    // Engagement metrics
    'Dias Sem Acessar': daysSinceLastActive(student),
    'Tendência': student.performanceTrend
  }));

  if (format === 'csv') {
    return convertToCSV(exportData);
  }

  return exportData;
};
```

## Definition of Done

- [ ] Class overview dashboard displays key metrics
- [ ] Individual student profiles show detailed progress
- [ ] Concept difficulty analysis identifies problem areas
- [ ] Alert system notifies about student concerns
- [ ] Real-time data updates reflect current status
- [ ] CSV export functionality works correctly
- [ ] Teacher can navigate between different views easily
- [ ] Performance optimized for classes of 30+ students
- [ ] Integration with authentication and student data
- [ ] User testing with actual teachers completed
- [ ] Documentation for interpreting analytics

## Performance Requirements

- **Dashboard loading**: <5 seconds for class of 30 students
- **Student detail view**: <2 seconds
- **Real-time updates**: <30 seconds latency
- **Data export**: <10 seconds for full class
- **Memory usage**: <200MB for full dashboard session

## Integration Points

### With Student Activity Data
```javascript
// Real-time sync with student progress
const syncStudentData = async (studentId, newProgressData) => {
  await updateStudentProgress(studentId, newProgressData);

  // Update teacher dashboard if affected teacher is online
  const affectedTeachers = await getTeachersForStudent(studentId);
  affectedTeachers.forEach(teacherId => {
    broadcastDashboardUpdate(teacherId, {
      type: 'student_progress_update',
      studentId,
      data: newProgressData
    });
  });
};
```

### With Activity Creator
```javascript
// Integration between dashboard insights and activity creation
const suggestNextActivity = (classAnalytics) => {
  const strugglingConcepts = identifyStrugglingConcepts(classAnalytics);

  return {
    recommendedType: 'fluencia', // based on struggle type
    targetConcepts: strugglingConcepts,
    suggestedModules: ['BateriaRápida', 'JeremiasResolver'],
    estimatedDuration: 20
  };
};
```

## Privacy and Ethics

### Data Privacy Measures
- **Anonymization options** for research/sharing
- **Granular access controls** (only assigned students)
- **Data retention policies** (school year limits)
- **Audit logging** of teacher data access

### Ethical Considerations
- **No punitive implications** from analytics
- **Focus on support**, not judgment
- **Transparent algorithms** - teachers understand how metrics are calculated
- **Student agency preserved** - analytics inform, don't dictate

## Testing Strategy

### Teacher Workflow Testing
- **Daily monitoring routine**: Check class status efficiently
- **Crisis response**: Identify and respond to struggling students
- **Planning integration**: Use insights for next activity creation
- **Data interpretation**: Understand metrics correctly

### Data Accuracy Testing
- **Real-time synchronization**: Dashboard reflects actual student activity
- **Calculation verification**: Metrics match manual calculations
- **Alert reliability**: Notifications trigger appropriately
- **Export integrity**: CSV data matches dashboard display

## Risks & Mitigations

| Risco | Prob | Impacto | Mitigação |
|-------|------|---------|-----------|
| Data overwhelm teachers | Média | Alto | Progressive disclosure, training |
| Privacy concerns | Baixa | Alto | Clear policies, minimal data collection |
| Performance with large classes | Média | Médio | Optimization, pagination |
| Misinterpretation of data | Alta | Médio | Clear labeling, help documentation |

## Future Enhancements (V2+)

- **Predictive analytics** for early intervention
- **Parent communication** tools with progress summaries
- **Cross-class comparisons** and benchmarking
- **Integration with school management systems**
- **Mobile app** for quick monitoring
- **AI-powered insights** and recommendations