# 🏇 Horse Race

**One prompt. Multiple agents. Best solution wins.**

Why settle for one AI-generated solution when you can have several compete for the best one? Horse Race spawns multiple Claude Code agents in parallel, lets them learn from each other, and picks the winner through a blind vote. All automatically.

## 📦 Installation

```bash
npx skills install qiao/horse-race-skill
```

## 🚀 Usage

Once installed, trigger the skill with the slash command:

```
/horse-race implement a linked list
```

Or just mention it naturally:

- "horse race this task"
- "race: implement a linked list"
- "compete on solving this bug"

Override defaults with e.g. "horse race this with 5 agents and 3 rounds".

## 🤔 Why Horse Race?

LLMs are non-deterministic. The same prompt can produce wildly different solutions. Some will be elegant, some will miss edge cases, some will be over-engineered. Instead of rolling the dice once and hoping for the best, Horse Race:

- 🔍 **Explores the solution space** by generating multiple independent approaches in parallel
- 🌱 **Cross-pollinates the best ideas** so each agent can learn from the others' strengths
- 🎭 **Eliminates bias** through blind judging. Judges never know which agent wrote which solution
- 🗳️ **Picks the winner fairly** using Borda count, a ranked-choice voting method that's hard to game

The result? Better code than any single generation, consistently. 💪

## ⚙️ How It Works

```mermaid
flowchart TD
    subgraph P1["Phase 1: Independent Implementation"]
        A1[Agent 1] -->|solves task| S1[Solution 1]
        A2[Agent 2] -->|solves task| S2[Solution 2]
        A3[Agent 3] -->|solves task| S3[Solution 3]
    end

    subgraph P2["Phase 2: Improvement Rounds"]
        S1 & S2 & S3 -->|share diffs| X((Cross-Pollination))
        X -->|improve| S1a["Solution 1'"]
        X -->|improve| S2a["Solution 2'"]
        X -->|improve| S3a["Solution 3'"]
    end

    S1a & S2a & S3a --> P3

    subgraph P3["Phase 3: Borda Count Voting"]
        J1[Judge 1] & J2[Judge 2] & J3[Judge 3] -->|rank & tally| W[Winner]
    end

    W -->|apply diff| R[Changes applied to working tree]
```

### 🐎 Phase 1: Independent Implementation

Multiple agents are spawned in parallel, each in its own isolated git worktree. Every agent independently solves the same task with no knowledge of what the others are doing. This naturally produces diverse approaches: different algorithms, different code structures, different edge case handling. It gives the process a wide solution space to work with.

### 🔄 Phase 2: Improvement Rounds

Here's where it gets interesting. After the initial implementations, each agent receives the diffs from **all** other agents. They study what others did better, identify clever approaches or edge cases they missed, and incorporate the best ideas into their own solution while maintaining coherence. This cross-pollination runs for multiple rounds (default: 2), with agents sharing updated diffs each time. Solutions converge toward higher quality while retaining their distinct approaches.

Think of it like a writers' room. Everyone drafts independently, then passes their work around the table for feedback and inspiration.

### 🏆 Phase 3: Borda Count Voting

Three fresh judge agents, with no connection to any solution, independently evaluate and rank all final solutions on correctness, code quality, edge case handling, simplicity, and performance. Scores are tallied using [Borda count](https://en.wikipedia.org/wiki/Borda_count): with N solutions, 1st place gets N-1 points, 2nd gets N-2, and so on. The solution with the highest total score wins. If there's a tie, a 4th tiebreak judge steps in.

The winning diff is applied to your working tree (but not committed), so you can review the changes before committing.

## 📋 Defaults

| Parameter | Default |
|---|---|
| Implementation agents | 3 |
| Improvement rounds | 2 |
| Voting judges | 3 |
