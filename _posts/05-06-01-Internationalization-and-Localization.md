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
러시아어나 세르비아어에서는 단수형 외에 두 가지 복수형이 있습니다.
총 네 가지나 대여섯 가지 형태를 사용하는 슬로베니아어,아일랜드어, 아라비아어도 존재합니다.

## 일반적인 구현 방법
PHP로 만든 소프트웨어를 국제화하는 가장 쉬운 방법은 배열 파일을 만든 후 그걸 템플릿 파일에서 
`<h1><?=$TRANS['title_about_page']?></h1>` 같은 식으로 사용하는 것입니다. 하지만 이런 방법은 복수화 작업 같이 프로젝트 아주 초반부터
불거질 수 있는 문제 등, 유지관리 하기 어렵게 만드는 원인이 되므로 제대로된 프로젝트에는 권할 수 없는 방법입니다.
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
그래서 Yii 에서는 숫자, 날짜, 시간, 시간 간격(intervals), 통화, 서수(ordinals) 의 포매팅 등 강력한 치환 기능을 사용할 수 있습니다.

추출기를 제공하지 않는 라이브러리를 사용하기로 결정했다면, gettext 포맷을 사용하는 것이 좋을 것 같습니다.
그러면 이 장의 나머지 부분에서 설명하는대로 Poedit를 포함한 gettext의 여러 도구들을 사용할 수 있습니다.

## Gettext

### 설치
여러분이 사용하고 있는 시스템에 따라, `apt-get` 이나 `yum` 같은 패키지 매니저를 사용해서 Gettext와 PHP 라이브러리를 설치하면 됩니다.
설치 후에는 `extension=gettext.so` (Linux/Unix) 나 `extension=php_gettext.dll` (Windows) 를 `php.ini` 파일에 추가합니다.

번역 파일을 만들 때에는 [Poedit]을 사용하려고 합니다. 사용하는 시스템의 패키지 매니저에서
Poedit을 찾을 수 있을 겁니다. [Poedit 웹 사이트에서 무료로 다운로드][poedit_download]해서 사용할 수도 있습니다.

### 구조

#### 파일 유형
gettext를 사용할 때 주로 작업하게 되는 파일이 세 종류 있습니다. 주요 파일은 PO (Portable Object)와
MO (Machine Object) 파일인데 PO는 사람이 읽을 수도 있는 "번역된 오브젝트" 목록이고 MO는
gettext 가 지역화를 할 때 읽는 바이너리 파일입니다. POT (템플릿) 파일도 있는데, 
여러분의 소스 파일에 있는 모든 키를 가지고 있어서 PO 파일을 생성하거나 업데이트 할 때 가이드로 사용되는 파일입니다.
템플릿 파일이 꼭 필요한 것은 아닙니다. PO/MO 파일만 사용해도 괜찮습니다.
번역한 언어마다 한 쌍의 PO/MO 파일이 꼭 있어야 하고, POT 파일은 도메인마다 하나씩 존재하는 구조입니다.

### 도메인
큰 프로젝트에서는 맥락에 따라 같은 단어라도 다른 의미를 나타내는 경우가 있어, 서로 분리해서 번역을 해야하는
일이 있습니다. 그런 경우 번역 결과물을 서로 다른 _도메인_ 으로 분리하면 됩니다. 기본적으로 도메인은
서로 다른 이름으로 그룹 지은 POT/PO/MO 파일들입니다. 그 파일 이름이 _번역 도메인_ 입니다. 소규모에서 중간 정도 규모의
프로젝트에서는 도메인 하나만 사용하는 편이 일반적입니다. 그 편이 단순해서 좋습니다. 그 때 도메인 이름은 아무렇게나 지어도
되지만 우리 예제에서는 "main" 이라고 하겠습니다.
도메인을 사용하는 예로, [Symfony] 프로젝트에서는 검증 로직에서 사용되는 메시지 번역을 구분하는데에 도메인을 사용하고 있습니다.

#### 로케일 코드
로케일이란 간단히 말하자면 한 가지 언어를 지칭하는 코드값입니다. 로케일은 [ISO 639-1][639-1] 와 [ISO 3166-1 alpha-2][3166-1] 라는
명세를 따릅니다. 언어를 나타내기 위해서 소문자 알파벳 두개를 사용하고, 선택적으로 그 뒤에 국가나 지역 코드를 붙입니다.
언어 코드 뒤에 밑줄을 붙이고 국가나 지역 코드를 지칭하는 대문자 알파벳 두 글자를 붙이는데, [드물게][rare] 세글자로 된 
국가코드를 사용하는 경우도 있습니다.

어떤 언어 사용자에게는 로케일의 국가 부분은 군더더기처럼 보일 겁니다. 그런데 또 어떤 언어는 국가별로
다른 사투리가 있습니다. 오스트리아 독일어 (`de_AT`)나 브라질 포르투갈어 (`pt_BR`)가 그렇습니다. 로케일 코드의 두 번째 부분은 이런 지역별 사투리를
구분하기 위해서 사용됩니다. 그 부분이 생략되면 "일반적인" 혹은 "혼합된" 버전의 언어를 지칭하는 것으로 받아들이면 됩니다.

### 디렉토리 구조
Gettext 를 사용할 때 맞춰야하는 폴더 구조가 있습니다. 일단은 소스 저장소 폴더 안에 l10n 파일을 모두 넣을 폴더 하나를 정해야 합니다. 
이 폴더 이름은 자유롭게 정해도 됩니다. 그 폴더 아래에 필요한 로케일 마다 폴더 하나씩을 만듭니다. 폴더 이름은 로케일 코드입니다. 
로케일 폴더 안에는 `LC_MESSAGES` 라는 폴더가 꼭 있어야 합니다. `LC_MESSAGES` 폴더에는 PO/MO 파일을 둡니다. 아래 예시를 참고하세요. 

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

### 복수형
앞서 이야기한 것처럼 언어마다 복수형을 표현하는 방법이 다릅니다. 하지만 gettext를 사용하면 문제를 피할 수 있습니다.
gettext 에서는 `.po` 파일마다 사용하는 언어에 맞는 [복수형 규칙][plural] 을 선언하게 되어 있습니다.
그리고 단수형과 복수형일 때 다르게 표시되어야 하는 문자열은 번역할 때 각 규칙에 맞는 형태의 문자열을 번역해서 넣게 되어 있습니다.
Gettext 함수를 호출하는 코드에는 해당 문자열에서 사용되는 숫자를 명시해 줍니다. 그러면 숫자에 맞게 단수형 혹은 복수형 중 맞는 번역 문자열이 사용됩니다.
문자열 치환이 필요한 경우에도 문제 없이 잘 동작합니다. 

"복수형 규칙"에는 이 언어에 몇 가지 복수형이 있는지와 주어진 숫자 `n` 에 대해서 어느 복수형을 사용하는 게 맞는지 결정할 수 있는 식이 들어 있습니다.
아래 예를 보시죠.

- 일본어: `nplurals=1; plural=0` - 규칙이 하나만 있음.
- 영어: `nplurals=2; plural=(n != 1);` - 두 가지 규칙이 있음. N이 1이면 첫 번째, 그 외에는 두 번째 규칙 사용.
- 브라질 포르투칼어: `nplurals=2; plural=(n > 1);` - 두 가지 규칙이 있음. N이 1보다 크면 두 번째 규칙 사용, 그 외에는 첫 번째 규칙 사용.

이제 복수형 규칙이 어떤 식으로 동작하는지 이해했으므로, [목록][plural] 중에서 적합한 규칙을 복사해서 사용하면 됩니다.
혹시 좀더 자세한 설명을 보고 싶다면 [LingoHub 튜토리얼][lingohub_plurals]을 참고하세요.

갯수를 포함한 문자열을 인자로 지정해서 Gettext 함수를 호출하는 코드에는 반드시
그 갯수도 인자로 지정해야 합니다. 그러면 번역 문자열 들 중 그 수에 맞는 것을 잘 찾아서 사용하게 될 것입니다.
그러기 위해서는 `.po` 파일에다 각 복수형 규칙에 해당하는 번역 문자열을 써두어야 합니다.

### 구현 예시
이론은 많이 보았으니, 간단히 실전을 경험해봅시다. `.po` 파일의 일부를 같이 봅시다. 파일 포맷은 자세히 보지 않아도 됩니다.
대신 내용이 전반적으로 어떻게 되어 있는지를 보는 게 좋습니다. 이 파일을 편하게 수정하는 방법은 나중에 배우게 될 것입니다.

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

plural forms and other things that are less relevant.
빈 `msgid`와 `msgstr`이 있는 첫 부분은 파일 헤더처럼 동작합니다. 파일 인코딩과 함께
복수형도 설정하고 있고, 그외에 크게 관련이 없는 것들이 기술되어 있습니다.
두 번째 덩어리에서는 간단한 영어 문자열을 브라질 포르투칼어로 번역해 둔 것을 볼 수 있고,
세 번째 덩어리는 두 번째 덩어리와 거의 같은데, [`sprintf`][sprintf]로 문자열 치환을 해서
사용자 이름과 방문 날짜를 포함하게 될 것 같습니다.
마지막 부분에서는 복수형을 다루는 예시를 볼 수 있습니다.
영어로 단수형과 복수형에 해당하는 `msgid`가 있고, 그에 해당하는 번역이 `msgstr` 0과 1로
번역되어 있습니다. 복수형에는 `%d` 로 문자열 치환을 사용해서 메시지 내에 숫자를 표시하게 되어 있습니다.
복수형을 구분해서 표현하는 곳에는 항상 `msgid` 가 두 개(단수형과 복수형) 나타납니다. 그러므로
번역의 원본이 되는 언어는 복잡하지 않은 언어를 사용하는 편이 좋습니다.

### l10n 키에 관한 논의
위에서 우리는 실제 영어 문자열을 메시지 ID로 사용했습니다. 그 `msgid` 는 프로젝트 내의 모든
`.po` 파일에서 일관되게 사용이 되어야 합니다. 각 언어별 파일에서 `msgid` 는 똑같은 값을 사용하면서
`msgstr` 만을 번역해서 사용하는 것이죠.

이 번역 키(`msgid`)를 바라보는 두 가지 관점이 있습니다.

1. _사용되는 실제 문자열을 `msgid`로 사용_.
    주된 장점:
    - 특정 언어에서 일부 문자열의 번역이 누락된 경우 `msgid` 를 그대로 표시하게 되는데, 그런 상황에서도 어느 정도 의미가
    전달됩니다. 즉, 여러분의 프로젝트가 주 언어는 영어이고 스페인어 번역도 해서 같이 제공은 할 수 있었지만
    프랑스어 번역은 도움이 필요한 경우라면, 프랑스어 번역을 비워두어도 웹 사이트에 접속한 프랑스어 사용자는
    영어로 사이트를 사용할 수 있는거죠.
    - 번역자가 `msgid` 만 봐도 맥락을 충분히 파악해서 적절한 번역을 하는데에 도움이 됩니다.
    - 주 언어 하나는 "공짜로" 지역화가 되는 효과가 있습니다.
    - 딱 하나의 단점: 주 언어의 텍스트가 바뀌는 일이 생기면, 해당되는 `msgid` 를 모든 언어 파일에서 모두 찾아서 똑같이 바꿔야 합니다.

2. _구조화 된 유일한 키값을 `msgid`로 사용_.
`msgid` 에 문자열의 내용을 넣는 것이 아니라, 그 문자열이 어플리케이션에서 사용되는 위치와 역할을 구조화된 방법으로 표현하는 것입니다.
    - 텍스트 내용과 템플릿 로직을 분리함으로써, 코드를 정돈할 수 있는 좋은 방법입니다.
    - 그러나, 번역자가 맥락을 파악하기 힘들어지는 문제가 있습니다. 특정 언어 번역을 위해서는
    반드시 주 언어 파일을 기초로 작업해야 합니다. 즉, 번역자들이 `fr.po` 에 프랑스어 번역을 작성하기 위해서는
    `en.po` 파일을 보면서 작업해야 합니다.
    - 번역이 누락된 문자열은 의미없는 키 값으로 표시됩니다. (`Hello there, User!` 대신 `top_menu.welcome` 이 표시되는 거죠.)
    최종 릴리스가 되기 전에 모든 문자열의 번역이 강제된다는 점에서는 좋을 수도 있지만,
    실수로 번역을 빠뜨리면 끔직한 화면이 노출되는 문제가 있죠. 어떤 라이브러리들은 "기본(fallback)" 언어를
    지정하는 기능을 제공하기도 합니다. 그러면 1번 방법과 같은 효과가 납니다.

[Gettext 설명서][manual] 는 1번 방법을 권하는데, 번역자가 작업하거나 문제가 생겼을 때 대처하기가 쉽다는
이유 때문입니다. 이 문서에서 우리는 이 방법을 따를 겁니다. 반면 [Symfony 문서][symfony-keys]는 
2번 방법을 권합니다. 템플릿에는 전혀 영향을 주지 않으면서 번역에만 독립적으로 변화를 줄 수 있는 장점이 있다는 이유죠.

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
