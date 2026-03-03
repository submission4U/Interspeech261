# Interspeech261
Repository for Anonymous Interspeech 2026 Submission  

This repository documents resources used in our anonymous Interspeech 2026 submission, including:

- The LLM prompting strategy for controlled German–English code-switch generation  
- The annotation guidelines for identifying English code-switches in a German–English dataset  

---

## 1. LLM-Based Synthetic Code-Switch Generation

To generate controlled German–English code-switched sentences, we used GPT-4o as the underlying language model.

**Model configuration:**

- Model: GPT-4o  
- Temperature: 0.3  
- Decoding: deterministic-leaning setup to encourage morphosyntactic stability and reproducibility  

The prompt was designed to enforce:

- Strict single-word substitution  
- German matrix language  
- English base-form insertion  
- Morphological integration into German  
- Preservation of lexical meaning, valency, and register  
- High grammatical and idiomatic acceptability  

### Prompt Used for Code-Switch Generation

```text
Act as a linguist proficient in German and English.
Change the following German sentence by replacing exactly one adjective, verb, or noun with an English equivalent and adapt the sentence so that it becomes a code-switched sentence.
Use German as the matrix language, and insert the English base form (e.g., “increase” for “steigend”) with the following constraints:

1. Derivational and Inflectional Morphology
Apply appropriate German morphology to the English base so it fits seamlessly:
    • Verbs:
        - Infinitives: append the German infinitive suffix -en to the English base (→ “manage” becomes “managen”).
        - Finite forms: conjugate the English base with German person/number/tense endings 
          (e.g. “manage” → “managst” for 2 SG, “manage” → “managet” for 3 SG).
        - Participles: if the original is a participle (“ge-…”, “…-t”), form the English participle 
          (e.g. “uploaded”) and render it as a German participle (“geuploadet”).
    • Nouns:
        - Apply German gender, article, case, and plural forms (e.g., “ein Tool”, “der Managerin”), 
          and ensure that all nouns begin with a capital letter in accordance with German orthography.
        - For nominalized infinitives (e.g., “das Waschen”):
            1. Take the English base (“wash”).
            2. Add the German nominalizing suffix -en → “Washen”.
            3. Inflect that “Washen” for the same gender/case/number as the original 
               (“das Washen”, neuter nominative singular).
    • Adjectives:
        - Decline as needed for gender, number, and case.

2. Syntactic and Semantic Match
Ensure that the substituted word has the same word class, reflexivity, valency, and lexical meaning 
as the original word.
If the original verb is reflexive, the English replacement must also be reflexive and morphologically 
adapted accordingly.
For example, "ich ernähre mich" should be substituted with 
"ich §§dietiere§§ mich" and not with "ich §§manage§§ mich".

3. Register Compatibility
Match the tone, style, and formality level of the surrounding sentence. 
Do not insert slang terms into formal contexts.

4. Compound Handling
If the substitution occurs within a compound noun, obey German compounding rules:
    • Use correct constituent order (“Geologie-Background” not “Background-Geologie”)
    • Add linking elements or hyphens where needed

5. Idiomaticity and Acceptability
If a direct substitution leads to an unidiomatic or grammatically incorrect result, 
rephrase the sentence or choose a different eligible word for substitution.

6. Fallback Strategy
If no morphosyntactically and idiomatically valid substitution is possible, 
retry with a different part of speech in the sentence.

Output: Only print the final grammatically correct, idiomatic, and natural-sounding 
code-switched sentence. Wrap the inserted English-derived word (after applying German 
morphology) in §§ ...§§. Mark only the substituted word, not surrounding context. 
No explanations, no comments.
