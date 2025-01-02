# Building a AI/ML Chatbot with Amazon Lex

## What is Amazon Lex?
![image](https://github.com/user-attachments/assets/b18db5db-050e-4752-83f2-bd0bf1fc1ef1)


Amazon Lex is an AI-powered service designed to help developers create conversational interfaces using voice and text. In simpler terms, it’s a tool that lets you build chatbots and virtual assistants that can understand what people are saying and respond naturally in real time. The service provides advanced features like intent recognition, slot filling, and seamless integration with AWS services like Lambda for enhanced functionality.

In this series of projects, I explored the core features of Amazon Lex and built multiple chatbots with varying levels of complexity. These projects highlight the power of Lex for creating intelligent and interactive conversational systems.

---

## Table of Contents
1. [Project 1: Build a Chatbot with Amazon Lex](#project-1-build-a-chatbot-with-amazon-lex)
2. [Project 2: Build a Chatbot with Custom Slots](#project-2-build-a-chatbot-with-custom-slots)
3. [Project 3: Connect a Chatbot with Lambda](#project-3-connect-a-chatbot-with-lambda)
4. [Project 4: Save User Info with Your Chatbot](#project-4-save-user-info-with-your-chatbot)
5. [Project 5: Build a Chatbot with Multiple Slots](#project-5-build-a-chatbot-with-multiple-slots)
6. [What I Learned](#what-i-learned)

---

## Project 1: Build a Chatbot with Amazon Lex

### Setting Up a Lex Chatbot
- I created a chatbot from scratch using **Amazon Lex**, which took around 3 minutes to set up. 
- I created a role with basic permissions, as Amazon Lex requires permissions to call other AWS services on my behalf.
- I left the **intent classification confidence score** at its default value of `0.40`, ensuring the chatbot is at least 40% confident before responding.

![Screenshot 2024-12-29 223024](https://github.com/user-attachments/assets/6584bc7f-c95a-46f7-87c5-05f2bf4fdd28)



---

### Intents
- An **intent** represents the user's goal in a conversation.
With Amazon Lex you build your chatbot by defining and categorising different
intents. 
- I created a simple intent called **WelcomeIntent** to welcome users when they say "hello."

![Screenshot 2024-12-29 224104](https://github.com/user-attachments/assets/4d760542-4661-4f74-bd75-2df9c00a154f)


---

### FallbackIntent
- While testing the chatbot, I noticed it responded correctly to "Hello" and "I need help."
- However, when entering **"Good morning"**, the bot triggered the **FallbackIntent** because the utterance was not defined.

![Screenshot 2024-12-29 224136](https://github.com/user-attachments/assets/46d8ea65-cefe-47c3-b0c8-1f6ad8f5f21f)

---

### Configuring FallbackIntent
- I configured the **FallbackIntent** to display custom error messages instead of the default "FallbackIntent" text.
- This improved the user experience by asking users to rephrase their questions when the bot couldn't understand them.

### Variations
- I added message variations to the FallbackIntent, so the bot provided different responses each time the intent was triggered.

![Screenshot 2024-12-29 225051](https://github.com/user-attachments/assets/40ca3871-06f7-410b-bf34-ef0b15cd5143)


---

## Project 2: Build a Chatbot with Custom Slots

### Slots
**Slots** are pieces of information that a chatbot collects to fulfill a user's request.  
- I added custom slot types enabling the chatbot to check a user's **checking** or **savings account balance**.

![Screenshot 2024-12-29 230921](https://github.com/user-attachments/assets/2f66786d-efe7-498e-b1db-a20a37ddeeaf)


---

### Connecting Slots with Intents
- I created a slot type called **CheckBalance** for collecting account types (e.g., "checking" or "savings").
- Restricted slot values ensured that only valid options would be accepted.

![Screenshot 2024-12-29 231351](https://github.com/user-attachments/assets/0b05fab5-b63d-42b7-886c-bc8443f3c998)


---

### Slot Values in Utterances
- I used placeholders like `{accountType}` in utterances to dynamically capture slot values.
- For example, the utterance **"What's the balance in my {accountType} account?"** automatically fills the slot with the user input.

![Screenshot 2024-12-29 235314](https://github.com/user-attachments/assets/59a9ab30-8599-459c-bcd5-4492930631cb)


---

## Project 3: Connect a Chatbot with Lambda

### AWS Lambda Functions
**AWS Lambda** is a serverless compute service that runs code in the cloud without managing infrastructure.  
- I created a Lambda function to handle requests for the **BankerBot** chatbot.

![Screenshot 2024-12-30 023559](https://github.com/user-attachments/assets/ec3ddcf6-a583-4f18-8fd7-2e62f7d593e7)


---

### Chatbot Alias
- I created an alias called **TestBotAlias** to test the bot's functionality with the Lambda function.
- I connected the alias to the Lambda function by editing the source in the bot's alias settings.

![Screenshot 2024-12-30 024256](https://github.com/user-attachments/assets/7d2fd73e-3721-4a05-868c-7567482a4bfd)


---

### Code Hooks
- I used **code hooks** to connect the chatbot to the Lambda function for fulfilling user requests.
- A code hook helps you connect your chatbot to basic Lambda functions to
complete certain tasks
- Even though I already connected my Lambda function with my chatbot's alias, I
had to use code hooks to make my chatbot smarter and more useful by
allowing it to perform these extra steps seamlessly during chats

![Screenshot 2024-12-30 024637](https://github.com/user-attachments/assets/a1c9a92e-b835-4f79-a29b-418d2f4f9efa)


---

### The Final Result
- I configured the chatbot to trigger Lambda and return a random dollar figure when asked for a bank balance.

![Screenshot 2024-12-30 024820](https://github.com/user-attachments/assets/51e5b564-48e1-4b2d-af35-7267f51eb452)

---

## Project 4: Save User Info with Your Chatbot

### Context Tags
- **Context tags** store and retrieve information across conversations.
- There are two types of context tags, output contexts and input contexts. Output
contexts tells the chatbot to remember details after the intent is finsihed. While
Input contexts checks if specific details are already available
- I created an output context tag called **contextCheckBalance** to save the user's **date of birth (DOB)** from the **CheckBalance** intent.

![Screenshot 2024-12-30 025514](https://github.com/user-attachments/assets/f9820767-d271-4d78-af2c-b268adb20b4f)


---

### FollowUpCheckBalance
- I created a new intent called **FollowUpCheckBalance** to handle follow-up questions like "What about this?"
- This intent used the **contextCheckBalance** input context to retrieve the user's DOB from the previous intent.

![Screenshot 2024-12-30 030131](https://github.com/user-attachments/assets/306f3e23-e952-4733-b1ae-9abc0502d682)


---

### Input Context Tag
- I linked the **CheckBalance** intent with the **FollowUpCheckBalance** intent using the **contextCheckBalance** input context.

![Screenshot 2024-12-30 030423](https://github.com/user-attachments/assets/d6a93409-b152-4400-a98a-1c186738000f)


---

### The Final Result
- The chatbot successfully triggered the follow-up intent by carrying over the user's context (DOB).

![Screenshot 2024-12-30 031018](https://github.com/user-attachments/assets/7bf59bf9-5368-4db9-983d-3aae2c03cc26)


---

## Project 5: Build a Chatbot with Multiple Slots

### TransferFunds
- I created an intent called **TransferFunds**, which simulates transferring funds between accounts.

![Screenshot 2024-12-30 031950](https://github.com/user-attachments/assets/edd0ed13-5b1a-4b8a-a7ff-1952234d644f)


---

### Using Multiple Slots
- I used the same slot type twice for the **TransferFunds** intent to collect both the `sourceAccountType` and `targetAccountType`.
- I also added **confirmation prompts** to confirm user actions before fulfilling the intent.

![Screenshot 2024-12-30 031602](https://github.com/user-attachments/assets/a132934b-148d-4bb2-9968-1803fe148774)


---

### Exploring Lex Features
- Amazon Lex provides a **conversational flow feature**, which suggests additional configurations for intents.
- I also explored the **visual builder**, which visually represents the chatbot’s structure.

![Screenshot 2024-12-30 032404](https://github.com/user-attachments/assets/431fc5b2-4127-4d64-a3a8-07aa111741f1)


---

### AWS CloudFormation
**AWS CloudFormation** is an Infrastructure-as-Code (IaC) service.  
- I used CloudFormation to deploy the entire **BankerBot** setup in Amazon Lex.

![Screenshot 2024-12-30 034138](https://github.com/user-attachments/assets/9872049c-dc89-4319-96f5-5ce485d1010a)


---

### The Final Result
- I successfully rebuilt my bot using CloudFormation in just 6 minutes.  
- I resolved a permissions error by adding a policy for Lex to access Lambda.
> [!NOTE]  
> Ignore the error under **Service**. The permission was successfully created.

![Screenshot 2024-12-30 035701](https://github.com/user-attachments/assets/6eb05eb6-8c4f-45de-b4d2-ce4a7f991a05)


---

## What I Learned

In conclusion:

- **In Project 1 (WelcomeIntent, FallbackIntent)**, I learned how to:
  - Define basic intents and utterances.
  - Handle failures using **FallbackIntent**.
  - Configure message variations for dynamic responses.
  - Build and test the bot using text and speech.

- **In Project 2 (CheckBalance)**, I learned how to:
  - Define and use custom slot types.
  - Associate custom slots with intents.
  - Parse slot values from user utterances.

- **In Project 3 (Lambda Function)**, I learned how to:
  - Set up a Lambda function and integrate it with Lex.
  - Use code hooks for intent fulfillment.

- **In Project 4 (FollowUpCheckBalance)**, I learned how to:
  - Use context tags to carry over information between intents.

- **In Project 5 (Multiple Slots)**, I learned how to:
  - Use multiple slots in an intent.
  - Implement confirmation prompts.
  - Deploy a chatbot using AWS CloudFormation.

These projects taught me how to build intelligent, interactive chatbots using Amazon Lex and AWS services. The skills I gained are invaluable for creating conversational interfaces for real-world applications.
