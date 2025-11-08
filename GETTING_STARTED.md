# Getting Started with apresai Skills Marketplace

**Supercharge Claude Code with expert domain knowledge in 10 minutes**

---

## What You'll Learn

By the end of this guide, you'll understand:
- What skills are and how they improve Claude's code generation
- How to install and use skills from this marketplace
- How to get the most value from domain-specific skills
- How to run your first AI-generated automation script

---

## What Are Skills?

**Skills are expert knowledge packages** that teach Claude Code how to generate professional-quality code in specific domains.

### The Problem

When you ask Claude to generate code, it uses general programming knowledge. For specialized domains (browser automation, computer vision, testing frameworks), this can result in:
- Basic examples that need extensive modification
- Missing best practices and security patterns
- No error handling or production-ready features
- Outdated or non-idiomatic approaches

### The Solution

Skills provide Claude with:
- Complete API references for specialized libraries
- Production-tested patterns and examples
- Security and error handling best practices
- Domain-specific knowledge that's hard to find elsewhere

### Example: Before and After

**Without a skill:**
```python
# You: "Create a web scraper for product prices"
# Claude generates basic requests/BeautifulSoup code
# Breaks easily when HTML changes
```

**With a skill:**
```python
# You: "Using the Nova Act skill, create a web scraper for product prices"
# Claude generates AI-native browser automation
# Resilient to HTML changes, production-ready
```

---

## Quick Start

### Prerequisites

- [ ] **Claude Code** installed and working
- [ ] **10 minutes** of free time
- [ ] **Python 3.10+** (for running generated scripts)

---

### Step 1: Add the Marketplace (30 seconds)

Open Claude Code and run:

```bash
/plugin marketplace add apresai/apresai-skills
```

You'll see confirmation that the marketplace was added.

---

### Step 2: Browse Available Skills (1 minute)

See what's available:

```bash
/plugin
```

You'll see a list of skills in this marketplace. Currently available:
- **nova-act-skill** - AI-powered browser automation

More skills coming soon (testing frameworks, data validation, computer vision, etc.)

---

### Step 3: Install Your First Skill (30 seconds)

Let's install the Nova Act browser automation skill:

```bash
/plugin install nova-act-skill
```

Claude Code now has expert knowledge of browser automation!

---

### Step 4: Try It Out (3 minutes)

The Nova Act skill requires a few setup steps. Let's do them:

**Get a Nova Act API key:**
1. Visit [nova.amazon.com/act](https://nova.amazon.com/act)
2. Sign in with your Amazon.com account (shopping account, not AWS)
3. Generate your API key
4. Copy it

**Set the API key:**
```bash
export NOVA_ACT_API_KEY="paste_your_key_here"
```

**Install Nova Act SDK:**
```bash
pip install nova-act
playwright install chrome
```

---

### Step 5: Ask Claude to Build Something (5 minutes)

Now ask Claude to generate a script, **mentioning the skill by name**:

**Example Prompt:**
> *"Using the Nova Act skill, create a script that searches Google for 'python programming' and prints the title of the first result."*

**What Claude will generate:**
```python
from nova_act import NovaAct

with NovaAct(starting_page="https://www.google.com") as nova:
    nova.act("search for python programming")
    nova.act("click on the first result")
    print(f"Title: {nova.page.title()}")
```

**Run it:**
```bash
python your_script.py
```

You'll see a browser window open, perform the search, and print the result!

---

## Understanding How Skills Work

### What Happens When You Use a Skill?

```
1. You mention the skill in your prompt
         ↓
2. Claude loads the skill's knowledge base
         ↓
3. Claude references:
   - Complete API documentation
   - Best practices and patterns
   - Security guidelines
   - Working examples
         ↓
4. Claude generates production-ready code
```

### Skills vs General Knowledge

| Aspect | Without Skill | With Skill |
|--------|---------------|------------|
| **Code Quality** | Basic/example level | Production-ready |
| **Best Practices** | May be missing | Built in |
| **Error Handling** | Minimal | Comprehensive |
| **Security** | Generic | Domain-specific |
| **Patterns** | Standard | Expert-level |
| **Examples** | Must look up | Referenced automatically |

---

## Getting the Most from Skills

### 1. Always Mention the Skill

**Good:**
> "Using the Nova Act skill, create a price monitoring script"

**Not as good:**
> "Create a price monitoring script"

When you mention the skill, Claude knows to use that expert knowledge.

### 2. Be Specific About Requirements

**Better:**
> "Using the Nova Act skill, create a script that searches Amazon for laptops under $1000, extracts the top 5 with prices and ratings, saves to CSV, and handles errors gracefully."

**Not as specific:**
> "Search Amazon for laptops"

### 3. Ask for Production Features

Skills include production patterns, so ask for them:
- Error handling and retries
- Logging and monitoring
- Parallel execution
- Security best practices

### 4. Request Examples from the Skill

Many skills include working examples:
> "Using the Nova Act skill, create a script similar to the price comparison example but for tracking real estate listings."

---

## Available Skills Deep Dive

### 🤖 nova-act-skill

**Domain:** Browser automation using AI

**What it teaches Claude:**
- Complete Nova Act API (20KB reference)
- Breaking tasks into atomic `act()` calls
- Pydantic schemas for data extraction
- Parallel browser sessions
- Authentication and session management
- Security patterns for credentials
- Error handling strategies

**Example use cases:**
- E-commerce price comparison
- Automated form filling
- Website monitoring and alerts
- QA testing workflows
- Data collection from multiple sources

**Example prompts to try:**
> "Using the Nova Act skill, create a script that monitors Best Buy for a specific product and alerts me when the price drops below $500."

> "Using the Nova Act skill, build a form-filling script that auto-fills job applications with my resume data."

> "Using the Nova Act skill, create a parallel web scraper that compares prices across Amazon, Walmart, and Target."

**See full documentation:** [plugins/nova-act-skill/README.md](./plugins/nova-act-skill/README.md)

---

## Common Questions

### Do I need to reinstall skills for each project?

No! Skills are installed globally in Claude Code. Once installed, just reference them by name in any conversation.

### Can I use multiple skills together?

Yes! Mention multiple skills:
> "Using the Nova Act skill and the data validation skill, create a web scraper that validates extracted data."

### How do I update a skill?

```bash
/plugin update nova-act-skill
```

Check the skill's README for version and changelog information.

### Do skills work offline?

Skills are reference documents Claude uses when generating code. Once installed, they work offline. However, the generated code may require internet (e.g., Nova Act requires API access).

### How are skills different from plugins?

- **Plugins** add new functionality to Claude Code (new commands, tools)
- **Skills** teach Claude domain expertise (better code generation)

Both are valuable, but serve different purposes.

---

## Troubleshooting

### Issue: "Skill not found"

**Solution:** Make sure you've added the marketplace:
```bash
/plugin marketplace add apresai/apresai-skills
/plugin install nova-act-skill
```

### Issue: Generated code doesn't use skill knowledge

**Solution:** Make sure you mention the skill in your prompt:
> "Using the Nova Act skill, create..."

### Issue: Skill-specific errors (e.g., "API key not found")

**Solution:** Check the skill's specific documentation:
- [Nova Act Quick Start](./plugins/nova-act-skill/QUICKSTART.md)
- Each skill has its own setup requirements

---

## Next Steps

### Explore Skill Examples

Each skill comes with production-ready examples:

```bash
# Nova Act examples
ls plugins/nova-act-skill/examples/

# Run them
python plugins/nova-act-skill/examples/01_price_comparison.py
```

### Read Skill Documentation

Deep dive into a skill's capabilities:
- **README.md** - Overview and quick start
- **SKILL.md** - Complete API reference
- **QUICKSTART.md** - 5-minute setup
- **examples/** - Working code

### Build Something Real

Think about your own use cases:
- What repetitive tasks could you automate?
- What data do you need to collect regularly?
- What workflows could be improved with automation?

Ask Claude to build it using the appropriate skill!

### Watch for New Skills

This marketplace is growing. Future skills planned:
- Testing frameworks (pytest, mocking, fixtures)
- Data validation (Pydantic, data quality)
- Computer vision (image processing, OCR)
- NLP (text analysis, sentiment)
- API clients (REST, GraphQL)

---

## Real-World Examples

### Example 1: E-commerce Research

**Goal:** Track prices for 20 products daily across 3 retailers

**Prompt:**
> "Using the Nova Act skill, create a script that checks prices for a list of products across Amazon, Best Buy, and Target, saves results to CSV with timestamps, and runs in headless mode for scheduling."

**Result:** Production-ready monitoring script with parallel execution

**Time saved:** 2 hours/day → 5 minutes/day

---

### Example 2: Job Application Automation

**Goal:** Apply to 50 positions with similar requirements

**Prompt:**
> "Using the Nova Act skill, create a form-filling script that logs into a job portal, finds positions matching criteria, auto-fills applications with my data from a JSON file, and handles captchas with user intervention."

**Result:** Semi-automated application workflow

**Time saved:** 25 hours → 3 hours

---

### Example 3: QA Testing

**Goal:** Test checkout flow with 10 different scenarios

**Prompt:**
> "Using the Nova Act skill, create a testing script that validates our checkout flow with different payment methods, addresses, and products. Take screenshots at each step and generate a report."

**Result:** Automated test suite with visual verification

**Time saved:** 2 hours → 10 minutes per test run

---

## Quick Reference

### Installation Commands
```bash
# Add marketplace
/plugin marketplace add apresai/apresai-skills

# Browse skills
/plugin

# Install a skill
/plugin install nova-act-skill

# Update a skill
/plugin update nova-act-skill

# Remove a skill
/plugin remove nova-act-skill
```

### Prompting Pattern
```
Using the [SKILL_NAME] skill, [DESCRIBE WHAT YOU WANT]

Examples:
- "Using the Nova Act skill, create a web scraper that..."
- "Using the testing skill, generate pytest fixtures for..."
- "Using the data validation skill, create schemas for..."
```

### Where to Get Help
- **Marketplace:** [README.md](./README.md)
- **Skills:** Individual skill documentation in `plugins/`
- **Issues:** [GitHub Issues](https://github.com/apresai/apresai-skills/issues)
- **Claude Code:** [claude.ai/code](https://claude.ai/code)

---

## You're Ready!

You now know how to:
- ✅ Install and use skills from this marketplace
- ✅ Prompt Claude to use domain expertise
- ✅ Generate production-ready code in specialized domains
- ✅ Find skill documentation and examples
- ✅ Build real-world automation workflows

**Start building with expert-level guidance!** 🚀

---

## What to Build First?

Not sure where to start? Here are ideas by skill level:

**Beginner:**
- Simple web scraper for a single website
- Form auto-filler for repeated data entry
- Website availability checker

**Intermediate:**
- Multi-site price comparison tool
- Automated job application workflow
- Website change detection system

**Advanced:**
- Parallel data collection pipeline
- Full QA testing suite
- Complex monitoring with alerting

**Pick one and ask Claude to build it using the appropriate skill!**
