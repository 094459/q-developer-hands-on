![Amazon Q Developer header](/images/q-vscode-header.png)

### Building our application - Part Three (final)

In our final lab we are going to get our application ready for shipping. We are going to do a few things:

* Use the Amazon Q Security Scan feature to identify any issues with our code that we need to fix
* Add some Tests
* Look at what we need to do to make our project "Open Source"
* Add documentation to our project, creating a README file

**Lab 03-1 Running a Security Scan**

We have some code, but how robust is it? In this lab we are going to use a feature of Amazon Q Developer to scan the project and help us shift left with security. This allows us to proactively detect and then resolve issues in our code, closing that feedback loop. Lets see how this works.

We are going to checkout a branch of the application that we have just finished working on. Run the following commands

```
git checkout lab-08
```
**Task 16**

1/ Click on the Amazon Q on the bottom status bar to bring up the Amazon Q Developer options, and select "Run Project Scan" from the menu. You should see that this kicks off and a popup will appear in the bottom right of your VSCode.

This will take a minute or two, but it should come back when its finished. Review the output. You will see after review these, that some are valid issues that you need to work on, whilst others are how the application is supposed to work and are false positives which can be safely accepted (for example, you will get CWE-862 - Missing Authorisation which in this particular application is correct, as we want to make our surveys sharable and we do not need nor want those people providing feedback to need to authenticate first)

2/ You can use Amazon Q to help you understand your issues. For example, you can use the Amazon Q chat interface and ask questions like:

> Explain CWE-798 - Hardcoded credentials
> How do I fix CWE-94 - Unsanitized input is run as code

For some CWE's you will need to do enter the prompts, whereas for others, where the security scan has additional information to help you, you can use the Explain feature to provide that info within the chat interface.

![CWE hover](/images/q-vscode-cwe-highlight.png)

![CWE explain](/images/q-vscode-cwe-explain.png)

3/ Select CWE-94, and then from the light bulb icon, select Explain. Review the output from the Chat interface. Once you have reviewed the output, in the same chat interface tab, enter a prompt:

> How do I resolve CWE-94 in this app.py file

Follow the guidance and test out the changes. Run the application to make sure it works, and then re-run the security scan. You should have one less issue to address now.

3/ We can address the two remaining issues by again, using the chat interface. As we want to ask Amazon Q to help us with multiple files, @workspace is a good choice

> @workspace how do I fix CWE-798 in login.html and register.html

Review the code that is output. You will likely find this message comes up:

> I apologize, but I'm unable to respond further. Please see the AWS Responsible AI Policy. Perhaps we could find another topic to discuss?

What you are seeing is the Amazon Q guardrails kicking in. In this instance, these have triggered and whatever we try we will not get a complete response.

> /dev Fix CWE-94 issues within this application login.html and register.html

This time we get success. Review the code changes and implement. As you do this test the application, making sure that you can:

* login successfully
* register a new user
* logout

This might take some time as you will encounter errors as you implement the updates. Use the chat interface to help you address these issues and update the code accordingly.

If you want to review the code from the baseline project, exit the running application and then use the following to view the code.

```
git checkout lab-09
```

**Task 17**

We now have a working application, but what we do not have is any tests. In this lab we are going to create some, using one of the advanced features of Amazon Q Developer, the Agent for software development (or /dev). There is a bit of waiting whilst this is working, so feel free to grab a refreshment while it is running or stretch your legs!

We will start from a working version of our code.

```
git checkout lab-09
```

If you are new to writing tests, we can get some hints from Amazon Q Developer in the Chat interface by typing the following prompt:

> What are the most popular Python testing frameworks that might be suitable for this project

This might be useful to help us better understand how to narrow down our choices, understand trade offs, and then let us start to write those tests. We can also get Amazon Q Developer Agent for software development to help us. By invoking this using the /dev, we can then ask it something like:

> /dev Create unit tests for this application that test registering a user, creating a survey, and submitting feedback on a survey

Do **NOT** accept the code when this has finished. We are going to use the Provide feedback and regenerate option in the next step.

> **Help!** As you only have limited use of /dev with the free Builder ID's, if you have exceeded your quote then you can try this prompt using @workspace as well. You will need to review all the output and make the code changes yourself, so this will take you longer.

After we hit return, watch as Amazon Q Developer Agents get to work. This will take around 4-5 minutes again, and once complete you will get a chance to see and review the code it wants to create. You have three chances to ask Amazon Q to refine or update its response. Sometimes you need to do this - it might have missed some files, or perhaps omitted one of your requirements. When it does that, make sure you use the "Provide Feedack and Generate".

> **Note!** As of writing, using Amazon Q Developer Agent provides you with three follow up prompts you can use to help tailor the output. Why is this important? Using /dev is an advanced feature of Amazon Q Developer, and there is  limited quota when using the Builder ID and so you need to make sure that if you do use it, you use up your allocated follow ups before you use another of your allocated units of /dev. You can increase that quote by using the Professional Tier of Amazon Q Developer - this workshop however assumes you are all using the basic Builder ID version.

In my example, the test code looked ok but was using Flask test and I forgot to put in my prompt that I wanted it to use pytest. So after clicking on the "Provide Feedack and Generate" button, I enter in the prompt:

> Can you update the code so it uses pytest

This will invoke Amazon Q Developer Agent again, and it will take another 3-4 minutes as it did before. Once the Amazon Q Developer Agent for software development finishes, I get updated code that this time uses pytest. Awesome, I click on the Accept Code after a quick review and now go to try it out. I now have a test_app.py in my root folder with my tests. We can now ask Amazon Q Developer how to run this.

> @workspace how do i run the tests in test_app.py

Review the output you get. 

Try running the code that you generated. Does it generate any errors or do all your tests work ok? It is very likely you will enconter issues at this stage. Another good reason for Test Driven Design (TDD). You will need to review the errors and use Amazon Q Developer to help you update the code, and then retest. This is an iterative process.

Use the output from the test errors to help guide you to both fix the tests but also adjust the application code so that it can be tested. For example in the login and register routes, you might want to think about

```
    if app.config.get('WTF_CSRF_ENABLED', True):
        app.logger.info(f"CSRF Token: {form.csrf_token.current_token}")
    else:
        app.logger.info("CSRF Protection is disabled.")
```

and then adding this to your Pytest configuration file as a way of switching this off.

Do not spend too much time in this lab - you may not get all the tests running, but see if you can get some working ok. If you get stuck, then you can see the working code by checking out the [following branch](https://github.com/094459/q-developer-workshop-demo-code/tree/lab-10)

```
git checkout lab-10
```

**Task 18**

Now that my project is ready for the world to see, I have decided that I am going to open source this project as I want to build a community of people who might be interested in helping me improve the code and make it better.

You can ask Amazon Q Developer to provide some guidance on what I need to do.

> **Warning!!** This guidance should be used in conjunction with conversations you have with any folk that you work with on Intelectual Property. This guidance here is only to show you how Amazon Q Developer can help you ONCE you have had those conversations and decided that open source is the way you want to go.

As I think about the best way of using Amazon Q to do this, this is going to need to review all the files in the workspace. There are two ways I could approach this. Using the native Amazon Q Developer Chat interface, or by using the Amazon Q Develoepr Agent for software development (/dev). I do not really want to use one of my /dev quotas, so I decide that I am going to use @workspace, and this is the sample prompt I use:

> @workspace I want to open source this project using an Apache 2.0 licence. My company name is Beachgeek Enterprises so please update any copyright messages accordingly. How do I need to update this projects files?

I am happy with the output it provides (see below), but I think it could be improved.

![example output of helping me open source my project](/images/q-vscode-open.png)

I follow this up with some more specific information:

> I want to include SPDX headers in the files, what do these look like

You might need to do this if you have any specific requirements you need to meet that come out with conversations with your open source program office (OSPO) - if you have one.

Explore this and update the code.

If you get stuck, or want to see the completed sample I came up with, check out the [branch here]() or you can switch branch using the following command:


**Task 19**

Documentation is a critical part of every application, and yet sometimes this is lacking in the code and projects we have to work with. We can use Amazon Q Developer to help us automate this task, updating both documentation in code, as well as creating README files to help others understand how to use this code. Lets create some documentation for our project.

We are going to do this two ways - first using @workspace to create the documentation which we will manually have to add, and then using /dev to automate the process of creating the documentation. We are first going to make sure we are all on the same page code wise:

```
git checkout lab-011
```

From a new Amazon Q Developer Chat interface, enter the following prompt:

> @workspace Add doc strings to the app.py and document how the code works, providing doc strings for each function and each route

Review the output. This is what mine looked like.

![output from using @workspace to create documentation](/images/q-vscode-document.png)

I could now go through and update the code, using the "Insert Code" link that you see at the bottom of the output window. I follow this up by asking Amazon Q Developer to create me a README file, showing how the project works, how to run it, and how to use it.

> @workspace create a README file that tells readers how the project works, how to run it, and how to use it.

I was very happy with the output, but maybe there is an easier way. Well there is, and again we can use Amazon Q Developer Agent for software development (/dev) to automate this for us. We invoke /dev from the Amazon Q Developer chat interface, and then enter this prompt.

> /dev Add doc strings to the app.py and document how the code works, providing doc strings for each function and each route. Create a README.md file that tells readers how the project works, how to run it, and how to use it.

I let it run for 3-4 minutes, and after it has finished, I can review what it has done, and the output. I can see it has created a new file (README.md) and it has updated my existing app.py file.

![Amazon Q documenting my project](/images/q-vscpde-readme.png)

Take a look at your output. What do you think? If you are not happy, remember to use the "Provide Feedack and Generate", and use additional prompts to tweak the documentation as you want or need it. When I ran this, I used this feedback loop to refine the README around how to create and configure the database as the initial output was not quite right (it referred to using a tool called flask db, which I did not have installed)

**Complete:** You now have completed thie workshop, congratulations. 


You can return back to the [Starting Page](README.md) to find out about additional resources that will help you stay on top of new updates and improvements with Amazon Q Developer.

We are driven by customer feedback, and I would appreciate you also completing a feedback form before you go.
