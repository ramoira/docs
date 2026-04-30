# Ramoira + Claude: Coworking Guide for Content Generation

This guide shows you how to use your Ramoira brand schema as a persistent context layer for Claude (either Claude Desktop or Claude Code), allowing you to generate perfectly on-brand content without writing massive "you are a brand copywriter" prompts.

## Phase 1: Capture the Brand

First, we need to extract the brand's voice and rules into a machine-readable format.

1. **Initialize the Schema:**
   Open your terminal and run the Ramoira interview process. This takes about 10-15 minutes of answering questions about your brand.
   ```bash
   # Set your Anthropic API key first
   export ANTHROPIC_API_KEY=your-key-here  # macOS/Linux
   # OR: $env:ANTHROPIC_API_KEY="your-key-here"  # Windows PowerShell

   npx ramoira init
   ```
   *Result:* This generates a `ramoira/brand.schema.json` file containing your entire brand DNA.

2. **Verify with the Brand Book:**
   Before generating content, verify that the AI understood the brand correctly by generating a human-readable brand book.
   ```bash
   npx ramoira book
   ```
   *Result:* This creates a `ramoira/brand-book.md` file. Read through it to ensure the archetype, tone, and positioning feel exactly right.

## Phase 2: Transfer Context to Claude

Now that you have your `brand.schema.json`, you can use it to feed Claude context.

### Option A: Using Claude Desktop / Web
1. Open Claude.
2. Click the **Attachment (paperclip)** icon and upload your `ramoira/brand.schema.json` file.
3. Start your conversation with one of the prompts below.

### Option B: Using Claude Code (CLI)
1. Navigate to the root directory of your project (where the `ramoira/` folder lives).
2. Run Claude Code. Because the schema is in your local directory, Claude can read it directly.
3. Use a prompt like: *"Read the brand rules in `ramoira/brand.schema.json` and help me write..."*

## Phase 3: Content Generation Prompts

Here are a few battle-tested prompts to get you started. Because Claude now has your full schema, you don't need to describe your brand's tone—you just need to assign the task.

### Prompt 1: Landing Page Hero Section
> "I need to write the hero section for our new landing page. Please review our `brand.schema.json`. Pay special attention to our Core Belief and our Primary Archetype. 
>
> Task: Write 3 variations of a hero headline (H1) and subheadline (H2). The product we are selling is [Product Name/Brief Description]. Ensure the tone perfectly matches our schema's voice descriptors and absolutely avoid any words in our 'avoidedWords' list."

### Prompt 2: UX / Error Messages (Microcopy)
> "Review our `brand.schema.json` for our brand's tone and register. We need to write a few standard UX error messages. Many brands sound robotic or overly quirky here. We need to sound like us.
>
> Task: Write the copy for the following scenarios:
> 1. A user enters the wrong password.
> 2. A 404 'Page Not Found' error.
> 3. A successful 'Form Submitted' confirmation.
>
> Use the 'voice.contrast' field in the schema to ensure you aren't falling into generic category tropes."

### Prompt 3: Social Media Launch Post
> "Review our `brand.schema.json`. We are launching a new feature: [Feature Description].
>
> Task: Draft a launch post for LinkedIn and a shorter version for X (Twitter). The post should lean heavily into our Narrative Territory. It needs to sound like it was written by the brand's founder, adhering strictly to our defined Tone."

### Prompt 4: The "Vibe Check" (Editing Existing Copy)
> "Here is a draft of an email campaign we wrote: 
> [Paste draft here]
>
> Task: Audit this draft against our `brand.schema.json`. Identify any sentences or phrases that violate our defined voice, tone, or archetype. Suggest specific rewrites for those sections to make them fully compliant with our brand schema."
