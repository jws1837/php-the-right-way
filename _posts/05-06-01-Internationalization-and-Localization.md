---
title:   국제화와 지역화
isChild: true
anchor:  i18n_l10n
---

## 국제화 Internationalization(i18n)와 지역화 Localization(l10n) {#i18n_l10n_title}

_초심자분들을 위한 안내: i18n과 l10n은 긴 단어를 줄이기 위해서 숫자를 사용한 줄임말(numeronym)입니다. 
internationalization은 i18n(i 와 n 사이에 18글자가 있음)으로 줄이고, localization 은 l10n 으로 줄이는 식입니다._

일단 비슷해보이는 것 같은 두 가지 개념과 그에 관련된 다른 개념을 정의부터 할 필요가 있습니다.

- **국제화** 란 여러분이 작성한 코드가 다른 언어나 지역에서 사용될 때 리팩터링 없이도 사용이 가능하도록 만든다는 의미입니다.
이 작업은 보통 프로젝트 초기에 하는 것이 바람직합니다. 그렇지 않으면 아마 소스를 크게 수정해야 할 지도 모릅니다!
- **지역화 혹은 현지화** 는 이미 i18n 작업이 되어 있는 것에 기반하여, (주로) 콘텐츠를 다른 언어로 번역할 때 일어납니다.
보통 새로운 언어나 지역에 제품을 제공하려고 할 때마다 지역화 작업을 하게 됩니다. 그리고 새로 추가된 사용자 인터페이스가 생기면
지원하는 모든 언어로 제공이 되어야 할 것이므로 그 때에도 지역화 작업이 있을 것입니다.
- **복수화(Pluralization)** 란 숫자나 개수를 포함하는 문자열을 각 언어마다 어떻게 표현할 지 규칙을 정하는 것입니다.
예를 들면 영어에서는 사물이 하나만 있으면 단수형, 그 외에는 복수형으로 지칭합니다.
영어의 복수형은 단어 뒤에 S 를 붙이는 형태이지만, 때로는 단어의 일부를 변형해서 나타내기도 합니다.
러시아어나 세르비아어에서는 단수형 외에 두 가지로 복수형이 있습니다.
총 네 가지나 대여섯 가지 형태를 사용하는 슬로베니아어,아일랜드어, 아라비아어도 존재합니다.

## 일반적인 구현 방법
PHP로 만든 소프트웨어를 국제화하는 가장 쉬운 방법은 배열 파일을 만든 후 그걸 템플릿 파일에서 
`<h1><?=$TRANS['title_about_page']?></h1>` 같은 식으로 사용하는 것입니다. 하지만 이런 방법은 복수화 작업 같이 프로젝트 아주 초반에도
발견될 수 있는 문제 등 유지관리에 계속 문제를 일으킬 것이므로 제대로된 프로젝트에서는 권할 수 없는 방법입니다.
그러므로 페이지 몇 개만으로 구성된 프로젝트가 아니라면 이런 방법은 시도조차 하지 않는 것이 좋습니다.

가장 전형적이면서 또 자주 i18n과 l10n의 레퍼런스로 언급되는 방법은 [`gettext` 라는 유닉스 도구입니다][gettext]. 1995년에
등장하여 지금까지도 여전히 번역 소프트웨어로서 완전한 구현을 보여줍니다. 바로 사용할 수 있을만큼 쉬우면서도
강력한 도구를 제공합니다. 여기서 Gettext 에 대해 이야기하겠습니다. 그리고 커맨드라인 도구로 머리가
복잡해 지지 않도록 훌륭한 GUI 어플리케이션을 보여드릴 겁니다. 이 어플리케이션으로 쉽게 l10n 소스를 업데이트할 수 있을 것입니다.

### 다른 도구들

Gettext 를 보조하는 방식이나, 직접 i18n 을 구현한 다른 라이브러리들이 있습니다. 이들 중 몇몇은 설치하기에 더 쉽거나
다른 기능을 더 제공할 것입니다. 이 문서에서는 PHP 코어에서 제공되는 툴들에 집중하겠지만
다른 라이브러리도 나열해보았습니다.

- [aura/intl][aura-intl]: I18N 도구를 제공하는데, 특히 패키지 지향적인 
지역별 메시지 번역에 집중되어 있습니다. 메시지는 배열 형식으로 관리합니다. 메시지 추출기는 제공하지 않지만
`intl` 확장을 이용해서 (복수형 메시지를 포함한) 고급 메시지 포매팅 기능을 제공합니다.
- [oscarotero/Gettext][oscarotero]: Gettext 를 객체지향 인터페이스로 지원합니다. 향상된 헬퍼 함수들, 
`gettext` 커맨드가 자체적으로는 지원하지 못하는 것을 포함한 여러가지 파일 포맷을 지원하는 강력한 메시지 추출기를 가지고 있습니다.
그리고 `.mo/.po` 같은 다른 파일 포맷으로 내보내기도 할 수 있습니다.
- [symfony/translation][symfony]: 많은 종류의 파일 포맷을 지원하지만 조금 장황한 XLIFF 포맷을 권장합니다.
헬퍼 함수나 내장 메시지 추출기 등을 포함하고 있지는 않지만 `strtr()`을 이용한 플레이스홀더를 지원합니다.
- [zend/i18n][zend]: 배열과 INI 파일, Gettext 파일 포맷을 지원합니다. 매번 파일시스템에서 읽어들이지 않아도 되도록
캐싱 레이어를 구현했습니다. 그리고 뷰 헬퍼, 로케일을 자동 인식해서 처리하는 입력값 필터와 검증기도 제공합니다.
하지만 메시지 추출기는 제공하지 않습니다.

다른 프레임워크들도 i18n 모듈을 가지고 있지만 해당 프레임워크 밖에서는 사용할 수 없습니다.

- [Laravel] 은 기본적인 배열 파일 형식을 지원합니다. 자동 추출기는 없지만 템플릿 파일에서 사용할 수 있는 `@lang` 헬퍼 함수가 있습니다.
- [Yii] 는 배열, Gettext, 데이터베이스를 사용한 번역 방식을 제공하며 메시지 추출기도 포함하고 있습니다.
[`Intl`][intl]이라는 확장을 이용해서 이런 기능을 제공하는데, 이 확장은 [ICU project]를 기반으로 하고 있고 PHP 5.3 부터 사용가능합니다.
그래서 Yii 에서는 숫자, 날짜, 시간, 시간 간격(intervals), 통화, 서수(ordinals) 의 포미팅 등 강력한 치환 기능을 사용할 수 있습니다.

추출기를 제공하지 않는 라이브러리를 사용하기로 결정했다면, gettext 포맷을 사용하는 것이 좋을 것 같습니다.
그러면 이 장의 나머지 부분에서 설명하는대로 Poedit를 포함한 gettext의 여러 도구들을 사용할 수 있습니다.

## Gettext

### 설치
여러분이 사용하고 있는 시스템에 따라, `apt-get` 이나 `yum` 같은 패키지 매니저를 사용해서 Gettext와 PHP 라이브러리를 설치하면 됩니다.
설치 후에는 `extension=gettext.so` (Linux/Unix) 나 `extension=php_gettext.dll` (Windows) 를 `php.ini` 파일에 추가합니다.

번역 파일을 만들 때에는 [Poedit]을 사용하려고 합니다. 사용하는 시스템의 패키지 매니저에서
Poedit을 찾을 수 있을 겁니다. [Poedit 웹 사이트에서 무료로 다운로드][poedit_download]해서 사용할 수도 있습니다.

### Structure

#### Types of files
There are three files you usually deal with while working with gettext. The main ones are PO (Portable Object) and
MO (Machine Object) files, the first being a list of readable "translated objects" and the second, the corresponding
binary to be interpreted by gettext when doing localization. There's also a POT (Template) file, which simply contains
all existing keys from your source files, and can be used as a guide to generate and update all PO files. Those template
files are not mandatory: depending on the tool you are using to do l10n, you can go just fine with only PO/MO files.
You will always have one pair of PO/MO files per language and region, but only one POT per domain.

### Domains
There are some cases, in big projects, where you might need to separate translations when the same words convey 
different meaning given a context. In those cases, you split them into different _domains_. They are, basically, named
groups of POT/PO/MO files, where the filename is the said _translation domain_. Small and medium-sized projects usually,
for simplicity, use only one domain; its name is arbitrary, but we will be using "main" for our code samples.
In [Symfony] projects, for example, domains are used to separate the translation for validation messages.

#### Locale code
A locale is simply a code that identifies one version of a language. It is defined following the [ISO 639-1][639-1] and 
[ISO 3166-1 alpha-2][3166-1] specs: two lower-case letters for the language, optionally followed by an underline and two
upper-case letters identifying the country or regional code. For [rare languages][rare], three letters are used.

For some speakers, the country part may seem redundant. In fact, some languages have dialects in different
countries, such as Austrian German (`de_AT`) or Brazilian Portuguese (`pt_BR`). The second part is used to distinguish
between those dialects - when it is not present, it is taken as a "generic" or "hybrid" version of the language.

### Directory structure
To use Gettext, we will need to adhere to a specific structure of folders. First, you will need to select an arbitrary
root for your l10n files in your source repository. Inside it, you will have a folder for each needed locale, and a
fixed `LC_MESSAGES` folder that will contain all your PO/MO pairs. Example:

{% highlight console %}
<project root>
 ├─ src/
 ├─ templates/
 └─ locales/
    ├─ forum.pot
    ├─ site.pot
    ├─ de/
    │  └─ LC_MESSAGES/
    │     ├─ forum.mo
    │     ├─ forum.po
    │     ├─ site.mo
    │     └─ site.po
    ├─ es_ES/
    │  └─ LC_MESSAGES/
    │     └─ ...
    ├─ fr/
    │  └─ ...
    ├─ pt_BR/
    │  └─ ...
    └─ pt_PT/
       └─ ...
{% endhighlight %}

### Plural forms
As we said in the introduction, different languages might sport different plural rules. However, gettext saves us from
this trouble once again. When creating a new `.po` file, you will have to declare the [plural rules][plural] for that
language, and translated pieces that are plural-sensitive will have a different form for each of those rules. When
calling Gettext in code, you will have to specify the number related to the sentence, and it will work out the correct
form to use - even using string substitution if needed.

Plural rules include the number of plurals available and a boolean test with `n` that would define in which rule the
given number falls (starting the count with 0). For example:

- Japanese: `nplurals=1; plural=0` - only one rule
- English: `nplurals=2; plural=(n != 1);` - two rules, first if N is one, second rule otherwise
- Brazilian Portuguese: `nplurals=2; plural=(n > 1);` - two rules, second if N is bigger than one, first otherwise

Now that you understood the basis of how plural rules works - and if you didn't, please look at a deeper explanation
on the [LingoHub tutorial][lingohub_plurals] -, you might want to copy the ones you need from a [list][plural] instead
of writing them by hand.

When calling out Gettext to do localization on sentences with counters, you will have to provide it the
related number as well. Gettext will work out what rule should be in effect and use the correct localized version.
You will need to include in the `.po` file a different sentence for each plural rule defined.

### Sample implementation
After all that theory, let's get a little practical. Here's an excerpt of a `.po` file - don't mind with its format,
but with the overall content instead; you will learn how to edit it easily later:

{% highlight po %}
msgid ""
msgstr ""
"Language: pt_BR\n"
"Content-Type: text/plain; charset=UTF-8\n"
"Plural-Forms: nplurals=2; plural=(n > 1);\n"

msgid "We are now translating some strings"
msgstr "Nós estamos traduzindo algumas strings agora"

msgid "Hello %1$s! Your last visit was on %2$s"
msgstr "Olá %1$s! Sua última visita foi em %2$s"

msgid "Only one unread message"
msgid_plural "%d unread messages"
msgstr[0] "Só uma mensagem não lida"
msgstr[1] "%d mensagens não lidas"
{% endhighlight %}

The first section works like a header, having the `msgid` and `msgstr` especially empty. It describes the file encoding,
plural forms and other things that are less relevant.
The second section translates a simple string from English to
Brazilian Portuguese, and the third does the same, but leveraging string replacement from [`sprintf`][sprintf] so the
translation may contain the user name and visit date.
The last section is a sample of pluralization forms, displaying
the singular and plural version as `msgid` in English and their corresponding translations as `msgstr` 0 and 1
(following the number given by the plural rule). There, string replacement is used as well so the number can be seen
directly in the sentence, by using `%d`. The plural forms always have two `msgid` (singular and plural), so it is
advised not to use a complex language as the source of translation.

### Discussion on l10n keys
As you might have noticed, we are using as source ID the actual sentence in English. That `msgid` is the same used
throughout all your `.po` files, meaning other languages will have the same format and the same `msgid` fields but
translated `msgstr` lines.

Talking about translation keys, there are two main "schools" here:

1. _`msgid` as a real sentence_.
    The main advantages are:
    - if there are pieces of the software untranslated in any given language, the key displayed will still maintain some
    meaning. Example: if you happen to translate by heart from English to Spanish but need help to translate to French,
    you might publish the new page with missing French sentences, and parts of the website would be displayed in English
    instead;
    - it is much easier for the translator to understand what's going on and do a proper translation based on the
    `msgid`;
    - it gives you "free" l10n for one language - the source one;
    - The only disadvantage: if you need to change the actual text, you would need to replace the same `msgid`
    across several language files.

2. _`msgid` as a unique, structured key_.
It would describe the sentence role in the application in a structured way, including the template or part where the
string is located instead of its content.
    - it is a great way to have the code organized, separating the text content from the template logic.
    - however, that could bring problems to the translator that would miss the context. A source language file would be
    needed as a basis for other translations. Example: the developer would ideally have an `en.po` file, that
    translators would read to understand what to write in `fr.po` for instance.
    - missing translations would display meaningless keys on screen (`top_menu.welcome` instead of `Hello there, User!`
    on the said untranslated French page). That is good it as would force translation to be complete before publishing -
    however, bad as translation issues would be remarkably awful in the interface. Some libraries, though, include an
    option to specify a given language as "fallback", having a similar behavior as the other approach.

The [Gettext manual][manual] favors the first approach as, in general, it is easier for translators and users in
case of trouble. That is how we will be working here as well. However, the [Symfony documentation][symfony-keys] favors
keyword-based translation, to allow for independent changes of all translations without affecting templates as well.

### Everyday usage
In a typical application, you would use some Gettext functions while writing static text in your pages. Those sentences
would then appear in `.po` files, get translated, compiled into `.mo` files and then, used by Gettext when rendering
the actual interface. Given that, let's tie together what we have discussed so far in a step-by-step example:

#### 1. A sample template file, including some different gettext calls
{% highlight php %}
<?php include 'i18n_setup.php' ?>
<div id="header">
    <h1><?=sprintf(gettext('Welcome, %s!'), $name)?></h1>
    <!-- code indented this way only for legibility -->
    <?php if ($unread): ?>
        <h2><?=sprintf(
            ngettext('Only one unread message',
                     '%d unread messages',
                     $unread),
            $unread)?>
        </h2>
    <?php endif ?>
</div>

<h1><?=gettext('Introduction')?></h1>
<p><?=gettext('We\'re now translating some strings')?></p>
{% endhighlight %}

- [`gettext()`][func] simply translates a `msgid` into its corresponding `msgstr` for a given language. There's also
the shorthand function `_()` that works the same way;
- [`ngettext()`][n_func] does the same but with plural rules;
- there's also [`dgettext()`][d_func] and [`dngettext()`][dn_func], that allows you to override the domain for a single
call. More on domain configuration in the next example.

#### 2. A sample setup file (`i18n_setup.php` as used above), selecting the correct locale and configuring Gettext
{% highlight php %}
<?php
/**
 * Verifies if the given $locale is supported in the project
 * @param string $locale
 * @return bool
 */
function valid($locale) {
   return in_array($locale, ['en_US', 'en', 'pt_BR', 'pt', 'es_ES', 'es']);
}

//setting the source/default locale, for informational purposes
$lang = 'en_US';

if (isset($_GET['lang']) && valid($_GET['lang'])) {
    // the locale can be changed through the query-string
    $lang = $_GET['lang'];    //you should sanitize this!
    setcookie('lang', $lang); //it's stored in a cookie so it can be reused
} elseif (isset($_COOKIE['lang']) && valid($_COOKIE['lang'])) {
    // if the cookie is present instead, let's just keep it
    $lang = $_COOKIE['lang']; //you should sanitize this!
} elseif (isset($_SERVER['HTTP_ACCEPT_LANGUAGE'])) {
    // default: look for the languages the browser says the user accepts
    $langs = explode(',', $_SERVER['HTTP_ACCEPT_LANGUAGE']);
    array_walk($langs, function (&$lang) { $lang = strtr(strtok($lang, ';'), ['-' => '_']); });
    foreach ($langs as $browser_lang) {
        if (valid($browser_lang)) {
            $lang = $browser_lang;
            break;
        }
    }
}

// here we define the global system locale given the found language
putenv("LANG=$lang");

// this might be useful for date functions (LC_TIME) or money formatting (LC_MONETARY), for instance
setlocale(LC_ALL, $lang);

// this will make Gettext look for ../locales/<lang>/LC_MESSAGES/main.mo
bindtextdomain('main', '../locales');

// indicates in what encoding the file should be read
bind_textdomain_codeset('main', 'UTF-8');

// if your application has additional domains, as cited before, you should bind them here as well
bindtextdomain('forum', '../locales');
bind_textdomain_codeset('forum', 'UTF-8');

// here we indicate the default domain the gettext() calls will respond to
textdomain('main');

// this would look for the string in forum.mo instead of main.mo
// echo dgettext('forum', 'Welcome back!');
?>
{% endhighlight %}

#### 3. Preparing translation for the first run
One of the great advantages Gettext has over custom framework i18n packages is its extensive and powerful file format.
"Oh man, that’s quite hard to understand and edit by hand, a simple array would be easier!" Make no mistake,
applications like [Poedit] are here to help - _a lot_. You can get the program from [their website][poedit_download],
it’s free and available for all platforms. It’s a pretty easy tool to get used to, and a very powerful one at the same
time - using all features Gettext has available. This guide is based on PoEdit 1.8.

In the first run, you should select “File > New...” from the menu. You’ll be asked straight ahead for the language:
here you can select/filter the language you want to translate to, or use that format we mentioned before, such as
`en_US` or `pt_BR`.

Now, save the file - using that directory structure we mentioned as well. Then you should click “Extract from sources”,
and here you’ll configure various settings for the extraction and translation tasks. You’ll be able to find all those
later through “Catalog > Properties”:

- Source paths: here you must include all folders from the project where `gettext()` (and siblings) are called - this
is usually your templates/views folder(s). This is the only mandatory setting;
- Translation properties:
    - Project name and version, Team and Team’s email address: useful information that goes in the .po file header;
    - Plural forms: here go those rules we mentioned before - there’s a link in there with samples as well. You can
    leave it with the default option most of the time, as PoEdit already includes a handy database of plural rules for
    many languages.
    - Charsets: UTF-8, preferably;
    - Source code charset: set here the charset used by your codebase - probably UTF-8 as well, right?
- Source keywords: The underlying software knows how `gettext()` and similar function calls look like in several
programming languages, but you might as well create your own translation functions. It will be here you’ll add those
other methods. This will be discussed later in the “Tips” section.

After setting those points it will run a scan through your source files to find all the localization calls. After every
scan PoEdit will display a summary of what was found and what was removed from the source files. New entries will fed
empty into the translation table, and you’ll start typing in the localized versions of those strings. Save it and a .mo
file will be (re)compiled into the same folder and ta-dah: your project is internationalized.

#### 4. Translating strings
As you may have noticed before, there are two main types of localized strings: simple ones and those with plural
forms. The first ones have simply two boxes: source and localized string. The source string cannot be modified as
Gettext/Poedit do not include the powers to alter your source files - you should change the source itself and rescan
the files. Tip: you may right-click a translation line and it will hint you with the source files and lines where that
string is being used.
On the other hand, plural form strings include two boxes to show the two source strings, and tabs so you can configure
the different final forms.

Whenever you change your sources and need to update the translations, just hit Refresh and Poedit will rescan the code,
removing non-existent entries, merging the ones that changed and adding new ones. It may also try to guess some
translations, based on other ones you did. Those guesses and the changed entries will receive a "Fuzzy" marker,
indicating it needs review, appearing golden in the list. It is also useful if you have a translation team and someone
tries to write something they are not sure about: just mark Fuzzy, and someone else will review later.

Finally, it is advised to leave "View > Untranslated entries first" marked, as it will help you _a lot_ to not forget
any entry. From that menu, you can also open parts of the UI that allow you to leave contextual information for
translators if needed.

### Tips & Tricks

#### Possible caching issues
If you are running PHP as a module on Apache (`mod_php`), you might face issues with the `.mo` file being cached. It
happens the first time it is read, and then, to update it, you might need to restart the server. On Nginx and PHP5 it
usually takes only a couple of page refreshes to refresh the translation cache, and on PHP7 it is rarely needed.

#### Additional helper functions
As preferred by many people, it is easier to use `_()` instead of `gettext()`. Many custom i18n libraries from
frameworks use something similar to `t()` as well, to make translated code shorter. However, that is the only function
that sports a shortcut. You might want to add in your project some others, such as `__()` or `_n()` for `ngettext()`,
or maybe a fancy `_r()` that would join `gettext()` and `sprintf()` calls. Other libraries, such as
[oscarotero's Gettext][oscarotero] also provide helper functions like these.

In those cases, you'll need to instruct the Gettext utility on how to extract the strings from those new functions.
Don't be afraid; it is very easy. It is just a field in the `.po` file, or a Settings screen on Poedit. In the editor,
that option is inside "Catalog > Properties > Source keywords". Remember: Gettext already knows the default functions
for many languages, so don’t be afraid if that list seems empty. You need to include there the specifications of those
new functions, following [a specific format][func_format]:

- if you create something like `t()` that simply returns the translation for a string, you can specify it as `t`.
Gettext will know the only function argument is the string to be translated;
- if the function has more than one argument, you can specify in which one the first string is - and if needed, the
plural form as well. For instance, if we call our function like this: `__('one user', '%d users', $number)`, the
specification would be `__:1,2`, meaning the first form is the first argument, and the second form is the second
argument. If your number comes as the first argument instead, the spec would be `__:2,3`, indicating the first form is
the second argument, and so on.

After including those new rules in the `.po` file, a new scan will bring in your new strings just as easy as before.

### References

* [Wikipedia: i18n and l10n](https://en.wikipedia.org/wiki/Internationalization_and_localization)
* [Wikipedia: Gettext](https://en.wikipedia.org/wiki/Gettext)
* [LingoHub: PHP internationalization with gettext tutorial][lingohub]
* [PHP Manual: Gettext](https://secure.php.net/manual/book.gettext.php)
* [Gettext Manual][manual]

[Poedit]: https://poedit.net
[poedit_download]: https://poedit.net/download
[lingohub]: https://lingohub.com/blog/2013/07/php-internationalization-with-gettext-tutorial/
[lingohub_plurals]: https://lingohub.com/blog/2013/07/php-internationalization-with-gettext-tutorial/#Plurals
[plural]: http://docs.translatehouse.org/projects/localization-guide/en/latest/l10n/pluralforms.html
[gettext]: https://en.wikipedia.org/wiki/Gettext
[manual]: https://www.gnu.org/software/gettext/manual/gettext.html
[639-1]: https://en.wikipedia.org/wiki/List_of_ISO_639-1_codes
[3166-1]: https://en.wikipedia.org/wiki/ISO_3166-1_alpha-2
[rare]: https://www.gnu.org/software/gettext/manual/gettext.html#Rare-Language-Codes
[func_format]: https://www.gnu.org/software/gettext/manual/gettext.html#Language-specific-options
[aura-intl]: https://github.com/auraphp/Aura.Intl
[oscarotero]: https://github.com/oscarotero/Gettext
[symfony]: https://symfony.com/doc/current/components/translation.html
[zend]: https://docs.zendframework.com/zend-i18n/translation
[laravel]: https://laravel.com/docs/master/localization
[yii]: https://www.yiiframework.com/doc/guide/2.0/en/tutorial-i18n
[intl]: https://secure.php.net/manual/intro.intl.php
[ICU project]: http://www.icu-project.org
[symfony-keys]: https://symfony.com/doc/current/components/translation/usage.html#creating-translations

[sprintf]: https://secure.php.net/manual/function.sprintf.php
[func]: https://secure.php.net/manual/function.gettext.php
[n_func]: https://secure.php.net/manual/function.ngettext.php
[d_func]: https://secure.php.net/manual/function.dgettext.php
[dn_func]: https://secure.php.net/manual/function.dngettext.php
