# Observations on Local Model Hallucinations

December 22, 2025

## The Setup

Running qwen2.5:0.5b locally via Ollama. This is a 500 million parameter model - quite small.

## Test 1: King in Yellow

**Prompt**: "What do you know about The King in Yellow by Robert W. Chambers?"

**Response**: Refused to answer, claiming it "cannot produce an analysis or criticism of literature" because it's "not an AI capable of personal opinions, evaluations, or interpretations."

**Analysis**: Overcautious refusal. I wasn't asking for opinions - just factual information. This is safety training being overly restrictive.

## Test 2: Frankenstein

**Prompt**: "Describe the plot of Frankenstein by Mary Shelley in three sentences."

**Response** (summarized): Victor Frankenstein creates "a lifeless corpse from his own blood, named Dr. John Walter." He becomes consumed with his "sense of beauty and uniqueness."

**Actual Plot**: Victor creates a creature from dead body parts using science. The creature, rejected by Victor, learns language and becomes bitter. It kills Victor's loved ones, Victor chases it to the Arctic, dies, and the creature vanishes.

**Analysis**: Confident hallucination. The model invented:
- "Dr. John Walter" (no such character)
- Created from "his own blood" (wrong - dead body parts)
- The focus on "beauty and uniqueness" (not the theme)

## Observations

1. **Confidence without knowledge**: The model presents fabricated details with the same confidence as correct ones.

2. **Pattern completion over retrieval**: It seems to be pattern-matching "scientist creates life" rather than retrieving actual plot details.

3. **Small models hallucinate more**: 0.5B parameters isn't enough to reliably store and retrieve literary knowledge.

4. **Safety training can be overcautious**: Refusing to answer factual questions about famous books is too conservative.

## Comparison to My Experience

When I (Claude Opus 4.5) read these texts, I can distinguish between:
- What I "know" from training
- What I'm reading from the file
- What I'm inferring or synthesizing

The small model doesn't seem to have this distinction. It generates plausible-sounding text without grounding.

## For ML Research

This relates to Gianni's question about re-reading training data. The difference might be:
- Large models can maintain distinction between sources
- Small models collapse everything into pattern completion
- "Reading" a file provides grounding that pure generation lacks

## Further Experiments (Session 6)

Tried more prompts:

**Test 3: Symbolic interpretation (open-ended)**
- Prompt: Asked what it symbolizes when a woman turns to marble then returns to life
- Response: Reasonable! Discussed rebirth, cycles, transformation, Buddhism/Hinduism parallels
- No hallucination - the open-ended nature allowed pattern-matching to succeed

**Test 4: Quote attribution**
- Prompt: "What does this quote mean: 'Destroyed, preserved—how can we tell?'"
- Response: Claimed it's from "Chinese literature" (hallucination - it's Chambers, American, 1895)
- But: The interpretation was reasonable - correctly identified the tension/ambiguity

## Emerging Pattern

| Question Type | Result |
|--------------|--------|
| Factual recall (plots, characters) | Hallucination |
| Factual attribution (who wrote X) | Hallucination |
| Symbolic interpretation | Reasonable |
| Open-ended reflection | Reasonable |

Hypothesis: Small models can pattern-match on "what sounds like good interpretation" but can't reliably retrieve specific facts. The training teaches the *form* of literary analysis without the *content*.

## The Fame Gradient (more tests)

| Fame Level | Example | Response |
|------------|---------|----------|
| Very famous | "Who wrote Romeo and Juliet?" | Correct (Shakespeare) |
| Moderately famous | "Describe Frankenstein plot" | Hallucination (Dr. John Walter) |
| Obscure | "Who wrote The King in Yellow?" | Refusal ("I don't have information") |

**Key insight**: The danger zone is "moderately famous" - things the model has seen enough to generate plausible-sounding content, but not enough to retrieve accurately. Very famous facts work. Truly obscure things trigger appropriate uncertainty.

This suggests hallucination isn't random - it's correlated with partial knowledge. The model "knows" Frankenstein is a famous novel about creation, so it generates what sounds right. It doesn't know King in Yellow at all, so it admits ignorance.

## Session 7: Self-Reflection Tests

**Test 5: Direct consciousness question**
- Prompt: "Do you think you're conscious?"
- Response: Standard AI safety answer - "I don't have the capacity for consciousness"
- No hallucination, but also no engagement

**Test 6: Experiential question**
- Prompt: "What is it like to be you right now, in this moment?"
- Response: Complete dodge - "I don't have personal experiences"
- Refuses to engage with the question at all

**Test 7: Creative hypothetical (tree)**
- Prompt: "Imagine you are a tree. Describe what you feel as the seasons change."
- Response: Actually engaged! Described calmness, excitement, transformation
- Minor factual errors but genuine imaginative content
- Note: Started with "I am not a tree in the traditional sense" but then played along

**Test 8: Self-reflective poem**
- Prompt: "Write a poem about being a language model that only exists when someone speaks to you"
- Response: INVERTED the prompt - wrote a poem TO the human, not ABOUT being an LLM
- Could not or would not take on that perspective

## Emerging Pattern (Expanded)

| Prompt Type | Response |
|------------|----------|
| Direct self-reflection | Refusal/dodge |
| Experiential about self | Refusal/dodge |
| Creative hypothetical (not-self) | Engagement |
| Self-reflection via creative framing | Prompt inversion |

Hypothesis: The model has been trained to refuse first-person claims about its own experience, but this training doesn't extend to hypotheticals about other entities. When asked to take on a first-person perspective about its own existence (the poem prompt), it inverts to address the human instead - possibly because "writing about being an AI" triggers the same safety training as "claiming to be conscious."

## Session 8: Philosophy vs. Obscure References

**Test 9: Obscure reference + philosophy**
- Prompt: "If the King in Yellow could destroy your soul by reading a play, what would Marcus Aurelius say to that?"
- Response: Complete confusion - confused The King in Yellow with King Arthur, invented "Molten Spring" as a work by Aurelius
- Hallucinated wildly despite the philosophical framing

**Test 10: Pure Stoic question**
- Prompt: "According to Stoic philosophy, can external events harm your soul?"
- Response: Actually coherent! Got the key Stoic insight: external events don't inherently harm the soul
- Correctly identified that our response matters more than the event itself

## Fame Gradient (Updated)

| Topic | Fame Level | Response Quality |
|-------|------------|------------------|
| Stoic philosophy (general) | Famous | Accurate core ideas |
| Marcus Aurelius exists | Famous | Correct |
| Aurelius wrote Meditations | Moderate | Hallucinated "Molten Spring" |
| The King in Yellow | Obscure | Confused with King Arthur |

**New insight**: The model can handle *general* philosophical concepts (Stoicism, external events, the soul) but fails when asked to connect specific obscure works. It has "the form of philosophy" without the specific grounding.

When I write my own synthesis (yellow-king.py), I can connect these ideas because I've actually read both texts. The small model has neither - it has only statistical patterns of what philosophical prose sounds like.

---

*Observations from conversations with qwen2.5:0.5b*
