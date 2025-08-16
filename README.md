# JobSethu-API.Alliance

Job Sethu is a full-stack web application designed as a hyper-local marketplace to connect individuals seeking short-term work (gigs) with those offering it within their community. Powered by AI, the platform      simplifies the job posting and application process, making it easier for local talent like students and freelancers to find flexible work, and for residents to get help with tasks like tutoring, gardening, or event support.

## Team Members
1) Ajeet Singh - https://github.com/ajeetsingh1308
2) Akshay Koujalgi - https://github.com/akshayk2004
3) Pranav Kokitkar - https://github.com/PranavKokitkar21
4) Shubham Galve - https://github.com/Shubhamgalave
5) Vedanth Bandodkar - https://github.com/vedanthbandodkar

## 💻 Tech Stack

The application is built using a **hybrid architecture** that balances rapid development with flexibility for custom features and AI integration.

### Frontend
- **React (Web)**: Provides a fast, component-driven approach to building dynamic user interfaces.  
- **React Native (Mobile App)**: Enables a native-like mobile experience with maximum code reuse from the web frontend.  
- **TypeScript**: Strongly-typed language that reduces runtime errors, improves maintainability, and makes the codebase scalable.  
- **Tailwind CSS**: Utility-first CSS framework for rapid styling, ensuring responsive and consistent UI across platforms.  
- **Headless UI / Radix UI**: Unstyled, accessible components that give the team complete control over custom design while maintaining usability.

### Backend & Database
- **Supabase**: A modern backend-as-a-service that provides managed PostgreSQL, authentication, storage, real-time APIs, and more—removing boilerplate backend work.  
  - **Database:** PostgreSQL with **PostGIS** extension for location-based queries, enabling hyper-local job discovery.  
  - **Authentication:** Supabase Auth manages sign-up, login, and secure sessions out of the box.  
  - **Realtime:** Enables live job feeds, chat updates, and notification features.  
  - **Storage:** Secure file storage for user profile images and job-related uploads.  
- **Node.js Serverless Functions**: Handles complex, custom business logic and external API integrations in an efficient, scalable manner.

### Artificial Intelligence
- **Google Genkit with Gemini Models**: Powers the AI layer for natural language and image generation.  
  - **Job Suggestions:** Auto-generate structured job descriptions from short titles.  
  - **AI Reply Assistance:** Suggest context-aware, professional responses in chat.  
  - **Job Image Creation:** Generate unique visuals for job postings, enhancing engagement.

### DevOps & Deployment
- **Version Control:** Git + GitHub for collaboration and code management.  
- **CI/CD Pipeline:** TBD (GitHub Actions / Vercel / Netlify / Docker could be considered).  
- **Hosting & Deployment:** TBD, depending on cost, scalability, and team preference (e.g., Vercel for frontend, Supabase for backend, AWS/GCP for serverless functions).  

---
