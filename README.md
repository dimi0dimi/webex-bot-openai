# webex-bot-openai
This is an instructor-led lab for first steps in building an interactive bot with AI capabilities.

# Lab Guide: Building an Interactive Webex bot with AI capabilities

> **Goal:** Build a Webex bot that can answer questions and provide information using OpenAI's API.

---

## Table of Contents
1. [Login to Webex Application](#1-login-to-webex-application)
2. [Create a Webex Bot](#2-create-a-webex-bot)
3. [Install Git, Node.js and NPM](#3-install-git-nodejs-and-npm)
4. [Install the Webex Node.js SDK](#4-install-the-webex-nodejs-sdk)
5. [Set up OpenAI API Key](#5-set-up-openai-api-key)
6. [Make your first API call to OpenAI](#6-make-your-first-api-call-to-openai)
7. [Integrate the bot with the OpenAI API](#7-integrate-the-bot-with-the-openai-api)
8.

## Prerequisites

| You need            | Details                                                                                                                                                                                                      |
|---------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Webex installed** | Download from [https://www.webex.com/downloads.html](https://www.webex.com/downloads.html)                                                                                                                   |
| **Python v3.13**    | Download from [https://www.python.org/downloads/](https://www.python.org/downloads/)                                                                                                                         |
| **Any Code IDE**    | VSCode [https://visualstudio.microsoft.com/downloads/](https://visualstudio.microsoft.com/downloads/)<br/>PyCharm [https://www.jetbrains.com/pycharm/download/](https://www.jetbrains.com/pycharm/download/) |
| **Node.js**         | Minimum v8.0.0 [https://nodejs.org/en/download](https://nodejs.org/en/download)                                                                                                                                              |

---

## 1. Login to Webex Application

The proctor will provide you with a Webex account to use for this lab. Please make sure to log in to the Webex application using the provided credentials.

> **Note:** For this lab, you will be using Webex for creating a chat bot. If you're looking for some easy free to use chat frameworks you can explore these two:<br>
>> 1. https://github.com/mesop-dev/mesop<br>
>> 2. https://streamlit.io/playground?example=llm_chat

---

## 2. Create a Webex Bot

1. Go to the [Webex for Developers](https://developer.webex.com/) page.
2. Click on **Sign In** in the top right corner.
3. Log in with your Webex account.
4. Click on **My Apps** in the top right corner.
5. Click on **Create a New App**.
6. Select **Bot** as the app type.
7. Fill in the required fields:
   - **App Name:** Choose a name for your bot (e.g., "Shop Assistant").
   - **App Description:** Provide a brief description of your bot.
   - **Redirect URI:** Leave this blank.
8. Click on **Create**.
9. Once the bot is created, you will be redirected to the app details page.
10. Copy the **Bot Token** and **Bot Name**. You will need these later.

11. Open the Webex app and find the bot using its **Bot Name** you set earlier.
12. Send a message to the bot. This will create a 1:1 space with you and the bot.

---

## 3. Install Git, Node.js and NPM

Look at the following table for installation instructions of the above two (Git and NPM):


<details>
  <summary>💻 Windows Installation 👇</summary>

    
  Run these three commands to install Git, Node.js and NPM on Windows:

  ```bash
    winget install -id Git.Git -e --source winget
    winget install OpenJS.NodeJS
    npm install -g npm
  ```


  **⚠️ PowerShell Script Execution Error?**

  <details>
    <summary>Click to see the error message & fix 👇</summary>

  If you ran: `npm install -g npm` and got this:
  ```commandline
  npm : File C:\Program Files\nodejs\npm.ps1 cannot be loaded because running scripts is disabled on this system. For
  more information, see about_Execution_Policies at https:/go.microsoft.com/fwlink/?LinkID=135170.
  At line:1 char:1
  + npm install -g npm
  + ~~~
      + CategoryInfo          : SecurityError: (:) [], PSSecurityException
      + FullyQualifiedErrorId : UnauthorizedAccess
   ```
  **Run this from elevated powershell to allow locally-created scripts to execute:**
  ```bash
    Set-ExecutionPolicy -Scope CurrentUser -ExecutionPolicy RemoteSigned -Force
  ```
  **Then close & reopen PowerShell and try again!**
  </details>

</details>

---

<details>
  <summary> MacOS Installation 👇</summary>
  Run these three commands to install Git, Node.js and NPM on MacOS:

  ```bash
    brew install git
    brew install node
    npm install -g npm
  ```
</details>

---

<details>
  <summary>🐧 Linux Installation 👇</summary>
  Run these three commands to install Git, Node.js and NPM on Linux:

  ```bash
    sudo apt-get install git
    sudo apt-get install nodejs
    sudo apt-get install npm
    npm install -g npm
  ```
</details>

---

## 4. Install the Webex Node.js SDK

1. Open your IDE code editor (VSCode or PyCharm) and run the following commands in its terminal:

```bash
  mkdir WebexAIchatbot
  cd WebexAIchatbot

  git clone https://github.com/WebexCommunity/webex-node-bot-framework
  npm install ./webex-node-bot-framework
```

2. Follow **Steps to get the bot working** from the 'webex-bot-starter' README.md file
   - [./webex-bot-starter/README.md](README.md) <br>
   OR from
   - [https://github.com/WebexSamples/webex-bot-starter](https://github.com/WebexSamples/webex-bot-starter)
<br>

### Next Step

✅ **Bot is working! Proceed to the next lab: “Making your first API call.”**

⚠️ **Stuck? Don’t worry—ask the proctor for help and get back on track.**

---

## 5. Set up OpenAI API Key

0. **Create an OpenAI account** if you don’t have one yet: [https://platform.openai.com/signup](https://platform.openai.com/signup)
1. Go to [OpenAI API Keys](https://platform.openai.com/account/api-keys).
2. Click **Create new secret key**.
3. Copy the value → `sk‑...` →  copy it somewhere.
   - **Note:** Even if you lose it - you can generate a new one.
4. Add it to the [**.env**](.env) like this `OPENAI_API_KEY="YOUR_TOKEN"`.
5. **Save the file**.

## 6. Make your first API call to OpenAI

1. Create a new file in the `WebexAIchatbot` folder called `generate_response.py`.
2. Add the following code to the file:

```python
from dotenv import load_dotenv
from openai import OpenAI

load_dotenv()

def main():
  system_prompt = "You're a helpful assistant."
  user_message = "Generate a haiku poem about the beauty of winter in Poland."
  client = OpenAI()
  response = client.chat.completions.create(
    model="gpt-4.1-nano",
    messages = [
      {"role": "system", "content": system_prompt},
      {"role": "user", "content": user_message}
    ]
  )

  print(response.choices[0].message.content)

main()
```

3. Create a virtual environment

```bash
  python -m venv venv
```
4. Activate the virtual environment

```bash
  # 💻 Windows
  venv\Scripts\activate
````
```bash
  #  MacOS/Linux
  source venv/bin/activate
```

4. Install the required packages

```bash
  pip install openai python-dotenv
```

5. Run the script in the terminal:

```bash
  python generate_response.py
```

---

## 7. Integrate the bot with the OpenAI API
1. Open [index.js](index.js) file in the `webex-node-bot-framework` folder.
2. Add the following code to the file on line 14:

```javascript
const { spawn } = require('child_process');
```

3. Add the following code to the file on line 102:

```javascript
# Add a new event handler for the bot with regex "I would like.*"
# Check how regex works: https://regex101.com/
framework.hears(/I would like.*/i, (bot, trigger) => {
  const { spawn } = require('child_process');
  const pyProcess = spawn('python', [
    'generate_response.py',
    '--user_message', trigger.text
  ]);

  let pythonData = '';
  pyProcess.stdout.on('data', (data) => {
    pythonData += data.toString();
  });
  pyProcess.stdout.on('end', () => {
    bot.say('markdown', pythonData);
  });
  pyProcess.stderr.on('data', (err) => {
    console.error(`Python error: \${err}`);
    bot.say('Something went wrong.');
  });
});
```

> Now the bot listens for messages that start with "I would like" and responds with the generated response from the OpenAI API.
> <br> We still need to change our python code to accept the user message from the bot and pass it to the OpenAI API.
> <br> We will do that in the next step.

4. Open [generate_response.py](generate_response.py) file in the `WebexAIchatbot` folder.
5. Add the following code to the file on line 1:

```python
# 1. Add this to the top of the file
import argparse

# 2. Add this at the top of main():
    parser = argparse.ArgumentParser()
    parser.add_argument('--user_message', required=True)
    args = parser.parse_args()
    
# 3. Change this line:
    user_message = user_message

# To this:
    user_message = args.user_message

# 4. And comment out user_message line by adding a "#" in front of it.
```

6. Save the file.
7. Restart the bot in the terminal by pressing `Ctrl + C` and then running the following command:

```bash
  npm start
```

8. Open the Webex app and send a message to the bot that starts with "I would like" followed by something of your choice and see what happens.

### Next Step

✅ **Bot is working! Proceed to the next lab: “Making your first API call.”**

⚠️ **Stuck? Don’t worry—ask the proctor for help and get back on track.**

---


