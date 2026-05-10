# ⚡ InternPulse — Full Stack Internship Platform

India's most affordable internship platform with cycle-based learning, progressive refunds, and real-world project experience.

## 🏗️ Tech Stack

| Layer | Technology |
|-------|-----------|
| Frontend | Next.js 14, React 18, TypeScript, Tailwind CSS |
| Backend | Next.js API Routes (serverless) |
| Database | Supabase PostgreSQL |
| Auth | Supabase Auth + Custom OTP |
| Payments | Razorpay |
| AI | OpenAI API (chatbot) |
| Email | Resend |
| Hosting | Vercel (frontend) + Supabase (backend) |

## 📁 Project Structure

```
internpulse/
├── public/                     # Static assets
├── supabase/
│   ├── schema.sql              # Complete database schema (run first!)
│   └── seed.sql                # Sample data (run second!)
├── src/
│   ├── app/
│   │   ├── page.tsx            # Landing page
│   │   ├── layout.tsx          # Root layout
│   │   ├── globals.css         # Global styles
│   │   ├── auth/
│   │   │   ├── actions.ts      # Server actions (OTP, signup)
│   │   │   ├── signin/page.tsx # Sign in page
│   │   │   └── signup/page.tsx # Multi-step signup
│   │   ├── (dashboard)/
│   │   │   ├── student/        # Student dashboard & pages
│   │   │   ├── mentor/         # Mentor dashboard & pages
│   │   │   └── admin/          # Admin dashboard & pages
│   │   ├── internships/
│   │   │   └── [slug]/         # Individual internship pages
│   │   └── api/
│   │       ├── payments/       # Razorpay integration
│   │       ├── certificates/   # Certificate CRUD & verification
│   │       ├── chat/           # AI chatbot
│   │       ├── internships/    # Internship CRUD
│   │       ├── users/          # User management
│   │       └── assessments/    # Coding tests & MCQs
│   ├── components/
│   │   ├── layout/             # Navbar, Footer
│   │   ├── landing/            # All landing page sections
│   │   ├── dashboard/          # Dashboard layout
│   │   └── ui/                 # Shared UI (ChatWidget, etc.)
│   ├── lib/
│   │   ├── supabase/           # Supabase clients (browser/server)
│   │   ├── constants.ts        # App constants, nav links, etc.
│   │   └── utils.ts            # Utility functions
│   ├── hooks/                  # Custom React hooks
│   ├── types/                  # TypeScript type definitions
│   └── middleware.ts           # Auth middleware
├── .env.example                # Environment variables template
├── package.json
├── tailwind.config.ts
├── tsconfig.json
└── next.config.js
```

## 🚀 Deployment Guide (Step by Step)

### Step 1: Set Up Supabase

1. Go to [supabase.com](https://supabase.com) and create a free account
2. Create a new project (choose a strong database password!)
3. Wait for the project to be ready (~2 minutes)
4. Go to **SQL Editor** → paste the contents of `supabase/schema.sql` → click **Run**
5. Then paste `supabase/seed.sql` → click **Run**
6. Go to **Settings** → **API** and copy:
   - Project URL → `NEXT_PUBLIC_SUPABASE_URL`
   - Anon public key → `NEXT_PUBLIC_SUPABASE_ANON_KEY`
   - Service role key → `SUPABASE_SERVICE_ROLE_KEY`

#### Create Storage Buckets
Go to **Storage** and create these buckets:
- `avatars` (Public)
- `resumes` (Private)
- `submissions` (Private)
- `certificates` (Public)
- `content` (Public)

### Step 2: Set Up Razorpay

1. Go to [razorpay.com](https://razorpay.com) and create a test account
2. Go to **Settings** → **API Keys** → Generate Test Keys
3. Copy Key ID → `RAZORPAY_KEY_ID` and `NEXT_PUBLIC_RAZORPAY_KEY_ID`
4. Copy Key Secret → `RAZORPAY_KEY_SECRET`

### Step 3: Set Up Resend (Email)

1. Go to [resend.com](https://resend.com) and create a free account (100 emails/day free)
2. Create an API key → `RESEND_API_KEY`
3. Add and verify your domain (or use the test domain for development)

### Step 4: Set Up OpenAI (Optional — for chatbot)

1. Go to [platform.openai.com](https://platform.openai.com)
2. Create an API key → `OPENAI_API_KEY`
3. Add $5 credit (lasts a long time with GPT-3.5-turbo)

### Step 5: Local Development

```bash
# Clone the project
git clone <your-repo-url>
cd internpulse

# Install dependencies
npm install

# Copy env file and fill in your values
cp .env.example .env.local

# Run development server
npm run dev
```

Open [http://localhost:3000](http://localhost:3000)

### Step 6: Deploy to Vercel

1. Push code to GitHub
2. Go to [vercel.com](https://vercel.com) → Import repository
3. Add all environment variables from `.env.example`
4. Deploy!

Your site will be live at `your-project.vercel.app`

### Step 7: Connect Custom Domain

1. Buy a domain (e.g., `internpulse.in` from GoDaddy/Namecheap — ~₹500/year)
2. In Vercel → Settings → Domains → Add your domain
3. Update DNS records as Vercel instructs
4. Update `NEXT_PUBLIC_APP_URL` in environment variables

## 🔧 Things You'll Need to Do Manually

### 1. Create Admin Account
After deployment, sign up normally then run this in Supabase SQL Editor:
```sql
UPDATE public.profiles SET role = 'admin' WHERE email = 'your-email@example.com';
```

### 2. Add Real Internship Content
Use the Admin Dashboard to:
- Create/edit internship tracks
- Add cycle modules and lessons
- Upload video content to Supabase Storage
- Create coding questions and MCQs

### 3. Set Up Razorpay Webhook
In Razorpay Dashboard → Webhooks:
- URL: `https://your-domain.com/api/payments/webhook`
- Events: `payment.captured`, `payment.failed`, `refund.processed`

### 4. Email Domain Verification
For production emails, verify your domain in Resend dashboard.

## 💰 Monthly Cost Breakdown (First 1000 Users)

| Service | Free Tier | When to Upgrade |
|---------|-----------|-----------------|
| Supabase | 500MB DB, 1GB storage, 50K auth users | > 500MB data |
| Vercel | 100GB bandwidth, serverless functions | > 100GB/month |
| Resend | 100 emails/day | > 3000 emails/month (₹1500/mo) |
| Razorpay | 2% per transaction | Volume discounts available |
| OpenAI | Pay per use (~$5 for thousands of chats) | N/A |
| Domain | ~₹500/year | N/A |

**Total monthly cost for first 1000 users: ₹0 - ₹500** (excluding domain and payment gateway fees)

## 🔐 Security Checklist

- [x] Row Level Security (RLS) on all tables
- [x] Server-side authentication checks on all API routes
- [x] Role-based access control (student/mentor/admin)
- [x] Razorpay signature verification on payments
- [x] OTP-based email verification
- [x] Input validation with Zod
- [x] CORS and rate limiting via Vercel/Supabase
- [x] Environment variables for all secrets

## 📈 Scaling Later

When you grow beyond free tiers:
1. **Supabase Pro** ($25/mo) — 8GB DB, 100GB storage
2. **Vercel Pro** ($20/mo) — more bandwidth and functions
3. **Resend Growth** ($20/mo) — 50K emails/month
4. Consider **Redis** for caching (Upstash free tier)
5. Consider **Vercel Edge Functions** for global performance
6. # ⚡ InternPulse — Full Stack Internship Platform

> AI-powered internship platform built by students for students.

🌐 **Live Demo:** https://internpulse-fullstack.vercel.app

## 🤝 Need Help?

This codebase is designed to be extended. Key areas you might want to add:
- Video player integration (YouTube embeds or Mux)
- WebRTC for live interviews (use Daily.co or LiveKit free tier)
- More detailed analytics with Mixpanel/PostHog
- Push notifications
- Mobile app with React Native

---

Built with ❤️ for InternPulse
