Jekyll-Scholar
==============
[![CI](https://github.com/inukshuk/jekyll-scholar/actions/workflows/ci.yml/badge.svg)](https://github.com/inukshuk/jekyll-scholar/actions/workflows/ci.yml)

Jekyll-Scholar는 모든 학술 블로거들을 위한 것입니다. 이는 블로그 지향의 정적 사이트 생성기인 [Jekyll](http://jekyllrb.com/)에 대한 확장 기능 세트로, 웹용 참고 문헌 및 독서 목록을 형식화하고 블로그 게시물에 인용 기능을 부여합니다.

이미 Jekyll-Scholar를 사용 중이며 도움이 되고 싶으신가요? 유지 관리자가 되고 싶으시면 저희에게 연락해 주세요!

설치
----

```
$ [sudo] gem install jekyll-scholar
```

또는 `Gemfile`에 추가하세요:

```
gem 'jekyll-scholar', group: :jekyll_plugins
```

### Github Pages

이 플러그인을 [기본 Github 페이지 워크플로우](https://help.github.com/articles/using-jekyll-with-pages/)와 함께 사용할 수 없음을 유의하세요.
Github은 보안상의 이유로 몇몇 선택된 플러그인만 실행을 허용하며, Jekyll-Scholar는 그 중 하나가 아닙니다.
사이트를 로컬에서 생성하고 결과를 사이트 저장소의 `master` 또는 `gh-pages` 브랜치로 푸시해야 합니다.
소스, 설정 및 플러그인은 별도의 브랜치에 보관할 수 있습니다; 예를 들어 [여기](http://davidensinger.com/2013/07/automating-jekyll-deployment-to-github-pages-with-rake/)를 참조하세요.

대안으로 [Github Actions](https://github.com/features/actions)에서 [jekyll-action](https://github.com/helaili/jekyll-action)을 사용하여 사이트를 Github Pages에 배포할 수 있습니다.

사용법
-----

새로운 [Jekyll](http://jekyllrb.com/) 디렉터리를 설치하고 설정하세요 (자세한 지침은 [Jekyll-Wiki](https://github.com/mojombo/jekyll/wiki/Usage)를 참조하세요). Jekyll-Scholar를 활성화하려면 플러그인 디렉터리의 파일(예: `_plugins/ext.rb`)에 다음 문을 추가하세요:

```
require 'jekyll/scholar'
```

대안으로 Jekyll 설정의 `gem` 목록에 `jekyll-scholar`를 추가하세요:

```
plugins: ['jekyll/scholar']
```

### 설정

Jekyll 설정 파일에서 `scholar` 키를 사용하여 Jekyll-Scholar 설정을 조정할 수 있습니다. 예를 들어 다음은 참고 문헌 스타일을 `modern-language-association`으로 설정합니다.

```
scholar:
    style: modern-language-association
```

아래 표는 일반적으로 사용되는 설정 옵션을 설명합니다. 모든 옵션과 기본값에 대한 설명은 [`defaults.rb`](/lib/jekyll/scholar/defaults.rb)를 참조하세요.

| 옵션 | 기본값 | 설명 |
|--------|---------|-------------|
| `style` | `apa` | 참고 문헌과 인용에 사용되는 스타일을 나타냅니다. [CiteProc-Ruby](https://github.com/inukshuk/citeproc-ruby)와 함께 제공되는 스타일을 이름으로 사용할 수 있습니다(예: apa, chicago-fullnote-bibliography). 이는 보통 [여기](https://github.com/citation-style-language/styles)에서 `.csl` 확장자 없이 파일 이름으로 볼 수 있습니다; `dependent/style` 디렉토리에서 하나를 사용하려면 `dependent/style`을 사용해야 합니다. 대안으로, 어떤 CSL 스타일에든 링크를 추가할 수 있습니다(예: 공식 [CSL 스타일 저장소](https://github.com/citation-style-language/styles)에서 사용 가능한 스타일에 링크할 수 있습니다). |
| `locale` | `en` | 참고 문헌을 형식화할 때 사용할 언어를 정의합니다(이는 보통 지역화된 용어에 적용됩니다. 예: 영어에서 편집자는 'Eds.'). |
| `source` | `./_bibliography` | 참고 문헌이 저장되는 위치를 나타냅니다. |
| `bibliography` | `references.bib` | 기본 참고 문헌의 이름을 나타냅니다. 최상의 결과를 얻으려면 참고 문헌이 ASCII 또는 UTF-8로 인코딩되었는지 확인하세요. 문자열에 `*`가 포함된 경우 `Dir::glob`에 전달되므로 `**/*.bib{,tex}`은 `source` 아래의 모든 `*.bib` 및 `*.bibtex` 파일을 찾습니다. |
| `allow_locale_overrides` | `false` | `true`일 때, BibTex의 `language` 항목이 개별 항목에 대해 `locale` 설정을 재정의할 수 있도록 허용합니다. 언어가 없으면 `locale`로 되돌아갑니다. 언어 값은 두 글자의 [ISO 639-1](https://www.loc.gov/standards/iso639-2/php/code_list.php) 표준을 사용하여 인코딩되어야 합니다. 예: 영어 = 'en', 스페인어 = 'es'. |
| `sort_by` | `none` | 참고 문헌 항목을 정렬할지 여부 및 방법을 지정합니다. 항목은 여러 필드에서 정렬할 수 있으며, 키 목록을 사용하여 정렬할 수 있습니다. 예: `year,month`. 정렬 수준별로 순서를 지정할 수 있습니다. 예: `order: descending,ascending`는 연도를 내림차순으로 정렬하지만 연도별로는 월이 오름차순입니다. 정렬 키가 주문 지시문보다 많으면 마지막 주문 항목이 나머지 키에 사용됩니다. |
| `order` | `ascending` | 참고 문헌 항목이 정렬되는 순서를 지정합니다. `ascending` 또는 `descending`일 수 있습니다. 정렬 수준별로 순서를 지정할 수 있습니다. 예: `descending,ascending`는 첫 번째 키에서는 내림차순으로, 두 번째 키에서는 오름차순으로 정렬합니다. 정렬 키가 주문 지시문보다 많으면 마지막 주문 항목이 나머지 키에 사용됩니다. |
| `group_by` | `none` | 참고 문헌 항목이 그룹화되는 방식을 지정합니다. 그룹화는 다단계일 수 있습니다. 예: `type, year`는 출판 유형별로 항목을 그룹화하고, 해당 그룹 내에서 연도별로 그룹화합니다. |
| `group_order` | `ascending` | 그룹의 정렬은 정렬 순서와 동일한 방식으로 지정됩니다. 출판 유형 - 그룹 키 `type`으로 지정된 출판 유형은 구성에서 `type_order`를 추가하여 정렬할 수 있습니다. 예를 들어, `type_order: [article,techreport]`는 학술지 논문을 기술 보고서 앞에 나열합니다. `type_order`에 언급되지 않은 유형은 언급된 유형보다 작게 간주됩니다. `type_aliases` 설정을 사용하여 여러 유형을 하나의 그룹으로 병합할 수 있습니다. 기본적으로 `phdthesis` 및 `mastersthesis`는 `thesis`로 그룹화됩니다. 예를 들어 `type_aliases: { inproceedings: article}`를 사용하면 학술지 논문과 학회 논문이 단일 그룹에 나타납니다. 항목 유형의 표시 이름은 `type_names`를 사용하여 지정됩니다. 일반적인 유형의 이름이 제공되지만, 확장하거나 재정의할 수 있습니다. 예를 들어, `article`의 기본 이름은 *Journal Articles*이지만, `type_names: { article: Papers }`를 사용하여 *Papers*로 변경할 수 있습니다. |
| `bibtex_filters` | `latex,smallcaps,superscript` | 항목의 값을 통과시킬 [BibTeX-Ruby](https://github.com/inukshuk/bibtex-ruby) 형식 필터를 구성합니다. 기본 `latex` 필터는 LaTeX 문자 이스케이프를 유니코드로 변환하고, `smallcaps`는 `\textsc` 명령을 HTML `<font style=\"font-variant: small-caps\">` 태그로 변환하며, `superscript`는 `\textsuperscript` 명령을 HTML `<sup>` 태그로 변환합니다. |
| `raw_bibtex_filters` | ` ` | 원시 BiBTeX 항목(즉, `{{ entry.bibtex }}`를 통해 사용할 수 있는 항목)을 통과시킬 [BibTeX-Ruby](https://github.com/inukshuk/bibtex-ruby) 형식 필터를 구성합니다. 이를 통해 예를 들어 `linebreaks` 필터를 사용하여 과도한 줄 바꿈을 제거할 수 있습니다. |

### 참고 문헌

Jekyll-Scholar를 로드하면 `.bib` 또는 `.bibtex` 확장자를 가진 모든 파일이 Jekyll을 실행할 때 변환됩니다(파일에 YAML 헤더를 추가하는 것을 잊지 마세요). 파일은 일반 HTML 또는 Markdown 및 BibTeX 항목을 포함할

 수 있습니다. 후자는 Jekyll 설정 파일에 정의된 인용 스타일 및 언어에 따라 Jekyll-Scholar에 의해 형식화됩니다.

예를 들어, 루트 디렉터리에 `bibliography.bib` 파일이 있는 경우:

    ---
    ---
    References
    ==========
    
    @book{ruby,
      title     = {The Ruby Programming Language},
      author    = {Flanagan, David and Matsumoto, Yukihiro},
      year      = {2008},
      publisher = {O'Reilly Media}
    }

이는 다음 내용으로 `bibliography.html`로 변환됩니다:

    <h1 id='references'>References</h1>
    
    <p>Flanagan, D., &#38; Matsumoto, Y. (2008). <i>The Ruby Programming Language</i>. O&#8217;Reilly Media.</p>

이로 인해 Jekyll 기반 블로그나 웹사이트에 참고 문헌을 쉽게 추가할 수 있습니다.

다른 변환기를 사용하여 사이트를 생성하는 경우에도 걱정하지 마세요. `bibliography` 태그를 사용하여 참고 문헌을 생성할 수 있습니다. 사이트 또는 블로그 게시물에서 다음과 같이 호출하세요:

    {% bibliography %}

이렇게 하면 기본 참고 문헌이 생성됩니다. 여러 개를 사용하는 경우, 이름을 전달하여 Jekyll-Scholar에게 어떤 참고 문헌을 렌더링해야 하는지 알릴 수 있습니다.

예를 들어, `_bibliography/books.bib` 및 `_bibliography/papers.bib`에 두 개의 참고 문헌이 저장되어 있는 경우, 각각 `{% bibliography --file books %}` 및 `{% bibliography --file papers %}`를 호출하여 사이트에 참고 문헌을 포함할 수 있습니다. 예를 들어, 여러 참조 목록이 있는 `references.md` 파일이 있을 수 있습니다:

    ---
    title: My References
    ---
    
    {{ page.title }}
    ================
    
    The default Bibliography
    ------------------------
    
    {% bibliography %}
    
    Secondary References
    --------------------
    
    {% bibliography --file secondary %}

마지막으로, 참고 문헌 태그는 선택적 필터 매개변수를 지원합니다. 이 필터는 구성 파일에 정의된 전역 필터보다 우선합니다.

    {% bibliography --query @*[year=2013] %}

위 예제는 2013년에 발표된 모든 항목의 참고 문헌을 출력합니다. 물론 파일 및 필터 매개변수를 다음과 같이 결합할 수도 있습니다:

    {% bibliography -f secondary -q @*[year=2013] %}

이는 `_bibliography/secondary.bib`의 2013년 출판물 목록을 출력합니다.

필터에 대한 자세한 내용은 아래 해당 섹션을 참조하거나 [BibTeX-Ruby](https://github.com/inukshuk/bibtex-ruby) 문서를 참조하세요.

참고 문헌 항목 수를 제한해야 하는 경우, `--max` 옵션을 사용할 수 있습니다:

    {% bibliography --max 5 %}

이는 참고 문헌의 첫 5개 항목만 포함하는 참고 문헌을 생성합니다(쿼리 필터 및 정렬 옵션이 적용된 후). 그룹화가 활성화된 경우 항목 제한이 비활성화됩니다.

### 참고 문헌 항목 수 반환

`bibliography_count`는 참고 문헌에 렌더링될 항목 수를 반환합니다. 이 태그는 `bibliography` 태그와 동일한 매개변수를 허용합니다.

    {% bibliography_count -f references --query @book[year <=2000] %}

추가 예제는 [#186](https://github.com/inukshuk/jekyll-scholar/blob/master/features/186.feature)을 참조하세요.

### 참고 문헌 템플릿

참고 문헌은 항상 순서가 지정된 목록으로 렌더링됩니다. 추가로, 각 참조는 HTML 태그(기본값은 `span`이지만 `reference_tagname` 설정을 사용하여 변경할 수 있음)로 묶여 있으며, 인용 키가 ID로 사용됩니다. 참조 문자열 자체는 CSL 스타일의 규칙에 의해 결정되지만, 주요 템플릿을 조금 커스터마이징할 수도 있습니다. 기본적으로 템플릿은 `{{reference}}`입니다 – 이는 참조 태그만 렌더링합니다. 템플릿은 Liquid를 사용하여 렌더링되며, 참조 외에도 인용 키(`key`), 항목의 `type`, 참고 문헌의 `index`, 파일 저장소 링크(`link`)를 노출합니다. 따라서 구성에서 템플릿을 다음과 같이 커스터마이징할 수 있습니다:

    scholar:
      bibliography_template: <abbr>[{{key}}]</abbr>{{reference}}

이렇게 하면 다음과 같이 처리됩니다:

    <li><abbr>[ruby]</abbr><span id="ruby">Matsumoto, Y. (2008). <i>The Ruby Programming Language</i>. O&#8217;Reilly Media.</span></li>

더 복잡한 요구 사항이 있는 경우, 템플릿이 구성 내에 있는 것이 번거로울 수 있습니다; 이러한 이유로 템플릿을 레이아웃 디렉터리에 넣을 수도 있습니다. Jekyll-Scholar는 구성에서 설정한 옵션이 기존 레이아웃(파일 확장자 제외)과 일치하면 이 템플릿을 로드합니다. 즉, 다음과 같이 설정하면:

    scholar:
      bibliography_template: bib

그리고 `_layouts/bib.html`(또는 다른 확장자를 가진 파일)이 있으면 이 파일의 내용이 템플릿으로 사용됩니다. 이 파일에 YAML 프런트 매터가 포함되어야 한다는 점을 유의하세요! 예를 들어, 다음은 더 복잡한 템플릿 파일입니다:

    ---
    ---
    {{ reference }}
    
    {% if entry.abstract %}
    <p>{{ entry.abstract }}</p>
    {% endif %}
    
    <pre>{{ entry.bibtex }}</pre>

템플릿을 구성에 포함시키는 대신, 각 참고 문헌 태그에 `--template` 또는 `-T` 옵션 매개변수를 전달하여 기본 참고 문헌 템플릿을 재정의할 수도 있습니다.

### 인용

블로그 게시물에서 참고 문헌의 책이나 논문을 참조하고 싶다면, Jekyll-Scholar도 도와줄 수 있습니다. 인용하려는 항목의 적절한 키를 사용하여 `cite` 태그를 사용하면, Jekyll-Scholar가 서식을 갖춘 인용 참조를 생성합니다. 빠른 예제를 보려면 다음 블로그 게시물을 참고하세요:

    ---
    layout: default
    title: A Blogging Scholar
    ---
    
    {{ page.title }}
    ================
    
    Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod tempor
    incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis
    nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat.
    Duis 'aute irure dolor in reprehenderit in voluptate' {% cite derrida:purveyor %}
    velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat
    cupidatat non proident, 'sunt in culpa qui officia deserunt mollit anim id est
    laborum' {% cite rabinowitz %}.
    
    Duis 'aute irure dolor in reprehenderit in voluptate' {% cite breton:surrealism %}
    velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat
    cupidatat non proident, 'sunt in culpa qui officia deserunt mollit anim id est
    laborum' {% cite rainey %}.
    
    References
    ----------
    
    {% bibliography %}

이렇게 하면 참조 섹션에 전체 참고 문헌이 출력됩니다. 페이지에 인용된 항목만 포함하려면 `cited` 옵션을 참고 문헌 태그에 전달하세요:

    {% bibliography --cited %}

기본적으로, `--cited` 옵션은 정렬 옵션을 설정한 경우에도 여전히 참고 문헌을 정렬합니다. 인용 번호를 사용하는 스타일의 경우, 이는 보통 원하는 동작이 아닙니다. 이러한 경우, `--cited` 대신 `--cited_in_order`를 사용하면 페이지에 인용된 순서대로 인용된 항목이 포함된 참고 문헌을 얻을 수 있습니다.

더 긴 인용문에는 Jekyll-Scholar가 제공하는 `quote` 태그를 사용하세요:

    {% quote derrida:purveyor %}
    Lorem ipsum dolor sit amet, consectetur adipisicing elit,
    sed do eiusmod tempor.
    
    Lorem ipsum dolor sit amet, consectetur adipisicing.
    {% endquote %}

예를 들어, 이는 다음과 같이 렌더링될 수 있습니다:

    <blockquote>
      <p>Lorem ipsum dolor sit amet, consectetur adipisicing elit,<br/>
      sed do eiusmod tempor.</p>
      <p>Lorem ipsum dolor sit amet, consectetur adipisicing.</p>
      <cite>
        <a href="#derrida:purveyor">(Derrida, 1975)</a>
      </cite>
    </blockquote>

#### 다중 인용

단일 인용에서 여러 항목을 인용하려면 인용하려는 항목의 모든 ID를 공백으로 구분하여 참조합니다. 예를 들어, `{% cite ruby microscope %}`는 다음과 같은 인용 태그를 생성합니다:

    <a href="#ruby">(Flanagan &amp; Matsumoto 2008; Shaughnessy 2013)</a>

#### 여러 참고 문헌이 있는 경우의 인용

위 예에서처럼 `_bibliography/books.bib` 및 `_bibliography/papers.bib`에 두 개의 참고 문헌이 저장되어 있는 경우를 생각해 봅시다. 또한, 주 참고 문헌, 예를 들어 `_bibliography/references.bib`도 있어야 합니다. 위에서 알 수 있듯이 `{% bibliography --file books %}` 또는 `{% bibliography --file papers %}`를 호출하여 주 참고 문헌이 아닌 참고 문헌을 사용할 수 있습니다.

하지만 주 참고 문헌에 없는 기사를 인용하려면 어떻게 해야 할까요? 위와 같은 방법을 사용하여 `books.bib` 참고 문헌에서 기사를 인용하려면 `{% cite ruby --file books %}`를 호출하면 됩니다.

#### 저자 이름 억제

때때로 텍스트에 이미 이름이 언급되었기 때문에 인용에서 저자 이름을 억제하고 싶을 때가 있습니다; 이러한 경우를 위해 Jekyll-Scholar는 `--suppress_author` 옵션(짧은 형식: `-A`)을 제공합니다. 예: `...as Matz explains {% cite ruby -A -l 42 %}`는 다음과 같이 생성됩니다: `...as Matz explains (2008, p. 42)`.

#### 페이지 번호 및 로케이터

인용에 페이지 번호나 유사한 로케이터를 추가하려면 `-l` 또는 `--locator` 옵션을 사용하세요. 예를 들어, `{% cite ruby --locator 23-5 %}`는 `(Matsumoto, 2008, pp. 23-5)`와 같은 인용을 생성합니다.

여러 항목을 인용할 때(위 참조) 각 항목 ID 목록 뒤에 여러 로케이터를 추가할 수 있습니다. 예를 들어, `{% cite ruby microscope -l 2 -l 24 & 32 %}`.

페이지는 기본 로케이터이지만, `-L` 또는 `--label` 옵션(로케이터별 하나씩)을 추가하여 로케이터 유형을 나타낼 수 있습니다. 예를 들어, `{% cite ruby microscope --label chapter --locator 3 -L figure -l 24 & 32 %}`는 다음과 같이 생성됩니다: `(Matsumoto, 2008, chap. 3; Shaughnessy, 2013, figs. 24 & 32)`.

#### 서식이 지정된 참고 문헌 항목 표시

서식이 지정된 전체 참고 문헌 항목을 표시하려면 `reference` 태그를 사용할 수 있습니다. 예를 들어, 다음과 같은 Bibtex 항목이 있는 경우,

    @book{ruby,
      title     = {The Ruby Programming Language},
      author    = {Flanagan, David and Matsumoto, Yukihiro},
      year      = {2008},
      publisher = {O'Reilly Media}
    }

페이지 어디에서든 `{% reference ruby %}`를 사용하면, "Flanagan, D., & Matsumoto, Y. (2008). *The Ruby Programming Language.* O'Reilly Media"와 같은 결과를 출력합니다(정확한 결과는 형식 스타일에 따라 다름).

`reference` 태그는 참고 문헌 태그와 동일한 --file/-f 매개변수를 허용합니다. 이는 특정 페이지의 입력으로 특별한 BibTeX 파일을 사용하려는 경우에 유용할 수 있습니다. 예를 들어, 태그

    {% reference ruby --file /home/foo/bar.bib %}

는 파일 `/home/foo/bar.bib`에서 키 `ruby`를 읽으려고 시도합니다. 기본 BibTeX 파일로 돌아가지 않습니다.

#### 사이트 내 다른 페이지를 가리키는 인용
때때로 인용이 사이트의 다른 페이지를 가리키기를 원할 수 있습니다(예: 별도의 참고 문헌 페이지). 솔루션으로 scholar 설정에 상대 경로를 추가합니다:

~~~ yaml
    scholar:
      relative: "/relative/path/file.html"
~~~

#### 하나의 문서 내 여러 참고 문헌([multibib.sty](http://www.ctan.org/pkg/multibib)처럼)

하나의 파일에 여러 `{% bibliography %}` 섹션이 있는 경우, Jekyll-Scholar는 동일한 `id` 속성을 가진 여러 목록을 생성합니다. 결과적으로 참조를 인용할 때 `id` 속성을 고유하게 해석할 수 없습니다. 브라우저는 항상 첫 번째 `id` 속성으로 이동합니다. 더욱이, 유효한 HTML은 고유한 `id` 속성을 요구합니다. 이러한 시나리오는 예를 들어 동일한 참조를 다른 블로그 게시물에서 인용하고, 이러한 게시물이 하나의 HTML 문서에 표시되는 경우 발생할 수 있습니다.

해결책으로, Jekyll-Scholar는 `--prefix` 태그를 제공합니다. 첫 번째 게시물에서 다음과 같이 인용할 수 있습니다:

    ---
    title: Post 1
    ---
    Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod tempor
    incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis
    nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat.
    Duis 'aute irure dolor in reprehenderit in voluptate'
    {% cite derrida:purveyor --prefix post1 %} velit esse cillum
    dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat
    non proident, 'sunt in culpa qui officia deserunt mollit anim id
    est laborum' {% cite rabinowitz --prefix post1 %}.
    
    References
    ----------
    
    {% bibliography --cited --prefix post1 %}

두 번째 블로그 게시물에서는 다음과 같이 인용합니다:

    ---
    title: Post 2
    ---
    Duis 'aute irure dolor in reprehenderit in voluptate'
    {% cite rabinowitz --prefix post2 %} velit esse cillum
    dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat
    non proident, 'sunt in culpa qui officia deserunt mollit anim id
    est laborum' {% cite rainey --prefix post2  %}.
    
    References
    ----------
    
    {% bibliography --cited --prefix post2 %}

두 게시물이 모두 'rabinowitz'를 인용하지만, 두 인용은 고유한 식별자로 할당되어 각각의 참조 섹션으로 링크됩니다. 두 게시물이 단일 HTML 문서로 렌더링되어도 마찬가지입니다.

#### 인용 참조에 사용자 정의 클래스 추가
기본적으로 Jekyll Scholar는 클래스를 가진 링크를 생성합니다:

    <a href="#ruby" class="citation">(Derrida, 1975)</a>

구성에서 이 클래스를 사용자 정의할 수 있습니다:

    scholar:
      cite_class: citation

### 파일 저장소

파일 저장소 지원은 Jekyll-Scholar 2.0 버전부터 추가되었습니다. 현재, 사이트에 논문의 PDF 또는 Postscript 파일이 포함된 폴더가 있는 경우, `repository` 구성 옵션을 사용하여 이 디렉터리를 지정할 수 있습니다. 참고 문헌을 생성할 때, Jekyll-Scholar는 해당 폴더에서 각 항목의 BibTeX 키와 일치하는 파일이 있는지 확인합니다. 파일이 있으면, 해당 파일의 경로가 참고 문헌 템플릿에 `link` 속성으로 노출됩니다.

버전 4.1.0부터 저장소는 PDF 및 PS 파일로 제한되지 않습니다. 이러한 파일은 참고 문헌 템플릿의 `links` 속성에 매핑됩니다. 지원 자료가 포함된 ZIP 아카이브에 링크하는 이 기능을 활용한 템플릿 예제는 다음과 같습니다:

    {{ reference }} [<a href="{{links.zip}}">Supporting Materials</a>]

버전 5.9.0부터 Jekyll-Scholar는 BibTeX 키로 시작하고 즉시 구분자가 뒤따르는 파일을 일치시킵니다(기본값: "."). 구분자 이후의 모든 텍스트는 파일 확장자로 처리됩니다. 예를 들어, `key.pdf` 및 `key.slides.pdf`라는 두 개의 파일이 발견되면, `{{links.pdf}}` 및 `{{links['slides.pdf']}}`가 모두 채워집니다. 기본 구분자를 변경하려면 `repository_file_delimiter` 구성 옵션을 사용할 수 있습니다.

### 상세 페이지

레이아웃 디렉터리에 참고 문헌 세부 사항에 대한 레이아웃 파일이 있는 경우(`details_layout` 구성 옵션), Jekyll-Scholar는 기본 참고 문헌의 각 항목에 대한 세부 사항 페이지를 생성합니다. 즉, 참고 문헌에 다음 항목이 포함된 경우:

    @book{ruby,
      title     = {The Ruby Programming Language},
      author    = {Flanagan, David and Matsumoto, Yukihiro},
      year      = {2008},
      publisher = {O'Reilly Media}
    }

'참고 문헌/ruby.html' 페이지는 세부 사항 페이지 레이아웃에 따라 생성됩니다. 레이아웃 파일에서는 BibTeX 항목의 모든 필드에 접근할 수 있습니다. 다음은 세부 사항 페이지 레이아웃의 예입니다:

    ---
    ---
    <html>
    <head></head>
    <body>
      <h1>{{ page.entry.title }}</h1>
      <h2>{{ page.entry.author }}</h2>
      <p>{{ page.entry.abstract }}</p>
    </body>
    </html>

Jekyll-Scholar가 세부 사항 페이지를 생성할 때, 생성된 참고 문헌에 각 항목의 세부 사항 페이지 링크도 추가합니다. 링크 이름은 `details_link` 구성 옵션을 통해 변경할 수 있습니다.

Jekyll-Scholar는 개별 세부 사항 페이지에 링크를 편리하게 추가하기 위한 Liquid 태그도 제공합니다. 예를 들어, 참고 문헌 항목 중 하나에 대한 간단한 링크를 페이지나 블로그 게시물에 추가하려면 `cite_details` 태그를 사용하여 링크를 생성할 수 있습니다. 이를 위해 인용하려는 항목의 BibTeX 키와 선택적으로 링크 텍스트를 태그에 전달합니다(기본 텍스트는 `details_link` 구성 옵션을 통해 설정할 수 있음).

    Duis 'aute irure dolor in reprehenderit in voluptate' velit esse cillum
    dolore eu fugiat nulla pariatur. Excepteur sint occaecat non
    proident {% cite_details key --text Click Here For More Details %}.

대안으로 `details_link` 태그를 사용하여 세부 사항 페이지로의 URL만 얻을 수 있습니다. 이는 Jekyll의 `link` 태그를 사용하여 마크다운에서 블로그 게시물에 링크하는 것과 동일한 방식으로 세부 사항 페이지에 링크하는 데 사용할 수 있습니다.

    [See our blog post]({% link _posts/2020-01-01-research-post.md %}) 
    or [find more details]({% details_link key %}).

### 참고 문헌 필터

기본적으로 Jekyll-Scholar는 참고 문헌을 생성할 때 메인 BibTeX 파일의 모든 항목을 포함합니다. 특정 기준과 일치하는 항목만 포함하려면, 'query' 구성 옵션을 조정할 수 있습니다. 예를 들어:

    query: "@book" #=> 책만 포함
    query: "@article[year>=2003]" #=> 2003년 이후 발표된 논문만 포함
    query: "@*[url]" #=> url 필드가 있는 모든 항목 포함
    query: "@*[status!=review]" #=> 상태 필드가 'review'로 설정되지 않은 모든 항목 포함
    query: "@book[year <= 1900 && author ^= Poe]" #=> 1900년 이전에 출판된 책 중 저자가 /Poe/와 일치하는 항목 포함
    query: "!@book" #=> 책이 아닌 모든 항목 포함

이 쿼리 중 일부는 BibTeX-Ruby 2.3.0 이상 버전이 필요할 수 있습니다. 위에서 설명한 것처럼 각 참고 문헌 태그에서 개별적으로 구성의 쿼리 매개변수를 덮어쓸 수도 있습니다.

기여
----

Jekyll-Scholar 소스 코드는 [GitHub에 호스팅](http://github.com/inukshuk/jekyll-scholar/)됩니다.
최신 코드를 Git을 사용하여 확인할 수 있습니다:

    $ git clone https://github.com/inukshuk/jekyll-scholar.git

RubyGems에서 제공하는 버전 대신 이 최신 버전을 사용하려면, 다음 줄을 `_plugins/ext.rb`에서 'jekyll/scholar'을 요구하기 전에 추가하세요:

    $:.unshift '/full/path/to/the/repository/lib'

여기서 `/full/path/to/the/repository`는 Jekyll-Scholar의 로컬 버전 경로입니다.

Jekyll-Scholar에 기여할 때는 모든 종속성을 설치하고 cucumber 기능을 실행하는 것을 잊지 마세요:

    $ bundle install
    $ rake

버그를 발견했거나 질문이 있는 경우, [Jekyll-Scholar 이슈 트래커](http://github.com/inukshuk/jekyll-scholar/issues)에 이슈를 등록하세요.
또는, 추가로, Jekyll-Scholar 저장소를 클론하고, 실패하는 예제를 작성하고, 버그를 수정하고, 풀 리퀘스트를 제출하세요.

또한, 하나 이상의 풀 리퀘스트가 병합된 경우, 원하시면 저장소에 대한 쓰기 권한을 드립니다.

라이선스
-------

Jekyll-Scholar는 Jekyll과 동일한 라이선스로 배포됩니다.

저작권 (c) 2011-2015 [Sylvester Keil](http://sylvester.keil.or.at/)

본 소프트웨어 및 관련 문서 파일('소프트웨어')의 사본을 취득한 모든 사람에게 무료로 사용, 복사, 수정, 병합, 출판, 배포, 서브라이선스 및/또는 소프트웨어의 사본을 판매할 수 있는 권한을 포함하여, 소프트웨어를 제한 없이 취급할 수 있는 권한을 부여합니다. 단, 다음 조건을 충족해야 합니다:

위의 저작권 고지 및 이 허가 고지는 소프트웨어의 모든 사본 또는 주요 부분에 포함되어야 합니다.

소프트웨어는 '있는 그대로' 제공되며, 명시적이든 묵시적이든 상품성, 특정 목적에 대한 적합성 및 비침해에 대한 보증을 포함하되 이에 국한되지 않는 어떠한 종류의 보증도 제공하지 않습니다. 저자 또는 저작권 보유자는 소프트웨어 또는 소프트웨어의 사용 또는 기타 거래와 관련하여 발생하는 어떠한 청구, 손해 또는 기타 책임에 대해서도 책임을 지지 않습니다.
