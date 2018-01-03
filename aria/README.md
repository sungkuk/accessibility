# 레진 WAI-ARIA 가이드라인

[WAI-ARIA](https://www.w3.org/TR/wai-aria-1.1/)는 HTML의 접근성 문제를 보완하는 W3C 명세입니다. WAI-ARIA는 HTML 요소에 `role` 또는 `aria-*` 속성을 추가하여 콘텐츠의 '역할(roles), 상태(states), 속성(properties)' 정보를 보조 기기에 제공합니다.



1. [HTML을 의미있게 작성한다.](#1-html-)
2. [탭 목록, 탭, 탭 패널(`role="tablist|tab|tabpanel"`).](#2-role-tablist-tab-tabpanel-)
3. [툴팁(`role="tooltip"`).](#3-role-tooltip-)
4. [알럿(`role="alert"`).](#4-role-alert-)
5. [알럿 대화상자(`role="alertdialog"`).](#5-role-alertdialog-)
6. [대화상자(`role="dialog"`).](#6-role-dialog-)
7. [탐색(`nav`, `role="navigation"`).](#7-nav-role-navigation-)
8. [보충(`aside`, `role="complementary"`).](#8-aside-role-complementary-)
9. [의미 없음(`role="none"`).](#9-role-none-)
10. [참고 문서](#-)



```
<!-- 레진엔터테인먼트에서 사용하고 있는 WAI-ARIA -->

<!-- 역할(roles) -->
<element role="tablist">
<element role="tab">
<element role="tabpanel">
<element role="tooltip">
<element role="alert">
<element role="alertdialog">
<element role="dialog">
<element role="navigation">
<element role="complementary">
<element role="none">

<!-- 상태(states) -->
<element aria-current="true|false|page|step|location|...">
<element aria-selected="true|false">
<element aria-haspopup="true|false|dialog|...">
<element aria-expanded="true|false">
<element aria-pressed="true|false|mixed">
<element aria-checked="true|false|mixed">

<!-- 속성(properties) -->
<element aria-controls="ID reference list">
<element aria-live="off|polite|assertive">
<element aria-labelledby="ID reference list">
<element aria-label="string">
<element aria-describedby="ID reference list">
```



## 1. HTML을 의미있게 작성한다.
대부분의 WAI-ARIA 명세는 HTML 요소와 속성을 흉내내는 것입니다. 올바른 HTML을 사용한다면 WAI-ARIA 사용을 최소화할 수 있습니다. WAI-ARIA를 사용하기에 앞서 HTML을 의미있게 사용했는지 충분히 검토합니다.

```
<!-- X -->
<a href="#" role="button">

<!-- O -->
<button type="button">
```
보조기기는 두 가지 예제를 모두 '버튼'으로 간주할 것입니다. 그러나 첫 번째 예제의 경우 브라우저는 문맥 메뉴를 통해 링크와 관련된 기능(새 탭에서 링크 열기, 링크 주소 복사 등)을 제공하게 되고 사용자를 혼란스럽게 합니다. 또한 첫 번째 예제에서 '버튼' 이라는 설명을 들은 보조기기 사용자는 '스페이스'키를 눌러 버튼 기능을 사용하려고 시도할 수 있습니다. 하지만 `a` 요소는 '엔터' 키 만으로 실행할 수 있습니다. `button` 요소는 '엔터' 키와 '스페이스' 키로 실행할 수 있기 때문에 `a` 요소로부터 '버튼' 이라는 설명을 전해들은 보조기기 사용자를 혼란스럽게 합니다. 결국 올바른 HTML의 선택은 사용자 경험과 접근성 측면에서 모두 중요합니다.



## 2. 탭 목록, 탭, 탭 패널(`role="tablist|tab|tabpanel"`).

탭은 스타일을 의미하는 것이 아니라 콘텐츠에 색인을 제공하는 구조(tablist, tab, tabpanel)를 의미합니다. 사이트 탐색 도구에 해당하는 요소는 `nav > h2 + ul` 또는 `aside > h2 + ul` 구조로 마크업 합니다.

```
<!-- O: 앵커 형식 탭 -->
<div class="weekly">
    <div role="tablist">
        <a id="mon-anchor" href="#mon" role="tab" aria-selected="true">월</a>
        <a id="tue-anchor" href="#tue" role="tab">화</a>
    </div>
    <div id="mon" tabindex="0" role="tabpanel" aria-labelledby="mon-anchor">
        월요일엔 빨간 장미를...
    </div>
    <div id="tue" tabindex="0" role="tabpanel" aria-labelledby="tue-anchor">
        화요일엔 노란 장미를...
    </div>
</div>

<!-- O: 버튼 형식 탭 -->
<div class="weekly">
    <div role="tablist">
        <button type="button" id="mon-anchor" aria-controls="mon" role="tab" aria-selected="true">월</button>
        <button type="button" id="tue-anchor" aria-controls="tue" role="tab">화</button>
    </div>
    <div id="mon" tabindex="0" role="tabpanel" aria-labelledby="mon-anchor">
        월요일엔 빨간 장미를...
    </div>
    <div id="tue" tabindex="0" role="tabpanel" aria-labelledby="tue-anchor">
        화요일엔 노란 장미를...
    </div>
</div>
```



## 3. 툴팁(`role="tooltip"`).

툴팁은 앵커 또는 폼 콘트롤 요소에 대한 참고용 콘텐츠입니다. 보통 마우스 오버 또는 키보드 초점을 받으면 표시하는 내용이지만 화면에 항상 표시할 수도 있습니다. 툴팁 요소에 `role="tooltip"` 속성으로 명시할 수 있습니다. 툴팁을 유발하는 앵커 또는 콘트롤에 `aria-describedby="ID reference list"` 속성을 명시하여 연결합니다.

```
<!-- O: 인풋 툴팁 -->
<label for="tel">전화번호</label>
<input id="tel" type="tel" aria-describedby="TIP-TEL">
<p id="TIP-TEL" role="tooltip" hidden>하이픈(-) 없이 숫자만 입력.</p>

<!-- O: 버튼 툴팁 -->
<button aria-describedby="TIP-DEL">삭제</button>
<p id="TIP-DEL" role="tooltip" hidden>삭제 후 복원할 수 없음.</p>
```

`role="alert"` 또는 `role="alertdialog"` 또는 `role="dialog"` 콘텐츠와 혼동하지 않도록 유의합니다.



## 4. 알럿(`role="alert"`).

알럿은 일시적으로 민감한 정보를 사용자에게 전달하는 콘텐츠입니다. 운영체제 또는 브라우저에서 제공하는 시스템 알럿 대신 HTML 마크업으로 스타일 처리한 알럿을 제공할 수 있습니다.

초점을 받을 필요가 없는 알럿은 `role="alert"` 으로 처리합니다. 알럿 발생 시 보조 기기에 실시간으로 알림을 전달하려면 `aria-live="assertive"` 속성을 명시합니다.

```
<!-- O: 알럿 -->
<div role="alert" aria-live="assertive">
    <p>로그인 후 이용할 수 있습니다.</p>
</div>
```

초점을 받을 수 있는 사용자 인터렉션 요소를 포함하고 있다면 알럿 대화상자 `role="alertdialog"` 또는 대화상자 `role="dialog"`를 사용합니다.

사용자 입력 콘트롤(`input`, `textaria`)의 실시간 오류를 표시하는 경우라면 알럿 대신 콘트롤 요소에 `aria-invalid="true|false"` 속성과 `aria-errormessage="ID reference"` 속성을 사용합니다.



## 5. 알럿 대화상자(`role="alertdialog"`).

사용자 동의 또는 확인이 필요한 인터렉션 요소(`input`, `button`)를 포함한 상태로 다른 과업을 차단하는 경우 알럿 대화상자 `role="alertdialog"`를 사용합니다. 사용자 입력 없이 '확인, 취소' 버튼을 제공하는 경우에 적절합니다. 알럿 대화상자 발생 시 보조 기기에 실시간으로 알림을 전달하려면 `aria-live="assertive"` 속성을 명시합니다.

 알럿 대화상자에는 `aria-labelledby="ID reference list"` 그리고 `aria-describedby="ID reference list"` 속성으로 알럿 대화상자의 제목과 설명을 연결합니다.

 알럿 대화상자는 다른 과업을 차단해야 하기 때문에 모달 윈도우 스타일로 처리한 다음 `aria-modal="true"` 속성을 추가합니다.

 알럿 대화상자를 표시할 때 키보드 초점을 대화상자 내부 첫 번째 콘트롤(예를 들면 '확인' 버튼 또는 '인풋')으로 옮겨야 합니다. 알럿 대화상자를 표시하는 동안 초점은 대화상자 안에서 벗어나지 않아야 합니다.

```
<!-- O: 알럿 대화상자 -->
<div role="alertdialog" aria-live="assertive" aria-modal="true" aria-labelledby="TITLE" aria-describedby="DESCRIPTION">
    <h2 id="TITLE">레진패스 안내</h2>
    <p id="DESCRIPTION">이 작품의 유료 에피소드 열람 시 자동으로 구매합니다. 레진패스를 적용하시겠습니까?</p>
    <button type="button">레진패스 적용</button>
    <button type="button">취소</button>
</div>
```

사용자가 응답할 필요 없는 내용이라면 `role="alert"` 속성이 적절합니다. 사용자가 하위창 맥락으로 벗어나 정보를 입력(`input`, `textarea`, `select`, `button`)하는 경우라면 대화상자 `role="dialog"`가 적절합니다.



## 6. 대화상자(`role="dialog"`).

대화상자 `role="dialog"`는 사용자 인터렉션이 필요한 현재 문서의 하위창(마치 윈도우 팝업)입니다. 사용자가 정보를 입력하거나 응답하도록 하는 내용(`input`, `textarea`, `select`, `button`)을 반드시 포함합니다.

대화상자에는 `aria-labelledby="ID reference list"` 또는 `aria-label="string"` 속성으로 설명을 제공합니다.

대화상자를 표시할 때 키보드 초점을 대화상자 내부 첫 번째 콘트롤으로 옮겨야 합니다. 대화상자를 표시하는 동안 초점은 대화상자 안에서 벗어나지 않아야 합니다.

모달 윈도우 스타일로 표시할 것인지 여부는 선택 사항입니다. 모달 윈도우 스타일로 처리하는 경우 `aria-modal="true"` 속성을 추가합니다.

```
<!-- O: 대화상자 -->
<form role="dialog" aria-live="polite" aria-modal="true" aria-labelledby="TITLE">
    <h2 id="TITLE">로그인</h2>
    <label for="ID">아이디</label>
    <input id="ID">
    <label for="PW">비밀번호</label>
    <input id="PW" type="password">
    <button>로그인</button>
</form>
```

사용자의 다른 과업을 차단하면서 '확인, 취소' 버튼만 제공하는 경우라면 `role="alertdialog"` 속성이 적절합니다. 사용자가 응답할 필요 없는 내용이라면 `role="alert"` 속성이 적절합니다.

대중적인 브라우저가 `<dialog>` 요소를 충분히 지원하면 `role="dialog"` 속성 대신 `<dialog>` 요소를 사용하는 것이 바람직합니다.



## 7. 탐색(`nav`, `role="navigation"`).

탐색은 현재 페이지 또는 연결된 페이지를 탐색하는 주요 탐색 블록(보통 링크 집합)입니다. 문서의 '주요 내용'을 탐색하는 경우에 사용하면 적절합니다. 모든 링크 집합이 탐색 블록은 아닙니다.

탐색 블록에 적절한 HTML 요소는 `<nav>` 요소입니다. `role="navigation"` 속성을 사용하기 전에 `<nav>` 요소를 먼저 고려하는 것이 좋습니다.

탐색 역할을 하는 요소(`<nav>`, `role="navigation"`)가 문서 안에서 유일한 경우 레이블(`aria-labelledby`, `aria-label`) 제공은 선택입니다. 그러나 탐색 역할을 하는 요소가 둘 이상인 경우 고유한 레이블을 제공해야 합니다.

```
<!-- O: 탐색에 nav 요소를 권장 -->
<nav>
    <h2>글로벌 네비게이션<h2>
    ...
</nav>

<!-- O: 탐색 역할을 하는 요소가 유일한 경우 레이블 생략 가능 -->
<div role="navigation">
    <h2>글로벌 네비게이션<h2>
    ...
</div>

<!-- O: 탐색 역할이 둘 이상인 경우 레이블 제공(nav) -->
<nav aria-labelledby="global-navigation">
    <h2 id="global-navigation">글로벌 네비게이션<h2>
    ...
</nav>
<nav aria-labelledby="notice-pagination">
    <h3 id="notice-pagination">공지사항 페이지네이션<h3>
    ...
</nav>

<!-- O: 탐색 역할이 둘 이상인 경우 레이블 제공(role="navigation") -->
<div role="navigation" aria-labelledby="global-navigation">
    <h2 id="global-navigation">글로벌 네비게이션<h2>
    ...
</div>
<div role="navigation" aria-labelledby="notice-pagination">
    <h3 id="notice-pagination">공지사항 페이지네이션<h3>
    ...
</div>
```

`<nav>` 요소는 섹셔닝 콘텐츠이기 때문에 문서 개요(outline)를 생성합니다. 제목 없는 개요를 만들지 않기 위해 헤딩을 제공하는 것이 좋습니다. 레이블 요소(예를 들면 헤딩)가 있는 경우 `aria-labelledby` 속성으로 연결합니다. 레이블 요소(예를 들면 헤딩)가 없는 경우 `aria-label` 속성을 사용합니다.



## 8. 보충(`aside`, `role="complementary"`).

보충은 주요 내용을 보완하는 블록입니다. 문서의 '주요 내용'이 아닙니다. 보충을 제거해도 주요 내용에 변함이 없어야 합니다. 주요 내용에서 보충을 분리한 경우에도 보충은 나름의 의미가 있습니다.

보충으로 적절한 HTML 요소는 `<aside>` 요소입니다. `role="complementary"` 속성을 사용하기 전에 `<aside>` 요소를 먼저 고려하는 것이 좋습니다.

보충 역할을 하는 요소(`<aside>`, `role="complementary"`)가 문서 안에서 유일한 경우 레이블(`aria-labelledby`, `aria-label`) 제공은 선택입니다. 그러나 보충 역할을 하는 요소가 둘 이상인 경우 고유한 레이블을 제공해야 합니다.

```
<!-- O: 보충에 aside 요소를 권장 -->
<aside>
    <h2>배너/광고<h2>
    ...
</aside>

<!-- O: 보충 역할을 하는 요소가 유일한 경우 레이블 생략 가능 -->
<div role="complementary">
    <h2>배너/광고<h2>
    ...
</div>

<!-- O: 보충 역할이 둘 이상인 경우 레이블 제공(aside) -->
<aside aria-labelledby="event">
    <h2 id="event">이벤트<h2>
    ...
</aside>
<aside aria-labelledby="advertisement">
    <h2 id="advertisement">배너/광고<h2>
    ...
</aside>

<!-- O: 보충 역할이 둘 이상인 경우 레이블 제공(role="complementary") -->
<div role="complementary" aria-labelledby="event">
    <h2 id="event">이벤트<h2>
    ...
</div>
<div role="complementary" aria-labelledby="advertisement">
    <h2 id="advertisement">배너/광고<h2>
    ...
</div>
```

`<aside>` 요소는 섹셔닝 콘텐츠이기 때문에 문서 개요(outline)를 생성합니다. 제목 없는 개요를 만들지 않기 위해 헤딩을 제공하는 것이 좋습니다. 레이블 요소(예를 들면 헤딩)가 있는 경우 `aria-labelledby` 속성으로 연결합니다. 레이블 요소(예를 들면 헤딩)가 없는 경우 `aria-label` 속성을 사용합니다.



## 9. 의미 없음(`role="none"`).

의미 없음(`role="none"`)을 선언하는 경우 보조 기기는 마크업의 의미를 제거 후 내용만 사용자에게 전달합니다. `role="none"` 속성은 `role="presentation"`과 동일하며 `role="presentation"`을 대체합니다.

HTML을 의미에 맞지 않게 마크업한 경우, 또는 스타일링에 필요한 마크업을 추가한 경우 `role="none"` 속성을 사용할 수 있습니다. 이 속성은 절제해야 합니다.

```
<!-- O: tablist와 tab 사이 li 요소의 의미 제거 -->
<ul role="tablist">
    <li role="none">
        <a href="#home" role="tab" aria-selected="true">Home</a>
    </li>
    <li role="none">
        <a href="#ongoing" role="tab">Ongoing</a>
    </li>
    <li role="none">
        <a href="#ranking" role="tab">Ranking</a>
    </li>
</ul>
```

의미 없음(`role="none"`)은 숨김(`hidden`, `aria-hidden="true"`) 속성과 다릅니다. 숨김 속성은 요소와 내용을 모두 감추어 버리지만 `role="none"` 속성은 내용을 드러내고 의미만 감춥니다.



## 참고 문서

* [WAI-ARIA 1.1](https://www.w3.org/TR/wai-aria/)
* [Using ARIA](https://www.w3.org/TR/using-aria/)
* [ARIA in HTML](https://www.w3.org/TR/html-aria/)
* [WAI-ARIA Authoring Practices 1.1](https://www.w3.org/TR/wai-aria-practices/)
* [ARIA Landmarks Example](https://www.w3.org/TR/wai-aria-practices/examples/landmarks/)