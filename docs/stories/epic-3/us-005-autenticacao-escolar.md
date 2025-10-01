# US-005: Sistema de Autenticação Escolar

**Epic:** EPIC-003 - Plataforma Base e Dashboard
**Prioridade:** Must Have (MVP)
**Estimativa:** 2 Story Points
**Tipo:** Core Infrastructure

## User Story

**As a** student in a supervised computer lab
**I want to** log in quickly without complex passwords
**So that** I can start learning immediately without technical barriers

## Business Value

Viabiliza o **modelo de uso supervisionado** do Dimidui em laboratórios escolares. Remove barreiras técnicas que poderiam impedir o foco pedagógico da plataforma.

## Acceptance Criteria

### AC-1: Simple Login Interface
**Given** I access the Dimidui platform in the school lab
**When** I reach the login screen
**Then** I should see:
- Clean, welcoming interface with Dimidui branding
- List of student names organized by class/grade
- Large, readable name buttons (48px+ height)
- Search functionality for large classes
- Teacher access option (separate workflow)

### AC-2: Name-Based Authentication
**Given** I see the list of student names
**When** I click on my name
**Then** the system should:
- Log me in immediately (no password required)
- Load my personalized learning profile
- Redirect to my dashboard with my progress
- Establish secure session for lab duration

### AC-3: Session Management
**Given** I'm logged in during lab time
**When** I work on the platform
**Then** the system should:
- Maintain my session throughout the lab period (45-60min)
- Auto-save progress every 2 minutes
- Handle browser refresh gracefully
- Provide manual logout option if needed

### AC-4: Security for Supervised Environment
**Given** this is a supervised educational environment
**When** authentication is processed
**Then** the system should:
- Restrict access to pre-registered students only
- Log all login activities for teacher oversight
- Prevent session hijacking between students
- Clear session data on explicit logout

## Technical Requirements

### Authentication Architecture
```javascript
// Simplified auth for supervised environment
const schoolAuthConfig = {
  type: "supervised_classroom",
  passwordRequired: false,
  sessionDuration: 3600, // 1 hour
  autoSave: 120, // 2 minutes

  security: {
    ipWhitelist: ["school_lab_range"],
    sessionIsolation: true,
    activityLogging: true,
    preventMultipleLogins: true
  }
}
```

### Student Registration Schema
```javascript
// Pre-populated student data
const studentRoster = {
  class: "5A_turma_manha",
  teacher: "Prof. Maria Silva",
  students: [
    {
      id: "uuid",
      name: "Ana Beatriz Santos",
      displayName: "Ana Beatriz",
      grade: 5,
      preferences: {
        font: "atkinson",
        theme: "light"
      },
      enrollmentDate: "2025-01-15"
    }
    // ... more students
  ]
}
```

### Session Management
```javascript
// Secure session handling
const sessionManager = {
  create: (studentId, ipAddress) => {
    return {
      sessionId: generateSecureToken(),
      studentId,
      ipAddress,
      loginTime: new Date(),
      expiresAt: addHours(new Date(), 1),
      isActive: true
    }
  },

  validate: (sessionId) => {
    // Check session validity, IP consistency, expiration
  },

  cleanup: () => {
    // Remove expired sessions
  }
}
```

## UI/UX Requirements

### Login Interface Design
```
┌─────────────────────────────────────┐
│           🎓 Dimidui               │
│    Aprenda Fazendo, Ensine Crescendo │
├─────────────────────────────────────┤
│                                     │
│  👋 Selecione seu nome para entrar:  │
│                                     │
│  🔍 [Buscar nome...]               │
│                                     │
│  📚 TURMA 5A - MANHÃ               │
│  ┌─────────────────────────────┐   │
│  │ [Ana Beatriz Santos]        │   │
│  │ [Bruno Oliveira Costa]      │   │
│  │ [Carlos Eduardo Silva]      │   │
│  │ [Daniela Ferreira Lima]     │   │
│  │ [Eduardo Santos Pereira]    │   │
│  │        ... mais ...         │   │
│  └─────────────────────────────┘   │
│                                     │
│  👩‍🏫 [Acesso do Professor]          │
│                                     │
└─────────────────────────────────────┘
```

### Visual Design Requirements
- **Clean, uncluttered interface** focused on name selection
- **Large touch targets** for easy clicking (48px+ height)
- **Consistent branding** with Dimidui color scheme
- **Accessible typography** (Atkinson Hyperlegible default)
- **Loading states** during authentication process

## Security Considerations

### Supervised Environment Security
- **IP restriction** to school network range
- **Time-based access** (lab hours only)
- **Session isolation** prevents cross-contamination
- **Activity logging** for teacher oversight
- **No sensitive data** stored in browser

### Privacy Compliance (LGPD)
- **Minimal data collection** (name, grade, learning progress)
- **Educational purpose** clearly defined
- **Parental consent** handled by school registration
- **Data retention** limited to school year
- **Right to deletion** upon student withdrawal

## Definition of Done

- [ ] Simple name-based login working reliably
- [ ] Student roster management system
- [ ] Session management with appropriate duration
- [ ] Security measures for supervised environment
- [ ] Teacher override/management capabilities
- [ ] Activity logging for oversight
- [ ] Performance: login process <3 seconds
- [ ] Mobile-responsive design (teacher monitoring)
- [ ] Accessibility compliance (WCAG AA)
- [ ] LGPD privacy compliance validated
- [ ] Integration tests with user scenarios
- [ ] Load testing for full lab (30 students)

## Integration Points

### Teacher Management Interface
```javascript
// Teacher can manage student access
const teacherControls = {
  viewActiveStudents: () => getActiveSessions(),
  addStudent: (name, grade) => addToRoster(),
  removeStudent: (studentId) => removeFromRoster(),
  viewActivityLog: (dateRange) => getLoginActivity(),
  emergencyLogout: (studentId) => forceLogout()
}
```

### Student Profile Integration
```javascript
// Seamless transition to student dashboard
const postLogin = {
  loadStudentProfile: async (studentId) => {
    const profile = await getStudentData(studentId);
    const progress = await getProgressData(studentId);
    const jeremias = await getJeremiasState(studentId);

    return {
      student: profile,
      progress: progress,
      character: jeremias,
      dashboard: generateDashboard(profile, progress)
    }
  }
}
```

## Testing Strategy

### User Acceptance Testing
- **Students (9-12 anos)**: Can log in independently
- **Teachers**: Can manage roster and monitor activity
- **IT administrators**: System works reliably in lab environment

### Security Testing
- **Session isolation**: Students can't access each other's data
- **IP restrictions**: External access properly blocked
- **Session expiration**: Automatic cleanup working
- **Activity logging**: All login events properly tracked

### Performance Testing
- **Concurrent logins**: 30 students logging in simultaneously
- **Network resilience**: Works with typical school internet
- **Browser compatibility**: Works on school computers
- **Memory usage**: Minimal impact on shared computers

## Risks & Mitigations

| Risco | Prob | Impacto | Mitigação |
|-------|------|---------|-----------|
| Múltiplos logins mesmo estudante | Média | Médio | Detecção e bloqueio automático |
| Falha rede durante sessão | Alta | Baixo | Auto-save frequente, recuperação |
| Lista de nomes muito longa | Média | Baixo | Busca, paginação, organização |
| Estudante esquece próprio nome | Baixa | Baixo | Professor pode ajudar, busca |

## Teacher Support Features

### Roster Management
- **Import from school system** (CSV, API integration)
- **Manual addition/removal** of students
- **Class organization** by grade/period
- **Bulk operations** for efficiency

### Monitoring Dashboard
- **Real-time view** of logged-in students
- **Activity overview** per student
- **Session duration** tracking
- **Quick intervention** tools (logout, reset)

## Future Enhancements (V2+)

- **QR code login** for faster access
- **Photo-based selection** for younger students
- **Integration with school management systems**
- **Offline mode** for network interruptions
- **Multi-language support** for diverse schools