# US-002: Sistema de Atividades Compostas

**Epic:** EPIC-001 - Sistema de Módulos Educacionais
**Prioridade:** Must Have (MVP)
**Estimativa:** 3 Story Points
**Tipo:** Core System

## User Story

**As a** teacher
**I want to** combine multiple modules into a single coherent activity
**So that** I can create pedagogically meaningful learning experiences

## Business Value

Permite a criação de atividades com **progressão pedagógica intencional** (ex: EstadoJeremias → BateriaRápida → AutoAvaliação), essencial para o modelo de ensino estruturado do Dimidui.

## Acceptance Criteria

### AC-1: Activity Composition
**Given** multiple module configurations
**When** I create a composite activity
**Then** the system should:
- Sequence modules in the specified order
- Validate module compatibility
- Calculate total estimated duration
- Preview the complete activity flow

### AC-2: Inter-Module Data Flow
**Given** modules that need to share data
**When** one module completes
**Then** the system should:
- Pass relevant data to the next module
- Maintain context across transitions
- Preserve student responses for analytics
- Handle data transformation if needed

### AC-3: Progress Tracking
**Given** a multi-module activity in progress
**When** student completes individual modules
**Then** the system should:
- Show progress indicator (3/5 modules complete)
- Save intermediate progress
- Allow resumption if interrupted
- Track time spent per module

## Technical Requirements

### Activity Configuration Schema
```json
{
  "activityId": "fluencia-divisao-semana-1",
  "name": "Prática de Divisão - Semana 1",
  "type": "fluencia",
  "estimatedDuration": 20,
  "modules": [
    {
      "moduleType": "EstadoJeremias",
      "config": { "context": "start", "duration": 2 },
      "order": 1
    },
    {
      "moduleType": "BateriaRápida",
      "config": { "operationType": "division", "quantity": 30 },
      "order": 2
    },
    {
      "moduleType": "AutoAvaliação",
      "config": { "concepts": ["divisão"] },
      "order": 3
    }
  ],
  "dataFlow": [
    {
      "from": "BateriaRápida",
      "to": "AutoAvaliação",
      "fields": ["score", "conceptsAttempted"]
    }
  ]
}
```

### System Components
- **ActivityOrchestrator**: Main flow control
- **ModuleSequencer**: Order and transitions
- **DataPipeline**: Inter-module communication
- **ProgressTracker**: State management

## Definition of Done

- [ ] Activity composition from multiple modules working
- [ ] Data flows between compatible modules
- [ ] Progress tracking across full activity
- [ ] Interruption/resumption handling
- [ ] Duration estimation accurate (±10%)
- [ ] Unit tests for orchestration logic
- [ ] Integration tests for sample activities
- [ ] Teacher can preview composed activities
- [ ] Student experience smooth across transitions

## Dependencies

- US-001: Sistema Runtime de Módulos (must be complete)
- Module state management system
- Progress persistence (Supabase)

## Technical Architecture

### State Management
```javascript
// Activity state structure
{
  activityId: "fluencia-divisao-semana-1",
  studentId: "uuid",
  status: "in_progress", // not_started, in_progress, completed
  currentModule: 2,
  modules: [
    { id: 1, status: "completed", data: {...} },
    { id: 2, status: "in_progress", data: {...} },
    { id: 3, status: "not_started", data: null }
  ],
  startedAt: "2025-10-01T14:00:00Z",
  estimatedEndAt: "2025-10-01T14:20:00Z"
}
```

### Data Flow Pipeline
- **Input validation**: Ensure data compatibility
- **Transformation**: Convert data formats if needed
- **Persistence**: Save at each transition
- **Error handling**: Graceful degradation

## Risks & Mitigations

| Risco | Prob | Impacto | Mitigação |
|-------|------|---------|-----------|
| Complexidade state management | Média | Alto | Estado simples, testes extensivos |
| Performance múltiplos módulos | Baixa | Médio | Lazy loading, cleanup |
| Dados inconsistentes entre módulos | Média | Médio | Validação pipeline, schemas |

## Testing Strategy

### Happy Path Testing
1. Create 3-module activity
2. Complete modules in sequence
3. Verify data flows correctly
4. Confirm final state accuracy

### Edge Cases
- Network interruption during transition
- Invalid data passed between modules
- Very long activities (>30 minutes)
- Rapid clicking during transitions

## User Experience Notes

- Smooth transitions between modules (no jarring jumps)
- Clear progress indication throughout
- Ability to pause/resume activities
- Consistent UI/UX across all modules