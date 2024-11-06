![Amazon Q Developer header](/images/q-vscode-header.png)

### Getting confident with Amazon Q Developer

Before we build an application, we will complete a few excercises that will allow you to get familiar with how the different capabilities of Amazon Q work. To get started we have a code repo that we will use. From a command prompt, create a new working directory and then run the following commands:

```
git clone xx
cd xx
code .
```

This will start a new VSCode instance with the files we are going to work with.

**Do you use other generative AI coding tools?**

If you currently are using a generative AI tool (e.g. Continue, Supermaven, Copilot, etc) then you may find that some of the keyboard shortcuts do not work. For the duration of this workshop, find the other AI coding tools you are using and temporarily disable them. To do this, go to the plugin section, locate your plugin and then from the settings select disable.


**1. Using in-line editor**

You can use Amazon Q Developer to help you as you write code within your editor. This section will walk you through how these work to provide you with confidence in how to get the best out of the tool.

*Basic functionality*

Amazon Q supports a number of different ways you can enter instructions (prompts) that it will use to provide code suggestions. We can summarise these as the following:

* Single line comment
* Multi line comment
* Single line prompt
* Multi line prompt
* Inline prompt

With the exception of the Inline prompt, after entering a prompt and hitting return, this will invoke Amazon Q and it will provide you with some code suggestions. You will get the opportunity to reivew the options using the < and > cursor keys. Occasionally there will only be a single code suggestion in which case you will not get any different output by using the < and > keys.

You will notice that the code suggestions are greyed out. To accept the suggestion, you hit TAB.

These code suggestions are automatic by default. The next section will walk you through how you can change this behaviour.


*Inline prompt*

Inline prompt works slightly differently. There are two modes you can use it in:

* From the either the position where your cursor is within the current open file
* Selecting a block of code 

The behaviour is similar. You invoke it with COMMAND + I, and this will bring up a command prompt window. If you do not see this, then there are two things you need to check:

1. Make sure that you do not have any other VSCode plugins that might be competing with this for COMMAND + I - when I was writing this, COMMAND + I did not work for me, and I had to temporarily disable another plugin that I was no longer using
2. Make sure the focus is in the actual file open - often I find that the focus is either in the chat interface, or the file explorer. I click within the open file to make sure

Once you enter the prompt, it will now start to work. You cannot select different code segments like the previous ways. You will see your code blocks highlight in Green/Red based on new or changed/deleted code, with an option to Accept (Enter) or Reject (ESC) the sugguestions.

I find this mode very powerful and improves the speed at which I can make updates/changes to my code. It tends to work well for specific ways you might want to work - updating existing code blocks for example, adding new functions, asking for optiisations, adding try / catch blocks, and more.


*Enabling and disabling auto prompting*

Something that you will need figure out as you start using Amazon Q Developer is whether you want it to automatically make code suggestions as you code, or whether you want to manually invoke it via some command line controls.

You can enable/disable the auto-suggestions by clicking on the "Pause Auto Suggestions" when you bring up the Amazon Q menu (click on the Amazon Q on the VSCode status bar at the bottom)

Spend some time experimenting with this. Open up a new file (experiment.py) and then at the top of it write the following:

\# Add Python library imports for Flask and SQLAlcamey

Hit return. You should see Amazon Q thinking (it will be greyed out) before providing you with some code suggestions.





*General considerations when entering prompts*

Avoid:

* Avoid single letter variable names
* Avoid generic names like data or value
* Avoid names abbreviations like val or num

Use:

*Use descriptive names like numberOfStudents or totalPrice
*Use names that are specific to the context of the code
*Use names that are specific to the type of the variable
*Use names that are specific to the value of the variable
*Use names that are specific to the purpose of the variable
*Use names that are specific to the scope of the variable (global, local, class, etc.)
*Use names that are specific to the lifetime of the variable (constant, temporary, etc.)
*Use names that are specific to the visibility of the variable (public, private, etc.)
*Use prompt directives to emphasize certain keywords in order to achieve desirable response.


**2. Using the VSCode Menu integration**




**3. Running a security scan**

Code generated by AI coding assistants should not be assumed to be free of defects or bugs. It is important therefore that we review the code as it is suggested by Amazon Q Developer. That is our starting point, but we can also use a great feature of Amazon Q which allows developers to run security scans on their code.

To do this, from the Amazon Q menu (click on the Amazon Q on the VSCode status bar at the bottom), and click on Run Project Scan. 

![Start Security Scan](/images/q-vscode-security-scan.png)

It will kick off a scanning job, and review your files.

![Kick off a security Scan](/images/q-vscode-sec-scanning.png)

After a few minutes (or longer depending on how big your project is) any findings will appear in the IDE (they will appear under the PROBLEMS TAB). If it finds no security issues, you will see somethign like this:

![No security findings](/images/q-vscode-no-security-findings.png)

If it does find something, it will list any issues in the PROBLEMS tab like this

![Security issue found](/images/q-sec-issue-found.png)

You can click on the link to take you directly to the issue, and you can also then use the integration with VSCode to get further informtion, including how to fix the issue. Occasionally Amazon Q will be able to suggest a fix automatically, and you will be able to use the Quick Fix to automatically address the problem.

![Review the issue](/images/q-vscode-finding-details.png)

![Review options](/images/q-vscode-finding-options.png)


In the repo we have provided a sample file (security-example.py) which contains an issue. Trigger a Security Scan and follow the steps to 1/ review the findings, 2/ use the explain and options links to see what suggestions are provided.

Implement any suggestions and re-run the scan again. You should find that you no longer have issues.


**4. Configuring and using @Workspace**

*View logs*

From VSCode, make sure you have the Terminal window open. From this select the OUTPTU tab, and then click on pull down menu. You will see multiple entries, there are two that are key for @workspace

* Amazon Q Logs - this will show you the index demon process starting and other high level details of how Amazon Q is working with @workspace
* Amazon Q Language Server - this provides the actual logs of when you are using @workspace

*Stop/Start*

When you are working with your projects, you might encounter occasional errors when using @workspace. One of the most common issues I have seen is where as the local project files change (perhaps due to git checkout) @workspace can sometimes provide incorrect guidance based on files that are no longer there. When this happens, I stop and restart the service. To do this:

1/ From the Amazon Q menu on the bottom of the VSCode status bar , select O'pen Settings"
2/ Navigate to the "Amazon Q: Workspace Index"
3/ Untick the box, wait a few seconds, and then re-tick the box.

When you click on the OUTPUT tab and then select the pull down menu, you may see multiple Amazon Q Language Server log entries. Try them all, and you should see in one of them, the indexing process starting.


*Clear out cache*

Occasionally stopping and restarting the Workspace Index will not resolve the issue. Opening the logs will show error messages (a common one I have seen is a Fais index failure). When this happens, it is often best to delete the cache and get Amazon Q to reindex. You can find the location of the cache by looking at the Amazon Q Language Server logs. Here is an extract from mine:

```
[Info  - 16:02:49] bm25 indexing complete, time: 0.92ms
[Info  - 16:02:49] start building tree index for /Users/ricsue/Projects/amazon-q/workshop/q-developer-workshop-supporting-repo/q-developer-workshop-demo-code
[Info  - 16:02:49] Finished parsing 1 python files. Time 15.07ms
[Info  - 16:02:49] start building vector index
[Info  - 16:02:49] Starting building vector index for 3 files
[Info  - 16:02:50] Vector index complete! Take 0.09364341600000001s. Total 3 files, 5 chunks
[Info  - 16:02:50] Vector index saved to /Users/ricsue/.aws/amazonq/cache/cache/eee4a16ba650a3c2a0cab92dd904c065feb5976341f32c94cd0be7fe5fcdd94a-0.9-VSCode.index
[Info  - 16:41:47] Adding file to vector index: /Users/ricsue/Projects/amazon-q/workshop/q-developer-workshop-supporting-repo/q-developer-workshop-demo-code/.qdeveloper/MY-PREFERENCES.md
```
From this we can see that **/Users/ricsue/.aws/amazonq/cache/** is the location where the Workspace Index will generate these indexes. These can be deleted as the index process will recreate them.To do this:

* Disable the Amazon Q: Workplace Index using the previous step
* From the terminal, locate the directory where the indexes are created and then delete them
* Re-enable the Workspace Index

**5. Chat interface**

*Scaffolding*

We can help steer the output of how Amazon Q works by providing scaffolds. There are two ways we can do this:

1/ Provide details in a markdown doc in our project directory, and refer to these in our prompts
2/ Provide them directly within the prompt itself

**6. Code Reference**

One of the questions you might be asking yourself is as Amazon Q Developer provides code suggestions, how much of that code might come from the open source projects used to train the large language models behind it. Amazon Q Developer allows you to configure this - you can either explicityly turn off any code suggestions that match code from open source repositories, or you can leave it enabled, and Amazon Q Developer will notify you. In practice I have found that I rarely encounter this.

Check out [this very short video](https://www.youtube.com/shorts/Fmn37wGQUY8) that shows what this looks like when it does happen.


**Complete!**

Ok, you are now up to speed with the Amazon Q Developer plugin, and it is time to see what you can achieve with it. Head over to the next section [Getting started building our application](building-our-app-part-1.md)