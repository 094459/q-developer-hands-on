![Amazon Q Developer header](/images/q-vscode-header.png)

### Building our application - Working with Data

**Overview**

In this lab we are going to take a slight detour and spend some time looking at how next generation developer tools like Amazon Q Developer can help us be more effecient when working with data. We have already seen how we can use Amazon Q to help us scaffold data models, but we can do more than that. We are going to dive a little deeper into that topic before looking at how we can write queries, understand database applications written in SQL, show how Database System Administrators (DBA's) can benefit, explore the wonderful world of synthentic data generation, and more.

> **Additional Reading Materials**
>
> *Here are some additional reading materials that are useful to dive deeper into how large language models and generative AI developer tools like Amazon Q work with SQL. After you have completed this lab you might find them helpful if you want to have a deeper understanding of how they work
>
> * [Improving how Amazon Q writes SQL by using context](https://community.aws/content/2oft0CjVFvJFbRoPlLdUbi7o6hM/writing-sql-with-amazon-q-developer-workspace-context)
> * [Good practices for using text to SQL] (https://community.aws/content/2oRFLQEJj1RWA8OumcGD9lNv7JY/best-practices-for-text-to-sql-use-cases-with-llms)
>

---

**Task D1**

1/ We are going to first set our codebase to the following baseline so everyone is on the same page.

```
git checkout lab-d01
```

2/ Next we are going to use a VSCode plugin called [Database Client JDBC](https://marketplace.visualstudio.com/items?itemName=cweijan.dbclient-jdbc) which makes it super easy to access databases via graphical UI.

3/ Close all open files and tabs from within VSCode. From the Amazon Q Chat interface, enter the following:

> Create SQL code for a customer feedback application

Review the output. Keep the chat interface conversation open.

4/ Open up the **app.py** Python code so that it is displayed within the VSCode editor, and then repeat the prompt:

> Create SQL code for a customer feedback application

Review: How does the chat interface language change? What does the SQL look like? This is an example of how Amazon Q uses the file you have open to provide more context. This is very important as you use these tools to understand how to influence the output.

5/ Close the app.py so that you have no open files. Close the chat tab to close the conversation, and then open up a new chat interface tab. Type in the following prompt:

> Create a data model for a customer survey application in Python

Review the output. This time open the **customer-feedback.sql** in the data folder so that it is the only file open in VSCode. Enter the prompt again:

> Create a data model for a customer survey application in Python

Review: How does the output differ? Again, you can see how having a specific file open can help you get better output.


**Task D2**

In this task we are going to see how we can generate diagrams from our SQL, as well as generate SQL from our diagrams. We will need to install a VSCode plugin if you do not already have this installed. From the VSCode marketplace, find the following plugin

![Enhanced Markdown Viewer](/images/vscode-markdown-enhanced.png)

You can use [this link](https://marketplace.visualstudio.com/items?itemName=shd101wyy.markdown-preview-enhanced)

This will provde you with a new option when you right click on any markdown document, as follows:

![Enhanced Markdown viewer](/images/vscode-markdown-enhanced-fileopen.png)


1/ Open up the **customer-feedbck.sql** file and from a new chat interface, add the following prompt:

> create a mermaid diagram of this sql code that I can display in a markdown doc

Review the output. In the same directory (data) create a new file called **"customer-feedback.md"** and add then copy the contents into the file. If you try and display this as it is, it will not work ( try it ). We need to add the markdown tag so that it knows to use the Mermaid diagram extension. If you are new to this, we can easily do this with the help of Amazon Q.

2/ Select all the code in the newly create file, and then use the Amazozn Q menu integration (right click, Amazon Q > Send to Prompt). From the prompt, enter the following:

> Add the markdown to display this

After a while you should see some updated code. Overwrite the contents with the output from the chat inteface (after reviewing), and then try again with the enhanced Markdown preview. You should get something like this.

![documenting the application schema](/images/vscode-mermaid-show.png)



Building Data Models from sratch
-reverse engineer sql from data classes
-using yaml format to build sql
-using uml exports (mermaid diagram)
-migrating sql from one dialect to another
-personalising output for your preferred db format
-Explaining and understanding SQL code

---

**Task D1**

Writing Queries
-human/text to SQL
-improve on existing SQL and make suggestions

---

**Task D1**

DBA functions

-export/import data and tools
-troubleshoot issues and errors

---

**Task D1**

Generating test data

-create an app to generate sample data for our application
-ask it to look at the app and build a data generator that will create surveys, with three options with randomised results
-run it to create 10000 records
-export it to generate csv file
-update to create parquet and avro files

---

**Task D1**

Analytics - take some simple existing tutorials and modify these so we can create code and test it

Building Spark code

Analyse the sample data we created and create some code in Spark
https://spark.apache.org/examples.html
https://medium.com/hasifsubair/data-engineering-with-apache-spark-d4684fb6cf0b
https://pub.towardsai.net/a-practical-introduction-to-pyspark-2b29b88ec1d

---

**Task D1**


Creating DAGs

create a simple dag to process the csv file produced from the survey app
add new tasks to copy to an S3 bucket
ask chat on how you can test this

Data warehouse
add data into warehouse - https://clickhouse.com/docs/en/getting-started/quick-start


---

**Task Dxx**

We have now completed this data focused lab. We can proceed with building our application, but before we do that we need to clean up our working environment. Follow these instructions.

```
delete the working files
git stash
```
and now we can reset the repo as follows

```
git checkout lab-01
```

**Complete:** Now that you have had a chance to experience how Amazon Q can help you with data, you can proceed to the next lab, [Part Two](building-our-app-part-2.md)



