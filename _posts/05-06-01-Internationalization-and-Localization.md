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
- [laminas/laminas-i18n][laminas]: 배열과 INI 파일, Gettext 파일 포맷을 지원합니다. 매번 파일시스템에서 읽어들이지 않아도 되도록
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

### 일상적인 사용법
일반적인 어플리케이션에서는 아마 웹페이지 내에서 정적인 텍스트를 넣을 때 Gettext 함수를 사용할 것입니다.
그런 텍스트들을 모아서 `.po` 파일을 만들어내고, 그것을 번역한 뒤 컴파일해서 `.mo` 파일을 만들면, 실제 사용자 인터페이스가 그려질 때
Gettext 가 그 파일을 읽어서 텍스트를 출력하게 될 것입니다. 지금까지 이야기했던 내용을 모아서 단계별 예제를 진행해봅시다.

#### 1. gettext 함수를 호출하는 몇 가지 방법을 보여주는 템플릿 파일 예제
{% highlight php %}
<?php include 'i18n_setup.php' ?>
<div id="header">
    <h1><?=sprintf(gettext('Welcome, %s!'), $name)?></h1>
    <!-- 가독성을 높이기 위해서 아래처럼 작성했습니다. -->
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

- [`gettext()`][func] 함수는 주어진 언어에 맞게 `msgid`를 `msgstr`로 번역해줍니다. `_()` 라고 짧게
써도 됩니다.
- [`ngettext()`][n_func] 같은 동작을 하지만 복수형 규칙을 사용하는 함수입니다.
- 그 외에 [`dgettext()`][d_func] 와 [`dngettext()`][dn_func] 함수가 있는데, 명시적으로 도메인을 지정할 수 있는 함수입니다.
다음 예제에서 도메인에 대해 다룰 겁니다.

#### 2. 알맞은 로케일과 Gettext 설정을 선택하는 예제 (앞선 예제에서 본 `i18n_setup.php` 같은 파일)
{% highlight php %}
<?php
/**
 * 주어진 $locale 이 이 프로젝트에서 지원되는 것인지 검증합니다.
 * @param string $locale
 * @return bool
 */
function valid($locale) {
   return in_array($locale, ['en_US', 'en', 'pt_BR', 'pt', 'es_ES', 'es']);
}

//정보 제공을 위해 기본 로케일 설정
$lang = 'en_US';

if (isset($_GET['lang']) && valid($_GET['lang'])) {
    // 쿼리스트링을 이용해서 로케일을 변경할 수 있음.
    $lang = $_GET['lang'];    //실제 제품 코드에선 입력값이 안전한지 반드시 검증해야 합니다!
    setcookie('lang', $lang); //쿠키에 저장했으므로 다른 곳에서 재사용할 수 있습니다.
} elseif (isset($_COOKIE['lang']) && valid($_COOKIE['lang'])) {
    // 쿠키에 이미 들어있는 값을 유지해서 사용합니다.
    $lang = $_COOKIE['lang']; //쿠키에 저장된 값도 안전한 입력값인지 반드시 검증 후 사용해야 합니다!
} elseif (isset($_SERVER['HTTP_ACCEPT_LANGUAGE'])) {
    // 기본값: 사용자 웹 브라우저에 설정된 언어를 찾습니다.
    $langs = explode(',', $_SERVER['HTTP_ACCEPT_LANGUAGE']);
    array_walk($langs, function (&$lang) { $lang = strtr(strtok($lang, ';'), ['-' => '_']); });
    foreach ($langs as $browser_lang) {
        if (valid($browser_lang)) {
            $lang = $browser_lang;
            break;
        }
    }
}

// 앞 단계에서 결정한 로케일을 어플리케이션 전역 로케일로 설정합니다.
putenv("LANG=$lang");

// 이렇게 해 두면 날짜 함수(LC_TIME)나 통화 포매팅(LC_MONETARY) 등의 작업에 도움이 될 것 같습니다.
setlocale(LC_ALL, $lang);

// Gettext 가 ../locales/<lang>/LC_MESSAGES/main.mo 를 사용하도록 합니다.
bindtextdomain('main', '../locales');

// 파일 인코딩을 지정해줍니다.
bind_textdomain_codeset('main', 'UTF-8');

// 어플리케이션에 다른 도메인이 더 있으면 여기서 꼭 바인딩 해줘야 합니다.
bindtextdomain('forum', '../locales');
bind_textdomain_codeset('forum', 'UTF-8');

// 어플리케이션에서 사용할 기본 도메인을 설정합니다.
textdomain('main');

// 이렇게 쓰면 main.mo 대신 forum.mo 를 사용해서 번역 문자열을 표시할 것입니다.
// echo dgettext('forum', 'Welcome back!');
?>
{% endhighlight %}

#### 3. 첫 번째 실행을 위한 번역 준비하기
Gettext가 커스텀 프레임워크 i18n 패키지에 비해 가지고 있는 가장 큰 장점은 광범위하고 강력한 파일 형식입니다.
"오, 그건 너무 이해하기도 어렵고 편집하기도 어려워요. 단순한 배열을 사용하는게 더 좋아요!" 하는 실수는 하지 않도록 합시다.
[Poedit] 같은 프로그램들이 도와줄거니까요. 그럿도 _아주많이_ 말이죠. [Poedit 웹사이트][poedit_download]에서 받을 수 있고,
무료이기도 하고 모든 플랫폼에서 사용할 수 있는 프로그램입니다. 배우기에 어렵지는 않으면서도 강력하기까지 한 도구라고 할 수 있습니다. 
Gettext 의 모든 기능을 사용할 수 있거든요. 이 가이드에서는 PoEdit 1.8  을 사용할 겁니다. 

처음 실행한 뒤 메뉴에서 "File > New..." 를 누르세요. 그러면 언어 선택 단계로 넘어갈텐데,
번역하고 싶은 언어를 검색해서 고를 수 있고, 앞에서 이야기했던 `en_US`나 `pt_BR` 같은
로케일 코드를 입력해서 언어를 결정할 수도 있습니다.

이제 파일을 저장하면 되는데 앞에서 봤던 디렉토리 구조에 맞게 저장을 하세요. 그리고나서 "소스에서 추출하기 Extract from source"를 눌러야 합니다.
이 단계에서 다양한 추출 설정과 번역 설정을 합니다. 나중에 "Catalog > Properties" 에서 그 설정을 확인할 수 있습니다.

- 소스 경로: 여러분의 프로젝트에서 `gettext()` 함수와 그 친구들을 호출하는 파일들이 모두 포함되어 있는 폴더를 소스 경로로 설정해야 합니다.
보통은 프로젝트의 템플릿/뷰 폴더일 것입니다. 꼭 필요한 설정은 이것 하나입니다.
- 번역 설정:
    - 프로젝트 이름과 버전, 팀 이름과 메일 주소: .po 파일 헤더 부분에 넣으면 유용한 내용입니다.
    - 복수형: 앞서 이야기했던 복수형에 대한 내용을 설정합니다. PoEdit가 언어별 복수형 데이터베이스를 가지고 있어서 보통은 기본값으로 두어도 괜찮습니다.
    - 문자셋: UTF-8 을 권장합니다.
    - 소스코드 문자셋: 프로젝트 소스코드의 문자셋을 설정합니다. 아마 요즘은 모두 UTF-8을 사용하고 있을 것입니다. (혹시 설마...?)
- 소스 키워드: Gettext 도구들이 이미 프로그래밍 언어별로 어떻게 `gettext()` 관련 함수들이
호출되는지 알고 있지만, 원하는 함수를 직접 만들 수도 있습니다. 여기에 그런 함수를 설정하면 됩니다.
나중에 "팁" 섹션에서 다뤄보겠습니다. 

설정을 하고 나면 소스파일을 모두 스캔해서 지역화 함수 호출을 모두 찾아낼 겁니다.
스캔을 실행할 때마다 어떤 변화를 발견했는지 실행 결과를 보여줄 것입니다. 
새로 추가된 문자열은 번역 테이블에 빈 값으로 추가됩니다. 번역이 필요한 곳에 번역된 내용을 작성하고
저장하면 .mo 이 컴파일되어서 생성(혹은 업데이트)됩니다. 짜잔~ 국제화된 프로젝트가 나왔습니다~ 

#### 4. 문자열 번역하기
앞서 살펴본 것처럼 지역화된 문자열은 단순한 종류와 복수형이 있는 종류 두 가지가 있습니다.
첫 번째 것은 원본 문자열과 지역화된 문자열 박스 두 개만 표시됩니다. 원본 문자열은
수정할 수 없게 되어 있습니다. 그건 소스 파일을 수정해야 하는 일이기 때문입니다. 변경이 필요하면 소스 파일 수정 후
다시 스캔해야 합니다. (팁: 번역 줄을 오른쪽 클릭하면 소스 파일과 문자열이 사용되는 위치를 알려줍니다.)
두 번째의 복수형이 있는 문자열은 두 개의 원본 문자열을 보여주는 박스와 각 복수형 문자열을 설정할 수 있는 
탭들이 표시됩니다.

소스 파일을 수정해서 번역도 업데이트할 필요가 있을 때에는 단순히 "새로고침"을 하면 됩니다. Poedit이 코드를
변경된 내용을 반영해줍니다. 다른 사람들이 이미 해놓은 번역에 기초해서 번역을 유추해주기도 합니다.
이렇게 유추된 항목에는 "퍼지" 표지가 붙습니다. 목록에서 금색으로 표시되는 이런 항목은 리뷰가 필요하다는
것을 나타냅니다. 그래서 프로젝트의 번역팀에서 번역을 하다가 조금 애매한 부분이 있어 다른 사람의 리뷰가 필요한 경우에도
퍼지 표지를 붙여두는 식으로 활용할 수 있습니다. 그러면 나중에 누군가 리뷰를 해주겠죠.

"보기 > 번역되지 않은 항목 먼저 (View > Untranslated entries first)"는 체크된 상태로 두는 게 좋습니다. 번역을
놓치는 항목이 생기지 않게 _상당히_ 도와줄 겁니다. 필요하다면 그 메뉴에서 번역자들을 위해서 도움이 되는 정보를
기록하는 기능을 사용할 수도 있습니다.

### 팁과 트릭

#### 캐싱 관련 문제
Apache 웹서버 모듈(`mod_php`)로 PHP가 동작할 때 `.mo` 파일이 캐시되어서 생기는 문제를 경험할 수도 있습니다.
캐시된 파일을 갱신하려면 웹서버를 재시작 해야할 수도 있습니다. Nginx에서 PHP5 를 사용할 때에는 몇 번 페이지를 새로고침하면
보통은 캐시된 번역이 갱신되는 편이고, PHP7 을 사용하면 거의 경험할 수 없을 것입니다.

#### 헬퍼 함수 추가하기
`gettext()` 보다 짧게 `_()` 라고 쓰는 게 쉬워서 많은 사람들이 헬퍼 함수를 선호합니다. 커스텀 i18n 라이브러리들도
`t()` 같은 비슷한 헬퍼 함수를 제공합니다. 하지만 보통은 그거 하나뿐입니다.
`ngettext()`를  `__()`나 `_n()`로 줄여서 쓰거나, `gettext()`와 `sprintf()` 호출을 엮어서 멋지게 `_r()`로
쓰고 싶다는 생각이 들 수 있습니다. [oscarotero's Gettext][oscarotero] 같은 라이브러리가
그런 헬퍼 함수를 제공합니다.

Gettext 도구가 그런 새로운 함수 호출 구문에서 문자열을 어떻게 추출할지 가르쳐 주기만 하면 헬퍼 함수를 추가할 수 있습니다.
어렵지 않게 할 수 있는 일입니다. `.po` 파일의 필드 하나일 뿐이고, Poedit 의 설정 화면에서도 설정할 수 있습니다.
Poedit의 "Catalog > Properties > Source keywords" 를 통해서 설정할 수 있습니다. 설정이 비어있어도 Gettext가
이미 여러 프로그래밍 언어의 기본 패턴을 내장하고 있어서 괜찮습니다. 정의하고 싶은 새로운 함수만 
[정해진 형식][func_format]에 따라 잘 추가해주면 됩니다.

- `t()` 같이 문자열 하나를 받아서 번역된 문자열을 리턴하는 함수를 만드는 경우에는 `t` 라고만 써주면
인자 하나를 받는 함수라는 것을 Gettext 가 인식해서 처리해줍니다.
- 인자가 여러 개인 함수는 각각이 어떤 용도인지 지정할 수 있습니다.
예를 들어 `__('one user', '%d users', $number)` 이렇게 사용하는 함수라면
`__:1,2` 라고 정의할 수 있습니다. 복수형의 첫 번째 형식은 첫 번째 인자이고 두 번째 형식은 두 번째 인자라는 의미입니다.
다르게 숫자(`$number`)를 첫 번째 인자로 사용하는 함수를 만든다면 `__:2,3` 이라고 정의하면 됩니다. 두 번째 인자가
복수형의 첫 번째 형식이라는 의미입니다.

`.po` 파일에 새 헬퍼 함수 규칙을 추가한 뒤에 스캔을 한 번 하면 이전과 같이 쉽게 번역 문자열을 찾아줄 것입니다.

### 참고자료

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
[symfony]: https://symfony.com/components/Translation
[laminas]: https://docs.laminas.dev/laminas-i18n/
[laravel]: https://laravel.com/docs/master/localization
[yii]: https://www.yiiframework.com/doc/guide/2.0/en/tutorial-i18n
[intl]: https://secure.php.net/manual/intro.intl.php
[ICU project]: https://icu.unicode.org/
[symfony-keys]: https://symfony.com/doc/current/translation.html#using-real-or-keyword-messages

[sprintf]: https://secure.php.net/manual/function.sprintf.php
[func]: https://secure.php.net/manual/function.gettext.php
[n_func]: https://secure.php.net/manual/function.ngettext.php
[d_func]: https://secure.php.net/manual/function.dgettext.php
[dn_func]: https://secure.php.net/manual/function.dngettext.php
