---
schemaId: urn:gala:schema:content-frontmatter:2.0.0
schemaVersion: 2.0.0
id: 01a168c7-551c-7b64-a0d7-ef1168a3b456
kind: article
title: How I Turned Claude Code Into My Personal Assistant... You can too!
description: 'I turned Claude Code into a personal assistant for everyday non-coding work. Here is the system design: output style, a file-based context system, skills and hooks.'
language: en-US
authors:
- 01a168e1-ab3d-763f-bac9-92638c7ec5cd
tags:
- claude-code
- personal-assistant
- context-engineering
- skills
- hooks
status: published
createdAt: '2025-10-31T12:00:00.000Z'
publishedAt: '2025-10-31T12:00:00.000Z'
slug: how-i-turned-claude-code-into-my-personal-assistant
hero:
  path: assets/content-how-i-turned-claude-code-into-my-personal-assistant-hero.jpg
  alt: Thumbnail of the video "How I Turned Claude Code Into My Personal Assistant... You can too!"
  role: informative
redirects: []
extensions: {}
---
*Adapted from the video [How I Turned Claude Code Into My Personal Assistant... You can too!](https://www.youtube.com/watch?v=aYAVSG4Ra40)*

> **TL;DR:** Coding agents like Claude Code can take on long, non-coding tasks, so I turned one into a personal assistant called L. It runs on an output style that replaces the software engineer identity, a file-based context system for memory, projects and tools, skills and lightweight CLI tools for actions, and hooks that load context and remind it to update memory every turn. In one session it did hours of YouTube research in about 12 minutes.

I started by asking my assistant, L, to help me research my next YouTube video: a list of 5 to 20 lessons I learned while turning Claude Code into the personal AI assistant I use every day, and how to get the most value out of it.

Without exaggerating, L is about to do in a few minutes what has taken me several hours. All of this is happening in real time, and we will let her do her thing while we talk about how she works.

## Coding agents beyond coding

*[Watch from 0:23](https://www.youtube.com/watch?v=aYAVSG4Ra40&t=23s)*

Coding agents like Claude Code and Cursor have become really good at complex, long horizon tasks. They are at the bleeding edge of what is possible and of where value is being generated in AI, and they are starting to feel more like thought partners, collaborators or even a technical co-founder than simple chatbots.

The idea is to adopt that leverage for non-coding tasks: things you do every day that take up a lot of time but could benefit from an agent going off for 5, 10 or 15 minutes at a time and coming back with a really great output.

I want to set expectations, because I hate overhyping things. I am not selling you the dream of a real-life Jarvis that runs your whole life. I want to show you what is actually working and generating real value for me, and how you can do the same. It takes a lot of work and optimizing to build a system that works the way you want.

I will not go through everything step by step or share the code, because there is a lot of personal data in it. I will focus on the core system design. What you are seeing is a custom agent I built with the Claude agent SDK. It works exactly like Claude Code because the agent SDK is built on top of Claude Code and just orchestrates it under the hood. I can run L with this custom agent or with Claude Code alone, so you can run everything you see by following how I set up Claude Code.

A quick note on credit. Everything you get from me, the ideas, the notes, the learnings, the code, is my own. But we all learn from and are influenced by others, and I take a lot of inspiration from thought leaders like Daniel Miessler and Indie Dev Dan. Daniel Miessler had an amazing blog post, which he turned into a YouTube video, on creating an AI assistant. Go check it out and give him some support.

## The output style

*[Watch from 4:08](https://www.youtube.com/watch?v=aYAVSG4Ra40&t=248s)*

It all starts with the .claude folder, and first the output style. An output style is a way to modify Claude's system prompt, letting you define a different personality and identity for the agent and add any special instructions. It replaces Claude Code's identity as a software engineer so you can generalize Claude Code to other purposes. Here I use it to turn Claude Code into a personal assistant named L, and I also give her awareness of the context system she uses.

## The context system

*[Watch from 5:10](https://www.youtube.com/watch?v=aYAVSG4Ra40&t=310s)*

The context system starts with a CLAUDE.md file that explains its subsystems: a memory system, a project system and a tools system. Each has its own CLAUDE.md that explains how that subsystem works. This is really the brains of the whole operation, and it relies heavily on context engineering, which boils down to giving the right context at the right time.

In my last video, on skills, we covered progressive disclosure, and the same idea is how this entire context system is designed. Whether it is a specific memory about me, my user preferences, or details of a finance, research or YouTube project, the context in these folders is only read by Claude as needed. Claude decides on the fly whether it needs more context for its next step, which makes this file-based system efficient. If you are not familiar with skills, go check out that video for a beginner to expert rundown.

## The research result and skills that use skills

*[Watch from 6:56](https://www.youtube.com/watch?v=aYAVSG4Ra40&t=416s)*

L has finished, after something like 10 minutes. The video concept is hard-won lessons from building and using Claude Code as a daily personal AI assistant. The research doc lists my last two related videos, the top competing videos, and a breakdown of each, including which channel it came from. Further down are the patterns it identified from competitor research, the content gaps, and then the options it generated.

It proposed several titles, including:

- I used Claude Code as my assistant for 3 months. Big mistakes were made.
- The Claude Code personal assistant lessons nobody tells you.
- I built my AI assistant wrong. Here is what actually works.
- 3 months of Claude Code assistant mistakes so you don't have to.
- The personal assistant setup that took me weeks to get right.

Each title comes with thumbnail concepts and an overall recommendation, and then a hook including how the scene should play out. The suggestion was a screen showing the Claude Code assistant interface, quick cuts of three to four visual mistakes, and a voiceover: this cost me 10 hours, this broke my entire workflow, and this almost made me give up completely. That is pretty good.

This is all generated using one of the skills I have added to L, for researching new videos. It describes everything I want in the research and leverages other tools to generate thumbnails, optimized titles and video hooks. These are skills leveraging other skills, compound skills if you will. The quality comes from progressively disclosing detailed design requirement documents. In the YouTube title skill, for example, there is mandatory reading of design requirements, and the skill itself gives a high-level summary of the steps. The design requirements are where I guide the agent to a title optimized for SEO, click-through rate and curiosity. The same goes for thumbnails and hooks.

Normally I would go through the research doc more carefully and choose, but here I told L to go forward with the suggested title, generate all the thumbnail options she came up with for it, and save everything and update the research doc.

## Autonomy and the project system

*[Watch from 11:27](https://www.youtube.com/watch?v=aYAVSG4Ra40&t=687s)*

She goes off and works for about 10 minutes at a time, then follows up and asks if she needs additional input. It is just like how Claude Code works on coding projects, except you can see how long horizon this is for a non-coding task. The goal is to automate as much as possible and make the assistant as autonomous as possible.

Back to the context system. In each subsystem there is another CLAUDE.md. The projects one explains each project I have, and the YouTube project goes into much more detail. It points to the episode research directory where we generate research documents, and to my YouTube project directory, which lives in a totally different folder on my file system, and it explains what all the files in those episode directories are. It is getting deeper and deeper context as needed.

## The memory system

*[Watch from 12:31](https://www.youtube.com/watch?v=aYAVSG4Ra40&t=751s)*

The memory system is really important. Its CLAUDE.md explains how memory works and breaks down the core memory files. I have instructed L to create memories in markdown files organized as learnings, user preferences, user projects and work status. Work status corresponds to the work she is doing now, a status update on what she has accomplished so far.

L is completely responsible for using her memories, writing to them, and keeping the whole context folder up to date. Any time she learns something new about me or my projects, she updates the necessary files. This works closely with the hook system, which I use to steer her and remind her when to update her memories.

## Reviewing the thumbnails

*[Watch from 13:55](https://www.youtube.com/watch?v=aYAVSG4Ra40&t=835s)*

L finished again. She updated the research doc, saved the thumbnails to a folder, and updated her work status. Concept one shows time evolution with mistakes, getting progressively worse from month one to month two to month three, ending in a complete biohazard meltdown. That makes more sense once you know the title, "I used Claude Code as my assistant for 3 months. Big mistakes were made."

Concept one B is a before and after disaster, which is actually really good except that the person on the left is not me. The system will not be perfect, but this is getting me about 95% there. I could pop the thumbnail into Nano Banana, which is what L uses to generate these thumbnails in the first place, face swap myself in, and it is good to go. You can even see VS Code loaded on the desktop in the background, which is awesome. Concept one C, a single mistake moment, was the recommended one, and it is not bad either. They are all not bad. I chose concept one B, the before and after disaster, and had L update the doc with the final decision.

## Tools and token-efficient documentation

*[Watch from 16:44](https://www.youtube.com/watch?v=aYAVSG4Ra40&t=1004s)*

Everything is context L uses to give me the most personalized and relevant output, and she is also responsible for managing it. The projects matter especially. As L learns who my YouTube competitors are, what my definition of a competitor is, or what subscriber range they should be in, she keeps updating those files.

The tools section holds extensive documentation. For example, I developed a local MCP for translating my videos' subtitle files, descriptions and titles into different languages, and this has complete documentation on the server and all its tools. That is a little less necessary for MCPs, because when you connect an MCP server to Claude, all the tool schemas are automatically injected into the system prompt, as long as you had good documentation and docstrings. I keep the documentation because I am in the process of replacing these MCP servers with CLI tools used through skills. Claude can then read the documentation only when it needs it, instead of injecting all that context every time, even for tools we may not use.

You can write very dense, comprehensive documentation and still be token efficient, because Claude can use glob to find the specific documentation it needs and grep to search within a document for just the tool it needs. It does not have to read the whole document. That is the context system, and it is one of the biggest pieces that makes this work.

## Actions: skills and lightweight CLI tools

*[Watch from 19:18](https://www.youtube.com/watch?v=aYAVSG4Ra40&t=1158s)*

We covered the system prompt and managing context. The final piece is the action system: what the agent can do, through tools built into Claude, MCP servers and, more importantly now, skills. A skill is packaged instructions in a SKILL.md file on what to do, with additional context you can include in the skills folder, such as a CLI reference.

Since Claude can run bash commands, a skill can run Python scripts, bash scripts or CLI tools. This is huge for me. I used to have several local MCPs for specific tool sets, like working with my YouTube analytics or interacting with Obsidian for my notes. Now I vibe code really lightweight CLI tools and install them with UV tools so they can be accessed from any terminal. Once I vibe code one, I have Claude write a usage reference for it. The Obsidian one, for example, lets her work with my brain dumps and the notes I take when learning something new, including the comprehensive doc I made for the last episode on Claude agent skills.

With the CLI tool, she can interact with Obsidian just like with an MCP server, but with progressive disclosure: I do not connect an MCP server and take up space in the system prompt context window. In the skill, I just point to the CLI reference, give short examples of how to use the CLI, and tell her to go to the full documentation if she needs more information or hits errors.

## Hooks

*[Watch from 21:45](https://www.youtube.com/watch?v=aYAVSG4Ra40&t=1305s)*

Hooks are the infrastructure that facilitates context engineering, tracing, debugging and steering the agent at the right time. They are a powerful control mechanism for the whole system. The trace hooks capture all the actions as L works, recorded to Langfuse, and I will show the full trace in a moment. The two most important hooks are these.

- **Load context.** Every time the user submits a prompt, it dumps the CLAUDE.md in the context directory into Claude's context. I do not want to rely on Claude deciding whether to read this file, because the context system is part of what makes the assistant run.
- **Memory reminder.** It also triggers on every prompt and injects a system reminder telling L that every conversation turn she should reflect on what has been done, update the work status document and create any new memories. That is what lets her continuously update the context system.

I got this idea from working with Claude Code a lot: you sometimes notice in tool outputs system reminders injected by the harness, reminding Claude after a certain tool to take a certain action. It works amazingly to steer the agent and get more reliable, repeatable behavior.

## The Langfuse trace

*[Watch from 24:12](https://www.youtube.com/watch?v=aYAVSG4Ra40&t=1452s)*

Using the hook system, I record everything L does in each session with Langfuse, an open source library. The first turn, my kickoff message, ran for 8 minutes. L searched for files, looked into her memory for prior research or YouTube information, read a ton of files in my YouTube project, used the YouTube analytics MCP server to get information on my channel, and constantly updated her to-dos. She then made many more calls to the analytics server, probably for the competitive research, before ending 8 minutes later with the output.

The second turn, where I said to go ahead and generate all the thumbnail options, ran for another 3 minutes, running bash commands to call the Gemini CLI and generate the thumbnails with Nano Banana. The last turn, choosing the final concept and updating the document, took about a minute.

## What it saves me

*[Watch from 26:38](https://www.youtube.com/watch?v=aYAVSG4Ra40&t=1598s)*

All in all, it took L maybe 12 or so minutes to do the competitive research, find similar videos, extract the patterns for titles, thumbnails and video ideas that were working, identify content gaps, produce the most likely title and thumbnail concepts using best practices, and generate the thumbnails with Nano Banana.

L can help with other things like financial analysis, and has other tools and skills, but I wanted to showcase this complex task. It used to be an entire workflow that would take me hours, from doing the research myself to brainstorming and finding the optimal combination of title, thumbnail and hook. I spent a lot of time getting this system to work as you see it, with outputs pretty much exactly how I want, and it took a decent amount of optimizing. But it is worth it, because it now saves me hours every single week. In my next video I will share the highest lever hard-won lessons I have learned making this system.

*[Watch the full video on YouTube](https://www.youtube.com/watch?v=aYAVSG4Ra40)*
