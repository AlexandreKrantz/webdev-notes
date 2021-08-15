# Introduction

## How this Course Will Work

- DO NOT SKIP ANYTHING. _Everything_ in the curriculum is intentionally included and vital for you to become a successful programmer.
- Additional resources are the only thing that are considered optional unless explicitly stated. These are here in case you feel like you need or want to dive deeper into a topic to get a better understanding.

## Introduction to Web Development

### Strategy for finding first web development job

1. Start building assets/advantages that make you more hirable
   - Network of professionals that can vouch for you. Attend local tech events to meet people. Do small jobs for work experience. If you can get a referral from a past employer or someone in the industry it will make you more credible.
   - Personal website and LinkedIn so you are discoverable.
   - Projects that showcase your technical ability.
2. Collect information on a large number of job openings to get a feel for the market
   - Send a nicely worded email to everyone in your network informing them of your current situation, the kind of work you're looking for and a link to your personal homepage where hirers can find out more about you.
   - If working with a recruiter, be wary of their motives. _Secrets of Power Negotiating_ is a recommended read.
3. Prioritize the leads based on a set of criteria, personal to you
4. Apply to the most attractive leads based on your criteria with a CV, and covering letter.
5. Prepare for and give a good interview when invited
6. Only make a decision when you have at least three offers in hand. This helps you avoid the worst case scenario of an offer that is below the market rate.

## Motivation and Mindset

### The Learning Process

Work consistently and not for too long at once. You don't want to sink entire days into this. Enjoy the process don't rush. As you learn new concepts you need to give your brain ample time to make connections. Mush of this is done subconciously when you are doing something else, so it is better to space out your learning and do a little bit every day.

## Asking For Help

### Checklist

1. Understand the code to the best of your ability
   - Map out what each line does and google anything you are unsure about.
   - Use a debugger.
2. Clearly describe the problem
   - Explain the context of what you are trying to do.
   - Explain the exact steps you took to produce the problem.
   - Explain what you expect to see.
   - Explain what you actually see.
3. Provide code
   - Only what is broken or relvant to the problem.
   - Make sure you don't modify this broken code while you wait for an answer. Create a new branch.
   - Double check that the snippet of code you're sharing can reproduce the problem.
   - Make sure the code is formatted consistently.
   - Check carefully for typos.
4. Explain what you did to troubleshoot the problem.
   - Come up with a list of hypotheses about what the problem might be and then test them methodically. For each hypothesis, explain what you did to test each hypothesis.
5. Explain what you think the problem might be based on your tests in the previous step.
6. Proofread your question.
7. Send updates and remember this will not be your last question.
   - If you’ve figured out the answer before anyone can respond, then tell people so they don’t waste time looking for an answer you’ve already found.
   - When you get an answer back, take time to digest it carefully and fully understand what the person is saying. Verify that their solution actually works.

### [The XY Problem](https://xyproblem.info/)

- User wants to do X.
- User doesn't know how to do X, but thinks they can fumble their way to a solution if they can just manage to do Y.
- User doesn't know how to do Y either.
- User asks for help with Y.
- Others try to help user with Y, but are confused because Y seems like a strange problem to want to solve.
- After much interaction and wasted time, it finally becomes clear that the user really wants help with X, and that Y wasn't even a suitable solution for X.
  The problem occurs when people get stuck on what they believe is the solution and are unable step back and explain the issue in full.

### [StackOverflow Best Practices](https://stackoverflow.com/help/how-to-ask)

- Title should summarize the specific problem. Write it last.
- Create an mwe (minimal workable example).
  1. Create a new program, adding in only what is needed to see the problem. Use simple, descriptive names for functions and variables – don’t copy the names you’re using in your existing code.
  2. If you’re not sure what the source of the problem is, start removing code a bit at a time until the problem disappears – then add the last part back.
  3. Use an online complier so that the program is not affected by your local variables like environment, installations and OS (JS Fiddle).
- Use spaces instead of tabs
- Use individual code blocks for each file or snippet you include. Provide a description for the purpose of each block.
- DO NOT use images of code. Copy the actual text from your code editor, paste it into the question, then format it as code. This helps others more easily read and test your code.
- Include all relevant tags
- Including links to related questions that haven't helped can help others in understanding how your question is different from the rest.

### Odin Project Discord Best Practices

- Use markdown to format questions and code
- Type `/help` for more information on chat commands.
- Show your appreciation to those who help you with `@username ++`.
- Dont send multiple messages in a row, type out your whole message, then push send.
- Use `https://replit.com/` for live code collaboration

## How Does the Web Work?

### How the Internet Works
- A router is connected to multiple computers and allows them to communicate by directing messages to their intended recipient. You can connect multiple routers to create a larger network.
  - This is sufficient to create a local network.
- The internet is a global network of computers. Routers around the world are connected using the telephone infastructure of cables.
  - A modem is required to turn inforomation from the router into information manageable by the telephone infastructure and vice versa.
  - An Internet Service Provider (ISP) acts as a gatekeeper to the internet. They manage a network of routers that is connected to other ISP networks. Via an ISP you can send a message to any other computer that is connected to an ISP.
    - The internet consists of the global network of ISP networks.
    - If you are connected to an ISP, you are (indirectly) connected to the internet.
- An ISP will assign each computer on their network a unique indentifier known as an IP address (IP stands for "internet protocol"). It is a series of four numbers separated by dots, for example: `192.168.2.10`.
  - The IP address indicates the location of your computer on the network. It is not linked to your computer's hardware in any way.
  - Your IP address is different each time you connect to the internet from a different place. If you restart your router, it is possible that your ISP assigns it a different IP. It refers to the location of the _connection_ to the network, not the hardware itself.
  - Domain names are just human readable aliases for IP addresses.
- _Web servers_ are computers connected to the internet that send data intelligible to web browsers. The web is a service built on top of the infastructure that is the internet.
  - Email is another example of a service built on top of the internet.
- Intranets are private networks not connected to the internet. Extranets are private networks only partly open to the internet. They both use same infastructure and protocols as the internet.

### How Does a DNS Request Work?
- Domain Name Servers are special servers that match up domain names to IP addresses and redirect client requests to the correct server.
- If the IP address for a domain is saved in cache, your computer sends a request directly to the web server. Otherwise, it gets the IP address from a DNS and then sends a request to the web server.

Really good vid: https://youtu.be/72snZctFFtA