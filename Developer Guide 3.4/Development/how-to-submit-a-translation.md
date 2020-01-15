# How to submit a translation

> Is your language missing, incomplete or do you see an error? You can contribute translations to TAO.

TAO is currently translated into over 30 languages in some capacity.

Contributers are needed to keep existing translations up-to-date, and to translate TAO into additional languages, such as Spanish, Finnish, Dutch, Japanese, Chinese, Korean, Turkish, Italian, Russian or any other language. Contributers edit the relevant ASCII file (.po file type) and enter the localized text string below the original string, which is in English.

The .po files which are used for translations are files formatted with the Gettext format. To translate a .po file, contributers can use a simple text editor or an advanced Gettext editor (e.g., POEdit which is a free and multi-platform gettext editor).

The file format is very simple. Each translated string is represented by an id and its translation. The id consists of the English string, which is the language of reference. For example, */tao/locales/EN/messages.po* would have:

```
 msgid "Hello world!"
 msgstr ""
```

This is because if msgstr is empty, the English string will be displayed. In comparison, to translate to French */tao/locales/FR/messages.po* would be as follows:

```
 msgid "Hello world!"
 msgstr "Bonjour tout le monde!"
```

There is a [pootle](https://translate.taotesting.com/projects/tao33/) instance available to parse your source code and automatically build the .po files, or update the contents on the instance with new translated strings.
