---
title: "Event 1"
date: 2024-01-01T00:00:00+07:00
weight: 1
chapter: false
pre: " <b> 4.1. </b> "
---

{{% notice warning %}}
⚠️ **Note:** The information below is for reference purposes only. Please **do not copy it verbatim** into your report, including this warning.
{{% /notice %}}

# Summary Report: "AWS Community Day Vietnam 2025"

**Venue:** Saigon Exhibition and Convention Center (SECC), Ho Chi Minh City  
**Time:** 09:00 – 17:00, Saturday, September 20, 2025  
**Organizers:** AWS User Group Vietnam, First Cloud Journey  
**Coordinators:** Phuong Nguyen, Minh Tran, Khanh Le

### Event Objectives

- Connect AWS community members across Vietnam
- Share real-world experiences with AWS cloud services
- Introduce new AWS features and best practices
- Provide hands-on workshops for beginners and intermediate users
- Foster knowledge sharing among developers, architects, and cloud enthusiasts

### Speakers

- **Nguyen Van Thanh** – Cloud Solutions Architect, AWS Vietnam
- **Tran Thi Lan** – Senior DevOps Engineer, VNG Corporation
- **Le Quoc Hung** – CTO, CloudTech Solutions
- **Pham Minh Duc** – AWS Community Hero

### Key Highlights

#### Identifying the drawbacks of legacy application architecture

- Long product release cycles → Lost revenue/missed opportunities  
- Inefficient operations → Reduced productivity, higher costs  
- Non-compliance with security regulations → Security breaches, loss of reputation  

#### Transitioning to modern application architecture – Microservices

Migrating to a modular system — each function is an **independent service** communicating via **events**, built on three core pillars:

- **Queue Management**: Handle asynchronous tasks  
- **Caching Strategy**: Optimize performance  
- **Message Handling**: Flexible inter-service communication  

#### Domain-Driven Design (DDD)

- **Four-step method**: Identify domain events → arrange timeline → identify actors → define bounded contexts  
- **Bookstore case study**: Demonstrates real-world DDD application  
- **Context mapping**: 7 patterns for integrating bounded contexts  

#### Event-Driven Architecture

- **3 integration patterns**: Publish/Subscribe, Point-to-point, Streaming  
- **Benefits**: Loose coupling, scalability, resilience  
- **Sync vs async comparison**: Understanding the trade-offs  

#### Compute Evolution

- **Shared Responsibility Model**: EC2 → ECS → Fargate → Lambda  
- **Serverless benefits**: No server management, auto-scaling, pay-for-value  
- **Functions vs Containers**: Criteria for appropriate choice  

#### Amazon Q Developer

- **SDLC automation**: From planning to maintenance  
- **Code transformation**: Java upgrade, .NET modernization  
- **AWS Transform agents**: VMware, Mainframe, .NET migration  

### Key Takeaways

#### Design Mindset

- **Business-first approach**: Always start from the business domain, not the technology  
- **Ubiquitous language**: Importance of a shared vocabulary between business and tech teams  
- **Bounded contexts**: Identifying and managing complexity in large systems  

#### Technical Architecture

- **Event storming technique**: Practical method for modeling business processes  
- Use **event-driven communication** instead of synchronous calls  
- **Integration patterns**: When to use sync, async, pub/sub, streaming  
- **Compute spectrum**: Criteria for choosing between VM, containers, and serverless  

#### Modernization Strategy

- **Phased approach**: No rushing — follow a clear roadmap  
- **7Rs framework**: Multiple modernization paths depending on the application  
- **ROI measurement**: Cost reduction + business agility  

### Applying to Work

- **Apply DDD** to current projects: Event storming sessions with business teams  
- **Refactor microservices**: Use bounded contexts to define service boundaries  
- **Implement event-driven patterns**: Replace some sync calls with async messaging  
- **Adopt serverless**: Pilot AWS Lambda for suitable use cases  
- **Try Amazon Q Developer**: Integrate into the dev workflow to boost productivity  

### Event Experience

Attending the **“GenAI-powered App-DB Modernization”** workshop was extremely valuable, giving me a comprehensive view of modernizing applications and databases using advanced methods and tools. Key experiences included:

#### Learning from Experienced Practitioners
- Speakers shared practical insights from real production environments, not just theoretical concepts
- Hearing about actual challenges and solutions from VNG Corporation and other companies was particularly valuable
- AWS Community Heroes provided mentorship and career guidance for aspiring cloud engineers

#### Hands-on Workshop Experience
- Building a complete web application from scratch reinforced my understanding of AWS services
- Troubleshooting issues during the workshop helped me learn debugging techniques
- Seeing how different AWS services integrate (VPC, EC2, RDS, ALB) gave me a holistic view
- Workshop materials were well-organized and easy to follow

#### Understanding Real-world Architecture
- The e-commerce demo with 10,000+ concurrent users showed scalability in action
- Learning about Auto Scaling policies helped me understand how applications handle traffic spikes
- Security session highlighted mistakes I should avoid in my own projects
- CI/CD demos illustrated modern software delivery practices

#### Networking with Community
- Met other students and junior developers passionate about cloud computing
- Exchanged contacts with experienced engineers who offered to help with learning questions
- Discovered local AWS User Group meetups for continued learning
- Connected with FCJ members and discussed internship experiences

#### Inspiration and Motivation
- Seeing Vietnamese companies successfully using AWS was inspiring
- Realized cloud engineering is a viable career path with growing opportunities
- Understood that continuous learning is essential in cloud technology
- Motivated to pursue AWS certifications after gaining more hands-on experience

#### Practical Skills Gained
- Configured VPC networking including subnets, route tables, and internet gateway
- Launched EC2 instances with proper security group configurations
- Set up RDS database with appropriate backup and maintenance windows
- Deployed applications behind Application Load Balancer
- Implemented basic Auto Scaling policies

#### Some event photos
*Event photos would be added here*  

> Overall, AWS Community Day Vietnam 2025 was more than just a learning event – it was an opportunity to immerse myself in the AWS ecosystem, connect with passionate community members, and gain practical knowledge that directly applies to my internship and future career.
