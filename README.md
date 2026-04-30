# n8n-Email-Automation-System

PowerPoint Link: https://1drv.ms/p/c/9239d57cde3fcee4/IQC_VmKVgomcT7LmEn9A8_u6AViIithznU2gxlybM8P7sl8?e=Kzwi1X

### REFLECTION:

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

------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------


### THE WORKFLOW:

<img width="2534" height="1200" alt="image" src="https://github.com/user-attachments/assets/51bf3100-0c10-429e-9286-6173a79c0751" />

MAIN WORKFLOW:

1. **New Email Arrives (Gmail Trigger)**:
    - Monitors Gmail inbox automatically
    - Triggers the entire workflow the moment a new email is received
    - Passes the email subject, sender, body, and ID to the next node
      
2. **Wait**
    - Pauses the workflow for 15 seconds before continuing to prevent Groq API rate limit errors from slowing down requests and crashing
      
3. **Basic LLM Chain (Groq AI)**
    - Sends the email subject, sender, and body to the Groq AI model
    - Instructs the AI to analyze the email and return a JSON response
    - Outputs a category, summary, and suggested draft reply for each email
      
4. **Parse AI Output (Code Node 1)**
    - Parses the raw JSON text returned by the AI
    - Extracts the category, summary, and suggested reply into clean fields
    - Formats the sender name and email address from Gmail's nested object structure
      
5. **Create a Database Page (Notion)**
    - Creates a new entry in my Notion database
    - Saves the email subject, sender, category, summary, date, and draft reply
      
6. **Pass Category Forward (Code Node 2)**
    - Reformats the data coming out of Notion
    - Passes the category and message ID forward so the Router can use them
    - Ensures the correct data structure reaches the next nodes
      
7. **Route by Category**
    - Reads the category assigned by the AI
    - Directs the workflow down one of three paths: Needs Reply, FYI, or Newsletter
    - Acts as the decision-making junction of the entire workflow
      
8. **Gmail Label Nodes (Label as TO REPLY, Label as FYI, Label as Newsletter)**
    - Applies the appropriate label to each email inside Gmail based on the category assigned by the AI
    - "**TO REPLY**" marks high priority emails that need your attention
    - "**FYI**" marks informational emails that don't require a response
    - "**Newsletter**" marks low priority or marketing emails, separating promotional content from emails that actually matter
    - Makes inbox visually organized without any manual sorting effort
      
9. **Create Draft Reply (Gmail)**
    - Only triggers for emails categorized as "Needs Reply"
    - Creates a pre-written draft reply in Notion database
    - Uses the AI-generated suggested reply as the draft body
      

WEEKLY REMINDER WORKFLOW:
1. **Weekly Reminder (Schedule Trigger)**
    - Triggers automatically every Monday morning at 9AM
    - Starts the weekly check without any manual input needed

2. **Fetch Old Needs Reply (Notion)**
    - Queries Notion database for emails older than 5 days
    - Filters specifically for entries that still have a "**Needs Reply**" status
    - Returns a list of emails I haven't responded to yet

3. **Send Weekly Summary (Gmail)**
    - Sends an email summarizing all unanswered emails from the past week
    - Reminds me of anything that has been waiting too long for a response
