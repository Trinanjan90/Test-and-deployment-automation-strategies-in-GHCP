## Exercise 6: Test Automation Using Playwright MCP Server

### Goal
Write Automation Test Scripts for the User Stories for the solution.

### Steps

1. **Check if Playwright MCP Server is added:**
   - Click on **Tools** in Chat Window.  
     ![Tools Menu](image-3.png)
   - In the dropdown that opens, ensure **Playwright MCP Server** is available and selected.  
     ![Playwright MCP](image-4.png)

2. **If Playwright MCP Server is not added:**
   - Press `Ctrl+Shift+P` and type:
     ```
     MCP: open User Configuration
     ```
     ![User Config](image-5.png)
   - Add the following configuration:

     ```json
     "playwright": {
       "command": "npx",
       "args": [
         "@playwright/mcp@latest"
       ],
       "type": "stdio"
     }
     ```

   - Click **Run**. The configuration should now appear in Tools.  
     ![Run Config](image-6.png)

3. **Create Automation Script from Scratch:**
   - Type the prompt:

     ```text
     Design Playwright Automation Test Cases for the attached functional TestCases JSON file using instructions attached. URL of the application is <<Your Application URL>>.
     ```

   - Also attach:
     - `AutomationScriptCreationInstructions` from the **Instructions File** in the Testing folder.
     - The relevant **UserStory**.

   - This will create all necessary Playwright UI Automation Test Cases.  
     ![Automation Prompt](image-8.png)

4. **Permissions:**
   - Ensure you select **Allow All** for all requests from the Agent.