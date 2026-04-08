# Sentiment-Based Workflow Routing System

## Overview
A sentiment-based routing workflow dynamically classifies user queries by emotional tone and routes them to specialized response paths. This ensures angry customers receive empathetic problem-solving, while satisfied users get upsell opportunities. The system uses LLM-powered sentiment analysis and conditional branching to optimize customer experience and support efficiency.

---

## Part 1: Sentiment Classification Framework

### Sentiment Categories

| Sentiment | Indicators | Priority | Response Type |
|-----------|-----------|----------|----------------|
| **Positive** | Satisfied, grateful, excited, complimentary | Low | Helpful suggestions, upsell, delight |
| **Neutral** | Informational, factual, no emotional tone | Medium | Direct, efficient, educational |
| **Negative** | Frustrated, disappointed, critical | High | Empathetic, solution-focused, ownership |
| **Highly Negative/Urgent** | Angry, hostile, threatening, crisis | Critical | Escalation, senior support, recovery |

### Sentiment Indicators (Keywords & Patterns)

**Positive Sentiment:**
- "Love," "amazing," "thank you," "great," "perfect," "excellent"
- Exclamation marks and emojis (😊, ❤️)
- Compliments about service or product

**Neutral Sentiment:**
- "How do I...," "What is...," "Can you...," "Need help with"
- Factual, no emotional adjectives
- "Please," "regards," "thanks"

**Negative Sentiment:**
- "Frustrated," "disappointed," "broken," "wrong," "failed"
- "Why isn't..." "This doesn't work," "Terrible"
- Mild complaints, expressed concerns

**Highly Negative/Urgent:**
- "Unacceptable," "angry," "lawsuit," "disaster," "lost money"
- "Demand," "NOW," "immediately," "escalate"
- Repeated complaints, hostile tone
- Revenue impact or safety concerns

---

## Part 2: Workflow Architecture

### High-Level Flow

```
User Input
    ↓
[SENTIMENT ANALYSIS]
    ├─ Extract sentiment score (0-100)
    ├─ Classify category (Pos/Neu/Neg/Urgent)
    └─ Identify key issues
    ↓
[CONDITIONAL ROUTING]
    ├─ Positive (80-100) → Upsell Path
    ├─ Neutral (40-79) → Standard Path
    ├─ Negative (10-39) → Recovery Path
    └─ Urgent (0-9) → Escalation Path
    ↓
[RESPONSE GENERATION]
    ├─ Apply tone modulation
    ├─ Tailor solution approach
    └─ Generate custom response
    ↓
Output + Metadata
```

### Decision Tree

```
START: User Input
  │
  ├─ SENTIMENT SCORE >= 80?
  │   YES → "POSITIVE" → Upsell/Delight
  │   NO → Continue
  │
  ├─ SENTIMENT SCORE >= 40?
  │   YES → "NEUTRAL" → Standard Help
  │   NO → Continue
  │
  ├─ SENTIMENT SCORE >= 10?
  │   YES → "NEGATIVE" → Empathy + Solve
  │   NO → Continue
  │
  └─ SENTIMENT SCORE < 10?
      YES → "URGENT" → Escalate to Human
      NO → Fallback (Neutral)
```

---

## Part 3: Routing Logic & Response Paths

### Path 1: Positive Sentiment (80-100)

**Characteristics:**
- Customer satisfied or delighted
- Low risk, high opportunity

**Response Strategy:**
- Acknowledge appreciation
- Introduce relevant upsells or add-ons
- Offer loyalty rewards or referral incentives
- Maintain warm, friendly tone

**Example Flow:**
```
Input: "Your product is amazing! Can I get more features?"
  ↓
Sentiment: 92 (Highly Positive)
  ↓
Routing: UPSELL PATH
  ↓
Response:
"Thank you so much! We love your enthusiasm! 
Based on your needs, you might benefit from our Premium tier, 
which includes [features]. Would you like a quick tour?"
```

---

### Path 2: Neutral Sentiment (40-79)

**Characteristics:**
- Customer seeking information
- Standard support request

**Response Strategy:**
- Provide clear, efficient answers
- Use step-by-step guidance
- Offer resources and documentation
- Remain professional and helpful

**Example Flow:**
```
Input: "How do I reset my password?"
  ↓
Sentiment: 55 (Neutral)
  ↓
Routing: STANDARD PATH
  ↓
Response:
"Sure! Here's how to reset your password:
1. Click 'Forgot Password' on the login page
2. Enter your email address
3. Check your email for a reset link
4. Create a new password
Let me know if you need further help."
```

---

### Path 3: Negative Sentiment (10-39)

**Characteristics:**
- Customer frustrated or disappointed
- Issue requires attention and empathy

**Response Strategy:**
- Acknowledge frustration explicitly
- Take ownership (avoid blame)
- Provide clear solutions
- Offer compensation or goodwill if needed
- Follow up to ensure resolution

**Example Flow:**
```
Input: "I tried to use your app this morning and it kept crashing. 
        This is really frustrating—I needed it for work!"
  ↓
Sentiment: 28 (Negative)
  ↓
Routing: RECOVERY PATH
  ↓
Response:
"I sincerely apologize for the frustration this caused you. 
I understand how critical app stability is, especially when you need it for work. 
Let's fix this right away:

1. Can you tell me which device/OS you're using?
2. Which version of the app do you have?
3. Try clearing the app cache: Settings → Apps → [AppName] → Clear Cache

We've identified a crash in the latest build. 
I'm escalating this to our engineering team as priority.
In the meantime, here's a discount code for the inconvenience: SORRY20.
Can I help with anything else?"
```

---

### Path 4: Highly Negative/Urgent (0-9)

**Characteristics:**
- Customer extremely frustrated, angry, or in crisis
- Immediate human intervention needed
- Risk of churn, escalation, or damage

**Response Strategy:**
- Express sincere empathy immediately
- Acknowledge severity
- Take urgent action (escalate to supervisor/specialist)
- Provide direct contact information
- Set clear expectations for next steps

**Example Flow:**
```
Input: "I've been locked out of my account for THREE DAYS! 
        I have important meetings scheduled and nothing is working. 
        This is unacceptable! I want to speak to a manager NOW!!!"
  ↓
Sentiment: 5 (Highly Negative/Urgent)
  ↓
Routing: ESCALATION PATH
  ↓
Response:
"I understand your frustration completely, and I sincerely apologize 
that this has persisted for three days. This is absolutely unacceptable, 
and I'm taking immediate action.

I'm escalating your case to our Senior Support Manager RIGHT NOW.
Your priority ticket: #Esc-2024-0847
Expected response time: Within 30 minutes

In the meantime:
- Direct line: [+1-555-SUPPORT]
- Senior Manager: [manager@company.com]
- Reference your ticket number in all communications

We will resolve this immediately. Thank you for your patience, 
and again, I apologize for this experience."
```

---

## Part 4: Reusable Prompt Template

### Master Sentiment Routing Prompt

```text
You are a sentiment-based customer support router.

Your task:
1. Analyze the sentiment of the user's message
2. Classify it into one of four categories
3. Route to the appropriate response path
4. Generate a response tailored to that path

---

USER MESSAGE:
{user_input}

---

STEP 1: SENTIMENT ANALYSIS
Analyze the emotional tone. Respond in JSON format:
{
  "sentiment_score": <0-100>,
  "sentiment_category": "positive" | "neutral" | "negative" | "urgent",
  "key_emotions": [list of detected emotions],
  "urgency_flags": [list of urgent keywords or patterns],
  "core_issue": "brief description of the underlying problem"
}

---

STEP 2: CONDITIONAL ROUTING
Based on sentiment_score, apply this logic:
- If score >= 80: Route to UPSELL_PATH
- If 40 <= score < 80: Route to STANDARD_PATH
- If 10 <= score < 40: Route to RECOVERY_PATH
- If score < 10: Route to ESCALATION_PATH

---

STEP 3: RESPONSE GENERATION
Generate a response appropriate for the routed path:

UPSELL_PATH:
- Acknowledge satisfaction
- Suggest related products/features
- Include relevant call-to-action
- Maintain warm, enthusiastic tone

STANDARD_PATH:
- Answer the question directly
- Provide clear, step-by-step guidance
- Offer additional resources
- Professional and helpful tone

RECOVERY_PATH:
- Express empathy for the frustration
- Take ownership of the issue
- Provide concrete solutions
- Offer compensation if appropriate
- Follow-up commitment

ESCALATION_PATH:
- Immediately acknowledge urgency and severity
- Apologize sincerely
- Provide direct contact information for senior support
- Create a priority ticket reference
- Set clear next-step expectations
- Senior support team notification trigger

---

OUTPUT FORMAT:
{
  "sentiment_analysis": {
    "score": <number>,
    "category": "<string>",
    "emotions": [<list>],
    "urgency_flags": [<list>]
  },
  "routing_decision": "<path>",
  "reasoning": "<brief explanation of routing>",
  "response": "<generated response>",
  "actions": {
    "escalate_to_human": <true/false>,
    "send_followup": <true/false>,
    "offer_compensation": <true/false>,
    "ticket_priority": "low" | "medium" | "high" | "critical"
  }
}

---

SELF-CHECK:
- Did I correctly identify the sentiment?
- Is the routing decision appropriate for the sentiment?
- Does the response match the routed path's tone and strategy?
- Are all urgent flags addressed?
- Is the escalation threshold appropriate?
```

---

## Part 5: Conditional Logic Rules

### Sentiment Scoring Algorithm

```python
def calculate_sentiment_score(user_input: str) -> int:
    """
    Calculate sentiment score (0-100) based on:
    1. Positive keyword count (weight +20 each)
    2. Negative keyword count (weight -20 each)
    3. Urgency keyword count (weight -15 each)
    4. Escalation keyword count (weight -30 each)
    5. Punctuation intensity (!, ?, length of caps) (weight ±10)
    
    Returns: Score 0-100 (0=most negative, 100=most positive)
    """
    
    positive_keywords = ["love", "amazing", "great", "thank", "excellent"]
    negative_keywords = ["frustrated", "disappointed", "broken", "wrong", "failed"]
    urgency_keywords = ["immediately", "asap", "now", "urgent", "crisis"]
    escalation_keywords = ["lawsuit", "angry", "unacceptable", "demand", "escalate"]
    
    score = 50  # Neutral baseline
    
    # Count keyword occurrences
    score += len([w for w in positive_keywords if w in user_input.lower()]) * 20
    score -= len([w for w in negative_keywords if w in user_input.lower()]) * 20
    score -= len([w for w in urgency_keywords if w in user_input.lower()]) * 15
    score -= len([w for w in escalation_keywords if w in user_input.lower()]) * 30
    
    # Punctuation analysis
    if "!!!" in user_input:
        score -= 10  # Intense emotion (negative context)
    if "???" in user_input:
        score += 5   # Confused but open
    score -= len([c for c in user_input if c.isupper()]) * 1  # ALL CAPS
    
    # Clamp score to 0-100
    return max(0, min(100, score))
```

### Routing Decision Table

| Score Range | Category | Path | Human Review | Escalation |
|-------------|----------|------|--------------|------------|
| 80-100 | Positive | Upsell | No | No |
| 40-79 | Neutral | Standard | No | No |
| 10-39 | Negative | Recovery | Maybe | No |
| 0-9 | Urgent | Escalation | **YES** | **YES** |

---

## Part 6: Real-World Examples

### Example 1: Positive Sentiment

**Input:**
```
"Your customer service is outstanding! I've been using your product 
for 6 months and couldn't be happier. Are there any advanced features 
or premium plans I should know about?"
```

**Analysis:**
```json
{
  "sentiment_score": 88,
  "sentiment_category": "positive",
  "key_emotions": ["satisfied", "grateful", "interested"],
  "urgency_flags": [],
  "core_issue": "Inquiry about premium features"
}
```

**Routing:** UPSELL_PATH

**Response:**
```
"Thank you so much for the kind words! We're thrilled you're getting 
great value from our product.

Based on your 6-month usage, you might be a perfect fit for our Pro Plan:
✓ Advanced analytics & reporting
✓ Priority support (1-hour response)
✓ Custom integrations
✓ Team collaboration features

Current savings: 30% off annual Pro Plan for loyal customers like you.
Ready to explore? Schedule a quick 15-minute demo: [link]

Also, we'd love a referral—every successful invite earns both you and 
your friend $50 credit!"
```

---

### Example 2: Neutral Sentiment

**Input:**
```
"Hi, I need to export my data in CSV format. Can you walk me through 
the steps? I'm using the mobile app."
```

**Analysis:**
```json
{
  "sentiment_score": 52,
  "sentiment_category": "neutral",
  "key_emotions": ["informational", "practical"],
  "urgency_flags": [],
  "core_issue": "Data export on mobile app"
}
```

**Routing:** STANDARD_PATH

**Response:**
```
"Absolutely! Here's how to export data as CSV on mobile:

1. Open the app and go to Menu → Data Management
2. Tap 'Export Data'
3. Select 'CSV Format' from the dropdown
4. Choose your date range
5. Tap 'Export'—a download link will be sent to your email

Note: CSV exports are available on the Pro plan and above.
Your current plan: [Plan Name]

If you need to upgrade, visit Settings → Plans.
Questions? Check our Help Center: [link]"
```

---

### Example 3: Negative Sentiment

**Input:**
```
"I've been trying to contact support for 2 days with no response. 
My account is having serious issues and I have a presentation tomorrow. 
This is really frustrating—your company says you have '24-hour support' 
but that's clearly not true."
```

**Analysis:**
```json
{
  "sentiment_score": 22,
  "sentiment_category": "negative",
  "key_emotions": ["frustrated", "concerned", "disappointed"],
  "urgency_flags": ["2 days", "serious", "tomorrow"],
  "core_issue": "Delayed support response with time-sensitive need"
}
```

**Routing:** RECOVERY_PATH

**Response:**
```
"I hear your frustration completely, and I sincerely apologize 
for not responding sooner. A 2-day wait falls short of our commitment, 
and that's on us.

Here's what I'm doing RIGHT NOW:

1. Escalating your case to our Priority Support Specialist
2. Assigning ticket #RCV-2024-1156 (high priority)
3. Direct contact: [specialist@company.com] + [phone]
4. Expected resolution within 2 hours

5 minutes ago, I triggered our system to investigate your account issue.

To help speed resolution:
- Can you briefly describe the account issue?
- What specifically is failing in your presentation?

We take service commitments seriously. 
Once this is resolved, let's discuss how we prevent this in future 
(we may offer a service credit).

I'm personally following your ticket. We'll get you ready for tomorrow."
```

---

### Example 4: Highly Negative/Urgent Sentiment

**Input:**
```
"UNACCEPTABLE!!! I paid $500 for this service a WEEK AGO and it's 
still not working. I have lost BUSINESS because of this. I'm filing a 
complaint with my credit card company and considering legal action. 
GET A MANAGER ON THE PHONE NOW!!!"
```

**Analysis:**
```json
{
  "sentiment_score": 3,
  "sentiment_category": "urgent",
  "key_emotions": ["angry", "threatened", "desperate"],
  "urgency_flags": ["UNACCEPTABLE", "NOW", "legal action", "complaint"],
  "core_issue": "Service failure with revenue impact; customer extremely hostile"
}
```

**Routing:** ESCALATION_PATH

**Response:**
```
"I understand your anger completely. Losing business due to our failure 
is unacceptable, and I sincerely apologize.

IMMEDIATE ACTIONS:
✓ Escalating to VP of Customer Success NOW
✓ Priority Ticket: #ESC-2024-0428-LEGAL
✓ Assigned Senior Account Manager: [name] [direct_line] [email]
✓ Expected contact within 15 MINUTES

NEXT STEPS:
1. Senior Manager will call you at [your_number] or preferred contact
2. We will investigate your account failure immediately
3. We will provide a resolution or full refund + compensation offer
4. Legal inquiry reference: Case #2024-0428

CONTACT INFO:
- Senior Support Hotline: +1-800-PRIORITY
- VP Escalation: [vp_email] + [urgent_line]
- Case Coordinator: [coordinator] available 24/7

You will hear from us within 15 minutes. 
This is being treated as a P1 crisis.
We take full responsibility and will make this right.

Again, I sincerely apologize for this experience and the impact on your business."
```

---

## Part 7: Fallback & Error Handling

### Misclassification Prevention

| Risk | Detection | Mitigation |
|------|-----------|-----------|
| False Positive (neutral → positive) | Verify with keyword double-check | Manual review if score near boundary |
| False Negative (negative → neutral) | Check for hidden frustration patterns | Weighted keyword analysis |
| Escalation under-trigger | Verify urgency keywords separately | Multi-factor urgency detection |
| Over-escalation (routine → urgent) | Require multiple urgency flags | Threshold set at 3+ concurrent flags |

### Fallback Logic

```python
def route_with_fallback(sentiment_score: int, sentiment_category: str):
    """
    Apply fallback routing if primary classification is uncertain.
    """
    
    # If score is near boundary (±10 points), escalate to manual review
    if (40-10 <= sentiment_score <= 40+10) or \
       (80-10 <= sentiment_score <= 80+10) or \
       (10-5 <= sentiment_score <= 10+5):
        return {
            "routing": "MANUAL_REVIEW",
            "reason": "Score near decision boundary",
            "fallback_category": sentiment_category,
            "escalate_to": "quality_control_team"
        }
    
    # If escalation keywords present but score < 9, manually verify
    if has_escalation_keywords(text) and sentiment_score >= 5:
        return {
            "routing": "ESCALATION_PATH",  # Prioritize escalation keywords
            "reason": "Crisis keywords detected despite moderate score",
            "override": True
        }
    
    # Default fallback: Route to NEUTRAL if uncertain
    if sentiment_score == 50 or not sentiment_category:
        return {
            "routing": "STANDARD_PATH",
            "reason": "True neutral—no strong sentiment indicators",
            "fallback_to_agent": True
        }
    
    return route_normally(sentiment_score)
```

---

## Part 8: Implementation Checklist

- [ ] **Sentiment Analyzer**: Implement keyword-based + LLM-based scoring
- [ ] **Routing Engine**: Build decision tree with boundary logic
- [ ] **Response Templates**: Create tone-specific response templates for each path
- [ ] **Escalation Mechanism**: Set up human handoff with ticket system
- [ ] **Quality Monitoring**: Track misclassification rates monthly
- [ ] **Feedback Loop**: Allow agents to correct sentiment classifications
- [ ] **Testing**: Validate against 100+ sample conversations
- [ ] **Logging**: Record all sentiment scores and routing decisions for analysis
- [ ] **Fallback Handler**: Implement manual review for boundary cases

