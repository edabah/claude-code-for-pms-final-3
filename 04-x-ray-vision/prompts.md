# 04 · X-Ray Vision — prompts

**Context:** You joined Rook two weeks ago as PM on Dispatch.
Release 4.2 shipped on 12 August, just before you arrived, and
landed badly. Last session the numbers showed you what happened: a
ping used to wait ninety seconds and now it waits sixty, people
missed pings they used to catch, and missing one counts the same as
turning one down — so four responders stopped hearing from us
altogether.

At the end of the session, type wrap up, and Claude Code saves the
prompts you wrote yourself below — not the starter prompt — updates
CLAUDE.md, and saves your work to GitHub. By Module 6 this file is a
prompt library built from your own questions.

---

### 1.
I dont understand anything about "supply". Can you explain that to a beginner? What are we talking about with supply, what is the dependency and correlation with availability?

### 2.
interesting..
back to understanding all the code, can you draw out a flow chart or some sort of diagram explaining all the different functions or capabilities the code does, how it works, where it lives, etc

### 3.
can you simplify the chart better with whats actually happening and how the logic works

### 4.
can you generate a UML diagram of the workflow with more details of how this works

### 5.
that is a sequence diagram. i want more of a flowchart (if someone responds, if they dont, how it works, etc)

### 6.
did anything change since 4.2 that could have contributed to the problem

### 7.
Are there any engineering comments in the codebase worth evaluating further

### 8.
how did scoring change?

### 9.
so acceptance actually went down and travel time went up...
does the data back up anything about proximity?

### 10.
an engineering manager asked:
Quick one, did the change to who gets asked first apply to responders who'd already been turning jobs down, or just new ones? Can't tell from the code and don't want to guess on this. Anyone know?

Where should I look in the code on this, and what can the historic data confirm about htis as well to understand how it is impacted?

### 11.
well, i'm not talking about "new responders"... 
I mean the history of who's been turning down jobs?
how does that historical data impact us

### 12.
why might marcus be asking the question, is there a deeper point hes trying to get to

### 13.
could it be that he was trying to debug the issues too?

### 14.
Find me the part of this code that takes points off somebody when they miss a ping or turn one down. Show it to me and explain it in plain English. Then find me every single thing in this code that puts points back on.

### 15.
what is the code not doing here
