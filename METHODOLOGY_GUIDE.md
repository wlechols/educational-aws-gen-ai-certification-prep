# Process Guide: Generating Educational Content with Kiro and Claude

This guide walks through the end-to-end process of using [Kiro](https://kiro.dev) (an AI-powered IDE with spec-driven development) and Claude models to generate high-quality educational content. The approach described here was used to build an 85-question interactive prep exam for the AWS Certified Generative AI Developer – Professional certification, but the methodology applies to any certification or educational topic.

## Prerequisites

Before starting, make sure you have:

- **Kiro IDE** — Download from [kiro.dev](https://kiro.dev). Kiro provides the spec-driven development workflow that structures the entire process.
- **An exam guide or curriculum document** — A scope-defining document that outlines the topics, domains, and weightings for the content you want to generate.
- **Example Q&A pairs** (recommended) — A small set of sample questions and answers from classes, learning materials, or practice resources that demonstrate the style, complexity, and format you want to achieve.
- **Access to official documentation** — For validating generated content against authoritative sources (e.g., AWS documentation for AWS certifications).

## Step 1: Define Content Scope

Start by identifying the boundaries of what you want to generate. A well-defined scope prevents the content from drifting off-topic and ensures comprehensive coverage.

**Use an exam guide as your scope document.** For this project, we used the [AWS Certified Generative AI Developer – Professional exam guide](https://docs.aws.amazon.com/aws-certification/latest/examguides/ai-professional-01.html) which defines:

- **Domains** — The major topic areas (e.g., "Domain 1: Fundamentals of AI and ML" with a specific weighting like 20%)
- **Tasks within each domain** — Specific skills and knowledge areas tested
- **Weightings** — The percentage of the exam devoted to each domain

Extract this structure into a reference document. This becomes your content spectrum — the map that tells you what topics to cover and in what proportion.

**For non-certification content**, you can define your own scope document with:
- Topic categories and their relative importance
- Learning objectives for each category
- Depth expectations (introductory, intermediate, advanced)

## Step 2: Prepare Input Materials

The quality of generated content depends heavily on the inputs you provide. Two types of input materials work together:

### Exam Guide (Content Spectrum)

The exam guide defines *what* to cover. Extract:
- The complete list of domains and their weightings
- Task statements within each domain
- Knowledge and skill statements

This ensures your generated content covers the full breadth of the subject matter in the right proportions.

### Example Q&A Pairs (Style and Complexity Calibration)

Example questions and answers define *how* the content should look. Gather 5–10 representative examples that demonstrate:
- **Question format** — Multiple choice, multiple select, scenario-based, etc.
- **Complexity level** — How deep the questions go, how many concepts they combine
- **Answer structure** — How correct answers are justified, how distractors are constructed
- **Justification style** — How explanations reference specific services, features, or concepts

These examples act as a style guide for the AI. Without them, generated content tends to be either too generic or inconsistently formatted.

**Tip:** Include a mix of easy, medium, and hard examples so the AI calibrates to the full difficulty range.


## Step 3: Create Kiro Specs

Kiro's spec-driven development workflow structures the generation process into three artifacts. Each plays a distinct role:

### requirements.md — What to Build

The requirements document defines the desired outcome using user stories and acceptance criteria. For educational content generation, this includes:

- What type of content to produce (e.g., multiple-choice questions with justifications)
- How many items to generate and in what distribution across domains
- Format and structure requirements (e.g., 4 answer options per question, detailed justifications for each option)
- Quality criteria (e.g., questions must reference specific AWS services, distractors must be plausible)

Kiro helps you write these requirements through an interactive process — you describe what you want, and Kiro structures it into formal acceptance criteria.

### design.md — How to Build It

The design document translates requirements into a technical plan. For content generation, this covers:

- The generation strategy (e.g., batch generation of 5 questions at a time per domain)
- Data models (e.g., the JSON or Markdown structure for questions, answers, and metadata)
- Content architecture (e.g., how questions are organized, tagged, and stored)
- Correctness properties (e.g., every question must have exactly one correct answer, justifications must reference the relevant AWS service)

The design document is where you make decisions about *how* the AI will generate content — batch sizes, iteration strategy, and validation rules.

### tasks.md — Incremental Implementation Steps

The tasks document breaks the design into small, ordered steps. For content generation, tasks might look like:

1. Generate 5 questions for Domain 1 (Fundamentals)
2. Review and validate Domain 1 questions against official docs
3. Generate 5 questions for Domain 2 (Model Development)
4. ...and so on through all domains
5. Review domain distribution and fill gaps
6. Build the delivery format (HTML exam, PDF, etc.)

Each task is scoped to be independently completable and verifiable. This incremental approach is critical — generating all content in one shot produces lower quality than iterating in small batches with review cycles.

## Step 4: Generate Content Iteratively

With specs in place, work through the tasks one at a time. For each batch of content:

1. **Provide context** — Give the AI the exam guide excerpt for the target domain, plus your example Q&A pairs as style reference
2. **Generate a small batch** — Ask for 3–5 questions at a time rather than dozens. Smaller batches produce more focused, higher-quality output
3. **Review immediately** — Check each batch before moving on. Look for:
   - Factual accuracy (does the question align with current AWS documentation?)
   - Appropriate difficulty (not too easy, not impossibly obscure)
   - Plausible distractors (wrong answers should be believable, not obviously wrong)
   - Clear justifications (explanations should teach, not just state the answer)
4. **Iterate and refine** — If a batch has issues, provide feedback and regenerate. The spec-driven approach means you can reference specific requirements when giving feedback (e.g., "This question doesn't meet acceptance criterion 3.4 — justifications must reference specific AWS services")

**Batch size matters.** This project generated questions in batches of 5, which balanced quality with throughput. Larger batches (10+) tend to produce repetitive or lower-quality items.

**Version your content.** The question set in this project went through multiple versions as questions were refined, replaced, and improved. Treat content generation as iterative, not one-shot.

## Step 5: Quality Control and Validation

After generating all content, perform a systematic quality review.

### Review Against Official Documentation

- Cross-reference each question's subject matter with the official AWS documentation
- Verify that service names, feature descriptions, and technical details are accurate and current
- Check that justifications correctly explain *why* an answer is right or wrong
- Flag any content that references deprecated services or outdated information

### Validate Answer Justifications

- Every correct answer justification should clearly explain the reasoning
- Every incorrect answer justification should explain why it's wrong *and* when it might be partially applicable
- Justifications should be educational — a learner reading them should understand the concept better

### Check Domain Distribution

- Tally the number of questions per domain
- Compare against the target weightings from the exam guide
- Fill gaps where domains are underrepresented
- Trim or replace questions in overrepresented domains

For this project, the target distribution was based on the five domains in the exam guide, each with a specific percentage weighting. The final 85-question set was balanced to approximate these weightings.

### Additional Quality Checks

- **No duplicate concepts** — Ensure questions test different aspects, not the same concept rephrased
- **Consistent formatting** — All questions should follow the same structure and style
- **Appropriate difficulty curve** — Mix of straightforward recall, applied knowledge, and scenario-based analysis questions

## Tips and Best Practices

- **Start small, then scale.** Generate a handful of questions first to calibrate the style and quality before committing to the full set. It's much easier to adjust the approach early.

- **Use the exam guide as your north star.** Every question should trace back to a specific domain and task in the exam guide. If you can't map a question to the guide, it's probably off-scope.

- **Example Q&A pairs are your most powerful input.** The difference between generic AI-generated questions and professional-quality ones often comes down to the examples you provide. Invest time in curating good examples.

- **Review in context, not in isolation.** Read questions in sequence as a learner would. This reveals issues like repetitive phrasing, uneven difficulty, or gaps in topic coverage that aren't visible when reviewing one question at a time.

- **Leverage Kiro's spec artifacts for traceability.** When you find an issue during review, trace it back to the relevant requirement or design decision. This makes it easier to fix systematically rather than playing whack-a-mole.

- **Don't skip the justification review.** Justifications are where most factual errors hide. A question might have the right answer but a misleading or incorrect explanation. These are the hardest to catch and the most important to fix.

- **Plan for multiple iterations.** Expect to revise your content at least 2–3 times. Each pass catches different types of issues — factual accuracy first, then style consistency, then coverage balance.

- **Keep your scope document updated.** If the exam guide or curriculum changes, update your requirements and regenerate affected content. The spec-driven approach makes this straightforward since you can identify exactly which tasks need to be re-executed.

- **Update the granularity of the requirements.** As you go through quality reviews, update the requirements to close quality gaps for the next batches, defining and improving the quality bar over time.