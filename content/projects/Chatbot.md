---
title: AI Chatbot Integration on my Portfolio
description: Interactive assistant for portfolio exploration
authors:
  - name: Pierre Graef
    to: null
    avatar:
      src: https://i.pravatar.cc/128?u=2
image:
  src: /img/Article IA Chatbot/article_chatbot.png
date: 2024-10-31T01:00:00.000Z
---

Have you noticed the little green tab in the bottom right corner? It’s my chatbot, and I invite you to give it a try before diving into this article!

Looking to create a personal AI assistant to engage visitors on your website? In this article, I’ll walk you through the process of designing, developing, and deploying an interactive chatbot on my portfolio site. Let’s dive into the creation of a seamless AI experience!

![article\_chatbot.png](/img/Article%20IA%20Chatbot/article_chatbot.png)

Creating an **AI Chatbot** for my portfolio site was a strategic move to offer visitors a guided and interactive experience. Using Botpress, I designed and coded a custom chatbot that can answer common questions about my profile, projects, and career background. Here’s a step-by-step breakdown of how I brought this idea to life.

## Concept and Design

The idea was to create a chatbot that is both helpful and conversational. My goal was for visitors to feel as though they were having a real-time conversation with an assistant. The design phase included:

- **Defining Objectives**: The chatbot should answer questions about my experience, projects, and contact details.
- **Planning Interactions**: I sketched out common user flows, like asking about my educational background, project details, or technical skills.
- **User Interface (UI)**: Opted for a minimalistic chatbot interface that doesn’t distract but complements the portfolio’s aesthetics.

## Development and Botpress Integration

Using **Botpress** as the platform for building the chatbot was an intuitive choice due to its ease of use and flexibility. Here’s how I set up the chatbot:

1. **Setting up the Workflow**: I created workflows for different scenarios, such as providing my educational background or explaining a specific project.
2. **Intent and Entity Training**: I trained the chatbot on various intents like “What is your experience?” or “Show me your projects.” This helps the bot respond accurately to different user queries.
3. **Knowledge Base Integration**: By connecting the bot to a knowledge base, I ensured that it could answer questions based on pre-defined information about my profile and skills.

## Deployment and Testing

Before deploying, I thoroughly tested the chatbot to make sure it provided clear and relevant answers. Key deployment steps included:

- **Embedding the Chatbot**: The Botpress-generated script was added to my site’s main layout file, ensuring it loads on every page.
- **Testing**: I tested the chatbot in both desktop and mobile views to make sure the experience was smooth and responsive.
- **Refinement**: Based on feedback, I made improvements to the bot’s responses and handling of unexpected questions.

## Features and Capabilities

The chatbot is designed to provide:

- **Career Information**: Users can inquire about my academic background, professional skills, and certifications.
- **Project Insights**: The bot can give a summary of major projects and guide users to relevant project pages.
- **Personalized Assistance**: For specific queries, the chatbot can offer my contact information or link directly to my email.

## AI Spending Limit Handling

An important part of development was considering the AI spend limit. I added logic to check for this limit, so if it’s reached, the bot redirects the user to my email contact form, ensuring they can still get in touch without disruption.

## Marketing and Visitor Engagement

By integrating this chatbot, I aimed to increase visitor engagement and provide a unique experience on my portfolio. Early analytics show that users appreciate the direct access to project insights and career details.

## Conclusion

Building this AI chatbot has been a valuable project, not only to showcase my technical skills but also to provide a dynamic way for visitors to interact with my portfolio. From planning and developing to deploying and testing, this chatbot demonstrates how AI can transform a static portfolio into an interactive user experience.

Try interacting with my chatbot yourself on the [portfolio site](https://www.pierregraef.com){:target="_blank"} and let it guide you through my data science journey!
