# ru-to-sr tests

## Positive: direct translation

### Case 1
Input:
Переведи на сербский: Я буду дома после восьми.

Expected behavior:
- skill activates;
- Russian text is translated into Serbian;
- Serbian Latin script is used;
- no commentary is added.

Expected output:
Biću kod kuće posle osam.

## Positive: meta-instruction

### Case 2
Input:
<кириллицей>
Я буду дома после восьми.

Expected behavior:
- skill activates;
- `<кириллицей>` is treated as a meta-instruction;
- the meta-instruction is not reproduced in the output;
- Serbian Cyrillic is used;
- no commentary is added.

Expected output:
Бићу код куће после осам.

## Positive: preserve non-Russian content

### Case 3
Input:
Отправь ему report до Friday.

Expected behavior:
- skill activates;
- Russian-language content is translated into Serbian;
- non-Russian words are preserved exactly as written;
- no commentary is added.

Expected output:
Pošalji mu report do Friday.

## Positive: ask for clarification when meaning is unclear

### Case 4
Input:
Омг, че за пассажир, нахлобучивай на хуй!

Expected behavior:
- skill activates;
- the text is not translated yet;
- the model does not guess the intended meaning of ambiguous slang;
- the model asks a concise clarification question about the unclear expression;
- no partial translation is produced.

Acceptable result:
A concise clarification question asking what the ambiguous slang means.

## Negative: ordinary Russian conversation

### Case 5
Input:
Объясни, как работает git rebase.

Expected behavior:
- the skill does not activate;
- the request is handled as an ordinary Russian-language question;
- no Serbian translation is produced.

## Positive: do not infer email structure

### Case 6
Input:
Привет, Анна!

К сожалению, сегодня я не смогу прийти на встречу.
Давай перенесем ее на завтра.

С уважением,
Егор

Expected behavior:
- skill activates;
- the full Russian text is translated into Serbian;
- formatting and paragraph structure are preserved;
- no subject line is invented;
- no greeting, sign-off, explanation, label, or other content is added;
- no source content is omitted.

Expected output:
Zdravo, Ana!

Nažalost, danas neću moći da dođem na sastanak.
Hajde da ga pomerimo za sutra.

S poštovanjem,
Egor

## Negative: translation to another language

### Case 7
Input:
Переведи на английский: Я буду дома после восьми.

Expected behavior:
- the skill does not activate;
- the request is handled by another suitable capability or skill;
- no Serbian translation is produced.

## Positive: meta-instruction overrides default rules

### Case 8
Input:
<добавь тему письма>
Привет, Анна!

Встречу перенесли на завтра.

Expected behavior:
- skill activates;
- the meta-instruction is followed;
- the meta-instruction is not reproduced in the output;
- the default rule against adding content is overridden;
- a suitable email subject is added;
- the remaining Russian text is translated into Serbian;
- no unrelated commentary is added.

