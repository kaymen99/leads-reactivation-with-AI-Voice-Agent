<!--
  Title: AI Voice Agent: Automated Leads Reactivation with Personalized Calls
  Description: The AI Voice Agent automates follow-up calls to re-engage inactive leads, qualify prospects, and streamline CRM updates, helping businesses boost revenue and optimize their sales pipeline.
  Author: Aymen
  Keywords: AI voice agent, lead reactivation, lead generation, VAPI platform, Retell AI, CRM automation, automated follow-up calls, GPT-4o, Twilio integration, OpenAI Realtime API, Airtable
-->

# AI Voice Agent for Leads Reactivation  

![leads-reactivation-automation](https://github.com/user-attachments/assets/d86511f9-dc6c-4d08-a145-7f534ef5fb3b)

This project automates the process of reactivating inactive leads using an AI voice agent. The system targets business warm leads/prospects who previously showed interest but didn’t convert—to help generate additional revenue. Instead of relying on less effective email campaigns, the AI voice agent makes personalized follow-up calls to re-engage leads, qualify them, or gather intent for the sales team. This approach is ideal for businesses with large lead databases and low conversion rates.  

## How It Works  

1. **CRM Integrations**  
   Dead leads are imported from your CRM, whether it’s Airtable, HubSpot, Google Sheets, or another system of your choice. The database includes key lead details (e.g., name, address, email) to personalize interactions without being overly intrusive.  

2. **Call Trigger**  
   A custom AI voice agent built on the VAPI platform initiates calls to the leads. Using a personalized script, the agent engages leads to gauge their interest and qualify them. The voice agent has also access to a scheduling tool connected to Google Calendar, enabling the agent to book follow-up appointments if a lead expresses interest.  

3. **Communication with VAPI**  
   During and after each call, VAPI sends data (such as call states, transcripts, recordings, and summaries) back to the server via a webhook. Two key webhook events are of interest:  
   - **Tool Calls:** When the agent schedules a follow-up appointment with a lead using the integrated booking tool.  
   - **End-Call Report:** A detailed report is sent at the end of each call, containing the transcript, cost, duration, and conversation messages. This data is used for our post call analysis and CRM updates.  

4. **Lead Qualification and CRM Update**  
   Once the end-call report is received, the system triggers a post-call analysis function. This function leverages an LLM (GPT-4o or alternatives) to analyze the transcript, determine the lead’s level of interest, and generate actionable notes for the sales team. Finally, the CRM is updated with all relevant call details, including the lead’s status.  

## Benefits  

- **Reactivates Leads:** Engages prospects who previously showed interest but didn’t convert.  
- **Boosts Revenue:** Generates additional income by reviving inactive leads.  
- **Scalable:** Handles high volumes of outreach with minimal effort.  
- **Actionable Insights:** Provides detailed analytics and call data to refine strategies.  
- **Streamlined Lead Qualification:** Automates the lead screening process, saving time and resources.  
- **Enhanced Sales Pipeline:** Delivers insights into conversion roadblocks, helping to improve the overall sales processes.  

## AI Voice Agent Frameworks  

Several frameworks are available for building voice agents, and this repository includes the necessary code for the most popular options: **VAPI**, **Retell AI**, and **OpenAI’s Real-Time API** (not fully implemented yet).  

- Both **VAPI** and **Retell AI** are no-code platforms that support a variety of LLMs providers and voice models for developing Voice AI agents, including OpenAI’s real-time APIs. They simplify the Voice agent development process, offering many features like direct telephony integration with Twilio, custom tool integration, interruption handling, and fallback responses during delays, etc.  
- **OpenAI’s Real-Time API** (speech-to-speech model) is currently the fastest voice technology available. You can either use it via platforms like VAPI or Retell AI for convenience or integrate it directly using openai to avoid additional platform fees.  

For this project, I used **VAPI** for the voice agent’s development, but, the code is designed to be adaptable and can be configured to work with **Retell AI** or **OpenAI’s Real-Time API**.

---

## API Integrations  

- **OpenAI**: Uses **GPT-4o** models. [Get API key here](https://platform.openai.com/docs/overview).  
- **Twilio**: Handles phone calls. Obtain a Twilio number [here](https://www.twilio.com/).  
- **Airtable CRM**: Connects to your Airtable contacts database. [Sign up](https://www.airtable.com/) and create a contacts database with the required fields.  
- **HubSpot CRM**: Integrates with HubSpot for managing your leads. Sign up for a [HubSpot account](https://www.hubspot.com/), create a private app, and obtain an API key. [Follow this tutorial](https://www.youtube.com/watch?v=hSipSbiwc2s) for help.  
- **Google APIs**: Used for accessing **Google Calendar** and **Google Sheets** (only when these are selected as CRM sources).  
- **Google Search Automation**: Utilizes the **Serper API** for web searches. [Get your API key here](https://serper.dev).  

---

## Tech Stack  

- **[Langchain](https://python.langchain.com/docs/introduction/)**.
- **[Vapi](https://dashboard.vapi.ai/)**  
- **[Retell AI](https://www.retellai.com/)**
- **[OpenAI](https://platform.openai.com/docs/overview)**

---

## How to Run  

### Prerequisites  
 
- Python 3.9+  
- Account and API keys for **Vapi** or **Retell AI**  
- OpenAI API key (or alternative LLMs like Claude or Groq)  
- Twilio phone number  
- Google API credentials (for Google Calendar or sheets when used as leads database).
- Integration keys for CRM tools (Airtable, HubSpot) and other services (e.g., Serper API). 
- NGROK installed 

---

### Setup  

1. **Clone the repository:**  

   ```sh  
   git clone https://github.com/kaymen99/leads-reactivation-with-AI-Voice-Agent.git  
   cd leads-reactivation-with-AI-Voice-Agent  
   ```  

2. **Set up a virtual environment and install dependencies:**  

   ```sh  
   python -m venv venv  
   source venv/bin/activate  # For Windows: `venv\Scripts\activate`  
   pip install -r requirements.txt  
   ```  

3. **Configure environment variables:**  

   Copy the example environment file and update it with your API keys:  

   ```bash  
   cp .env.example .env  
   ```  

   Open the `.env` file and fill in your API keys and other configurations.  

4. **Run the app server:**  

   Start the server application:  

   ```sh  
   python app.py  
   ```  

   To expose the server publicly, use **ngrok**. Open a new terminal and run:  

   ```sh  
   ngrok http 8000  
   ```  

   Copy the public URL generated by ngrok and update the `SERVER_URL` variable in your `.env` file.  

5. **Set up the voice agent:**  

   - Create the necessary tools for the voice agent in Vapi. For this project, we only have a single tool for booking appointements with Google Calendar. Run the provided script:  

     ```sh  
     python scripts/create_or_update_tool.py  
     ```  

     This script will return a tool ID. Copy it for the next step.  

   - Update the tool configuration in `scripts/create_or_update_assistant.py` by adding the tool ID to `tool_ids_list`. Then, create the assistant:  

     ```sh  
     python scripts/create_or_update_assistant.py  
     ```  

     Copy the assisatnt id to your `.env` file (`VAPI_ASSISTANT_ID`).

     Note: The assistant configuration uses GPT-4o by default. You can switch to another supported LLM by modifying the configuration before running the script.  

   - Import your Twilio phone number into Vapi. Ensure you have added your Twilio credentials (watch this quick [tutorial](https://www.youtube.com/watch?v=aM79mkF6UkA)), then run:  

     ```sh  
     python scripts/import_phone_number.py  
     ```  

     Copy the Vapi phone number id to your `.env` file (`VAPI_PHONE_ID`).

---

### Testing the Automation  

To test the setup, you can simulate a call to your own phone number:  

1. Ensure the server is running and accessible.  
2. Trigger the automation route `/execute` using Postman or directly via the API docs dashboard.  
3. The automation expects a list of lead IDs. You can either specify specific IDs or leave it empty to fetch any new leads.  
4. You should receive a call from the voice agent.  

After the call, check the CRM for updates or view the call logs directly in the VAPI dashboard.  

---

### Customizing the Automation

If you want to integrate another CRM or customize the behavior of the automation, please refer to the [Customization Guide](/docs/customization.md). The guide covers:

- **Integrating Custom CRMs**: Instructions for adding your CRM to the system by extending the base class.
- **Customizing Lead Statuses**: Learn how to modify the statuses used to filter and fetch leads.
- **Updating CRM Fields**: Tailor the automation to handle different CRM field names or additional fields.
- **Customizing Prompts**: Update the prompts used for AI voice agent and the post call analysis (all pormpts are in `src/prompts.py`).

---

## Contributing

Contributions are welcome! Please open an issue or submit a pull request for any improvements or features you’d like to see.

## Contact

For questions or suggestions, contact me at `aymenMir1001@gmail.com`.
