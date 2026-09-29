# teach-by-asking

A skill that teaches you a topic by asking you questions first and correcting your answers. You do the remembering and explaining. The agent corrects, gives hints and decides what to ask next.

Most people learn from an AI the way they learn from a book: they read an explanation, nod and move on. It feels like learning, but little of it stays. This skill turns the conversation around, because decades of research on memory show that **answering beats reading**.

## Example

> **You:** teach me the JavaScript event loop
>
> **Agent:** I'll ask, you answer from memory, I'll correct. Say "just tell me" any time, or "stop" to finish. What do you already know about the event loop? A rough idea or a guess is fine.
>
> **You:** It runs callbacks when the main code is done? Something with a queue.
>
> **Agent:** That's the core of it: callbacks wait in a queue and run once the call stack is empty. What does this print? And how sure are you, 1–5?
>
> ```js
> setTimeout(() => console.log("timeout"), 0);
> Promise.resolve().then(() => console.log("promise"));
> console.log("sync");
> ```
>
> **You:** sync, timeout, promise. 4.
>
> **Agent:** "sync" first is right: synchronous code always finishes before any callback. But "promise" comes before "timeout", and since you were fairly sure, this one is worth remembering. There are two queues. Promise callbacks go to the microtask queue, which is emptied completely before the next task, such as a timer callback, runs. So why can a `setTimeout(..., 0)` be delayed by a long chain of `.then()` calls?

## Why it works

Each rule in the skill is based on a well-replicated finding.

**1. Recalling beats rereading (the testing effect).** Students who read a text and then tried to recall it remembered far more a week later than students who read it again. Rereading only won on a test five minutes later ([Roediger & Karpicke, 2006](#references)). Recall practice also beat building concept maps ([Karpicke & Blunt, 2011](#references)). A large review of study techniques rated practice testing as one of only two "high utility" methods. Rereading and highlighting were rated low ([Dunlosky et al., 2013](#references)). *In the skill:* every turn is a question you answer from memory.

**2. Guessing before you know helps, even when the guess is wrong (the pretesting effect).** People who tried to answer before being taught learned the answers better than people who spent the same time just studying, even though their guesses were mostly wrong ([Richland, Kornell & Kao, 2009](#references); [Kornell, Hays & Bjork, 2009](#references)). *In the skill:* the session opens with "what do you already know?", and a complete beginner is asked to guess.

**3. Producing an answer beats being handed it (the generation effect).** Information you generate yourself is remembered better than the same information you read ([Slamecka & Graf, 1978](#references)). *In the skill:* when you're stuck, you get a hint, then a stronger hint, and the answer only after that. You still do the last step of the thinking.

**4. Just enough support (scaffolding and the zone of proximal development).** Learners progress fastest on tasks slightly beyond what they can do alone, with a tutor giving just enough help and withdrawing it as they improve ([Vygotsky, 1978](#references); [Wood, Bruner & Ross, 1976](#references)). *In the skill:* questions get harder after right answers and step back after wrong ones.

**5. Feedback that explains (corrective feedback).** Answering without feedback lets errors stick. Feedback after a mistake is what fixes it ([Pashler et al., 2005](#references)). Feedback helps most when it's about the task and how to improve, not praise ([Hattie & Timperley, 2007](#references)). *In the skill:* every correction says what was right, what was wrong and why, with no "Great question!".

**6. Confident mistakes get fixed best (the hypercorrection effect).** Errors made with high confidence are more likely to be corrected than low-confidence ones, once you see the right answer. The surprise makes the correction memorable ([Butterfield & Metcalfe, 2001](#references)). *In the skill:* now and then the agent asks how sure you are and points out when a confident answer was wrong.

**7. Explaining it yourself (self-explanation).** Learners who explain material to themselves understand it more deeply than those who only read it ([Chi et al., 1994](#references)). *In the skill:* the questions ask "why" and "what happens if", and the session ends with you summing up from memory.

**8. Coming back later (spacing).** The same practice spread out over time is remembered much longer than practice crammed into one sitting ([Cepeda et al., 2006](#references)). *In the skill:* missed points come back later in the session in a new form, and the final recap gives you questions to answer again in a few days.

**9. A map before the details (advance organizers).** Learners remember new material better when they first get a short overview of its main parts, which gives each later detail a place to fit ([Ausubel, 1960](#references)). *In the skill:* after your first answer, the agent lists the main parts of the topic in one line each. Every new term is defined the first time it's used, and the final recap includes a glossary.

**Why it feels harder, and why that's fine.** Most students prefer rereading because it feels fluent, even though recall works better ([Karpicke, Butler & Roediger, 2009](#references)). Effort that slows you down in the moment but improves long-term memory is what Bjork calls a *desirable difficulty* ([Bjork, 1994](#references)). If a session feels harder than reading an explanation, it's working.

A note on the brain: dopamine neurons signal the difference between what you expected and what happened, a "prediction error" ([Schultz, Dayan & Montague, 1997](#references)). That's one reason surprise helps learning, but it's background. The evidence behind this skill is the behavioral research above.

## When to use it

- **Checking what you really know.** You've read the docs or watched the talk. Now find the gaps.
- **Deepening a topic you half know.** This is where the skill works best: there's something to recall and plenty to correct.
- **Onboarding to a codebase.** "Teach me how authentication works in this repo." The agent reads the code first and asks about the real files, so you learn the system rather than a generic version of it.
- **Preparing for an interview, exam or design discussion**, when you'll need to explain things out loud without notes.
- **Unlearning a misconception**, when you suspect your mental model is off.

## When not to use it

- **You need the answer now.** Ask a normal question. Being quizzed when you need a quick fix is frustrating. Mid-session, "just tell me" does the same thing.
- **A complex skill that's completely new to you.** For beginners, studying a worked example first is often more efficient than solving problems (the worked-example effect, [Sweller & Cooper, 1985](#references)). Read one good explanation, then use this skill to test yourself.
- **A multi-week course.** This skill is one session with no saved state. For a structured course with lessons and progress tracking, see the `teach` skill in [mattpocock/skills](https://github.com/mattpocock/skills).
- **Topics the model is likely to get wrong**, such as very recent releases or niche internal tools. The agent is told to check sources, but keep the docs at hand.

## Tips

- **Answer before you look anything up.** A wrong guess helps. Skipping doesn't.
- **Turn off prompt suggestions in Claude Code (`/config`)**, or they may suggest the answer.
- **Rate your confidence honestly.** The confident mistakes are the valuable ones.
- **Steer it.** "harder", "easier", "go deeper on X", "just tell me" and "stop" all work.
- **Lost in the jargon?** Say "terms" to get every term covered so far, one line each.
- **Come back in two or three days** and answer the recap questions from memory. That's where most of the long-term benefit comes from.

## Install

This skill is part of [bzhn/skills](../../README.md). Once installed, just ask the agent: "teach me X", "quiz me on X" or "test my understanding of X".

## References

- Ausubel, D. P. (1960). The use of advance organizers in the learning and retention of meaningful verbal material. *Journal of Educational Psychology, 51*(5), 267–272.
- Bjork, R. A. (1994). Memory and metamemory considerations in the training of human beings. In J. Metcalfe & A. Shimamura (Eds.), *Metacognition: Knowing about knowing* (pp. 185–205). MIT Press.
- Butterfield, B., & Metcalfe, J. (2001). Errors committed with high confidence are hypercorrected. *Journal of Experimental Psychology: Learning, Memory, and Cognition, 27*(6), 1491–1494.
- Cepeda, N. J., Pashler, H., Vul, E., Wixted, J. T., & Rohrer, D. (2006). Distributed practice in verbal recall tasks: A review and quantitative synthesis. *Psychological Bulletin, 132*(3), 354–380.
- Chi, M. T. H., De Leeuw, N., Chiu, M.-H., & LaVancher, C. (1994). Eliciting self-explanations improves understanding. *Cognitive Science, 18*(3), 439–477.
- Dunlosky, J., Rawson, K. A., Marsh, E. J., Nathan, M. J., & Willingham, D. T. (2013). Improving students' learning with effective learning techniques. *Psychological Science in the Public Interest, 14*(1), 4–58.
- Hattie, J., & Timperley, H. (2007). The power of feedback. *Review of Educational Research, 77*(1), 81–112.
- Karpicke, J. D., & Blunt, J. R. (2011). Retrieval practice produces more learning than elaborative studying with concept mapping. *Science, 331*(6018), 772–775.
- Karpicke, J. D., Butler, A. C., & Roediger, H. L. (2009). Metacognitive strategies in student learning: Do students practise retrieval when they study on their own? *Memory, 17*(4), 471–479.
- Kornell, N., Hays, M. J., & Bjork, R. A. (2009). Unsuccessful retrieval attempts enhance subsequent learning. *Journal of Experimental Psychology: Learning, Memory, and Cognition, 35*(4), 989–998.
- Pashler, H., Cepeda, N. J., Wixted, J. T., & Rohrer, D. (2005). When does feedback facilitate learning of words? *Journal of Experimental Psychology: Learning, Memory, and Cognition, 31*(1), 3–8.
- Richland, L. E., Kornell, N., & Kao, L. S. (2009). The pretesting effect: Do unsuccessful retrieval attempts enhance learning? *Journal of Experimental Psychology: Applied, 15*(3), 243–257.
- Roediger, H. L., & Karpicke, J. D. (2006). Test-enhanced learning: Taking memory tests improves long-term retention. *Psychological Science, 17*(3), 249–255.
- Schultz, W., Dayan, P., & Montague, P. R. (1997). A neural substrate of prediction and reward. *Science, 275*(5306), 1593–1599.
- Slamecka, N. J., & Graf, P. (1978). The generation effect: Delineation of a phenomenon. *Journal of Experimental Psychology: Human Learning and Memory, 4*(6), 592–604.
- Sweller, J., & Cooper, G. A. (1985). The use of worked examples as a substitute for problem solving in learning algebra. *Cognition and Instruction, 2*(1), 59–89.
- Vygotsky, L. S. (1978). *Mind in society: The development of higher psychological processes*. Harvard University Press.
- Wood, D., Bruner, J. S., & Ross, G. (1976). The role of tutoring in problem solving. *Journal of Child Psychology and Psychiatry, 17*(2), 89–100.
