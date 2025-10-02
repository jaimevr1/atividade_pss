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

---

## QA Results

### Review Date: 2025-10-01

### Reviewed By: Quinn (Test Architect)

### Review Stage: PLANNING

**Status**: This is a planning-stage review assessing specification quality before implementation.

### Code Quality Assessment

N/A - Planning stage, no code implementation yet.

**Specification Quality**: 68/100
- Acceptance Criteria Clarity: 85/100 (clear Given-When-Then, well-structured)
- Performance Requirements: 70/100 (duration estimation defined, orchestration targets missing)
- Risk Assessment: 75/100 (8 risks identified, 2 critical requiring architecture work)
- Technical Architecture: 80/100 (excellent component separation, state management needs detail)
- Definition of Done: 90/100 (comprehensive, one subjective criterion needs quantification)

### Compliance Check

- ✓ Story template followed correctly
- ✓ Given-When-Then format in ACs
- ✓ Dependencies documented (critical US-001 dependency)
- ✓ Risk matrix present
- ⚠ State management architecture needs ADR (critical gap)
- ⚠ Performance targets incomplete (orchestration overhead, transition time)
- ⚠ "Smooth transitions" subjective (needs quantification)

### Improvements Checklist

**Story Specification Improvements Needed**:
- [ ] Create ADR-004: Activity State Management Architecture - **CRITICAL**
- [ ] Add performance targets: orchestration <500ms, transitions <1s, pipeline <50ms - **HIGH**
- [ ] Quantify "smooth transitions" in DoD-9 (transition time, no flicker, animations) - **MEDIUM**
- [ ] Define inter-module data schemas with validation - **HIGH**
- [ ] Create module compatibility matrix - **HIGH**
- [ ] Add state consistency rules and conflict resolution strategy - **CRITICAL**

**Dependency Validation** (CRITICAL):
- [ ] Verify US-001 quality gate is PASS before starting US-002
- [ ] Verify US-001 state management is stable and tested
- [ ] Verify US-001 error isolation works correctly
- [ ] Test US-001 with multiple module instances (US-002 scenario)

**Technical Documentation Needed**:
- [ ] Create ADR-004: Activity State Management Architecture - **CRITICAL**
  - State machine diagram with all transitions
  - Persistence strategy (localStorage + Supabase)
  - Conflict resolution rules (last-write-wins with timestamp)
  - State validation mechanisms
- [ ] Document data pipeline architecture (schemas, transformations, validation)
- [ ] Document module compatibility matrix
- [ ] Create activity configuration templates (known-good patterns)

### Security Review

**Status**: PASS - Inherits US-001 security foundation

**Positive Indicators**:
- Reuses US-001 ModuleValidator for activity configurations
- Activity schema is structured, not user-generated code
- Student data encryption via Supabase RLS
- Data flow pipeline uses defined schemas

**New Attack Surfaces Identified**:
1. **Inter-module data injection**: Malicious module could inject data affecting next module
   - **Mitigation**: Validate and sanitize all data passed through DataPipeline
2. **Activity configuration tampering**: Modified dataFlow could route data incorrectly
   - **Mitigation**: Validate dataFlow configurations against module compatibility
3. **Progress data exposure**: Activity state contains student responses
   - **Mitigation**: Encrypt student progress data at rest, implement proper RLS

**Security Testing Requirements**:
- [ ] Data pipeline injection attempt tests
- [ ] Activity config tampering detection
- [ ] Student data isolation tests
- [ ] Inter-module data sanitization validation

### Performance Considerations

**Status**: CONCERNS - Targets partially defined, orchestration overhead not quantified

**Targets Defined**:
- ✓ Duration estimation accuracy: ±10% (clearly defined)

**Gaps Identified**:
- ✗ No orchestration overhead target (recommend <500ms)
- ✗ No transition time target (recommend <1s including save)
- ✗ No data pipeline transformation target (recommend <50ms)
- ✗ No multi-module memory strategy (1 module = 50MB, what about 5?)

**Recommended Performance Budget**:

*Single Module (US-001 baseline)*:
- Initialization: 2s
- Memory: 50MB
- Validation: 100ms

*3-Module Activity (US-002 target)*:
- Total initialization: 3s (includes orchestration overhead)
- Total memory: 120MB (modules + orchestrator state)
- Activity setup: 500ms (sequencing + validation)
- Per-transition: 1s (save + load + transform)

*5-Module Activity (US-002 stress)*:
- Total initialization: 5s (acceptable degradation)
- Total memory: 180MB (warn if >200MB)
- Activity setup: 800ms
- Per-transition: 1s (same, isolated)

**Additional Recommendations**:
- Add performance test suite for multi-module scenarios
- Monitor duration estimation accuracy in production
- Track state save/recovery performance (<200ms auto-save)

### Files Modified During Review

N/A - Planning stage, no code files exist yet.

**Specification files recommended for update**: docs/stories/epic-1/us-002-sistema-atividades-compostas.md

### Gate Status

**Gate**: CONCERNS → docs/qa/gates/1.002-sistema-atividades-compostas.yml

**Risk profile**: docs/qa/assessments/1.002-risk-20251001.md (8 risks, 2 critical)

**NFR assessment**: docs/qa/assessments/1.002-nfr-20251001.md (Security: PASS, Performance: CONCERNS, Reliability: CONCERNS, Maintainability: PASS)

**Test design**: docs/qa/assessments/1.002-test-design-20251001.md (35 scenarios: 12 unit, 14 integration, 6 E2E, 3 security)

**Trace matrix**: docs/qa/assessments/1.002-trace-20251001.md (91% full coverage, 1 partial gap acceptable for MVP)

### Recommended Status

**⚠ Changes Required - Architecture & Dependency Refinement Needed Before Implementation**

**Rationale**: Story has excellent component architecture with clear orchestration responsibilities. However, critical gaps must be addressed in planning phase:

**CRITICAL BLOCKERS** (Must fix before coding starts):
1. **Verify US-001 dependency**: Ensure US-001 passes quality gates - CRITICAL
   - State management must be proven stable
   - Error isolation must work correctly
   - If US-001 has issues, US-002 will amplify them 3-5x
2. **Create ADR-004**: Activity State Management Architecture - CRITICAL
   - State machine diagram with all transitions
   - Persistence strategy (localStorage + Supabase)
   - Conflict resolution rules
   - State validation mechanisms
3. **Add performance targets**: Orchestration, transition, pipeline - HIGH
4. **Define inter-module data schemas** with validation - HIGH
5. **Create module compatibility matrix** - HIGH

**Estimated refinement time**: ~20 hours
- US-001 dependency verification: 2 hours
- State management architecture (ADR-004): 6 hours
- Performance targets refinement: 2 hours
- Data pipeline design: 4 hours
- Module compatibility matrix: 2 hours
- Test infrastructure setup (Supabase): 4 hours

Once refinement checklist is complete AND US-001 verified, story is **READY FOR IMPLEMENTATION** with high confidence.

### Quality Score

**68/100** (Planning baseline)

**Calculation**: 100 - (10 × CONCERNS state mgmt) - (10 × CONCERNS perf targets) - (5 × CONCERNS dependency) - (5 × CONCERNS data validation) - (2 × subjective UX) = 68

**Gate Decision**: CONCERNS - PENDING IMPLEMENTATION (with critical refinement and dependency verification)

### Critical Risks Requiring Immediate Attention

**Risk Score: 52/100** (8 risks identified, 2 critical)

#### 1. DATA-002: Complex State Management Across Multiple Modules (Score: 9 - CRITICAL)
- **Probability**: High (3) - Activity state spans multiple modules with transitions
- **Impact**: High (3) - State corruption breaks entire activity, student loses all progress
- **Mitigation**: Single source of truth (ActivityOrchestrator), immutable state updates, comprehensive state testing (20+ scenarios)
- **Owner**: dev
- **Timeline**: Critical - requires ADR before coding

#### 2. FLOW-001: Inter-Module Data Pipeline Failures (Score: 9 - CRITICAL)
- **Probability**: High (3) - Data transformation between incompatible modules
- **Impact**: High (3) - Next module receives invalid data, activity breaks
- **Mitigation**: Strict data contracts, schema validation at boundaries, transformation registry, graceful defaults
- **Owner**: dev
- **Timeline**: Must be in place before multi-module activities

#### 3. PERSIST-001: Progress Persistence and Resumption Failures (Score: 6 - HIGH)
- **Probability**: Medium (2) - Network issues, browser crashes
- **Impact**: High (3) - Student loses significant work
- **Mitigation**: Auto-save every 30s, dual persistence (localStorage + Supabase), offline-first architecture
- **Owner**: dev

**Additional Risks**: 5 more risks documented in risk profile (SEQ-001, PERF-003, UX-001, DATA-003, ORCH-001)

### Test Coverage Summary

**Total Test Scenarios Designed**: 35
- **Unit Tests**: 12 (40%) - Orchestration, sequencing, data flow, progress logic
- **Integration Tests**: 14 (40%) - Multi-module coordination, state management, data pipeline
- **E2E Tests**: 6 (20%) - Complete student/teacher journeys
- **Security Tests**: 3 - Data pipeline injection, config tampering, data isolation

**Requirements Coverage**: 91% (10/11 requirements fully covered)
- **Fully Covered**: 10 requirements
- **Partially Covered**: 1 requirement (DoD-9 "smooth transitions" - quantified as <1s)
- **Not Covered**: 0 requirements

**Priority Distribution**:
- **P0 (Critical)**: 14 tests - Core state management, data integrity, progress persistence
- **P1 (Important)**: 16 tests - Teacher UX, analytics, duration accuracy
- **P2 (Nice to Have)**: 5 tests - Edge cases, UX polish

**Risk Coverage**: All 8 identified risks have explicit test scenarios mapped

### Key NFR Findings

**Security**: PASS
- Inherits US-001 security foundation
- New data flow validation needed
- Student data encryption via Supabase

**Performance**: CONCERNS
- Duration estimation defined (±10%) ✓
- Orchestration overhead not quantified ✗
- Transition time not defined ✗
- Multi-module memory strategy missing ✗

**Reliability**: CONCERNS
- Progress persistence addressed ✓
- State consistency rules undefined ✗
- Conflict resolution strategy missing ✗
- Error cascading not specified ✗

**Maintainability**: PASS
- Excellent component separation ✓
- Clear single responsibilities ✓
- Integration tests planned ✓

### Dependency Gate Check

**US-001 Prerequisites** (Must be verified before US-002 implementation):
- [ ] US-001 quality gate is PASS or PASS WITH MINOR ISSUES
- [ ] US-001 state management proven stable
- [ ] US-001 error boundaries working correctly
- [ ] US-001 performance targets met (<2s init, <50MB memory)
- [ ] US-001 security validation working (JSON schema, XSS prevention)

**Impact if US-001 has issues**: State management problems will amplify 3-5x across multiple modules in US-002

---

**QA Contact**: For questions about this review or planning artifacts, contact Quinn (Test Architect).

**Next Review Trigger**: When US-001 gate is PASS AND ADR-004 created AND performance targets added OR when implementation begins