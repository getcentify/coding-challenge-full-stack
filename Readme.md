# Centify Case Study - Senior Full Stack Developer

## Overview
Build a simplified version of Centify, a commission management and sales performance tracking platform. This case study should take approximately **6 hours** to complete and demonstrate full-stack development capabilities.

## Product Context
Centify helps sales organizations:
- Track deals and sales performance
- Set targets/quotas for sales reps
- Calculate commissions based on deals and incentive plans
- Monitor attainment (performance vs targets)
- Manage payouts

## Scope & Requirements

### Core Features to Implement

#### 1. **Deals Management** (2 hours)
**Backend:**
- REST API endpoints for CRUD operations on deals
- Database schema: `Deal` model with fields:
  - `id`, `name`, `value` (revenue amount), `closeDate`, `ownerId` (employee reference)
  - `organizationId` (for multi-tenancy)
  - `createdAt`, `updatedAt`
- Basic validation and error handling

**Frontend:**
- Deals list page with table view
- Create/Edit deal form (modal or separate page)
- Display: Deal name, value, close date, owner
- Basic filtering by owner
- Responsive design

#### 2. **Employees Management** (1 hour)
**Backend:**
- REST API endpoints for listing employees
- Database schema: `Employee` model with:
  - `id`, `firstName`, `lastName`, `email`
  - `organizationId`
- Simple GET endpoint (no CRUD needed for case study)

**Frontend:**
- Employee dropdown/select in deal form
- Display employee name in deals table

#### 3. **Targets Management** (1.5 hours)
**Backend:**
- REST API endpoints for CRUD operations on targets
- Database schema: `Target` model with:
  - `id`, `name`, `targetValue` (numeric), `startDate`, `endDate`
  - `employeeId` (assigned employee)
  - `organizationId`
- Basic validation (endDate > startDate, targetValue > 0)

**Frontend:**
- Targets list page
- Create/Edit target form
- Display: Target name, value, date range, assigned employee
- Link targets to employees

#### 4. **Attainment Calculation** (1 hour)
**Backend:**
- Calculate attainment percentage for each employee
- Endpoint: `GET /api/org/:orgId/attainments`
- Logic: Sum of deal values for employee in target date range / target value * 100
- Return: `employeeId`, `targetId`, `attainment`, `dealsCount`, `totalDealValue`

**Frontend:**
- Attainment dashboard/table
- Display: Employee name, target name, attainment %, deal count, total revenue
- Visual indicator (color coding: green >100%, yellow 50-100%, red <50%)

#### 5. **Basic Authentication & Multi-tenancy** (0.5 hours)
**Backend:**
- Simple API key or JWT-based auth (can use a mock/simplified approach)
- All endpoints scoped by `organizationId`
- Middleware to extract organization context

**Frontend:**
- Basic login page (can be simplified - just organization ID input)
- Store auth token/org context
- Pass auth headers in API calls

### Technical Requirements

#### Backend Stack
- **Framework**: Express.js or Fastify
- **Database**: PostgreSQL with Prisma ORM (or TypeORM/Sequelize)
- **Language**: TypeScript
- **Validation**: Zod or similar
- **API**: RESTful endpoints with JSON responses

#### Frontend Stack
- **Framework**: React with TypeScript
- **UI Library**: Tailwind CSS (or similar utility-first CSS)
- **State Management**: React Query / TanStack Query (or Context API)
- **Table Component**: TanStack Table (or similar)
- **HTTP Client**: Axios or fetch
- **Build Tool**: Vite or Create React App

#### Database Schema (Simplified)
```prisma
model Organization {
  id        String   @id @default(cuid())
  name      String
  createdAt DateTime @default(now())
  
  employees Employee[]
  deals     Deal[]
  targets   Target[]
}

model Employee {
  id             String   @id @default(cuid())
  firstName      String
  lastName       String
  email          String
  organizationId String
  organization   Organization @relation(fields: [organizationId], references: [id])
  
  ownedDeals Deal[]
  targets    Target[]
  
  createdAt DateTime @default(now())
}

model Deal {
  id             String   @id @default(cuid())
  name           String
  value          Float
  closeDate      DateTime
  ownerId        String
  owner          Employee @relation(fields: [ownerId], references: [id])
  organizationId String
  organization   Organization @relation(fields: [organizationId], references: [id])
  
  createdAt DateTime @default(now())
  updatedAt DateTime @updatedAt
}

model Target {
  id             String   @id @default(cuid())
  name           String
  targetValue    Float
  startDate      DateTime
  endDate        DateTime
  employeeId     String
  employee       Employee @relation(fields: [employeeId], references: [id])
  organizationId String
  organization   Organization @relation(fields: [organizationId], references: [id])
  
  createdAt DateTime @default(now())
  updatedAt DateTime @updatedAt
}
```

### Deliverables

1. **Working Application**
   - Backend API running locally
   - Frontend application running locally
   - Database with seed data (at least 2-3 employees, 5-10 deals, 2-3 targets)

2. **Code Quality**
   - Clean, readable code
   - TypeScript types properly defined
   - Basic error handling
   - Consistent code style

3. **Documentation**
   - README with setup instructions
   - API endpoint documentation (can be inline comments or simple markdown)
   - Brief explanation of architectural decisions

### Evaluation Criteria

1. **Functionality** (40%)
   - All core features implemented and working
   - Data flows correctly between frontend and backend
   - Calculations are accurate

2. **Code Quality** (30%)
   - Clean, maintainable code
   - Proper TypeScript usage
   - Error handling
   - Code organization

3. **User Experience** (20%)
   - Intuitive UI
   - Responsive design
   - Good visual feedback

4. **Technical Decisions** (10%)
   - Appropriate technology choices
   - Database design
   - API design

### Out of Scope (Don't Implement)

- CRM integrations (HubSpot, Salesforce, etc.)
- Complex commission calculations
- Clawbacks and adjustments
- Advanced filtering/search
- Pagination (can use simple lists)
- Real-time updates
- File uploads
- Email notifications
- Complex authentication (OAuth, etc.)

### Time Breakdown Suggestion

- **Setup & Database Schema**: 30 minutes
- **Backend API (Deals, Employees, Targets)**: 2 hours
- **Frontend (Deals & Targets pages)**: 2 hours
- **Attainment Calculation**: 1 hour
- **Polish & Documentation**: 30 minutes

### Getting Started

1. Set up project structure (monorepo or separate repos)
2. Initialize database and run migrations
3. Create seed data
4. Build backend API endpoints
5. Build frontend pages
6. Implement attainment calculation
7. Test end-to-end flow
8. Write documentation

### Bonus Points (Optional)

- Unit tests for critical logic (attainment calculation)
- Docker setup for easy running
- Basic data validation on frontend
- Loading states and error messages
- Date formatting and currency formatting

---

## Notes for Evaluators

This case study focuses on:
- Full-stack development skills
- Understanding of business domain (sales/commission management)
- Database design and relationships
- API design
- Frontend state management
- Basic calculations and data aggregation

The candidate should demonstrate:
- Ability to work with TypeScript
- Understanding of REST APIs
- Database modeling skills
- React best practices
- Time management (6 hours is tight, so prioritization matters)

