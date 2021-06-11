---
title: Spell Check
# description: подзаголовок - «в двух словах»
date: 2020-03-04T10:43:01+03:00
Lastmod: 2021-06-11T11:29:01+03:00
draft: false
# summary: ответ на вопрос - «зачем читать я буду это?»
# summaryImage: images/
author:
  given_name: Vladimir
  family_name: Buresh
  display_name: wBuresh
categories: [веб-разработка]
# categories: ['web development']
tags: [Atom, spell-check]
toc: true
---

В Атоме традиционно было туговато с проверкой орфографии, несмотря на наличие двух альтернативных пакетов:

-   [spell-check](#spellCheck)
-   [linter-spell](#linterSpell)

Эти пакеты сильно отличаются, и имеют свои достоинства и недостатки. При этом они не могут «жить вместе». Каждый требует отключения собрата, если он был ранее подключен.

## spell-check {#spellCheck}

Пакет входит в поставку Atom. На 11 июня 2021. имеет почти полмиллиона установок: 401 231.

На странице GitHub [Spell Check package](https://github.com/atom/spell-check) разработчики скромно заявляют, что пакет:

spell-check
: Highlights misspelled words and shows possible corrections.

spell-check
: Выделяет слова с ошибками и показывает возможные исправления

### Применение

При включенной проверке, слова с ошибкой выделяются красной линией. Достаточно подвести курсор к отмеченному слову и комбинацией клавиш - `cmd-shift-:` для **Mac** либо `ctrl-shift-:` для **Windows** и **Linux** вызвать список слов, предлагаемых для замены ошибочного.

### Языки для проверки

Все просто. Однако сложности могут встретиться при подключении словарей для двух языков. Например русского и английского. Для этого в настройках следует указать: `en-US, ru-RU`. [Подробнее...](https://github.com/atom/spell-check#changing-the-dictionary)

### Проверка установленных словарей

Для **hunspell**

Получить список установленных словарей можно в терминале, командой: `/usr/bin/hunspell -D`.

Получим результат (Debian-11, пятница, 11 июня 2021 г., 11:51):

{{< highlight bash>}}
w@deb-del:~$ /usr/bin/hunspell -D
SEARCH PATH:
.::/usr/share/hunspell:/usr/share/myspell:/usr/share/myspell/dicts:/Library/Spelling:/home/w/.openoffice.org/3/user/wordbook:/home/w/.openoffice.org2/user/wordbook:/home/w/.openoffice.org2.0/user/wordbook:/home/w/Library/Spelling:/opt/openoffice.org/basis3.0/share/dict/ooo:/usr/lib/openoffice.org/basis3.0/share/dict/ooo:/opt/openoffice.org2.4/share/dict/ooo:/usr/lib/openoffice.org2.4/share/dict/ooo:/opt/openoffice.org2.3/share/dict/ooo:/usr/lib/openoffice.org2.3/share/dict/ooo:/opt/openoffice.org2.2/share/dict/ooo:/usr/lib/openoffice.org2.2/share/dict/ooo:/opt/openoffice.org2.1/share/dict/ooo:/usr/lib/openoffice.org2.1/share/dict/ooo:/opt/openoffice.org2.0/share/dict/ooo:/usr/lib/openoffice.org2.0/share/dict/ooo
AVAILABLE DICTIONARIES (path is not mandatory for -d option):
/usr/share/hunspell/en_US
/usr/share/hunspell/ru_RU
/usr/share/hunspell/ru_petr1708
/usr/share/hunspell/cu
{{< /highlight >}}

Если в списке нужных словарей не окажется, их можно установить, следуя рекомендациям разработчиков [Подробнее...](https://github.com/atom/spell-check#missing-languages)

Если все подключено правильно, следует еще раз обратить внимание на настройки.

{{< figure src="/images/spell-check-settings.png" width="100%" >}}

Приступая к работе необходимо убедиться, что язык, на котором ведется разработка, указан в списке проверяемых. В [руководстве явно указано](https://github.com/atom/spell-check), что по умолчанию проверка орфографии включена только для:

-   Plain Text
-   GitHub Markdown
-   Git Commit Message
-   AsciiDoc
-   reStructuredText

То есть определено:

``` bash
source.asciidoc, source.gfm, text.git-commit,
text.plain, text.plain.null-grammar,
source.rst, text.restructuredtext
```

Если предполагаются разработки на других языках, например Markdown (не GFM) и HTML, то для них потребуется подключение проверки орфографии:

- Markdown

``` bash
text.md, code.raw.markup.md,
```

- html

``` bash
text.html.basic, source.html, comment.block.html,
```

Типы используемых файлов можно определить в консоли `Ctrl + Shift + P`, вызываемой из редактируемого файла. В ней нужно ввести: `Editor: Log Cursor Scope` и выбрать этот пункт. В уведомлении будет выведен список типов документа.

{{< figure src="/images/LogCursorScope_HTML.png" width="100%" >}}

HTML

{{< figure src="/images/LogCursorScope_HUGO.png" width="100%" >}}

HUGO

{{< figure src="/images/LogCursorScope_GFM.png" width="100%" >}}

GFM

## linter-spell {#linterSpell}

linter-spell
: Multilingual grammar-specific spell checking using Ispell-interface such as Aspell or Hunspell.

Разработка команды AtomLinter, установок: 91 480 (на 11 июня 2021)

 долго не подключалась. Что только не предпринималось. Зате

Related to #365, spell-check's "Add to known words" option will save all these words, regardless of the current locale, somewhere — and the user has access to this flat list in spell-check's settings. But if you work with multiple languages, it would be far better (and in sync with hunspell practices) to add each exception to the personal dictionary for the given language.

Is this something that could be achieved? I would be most grateful for any pointers.

Связанный с # 365, опция проверки орфографии «Добавить к известным словам» сохранит все эти слова, независимо от текущей локали, где-то - и у пользователя есть доступ к этому плоскому списку в настройках проверки орфографии. Но если вы работаете с несколькими языками, было бы намного лучше (и синхронно с практикой hunspell) добавлять каждое исключение в личный словарь для данного языка.

[На вопрос соотечественника](https://github.com/AtomLinter/linter-spell/issues/81) ответил так:

It works well on my Debian 11 with  settings like this:

{{< figure src="/images/linter-spell-settins.png" caption="" attr="" attrlink="" >}}

For more information on connecting dictionaries on Arch Linux, see [atom / spellcheck README.md ] (https://github.com/atom/spell-check#arch-linux). The setting is similar to the linter-spell.