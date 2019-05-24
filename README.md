# DotNetCoreWebExample

This is an example project that shows step by step how to build a simple web application with asp.net core. 

## Result

The example is a simplified version of www.sli.do. That is a website for audiences to ask questions to the speaker.

It shows a list of questions. The users can add new questions to the list and vote for questions. 

## Used server side frameworks

* Dotnet core 2.2
* Asp.net core 2.2
* Swashbuckle 4.0.1
* Entity framework core 2.2
* Asp.net SignalR core 1.1
* XUnit 2.4.1
 
## Used client side frameworks
* Twitter Bootstrap 4.3.1
* Vue.js 2.6.10
* Axios 0.18

## Steps

To recreate this project, the following four steps have to be followed. To get a detailed description of each step, click on the corresponding header.

### [Step 1 - Setup the project](Step1.md)

Create a new projects for the WebApi and the Unit Tests.

### [Step 2 - Web Api implementation](Step2.md)

Add the implementation for the database access, commands, queries, unit tests and add the swagger ui.

### [Step 3 - Add web site](Step3.md)

Add the implementation of the web site.

### [Step 4 - Client Refresh](Step4.md)

Add the implementation for the server-to-client communication for the page refresh.