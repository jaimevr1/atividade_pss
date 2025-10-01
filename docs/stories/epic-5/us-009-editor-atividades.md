# US-009: Editor de Atividades para Professor

**Epic:** EPIC-005 - Editor de Atividades (Professor)
**Prioridade:** Should Have (MVP+)
**Estimativa:** 5 Story Points
**Tipo:** Teacher Tools

## User Story

**As a** teacher
**I want to** create custom activities by combining educational modules
**So that** I can personalize learning experiences for my students' specific needs

## Business Value

Viabiliza a **autonomia pedagógica** do professor e a **personalização do ensino**. Implementa o Princípio P1 "Modularidade Radical" permitindo que professores criem atividades sem depender de desenvolvedores.

## Acceptance Criteria

### AC-1: Activity Creation Interface
**Given** I want to create a new activity
**When** I access the activity editor
**Then** I should see:
- Clear activity creation workflow
- Choice between activity types (Fluência/Integração)
- Module library with descriptions and configurations
- Drag-and-drop or click-to-add interface
- Real-time preview of activity flow

### AC-2: Module Configuration
**Given** I add a module to my activity
**When** I need to configure it
**Then** I should be able to:
- Set module-specific parameters through intuitive forms
- See parameter descriptions and examples
- Validate configurations before saving
- Preview how the module will appear to students

### AC-3: Activity Preview and Testing
**Given** I complete activity configuration
**When** I want to test the activity
**Then** I should be able to:
- Preview the complete student experience
- Test activity flow from start to finish
- Make adjustments based on preview
- Save activity for future use

### AC-4: Activity Management
**Given** I have created activities
**When** I need to manage them
**Then** I should be able to:
- View list of all my created activities
- Edit existing activities (if not yet used by students)
- Duplicate activities for modification
- Archive or delete unused activities

## Technical Requirements

### Activity Editor Architecture
```javascript
const ActivityEditor = {
  // Core editor state
  activityConfig: {
    id: null,
    name: '',
    type: 'fluencia', // 'fluencia' | 'integracao'
    estimatedDuration: 20,
    modules: [],
    targetGrade: [4, 5, 6],
    bnccCodes: [],
    createdBy: 'teacher_id',
    status: 'draft' // 'draft' | 'published' | 'archived'
  },

  // Available modules with configurations
  moduleLibrary: {
    'BateriaRápida': {
      name: 'Bateria Rápida',
      description: 'Prática intensiva de operações matemáticas',
      estimatedDuration: '10-15 min',
      configurableParams: [
        {
          name: 'operationType',
          label: 'Tipo de Operação',
          type: 'select',
          options: ['addition', 'subtraction', 'multiplication', 'division'],
          required: true
        },
        {
          name: 'quantity',
          label: 'Número de Problemas',
          type: 'number',
          min: 10,
          max: 50,
          default: 30
        },
        {
          name: 'numberRange',
          label: 'Alcance Numérico',
          type: 'range',
          min: 1,
          max: 1000,
          default: { min: 10, max: 100 }
        }
      ]
    },

    'IdentificaOperação': {
      name: 'Identifica Operação',
      description: 'Interpretação de problemas verbais',
      estimatedDuration: '5-8 min',
      configurableParams: [
        {
          name: 'problemCount',
          label: 'Número de Problemas',
          type: 'number',
          min: 5,
          max: 20,
          default: 10
        },
        {
          name: 'difficulty',
          label: 'Dificuldade',
          type: 'select',
          options: ['basic', 'intermediate', 'advanced'],
          default: 'basic'
        }
      ]
    }
    // ... other modules
  }
}
```

### Activity Configuration Schema
```json
{
  "activityId": "divisao-pratica-turma-5a",
  "name": "Divisão com Resto - Turma 5A",
  "type": "fluencia",
  "createdBy": "prof-maria-silva",
  "targetGrade": [5],
  "estimatedDuration": 22,
  "bnccCodes": ["EF05MA03"],

  "modules": [
    {
      "type": "EstadoJeremias",
      "order": 1,
      "config": {
        "context": "activity_start",
        "duration": 2
      }
    },
    {
      "type": "BateriaRápida",
      "order": 2,
      "config": {
        "operationType": "division",
        "quantity": 25,
        "numberRange": { "min": 15, "max": 75 },
        "timeLimit": null
      }
    },
    {
      "type": "AutoAvaliação",
      "order": 3,
      "config": {
        "concepts": ["divisão"],
        "scaleType": "4-point"
      }
    }
  ],

  "metadata": {
    "createdAt": "2025-10-01T10:30:00Z",
    "lastModified": "2025-10-01T10:45:00Z",
    "version": "1.0",
    "status": "published",
    "usageCount": 0
  }
}
```

## UI/UX Requirements

### Activity Editor Layout
```
┌─────────────────────────────────────────────────┐
│ 📝 Criar Nova Atividade                        │
├─────────────────────────────────────────────────┤
│ Nome: [Divisão com Resto - Turma 5A        ]   │
│ Tipo: ◉ Fluência  ○ Integração                 │
│ Série: [✓] 4º [✓] 5º [✓] 6º                   │
├─────────────────────────────────────────────────┤
│                                                 │
│ 📚 BIBLIOTECA DE MÓDULOS    │ 🏗️ ATIVIDADE      │
│ ┌─────────────────────────┐ │ ┌─────────────────┐│
│ │ 🔢 Bateria Rápida      │ │ │ 1. Estado       ││
│ │ Prática de operações   │ │ │    Jeremias     ││
│ │ [+ Adicionar]          │ │ │ 2. Bateria      ││
│ │                        │ │ │    Rápida       ││
│ │ 📖 Identifica Operação │ │ │ 3. Auto         ││
│ │ Problemas verbais      │ │ │    Avaliação    ││
│ │ [+ Adicionar]          │ │ │                 ││
│ │                        │ │ │ ⏱️ 22min total   ││
│ │ 🤔 Jeremias Resolver   │ │ │                 ││
│ │ Ensino do personagem   │ │ │ [Preview] [Save]││
│ │ [+ Adicionar]          │ │ └─────────────────┘│
│ └─────────────────────────┘ │                   │
└─────────────────────────────────────────────────┘
```

### Module Configuration Modal
```
┌─────────────────────────────────────┐
│ ⚙️ Configurar: Bateria Rápida      │
├─────────────────────────────────────┤
│                                     │
│ Tipo de Operação: *                │
│ ◉ Divisão  ○ Multiplicação         │
│ ○ Adição   ○ Subtração             │
│                                     │
│ Número de Problemas: [25    ]      │
│ (Entre 10 e 50)                    │
│                                     │
│ Alcance Numérico:                  │
│ Min: [15 ] Max: [75 ]              │
│                                     │
│ Tempo Limite: ○ Sem limite         │
│              ○ [  ] minutos        │
│                                     │
│ 💡 Esta configuração gerará 25     │
│    problemas de divisão com        │
│    números entre 15 e 75.          │
│                                     │
│ [Cancelar]  [Confirmar]             │
└─────────────────────────────────────┘
```

## Template System

### Pre-built Activity Templates
```javascript
const activityTemplates = {
  fluencia_matematica: {
    name: "Fluência Matemática (Modelo)",
    description: "Prática focada em velocidade e precisão",
    estimatedDuration: 20,
    modules: [
      {
        type: "EstadoJeremias",
        config: { context: "activity_start", duration: 2 }
      },
      {
        type: "BateriaRápida",
        config: { quantity: 30, timeLimit: null }
      },
      {
        type: "AutoAvaliação",
        config: { scaleType: "4-point" }
      }
    ]
  },

  integracao_narrativa: {
    name: "Integração com Jeremias (Modelo)",
    description: "Ensino através da técnica Feynman",
    estimatedDuration: 20,
    modules: [
      {
        type: "EstadoJeremias",
        config: { context: "activity_start", duration: 2 }
      },
      {
        type: "IdentificaOperação",
        config: { problemCount: 5, difficulty: "basic" }
      },
      {
        type: "JeremiasResolver",
        config: { concept: "multiplicação", difficulty: "level_2" }
      },
      {
        type: "AutoAvaliação",
        config: { concepts: ["multiplicação"] }
      }
    ]
  }
}
```

### Template Usage Flow
```javascript
const useTemplate = (templateId) => {
  const template = activityTemplates[templateId];

  return {
    ...template,
    id: generateId(),
    name: `${template.name} - ${getCurrentDate()}`,
    createdBy: getCurrentTeacher().id,
    status: 'draft',
    modules: template.modules.map(module => ({
      ...module,
      order: module.order || getNextOrder()
    }))
  };
}
```

## Definition of Done

- [ ] Activity creation workflow functional
- [ ] Module configuration forms working for all MVP modules
- [ ] Drag-and-drop or intuitive module ordering
- [ ] Real-time duration estimation
- [ ] Activity preview system functional
- [ ] Template system with 2+ pre-built templates
- [ ] Activity save/load/edit functionality
- [ ] Input validation and error handling
- [ ] Integration with module runtime system
- [ ] Teacher can successfully create and test activities
- [ ] Performance: editor loads <3 seconds
- [ ] User testing with actual teachers completed

## Integration Points

### With Module Runtime System
```javascript
// Generated activities work with runtime
const executeActivity = (activityConfig, studentId) => {
  const runtime = new ActivityRuntime(activityConfig);

  return runtime.start({
    student: await getStudentData(studentId),
    context: activityConfig.metadata,
    modules: activityConfig.modules
  });
}
```

### With Teacher Dashboard
```javascript
// Activities appear in teacher management
const teacherDashboard = {
  myActivities: await getActivitiesByTeacher(teacherId),
  templates: getAvailableTemplates(),
  analytics: await getActivityUsageStats(teacherId)
}
```

## Validation System

### Configuration Validation
```javascript
const validateActivityConfig = (config) => {
  const errors = [];

  // Basic validation
  if (!config.name?.trim()) {
    errors.push({ field: 'name', message: 'Nome da atividade é obrigatório' });
  }

  if (config.modules.length === 0) {
    errors.push({ field: 'modules', message: 'Atividade deve ter pelo menos um módulo' });
  }

  // Duration validation
  const totalDuration = config.modules.reduce((sum, module) =>
    sum + getModuleDuration(module), 0);

  if (totalDuration > 30) {
    errors.push({
      field: 'duration',
      message: 'Atividade muito longa (máx 30min recomendado)'
    });
  }

  // Module-specific validation
  config.modules.forEach((module, index) => {
    const moduleErrors = validateModuleConfig(module);
    errors.push(...moduleErrors.map(error => ({
      ...error,
      field: `modules[${index}].${error.field}`
    })));
  });

  return errors;
}
```

## Testing Strategy

### Teacher Usability Testing
- **Task completion**: Create activity from scratch
- **Template usage**: Modify existing template
- **Error handling**: Invalid configurations
- **Workflow efficiency**: Time to create typical activity

### Technical Integration Testing
- **Activity execution**: Generated activities run correctly
- **Data persistence**: Save/load reliability
- **Module compatibility**: All modules work in editor
- **Performance**: Large activity configurations

## Risks & Mitigations

| Risco | Prob | Impacto | Mitigação |
|-------|------|---------|-----------|
| Interface muito complexa | Alta | Alto | User testing iterativo, simplificação |
| Configurações inválidas | Média | Médio | Validação robusta, feedback claro |
| Performance com muitos módulos | Baixa | Médio | Lazy loading, otimização |

## Performance Requirements

- **Editor loading**: <3 seconds
- **Module addition**: <500ms
- **Configuration saving**: <1 second
- **Activity preview**: <2 seconds
- **Memory usage**: <100MB for editor session

## Future Enhancements (V2+)

- **Visual drag-and-drop** interface
- **Activity sharing** between teachers
- **Advanced analytics** on activity effectiveness
- **Collaborative editing** for teaching teams
- **Integration with external content** libraries