# Product Requirements Document (PRD)

## Project Name

WhatsApp Clone Frontend Application

## Version

2.0 – Final Consolidated Version

## Date

2025-06-22

## Author

sabriboulahia2011-alt (ss bb work)

## Participants

- Product Owner: sabriboulahia2011-alt
- Lead Developer: [TBD]
- UI/UX Designer: [TBD]
- QA Lead: [TBD]
- Stakeholders: [TBD]

## Status

On target

## Target Release

2026-08-01

---

## 1. Purpose and Background

The goal of this project is to create a feature-rich, robust, and scalable WhatsApp clone frontend application, leveraging modern technologies and best practices. The app should provide a seamless user experience, real-time messaging, media sharing, group chats, and a familiar UI/UX akin to WhatsApp. The backend will be assumed to use technologies compatible with Prisma and PostgreSQL.

**Strategic Fit:**  
This project serves as a foundation for messaging platforms, enabling rapid prototyping and customization for various business or open-source needs.

---

## 2. Team Goals and Business Objectives

- Deliver a production-ready, open-source chat application with a familiar WhatsApp-like experience.
- Provide a customizable codebase for developers and organizations.
- Ensure high performance, security, and accessibility.
- Facilitate easy onboarding and documentation for contributors.

---

## 3. Scope

- Build a high-fidelity, production-ready WhatsApp clone frontend.
- Integrate with a backend API (REST or GraphQL) for authentication, messaging, and media.
- Support real-time communications using WebSockets or a similar solution.
- Implement responsive design for mobile and desktop.
- Include advanced UX features like typing indicators, message reactions, and read receipts.
- Use Prisma as the ORM for database access (schema and integration details provided).
- Provide clear documentation and developer onboarding.

---

## 4. Assumptions

- Backend API will be available and compatible with the defined schema.
- Users will primarily access the app via modern browsers.
- Push notification services are available for web and mobile.
- End-to-end encryption is supported by the backend.

---

## 5. Target Users

- End-users seeking a WhatsApp-like communication experience.
- Developers or companies looking for a customizable, open-source chat app foundation.
- QA and product teams involved in messaging platform benchmarking.

---

## 6. Key Features

### 6.1 Authentication & User Management

- Sign-up/in with email and password (OAuth, phone number optional).
- JWT-based or session authentication.
- User profile management (avatar, name, status).
- Password reset and email verification.

### 6.2 Chat Experience

- One-to-one and group chats.
- Real-time messaging with WebSocket support.
- Message delivery/read receipts.
- Typing indicators.
- Message reactions (emoji support).
- Starred/favorite messages.
- Searchable chat history.

### 6.3 Media & File Sharing

- Image/file upload and sharing in chats.
- Audio/video message support.
- Preview of shared media in the chat window.

### 6.4 Notifications

- Push notifications (web and mobile).
- In-app notifications for new messages, mentions.

### 6.5 UI/UX

- Responsive layout for mobile/desktop/tablets.
- Theme support (light/dark mode).
- WhatsApp-inspired design system.
- Accessibility (WCAG 2.1 compliance).

### 6.6 Settings & Preferences

- Mute/block users and groups.
- Chat background customization.
- Privacy settings (last seen, read receipts).

---

## 7. User Stories

- As a user, I want to sign up and log in securely so that my messages are private.
- As a user, I want to send and receive messages in real time.
- As a user, I want to share images and files in chat.
- As a user, I want to join and create group chats.
- As a user, I want to customize my chat background and theme.

// (Link to full user story list in issue tracker or documentation.)

---

## 8. User Interaction and Design

- [Insert Figma/Sketch/Design links here]
- Wireframes and mockups for main screens (login, chat, group chat, settings, etc.)
- Accessibility considerations and color contrast checks.

---

## 9. Technical Requirements

### 9.1 Tech Stack

- **Frontend:** Next.js (15+), React (19+), TypeScript (5+)
- **State Management & Database:**
  - Zustand: ^5.x (global state)
  - React Query/TanStack Query: server state/caching
  - Prisma: ORM for PostgreSQL
- **Styling:** Tailwind CSS (3.x), CSS Modules
- **Animation:** Framer Motion, Lottie
- **Testing:** Jest, React Testing Library, Playwright (E2E)
- **Tooling:** ESLint, Prettier, Husky, Commitlint
- **API:** REST/GraphQL clients (Axios/urql)
- **Real-time:** Socket.IO or native WebSockets
- **CI/CD:** GitHub Actions

### 9.2 Database Schema (Prisma)

```prisma
// schema.prisma
generator client {
  provider = "prisma-client-js"
}

datasource db {
  provider = "postgresql"
  url      = env("DATABASE_URL")
}

model User {
  id         String   @id @default(uuid())
  email      String   @unique
  name       String?
  avatar     String?
  phone      String?
  status     String?
  chats      ChatUser[]
  messages   Message[]
  createdAt  DateTime @default(now())
  updatedAt  DateTime @updatedAt
}

model Chat {
  id         String     @id @default(uuid())
  name       String?
  isGroup    Boolean    @default(false)
  users      ChatUser[]
  messages   Message[]
  createdAt  DateTime   @default(now())
  updatedAt  DateTime   @updatedAt
}

model ChatUser {
  id      String  @id @default(uuid())
  chat    Chat    @relation(fields: [chatId], references: [id])
  chatId  String
  user    User    @relation(fields: [userId], references: [id])
  userId  String
  role    String  // "admin" or "member"
}

model Message {
  id        String   @id @default(uuid())
  content   String?
  type      String   // text, image, video, file
  chat      Chat     @relation(fields: [chatId], references: [id])
  chatId    String
  sender    User     @relation(fields: [senderId], references: [id])
  senderId  String
  createdAt DateTime @default(now())
  updatedAt DateTime @updatedAt
  isRead    Boolean  @default(false)
  reactions Json?
}
```

### 9.3 TypeScript Interfaces

```typescript
// User Object
interface User {
  id: string;
  email: string;
  name: string;
  avatar: string | null;
  role: 'standard' | 'premium' | 'admin';
  status?: string;
  createdAt: string;
  updatedAt: string;
}

// Chat Object
interface Chat {
  id: string;
  name?: string;
  isGroup: boolean;
  users: User[];
  messages: Message[];
  createdAt: string;
  updatedAt: string;
}

// Message Object
interface Message {
  id: string;
  content?: string;
  type: 'text' | 'image' | 'video' | 'file';
  chatId: string;
  senderId: string;
  sender: User;
  createdAt: string;
  updatedAt: string;
  isRead: boolean;
  reactions?: Record<string, string[]>;
}
```

---

## 10. Project Structure

```text
/whatsapp-clone
│
├── public/                   # Static files (images, icons, etc.)
├── src/
│   ├── components/           # UI components
│   ├── pages/                # Next.js pages/routes
│   ├── hooks/                # Custom React hooks
│   ├── lib/
│   │   ├── auth.ts           # Authentication utilities
│   │   ├── user.ts           # User utilities
│   │   ├── prisma.ts         # Prisma-generated types
│   │   └── api.ts            # API clients (REST/GraphQL)
│   ├── prisma/
│   │   └── schema.prisma     # Prisma database schema
│   ├── styles/               # Tailwind & global styles
│   ├── utils/                # Helper functions
│   └── tests/                # Unit and integration tests
├── docs/
│   ├── API.md
│   ├── DATABASE.md           # Database schema and migrations
│   └── DEPLOYMENT.md         # Deployment instructions
├── .env.example              # Environment variables template
├── .env.local                # Local development environment
├── package.json
├── tsconfig.json
├── README.md
└── Dockerfile
```

---

## 11. Security & Privacy

- End-to-end encryption for messages (if backend supports).
- Secure authentication (JWT, OAuth, HTTPS).
- Input validation and sanitization.
- GDPR compliance and privacy notice.

---

## 12. DevOps & Deployment

### 12.1 Database & Docker Configuration

#### Environment Variables

```bash
# .env.example
DATABASE_URL="postgresql://username:password@localhost:5432/whatsapp_clone"
NEXTAUTH_URL="http://localhost:3000"
JWT_SECRET="your_jwt_secret"
```

#### Dockerfile

```dockerfile
FROM node:18-alpine AS builder
WORKDIR /app
COPY package*.json ./
COPY prisma ./prisma/
RUN npm ci --only=production
RUN npx prisma generate
COPY . .
RUN npm run build

FROM node:18-alpine AS runner
WORKDIR /app
ENV NODE_ENV=production
COPY --from=builder /app ./
EXPOSE 3000
CMD ["npm", "start"]
```

---

## 13. Documentation

- All components, hooks, and utils should have JSDoc comments.
- API integration documented in `/docs/API.md`.
- Setup and deployment steps in `/docs/DEPLOYMENT.md`.
- Database schema in `/docs/DATABASE.md`.

---

## 14. Questions/Decisions

| Question/Decision | Status | Owner | Notes |
|-------------------|--------|-------|-------|
| Will we support phone number authentication in v2.0? | Open | Product Owner | TBD |
| Will the backend support end-to-end encryption? | Open | Backend Lead | TBD |
| What is the minimum browser support? | Open | Dev Lead | TBD |

---

## 15. Out of Scope

- Native mobile apps (iOS/Android) – web responsive only.
- Voice/video calling (future roadmap).
- Payment or monetization features.

---

## 16. What We're Not Doing

- No support for legacy browsers (IE11, etc.).
- No integration with third-party chat platforms.
- No built-in analytics or tracking (future consideration).

---

## 17. Appendix

- Contributors: sabriboulahia2011-alt (`w98852608@gmail.com`)
- All user and project management data is in `users.json` and `projects.json` (currently empty).

---

## 18. References

- [Atlassian: How to create a product requirements document (PRD)](https://www.atlassian.com/agile/product-management/requirements)
- [Claude conversation context](https://claude.ai/share/f47e6f86-f1c6-4734-bb8b-3f3e75f7eba9)
- Repo JSON files: [`conversations.json`](https://github.com/sabriboulahia2011-alt/claude/blob/main/conversations.json), [`users.json`](https://github.com/sabriboulahia2011-alt/claude/blob/main/users.json), [`projects.json`](https://github.com/sabriboulahia2011-alt/claude/blob/main/projects.json)

---

## 19. Warnings & Notes

- ⚠️ **Always use the latest stable versions of all packages and consult their official documentation to ensure best practices and ongoing improvements.**

---

## 20. Project Handoff & Outgoing Status

This project is now outgoing and ready for external use, review, and contribution. All requirements, technical details, and project scope are defined in this PRD. Please refer to this document as the source of truth for onboarding, development, and deployment.
