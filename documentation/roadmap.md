# Project Roadmap

Build toward this goal: **a Python RL agent that learns an easy Geometry Dash level through a C++ mod with precise control over game timing.**

Keep the first version limited to **one short cube-only level, numerical observations, and two actions: hold or release jump**.

### Phase 1 — Plan the project now
*No installations needed.*

- Create your GitHub repository.
- Write a README explaining the goal, planned architecture, and current status.
- Create GitHub issues for the milestones below.
- Write down the planned interface:
  - `reset()` → initial state
  - `step(action, ticks)` → resulting state, ticks advanced, death/completion
- Choose a short level for initial experiments.

**Done when:** someone reading the repository can understand what you’re building and what you’ll implement first.

### Phase 2 — Learn enough Geode to make a working mod
*Install development tools when you’re ready.*

- Set up the Windows C++ toolchain and compatible Geode loader/SDK.
- Generate and build a starter mod.
- Add a small on-screen debug display.
- Read and display the player’s position.
- Detect level start, death, and reset.

Focus your learning on **hooks, inheritance, pointers, and the game update loop**.

**Done when:** your mod displays live player information and reliably detects each attempt.

### Phase 3 — Control inputs and attempts

- Apply jump press and release through the game.
- Trigger a level restart.
- Clear held inputs when restarting.
- Replay a simple, predefined sequence of jumps.
- Record player state during the attempt.

**Done when:** the mod can run and restart attempts without keyboard automation.

### Phase 4 — Build precise simulation stepping
**This is the hardest engineering milestone.**

- Freeze gameplay while keeping the application responsive.
- Advance exactly one physics tick on command.
- Apply the chosen input before advancing.
- Support advancing several ticks, stopping early on death or completion.
- Prevent the regular update loop from advancing gameplay a second time.

Validate it by replaying identical inputs from the same initial state. Compare trajectories and death timing. Introduce delays between commands and verify they don’t affect the result.

**Done when:** actions depend on simulation ticks rather than how quickly your program sends commands.

### Phase 5 — Connect Python

- Add a localhost socket connection.
- Queue incoming commands for execution on the game thread.
- Implement Python methods for `reset()` and `step()`.
- Include request IDs and clear error responses.
- Run a random-action script across repeated attempts.

Keep the first communication format simple; JSON is sufficient for an initial prototype.

**Done when:** Python can complete 100 consecutive attempts without hangs, missed resets, or mixed-up responses.

### Phase 6 — Create the RL environment

Wrap the Python client in a Gymnasium environment.

| Component | Initial design |
|---|---|
| Observation | Player height, vertical velocity, grounded state, nearby obstacle positions and sizes |
| Actions | Hold jump / release jump |
| Reward | Forward progress, death penalty, completion bonus |
| Episode end | Death or completion |
| Truncation | Maximum allowed ticks |
| Reset | Restart and return a fresh observation |

Normalize numerical observations. Ensure death and completion are reported before the game resets.

**Done when:** the environment passes Gymnasium/SB3 environment checks and a random agent can use it reliably.

### Phase 7 — Train your first agent

- Start with PPO and a small neural network.
- Train on the easiest level or section.
- Log reward, progress, episode length, and completion rate.
- Save model checkpoints.
- Evaluate saved models in separate attempts.
- Watch replays to catch problems with the reward or observations.

If learning stalls, check the environment and reward before spending hours tuning the model.

**Done when:** a saved agent repeatedly completes your initial level without further training.

### Phase 8 — Make it a strong project

Choose **one** extension first:

| Extension | Question it answers |
|---|---|
| Unseen levels | Did it learn transferable behavior or memorize one level? |
| Screenshot observations | Can it learn using vision? |
| Different action intervals | How much timing precision does it need? |
| Faster simulation | How much can you increase training throughput? |
| In-mod inference | Can the trained agent run without the Python trainer? |

Document results with a gameplay video, completion rates, training attempts, hardware, and exact software versions. Compare against a random agent and a simple scripted baseline.

**Your next task today:** create the README and turn these eight phases into GitHub milestones. Your first coding target later is **a mod that displays player position and detects death**.