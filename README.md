# Take-Home Assignment: Rate Limiter Service

Welcome, and thank you for taking the time to work on this assignment. We've designed it to be a focused exercise that gives us insight into how you think about design, tradeoffs, and code quality — not just whether you can produce working code.

We respect your time. **The assignment is scoped to take 3–5 hours.** Please don't spend significantly more than that. If you run out of time, document what you would have done next in your `DESIGN.md` — we value clear thinking about tradeoffs as much as completed features.

---

## The Problem

Build a **rate limiter** that can be used to protect APIs, services, or any resource that needs to enforce request quotas. Your implementation will be evaluated as both a reusable library and a small demo service.

Rate limiters are deceptively simple on the surface but involve real design decisions around algorithms, concurrency, accuracy, and memory. There is no single "correct" solution — we want to see *your* approach and *your* reasoning.

---

## Requirements

### Core (must-have)

1. **Implement at least two rate-limiting algorithms.** Choose any two from:
   - Fixed window counter
   - Sliding window log
   - Sliding window counter
   - Token bucket
   - Leaky bucket

2. **Support per-key limiting.** Limits should apply independently per identifier (e.g. user ID, API key, IP address).

3. **Configurable limits.** Each key (or group of keys) should be able to have its own limit (e.g. `100 requests per minute`).

4. **Thread-safe.** The limiter must behave correctly under concurrent access.

5. **Expose a clean API.** A single method like `boolean tryAcquire(String key)` is fine — but the design is up to you.

6. **Demo HTTP service.** Wrap your limiter in a small Spring Boot (or Javalin / Micronaut / plain `HttpServer` — your choice) service with at least one endpoint protected by the limiter. Return appropriate status codes and headers (`429 Too Many Requests`, `Retry-After`, etc.).

7. **Tests.** Unit tests for the algorithms and at least one concurrency test demonstrating correctness under load.

### Stretch (optional — only if you have time)

Pick whichever interests you. Doing one well is better than doing all three poorly.

- **Distributed mode.** Sketch (or implement) how your limiter would work across multiple service instances. Redis is a common choice but not required.
- **Observability.** Expose metrics (allowed count, rejected count, current usage per key).
- **Dynamic configuration.** Allow limits to be updated at runtime without restarting.
- **Burst handling.** Demonstrate how your chosen algorithm handles traffic bursts and explain the tradeoff.

---

## What We're Looking For

We care about these things, roughly in order of importance:

1. **Design reasoning.** Why did you pick the algorithms you did? What tradeoffs did you make? This is the most important thing.
2. **Concurrency correctness.** Does your code actually work under load, or does it have subtle race conditions?
3. **Code quality.** Readable, well-structured, idiomatic Java. We're not looking for clever — we're looking for clear.
4. **Testing.** Tests that demonstrate the behavior matters, including edge cases.
5. **Production-mindedness.** Do you think about failure modes, observability, and operational concerns?

We are **not** evaluating:

- UI/frontend work (none required)
- How many features you cram in
- Whether you used a "trendy" framework
- Whether your solution matches some reference implementation

---

## The DESIGN.md (Important)

Alongside your code, please include a `DESIGN.md` file (max ~2 pages) covering:

1. **Which algorithms you chose and why.** What are their tradeoffs? When would you pick one over the other?
2. **How you handled concurrency.** What primitives did you use? Why? What did you consider and reject?
3. **Memory and scaling characteristics.** What happens when you have 1M unique keys? 100M?
4. **What you'd change for production.** What's missing? What did you deliberately leave out?
5. **What you'd do with another 4 hours.**

We weight this document heavily. A modest implementation with thoughtful design notes will score higher than an ambitious implementation with no reasoning.

---

## Tech Requirements

- **Language:** Java 17 or newer
- **Build tool:** Maven or Gradle
- **Framework:** Your choice (Spring Boot is fine; so is anything else)
- **Dependencies:** Minimize them. Don't pull in a library that already does rate limiting — the point is to build it yourself. Standard utility libraries (Guava, Caffeine, etc.) are fine if justified in `DESIGN.md`.
- **Persistence:** In-memory is fine for the core requirements.

---

## Submission

1. Push your code to a **private GitHub repository** and add the reviewer(s) as collaborators. (We will share usernames separately.)
2. Make sure the repo includes:
   - All source code
   - `DESIGN.md`
   - A `README.md` with clear setup and run instructions
   - Tests that can be run with a single command
3. Reply to the original email with the repo link when you're done.

---

## How to Run (your README should look like this)

Your repo's `README.md` should let us get the service running with no friction. Something like:

```bash
# Build
./mvnw clean install

# Run tests
./mvnw test

# Start the demo service
./mvnw spring-boot:run

# Try it
curl -i http://localhost:8080/api/hello -H "X-User-Id: alice"
```

---

## After You Submit

If your submission moves forward, we'll schedule a **45-minute follow-up discussion** where we'll:

- Walk through your code together
- Ask why you made certain decisions
- Discuss how you'd extend or change the design under different constraints (e.g. distributed, 10x scale, stricter accuracy)

This conversation matters as much as the code itself. Come ready to defend your choices — and to acknowledge what you'd do differently with hindsight.

---

## Questions?

If anything is unclear, please ask before you start coding. Asking good clarifying questions is a positive signal, not a negative one.

Good luck — we're looking forward to seeing your approach.
