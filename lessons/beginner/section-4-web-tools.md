# Section 4: Web Tools (Lessons 18-20)

## Overview
Learn to leverage web capabilities for research, documentation access, and staying current with latest practices.

---

## Lesson 18: Web Search

### Objectives
- Search for current information beyond Claude's knowledge cutoff
- Use domain filtering effectively
- Cite sources properly in responses
- Combine web research with coding tasks

### Understanding Web Search

Web Search allows Claude to find current information from the internet. This is essential when:
- Working with rapidly evolving technologies
- Checking latest API documentation
- Finding solutions to new issues
- Researching best practices

### When to Use Web Search

**Good use cases:**
```
"Search for the latest React 19 features"
"Find current best practices for Next.js 14 app router"
"Look up the newest TypeScript 5.4 syntax"
"Search for solutions to [specific error message]"
```

**Not needed:**
```
"How do I write a for loop in JavaScript" (basic knowledge)
"What is REST API" (stable concepts)
"Python list comprehension syntax" (fundamental)
```

### Domain Filtering

You can focus searches on specific sites:

```
"Search for authentication best practices on auth0.com and okta.com"
"Find React testing tutorials only from testing-library.com"
"Look up AWS Lambda pricing on aws.amazon.com"
```

### Citing Sources

Claude automatically provides sources. Always verify important information from the original source.

### Exercise 18: Research and Implement

**Task**: Research a technology and implement what you learn

1. Ask Claude: "Search for the latest Zod validation library features and best practices"
2. Have Claude implement a validation schema using what it found
3. Ask Claude to cite where it learned each technique

**Verification**:
- [ ] Search returned current information
- [ ] Implementation uses recent features
- [ ] Sources are properly cited

---

## Lesson 19: Web Fetch

### Objectives
- Fetch and analyze specific web pages
- Extract information from documentation
- Handle redirects properly
- Use fetched content in coding tasks

### Understanding Web Fetch

Web Fetch retrieves content from a specific URL and processes it. Unlike Web Search, it targets a known page rather than searching.

### Common Use Cases

**Fetching documentation:**
```
"Fetch https://docs.example.com/api and summarize the endpoints"
```

**Reading specifications:**
```
"Fetch the JSON Schema spec page and explain the format"
```

**Analyzing examples:**
```
"Fetch this GitHub README and explain the project setup"
```

### Handling Redirects

Some URLs redirect. Claude will inform you and provide the redirect URL:

```
Original: http://example.com/docs
Redirect: https://docs.example.com/v2

Claude: "The URL redirected to https://docs.example.com/v2. Let me fetch that instead."
```

### Processing Large Pages

Claude can:
- Summarize lengthy content
- Extract specific sections
- Find relevant code examples
- Identify key configuration

### Exercise 19: Documentation Analysis

**Task**: Fetch and use documentation

1. Ask Claude: "Fetch the Express.js routing documentation and show me how to set up parameterized routes"
2. Have Claude create a sample implementation
3. Request Claude fetch error handling docs and add proper error handling

**Verification**:
- [ ] Documentation successfully fetched
- [ ] Key information extracted
- [ ] Implementation matches documentation

---

## Lesson 20: When to Use Web Tools

### Objectives
- Identify when web tools add value
- Understand Claude's knowledge limitations
- Prefer local docs when available
- Avoid unnecessary web requests

### Decision Framework

```
┌─────────────────────────────────────────────┐
│         Do I Need Web Tools?                │
├─────────────────────────────────────────────┤
│                                             │
│  Is this stable, fundamental knowledge?     │
│     YES → No web needed                     │
│     NO  ↓                                   │
│                                             │
│  Is this about recent releases/changes?     │
│     YES → Use Web Search                    │
│     NO  ↓                                   │
│                                             │
│  Do I need content from a specific URL?     │
│     YES → Use Web Fetch                     │
│     NO  ↓                                   │
│                                             │
│  Is there a local doc file?                 │
│     YES → Read the local file               │
│     NO  → Ask Claude directly first         │
│                                             │
└─────────────────────────────────────────────┘
```

### Examples by Category

**No web needed (stable knowledge):**
- Language syntax (JavaScript, Python, etc.)
- Data structures and algorithms
- Design patterns
- SQL basics
- Git commands

**Web Search helpful:**
- Latest framework versions
- New library features
- Current security advisories
- Recent API changes
- Community solutions to new issues

**Web Fetch appropriate:**
- Specific documentation pages
- API references
- Configuration guides
- README files

**Use local files:**
- Project documentation
- Team conventions
- Local API specs
- Configuration examples

### Token Efficiency

Web tools use tokens. Minimize usage by:
1. Asking Claude directly first for common knowledge
2. Using specific queries, not broad searches
3. Fetching specific pages, not entire sites
4. Caching results mentally for the session

### Exercise 20: Tool Selection Practice

**Task**: For each scenario, choose the right approach

**Scenarios**:

1. "I need to know how to use async/await in JavaScript"
   - Answer: No web needed - fundamental JavaScript

2. "What's new in Node.js 22?"
   - Answer: Web Search - recent release

3. "I want to implement Stripe payments following their guide"
   - Answer: Web Fetch - specific documentation

4. "How do I set up Jest in my project?"
   - Answer: Check for local jest.config.js first, then ask Claude

5. "What's the current Tailwind v4 syntax for gradients?"
   - Answer: Web Search - recent version changes

**Verification**:
- [ ] Correctly identified when web is needed
- [ ] Can distinguish Search vs Fetch use cases
- [ ] Understands local-first approach

---

## Section 4 Summary

### Key Takeaways

1. **Web Search** finds current information across the web
2. **Web Fetch** retrieves specific pages for analysis
3. **Local first** - prefer local docs when available
4. **Stable knowledge** doesn't require web lookups
5. **Cite sources** for accountability and verification

### Commands Learned
- Web Search with domain filtering
- Web Fetch with redirect handling
- Combining web content with coding

### Next Steps
Proceed to Section 5: Basic Configuration to learn about CLAUDE.md and settings.
