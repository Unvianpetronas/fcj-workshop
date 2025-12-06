---
title: "Feedback and Sharing"
date: "December 6, 2025"
weight: 700
chapter: false
pre: " <b> 7. </b> "
---

> Here I would like to share my personal experiences participating in the internship program at AWS Vietnam, along with feedback to help improve the program for future interns.

---

## General Evaluation

### 1. Work Environment

The work environment at AWS Vietnam is extremely professional yet very friendly and supportive. What impressed me most is the "learn by doing" culture - I was trusted to independently design and deploy a real production project (AWS Cloud Health Dashboard) instead of just doing small, scattered tasks.

**Strengths:**
- **Creative freedom:** I was given the autonomy to make architectural decisions, choose technologies, and implement in the way I thought best, then receive feedback for improvement
- **Complete tools:** Provided with full AWS resources, official technical documentation, and access to necessary cloud services
- **Learning space:** Everyone always encouraged me to experiment, even allowing me to "break things" to learn from mistakes
- **Open communication:** Could freely discuss technical decisions without having solutions imposed from above

### 2. Support from Mentor / Team Admin

My mentor was truly excellent at balancing between guidance and letting me explore independently. Instead of providing direct answers, the mentor often asked counter-questions that made me think more deeply about the problem.

### 3. Alignment Between Work and Academic Major

The AWS Cloud Health Dashboard project was completely aligned with and truly enhanced what I learned in school.

**Knowledge from school applied:**
- **Database:** Applied NoSQL knowledge to design optimized DynamoDB schema for multi-tenant architecture
- **Web Programming:** Used knowledge of REST APIs, HTTP methods, authentication to build FastAPI backend
- **Information Security:** Applied encryption, hashing, JWT tokens - concepts learned in Security courses
- **Software Architecture:** Designed distributed systems, microservices thinking (although not full microservices yet)

**New knowledge learned beyond school:**
- **Cloud-native architecture:** How to design applications that fully leverage cloud capabilities
- **DevSecOps practices:** CI/CD pipeline, infrastructure as code, automated testing
- **Production debugging:** How to debug issues in real production environments
- **Cost optimization:** Balancing performance and cost - not taught in school
- **Multi-tenant security:** Isolation, data privacy, cross-account access - very practical

**Ideal ratio:**
- 40% foundational knowledge from school
- 60% new, practical, production-grade knowledge

This is the ideal ratio that kept me from being overwhelmed while still learning many new things.

### 4. Learning Opportunities & Skill Development

This is the aspect I valued most in the internship program.

**Technical Skills:**

*AWS Services Deep Dive:*
- **EC2:** Not just start/stop instances but understanding instance types, pricing models, placement strategies
- **DynamoDB:** Partition key design, GSI/LSI, query optimization, capacity planning
- **IAM:** Cross-account roles, trust relationships, least privilege principle
- **GuardDuty:** Security findings interpretation, threat detection
- **Cost Explorer:** API usage optimization, cost allocation tags

*Development Skills:*
- **Python/FastAPI:** Async programming, dependency injection, middleware, error handling
- **React:** Hooks, state management, component architecture, performance optimization
- **Git workflow:** Branch strategies, meaningful commits, code review process

*DevOps/Cloud Skills:*
- **CI/CD:** AWS CodePipeline, CodeBuild, automated deployment
- **Monitoring:** CloudWatch metrics, alarms, log analysis
- **Security:** Encryption (Fernet, KMS), secrets management, vulnerability scanning

**Soft Skills:**

*Problem Solving:*
I learned how to approach a complex problem:
1. Understand the problem clearly (e.g., costs too high)
2. Gather data (check API calls, calculate cost per call)
3. Analyze root cause (calling API too frequently)
4. Propose multiple solutions (caching, alternative data sources, longer intervals)
5. Evaluate trade-offs (freshness vs cost)
6. Implement & monitor

*Technical Communication:*
- Write clear documentation (README, architecture docs, API specs)
- Explain technical decisions (why choose DynamoDB instead of RDS?)
- Present findings (cost analysis report, security audit results)

*Time Management:*
- Balance multiple tasks (bug fixes, new features, documentation)
- Prioritize based on impact (security bugs > new features)
- Set realistic timelines

---

## Additional Questions

### What do you think the company **needs to improve** for future interns?

**I think we need real-world use cases so everyone can visualize what a real project looks like, and can apply it to their projects, including security and optimizing both performance and cost.**

### If introducing to friends, would you **recommend they intern here**? Why?

**Yes, I would strongly recommend - but with some caveats.**

**Who I would recommend:**

**1. Students who want to REALLY learn, not just "build a resume"**

If you just want:
- FCJ name on CV
- Easy, relaxed work
- 9-to-5 then go home

→ This is NOT the place for you.

But if you:
- Want to build something real and meaningful
- Are ready to face challenges
- Want to learn by doing, not afraid to "break things"
- Are passionate about cloud/AWS/technology

→ This is the PERFECT place for you.

**2. Students with good foundation or fast learners**

To succeed here, you need:
- **Programming fundamentals:** Python or at least 1 backend language
- **Basic web development:** Understand HTTP, REST APIs, databases
- **Willingness to learn:** AWS has a learning curve, must be ready to read lots of docs
- **Problem-solving mindset:** Not afraid to Google, debug, trial-and-error

I wasn't great at programming when I started, but I was willing to learn → that's what matters most.

**Reasons to recommend:**

**Technical Growth:**
- 📈 Learn production-grade AWS architecture
- 📈 Hands-on with 17+ AWS services
- 📈 Real DevSecOps experience
- 📈 Portfolio project you can show employers

**Soft Skills:**
- 💪 Decision-making under uncertainty
- 💪 Technical communication
- 💪 Problem-solving methodology
- 💪 Ownership mindset

**Career:**
- 🚀 AWS experience on CV (very valuable!)
- 🚀 Real production project to discuss in interviews
- 🚀 Understanding of cloud economics (cost optimization)
- 🚀 Network with AWS professionals

**Culture:**
- ❤️ Supportive mentorship
- ❤️ Safe environment to fail and learn
- ❤️ Meritocracy (good work gets recognized)
- ❤️ Innovation encouraged

---

## Suggestions & Aspirations

### Do you have any suggestions to improve the internship experience?

*Suggestion: Internal Knowledge Base*
- **Content needed:**
    - Common architectural patterns used in the team
    - Best practices for AWS services commonly used by the team
    - Troubleshooting guides (what problems encountered, how to fix)
    - Code examples and templates
- **Format:** Internal wiki or Git repo
- **Maintenance:** Each intern contributes when they solve a problem

### Would you like to continue this program in the future?

**Yes, I would very much like to continue with FCJ until the end of the program.**