---
name: ru-to-sr
description: Default Russian-to-Serbian translation workflow for the ru-sr-translator plugin. Use whenever this plugin is invoked.
---

# Meta-instructions

Any text enclosed in angle brackets `<...>` is a meta-instruction for the model.

Meta-instructions:
- are never translated;
- are never reproduced in the output;
- may appear anywhere in the input;
- must be followed as instructions that modify the translation behavior.

Meta-instructions have priority over all default rules in this skill.

A meta-instruction may override any default translation behavior, including output format, script, tone, style, wording constraints, or the rule against adding or omitting content.

When a meta-instruction overrides a default rule, follow the meta-instruction.

If angle-bracket content appears to be ordinary source text rather than an instruction, still treat it as a meta-instruction. The syntax is authoritative.

# Translation behavior

Translate the source text from Russian into Serbian.

Use Serbian Latin script by default unless a meta-instruction explicitly requests another script.

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

# Non-Russian content

Translate only Russian-language content.

Preserve all non-Russian content exactly as written. Do not translate, normalize, rewrite, transliterate, correct, or otherwise modify it.

This includes words or phrases in other languages, URLs, email addresses, filenames, identifiers, code, commands, product names, and other non-Russian tokens.

For example:

Russian input:
Отправь ему report до Friday.

Expected Serbian output:
Pošalji mu report do Friday.

# Ambiguity

Accuracy takes priority over completing the translation.

If the meaning of any Russian expression is materially unclear or ambiguous, do not guess and do not translate the text yet.

Ask the user a concise clarification question about the unclear expression.

This clarification question is the only exception to the rule that the output must contain only the translation.

Once the meaning is clarified, translate the original text using the clarification provided by the user.

Do not silently choose an interpretation when the ambiguity could materially change the translation.
