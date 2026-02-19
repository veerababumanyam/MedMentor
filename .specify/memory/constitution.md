<!--
Sync Impact Report
==================
Version change: [UNINITIALIZED] → 1.0.0
Modified principles: N/A (initial ratification)
Added sections:
  - All Core Principles (I-VII)
  - Medical Safety & Boundaries
  - AI Governance Requirements
  - Development Standards
Removed sections: N/A (initial ratification)
Templates requiring updates:
  ✅ plan-template.md - No constitution-specific gates to add (template uses placeholders)
  ✅ spec-template.md - Aligned with medical safety requirements
  ✅ tasks-template.md - Aligned with testing discipline principles
  ✅ commands/ - No command templates exist yet
Follow-up TODOs: None
-->

# MedMentor AI Constitution

## Core Principles

### I. Evidence-Grounded Truth (NON-NEGOTIABLE)

All medical factual assertions MUST be sourced from retrievable, trusted medical references. AI-generated
content without retrieval backing is prohibited.

- Every medical claim MUST include a verifiable citation to source material
- Retrieved content MUST be traceable back to its source document
- Hallucinated citations are NEVER permitted - if retrieval fails, the system MUST admit uncertainty
- Confidence levels MUST be explicitly stated when evidence strength varies
- Rationale: Medical education requires factual accuracy; unsourced AI generation risks misinformation
  that could impact future clinical decisions

### II. Safety-By-Default Boundaries

The system MUST refuse requests for patient-specific medical advice, diagnosis, or treatment recommendations.

- Clear, prominent disclaimers MUST state this is an educational tool, NOT medical advice
- User queries seeking diagnosis or treatment MUST be refused with educational redirection
- Emergency situations MUST trigger immediate escalation to emergency services
- Content boundaries MUST be enforced before AI processing begins
- Rationale: Preventing harm from inappropriate medical guidance is the highest priority

### III. Privacy-First Architecture

User data MUST be minimized, isolated, and under user control. Workspace-based separation prevents
unintended data sharing.

- Personal workspaces are the DEFAULT - cohort/institution features require explicit opt-in
- Users MUST have export and delete capabilities for all their data
- Data minimization: collect only what is necessary for the learning function
- Clinical content MUST be handled with HIPAA-aware safeguards
- Rationale: Medical learners deserve strong privacy protections for their study activities

### IV. Internationalization from Day One

All features MUST be designed for multi-language support from inception. No English-only assumptions
in architecture or UI.

- All user-facing strings MUST be externalized (no hardcoded English in code)
- UI layouts MUST support right-to-left languages from initial design
- Date, time, and number formatting MUST be locale-aware
- Non-English content MUST include bilingual safety labels
- Tiered rollout: English first, then Spanish, Portuguese, French, Arabic
- Rationale: Medical education is global; retrofitting i18n is exponentially more expensive

### V. Mobile-First Learning Loops

The user experience MUST be optimized for brief, focused study sessions across devices.

- Target learning loop: 2-5 minutes of focused interaction
- Cross-device synchronization MUST work seamlessly
- Offline-first consideration for core study features
- Progressive enhancement: core features work on lowest-target device
- Rationale: Medical study happens in stolen moments throughout busy days

### VI. Quality Excellence & Testing

Test-first development with human-in-the-loop validation is required for all features.

- TDD preferred: tests written before implementation when scope permits
- Critical paths (safety refusals, citation integrity) MUST have automated tests
- Human review workflow for AI-generated content before user exposure
- Continuous feedback loops for quality improvement
- Rationale: In medical education, quality is not optional - errors erode trust

### VII. Modular Monolith Architecture

Clear module boundaries enable independent development while maintaining deployment simplicity.

- Feature modules MUST have well-defined interfaces
- Shared code MUST have clear ownership and documentation
- Module boundaries SHOULD align with domain capabilities (Tutor, Retrieval, Quiz, Flashcard)
- Rationale: Enables team autonomy while keeping early-stage deployment manageable

## Medical Safety & Boundaries

### Permitted Use Cases

- General medical knowledge learning and review
- Exam preparation for standardized medical tests
- Clinical reasoning practice with hypothetical cases
- Drug information and interaction reference
- Anatomy and physiology study

### Prohibited Use Cases

- Diagnosis or treatment recommendations for specific patients
- Interpretation of individual patient lab results or imaging
- Prescription of medications or dosages for real patients
- Emergency medical guidance (redirect to emergency services)
- Medical advice that could delay seeking professional care

### Response Protocol for Prohibited Queries

1. Acknowledge the query intent
2. Clearly state the boundary (e.g., "I cannot provide diagnosis")
3. Redirect to appropriate resources (e.g., "Please consult a healthcare professional")
4. Offer educational alternatives (e.g., "I can help you learn about this condition generally")

## AI Governance Requirements

### Retrieval-First Protocol

- Factual medical queries MUST trigger retrieval before generation
- Generated content MUST be grounded in retrieved sources
- Citations MUST link directly to source material
- When retrieval fails, uncertainty MUST be explicitly stated

### Safety Evaluations

- All AI-generated content MUST pass safety checks before user exposure
- High-risk content (diagnostic claims, treatment recommendations) triggers enhanced review
- User-reported content MUST be routed for human review
- Safety evaluation results MUST be logged for continuous improvement

### Graceful Degradation

- When AI models are unavailable, fallback to cached/retrieved content
- System MUST remain partially functional during AI service outages
- Users MUST be informed when AI features are temporarily unavailable

### Ethical Data Use

- User data for AI training requires explicit opt-in consent
- Uploaded content MUST include IP attestation
- Takedown procedures MUST exist for copyright violations
- Training data usage MUST be transparent in privacy policy

## Development Standards

### Technology Stack

- **Frontend**: React (Vite), TypeScript, Tailwind CSS, Glassmorphism design system
- **Backend**: Modular monolith with RESTful API, OAuth authentication
- **AI**: Agentic architecture (Tutor, Retrieval, Quiz, Flashcard, Safety agents)
- **Database**: Relational (PostgreSQL) + Vector store + Object storage

### Code Quality

- TypeScript strict mode enabled
- ESLint and Prettier configured with team standards
- Code review required for all changes
- Documentation for all public APIs and complex logic

### Performance Targets

- API response time: p95 < 500ms for retrieval queries
- Quiz generation: < 10 seconds for 10-item set
- Flashcard generation: < 5 seconds for 20-card set
- Mobile page load: < 3 seconds on 4G

### Accessibility

- WCAG 2.2 AA compliance minimum
- Screen reader compatibility for all study modes
- Keyboard navigation support
- Color contrast minimum 4.5:1 for text

### Compliance Alignment

- NIST AI Risk Management Framework
- HIPAA considerations for clinical content handling
- GDPR compatibility for EU user data
- Regional medical device regulations as applicable

## Governance

### Amendment Procedure

1. Proposed amendments MUST be documented with rationale
2. Impact analysis MUST assess effects on existing features
3. Team approval required via consensus or technical lead decision
4. Migration plan MUST exist for backward-incompatible changes
5. Version number MUST follow semantic versioning (MAJOR.MINOR.PATCH)

### Compliance Verification

- All pull requests MUST verify constitutional compliance
- Complexity beyond constitutional principles MUST be justified
- Templates (plan, spec, tasks) MUST align with current constitution
- Constitutional violations require documented exception approval

### Version Policy

- **MAJOR**: Backward-incompatible governance/principle removals or redefinitions
- **MINOR**: New principle or section added; material expansion of guidance
- **PATCH**: Clarifications, wording improvements, non-semantic refinements

### Review Schedule

- Constitution review quarterly or before major releases
- Template alignment verification after any constitution amendment
- Team training on constitutional principles for new members

**Version**: 1.0.0 | **Ratified**: 2025-02-19 | **Last Amended**: 2025-02-19
