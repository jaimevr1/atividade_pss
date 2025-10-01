# US-007: Sistema de Progressão Conceitual

**Epic:** EPIC-003 - Plataforma Base e Dashboard
**Prioridade:** Must Have (MVP)
**Estimativa:** 5 Story Points
**Tipo:** Core Academic System

## User Story

**As a** educational system
**I want to** track student progress against BNCC standards and unlock content appropriately
**So that** students receive challenges matched to their skill level and meet educational objectives

## Business Value

Implementa o **coração pedagógico** do Dimidui: progressão baseada em evidências que garante **domínio conceitual** antes de avançar. Alinha com **10 habilidades BNCC prioritárias** e implementa **Princípio P6** (Progressão Desbloqueável).

## Acceptance Criteria

### AC-1: BNCC Concept Mapping
**Given** student activities generate performance data
**When** the system processes learning interactions
**Then** it should:
- Map each interaction to specific BNCC competencies
- Track attempts, successes, and error patterns per concept
- Calculate mastery levels using evidence-based thresholds
- Update concept status in real-time

### AC-2: Mastery Threshold Calculation
**Given** a student practices a specific BNCC concept
**When** they accumulate sufficient practice data
**Then** the system should:
- Require minimum 15 attempts for threshold calculation
- Apply 80% accuracy threshold for mastery designation
- Weight recent performance more heavily than older data
- Account for different problem difficulty levels

### AC-3: Content Unlocking Logic
**Given** a student achieves mastery in prerequisite concepts
**When** the system evaluates their readiness
**Then** it should:
- Unlock advanced content automatically
- Prevent access to content without prerequisites
- Provide clear feedback about why content is locked
- Suggest specific practice to unlock desired content

### AC-4: Progress Transparency
**Given** a student or teacher views progress data
**When** they access the progress interface
**Then** they should see:
- Clear visual representation of concept mastery
- Specific BNCC codes and descriptions
- Progress toward unlocking new content
- Recommendations for continued learning

## Technical Requirements

### BNCC Concept Database Schema
```sql
-- Core BNCC competencies for MVP
CREATE TABLE bncc_concepts (
  id UUID PRIMARY KEY,
  code TEXT UNIQUE NOT NULL, -- e.g., "EF04MA07"
  title TEXT NOT NULL,
  description TEXT,
  subject TEXT, -- "mathematics", "portuguese"
  grade_level INTEGER,
  prerequisites TEXT[], -- array of prerequisite concept IDs
  estimated_hours DECIMAL
);

-- Student progress tracking
CREATE TABLE concept_mastery (
  id UUID PRIMARY KEY,
  student_id UUID REFERENCES students(id),
  bncc_code TEXT REFERENCES bncc_concepts(code),

  -- Performance metrics
  total_attempts INTEGER DEFAULT 0,
  correct_attempts INTEGER DEFAULT 0,
  mastery_level DECIMAL DEFAULT 0.0, -- 0.0 to 1.0

  -- Tracking
  first_attempt TIMESTAMP,
  last_attempt TIMESTAMP,
  mastery_achieved_at TIMESTAMP,

  -- Analytics
  time_to_mastery INTEGER, -- minutes from first to mastery
  error_patterns JSONB,
  difficulty_progression JSONB,

  UNIQUE(student_id, bncc_code)
);
```

### Mastery Calculation Algorithm
```javascript
const calculateMastery = (attempts, correct, recentPerformance) => {
  // Minimum attempts required
  if (attempts < 15) {
    return {
      mastery: (correct / attempts),
      status: "insufficient_data",
      needsMore: 15 - attempts
    };
  }

  // Base accuracy rate
  const baseAccuracy = correct / attempts;

  // Weight recent performance (last 10 attempts)
  const recentWeight = 0.7;
  const historicalWeight = 0.3;

  const recentAccuracy = recentPerformance.reduce((sum, result) =>
    sum + (result ? 1 : 0), 0) / recentPerformance.length;

  const weightedMastery = (recentAccuracy * recentWeight) +
                         (baseAccuracy * historicalWeight);

  // Determine status
  const threshold = 0.80;
  const status = weightedMastery >= threshold ? "mastered" : "practicing";

  return {
    mastery: weightedMastery,
    status,
    threshold,
    confidence: calculateConfidence(attempts, weightedMastery)
  };
};
```

### Dependency Graph System
```javascript
const conceptDependencies = {
  "EF05MA03": { // Division with remainder
    prerequisites: ["EF04MA07"], // Multiplication problems
    unlocks: ["EF06MA11"], // Division algorithms
    estimatedHours: 8
  },

  "EF04MA07": { // Multiplication problems
    prerequisites: ["EF04MA02"], // Number order
    unlocks: ["EF05MA03", "EF05MA08"], // Division, fractions
    estimatedHours: 6
  },

  "EF05MA08": { // Fractions
    prerequisites: ["EF04MA07", "EF05MA03"],
    unlocks: ["EF06MA07"], // Fraction operations
    estimatedHours: 10
  }
  // ... more concepts
};

const checkPrerequisites = (studentId, targetConcept) => {
  const prerequisites = conceptDependencies[targetConcept].prerequisites;

  return Promise.all(
    prerequisites.map(async (prereq) => {
      const mastery = await getConceptMastery(studentId, prereq);
      return mastery.status === "mastered";
    })
  ).then(results => results.every(Boolean));
};
```

## BNCC Competencies (MVP Priority)

### Mathematics (6 competencies)
```javascript
const mathConcepts = {
  "EF04MA02": {
    title: "Ordem de Grandeza",
    description: "Ler, escrever e ordenar números até dezena de milhar",
    modules: ["M4_OrdemCrescente", "M2_LinhaNumérica"],
    threshold: 0.80
  },

  "EF04MA07": {
    title: "Problemas de Multiplicação",
    description: "Resolver problemas envolvendo diferentes significados da multiplicação",
    modules: ["M1_BateriaRápida", "M10_JeremiasResolver", "M5_IdentificaOperação"],
    threshold: 0.80
  },

  "EF05MA03": {
    title: "Divisão com Resto",
    description: "Identificar e representar divisão com resto",
    modules: ["M1_BateriaRápida", "M10_JeremiasResolver"],
    threshold: 0.75 // Slightly lower due to complexity
  },

  "EF05MA08": {
    title: "Frações",
    description: "Representar e comparar frações",
    modules: ["M3_FraçãoVisual"],
    threshold: 0.80
  },

  "EF05MA20": {
    title: "Figuras Planas",
    description: "Reconhecer propriedades de triângulos e quadriláteros",
    modules: ["M16_ConstruçãoGeométrica"],
    threshold: 0.80
  },

  "EF06MA24": {
    title: "Gráficos",
    description: "Resolver problemas com dados em gráficos",
    modules: ["M17_GráficoInterativo"],
    threshold: 0.80
  }
};
```

### Portuguese (4 competencies)
```javascript
const portugueseConcepts = {
  "EF04LP04": {
    title: "Ortografia e Concordância",
    description: "Usar regras básicas de concordância nominal e verbal",
    modules: ["M8_CorrigeOrtografia"],
    threshold: 0.75
  },

  "EF04LP15": {
    title: "Localização de Informação",
    description: "Localizar informações explícitas em texto",
    modules: ["M9_CompreensãoLeitora", "M6_DestacaPalavraChave"],
    threshold: 0.80
  },

  "EF05LP05": {
    title: "Ortografia: Acentuação",
    description: "Grafar palavras com regras de acentuação",
    modules: ["M8_CorrigeOrtografia"],
    threshold: 0.75
  },

  "EF05LP15": {
    title: "Interpretação Inferencial",
    description: "Reconhecer relações lógicas entre partes de texto",
    modules: ["M9_CompreensãoLeitora", "M11_CenárioSequencial"],
    threshold: 0.80
  }
};
```

## Progress Visualization System

### Student Interface
```javascript
const ProgressVisualization = ({ studentId }) => {
  const concepts = useBNCCProgress(studentId);

  return (
    <div className="progress-grid">
      {concepts.map(concept => (
        <ConceptCard
          key={concept.code}
          title={concept.title}
          mastery={concept.mastery}
          status={concept.status}
          attempts={concept.attempts}
          unlocked={concept.unlocked}
        />
      ))}
    </div>
  );
};

const ConceptCard = ({ title, mastery, status, attempts, unlocked }) => (
  <div className={`concept-card ${status}`}>
    <h3>{title}</h3>
    <ProgressBar value={mastery} threshold={0.80} />
    <div className="stats">
      <span>{attempts} tentativas</span>
      <span>{Math.round(mastery * 100)}% domínio</span>
    </div>
    {!unlocked && <LockIcon />}
  </div>
);
```

### Teacher Analytics Interface
```javascript
const ClassProgressDashboard = ({ classId }) => {
  const classData = useClassProgress(classId);

  return (
    <div className="class-dashboard">
      <ConceptDifficultyChart concepts={classData.conceptStats} />
      <StudentProgressGrid students={classData.students} />
      <InterventionSuggestions alerts={classData.alerts} />
    </div>
  );
};
```

## Definition of Done

- [ ] BNCC concept database populated with 10 priority competencies
- [ ] Mastery calculation algorithm implemented and tested
- [ ] Dependency graph system working correctly
- [ ] Content unlocking logic functional
- [ ] Real-time progress updates across all interfaces
- [ ] Teacher analytics dashboard provides actionable insights
- [ ] Student progress visualization clear and motivating
- [ ] Performance optimized for 30+ concurrent users
- [ ] Integration with all MVP modules complete
- [ ] Data accuracy validated against manual calculations
- [ ] User testing shows understanding of progress system
- [ ] Documentation for adding new BNCC concepts

## Performance Requirements

- **Progress calculation**: <200ms per student
- **Dashboard loading**: <3 seconds for full class view
- **Real-time updates**: <100ms latency
- **Database queries**: Optimized with proper indexing
- **Concurrent users**: Support 30 students + teachers

## Integration Points

### Module Data Collection
```javascript
// Each module reports progress data
const reportProgress = (moduleType, studentId, interactions) => {
  const bnccMappings = getModuleBNCCMappings(moduleType);

  interactions.forEach(interaction => {
    bnccMappings.forEach(bnccCode => {
      updateConceptMastery(studentId, bnccCode, interaction);
    });
  });

  // Check for new unlocks
  checkAndUnlockContent(studentId);
};
```

### Activity Unlocking
```javascript
// Determines which activities student can access
const getAvailableActivities = async (studentId) => {
  const masteredConcepts = await getMasteredConcepts(studentId);
  const allActivities = await getAllActivities();

  return allActivities.filter(activity => {
    return activity.prerequisites.every(prereq =>
      masteredConcepts.includes(prereq)
    );
  });
};
```

## Testing Strategy

### Algorithm Testing
- **Mastery calculation accuracy** with known datasets
- **Dependency resolution** with complex prerequisite chains
- **Edge cases**: insufficient data, perfect scores, declining performance
- **Performance testing** with large student populations

### Educational Validation
- **Threshold appropriateness** validated with educators
- **Progress visualization** tested with students and teachers
- **Unlocking logic** provides appropriate challenge progression
- **BNCC alignment** verified by curriculum specialists

## Risks & Mitigations

| Risco | Prob | Impacto | Mitigação |
|-------|------|---------|-----------|
| Thresholds too high/low | Média | Alto | A/B testing, educator input, adjustable |
| Performance degrada | Baixa | Alto | Indexing, caching, query optimization |
| BNCC alignment incorreta | Baixa | Alto | Curriculum expert review, validation |

## Analytics & Insights

### System-Level Metrics
```javascript
const systemAnalytics = {
  conceptDifficulty: "average_attempts_to_mastery_by_concept",
  progressionPatterns: "common_learning_paths_through_concepts",
  unlockingEffectiveness: "engagement_after_content_unlock",
  thresholdAppropriateness: "false_positive_mastery_rate"
}
```

### Educational Research Data
- **Learning trajectory patterns** for curriculum improvement
- **Concept difficulty ranking** for content adjustment
- **Time-to-mastery distributions** for planning
- **Prerequisite relationship validation** through data

## Future Enhancements (V2+)

- **Adaptive thresholds** based on individual learning patterns
- **Micro-competency tracking** within BNCC concepts
- **Learning path optimization** using ML
- **Cross-curricular concept connections**
- **Parent progress reports** with educational context