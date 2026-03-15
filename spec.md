# CredMatch Backend Specification

## Overview

CredMatch is a federal talent marketplace that combines AI-powered matching with human credibility signals to help federal contractors find qualified talent faster. The platform serves four primary personas: Platform Operators/Admins, Federal Contractors/Employers, Job Candidates, and Recruiters/Talent Partners. The backend must provide secure, compliant APIs that serve the existing frontend application at `/Users/puttaiaharugunta/codebase/infi/frontend-fleet-fullauto/apps/credmatch`.

This specification defines the backend services needed to power a complete multi-tenant marketplace with AI-driven candidate-to-job matching, referral workflows with rewards, compliance audit logging, and federal hiring regulation adherence (Section 508, OFCCP, FAR 52.204-21, NIST 800-63-4). The platform differentiates itself by integrating AI matching scores with human credibility indicators—referrals, trust scores, and delivery history—to surface candidates who not only qualify but can deliver.

The backend must serve the existing React frontend without breaking the TypeScript type contracts defined in `/Users/puttaiaharugunta/codebase/infi/frontend-fleet-fullauto/apps/credmatch/src/types/index.ts`. All APIs must produce data that matches these interfaces directly to avoid frontend transformation layers.

## Current System Capabilities

The frontend application at `/Users/puttaiaharugunta/codebase/infi/frontend-fleet-fullauto/apps/credmatch` provides a complete UI shell with 50+ page components organized by persona. The frontend currently uses mock data files for visualization and has no backend integration. The following capabilities are already implemented in the UI and must be supported by backend APIs:

**Admin Capabilities:**
- View dashboard with platform KPIs (active tenants, placements, flagged referrals)
- Create and manage employer and recruiter agency tenants with billing status
- Manage platform users with persona assignment, MFA enforcement, and invitations
- View and moderate marketplace jobs and profiles
- Configure AI matching engine scoring factors, weights, and thresholds
- Define referral payout models, rewards, and fraud prevention rules
- Access compliance center with audit log exports, record retention settings, and OFCCP configuration
- Configure SSO providers per tenant and manage security policies
- View analytics including placement funnels, match quality metrics, and compliance reports

**Employer Capabilities:**
- View dashboard with open requisitions, active submissions, and matched candidates
- Manage organization profile including federal domain areas and referral policy
- Create, edit, view, and manage job requisitions with federal contract details
- Search talent marketplace with advanced filters (clearance, location, skills, agency experience)
- View candidate profiles with match scores, credibility scores, and referral chains
- Assign and manage recruiters with access to specific jobs
- Track hiring pipeline with statuses: Applied → Referred → Screened → Submitted → Interview → Offer → Placed
- Manage referral requests, rewards, liability tracking, and payout approvals
- Access compliance center with applicant logs, disposition reasons, and audit exports

**Candidate Capabilities:**
- View dashboard with profile completeness, recommended jobs, applications, and credibility score
- Build complete profile with personal info, resume upload, skills, certifications, clearance, work history
- View trust score, delivery score, and improvement tips
- Browse job marketplace with filters and view detailed job information
- Apply to jobs with application tracking
- Request and view referrals from recruiters
- Track application status timeline and interview schedules
- Manage documents (resumes, certifications) with verification status
- Control profile visibility and privacy settings

**Recruiter Capabilities:**
- View dashboard with assigned jobs, candidate matches, referral requests, and rewards
- View jobs assigned by employers with SLA deadlines and required qualifications
- Search and browse candidate profiles with fit scores
- Match candidates to jobs with comparison, match explanations, notes, and submission actions
- View inbound referral requests from candidates and accept or decline
- Track submitted candidates with employer feedback, interview/offer/placement status
- View earned rewards, performance metrics, and ranking
- Manage notifications, payout details, team members, and permissions

**Data Contracts (Must Preserve):**
The frontend TypeScript interfaces define the authoritative data contracts that must be preserved. Key types include Candidate, Job, MatchScore, Referral, Application, EngagementRecord, EmployerFeedback, AuditEvent, Tenant, Notification, KpiItem, and all enums (Persona, ClearanceLevel, ClearanceStatus, ContractType, WorkArrangement, JobStatus, Priority, ApplicationStatus, ReferralStatus, RewardStatus, Trend).

## User Stories & Requirements

### Authentication & Access Control

- As an **Admin**, I want to configure SSO providers (Google Workspace, Okta, Auth0) per tenant so that organizations can use their existing identity systems.
- As an **Employer Admin**, I want to manage user permissions within my organization so that I control who can post jobs, view candidates, or approve payouts.
- As a **Platform Admin**, I want to enforce multi-factor authentication for all users so that access to sensitive hiring data is secure.
- As a **Recruiter**, I want to see only jobs assigned to me by employers who have granted access so that I cannot view unauthorized requisitions.
- As a **Candidate**, I want to control my profile visibility (public, referral-only, private) so that I manage my exposure in the marketplace.

### Job Management

- As an **Employer**, I want to create job requisitions with federal contract details (agency, contract vehicle, clearance required, labor category) so that candidates understand the role requirements.
- As an **Employer**, I want to specify required skills, certifications, and years of experience so that AI matching produces accurate scores.
- As an **Employer**, I want to mark jobs as draft, active, paused, or closed so that I control when positions are visible to candidates.
- As an **Candidate**, I want to search jobs by clearance level, location, work arrangement, and skills so that I find relevant federal opportunities.
- As a **Candidate**, I want to filter jobs by federal agency and contract vehicle so that I can target agencies where I have experience.

### Candidate Profile Management

- As a **Candidate**, I want to create a profile with personal info, professional headline, and summary so that employers can evaluate my qualifications.
- As a **Candidate**, I want to upload my resume/CV for parsing so that skills and experience are auto-populated.
- As a **Candidate**, I want to see my profile completeness score so that I know what information to add to improve visibility.
- As an **Employer**, I want to view candidate profiles with clearance status, skills, certifications, and trust scores so that I can evaluate both qualification and delivery capability.
- As a **Candidate**, I want to see my trust and delivery scores so that I understand my reputation on the platform.

### AI Matching Engine

- As an **Employer**, I want to see candidates ranked by overall match score for each job so that I can prioritize who to review.
- As an **Employer**, I want to see match score breakdown (clearance, technical skills, certifications, education, location, experience) so that I understand why a candidate ranked highly.
- As an **Admin**, I want to configure matching weights (e.g., 35% qualifications, 20% federal relevance) so that scoring aligns with different hiring priorities.
- As an **Employer**, I want to see which must-have requirements matched and skill gaps for each candidate so that I can quickly assess fit.
- As an **Recruiter**, I want to see how my referrals influence candidate rankings so that I understand the credibility value I provide.

### Referral System

- As a **Recruiter**, I want to submit candidates for jobs with notes on fit so that employers receive quality submissions.
- As a **Candidate**, I want to request referrals from recruiters for specific jobs so that I can leverage their credibility to improve my visibility.
- As a **Recruiter**, I want to track referral status (Requested → Accepted → Submitted → Interviewed → Offered → Placed) so that I know where my referrals stand.
- As an **Employer**, I want to see referral chains (who referred the candidate and their credibility) so that I can assess trustworthiness alongside qualifications.
- As a **Recruiter**, I want to see earned rewards, pending rewards, and payout status so that I can track my earnings from successful placements.

### Compliance & Auditing

- As an **Admin**, I want to see immutable audit logs for all platform actions so that I can investigate issues and demonstrate regulatory compliance.
- As an **Admin**, I want to export audit logs for compliance reporting so that I can meet OFCCP audit requirements.
- As an **Employer**, I want to select disposition reasons when rejecting candidates so that OFCCP reporting is accurate and defensible.
- As an **Admin**, I want to configure applicant record retention periods so that privacy and compliance requirements are balanced.
- As a **Platform**, I want to store self-ID data (disability, veteran status) separately from matching algorithms so that protected class information does not influence recommendations.

### Multi-Tenant Isolation

- As an **Admin**, I want to create and manage employer and recruiter agency tenants so that I can onboard new customers.
- As an **Admin**, I want to ensure that tenant data is strictly isolated so that employers cannot access each other's candidates, jobs, or data.
- As a **Recruiter**, I want to see only jobs from employers who have explicitly assigned me so that I do not access unauthorized data.
- As an **Employer**, I want my candidates and hiring data to be private so that competitors cannot view my talent pipeline.
- As an **Admin**, I want to see tenant status, contract plan, and billing information so that I can manage platform customers.

### Notifications

- As a **User**, I want to receive notifications for new matches, application updates, referral requests, and rewards so that I stay informed of marketplace activity.
- As a **User**, I want to see notifications grouped by type and persona so that I can manage them efficiently.
- As a **User**, I want to configure notification preferences (email, in-app, none) by notification type so that I control alert volume.
- As an **Admin**, I want to manage notification templates and frequency so that platform communication is consistent and professional.

### Reporting & Analytics

- As an **Admin**, I want to see platform-wide KPIs (active tenants, total placements, flagged referrals) so that I understand marketplace health.
- As an **Employer**, I want to see placement funnel metrics (applications to placed ratio) so that I can evaluate hiring efficiency.
- As an **Recruiter**, I want to see performance metrics (placement success rate, employer satisfaction) so that I can improve my reputation.
- As an **Admin**, I want to generate compliance reports (applicant disposition summary, record retention status) so that I can prepare for audits.
- As an **Employer**, I want to export match quality analytics so that I can evaluate whether the matching engine is producing good candidate shortlists.

## Business Rules

### Protected Class Data Non-Discrimination
- Protected class data (race, ethnicity, age, disability status, veteran status) must NOT influence matching scores, recommendations, or any ranking algorithms. This data may be stored for compliance purposes (OFCCP self-ID workflows) but must be computationally excluded from all matching and recommendation logic.
- Source: Constitution.md Principle IV — Non-negotiable requirement for federal compliance.

### Multi-Tenant Data Isolation
- All tenant data (employers and recruiter agencies) must be strictly isolated at the database query layer. Queries must be scoped by tenant_id. Employers cannot view other employers' candidates, jobs, applications, or any data.
- Recruiters see only jobs assigned to them by employers who have granted access explicitly.
- Source: Constitution.md Principle II — Multi-tenant isolation is non-negotiable.

### Credibility-Centric Matching
- Match scoring must incorporate referral strength, trust scores, and delivery history alongside technical qualifications. The credibility layer is fundamental and must not be reduced to an optional feature.
- Referral boost to match scores must be configurable (typically 5-15%) and transparently shown in match breakdowns.
- Source: Constitution.md Principle VI — Core value proposition of CredMatch.

### Compliance Record Retention
- Applicant records must be retained for minimum 2 years to meet OFCCP requirements. This retention period must be configurable per data type.
- Audit events must be immutable and preserved for regulatory compliance.
- Source: Constitution.md Principle III — Compliance-first design.

### Referral Fraud Prevention
- Duplicate referrals must be prevented (same candidate to same job by same recruiter).
- Circular referrals must be detected (A refers B, B refers A to same job).
- Fake profile risk scoring must be implemented based on patterns in profile creation and referral activity.
- Rewards must have a hold period for validation (configurable, typically 30-90 days) before payout.

### Score Calculation Rules
- Profile completeness score is the percentage of required fields completed by a candidate.
- Matchability score is the average match score across all jobs for a candidate.
- Delivery score (0-100) is based on engagement outcomes and employer feedback categories (technical skills, communication, delivery, professionalism).
- Trust/credibility score is based on referrals made and received, delivery history, and engagement quality.

### Matching Weight Configuration
- Default matching weight configuration: 35% hard qualification fit, 20% federal/domain relevance, 15% experience depth, 10% availability and logistics, 10% referral credibility, 10% delivery history/performance signals.
- Weights are configurable by platform admin within approved limits.
- Per-employer weight adjustments must be allowed within platform-defined boundaries.

### Document Validation
- Clearance documents must store only metadata (verification status, issue date, authority) not the actual documents themselves for security.
- Documents may be uploaded but storage location and verification status tracking is required.
- Verification requests must be tracked with status (pending, verified, rejected).

### Application Pipeline Constraints
- Application statuses follow defined flow: Applied → Referred → Screened → Submitted → Interview → Offer → Placed, with Not Selected and Withdrawn as terminal states.
- Disposition reasons are required for all rejections to satisfy OFCCP requirements.
- Interview dates must be trackable for scheduling and audit purposes.

## Data Entities

### User Entity
Represents platform users with authentication credentials, persona assignment, and tenant membership. Key attributes include unique identifier, email address, password hash, MFA secret (for TOTP authentication), tenant association, persona role (admin, employer, candidate, recruiter), and timestamps for creation and last update. Users are associated with tenants except for platform admins. The user entity is the foundation for all authentication and authorization.

### Tenant Entity
Represents employer organizations or recruiter agencies using the platform. Key attributes include unique identifier, organization name, tenant type (Employer or Recruiter Agency), operational status (Active, Suspended, Pending), subscription plan (Starter, Pro, Enterprise), SSO configuration (provider type, connection details), user count, active job count, placement count, creation timestamp, and contact email. Tenants provide the multi-tenant isolation boundary.

### Candidate Entity
Represents job seekers using the platform. Key attributes include unique identifier, associated user ID, full name, professional title, clearance level (TS/SCI, Secret, Public Trust, None), clearance status (Active, Eligible, Expired, N/A), geographic location, remote availability flag, skills array, certifications array, education (degree, field, institution), years of experience, profile completeness percentage (0-100), availability timeframe, rate range (min/max), professional summary, federal agency experience array, trust score (0-100), delivery score (0-100), and optional engagement records. Candidates are the primary talent resource matched against job requisitions.

### Job Entity
Represents federal contract positions posted by employers. Key attributes include unique identifier, associated employer ID (tenant), position title, federal agency name, contract vehicle type (IDIQ, BPA, T&M, FFP, CPFF, Other), required clearance level, geographic location, work arrangement (Remote, Hybrid, Onsite), salary range (min/max), required skills array, required certifications array, posting date, application deadline, status (Active, Draft, Paused, Closed), priority level (Critical, High, Normal), applicant count, matched candidate count, full job description, labor category, years of experience required, education requirement, referral eligibility flag, and funding status. Jobs are the positions that candidates are matched against.

### MatchScore Entity
Represents the AI-powered matching result between a specific candidate and job. Key attributes include unique identifier, candidate ID, job ID, overall match score (0-100), dimension breakdown (clearance, technical skills, certifications, education, location, experience) with each dimension having score, label (Strong Match, Partial Match, Gap), detail, matched items, and gaps, narrative explanation, referral boost percentage, confidence band (High, Medium, Low), red flags array, and creation timestamp. Match scores are the core AI product surface to users.

### Referral Entity
Represents recruiter-to-candidate referrals for specific jobs. Key attributes include unique identifier, recruiter ID, recruiter name, candidate ID, candidate name, job ID, job title, status (Requested, Accepted, Submitted, Interviewed, Offered, Placed, Rejected), referral date, last updated date, reward amount, reward status (Pending, Approved, Paid, Disputed), and referral notes. Referrals track the human credibility workflow that differentiates the platform.

### Application Entity
Represents candidate job applications with status tracking. Key attributes include unique identifier, candidate ID, candidate name, job ID, job title, employer name, status (Applied, Referred, Screened, Submitted, Interview, Offer, Placed, Not Selected, Withdrawn), application date, last updated date, match score, optional recruiter name (if referred), optional notes, and optional interview date. Applications track the formal hiring pipeline for each candidate-job pairing.

### EngagementRecord Entity
Represents work history for candidates on federal contracts. Key attributes include unique identifier, candidate ID, job ID, job title, federal agency, contract type, start date, optional end date, engagement status (Active, Completed, Extended, EarlyTermination), optional employer feedback, and recruiter name. Engagement records provide work history context and feed into delivery score calculations.

### EmployerFeedback Entity
Represents post-engagement ratings from employers that impact candidate credibility scores. Key attributes include unique identifier, engagement ID, employer ID, employer name, overall rating (1-5), category ratings (technical skills, communication, delivery, professionalism each 1-5), qualitative notes, engagement outcome (Completed, Extended, EarlyTermination, Ongoing), submission timestamp, impact on trust score, and impact on delivery score. Feedback is critical to the credibility scoring system.

### AuditEvent Entity
Represents immutable audit logs for compliance and security. Key attributes include unique identifier, precise timestamp, actor (user), actor role (persona), action performed, affected resource, resource identifier, outcome (Success, Failure, Warning), originating IP address, and detailed information. Audit events are append-only and must be preserved for regulatory compliance.

### RewardPayout Entity
Represents financial rewards paid to recruiters for successful referrals. Key attributes include unique identifier, associated referral ID, reward amount, payout status, approval timestamp, payment timestamp, approving user identifier, and notes. Reward payouts track the financial incentive system that drives quality referrals.

### Notification Entity
Represents platform notifications sent to users. Key attributes include unique identifier, notification type (match, application, referral, system, reward), title, message body, timestamp, read status flag, optional navigation link, persona array (which personas receive this notification type), and recipient identifier. Notifications keep users informed of marketplace activity.

### MatchingConfig Entity
Represents configurable matching engine parameters. Key attributes include unique identifier, scope (platform-wide or per-tenant), dimension weights (clearance, technical skills, certifications, education, location, experience, referral credibility, delivery history), minimum score thresholds, and confidence band boundaries. Matching configs allow the platform to adapt scoring to different hiring priorities while maintaining transparency.

### Role Entity
Represents permission roles in the RBAC system. Key attributes include unique identifier, role name (admin, employer_admin, employer_user, candidate, recruiter), role description, and associated permissions. Roles define what actions users can perform within the platform.

### Permission Entity
Represents fine-grained permissions for role assignments. Key attributes include unique identifier, permission name, resource type, and action (create, read, update, delete, approve, configure). Permissions enable granular access control within organizations.

### ComplianceDocument Entity
Represents compliance-related document metadata. Key attributes include unique identifier, associated user ID, document type (clearance, certification, etc.), verification status, verification timestamp, and metadata JSON. This entity stores only metadata for sensitive documents, not the documents themselves, for security compliance.

## API Requirements

### Authentication APIs
The system must provide endpoints for user authentication and session management. SSO integration endpoints must support OAuth 2.0 and SAML 2.0 flows for Google Workspace, Okta, and Auth0 providers. Email/password authentication with MFA (TOTP) must also be supported. JWT-based access tokens and refresh tokens must be issued with configurable expiration times. Session management must support login, logout, token refresh, MFA setup and verification, and session invalidation.

### Authorization APIs
The system must provide endpoints for role-based access control management. Admin users must be able to create roles, assign permissions to roles, and assign users to roles and tenants. Tenant administrators must be able to manage permissions within their organization scope. Permission checks must be enforced at the API level for all protected resources.

### Tenant Management APIs
The system must provide endpoints for creating, updating, and deleting employer and recruiter agency tenants. Admin users must be able to view all tenants, their status, plan, billing information, and SSO configuration. Tenant administrators must be able to manage their own organization profile, users, and settings. All tenant-scoped data must be automatically filtered to prevent cross-tenant access.

### Job Management APIs
The system must provide endpoints for creating, reading, updating, and deleting job requisitions. Employers must be able to post jobs with federal contract details, required qualifications, and visibility settings. Candidates and recruiters must be able to search and filter jobs by various criteria (clearance, location, skills, agency, contract vehicle). Job detail endpoints must return complete job information including required skills, certifications, and compensation range.

### Candidate Profile APIs
The system must provide endpoints for creating and updating candidate profiles. Candidates must be able to upload and parse resumes for skills and experience extraction. Profile completeness scoring must be calculated and returned. Employers and recruiters must be able to view candidate profiles within their access scope. Profile visibility controls must be enforced.

### AI Matching APIs
The system must provide endpoints for generating match scores between candidates and jobs. Matching must be multi-dimensional with configurable weights. Match results must include overall score, dimension breakdowns, explanation narrative, referral boost impact, confidence band, and red flags. Matching configuration endpoints must allow admins to adjust weights and thresholds. Candidate-to-job and job-to-candidate matching views must both be supported.

### Referral APIs
The system must provide endpoints for creating and managing referrals. Recruiters must be able to submit candidates for jobs with notes. Candidates must be able to request referrals from recruiters for specific jobs. Referral status tracking through the complete workflow must be supported. Reward calculation and payout tracking endpoints must be provided.

### Application Pipeline APIs
The system must provide endpoints for creating and tracking job applications. Candidates must be able to apply to jobs. Employers must be able to manage application statuses through the hiring pipeline. Disposition reason tracking for rejections must be enforced. Interview scheduling must be supported.

### Engagement and Feedback APIs
The system must provide endpoints for recording work engagements and collecting employer feedback. Engagement records track candidate work history. Feedback collection must capture overall rating, category ratings (technical skills, communication, delivery, professionalism), notes, and outcomes. Feedback must automatically update trust and delivery scores.

### Compliance and Audit APIs
The system must provide endpoints for accessing immutable audit logs. Audit exports must support compliance reporting. Applicant record retention configuration must be manageable. Disposition reason code management must be supported. Compliance reports must be generatable for OFCCP audits.

### Notification APIs
The system must provide endpoints for creating, delivering, and managing notifications. Users must be able to view notifications and mark as read. Notification preference management by type must be supported. Notification templates and frequency management must be available for admins.

### Reporting and Analytics APIs
The system must provide endpoints for KPI data, placement funnel metrics, match quality analytics, recruiter performance metrics, and compliance reports. Analytics must be aggregatable by persona, tenant, and time period. Export capabilities for reports must be provided.

### Document Management APIs
The system must provide endpoints for uploading and managing candidate documents. Resume parsing for skills and experience extraction must be supported. Document verification status tracking must be implemented. Only metadata must be stored for sensitive documents like clearance docs.

## UI Screens & User Flows

### Admin Flows

**Tenant Management Flow:** Admin navigates to tenant management page, views list of all tenants with status and plan details, creates new tenant (employer or recruiter agency), configures SSO provider for tenant, manages tenant status (Active, Suspended, Pending), views tenant details including user count and activity metrics.

**User Management Flow:** Admin navigates to user management page, views list of platform users, assigns personas and permissions, enforces MFA for specific users or globally, sends invitation emails to new users, views user activity and login history.

**Matching Configuration Flow:** Admin navigates to matching config page, views current matching weights and thresholds, adjusts dimension weights within approved limits, sets minimum score thresholds, views matching performance metrics, configures confidence band boundaries.

**Compliance Audit Flow:** Admin navigates to compliance audit page, views audit log with filters by date, actor, action, and outcome, exports audit logs for compliance reporting, configures record retention periods, manages disposition reason codes, generates compliance reports for OFCCP audits.

**Marketplace Governance Flow:** Admin navigates to marketplace page, views flagged jobs and profiles for moderation, views duplicate detection alerts, moderates inappropriate content, views marketplace health metrics.

### Employer Flows

**Job Posting Flow:** Employer navigates to create job page, enters job title and federal contract details (agency, contract vehicle), specifies required clearance level and work arrangement, adds required skills and certifications, sets salary range, marks job as draft, active, paused, or closed, saves job requisition.

**Candidate Search Flow:** Employer navigates to talent marketplace page, applies filters (clearance, location, skills, agency experience), views list of matching candidates with overall scores, clicks candidate to view detailed profile, sees match score breakdown and gaps, sees trust and delivery scores, reviews referral chain if applicable, saves candidate for later review or contacts recruiter.

**Hiring Pipeline Flow:** Employer navigates to pipeline page, views applications in each pipeline stage, drags candidates between stages (Applied → Referred → Screened → Submitted → Interview → Offer → Placed), selects disposition reason when rejecting candidates, views candidate profiles and feedback from previous stages, schedules interviews with selected candidates.

**Recruiter Assignment Flow:** Employer navigates to recruiters page, views assigned recruiters and their performance, assigns specific jobs to recruiters, views recruiter submissions for assigned jobs, provides feedback on recruiter quality.

**Referral Management Flow:** Employer navigates to referrals page, views inbound referrals from recruiters, views referral chains and recruiter credibility, manages reward approval and payouts, views referral performance metrics.

### Candidate Flows

**Profile Building Flow:** Candidate navigates to profile builder page, enters personal information, uploads resume for parsing, adds skills and certifications, enters clearance level and status, adds federal agency experience, views profile completeness score, saves profile.

**Job Search Flow:** Candidate navigates to job marketplace page, applies filters (clearance, location, work arrangement, skills, agency, contract vehicle), views list of matching jobs with match scores, clicks job to view details, sees match explanation and gaps, views recruiter assignments and referral eligibility, applies to job or requests referral.

**Referral Request Flow:** Candidate navigates to referrals page, views assigned recruiters for jobs, requests referral from recruiter with note, tracks referral request status, views referral chain and recruiter credibility.

**Application Tracking Flow:** Candidate navigates to applications page, views all applications with status, clicks application to view details, sees timeline and feedback from employer, views and updates interview schedule.

**Document Management Flow:** Candidate navigates to documents page, uploads resume, uploads certifications, enters clearance document metadata, views verification status for each document, manages document visibility settings.

### Recruiter Flows

**Assigned Jobs Flow:** Recruiter navigates to assigned jobs page, views jobs assigned by employers with deadlines, views job details and required qualifications, views candidate matches for each job.

**Candidate Sourcing Flow:** Recruiter navigates to candidate sourcing page, applies filters (skills, clearance, location, experience), views candidate profiles with fit scores, views candidate engagement history and feedback, saves candidates to lists.

**Match Workspace Flow:** Recruiter navigates to match workspace for specific job, views selected candidate with job requirements, sees match score comparison and explanation, views candidate credentials and engagement history, enters notes on fit, submits candidate to employer or places on hold or declines.

**Referral Request Management Flow:** Recruiter navigates to referrals page, views inbound referral requests from candidates, views candidate profile and job details, accepts or declines referral request, enters notes on decision.

**Submission Tracking Flow:** Recruiter navigates to submissions page, views submitted candidates with status, views employer feedback on submissions, tracks referral status through workflow (Submitted → Interviewed → Offered → Placed), views reward status and amount.

**Rewards Flow:** Recruiter navigates to rewards page, views earned rewards, pending rewards, and payout history, views performance metrics (placement success rate, employer satisfaction), views ranking among recruiters, views payout schedule.

## Authentication & Authorization Requirements

### Authentication Capabilities
The system must support multiple authentication methods. SSO integration must be available via Google Workspace OAuth 2.0/OpenID Connect, Okta OAuth 2.0/SAML 2.0, and Auth0 OAuth 2.0/SAML 2.0. Email and password authentication must also be supported. Multi-factor authentication via TOTP (Time-based One-Time Password) must be enforced as configurable policy. Session management must use JWT access tokens with short expiration (default 15 minutes) and refresh tokens with longer expiration (default 7 days). Session invalidation must occur on logout and password change.

### Authorization Capabilities
Role-based access control must be implemented with four primary personas: admin, employer, candidate, and recruiter. Admin users have platform-wide access. Employer users are scoped to their tenant. Candidate users access their own profile and apply to jobs. Recruiter users see only jobs explicitly assigned to them by employers. Fine-grained permissions within organizations must be supported (post jobs, view candidates, approve payouts, configure matching, etc.). Tenant data isolation must be enforced at the database query layer—all data queries must be scoped by tenant_id for non-admin users.

### Permission Enforcement
API endpoints must enforce appropriate authorization based on resource type and user persona. Admin endpoints require admin persona. Employer endpoints require employer persona and tenant membership. Recruiter endpoints require recruiter persona and job assignment validation. Candidate endpoints primarily access own data with appropriate visibility controls. Public job search endpoints require no authentication but must filter results appropriately.

### Session Security
JWT tokens must be signed with RS256 asymmetric keys. Access tokens must have configurable expiration times. Refresh tokens must rotate on use to prevent replay attacks. Concurrent session limits must be configurable per tenant. IP-based session monitoring must be available for security auditing. Rate limiting must be implemented to prevent brute force attacks.

## External Dependencies

### SSO Identity Providers
The system must integrate with Google Workspace, Okta, and Auth0 for SSO authentication. Integration must support OAuth 2.0 and SAML 2.0 protocols. SSO configuration must be stored per tenant. Token validation, user attribute mapping, and session management must be handled by the backend.

### Email Delivery Services
The system must deliver transactional emails for invitations, notifications, and system communications. GCP Gmail/Workspace API is recommended for integration. SendGrid or SMTP fallbacks must be supported. Email templates must be configurable.

### File Storage Services
Document uploads (resumes, certifications) must be stored in GCP Cloud Storage or equivalent object storage. Storage buckets must have lifecycle policies for document retention. Access controls must ensure candidates can only access their own documents.

### Caching Layer
A caching layer using Memorystore (Redis) or equivalent must be integrated for frequent query results, session data, and match scores. Cache invalidation must occur on data updates. Distributed caching must support multi-pod deployments on GKE.

### Monitoring and Observability Services
The system must integrate with error tracking (Sentry), metrics collection (Prometheus), distributed tracing (OpenTelemetry), and log aggregation (Cloud Logging). Health check endpoints must be available for liveness and readiness probes.

### Compliance Services
No external compliance services are required—compliance is achieved through internal data isolation, audit logging, and retention policies. However, export capabilities for compliance reporting must integrate with standard file storage.

## Non-Functional Requirements

### Performance Requirements
Marketplace search queries must complete within 2 seconds. Match scoring for standard queries must complete within 3 seconds. API response time must be less than 500ms at p95 percentile. The system must support 10,000+ concurrent users. Database queries must be optimized with proper indexing. Frequent queries must be cached with appropriate TTL.

### Scalability Requirements
Horizontal scaling must be supported via GKE autoscaling. The system must support minimum 3 replicas for high availability and scale to 20 replicas under load. Database connection pooling must be implemented. Read replicas must be supported for query offloading. Redis caching must be distributed across pods.

### Availability Requirements
99.9% uptime SLA must be targeted. Multi-zone GKE deployment must be configured for high availability. Cloud SQL high availability with primary and standby must be implemented. Redis Memorystore high availability configuration must be used. Graceful degradation must occur during outages—cache misses must not cause failures.

### Security Requirements
Encryption at rest using AES-256 must be enforced via Cloud SQL. Encryption in transit using TLS 1.3 minimum must be enforced on all endpoints. Secret management via Cloud KMS for encryption keys and Secret Manager for credentials must be implemented. Input validation must use Pydantic v2 schemas for all API inputs. SQL injection prevention must use parameterized queries. XSS protection must use input sanitization. CSRF protection must be stateless with JWT origin validation. Rate limiting via Cloud Armor WAF rules must be implemented per tenant.

### Compliance Requirements
Section 508 WCAG 2.1 AA compliance for web content is frontend responsibility but backend APIs must provide accessibility metadata for documents. OFCCP voluntary self-ID workflows must be supported with separate data storage. OFCCP applicant record retention minimum 2 years must be enforced. OFCCP disposition tracking with required reason codes must be implemented. FAR 52.204-21 contractor information safeguards must be enforced through access controls. NIST 800-63-4 digital identity guidelines must be supported through SSO and MFA. Audit log retention must meet regulatory requirements. Immutable audit logging must be implemented for all actions.

### Observability Requirements
Structured JSON logging must include timestamp, level, request ID, user ID, tenant ID, action, outcome, and details. Log levels must use DEBUG, INFO, WARNING, ERROR, CRITICAL. Sensitive data must be automatically redacted from logs. Prometheus metrics must capture request latency, error rates, active sessions, and queue lengths. Custom business metrics must track match count, referral count, and placement rate. OpenTelemetry distributed tracing must span across service boundaries. Cloud Trace must provide span visualization and latency analysis. Request ID must propagate through system for trace correlation. Sentry must aggregate errors with user, tenant, and request context for alerting. Health check endpoints must include liveness (quick always-200) and readiness (checks database, cache, external dependencies).

### Maintainability Requirements
Clean or hexagonal architecture must be implemented with domain separation. Dependency injection via FastAPI Depends must be used. Repository pattern for data access must be implemented. Service layer for business logic must separate from data models. Domain models must be separate from data models. Comprehensive test coverage of 80% overall with 100% on critical paths must be achieved. API documentation via OpenAPI/Swagger must be automatically generated.

## Acceptance Criteria

### Functional Completeness
All current frontend features must be functional with real backend data. All user stories defined in this specification must be implemented. Business rules must be preserved and enforced in backend logic. Multi-tenant data isolation must be verified through testing. Protected class data must be verified as excluded from matching algorithms.

### Frontend Contract Preservation
All APIs must produce data that matches the TypeScript interfaces defined in `/Users/puttaiaharugunta/codebase/infi/frontend-fleet-fullauto/apps/credmatch/src/types/index.ts`. No transformation layers should be required in the frontend. All enums must be returned with exact string values. All data structures must match field names and types.

### Performance Standards
Marketplace search must respond within 2 seconds for standard queries. Match scoring must complete within 3 seconds. API p95 response time must be less than 500ms. System must support 10,000 concurrent users in load testing. Cache hit rates must be above 80% for frequent queries.

### Security Standards
All OWASP Top 10 vulnerabilities must be prevented through design and testing. Authentication must support all configured SSO providers. MFA must be enforceable as policy. Authorization must prevent cross-tenant data access. Audit logging must capture all actions with immutable records.

### Compliance Standards
OFCCP record retention of 2 years minimum must be enforced. Disposition reasons must be required for all rejections. Audit logs must be exportable for compliance reporting. Self-ID data must be stored separately from matching algorithms. Section 508 accessibility metadata must be provided for documents.

### Test Coverage
Overall test coverage must meet or exceed 80%. Critical business logic (auth, matching, compliance) must have 100% coverage. API routes must have 85% coverage. Repository layer must have 90% coverage. Utilities must have 90% coverage. Integration tests must validate multi-tenant data isolation. E2E tests must cover critical user flows per persona.

### Observability Standards
Structured logs must be automatically aggregated and searchable. Metrics must be available in Grafana dashboards. Distributed traces must be viewable in Cloud Trace. Errors must be aggregated in Sentry with alerting. Health checks must pass liveness and readiness probes.

## Review & Acceptance Checklist

- [ ] This spec contains no technical implementation details (no database schemas, framework choices, or API endpoint designs)
- [ ] All functional requirements are clearly defined from a user perspective
- [ ] Business rules are documented with source file references
- [ ] User stories cover all major use cases and workflows
- [ ] Data entities are described conceptually without technical schema details
- [ ] API requirements describe capabilities, not endpoint designs
- [ ] Non-functional requirements are measurable
- [ ] Acceptance criteria can be verified
- [ ] Spec aligns with constitution.md principles
- [ ] Frontend TypeScript interfaces are referenced as authoritative source of truth for data contracts
- [ ] Multi-tenant isolation requirements are clearly specified
- [ ] Protected class data non-discrimination is explicitly stated
- [ ] Compliance requirements (Section 508, OFCCP, FAR, NIST) are documented
