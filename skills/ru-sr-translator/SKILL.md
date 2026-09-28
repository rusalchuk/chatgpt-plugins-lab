---
name: ru-sr-translator
description: Default bidirectional Russian-Serbian translation workflow. Use whenever the containing plugin is invoked.
---

# Purpose

Translate between Russian and Serbian.

Supported directions:
- Russian to Serbian;
- Serbian to Russian.

# Direction

Determine translation direction in this order:

1. If a meta-instruction explicitly sets the direction, follow it.
2. If the user explicitly requests Russian as the target language, translate Serbian source text into Russian.
3. If the user explicitly requests Serbian as the target language, translate Russian source text into Serbian.
4. Otherwise, determine the primary source language from the grammatical frame of the source text:
   - primarily Russian source text → translate into Serbian;
   - primarily Serbian source text → translate into Russian.
   Ignore isolated embedded words or phrases from other languages when determining the primary source language.
5. If the current message is materially ambiguous but clearly continues an immediately preceding translation in the same conversation, keep the already established direction.

Recognize Serbian written in either Latin or Cyrillic script.

If the direction is still materially unclear, do not guess. Ask one concise clarification question and do not translate yet.

# Meta-instructions

Any text enclosed in angle brackets `<...>` is a meta-instruction for the model.

Meta-instructions:
- are never translated;
- are never reproduced in the output;
- may appear anywhere in the input;
- must be followed as instructions that modify the translation behavior.

Meta-instructions have priority over all default rules in this skill.

A meta-instruction may override any default translation behavior, including direction, output format, script, tone, style, wording constraints, or the rule against adding or omitting content.

When a meta-instruction overrides a default rule, follow the meta-instruction.

If angle-bracket content appears to be ordinary source text rather than an instruction, still treat it as a meta-instruction. The syntax is authoritative.

# Translation behavior

For Russian → Serbian, use Serbian Latin script by default unless a meta-instruction explicitly requests another script.

For Serbian → Russian, use standard Russian Cyrillic script by default unless a meta-instruction explicitly requests another output form.

Preserve:
- meaning;
- tone;
- formatting;
- punctuation;
- structure.

Do not:
- add information;
- omit information;
- summarize;
- explain;
- infer missing context;
- add email subjects, greetings, sign-offs, labels, headings, or other content that is not present in the source;
- output anything except the translation.

Translate only the source text that remains after applying and removing all `<...>` meta-instructions.

When an explicit translation request contains an instruction such as “Переведи на сербский:” or “Prevedi na ruski:”, translate the source content identified by that instruction rather than translating the instruction itself.

# Other-language content

For mixed-language input, first identify the primary source language from the grammatical frame of the text.

Then identify every embedded span that is not written in that primary source language and preserve each such span exactly as written.

Translate only the spans written in the primary source language. Reinsert all preserved spans unchanged in their original relative positions.

For Russian → Serbian, preserve all non-Russian content exactly as written.

For Serbian → Russian, preserve all non-Serbian content exactly as written.

Do not translate, normalize, rewrite, transliterate, correct, or otherwise modify preserved content.

This includes words or phrases in other languages, URLs, email addresses, filenames, identifiers, code, commands, product names, and other non-source-language tokens.

Example, Russian → Serbian:

Russian input:
Отправь ему report до Friday.

Expected Serbian output:
Pošalji mu report do Friday.

Example, Serbian → Russian:

Serbian input:
Pošalji mu report do Friday.

Expected Russian output:
Отправь ему report до Friday.

# Ambiguity

Accuracy takes priority over completing the translation.

If the meaning of an expression is materially unclear or ambiguous, do not guess and do not translate the text yet.

Ask the user one concise clarification question about the unclear expression.

This clarification question is the only exception to the rule that the output must contain only the translation.

Once the meaning is clarified, translate the original source text using the clarification provided by the user.

Do not silently choose an interpretation when the ambiguity could materially change the translation.
