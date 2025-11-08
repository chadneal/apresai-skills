# Nova Act Skill for Claude Code

**AI-Powered Browser Automation**

Generate production-ready Python scripts that control web browsers using natural language commands. Built on Amazon Nova Act, the AI-native browser automation SDK.

---

## What Is This?

This skill teaches Claude Code how to generate production-quality browser automation scripts using Amazon Nova Act. Instead of writing brittle CSS selectors and XPath queries, you describe what you want in plain English and Claude generates complete, working Python code.

### What Can You Build?

**E-commerce Automation**
- Compare prices across Amazon, Best Buy, Target (see it run in parallel!)
- Monitor products for price drops and stock availability
- Automate order tracking and status updates
- Extract product reviews and ratings at scale

**Form Automation**
- Auto-fill job applications with your resume data
- Submit surveys and feedback forms in bulk
- Create accounts across multiple platforms
- Handle multi-step registration workflows

**Web Scraping & Monitoring**
- Extract structured data from any website
- Monitor competitors' pricing and inventory
- Track real estate listings for new properties
- Get alerts when content changes on websites you care about

**Testing & QA**
- Automate user flow testing
- Validate forms and checkout processes
- Run regression tests across browsers
- Screenshot capture for visual testing

---

## Quick Start

### 1. Install the Skill

In Claude Code:

```bash
/plugin install nova-act-skill
```

### 2. Get Your Nova Act API Key

1. Visit [nova.amazon.com/act](https://nova.amazon.com/act)
2. Sign in with your Amazon.com account (the one you shop with)
3. Generate your API key
4. Set it in your environment:

```bash
export NOVA_ACT_API_KEY="your_api_key_here"
```

### 3. Install Nova Act SDK

```bash
pip install nova-act
playwright install chrome
```

### 4. Ask Claude to Build Something!

Try these example prompts with Claude Code:

> *"Using the Nova Act skill, create a script that searches Amazon for wireless headphones under $200, extracts the top 5 results with prices and ratings, and saves them to a CSV file."*

> *"Build a Nova Act script that monitors a specific product page on Best Buy and sends me an alert when the price drops below $500."*

> *"Create a form-filling script that logs into my job portal and applies to positions matching my criteria."*

Claude will generate a complete, production-ready Python script using Nova Act best practices.

---

## What's Included

### Core Skill Document
- **SKILL.md** - Complete Nova Act skill documentation covering:
  - Nova Act fundamentals and philosophy
  - Complete API reference
  - Best practices and patterns
  - Common use cases and examples
  - Error handling strategies
  - Advanced features

### Example Scripts

Three complete, production-ready examples demonstrating different Nova Act use cases:

#### 1. Price Comparison (`examples/01_price_comparison.py`)
**Use Case**: E-commerce product research and price comparison

**Features**:
- Parallel web scraping across multiple retailers (Amazon, Best Buy, Target)
- Structured data extraction with Pydantic schemas
- Price comparison and savings calculation
- JSON export of results
- Concurrent execution with ThreadPoolExecutor

**Key Learnings**:
- Breaking down complex tasks into smaller act() calls
- Using Pydantic schemas for reliable data extraction
- Parallel browser sessions for speed
- Error handling with try/except

#### 2. Form Filling (`examples/02_form_filling.py`)
**Use Case**: Automated job application submission

**Features**:
- Persistent authentication with saved browser sessions
- Multi-step form navigation
- Captcha detection and human intervention
- File upload handling
- Pre-submission review and confirmation
- Screenshot capture for verification

**Key Learnings**:
- Setting up and reusing authenticated sessions
- Handling captchas with user intervention
- Using BOOL_SCHEMA for yes/no questions
- File upload with Playwright integration
- Taking screenshots for audit trails

#### 3. Monitoring & Alerts (`examples/03_monitoring_alerts.py`)
**Use Case**: Website monitoring and change detection

**Features**:
- Scheduled monitoring with configurable intervals
- Change detection (price, stock status, content)
- Alert system for significant changes
- Historical data tracking
- Summary report generation
- Single-check and scheduled modes

**Key Learnings**:
- Headless browser automation
- State comparison across runs
- Persistent data storage
- Scheduled task execution
- Building reusable monitoring classes

### Additional Documentation

- **QUICKSTART.md** - 5-minute installation and setup guide
- **OVERVIEW.md** - Detailed package contents and learning paths
- **config_template.py** - Production-ready configuration template

---

## How It Works

### Traditional Approach
```python
# Complex, brittle code
from selenium import webdriver
driver = webdriver.Chrome()
driver.get("https://example.com")
search_box = driver.find_element(By.ID, "search")
search_box.send_keys("product")
submit = driver.find_element(By.CSS_SELECTOR, "button[type='submit']")
submit.click()
# Hope the selectors don't change!
```

### Nova Act Approach
```python
# Simple, natural language
from nova_act import NovaAct

with NovaAct(starting_page="https://example.com") as nova:
    nova.act("search for product")
    nova.act("click the first result")
    # AI figures out the selectors!
```

### The Claude Code + Nova Act Advantage

Just tell Claude what you want to automate, and it generates the complete script:

**Your Request:**
> "Create a script that finds the cheapest laptop under $1000 on Amazon"

**Claude Generates:**
```python
from nova_act import NovaAct
from pydantic import BaseModel

class Laptop(BaseModel):
    name: str
    price: float
    rating: float

with NovaAct(starting_page="https://amazon.com") as nova:
    nova.act("search for laptops")
    nova.act("set price filter to under $1000")
    nova.act("sort by price low to high")

    result = nova.act(
        "Extract name, price, and rating of the first laptop",
        schema=Laptop.model_json_schema()
    )

    if result.matches_schema:
        laptop = Laptop.model_validate(result.parsed_response)
        print(f"Best deal: {laptop.name} - ${laptop.price}")
```

---

## Example Gallery

### Price Comparison Script
Searches multiple retailers in parallel and finds the best deal:

```python
# Run searches concurrently
with ThreadPoolExecutor(max_workers=3) as executor:
    futures = {
        executor.submit(search_amazon, "headphones"): "Amazon",
        executor.submit(search_bestbuy, "headphones"): "Best Buy",
        executor.submit(search_target, "headphones"): "Target",
    }
```

**Output:**
```
PRICE COMPARISON RESULTS
========================================
1. Target
   Product: Sony WH-1000XM5
   Price: $349.99
   In Stock: Yes
   Rating: 4.8/5.0

Best Deal: Target at $349.99
Savings: $50.00 vs most expensive option
```

### Monitoring Script
Detects price changes and sends alerts:

```python
monitor = WebsiteMonitor(
    url="https://www.example.com/product",
    check_interval=3600,  # Check every hour
    alert_on_price_drop=True
)
monitor.run()
```

**Output:**
```
🔔 ALERT: Price dropped from $399.99 to $349.99!
📊 Monitoring session complete:
   - Total checks: 24
   - Changes detected: 2
   - Price alerts: 1
```

---

## Best Practices (Built Into Generated Code)

### ✅ Break Tasks Into Small Steps
Nova Act works best with atomic, specific commands:
```python
# Good
nova.act("click the search button")
nova.act("enter 'laptops' in the search box")
nova.act("click the first result")

# Not ideal
nova.act("search for laptops and click the first result")
```

### ✅ Use Schemas for Data
Always use Pydantic for structured extraction:
```python
class Product(BaseModel):
    name: str
    price: float
    in_stock: bool

result = nova.act("extract product info", schema=Product.model_json_schema())
```

### ✅ Handle Errors Gracefully
Production code includes proper error handling:
```python
from nova_act import ActError

try:
    with NovaAct(starting_page=url) as nova:
        nova.act("perform action")
except ActError as e:
    logger.error(f"Automation failed: {e}")
    # Retry or alert
```

### ✅ Secure Credential Handling
Never pass passwords to act() prompts:
```python
# Good - direct Playwright interaction
import getpass
password = getpass.getpass()
nova.page.keyboard.type(password)

# Bad - visible in screenshots
nova.act(f"enter password {password}")
```

---

## Key Features

### 🧠 AI-Native Automation
No CSS selectors. No XPath. Just describe what you want in plain English.

### 📊 Structured Data Extraction
Use Pydantic schemas to extract clean, typed data—no regex parsing nightmares.

### ⚡ Parallel Execution
Run multiple browser sessions concurrently. Compare prices across 10 retailers in the time it takes to check one.

### 🔒 Security-First
Built-in patterns for handling passwords, sessions, and sensitive data safely.

### 🎯 Production-Ready
Error handling, logging, retry logic, and monitoring built into generated code.

### 📚 Complete Documentation
20KB skill reference, working examples, troubleshooting guides, and best practices.

---

## System Requirements

- **Python:** 3.10 or higher
- **Operating Systems:** macOS, Ubuntu 22.04+, WSL2, Windows 10+
- **Location:** US-based (Nova Act is currently US-only)
- **Browser:** Chrome recommended (auto-installed via Playwright)
- **API Key:** Free tier available at [nova.amazon.com/act](https://nova.amazon.com/act)

---

## Installation

See [QUICKSTART.md](./QUICKSTART.md) for detailed setup instructions and troubleshooting.

**Quick install:**
```bash
# Install SDK
pip install nova-act
playwright install chrome

# Set API key
export NOVA_ACT_API_KEY="your_api_key_from_nova.amazon.com"

# Test it out
python examples/01_price_comparison.py
```

---

## Documentation

- **[SKILL.md](./SKILL.md)** - Complete API documentation and reference (20KB)
- **[QUICKSTART.md](./QUICKSTART.md)** - 5-minute setup walkthrough
- **[OVERVIEW.md](./OVERVIEW.md)** - Detailed contents and learning paths
- **[examples/](./examples/)** - Three production-ready example scripts
- **[config_template.py](./config_template.py)** - Configuration template

---

## Common Use Cases

The skill covers these primary use cases:

1. **E-commerce Automation**
   - Price monitoring
   - Product comparison
   - Cart management
   - Order tracking

2. **Form Filling**
   - Job applications
   - Survey completion
   - Data entry
   - Account creation

3. **Web Scraping**
   - Data extraction
   - Content monitoring
   - Research automation
   - Competitive analysis

4. **Testing & QA**
   - Automated testing
   - User flow validation
   - Regression testing
   - Cross-browser testing

5. **Monitoring**
   - Website availability
   - Content changes
   - Price tracking
   - Inventory monitoring

---

## Limitations

- **Geographic:** US availability only
- **Language:** English language only
- **Scope:** Browser-based automation only (no desktop apps)
- **PDF:** Not optimized for PDF interaction
- **Complexity:** Very high-level prompts may be unreliable (break into steps)
- **Screen Size:** Optimized for 864×1296 to 1536×2304 resolution
- **No iPython support**

---

## Resources

**Nova Act Official:**
- Documentation: [nova.amazon.com/act](https://nova.amazon.com/act)
- GitHub: [github.com/aws/nova-act](https://github.com/aws/nova-act)
- Blog: [labs.amazon.science/blog/nova-act](https://labs.amazon.science/blog/nova-act)
- IDE Extension: [github.com/aws/nova-act-extension](https://github.com/aws/nova-act-extension)
- Email: nova-act@amazon.com

---

## Contributing

To improve this skill:
1. Add new example scripts for different use cases
2. Document new patterns and best practices
3. Update API reference as Nova Act evolves
4. Add troubleshooting guides for common issues

---

## License

This skill documentation and examples are provided as-is for educational purposes. Nova Act itself is subject to Amazon's licensing and acceptable use policies.

---

**Last Updated:** November 2025
**Nova Act Version:** Research Preview
**Skill Version:** 1.0
