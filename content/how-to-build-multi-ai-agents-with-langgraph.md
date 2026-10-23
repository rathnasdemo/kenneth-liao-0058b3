---
schemaId: urn:gala:schema:content-frontmatter:2.0.0
schemaVersion: 2.0.0
id: 01a168c7-55a5-7add-a098-cb266ab2bf9c
kind: article
title: How to Build Multi AI Agents with Langgraph
description: How to build a supervisor multi-agent system in LangGraph, and when a multi-agent design is the right choice. A researcher and a copywriter are managed by one supervisor.
language: en-US
authors:
- 01a168e1-ab3d-763f-bac9-92638c7ec5cd
tags:
- langgraph
- multi-agent
- ai-agents
- supervisor-pattern
- python
status: published
createdAt: '2025-09-27T12:00:00.000Z'
publishedAt: '2025-09-27T12:00:00.000Z'
slug: how-to-build-multi-ai-agents-with-langgraph
hero:
  path: assets/content-how-to-build-multi-ai-agents-with-langgraph-hero.jpg
  alt: Thumbnail of the video "How to Build Multi AI Agents with Langgraph"
  role: informative
redirects: []
extensions: {}
---
*Adapted from the video [How to Build Multi AI Agents with Langgraph](https://www.youtube.com/watch?v=rwqGQEzXF-o)*

> **TL;DR:** Multi-agent systems are not always the right tool, so I build the supervisor design pattern in LangGraph and explain when to use it. A supervisor talks to the user and hands tasks to a researcher and a copywriter, each with its own tools, prompts and context. If a problem can be hard coded, build a workflow; if it is open-ended, use an agent; if one agent bottlenecks on context or tools, split into several. The code uses subgraphs and the Command primitive.

I am going to show you how to build a multi AI agent application in LangGraph. 2025 has definitely been the year of agents. We have not only seen the potential but proven value from AI systems like Claude Code, and they will only become more capable and prevalent as the models get better. But multi-agent systems are not always the right tool for the job. There are specific use cases where the multi-agent approach is the best and even the only solution to a problem.

So I want to focus on the supervisor multi-agent design pattern. If you watch through to the end, you should have a better understanding of when and how to implement it. As always, all of the code is available for free at the AI Launchpad repo, linked in the description.

## The system we are building

*[Watch from 0:51](https://www.youtube.com/watch?v=rwqGQEzXF-o&t=51s)*

We build a supervisor that is the only agent interacting with us, the user, and that manages two other agents, a researcher and a copywriter. Each has its own tools, system prompts and so on, so the context is split.

- **The researcher** can search the web, extract specific raw content from any URL, and generate a research report. Reports are saved to the central state managed by the supervisor.
- **The copywriter** can review the research reports the researcher generated, which give it factual, grounding context, and write optimized LinkedIn posts or blog posts.

We keep it simple with just these two, but you can imagine adding more content sources.

## Demo: a research-only request

*[Watch from 2:49](https://www.youtube.com/watch?v=rwqGQEzXF-o&t=169s)*

Remember that we only speak with the supervisor. We never interact with the researcher or the copywriter directly. If a question is only about research, the supervisor routes the request to the researcher and gives us the response when it gets the output.

I asked what the top five US states by total land area are. The supervisor called a handoff to sub agent tool, with two inputs: the agent name, the researcher, and a task description. The supervisor expanded my basic prompt with detail, asking for an ordered list with each state's land area in square miles and citations of authoritative sources like the US Census Bureau or NOAA.

The researcher searched the web, then extracted the raw content of a Census web page. A plain web search only gives you a preview of the results in a short summary, so extraction gets the full contents. It then called the generate research report tool to save the report locally and responded in markdown with a summary and citations at the bottom, which come from the system prompt. I checked the Census data: Alaska is number one with 570,000 square miles, broken out by water and land, so the report is grounded in real data.

That matters. If you write content, especially news such as AI news, you want everything grounded in facts and not made up, so citation creation is important for any reports you generate. The supervisor then sees the output and summarizes the findings for us.

## Demo: research plus a LinkedIn post

*[Watch from 6:12](https://www.youtube.com/watch?v=rwqGQEzXF-o&t=372s)*

The point of the system is to write content, so the supervisor handles research, generates reports through the researcher, and feeds them to the copywriter, who writes high quality content backed by real sources. For this I used a prompt from the repo asking it to write a LinkedIn post on the top AI tools that small businesses and entrepreneurs need to scale, with real world examples and case studies with real numbers, and a call to action at the end for readers to follow me for more actionable playbooks.

The supervisor first handed off a research task, labeled research task number one. The supervisor's system prompt tells it to break complex tasks into concrete, specific research sub tasks, and the researcher focuses on one task until the supervisor asks for a follow-up or the next one. The researcher ran multiple queries to take different angles, such as top AI tools for small businesses across marketing, sales and customer support, and generated a first report: a 12 item list of AI SaaS apps useful to small businesses.

The supervisor then moved to task number two, finding real world use cases and case studies with real numbers. The researcher did about ten web searches and generated a second report.

## The supervisor steers its sub agents

*[Watch from 10:27](https://www.youtube.com/watch?v=rwqGQEzXF-o&t=627s)*

The supervisor was not satisfied with that output, so it asked again with a more specific request about small businesses, and then once more for six to eight case studies across varied industries, SMBs using AI tools at scale. The third report was far more comprehensive, with a breakdown by company and the numbers we originally asked for.

This is where the supervisor steers the outputs of the sub agents, which is really important in these systems. You typically use a more powerful model for the supervisor. The sub agents work on very specifically defined tasks, so you can usually get away with cheaper, less performant models, but the supervisor is how you get this kind of course correction.

Finally, the supervisor saw a satisfactory report and handed the copywriter detailed instructions. The copywriter reviewed the research reports, which were saved to the state, and generated the LinkedIn post: a hook for founders who treat AI as nice to have, a focus stack with recommended tools and typical impact, measurable outcomes, real world vendor case studies with real numbers, a seven day founder playbook, and the call to action I asked for. The supervisor then summarized everything. It ran for probably three to five minutes. I am pretty happy with the post, though it should be shorter: I would shorten the sections describing the tools and keep a short list with the numbers to back it up. Overall it is a nice example of pairing research with content writing.

## Workflow, agent or multi-agent

*[Watch from 13:02](https://www.youtube.com/watch?v=rwqGQEzXF-o&t=782s)*

You could solve this very narrow task, research a topic and use it to write a LinkedIn post, with a predefined workflow. There are pros and cons, so it is important to understand the tradeoffs and when to use an AI workflow, an AI agent or a multi-agent system. My one-line guideline is this.

- **A workflow.** If the problem can be solved in a well-defined set of steps, meaning you could hard code it (one function does one thing and feeds its output to another, also known as prompt chaining), build an AI workflow. Solve problems in the simplest way possible, because that is the fastest path to results. It is usually easier to implement, debug, manage and optimize, and you get a more deterministic, repeatable solution with much better reliability, which is crucial for production.
- **An agent.** If the problem is open-ended and it is hard to know beforehand all the steps required, so you cannot hard code it, an agent makes sense. The most ubiquitous example is coding. You might write code, find a bug and debug it, search for docs, write and run a test, or write, read and edit files, so the set of steps is basically infinite.
- **Multiple agents.** If you have a good problem for an agent and it is complex enough that a single agent bottlenecks, that is a candidate. One case is too much context that is important and required, but too much for the context window for the agent to perform well, because you can separate context between agents. Another is too many tools for a single agent to handle.

For workflows and agents, the best guide is the Anthropic blog post on building effective agents. There is also a video, linked in the description, where I built all the workflows and the agent from that post in LangGraph. That project is in the AI Launchpad repo too.

## Why use multiple agents

*[Watch from 17:04](https://www.youtube.com/watch?v=rwqGQEzXF-o&t=1024s)*

There are four main benefits of multi-agent systems over a single agent.

1. **Efficiency and breadth.** Multiple agents can work on different aspects of a problem in parallel, which greatly reduces overall latency and covers a lot more ground in less time. In research, a supervisor kicks off tasks, spawns research sub agents that each look into a sub topic in parallel, gets back all the reports, and synthesizes the final output.
2. **Better context management.** Agents work on a specific task instead of cramming everything into one. In our example, putting the system prompt, tools and all the research reports for both research and content writing into a single agent would take up a huge context and be confusing. Separating them keeps agents focused with fewer distractions, which improves performance and reliability.
3. **Scaling token usage.** Several studies show that the more reasoning tokens models are given, the better they are at solving tasks. A paper shows accuracy going up as the number of thinking tokens increases. That holds to a certain extent: too much thinking can saturate performance and eventually worsen it. Up to a standard amount, though, more reasoning tokens give a high improvement. With multiple agents we have multiple context windows, each with its own reasoning tokens, so we effectively multiply the reasoning tokens across the whole system. The context window scales with the number of agents, which leads directly into how much work can be done.
4. **Modularization and reusability.** Building agents separately and optimizing each for one task lets you use the right model, tools and prompts for it, and update and optimize each independently without affecting the others. You can also reuse them: a highly optimized research agent could serve a content creation system, a competitor analysis system or a customer prospecting system, with minor tweaks.

That was a lot of theory, but I think it is important to step back and understand the tradeoffs of different AI solutions and when and why to use them. A quick plug: I spend a lot of time on the reports I write up in the AI Launchpad repo's readmes, so do not sleep on them.

## Setting up the AI Launchpad repo

*[Watch from 22:11](https://www.youtube.com/watch?v=rwqGQEzXF-o&t=1331s)*

The AI Launchpad repo has all the code templates, examples and tutorials from this channel. You only need to set it up once and then pull each time a new project is released. Starring the repo helps my channel, as does liking the video. The getting started section has four easy steps.

1. Install UV, your Python project manager.
2. Clone the repo.
3. Install the virtual environment and dependencies with one command, UV sync.
4. Set up your API keys. This project needs only two: the OpenAI API key and Tavily for web search.

We work in the LangGraph module, in the new multi agent folder, which holds the supervisor project.

## Subgraphs versus agents called from tools

*[Watch from 23:40](https://www.youtube.com/watch?v=rwqGQEzXF-o&t=1420s)*

LangGraph is a lower level framework than many other agent frameworks, so you can build the same system in different ways. For multi-agent systems, you can call agents from tools, or build each agent as a graph and use it as a subgraph in a larger graph. I do the latter, because there are plenty of tool-calling examples online and in this repo, and I think it is important to understand how subgraphs work. It is also much better for observability and tracing: you can follow every node execution and tool call inside each subgraph, whereas if you call a sub agent from an MCP tool or some online API, you cannot. If you build the multi-agent system all in one place, go with the subgraph approach.

I assume you are somewhat familiar with LangGraph, but this is still beginner friendly. I focus on the supervisor multi-agent specifics, and I have an introduction to LangGraph video linked in the description if you want the basics.

## The researcher

*[Watch from 25:37](https://www.youtube.com/watch?v=rwqGQEzXF-o&t=1537s)*

Each agent loads its system prompt from a prompts folder, one per agent. The researcher prompt is a markdown file explaining the role, the available tools (search web and extract content from web page), and a report format: a topic, which is the title, and the report itself following an example structure. That is why you saw the citations section at the bottom.

The search web tool uses Tavily web search. Since the raw search returns many fields, I curate what goes to the model: a processed results dictionary with the title, the URL and a preview of the content. If the agent wants more, it extracts the full raw contents of the page with the Tavily extract object. Then there is the research report tool, which returns a research report object with a topic and a report.

## The Command primitive

*[Watch from 27:12](https://www.youtube.com/watch?v=rwqGQEzXF-o&t=1632s)*

For the report tool, what matters is that it returns a Command object. It is a newer construct in LangGraph that maybe not a lot of people understand. It takes four input arguments.

- **graph.** The graph to send the command to. If you put nothing, it is the current graph.
- **update.** A mapping between the graph's state keys and the new values.
- **goto.** A string telling the graph which node to go to next from this node.
- **resume.** Used if you hit an interrupt, such as human in the loop. You supply a resume value and the graph resumes, passing it to another node.

The tool takes in a topic and report, generated by the LLM, so they need good descriptions, and creates a Pydantic class for the research report. The researcher state has the normal messages key and a research reports key, set to an empty list; each new report is appended. In the tool we use only the update argument: it updates research reports with the new report, and it adds a tool message holding the report as a JSON string, with the same tool call ID the tool was called with. Normally a basic tool just returns a string, but returning a Command lets us update the state.

The rest is standard LangGraph: create the tools list, bind the tools to the LLM, and define the researcher agent node, which invokes the LLM with the system prompt and all the messages in the state. You may wonder why the node returns a dictionary and not a Command. Nodes in LangGraph are always state in, state out, so a node just specifies which key to update, here adding the LLM response to messages. The research router checks for tool calls and routes to the tools node, otherwise to the end.

We build the state graph, add the researcher and tools nodes, add an edge from tools back to the researcher, so the agent can review the output of the tool and decide what to do next, and add the conditional edge for the router. Normally you would pass a checkpointer, usually an in-memory saver for short-term memory. As a subgraph it does not need one, because a checkpointer on the parent graph gets passed through to the subgraphs. I left it commented out to make that clear. You can uncomment the two lines at the bottom to visualize the graph, which is a simple two node graph.

## The copywriter

*[Watch from 33:46](https://www.youtube.com/watch?v=rwqGQEzXF-o&t=2026s)*

The copywriter is simpler. It loads its system prompt and a couple of markdown files with example LinkedIn and blog posts, which I generated with ChatGPT and put in an example content folder. The system prompt tells it to write posts that look like these.

Its state is identical to the researcher's, though it does not have to be. That is the beauty of multiple agents: state, prompts and tools are all separate. We share the research reports key because we want the copywriter to see the reports the researcher generates, so that key is shared by all three agents, including the supervisor.

It has three simple tools. Review research reports annotates the copywriter state with the injected state class from LangGraph, which injects the current graph state into the tool, and prints the current reports in the list. The two content tools take a title and the post content, write it to a local file in an AI files directory, and return a string telling the agent the post was generated and where it was saved.

If this is your first time building with the new GPT-5 models, you can specify the reasoning effort as minimal, low, medium or high. You will notice a significant latency increase going to low, medium or high. I use minimal here, which is almost instant, and I think zero reasoning tokens if I remember correctly from the API logs. To make the model slightly more powerful without moving up to the next state-of-the-art GPT-5 model, you can go to low or medium, at the cost of latency. The rest is like the researcher: agent node and tools router.

## The supervisor prompt

*[Watch from 37:22](https://www.youtube.com/watch?v=rwqGQEzXF-o&t=2242s)*

The supervisor is the final and most important part. Its system prompt is more comprehensive, and I spent more time on it, because the supervisor is the facilitator and main agent that dictates how good the outcomes of the overall system are. Optimize all the system prompts, but this one is more complicated: it explains the core capabilities, gives guidelines for content creation with the copywriter, and for assigning research tasks to the researcher.

It has conversation guidelines outlining the tools. This supervisor has only one tool, the handoff tool, to simplify the example; you can add more. I have built many systems where the supervisor can hand off and also do a lot on its own. It outlines the agents and when and when not to use them, which is really important for any tool description. It has examples of an overall user request and how to complete it. It should always first come up with a plan, and it can call the researcher or copywriter multiple times, as many as needed, for example calling the researcher on each research subtopic. It also reminds it to keep research tasks focused and atomic.

## The handoff tool

*[Watch from 40:04](https://www.youtube.com/watch?v=rwqGQEzXF-o&t=2404s)*

The supervisor's state has one extra key compared to the others: the task description, for keeping track of the current task, since the supervisor comes up with the tasks. The handoff tool is the most important part of the supervisor. It takes two arguments: the agent name, a literal list so the model can only choose between the two options, and the task description.

Unlike the other agents, the Command it returns specifies both update and goto, the name of the next node. The update sets the task description and appends a tool message saying the task was successfully handed off. That closing tool message is needed because of how LLM provider APIs work: any time you call a tool, you have to follow it with a tool message indicating it was called. The goto is the prefix call underscore plus the agent name, and it has to exactly match the node names created below, call underscore researcher and call underscore copywriter. So instead of going back to the supervisor after a tool call, the tool acts as a routing mechanism to the node where we call the subgraph.

## Calling the subgraphs

*[Watch from 41:57](https://www.youtube.com/watch?v=rwqGQEzXF-o&t=2517s)*

The call researcher node is where we invoke the subgraph. There is a specific node for each sub agent, and we import the compiled graph from the other files. We take the task description from the state and pass it as a single human message, so the research agent sees only that message, not the supervisor's full conversation history. That is how we maintain separate context, and everything inside the researcher graph is independent.

Once we get the final response, we take just its last message, create a new AI message with the name researcher, and add it to the supervisor's conversation history. So the supervisor does not get all the tool calls and back and forth, only the final output with the report details. Invoking a graph returns the full state, so because the research reports were saved to the state, we can read them and update the research reports key in the supervisor.

The copywriter works the same way with one difference: for the input, we also pass in whatever research reports the supervisor's state has. If the supervisor calls the copywriter before the researcher has generated reports, that key would be empty. You have to account for that unpredictability of agents, for example by putting in the copywriter's prompt that if there are no research reports, it should ask the supervisor to run the research first. Then we take the very last message from the copywriter and append it to the supervisor's conversation history.

## Reasoning effort, planning and parallel tool calls

*[Watch from 45:06](https://www.youtube.com/watch?v=rwqGQEzXF-o&t=2706s)*

Being the supervisor is a slightly more challenging problem than being the researcher or copywriter, so I bump the reasoning effort from minimal to low. That adds some latency, but it is better at planning. This is a simple example. In a more robust version you would not rely on the LLM creating a plan and referring to it in the conversation history. You would give it planning tools to create a plan, write it to a file or save it to the state, revisit it and check off tasks as it goes.

When binding tools, I set the parallel tool call flag to false, because this supervisor workflow is more sequential. Otherwise it could hand off four research tasks and a copywriter task at once, but the copywriter needs the research completed before it can start. You could design a more robust system that parallelizes.

## Building the graph

*[Watch from 47:52](https://www.youtube.com/watch?v=rwqGQEzXF-o&t=2872s)*

The supervisor graph differs from the sub agents. We add the two subgraphs as nodes, plus the normal tool node and the router that catches tool calls. We do not need the edge from tools back to the supervisor, because the handoff tool, its only tool, routes with the Command primitive to the researcher or copywriter. So I comment that edge out and add edges so that after either node we route back to the supervisor, to review the sub agents' outputs and decide what to do next.

If you visualize this graph, it will not render quite right: the two subgraphs show up separated from the supervisor graph. That is a quirk of using subgraphs in LangGraph. To fix it, uncomment the destinations tuple at the supervisor node, which only affects visualization, not functionality, and comment out the conditional edge, otherwise it gets confused. Re-rendering then shows the supervisor, the tool node with the command routing to the call copywriter or call researcher subgraph, and the return to the supervisor. It was hard for me to wrap my head around the first time, so if you have questions, hit me up in the comments. I respond to all of them.

## Running it

*[Watch from 49:23](https://www.youtube.com/watch?v=rwqGQEzXF-o&t=2963s)*

The last file is main.py, which is how you run the agent, the same file I ran for the demo. Make sure you are in the supervisor directory and run it with UV run, Python main.py. At the user prompt, type exit to exit. You can also run it in interactive mode by highlighting all the code in main.py and hitting shift enter, which gives you a printout in your interactive terminal. I did not cover how I built the little CLI tool for interacting with the graph since it is not the focus, but feel free to look at it. To learn more about building AI agents, check out the AI Launchpad repo and the other videos on my channel.

*[Watch the full video on YouTube](https://www.youtube.com/watch?v=rwqGQEzXF-o)*
