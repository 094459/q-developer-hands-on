![Amazon Q Developer header](/images/q-vscode-header.png)

### Building our application - Part One

**Overview**

In the first part of this lab we are going to start by first looking at the data model for our application. As we are building a customer survey feedback application, we want to spend some time thinking about how to craft the right prompts.

**Using Git branching when coding using AI coding assistants**

Whilst you will be generating code in this lab, to make it easier to follow along I have provided a git repo that provides code that will support you as you explore. When you check out this repo there will be no code - don't worry, this is as per design. As we progress, we will use different branches within this repo to bring you to a working place. Feel free to ignore these and use your own code if you want. You can always reset and get back on track as the lab will provide necessary instructions.

**Creating our Python Virtual Environment**

From a terminal, navigate to a new directory where we will start work (I am using projects here as this is what I use, but feel free to switch this to whatever you use), and then type in the following to clone the repo into your workspace. 

```
cd ~/projects
git clone https://github.com/094459/q-developer-workshop-demo-code.git
cd q-developer-workshop-demo-code
```
Fom the terminal, we can now start VSCode by typing in "code ."

Once VSCode has started, the first thing we need to do is make sure that we are running in the Python virtual environment we created. Open up a Terminal in VSCode, and from that terminal run the following commands:

```
python -m venv .venv
source .venv/bin/activate
```

You should notice that the Terminal inside VSCode now has the (.venv) prefix, indicating you are running in this new virtual Python environment.

You should also switch your VSCode Python environment to use this. From VSCode press CTRL + COMMAND + P (Mac), or CTRL + SHIFT + P (Windows) and type in Python. From the list, select "Select Interpreter" and from the available options, make sure you select the one which has the correct directory location (.venv) which should be the recommended one. 

![Selecting Python interpreter](/images/q-vscode-python-env.png)

You are now ready to go.

**Lab 01-1 Our application - Creating our Data Model**

The application we want to build is a simple tool for collecting customer feedback. We are going to start by using Amazon Q Developer to help us define the data model for this application, providing various prompts that will help provide a good starting point of what we want our data model to look like. Once we have our data model, we can then start to create some code artefacts.

Our data Model is a reflection of the User requirements of our application. For this simple customer feedback application, we might use the following to describe this.*Create a data model for a simple customer survey application. Users will log in using an email addresses. Users will be able to create new surveys, or update/delete existing surveys. Users can create multiple surveys. Surveys can have between 2 and 5 options. Once a Surveys is created, the End Users will be able to submit feedback.*

**Task 1**

We will now see what Amazon Q Developers thinks when we ask it. 

1/ Open up the Amazon Q Developer Chat interface, and from the prompt enter the following:

> Create a data model for a simple customer survey application. Users will log in using an email addresses. Users will be able to create new surveys, or update/delete existing surveys. Users can create multiple surveys. Surveys can have between 2 and 5 options. Once a Surveys is created, the End Users will be able to provide feedback.

Give the non deterministic nature of these tools, it is hard to predict the output you will get. 

We are going to now try again, and this time use a feature of the Amazon Q Developer that will help us personalize the output that we get. In the project directory you git cloned, you will notice there is a folder called ".qdeveloper" which contains a single file - MY-PREFERENCES.md - Open the file and review it. 

We are going to use this file to help tailor the output of Amazon Q Developer, using the @workspace feature. This feature is disabled by default (it is currently a BETA feature) and so we are going to have to enable it. Once enabled, this is going to index your local VSCode workspace, looking at all the files including the DBA.md. Once the indexing has completed, we can use the @workspace in the Amazon Q Developer chat interface to add specific files as part of the context. In this case we are using it to help steer the kind of output we want Amazon Q Developer to give us, but we can use it for lots of other use cases too.

Right, lets enable this now.

**Task 2**

1/ Open up the Amazon Q Developer plugin settings (remember from the overview, using the link at the bottom of VSCode to bring up the menu, where you will find the "Open Settings" option). Look for the "Amazon Q: Workspace Index" section. This will have a check box next to it that will be blank (not set). Click on the box to enable this and then close the settings tab.

2/ From the Terminal menu options, select OUTPUT and then from the pull down menu, click on the down arrow to bring up all the different output otions. You should have a NEW entry called "Amazon Q Language Server". Click on this and you should see that it has started to index the files in this project. It will look something like this:

```
LSP server starts
Loaded model from /Users/ricsue/.vscode/extensions/amazonwebservices.amazon-q-vscode-1.27.0/resources/qserver
[Warn  - 16:44:27] Unknown tokenizer class "CodeSageTokenizer", attempting to construct from base class.
Using number of intra-op threads: 2
Embedding provider initialized.
Validating embedding:s1 0.26636229613723583, s2 0.20119692089068866, s3 0.4491747579475595
Indexing 2 files under rootpath: /Users/ricsue/Projects/amazon-q/workshop/q-developer-workshop-demo-code
Starting index 2 files
Indexing done for 1/2 files
Index complete! Take 21.269275083s. Total 2 files, 11 chunks
Index saved to /Users/ricsue/.aws/amazonq/cache/cache/d7118a0df04a8a3a08dcf3d3f868d272db1601d953802875b4be8bda158fe919-0.9-VSCode.index
```
You can also see the logs by changing the OUTPUT menu option to "Amazon Q Logs"

```
2024-10-03 16:44:27.504 [info] LspController: Starting to build vector index of project
2024-10-03 16:44:27.717 [info] LspController: Found 10 files in current project /Users/ricsue/Projects/amazon-q/workshop/q-workshop
2024-10-03 16:45:31.223 [info] AB Testing Cohort Assignments []
2024-10-03 16:45:36.759 [info] LspController: LSP server CPU 0.03829787234042553%, LSP server Memory 413.34375MB
```

>*Tip!* The directory where these indexes are created can be deleted if needed. As this is still a BETA capability, sometimes you might need to delete the index files and then restart the IDE to recreate them if you start getting strange error messages. 

We can now try this feature out.

**Task 3**

1/ From the Amazon Q Developer Chat interface, type @ and you should see workspace appear. Tab to complete that, and then add the following to your prompt:

> @workspace DEVSTYLE Create a data model for a simple customer survey application. Users will log in using an email addresses. Users will be able to create new surveys, or update/delete existing surveys. Users can create multiple surveys. Surveys can have between 2 and 5 options. Once a Surveys is created, the End Users will be able to provide feedback.

Hit return and review the output. You should get very different output this time. 

2/ Make a change to the MY-PREFERENCE.md file, changing sqlite to a different database (I tried this with Oracle, but feel free to experiment). Try the same prompt again after a few moments. Did you get different output? Change the text in MY-PREFERENCE.md back to what it was.

**Task 4**

We will now create our data model. You can do this a number of ways, and it will vary between your preferences and the norms and guidelines you might need to adopt where you are developing. We will show you how to create both SQL and Python code that will generate your tables.

*Generating SQL*

1/ First we are going to create a folder called "data" and create a new file called "customer-feedback.sql". This will be empty for the time being, but we will paste SQL into this file as Amazon Q generates output. We will use this file to create the tables in our database - for the purposes of making this easy to follow, we will use sqlite.

2/ Run the prompt from the Amazon Q Developer chat interface

> @workspace DEVSTYLE Create a data model for a simple customer survey application. Users will log in using an email addresses. Users will be able to create new surveys, or update/delete existing surveys. Users can create multiple surveys. Surveys can have between 2 and 5 options. Once a Surveys is created, the End Users will be able to submit feedback. 

Review the SQL - does it look right?

*Getting Amazon Q Developer to help explain code*

3/ Amazon Q Developer is integrated into the VSCode IDE which allows you to easily invoke it to help you with certain activities. For example, from the IDE open up the customer-feedback.sql we have just created and select all (CTRL + A). Right click on the menu and select "Amazon Q" and from the list of options, select "Explain".

In the Amazon Q Developer Chat interface, you should see a good explaination of the SQL and how it works. You can use this feature any time you have code that you might want a quick summary of how it works.

Copy and paste this into the file you just created. 

*Generating SQL*

4/ Run this modified version of the prompt. This time we have added "I want to code in Python" which should provide different output.

> @workspace MYSTYLE Create a data model for a simple customer survey application. Users will log in using an email addresses. Users will be able to create new surveys, or update/delete existing surveys. Users can create multiple surveys. Surveys can have between 2 and 5 options. Once a Surveys is created, the End Users will be able to submit feedback. I want the code in Python.

Review the output. Create a new file called "customer-survey.py" and paste the code into this file. Take some time to review the code, and use the Explain feature if needed. Before we run this code, it is likely you will need to install some Python libraries before you can run the code.

```
pip install sqlalchemy
```

Run the code and you should see a new sqlite database created in the local directory. If you have installed the VSCode plugin Database JDBC Client, you should be able to connect to this and see that the tables have been created.

Congratulations, you have created your data model, and we can now start to build the application code.

**Complete:** When your database now has your created tables and indexes, you can proceed to the next lab, [Part Two](building-our-app-part-2.md)


