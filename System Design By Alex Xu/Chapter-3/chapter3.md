# CHAPTER 3: A FRAMEWORK FOR SYSTEM DESIGN INTERVIEWS

System Design is for us demonstrating our skills, defend your design choices, and respond to feedback in a constructive manner

What are the goal of system design interview:

<ul>
    <li>Evaluating a person's technical design skills</li>
    <li>Evaluating a person's ability to collaborate, to work under pressure, resolve ambiguity and to ask questions.</li>
    <li>Do this person have over-engineering problem, narrow mindedness, stubbornness, etc.</li>
</ul>

## A 4-step process for effective system design interview

### Step 1 - Understand the problem and establish design scope

Dont rush and slow down think deeply and ask questions to clarify requirements and assumptions.

One important skill is to ask the right question, make proper assumptions, and gather needed information.

A list of question can be asked if the interview lets you to make assumptions.

<ul>
    <li>What specific features are we going to build?</li>
    <li>How many users does the product have?</li>
    <li>How fast does the company anticipate to scale up? What are the anticipated scales in 3 months, 6 months, and a year </li>
    <li>What is the company's technology stack? What existing services you might leverage to simplify the design</li>
</ul>

### Step 2 - Propose high-level design and get buy-in

In this step, we might want to develop a high-level design and make some agreement with your interviewer.

<ul>
    <li>Make an initial blueprint for the design. Ask for feedback</li>
    <li>Draw box diagrams with key components like clients, APIs, web servers, data stores, cache, CDN, message queue, etc.</li>
    <li>Do back-of-the-envelope calculations</li>
</ul>

Go through a few use cases to see if there is any edge cases existing.

It depends if we want to include our APIs endpoints and database schema here.

Example: "Design a news feed system".
<img src="https://github.com/matoanbach/System-Design-Notes/blob/main/System%20Design%20By%20Alex%20Xu/Chapter-2/pics/figure3-1.png">

<img src="https://github.com/matoanbach/System-Design-Notes/blob/main/System%20Design%20By%20Alex%20Xu/Chapter-2/pics/figure3-2.png">

### Step 3 - Design deep dive

At this stage, we will identify and focus on key components.

Key things that you can focus on:

1. High-level design
2. System performance characteristics
3. Dig into key components

Examples:

1. For URL shortener - the hash function shortening the URLs
2. For chat system - reduce latency and supporting online/offline status

Example: Design a news feed system

1. Feed publishing
2. News feed retrieval

<img src="https://github.com/matoanbach/System-Design-Notes/blob/main/System%20Design%20By%20Alex%20Xu/Chapter-2/pics/figure3-3.png">

<img src="https://github.com/matoanbach/System-Design-Notes/blob/main/System%20Design%20By%20Alex%20Xu/Chapter-2/pics/figure3-4.png">

## Step 4 - Wrap up

In this step, the interviewer will asl a few follow-up questions. Here are what you should do

<ul>
    <li>No system is perfect. So you should always find out what are your system bottles and then discuss potential improvement.</li>
    <li>A recap of you design is recommended.</li>
    <li>Remember to talk about error cases (server failure, network loss, etc.) </li>
    <li>Mention how can we monitor our system with monitor metrics and error logs</li>
    <li>Try to find a potential way to scale up. For example, what changes are needed if we want to support 10 million users</li>
    <li>Propose other refinements you need if you had more time</li>
</ul>

Summarization of Dos and Don'ts.
Dos

<ul>
    <li>Make assumptions and then ask for clarification</li>
    <li>Make sure to understand the requirements of the system</li>
    <li>No right answer and no best answer is ever existed. </li>
    <li>Think out loud and communicate.</li>
    <li>Suggest multiple approaches if possible</li>
    <li>Dive into each components once you have the blueprint in hand</li>
    <li>Discuss with the interview as your teammate.</li>
    <li>Just don't give up</li>
</ul>

Don'ts
<ul>
    <li>Don't be unprepared for typical interview questions</li>
    <li>Don't jump into a solution without clarifying the requirement and assumptions</li>
    <li>Don't dig too deep unless you are asked</li>
    <li>Don't hesitate to ask for hints</li>
    <li>Don't think in silence</li>
    <li>Don't say done. Only the interviewer says done</li>
</ul>

#### Time allocation for each step

System design interview questions are usually within 45 mins.

<ul>
    <li>Step 1 - Understand the problem and establish the design scope: 3 - 10 minutes</li>
    <li>Step 2 - Propose high-level design and get buy-in: 10 - 15 mins</li>
    <li>Step 3 - Design deep dive: 10 - 25 minutes</li>
    <li>Step 4 - Wrap up : 3 - 5 minutes</li>
</ul>
