# n8n-Email-Automation-System

REFLECTION:
The goal of this project was to build an automated email management system using n8n, AI, Gmail, and Notion. Instead of manually sorting through emails every day, I wanted an intelligent workflow that could read incoming emails, understand what they were about, categorize them by priority, save organized summaries to a Notion database, and automatically draft replies when needed to increase productivity.

Building the workflow was more technically involved than expected. The initial plan used Claude as the AI model, but API credit limitations required switching to a free alternative. After testing several options including Gemini and Groq, I settled on Groq with the llama-3.1-8b-instant model, which offered the highest free tier limits and fastest response times. The most challenging parts were getting the AI output parsed correctly and mapping data between nodes, which required adding dedicated Code nodes to reformat data and handle inconsistencies in how Gmail and Groq structured their outputs.

Once these issues were resolved, the final workflow successfully:
* Triggers automatically on every new email
* Classifies emails into three categories: Needs Reply, FYI, and Newsletter
* Saves structured summaries to a Notion database
* Applies Gmail labels automatically
* Creates draft replies for emails requiring a response
* Sends a weekly reminder for unanswered emails every Monday
  
Since this was my first time using n8n, I intentionally kept the workflow simple so I could fully understand each component. However, if I had more time I would expand it in two ways. First, I would add a node that generates draft replies in my personal writing tone by referencing examples of emails I have previously sent, making the AI responses feel more authentic. Second, I would improve the draft reply functionality so that generated drafts are not only saved to my Notion database but also automatically placed in my Gmail Drafts folder, making them immediately ready to review and send.

Overall, I really enjoyed working on this project and learned a lot about n8n and APIs. It was interesting to see how automation and AI can work together to handle repetitive cognitive tasks, turning a cluttered inbox into an organized, searchable knowledge base with minimal manual effort.
