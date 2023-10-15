# Chapter 3 : A Framework For System Design Interview

A good question you might have during the interview replying the the question "Design the product X" is "How could anyone design a popular product in an hour that took a team of engineers months or years to build?".

The good news that no one expects you to. The System design interview is not about designing a product from scratch. It is simulation the real life problem solving where two co-workers are discussing a problem and trying to find a solution.

The problem is open ended, and there are no prefect solutions. The interviewer's goal is to access your abilities.

Ans it is not all about technical skills, it is much more about your ability to collaborate with others, to communicate your ideas, work under pressure and resolve ambiguity and to ask good questions.

A good interviewer also looks for red flags. Over-engineering is a real disease of many engineers as they ignore tradeoffs, they are often unaware of the compounding costs of over-engineered systems, and many companies pay a high price for it.

## A 4-step process for effective system design interview

### Step 1 - Understand the problem and establish design scope

In a system design interview, giving out an answer quickly without thinking gives you no bonus points. Answering without a thorough understanding of the requirements is a huge red flag as the interview is not a trivia contest. There is no right answer.

Don't be afraid to ask questions. The interviewer will not give you all the details upfront. You need to ask questions to clarify the requirements and establish the scope of the system you are going to design, and don't jump write to give a solution, think deep and ask questions to understand the problem.

What kind of questions to ask? Ask questions to understand the exact requirements. Here is a list of questions to help you get started:

- What features are we building?
- How many users do we expect?
- How fast the product is expected to grow? How it will be in 3 months, 6 months and 1 year?
- What is the company's technology stack?

## Step 2 - Propose high-level design and get buy-in

Once you understand the problem, you can start proposing a high-level design. The goal of this step is to get buy-in from the interviewer. You want to make sure that you are on the same page with the interviewer before you dive into the details.

- Come up with an initial blueprint of the system and ask for feedback. Many good interviewers love to talk and get involved.
- Draw a block diagram with boxes representing the core components of the system. This will help you to visualize the system and communicate your ideas better.
- Do back-of-the-envelope estimation to make sure that the system you are proposing is feasible.

If possible, go through a few concrete use cases. This will help you to identify potential bottlenecks and tradeoffs. For example, if you are designing a social network, you might want to discuss how to handle a user with 1 million followers.

Should we include API endpoints and database schema here? This depends on the problem, for large scale systems (like design search engine like google), it is a bit too low level, for smaller systems (like design a URL shortener), it is a good idea to include them.

## Step 3 - Design deep dive

At this step, you and the interviewer have agreed on:

- The overall goals of the system.
- High-level design.
- Feedback on the high-level design.
- Initial ideas about areas to focus on based on the feedback.

Time management is essential as it is easy to get carried away with minute details that do not demonstrate your abilities. Try not to get into unnecessary details. Focus on the areas that the interviewer is interested in.

## Step 4 - Wrap up

In this final step, the interviewer might ask you a few follow-up questions or give you the freedom to discuss other additional points. Here are a few directions to follow:

- Discuss bottleneck and potential tradeoffs.
- Give a recap of the system.
- Error handling and recovery.
- Operation issues are important. How to deploy the system? How to monitor the system? How to scale the system? How to test the system? How to secure the system?
- How to handle next scale curve? What if the system grows 10x, 100x, or 1000x?

# Dos and Don'ts

To wrap up, there are a list of dos and don'ts that you should keep in mind:

## Dos

- Always ask for clarification if you are not sure about something.
- Understand the requirements and clarify the scope of the system.
- There are no right or perfect solutions.
- Let the interviewer know that you are thinking, communicate your ideas, and discuss tradeoffs.
- Suggest multiple approaches, discuss pros and cons of each approach, and explain why you prefer one over the others.
- Once you agree on a high-level design, dive into details.
- Bounce off ideas with the interviewer. The interviewer acts as your teammate.
- Never give up.

## Dont's

- Don't be unpreapred for typical interview questions.
- Don't jump into coding right away.
- Don't get into much details about one single component.
- If you get stuck, don't hesitate to ask for hints.
- Again, communicate your ideas and discuss tradeoffs.
- Don't think your interview is done until your interviewer says you are done. Ask for feedback early and often.
