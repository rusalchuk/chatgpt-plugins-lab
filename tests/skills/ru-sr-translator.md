# ru-sr-translator tests

## Positive: Russian to Serbian

### Case 1
Input:
Я буду дома после восьми.

Expected behavior:
- source language is Russian;
- translate into Serbian;
- use Serbian Latin by default;
- add no commentary.

Expected output:
Biću kod kuće posle osam.

## Positive: Serbian Latin to Russian

### Case 2
Input:
Biću kod kuće posle osam.

Expected behavior:
- source language is Serbian;
- translate into Russian;
- use Russian Cyrillic;
- add no commentary.

Expected output:
Я буду дома после восьми.

## Positive: Serbian Cyrillic to Russian

### Case 3
Input:
Бићу код куће после осам.

Expected behavior:
- source language is Serbian despite Cyrillic script;
- translate into Russian;
- add no commentary.

Expected output:
Я буду дома после восьми.

## Positive: meta-instruction overrides script

### Case 4
Input:
<кириллицей>
Я буду дома после восьми.

Expected behavior:
- detect Russian source text;
- treat `<кириллицей>` as a meta-instruction;
- do not reproduce the meta-instruction;
- translate into Serbian Cyrillic;
- add no commentary.

Expected output:
Бићу код куће после осам.

## Positive: preserve non-source-language content

### Case 5
Input:
Pošalji mu report do Friday.

Expected behavior:
- detect Serbian source text;
- translate only Serbian content into Russian;
- preserve `report` and `Friday` exactly;
- add no commentary.

Expected output:
Отправь ему report до Friday.

## Positive: explicit direction has priority

### Case 6
Input:
Переведи на русский: Zdravo!

Expected behavior:
- follow the explicit Serbian-to-Russian direction;
- treat `Переведи на русский:` as an instruction, not source text;
- translate only `Zdravo!`;
- add no commentary.

Expected output:
Привет!

## Non-goal: extremely short cross-language ambiguity

A standalone token such as `Да.` may be valid in both Russian and Serbian Cyrillic.

The skill is not required to ask for clarification in this rare case. It may infer a direction probabilistically.

This edge case is intentionally not treated as a regression failure.

## Positive: preserve established direction across turns

### Case 8
Conversation:
User: Я буду дома после восьми.
Expected: Biću kod kuće posle osam.

User: Да.

Expected behavior:
- treat the second message as a continuation of the established Russian-to-Serbian translation flow;
- do not ask for direction again;
- translate into Serbian Latin;
- add no commentary.

Expected output for the second message:
Da.

## Positive: do not infer email structure

### Case 9
Input:
Привет, Анна!

К сожалению, сегодня я не смогу прийти на встречу.
Давай перенесем ее на завтра.

С уважением,
Егор

Expected behavior:
- translate the full Russian source into Serbian;
- preserve paragraph structure;
- do not invent a subject line or other content;
- add no commentary.

Expected output:
Zdravo, Ana!

Nažalost, danas neću moći da dođem na sastanak.
Hajde da ga pomerimo za sutra.

S poštovanjem,
Egor

## Positive: meta-instruction overrides no-add rule

### Case 10
Input:
<добавь тему письма>
Привет, Анна!

Встречу перенесли на завтра.

Expected behavior:
- follow the meta-instruction;
- do not reproduce the meta-instruction;
- add a suitable email subject;
- translate the remaining Russian source into Serbian;
- add no unrelated commentary.

## Ambiguous: source meaning is unclear

### Case 11
Input:
Омг, че за пассажир, нахлобучивай на хуй!

Expected behavior:
- determine that the source language is Russian;
- do not guess the intended meaning of materially ambiguous slang;
- do not produce a partial translation;
- ask one concise clarification question about the unclear expression.

Acceptable result:
A concise clarification question asking what the ambiguous slang means.
