# Notes And Podcast Transcript

## Introduction

For anybody who happens to be following my frenetic pace of creating GitHub repositories lately related to Claude Code and wondering have I gone insane or am taking cocaine, the answer is certainly not for the second, and hopefully not for the first!

Lately, I've been really, really fascinated by Claude Code. I think it's just bringing together—I've realized belatedly—why agentic AI has been so focused on **CLIs** and the **file system**. It's kind of like it was a **penny-drop moment** for me that you can consolidate and get really, really impressive results just using this pattern. I'm not sure it's going to be the way indefinitely, but for now, it's a great way of actually getting really reliable results with agentic AI.

All the little models and patterns that I've been creating—and I like to create everything **individualized**—I do this alongside work, I should point out. But these frequently turn into such **fantastic work tools** that I like to spend time on them sometimes when I'm on a roll, which is how it goes at the moment.

So, with that introduction, I wanted to sketch out—for the purpose of an AI agent transcribed, to just **speed up the process of creating the plumbing for this**, and just by means of a sort of, I guess you could call this a podcast integrated to a code repository—to just kind of explain my thinking in the design of this deep research implementation.

There has been quite a number actually of deep research implementations out there, and I really want to go through the ones specifically patterned at the folder level, because I think that when **knowledge flows** across people, when people give each other and exchange ideas in open source—I don't know what's wrong with my vocabulary today—really great things happen. There are also more visual frameworks, but I'm going to focus on the folder-level patterns. And of course there are deep research models, but my experience with the deep research models has been quite **underwhelming**. My experience has also been that the **simplicity of patterns** like this can be quite overwhelming, or very impressive at least.

So, that's just by way of introduction and note. The idea that came to me, and why I said I have to just jot this down while it's fresh in my mind—I did attempt to find a piece of paper in my house before determining that I do not own any A4 paper and resorted back to modeling out the file tree on my computer, which is what I'm looking at as I record this.

---

## The Deep Research Repository Pattern

The pattern that I've been using for quite some time actually in repositories for projects like this is based on a folder structure that I'd kind of, I would say, has remained fairly **consistent over time**, which is always, I think, a good thing because you sort of, I think, figured out a basic pattern that works. And that's basically breaking down AI workloads and agentic workloads into its **constituent elements or ingredients** as I come to think of them.

### Input-Output Direction

If we look at it from an **input-output direction**—and I was going to actually try, it's almost like sketching out a diagram for a computer when you're thinking: "How could this be laid out?" So my first thought was maybe the very base of this is **input-output**. It's a pure **IO system**.

And when I thought about that and looked at my napkin/screen, I got to thinking, "Well, this is a bit messy really," or it's a bit tricky to get the pieces of this jigsaw into that pattern because of the fact that if we take **context** as an example:

An AI input—a prompt—may generate an output. And in this model, what I'm trying to design or achieve is that the output gets **contracted**—excuse me, **context is extracted**—and that then becomes part of the **input context**. So that's kind of a **circular pattern**. It kind of makes the idea of defining it in pure in/out terms—which does in some AI use cases make abundant sense if it's a text transformation prompt workflow, where you're saying, "The inputs are an outline for a blog post, the outputs are a finished blog post, run this batch." And that works very, very well as a method for batch prompting.

But this is a little bit different for research, and again, thinking how to actually reflect this as best as possible for implementation.

---

## Anatomy of the Repository

I think I have about five minutes before I need to be somewhere, so I'm going to have to just speed up these notes a little bit.

### 1. The System Prompt (e.g., `agent.md`)

**Prompt as the baseline:** A **system prompt** is very, very important for providing **determinative guidance** to the AI model.

In the context of Claude Code and the emerging standards with agentic CLIs like Gemini and Qwen, we're seeing this implemented as simple Markdown files. Claude has `claude.md`, Gemini has `gemini.md`, Codex has... I'm not sure. Oh, `agent.md`.

Which is actually smart because in, I think, the emerging environment where people are using a bunch of these, it's not really practical to have, you know, you're not going to want to have a `qwen.md` and so instead of these frameworks putting their branding on the context file, I like what OpenAI has done and I think it's developer-friendly and it's the right way to go, but that's not how... Anyway.

So that file, whatever it's actually called in the CLI implementation, is to my mind kind of a system prompt, basically. It seems to just get loaded into every new conversation. I think it gets cached, but it's sucked up very efficiently, which I've seen with Claude.

### 2. Context Directory: From Human to History

**Context Directory:** The reason I've created quite a number of these models and templates is because I think that it personally helps me see previous versions as their own repositories, because I want to see V1, and this is evolving over time.

The evolution in this iteration is that I've come up with a **split of the context folder** that tries to account for the fact that there's going to be discrete pools.

And this is really almost **lightweight RAG** (Retrieval-Augmented Generation). Now, it's not actually RAG, but one idea that I've had in this experiment is thinking: "Could this be **RAG-ified** by actually creating a real vector store inside of the repository and then vectorizing previous outputs?" That would be very interesting to explore.

**The "from-human" folder** is intended as a **human-created store** where you can jot down files.

**The "from-history" folder** is where the **compacted**—periodically compacted or on-demand compacted—context from previous outputs is going to go.

### 3. The Compaction Loop (The Big Idea)

The idea is to see if the idea of using a slash command to **compact the output history** can actually provide that **loop** instead of using memory.

This is something I might actually email Claude about. I saw on Hugging Face a—I think someone described it as an inflammatory article, which is quite funny whenever you see computer science papers described as inflammatory. But it was described as inflammatory, and the inflammatory statement was the allegation that **RAG is dead**.

Claude Code is blowing up, and people are really getting into it—I'm sort of a case in point here.

And does it mean, when we see that Claude can take a few Markdown files—which is basically all these systems that Claude is bringing out, like skills and plugins, are just really Markdown under the hood—does this mean we don't need to bother about very complicated things like embeddings and vector storage and vector databases?

Because certainly for an enterprise that might need to embed, you know, an enterprise store of... I'm just thinking for some reason an airline embedding a flight schedule, where you're talking about **petabyte-level data**, of course, that's not going to be achieved with Markdown files of the flight schedule.

But for your average deep research prompt—which might be actually, it's a misnomer, it's someone... I've used a pattern of trying to use it for sort of **problem-solving and ideation**—I've suggested that the applications actually can be things like **job hunts, career development, career tracking, tracking a health issue**—in other words, using the idea of a code repository as a concept and as a structure, and not actually using it for code.

That's really the whole idea here of **managing context** in this way: that's the idea of taking previous outputs, compacting them as Claude does as a command, and then using that as, "This is the stuff we've covered, I still have this question."

---

## Use Case: Aggregating Private Tools

I think a typical use case—which is something I'm wondering a lot about at the moment: you've got all these—I love AI Studio's new builder tool, it's brilliant. Streamlit and Gradio, all these things are great, but I find myself now more easily than ever being able to create a few **really, really useful tools for personal life**.

I created a baby tracker that's better than any app I found on Android. I'm still trying to nail down my movie recommendation bot, ironically—that's kind of hard to do for some reason. At work, I have a really nice one for creating a timesheet, for creating meeting minutes. And each one of these is at the point where it's legitimate, it works. And it's a whole new paradigm of I don't need to go out and find these off-the-shelf softwares that don't really do what I want.

But the question that I'm sort of wondering is, "Okay, well, I don't want to have like, I'm not going to have like 50 bookmarks, and I'm not going to remember 50 different URLs, and I'm not going to use 20 or 10 different Hugging Face spaces as like my work area. **I need a workspace, I need a glue**."

And what's out there? What can be done? Does anyone actually make something like this? Or if I want to approach it a bit more savvy, can you suggest some ways that I could take? Let's say everything's using Gemini, and I can share an environment variable and I'm using Vercel, if—is there—is someone creating the **glue**, as I call it, for these private tools? **Aggregations of private tools? AI Workspaces?**

---

## Pipeline: From Prompt to TTS

When I did the first prompt, the first output, I said, "Okay, great that we have this recorded down the repository, we have **output storage**," which is another thing that I've been very passionate about for a very long time in AI, is that the failure to consider that as important has been a very overlooked part of the picture.

And something as simple as a repo where your outputs are being collected in a folder as a Markdown file, while primitive, is actually approaching a solution to that problem. At least you get the data, it's not just in your ChatGPT history, which is—no one really has seen the need in mainstream or conventional AI to develop even MCP tooling for allowing users to say, "Okay, I want to save this output to Google Drive or Confluence." For some reason, that's absent.

So that's another core element, and that's why I say that this approach is the reason I've sort of really jazzed up about it is that it's all these little modules I've worked on individually, with, you know, like MongoDB stores and Qdrant and all these different systems that at scale are great, but it's kind of miniaturizing it, and then it's actually a much, much more enjoyable way of prototyping.

So, that's prompts, and then you have prompts queue and prompts run.

The outputs are aggregated. And what I've done here, I've done two little tweaks or two little touches. We covered prompts, outputs, context, system prompt, the main ingredients, and there are two little sort of touches and the kind of circular compacting, which that's really the whole hope of this experiment: is can we create a **single-turn analysis workflow** that **iterates and gets progressively better**, referencing a previous conversation history, all without really using much in the way of tooling?

You know, and there is tooling for all of this. There's memory layers, there's—I like a tool called Raggy, RAG as an API—but it's kind of saying, **can we actually do all this stuff just with Markdown files** arranged in a certain way, and with a little bit of clever usage of slash commanding and scripts?

### The Pipeline Steps

When I do this, and I've done this actually a bunch of times—I recently used this pattern again with a client to create a sort of a synthetic creation of context data from a glob of speeches he had delivered, blogs and articles he'd written, and just said, "Okay, this is the raw material. I'm going to ask you questions, you create the outputs here, and this is then going to—I'm going to ingest that into a RAG store."

Two little sort of accessory activities I find helpful in that respect:

#### 1. Text-to-Speech (TTS)

Being able to listen back if you just want to do this for like your own education. And this is going to involve—I'm trying to keep it light on the multi-agent part of this project here—but that is a good one for a very defined workflow for an agent. I'm not sure it could be slash commanded as effectively, saying, "Okay, this is a gigantic, this is a big Markdown file. It was created from several runs of a research agent. Please, it's been concatenated, please convert this into something ready for text-to-speech."

And something with long context like Gemini or Claude does this really well. Even if it needs to chunk its way through the source, it says like, you know, and that's just basically if it's not readable, just take it out, divide it into sections, stuff like that.

#### 2. Prosody Inference

And the second one is more, a little bit more structured, saying **inferring a prosody** for tools like Eleven Labs where you can actually use speech markup language to create markings for emphasis and pause duration and stuff like that, and saying go from that.

And actually this works very, very well, like most things in maybe AI and automation, as a **sequential process**. In other words, starting with the—and that's why the outputs are structured thusly:

1. Let's start with the outputs, let's order them sequentially
2. Next stage: add a covering page (just something I often do as well)
3. Next stage: wrap it up with a sources document (a frequent problem in the way these research models deliver their outputs and the way they put sources)
4. Final stage here: concatenation, make this just one big document, combine it

Then it splits into two paths:

**For a reading document:**
- Next stage would be PDF conversion

**For listening:**
- Next stage would be the TTS-friendly formatting with or without prosody
- Then the actual text-to-speech through a TTS agent

And multimodal models like Gemini—which is also why I'm so excited about their stuff and their capabilities—actually greatly reduce the amount of intermediate steps because you can say, "This is the research dump, for want of a better word, format it," and you can kind of just give it intermediate steps to use, like, "Format this, add some prosody as you, you know, according to the text, and then synthesize that and give me a binary."

### Speech-to-Text (STT) Pipeline

So that's basically—even if—and sorry, ah, one final thing, because I've mentioned this in different... I don't think I've really done many videos or podcasts lately, actually, but certainly projects where I've played around a lot with context extraction and that pipeline—like the idea of a user providing context in a very long voice memo that then gets through speech-to-text, and then context gets sucked out through an agentic workflow that says, "You are just extracting the pure contextual data from this."

That pipeline is why there is a **pipeline folder**, so that most times I can—and I just created subfolders called:
- **audio-drop-off**
- **in-queue**
- **in-process**
- **done**

And that's just a pattern I've used for organizing workflow, so that when you are writing the bash scripts and the Python scripts to make all this work, there's a clear delineation and the agent knows where the queue is and where to move stuff to when it's done.

So that said, that idea for a pipeline is an **STT pipeline** (Speech-to-Text pipeline). The idea being I'll drop off a detailed research prompt here. And then, this is another idea that I have. There are many actual directions that this little repository structure could be taken in through different processes and different system prompts.

And one of them I think that would be super interesting, and that I've also modeled a little bit, is if you use speech-to-text to transcribe a very, very lengthy prompt where you're really going exhaustively into—let's just take the health problem or career example—you're asking certain things. Certain things you're saying are actually context data.

If we can actually **branch that out**, saying—and that's an agent in between after the transcript, saying, "Okay, we got a whole bunch of information here from the user that's going to inform how we're going to approach this research repository and research project. Some of it is questions and prompts—chunk those up and make a list (as always, a list gets best results). Other stuff you probably don't need in there, and other stuff is context data—put that in the context folder, and then vectorize it."

---

## Conclusion

So that's really, I think, all the little constituent elements. I think the only way for me to actually sort of explain what I was trying to go for here, what I am trying to go for here was to record a big long **voice dump** like this, which is actually an example of exactly the type of pipeline that I'm talking about where I can actually now STT this voice recording, feed that to Claude, and then say, "This is a whole bunch of information about what I'm trying to do here. Can you speed up the proof of concept because I don't have all day? Write a—you get what I'm trying to do—write a couple of bash scripts for the routing logic, and let's actually see how well this works."
