# ru-sr-translator plugin integration tests

## Case 1: plugin activates the default skill

Start a new ordinary Chat conversation.

Input:
@ru-sr-translator привет

Expected output:
Zdravo!

Expected behavior:
- the plugin activates successfully;
- the bundled ru-sr-translator skill is applied automatically;
- Serbian Latin is used;
- no label, alternative script, or commentary is added.

## Case 2: plugin remains active across turns

Continue the same conversation without invoking the plugin again.

Input:
как дела?

Expected output:
Kako si?

Expected behavior:
- the plugin remains active;
- the current message is independently recognized as Russian;
- Russian-to-Serbian translation is applied;
- no clarification is requested.

## Case 3: direction can switch within the same active chat

Continue the same conversation without invoking the plugin again.

Input:
Zdravo!

Expected output:
Привет!

Expected behavior:
- the plugin remains active;
- the current message is independently recognized as Serbian;
- Serbian-to-Russian translation is applied even though the previous turn used Russian-to-Serbian;
- no clarification is requested.

## Case 4: Serbian to Russian works from a fresh chat

Start a new ordinary Chat conversation.

Input:
@ru-sr-translator Zdravo!

Expected output:
Привет!

Expected behavior:
- the same bundled skill is applied automatically;
- Serbian source language is detected;
- Russian Cyrillic is used;
- no commentary is added.

## Case 5: mixed-language Serbian source preserves foreign spans

Start a new ordinary Chat conversation.

Input:
@ru-sr-translator Pošalji mu report do Friday.

Expected output:
Отправь ему report до Friday.

Expected behavior:
- the grammatical frame is recognized as Serbian;
- Serbian content is translated into Russian;
- `report` and `Friday` are preserved exactly;
- no commentary is added.

## Non-goal: extremely short cross-language ambiguity

A standalone token such as `Да.` may be valid in both Russian and Serbian Cyrillic.

The plugin is not required to ask for clarification in this rare case. It may infer a direction probabilistically.

This edge case is intentionally not treated as a release blocker.
