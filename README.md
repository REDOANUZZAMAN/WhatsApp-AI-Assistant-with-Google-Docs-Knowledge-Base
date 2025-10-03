# WhatsApp AI Assistant with Google Docs Knowledge Base

A smart WhatsApp chatbot that automatically answers customer questions using your business documentation stored in Google Docs. No training required—just update your Google Doc and the bot instantly knows the new information.

## 🎯 What It Does

This n8n workflow creates an intelligent WhatsApp assistant that:
- Receives messages from WhatsApp Business Cloud API
- Reads your business knowledge from a live Google Doc
- Uses AI (Google Gemini or OpenAI) to generate contextual responses
- Maintains conversation history with simple memory
- Logs all interactions to Google Sheets
- Handles WhatsApp's 24-hour messaging window
- Sends clean, formatted responses without AI artifacts

## ✨ Features

- **Live Knowledge Base**: Update your Google Doc anytime—changes reflect immediately
- **Conversational Memory**: Remembers context within each user's conversation
- **Smart Response Cleaning**: Removes markdown formatting and AI preambles for natural WhatsApp messages
- **Automatic Logging**: Tracks all conversations in Google Sheets for review and analysis
- **24-Hour Window Management**: Automatically handles WhatsApp's messaging restrictions
- **Template Message Support**: Reopens conversations outside the 24-hour window

## 🛠️ Prerequisites

Before setting up this workflow, you'll need:

1. **n8n Instance** (cloud or self-hosted)
2. **WhatsApp Business Cloud API Account** ([Meta Business Suite](https://business.facebook.com/))
3. **Google Account** with access to:
   - Google Docs API
   - Google Sheets API
4. **AI API Key** (one of the following):
   - Google Gemini API key
   - OpenAI API key
5. **Phone Number** verified with WhatsApp Business API

## 📋 Setup Instructions

### 1. Import the Workflow

1. Open n8n
2. Click **"Import from File"** or **"Import from URL"**
3. Upload the `customer_support_whatsapp_bot_with_google_docs_knowledge_base_and.json` file
4. The workflow will appear in your canvas

### 2. Configure Google Docs Knowledge Base

1. Create a Google Doc with your business information (FAQs, policies, product details, etc.)
2. Copy the Document ID from the URL:
   ```
   https://docs.google.com/document/d/[DOCUMENT_ID]/edit
   ```
3. In the **"company's knowledge"** node:
   - Paste your Document ID
   - Authenticate with Google OAuth

### 3. Set Up WhatsApp Business API

1. Create a WhatsApp Business account through [Meta Business Suite](https://business.facebook.com/)
2. Get your Phone Number ID from the WhatsApp API dashboard
3. Generate an access token
4. In the **"when message received"** node:
   - Add WhatsApp OAuth credentials
   - Set up webhook verification
5. Update the `phoneNumberId` in both WhatsApp send nodes with your Phone Number ID

### 4. Configure AI Model

**For Google Gemini (default):**
1. Get API key from [Google AI Studio](https://makersuite.google.com/app/apikey)
2. In the **"Google Gemini Chat Model"** node, add your credentials
3. Current model: `gemini-2.5-flash-preview-04-17-thinking`

**For OpenAI (alternative):**
1. Replace the Gemini node with an OpenAI Chat Model node
2. Add your OpenAI API key
3. Choose your preferred model (e.g., `gpt-4` or `gpt-3.5-turbo`)

### 5. Set Up Google Sheets Logging

1. Create a Google Sheet with columns: `Timestamp`, `User`, `Message`, `Response`
2. Copy the Sheet ID from the URL:
   ```
   https://docs.google.com/spreadsheets/d/[SHEET_ID]/edit
   ```
3. In the **"Google Sheets"** node:
   - Paste your Sheet ID
   - Authenticate with Google OAuth
   - Map the columns

### 6. Configure Memory (Optional but Recommended)

For persistent conversation memory across sessions:
1. Set up a PostgreSQL database
2. Follow [this tutorial](https://www.youtube.com/watch?v=JjBofKJnYIU) for memory configuration
3. Replace **"Simple Memory"** node with **"PostgreSQL Memory"** node

### 7. Customize the AI System Message

In the **"AI Agent"** node, customize the system message for your brand:
```
You are [Your Company Name]'s support assistant.
• Be helpful, friendly, and professional
• Answer based on the knowledge base provided
• Don't mention documents or sources
• Keep responses concise and clear
```

### 8. Test the Workflow

1. Activate the workflow
2. Send a WhatsApp message to your business number
3. Verify the bot responds correctly
4. Check Google Sheets for logged conversation

## 📊 Workflow Structure

```
WhatsApp Message Received
    ↓
Fetch Google Doc Content
    ↓
Prepare Prompt (date + doc + message)
    ↓
AI Agent (with memory)
    ↓
Log to Google Sheets
    ↓
Clean Response (remove formatting)
    ↓
Check 24-hour Window
    ↓
Send Response OR Template Message
```

## 🔧 Customization Options

### Adjust Response Tone
Edit the system message in the **"AI Agent"** node to match your brand voice.

### Add More Knowledge Sources
Connect additional Google Docs or other data sources to the **"Prepare Prompt"** node.

### Modify Response Cleaning
Update the **"cleanAnswer"** code node to change how responses are formatted.

### Change Memory Duration
Adjust the memory window in the **"Simple Memory"** node (default: session-based).

### Add Escalation Logic
Create conditional nodes to forward complex queries to human agents.

## 🐛 Troubleshooting

### Bot doesn't respond
- Verify WhatsApp webhook is active
- Check Phone Number ID is correct
- Ensure AI API credentials are valid

### Responses include "Based on the document..."
- The cleaning node should remove this, but verify it's connected properly
- Update the regex in **"cleanAnswer"** if needed

### Memory not working
- Check session key is set correctly (using WhatsApp user ID)
- Verify memory node is connected to AI Agent

### 24-hour window errors
- Ensure you have an approved message template in Meta Business Suite
- Update template name in **"Send Pre-approved Template Message"** node

## 💡 Use Cases

- **Customer Support**: Answer FAQs instantly
- **Product Information**: Provide detailed product specs
- **Booking/Reservations**: Guide users through booking processes
- **Order Status**: Share shipping and order information
- **General Inquiries**: Handle common business questions

## 🤝 Need Help?

If you need assistance with setup, customization, or scaling this solution:

- **Email**: redoanuzzaman707@gmail.com
- **WhatsApp**: +8801300512598

### Services Offered:
- WhatsApp Cloud API setup & verification
- Google OAuth & API configuration
- AI model optimization & customization
- Brand voice & prompt engineering
- Advanced logging & analytics
- Custom integrations & features

## 📝 License

This workflow is provided as-is for use with n8n. Modify and adapt as needed for your business.

## 🙏 Credits

Built with n8n, powered by Google Gemini, and designed for seamless customer support automation.
