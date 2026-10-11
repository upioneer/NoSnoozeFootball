# NoSnoozeFootball (No Snooze Football) Implementation Plan and Task Backlog

## Document Control

* Status: Ready for Agent Execution
* Architecture Reference: project_details/design.md (Version 2.4)
* Target Environment: Multi container Docker Compose with local and remote access options
* Target Runtimes: Python 3.13 (FastAPI, uv), Node 24 LTS (React 19, Vite, Tailwind v4)

## Table of Contents

* Overview and Execution Philosophy
* Phase 0.0: Mandatory Agent Orientation and Repository Inspection Protocol (Prerequisite)
* Phase 0: Foundations and Environment Scaffolding
* Phase 1: Ingestion Layer and Replay Test Harness
* Phase 2: Live Scoreboard and Real Time Transport
* Phase 3: Scoring Engine, Diffing, and Win Probability
* Phase 4: AI Commentary Booth and Budget Tiers
* Phase 5: Strategic Roster Defense and Scouting Tools
* Phase 6: Draft Mode Workspace
* Phase 7: Polish, TV Wall Mode, and Documentation Index
* Verification Protocol for Autonomous Agents

## Overview and Execution Philosophy

This implementation plan provides a phased, actionable task backlog for building NoSnoozeFootball (No Snooze Football). Each phase builds sequentially on verified foundations. Every task defines clear acceptance criteria and proof expectations.

* Strict Test Driven Baseline: Scoring logic, win probability algorithms, and prompt sanitizers must have offline unit tests before being connected to UI components.
* Zero Hyphen Rule: All generated documentation artifacts must remain strictly free of hyphens and em dashes, using asterisks for all bullet lists.
* Mobile First Requirement: Every web view must be implemented and verified at 320 px before desktop styling is applied.
* Tasty Anti Slop UI Protocol: Every user interface view and component must strictly comply with the Tasty design engine (.skills/tasty/SKILL.md). The application must deliberately reject the generic AI boilerplate signature (no purple gradients, no pure blacks, no neutral grays, no Inter or system font defaults, no bouncy easing, and no repetitive bento card crutches). The design must embody a bespoke, utilitarian Workbench and Broadcast War Room macrostructure with high telemetry density, purposeful easing, locked design tokens, and strict eight state discipline for all interactive controls.

## Phase 0.0: Mandatory Agent Orientation and Repository Inspection Protocol (Prerequisite)

Goal: Ensure any incoming coding agent gains complete contextual awareness and operational understanding of the architecture, constraints, governance, and existing tooling before modifying or creating any code.

### Tasks

* [ ] Task 0.0.1: Full Architectural Review of project_details/design.md
  * Read project_details/design.md in its entirety (Version 2.4).
  * Understand the data source reality check: verified endpoints, feature to source mapping, and fallbacks.
  * Understand the container architecture: single worker FastAPI backend, unprivileged nginx proxying /api and /ws, SQLite with WAL mode.
  * Understand the core scoring engine, four tier event classifier, live normal distribution win probability model, and football budget tiers.
  * Understand the in app documentation center requirements at route /docs.
  * Understand the Tasty design philosophy, broadcast war room macrostructure, tinted color palette, and eight state interactive component requirements in Section 18.
  * Acceptance: Agent explicitly confirms comprehension of the full system architecture in its initial output.

* [ ] Task 0.0.2: Repository Governance and Protocol Review of project_details/
  * Read all governance documents in project_details/ (agent.md, testing.md, contributing.md).
  * Master the immutable constraints: strict semantic versioning, versioned changelogs, README badges verification, environment centric configuration (.env), zero emojis, and zero hyphens or em dashes in documentation artifacts.
  * Inspect project_details/playbooks/, project_details/proof/, and project_details/changelog/.
  * Acceptance: Agent confirms understanding of repository governance and immutable constraints.

* [ ] Task 0.0.3: Agent Skills and Automation Capabilities Review of .skills/
  * Inspect all installed skills in .skills/ (changelog_plus, docker, playwright, skeleton, tasty, etc.).
  * Study the Tasty Anti Slop Engine (.skills/tasty/SKILL.md) to internalize the macrostructure rotation, the Eight State Discipline, the Anti Slop Manifesto bans, the pre flight scan protocol, the pre emit self critique, and the command vocabulary (/tasty audit, /tasty polish, /tasty critique, /tasty distill, /tasty bolder, /tasty quieter, /tasty animate).
  * Understand the Changelog Plus release pipeline (.skills/changelog_plus/SKILL.md) and how bump_version.ps1 integrates with automated media capture.
  * Review Playwright testing workflows (.skills/playwright/SKILL.md) for automated headless visual capture and responsive verification.
  * Review Skeleton layout reservation patterns (.skills/skeleton/SKILL.md) for zero Cumulative Layout Shift performance.
  * Acceptance: Agent explicitly confirms comprehension of the Tasty anti slop guidelines and automation harnesses before initiating frontend construction.

* [ ] Task 0.0.4: Environment and Template Inspection
  * Inspect root configuration files, including .env, .env.example, package configs, and docker manifests.
  * Note existing scaffold keys in .env that must be preserved for future consumption.
  * Acceptance: Agent verifies environment variables and secret boundaries before running local builds.

## Phase 0: Foundations and Environment Scaffolding

Goal: Establish the runnable multi container workspace, environment templates, database migration engine, version bumping script, and mobile first application shell adhering to Tasty design tokens.

### Tasks

* [ ] Task 0.1: Project Directory Scaffolding
  * Create src/backend and src/frontend directories per the design layout.
  * Initialize src/backend with uv, pyproject.toml, and basic package structure.
  * Initialize src/frontend with Vite, React 19, TypeScript, and Tailwind CSS v4.
  * Acceptance: Clean directory structure matching Section 3 of design.md.

* [ ] Task 0.2: Containerization and Local Compose Stack
  * Create deploy/Dockerfile.api (multi stage, non root user, read only root filesystem).
  * Create deploy/Dockerfile.web (multi stage Vite build, unprivileged nginx).
  * Create deploy/nginx.conf (reverse proxying /api and /ws to api container, single origin).
  * Create deploy/compose.yaml (services for api and web, named volume for /data, port 3000 published).
  * Add extra_hosts mapping host.docker.internal to host gateway for Linux compatibility.
  * Acceptance: Running docker compose up starts healthy containers with no privilege escalation warnings.

* [ ] Task 0.3: Environment Configuration Baseline
  * Create .env.example containing all runtime variables while preserving existing scaffold keys.
  * Implement src/backend/app/config.py using pydantic settings to parse and validate .env keys.
  * Acceptance: FastAPI fails fast on missing required variables with descriptive error messages.

* [ ] Task 0.4: Version Bump Script and Release Automation Wiring
  * Deploy bump_version.ps1 from .skills/changelog_plus/templates to the repository root.
  * Adapt package manifest detection to update both src/frontend/package.json and src/backend/pyproject.toml.
  * Connect automated capture scripts from Changelog Plus.
  * Acceptance: Dry run of ./bump_version.ps1 0.1.0 succeeds and validates paths.

* [ ] Task 0.5: Database Layer and Migration Baseline
  * Configure SQLAlchemy 2 with aiosqlite and WAL mode on /data/nosnooze.db.
  * Implement single writer queue to serialize database write transactions.
  * Set up Alembic migration environment in src/backend/app/db.
  * Create initial migration script reflecting all STRICT tables defined in Section 13 of design.md.
  * Acceptance: Alembic upgrades database cleanly from zero to head with valid SQLite schemas.

* [ ] Task 0.6: FastAPI Application Factory and Health Probes
  * Implement src/backend/app/main.py with lifespan handlers for database and poller lifecycle.
  * Implement GET /api/health (liveness probe) and GET /api/ready (readiness probe).
  * Mount slowapi rate limiting middleware.
  * Acceptance: Curl requests to /api/health return HTTP 200 with JSON status ok.

* [ ] Task 0.7: Mobile First Frontend Shell and Tasty Token Architecture
  * Execute Tasty Pre Flight Scan to establish design tokens in Tailwind v4 and CSS variables:
    * Surface palette: Tinted dark surfaces with deep slate and obsidian undertones (zero pure blacks #000000 and zero neutral grays).
    * Accent palette: Broadcast emerald turf and stadium amber (zero purple or violet gradients per the Lila Rule).
    * Typography stack: Distinctive athletic condensed display font for headings and scores, tabular mono numerals for live stats, and clean humanist sans for body text (zero Arial or Inter defaults).
    * Motion stance: Crisp cubic bezier easing curves (cubic bezier(0.16, 1, 0.3, 1)) with zero bouncy or elastic easing.
  * Implement base component styles enforcing the Eight State Discipline (default, hover, focus visible, active, disabled, loading, error, success) for all buttons and interactive controls.
  * Implement React Router with route placeholders: / (Game Day), /draft, /history, /docs, /settings, /onboarding, /login.
  * Build responsive layout shell: sticky header with connection status dot and budget badge, bottom tab bar for mobile viewports (320 px to 767 px), and top bar for desktop viewports.
  * Implement basic /docs documentation viewer frame.
  * Capture baseline Playwright full page screenshot to verify visual hierarchy and zero layout shifts.
  * Acceptance: Shell loads at 320 px viewport width with zero horizontal overflow, minimum 48 px touch targets, and verified compliance with Tasty design tokens.

## Phase 1: Ingestion Layer and Replay Test Harness

Goal: Build resilient clients for Sleeper and ESPN, establish local player database caching, create the onboarding flow, and construct the offline game day recorder.

### Tasks

* [ ] Task 1.1: Sleeper API Client Implementation
  * Create src/backend/app/ingest/sleeper_client.py using httpx AsyncClient.
  * Implement token bucket rate limiter strictly capping calls at 30 requests per minute.
  * Add typed methods for state, users, leagues, rosters, matchups, transactions, drafts, and picks.
  * Implement methods for undocumented stats and projections endpoints.
  * Acceptance: Client successfully queries Sleeper endpoints with automatic retries and exponential backoff.

* [ ] Task 1.2: ESPN Scoreboard Auxiliary Client
  * Create src/backend/app/ingest/espn_client.py querying the public NFL scoreboard endpoint.
  * Parse game clock, period, possession, down and distance, red zone flags, and recent play text.
  * Add team abbreviation alias map reconciling Sleeper and ESPN team name discrepancies.
  * Acceptance: Client correctly normalizes live NFL game situations into typed pydantic models.

* [ ] Task 1.3: Master Player Database Daily Sync
  * Implement ingestion routine for GET https://api.sleeper.app/v1/players/nfl.
  * Store player metadata in the players table with daily timestamp tracking.
  * Enforce rate guidance: never refresh the 5 MB player database more than once per 24 hours.
  * Build fast local player search index by full name and position.
  * Acceptance: Player database hydrates locally and resolves player names within 2 milliseconds.

* [ ] Task 1.4: Onboarding API and First Run User Experience
  * Implement GET /api/onboarding/leagues?username= to list available leagues.
  * Implement POST /api/onboarding to persist tracked league selections and designate the pinned league.
  * Build frontend onboarding screen guiding user through username entry and league selection.
  * Apply Tasty eight state styling to all inputs, league cards, and confirmation buttons.
  * Acceptance: Entering a Sleeper username displays league cards and persists selections to the database.

* [ ] Task 1.5: Offline Game Day Recorder and Replay Engine
  * Create src/backend/app/replay/recorder.py to serialize raw API responses during live game windows.
  * Create src/backend/app/replay/replayer.py to replay recorded Sundays across a virtual clock (1x to 60x speed).
  * Generate a synthetic Sunday replay dataset in tests/fixtures for offline development.
  * Acceptance: Replay engine simulates live game cycles deterministically without external network calls.

## Phase 2: Live Scoreboard and Real Time Transport

Goal: Implement the schedule driven adaptive polling engine, persistent snapshot diffing baseline, WebSocket real time hub, and interactive mobile first broadcast scoreboard.

### Tasks

* [ ] Task 2.1: Adaptive Polling State Machine
  * Implement src/backend/app/poller/state_machine.py with states: Offseason, Idle, Pregame, Live, Settling.
  * Drive state transitions dynamically using ESPN kickoff schedules and live game statuses.
  * Implement cadence registry scheduling matchups (60s in Live), ESPN scoreboard (30s), and transactions (15m).
  * Build support for manual override cadences and ad hoc refresh requests.
  * Acceptance: Poller transitions between states based on simulated game clock timestamps.

* [ ] Task 2.2: Snapshot Storage and Payload Hashing
  * Implement matchup snapshot persistence in the matchup_snapshots table.
  * Hash incoming payloads using SHA256 to skip downstream diffing when data is unchanged.
  * Acceptance: Unchanged poll responses consume zero database write transactions.

* [ ] Task 2.3: WebSocket Streaming Hub and Recovery Protocol
  * Implement src/backend/app/realtime/hub.py managing active client WebSocket connections.
  * Wrap all messages in the standard envelope with sequence numbers, timestamps, and message types.
  * Maintain an in memory ring buffer of the last 500 messages.
  * Implement HELLO handshake with sequence recovery to replay missed messages upon client reconnection.
  * Implement 20 second heartbeat ping pong loop.
  * Acceptance: Disconnecting and reconnecting a client receives replayed sequence messages without gaps.

* [ ] Task 2.4: Hero Matchup Pane Component
  * Build frontend Hero Matchup component adhering to Tasty Workbench macrostructure guidelines.
  * Display pinned head to head contest with high telemetry density, avoiding generic card or bento templates.
  * Display team names, live total scores, projected scores, and expandable starter lists with mono tabular numerals.
  * Add requestAnimationFrame count up animation for live score increments with crisp cubic bezier easing.
  * Add pulsing status rings for starters whose NFL offenses currently possess the ball.
  * Implement all eight interactive states across starter expansion triggers.
  * Acceptance: Score updates animate smoothly at 60 fps with responsive layout on 320 px screens and zero visual slop.

* [ ] Task 2.5: League Matchup Grid Component
  * Build responsive league grid displaying all concurrent matchups in the active league.
  * Implement bespoke command center dense layout adapting automatically to any league size (8 to 16 teams) without repetitive card slop.
  * Add lead change indicator badges with high contrast styling.
  * Implement click to swap interaction moving any secondary matchup into focus, featuring full eight state button treatments.
  * Acceptance: Grid displays cleanly across mobile list views and desktop multi column grids with distinct visual hierarchy.

* [ ] Task 2.6: Real Time Scoring Ticker Component
  * Build persistent horizontal play ticker docked at the bottom of the dashboard with broadcast styling.
  * Support modes: My Matchup, League Wide, and NFL Explosives.
  * Include pause on hover and pause on focus interactions with clear visual feedback.
  * Respect user prefers reduced motion accessibility setting by rendering static updates.
  * Acceptance: Ticker scrolls scoring plays smoothly and halts on touch or cursor interaction with full keyboard accessibility.

* [ ] Task 2.7: Passcode Authentication and Session Security
  * Implement POST /api/auth/login and POST /api/auth/logout using Argon2id passcode verification.
  * Issue HttpOnly, SameSite Strict session cookies.
  * Enforce authentication when APP_BIND_HOST is configured to a non loopback address.
  * Enforce login rate limits (maximum 5 attempts per minute per IP).
  * Build polished login view conforming to Tasty eight state input and submission styling.
  * Acceptance: Unauthorized requests on LAN interfaces receive HTTP 401 until passcode is authenticated.

## Phase 3: Scoring Engine, Diffing, and Win Probability

Goal: Build the league custom scoring calculator, stat level diffing engine, four tier event classifier, and live normal distribution win probability model.

### Tasks

* [ ] Task 3.1: Custom League Scoring Engine
  * Implement src/backend/app/engine/scoring.py to rescore raw player stat lines against league settings.
  * Support passing, rushing, receiving, kicking, defense, and position specific scoring rules.
  * Verify computed point values against Sleeper matchup points, logging discrepancies over 0.05 points.
  * Acceptance: Unit test suite verifies exact score matching across PPR, half PPR, and standard leagues.

* [ ] Task 3.2: Deterministic State Diff Engine
  * Implement src/backend/app/engine/diff.py comparing snapshot N against snapshot N minus 1.
  * Isolate scoring deltas across team totals, individual player points, and granular stat lines.
  * Ensure initial poll after container boot establishes a silent baseline with zero emitted events.
  * Generate deterministic unique event keys to guarantee idempotent deduplication in the database.
  * Process negative score deltas as normal scoring events.
  * Acceptance: Repeated replays of identical cycles emit zero duplicate events.

* [ ] Task 3.3: Event Classifier and Severity Engine
  * Implement src/backend/app/engine/classifier.py assigning severity rankings: Minor, Notable, Major, Critical.
  * Detect touchdowns, turnovers, 50 yard field goals, big plays, and defensive sacks.
  * Implement team level red zone alerts and 1st and goal alerts from ESPN game situation data.
  * Implement Vulture Alert detecting goal line touchdowns by non starting athletes at the same position.
  * Acceptance: Replay test suite correctly classifies test plays into designated severity tiers.

* [ ] Task 3.4: Live Win Probability Model
  * Implement src/backend/app/engine/winprob.py using the normal distribution model defined in Section 9.
  * Calculate expected remaining points and position specific variance based on remaining game minutes.
  * Compute win probability using the standard normal cumulative distribution function.
  * Add support for weekly median matchup probability in leagues utilizing median scoring.
  * Store calculations in the win_prob_history table to generate momentum curves.
  * Acceptance: Model calculates accurate probabilities that converge to 100 percent when games conclude.

* [ ] Task 3.5: Deterministic Template Feed and Live Stream UI
  * Build string template bank rendering descriptive plain text summaries for all events with zero token cost.
  * Build vertical broadcast style event stream component on the frontend with high information density.
  * Support filter chips: Pinned Matchup, League Wide, Alerts Only, implementing eight state interactive styling.
  * Implement pin play action allowing users to keep high leverage plays visible.
  * Apply Skeleton loading reservations to prevent layout shifts during incoming play stream updates.
  * Acceptance: Event stream renders play cards in real time with zero layout shift and verified contrast.

## Phase 4: AI Commentary Booth and Budget Tiers

Goal: Implement the unified frontier AI gateway with Ollama host bridging, persona prompt builder, football themed budget ledger, and Web Speech audio synthesis.

### Tasks

* [ ] Task 4.1: Unified Frontier AI Gateway and Local Engine Bridge
  * Implement src/backend/app/booth/gateway/ supporting direct frontier APIs: Anthropic Messages, OpenAI Chat Completions, Google AI Studio Gemini, Moonshot Kimi, and Meta AI (Meta Muse Spark and Llama 3.3).
  * Implement OpenRouter aggregator client for single key access to frontier and open weights models (including DeepSeek, Mistral, and Qwen).
  * Implement custom OpenAI compatible endpoint support for generic providers (Groq, Together, DeepInfra, vLLM, LM Studio).
  * Add support for local engines: Ollama (http://host.docker.internal:11434/v1) and llama.cpp server (http://host.docker.internal:8080/v1).
  * Support target model declaration strictly via free form text input (LLM_MODEL) in configuration and the /settings dashboard, avoiding restrictive or fragile hardcoded dropdown lists.
  * Implement 8 second generation timeout with automatic fallback to deterministic templates.
  * Acceptance: Gateway successfully executes mock and real completions across cloud frontier keys and local engines, falling back gracefully on timeout.

* [ ] Task 4.2: Persona Registry and Template Banks
  * Create persona definition JSON files in src/backend/personas for all six characters:
    * Tony Romeo (tony_romeo)
    * Peyton Landing (peyton_landing)
    * Marshawn Grinch (marshawn_grinch)
    * Jameisnot (jameisnot)
    * Senior Sanders (senior_sanders)
    * The Suit (the_suit)
  * Equip every persona with a deterministic offline template bank for all event types.
  * Acceptance: Persona registry loads dynamically on startup and provides templates when AI is disabled.

* [ ] Task 4.3: Single Turn Multi Persona Prompt Builder
  * Implement src/backend/app/booth/prompt_builder.py packing active personas into a single prompt.
  * Format compressed play context blocks (100 to 140 tokens).
  * Require structured JSON output conforming to pydantic response schemas.
  * Implement prompt caching optimization for static persona instructions.
  * Acceptance: Multi persona prompt dispatches return validated JSON containing one reaction per persona.

* [ ] Task 4.4: Content Guards, Sanitizers, and Output Verification
  * Implement emoji filter stripping all emojis and pictographs from model outputs.
  * Implement hallucination guard replacing lines that mention athletes absent from the play context.
  * Sanitize user controlled team names and handles before prompt injection.
  * Enforce maximum 220 character length limit per reaction.
  * Acceptance: Tests verify emoji stripping, length enforcement, and prompt injection neutralization.

* [ ] Task 4.5: Football Themed Weekly Budget Ledger
  * Implement llm_usage database ledger recording prompt tokens, completion tokens, latency, and costs.
  * Support the six football themed tiers: No Budget (0), First Down (10k), Field Goal (30k), Touchdown (100k), Pick 6 (250k), and Unlimited.
  * Implement automated degradation: limit to two personas at 80 percent, lock out AI at 100 percent.
  * Implement 120 second per matchup debounce timer with event coalescing.
  * Build top bar budget gauge and settings tier selector in the UI.
  * Acceptance: Replay test reaching budget threshold smoothly transitions commentary to template banks.

* [ ] Task 4.6: Client Side Text to Speech Synthesis and Commentary UI
  * Build distinctive commentator reaction bubbles in the live feed with two letter monogram badges (zero emojis and zero fake avatars).
  * Implement Web Speech API integration in the frontend stream component.
  * Map persona registry pitch and rate settings to local browser synthesis voices.
  * Restrict spoken commentary to the pinned matchup and serialize line playback.
  * Include persistent audio mute toggle in the top bar with full eight state styling.
  * Acceptance: Enabling audio speaks incoming pinned reactions in sequence without overlapping, with crisp visual feedback.

## Phase 5: Strategic Roster Defense and Scouting Tools

Goal: Deliver on demand deterministic decision engines for start sit decisions, opponent roster audits, waiver blocking, FAAB budgeting, and snap share trends.

### Tasks

* [ ] Task 5.1: Start Sit Audit Engine
  * Implement POST /api/strategy/start_sit evaluating starting slots against bench alternatives.
  * Flag inactive, injured, or bye week starters.
  * Highlight late swap opportunities recommending latest kickoff players in flex positions.
  * Acceptance: API returns ranked start sit suggestions with projected point gains.

* [ ] Task 5.2: Opponent Defense Radar and Waiver Block Engine
  * Implement POST /api/strategy/opponent_audit identifying upcoming opponent roster vulnerabilities.
  * Implement POST /api/strategy/waiver_block analyzing available free agents at the opponent thinnest position.
  * Calculate defensive stash recommendations where blocking rival points exceeds lowest bench value.
  * Acceptance: Tool identifies opponent vulnerabilities and highlights high impact defensive waiver stashes.

* [ ] Task 5.3: FAAB Ledger Intelligence
  * Implement GET /api/strategy/faab analyzing league waiver transactions and remaining budgets.
  * Calculate historical winning bid percentiles by position.
  * Suggest optimal minimum bids outbidding rivals who share positional needs.
  * Acceptance: Tool outputs calculated optimal FAAB bids based on actual league transaction history.

* [ ] Task 5.4: Snap Share Jump Engine and Strength of Schedule
  * Implement GET /api/strategy/snap_trends calculating two week snap share surges of 15 percent or more.
  * Implement GET /api/strategy/sos calculating lookahead defensive matchup difficulty for upcoming weeks.
  * Display Sleeper trending adds with required Sleeper attribution label.
  * Acceptance: Snap share surges and four week streaming outlooks display accurately in the strategy sheet.

* [ ] Task 5.5: Strategic Companion UI Sheet
  * Build collapsible utilitarian war room drawer component on the frontend housing all strategic tools.
  * Design dense telemetry cards for start sit audits, opponent radar, waiver blocks, and FAAB percentiles without generic card boilerplate.
  * Add optional request summary narrative button powered by The Suit (rate limited to 6 calls per hour) with loading and success states.
  * Acceptance: Drawer opens smoothly on mobile and desktop, displaying computed strategic telemetry with zero layout overflow.

## Phase 6: Draft Mode Workspace

Goal: Build the dedicated Draft Mode interface for live drafts and mock drafts with pick synchronization and tier cliff alerts.

### Tasks

* [ ] Task 6.1: Draft Synchronization Engine
  * Implement GET /api/drafts and GET /api/drafts/{draft_id}/board syncing Sleeper draft picks.
  * Poll draft pick stream every 3 seconds while draft status is active.
  * Calculate pick clock math (picks remaining until user turn) supporting snake, linear, and third round reversal.
  * Acceptance: Sync engine accurately updates draft picks with sub second latency during simulated drafts.

* [ ] Task 6.2: Positional Tier Cliff Tracker
  * Compute positional tiers using Sleeper ADP or custom imported rankings.
  * Calculate standard deviation point drop offs between consecutive players.
  * Trigger tier cliff alert when remaining players in user top tier equal picks until next turn.
  * Acceptance: Tier cliff warnings trigger reliably when positional depth nears a sharp drop off.

* [ ] Task 6.3: Standalone Draft Mode UI (Route /draft)
  * Build dedicated draft board interface decoupled from the game day view with high information density.
  * Display live pick board, available player pool with positional color banding, roster need meter, and tier cliff warnings.
  * Ensure all pick selections and filter toggles implement eight state interactive discipline.
  * Acceptance: Draft interface renders with zero lag, providing smooth filtering by position and sharp contrast on all screens.

## Phase 7: Polish, TV Wall Mode, and Documentation Index

Goal: Build the complete In App Documentation Center, TV Wall Mode, PWA manifest, and conduct comprehensive Tasty design audits and production hardening.

### Tasks

* [ ] Task 7.1: In App Help and Documentation Center Implementation
  * Build full frontend documentation viewer at route /docs with clean editorial layout and zero nested container noise.
  * Author all eight self contained documentation modules specified in Section 18 of design.md.
  * Implement sub millisecond client side search index with keyword highlights.
  * Add hash anchor deep linking and one click snippet copy buttons with eight state visual feedback.
  * Acceptance: /docs renders all guides offline with instant search on 320 px and desktop viewports.

* [ ] Task 7.2: TV Wall Mode Watch Party Layout
  * Build full screen Wall Mode layout at route /?mode=wall for screens 1440 px and wider.
  * Include large broadcast display typography, auto rotating matchup views every 30 seconds, and audible audio.
  * Acceptance: Wall Mode runs hands free on large displays with automatic focus rotation and high visibility.

* [ ] Task 7.3: Tasty Design Audit, Polish Pass, and Container Hardening
  * Execute Tasty design review pass using the /tasty audit criteria across all views.
  * Score every view across the six Tasty axes: Philosophy, Hierarchy, Execution, Specificity, Restraint, Variety (all must score 3 or higher).
  * Run /tasty polish pass to eliminate stray styles and lock all tokens to the Tailwind v4 design system.
  * Execute Playwright headless visual test harness capturing full page screenshots across 320 px, 390 px, 768 px, 1024 px, and 1440 px viewports, verifying zero layout shifts and minimum 48 px touch targets.
  * Run security audits across backend and frontend dependencies (pip audit, npm audit).
  * Verify Docker image non root compliance and read only filesystem enforcement.
  * Acceptance: All automated test suites, container health checks, and Tasty visual quality audits pass cleanly.

## Verification Protocol for Autonomous Agents

Before marking any phase or task complete, the executing agent must:

* 1. Run full test suite: pytest for backend, npm test for frontend.
* 2. Validate responsive compliance: Verify interfaces at 320 px baseline viewport with zero horizontal overflow.
* 3. Validate Tasty UI standards: Confirm adherence to the Anti Slop Manifesto (no purple gradients, no pure blacks, locked tokens, eight state interactive discipline, and minimum 3 score across all six critique axes).
* 4. Perform visual proof capture: Use Playwright to capture full page screenshots of modified screens and store them in project_details/proof.
* 5. Validate zero hyphen rule: Ensure no documentation artifacts or changelog entries contain hyphens or em dashes.
* 6. Record proof artifacts: Save test logs and execution output to project_details/proof.
* 7. Seek explicit user permission: Never execute git push or version bump scripts without prior user approval.
