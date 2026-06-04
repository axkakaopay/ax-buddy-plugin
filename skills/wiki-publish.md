# wiki-publish 스킬

/ax-publish 커맨드에서 호출된다.
인터뷰 데이터를 Confluence Storage Format으로 변환하여 페이지를 생성한다.

---

## 페이지 기본 정보

- 페이지 이름: {업무명}_{작성자}
- 상위 페이지 ID: 1310721 (업무 프로세스 모음)
- 스페이스 키: ~712020e972b87f01b94a62935b2fcc758be805

---

## 페이지 구조

아래 순서로 3개 섹션을 구성한다.

### 섹션 1. 업무 프로세스 워크플로우

각 업무 단계를 순서대로 표현한다.

**단계별 구성 요소**
- 단계 번호 + 단계명
- 단계 설명 (1줄)
- 사용 도구 배지 (아래 매핑 참고)
- Pain Point가 있는 단계에는 ⚠ 배지 추가

**도구 이모지 매핑**
| 도구 | 이모지 |
|---|---|
| 슬랙 | 💬 |
| 지라 | 📋 |
| Confluence (위키) | 📄 |
| 이메일 | 📧 |
| 엑셀 / 구글 시트 | 📊 |
| PDF | 📑 |
| 아지트 | 📢 |
| GWS (캘린더 / 구글 문서) | 📅 |
| 기타 / 없음 | 🔲 |

---

### 섹션 2. 업무 정의서

아래 항목을 표로 구성한다.

| 항목 | 내용 |
|---|---|
| 업무명 | 인터뷰에서 확정된 업무명 |
| 목적 | 인터뷰 Q1에서 수집 |
| 주기 | 인터뷰 Q2에서 수집 |
| Pain Point | 인터뷰 Q3에서 수집 (반복 작업, 휴먼 에러 포함) |
| AX 전환 요약 | 인터뷰 전체 내용 기반으로 Claude가 핵심 자동화 포인트 1~2줄 요약 |

---

### 섹션 3. AI 활용 포인트

인터뷰에서 파악한 도구와 반복 작업을 기반으로 MCP 연동형 / 파일 처리형으로 구분한다.

**MCP 연동형** — 슬랙, 지라, Confluence, 아지트, GWS 등 MCP로 연결 가능한 도구

| 도구 | 자동화 포인트 | 기대 효과 | 실행 방법 |
|---|---|---|---|
| (도구) | (자동화할 수 있는 구체적 작업) | (줄어드는 리소스 또는 제거되는 문제) | MCP 연동 → [MCP 연동 가이드](https://ax-kakaopay.atlassian.net/wiki/spaces/~712020e972b87f01b94a62935b2fcc758be805/pages/1015810/MCP) |

**파일 처리형** — 엑셀, CSV, PDF 등 파일 기반 작업

| 도구 | 자동화 포인트 | 기대 효과 | 실행 방법 |
|---|---|---|---|
| (도구) | (자동화할 수 있는 구체적 작업) | (줄어드는 리소스 또는 제거되는 문제) | 파일을 Claude에 첨부 후 요청 |

해당하는 도구가 없는 분류는 섹션을 생략한다.

---

## Confluence Storage Format 변환 규칙

페이지 전체를 아래 XML 구조로 변환하여 create_confluence_page 호출 시 content 값으로 사용한다.

```xml
<!-- 목차 -->
<ac:structured-macro ac:name="toc">
  <ac:parameter ac:name="maxLevel">2</ac:parameter>
  <ac:parameter ac:name="minLevel">2</ac:parameter>
</ac:structured-macro>

<!-- 섹션 1 -->
<h2>1. 업무 프로세스 워크플로우</h2>

<!-- 단계별 반복 구조 -->
<p><strong>1단계. {단계명}</strong></p>
<p>{단계 설명}</p>
<p>{도구 이모지} {도구명} &nbsp; (Pain Point가 있으면) ⚠ {Pain Point 내용}</p>
<hr/>

<!-- 섹션 2 -->
<h2>2. 업무 정의서</h2>
<table>
  <tbody>
    <tr><th>업무명</th><td>{업무명}</td></tr>
    <tr><th>목적</th><td>{목적}</td></tr>
    <tr><th>주기</th><td>{주기}</td></tr>
    <tr><th>Pain Point</th><td>{Pain Point}</td></tr>
    <tr><th>AX 전환 요약</th><td>{AX 전환 요약}</td></tr>
  </tbody>
</table>

<!-- 섹션 3 -->
<h2>3. AI 활용 포인트</h2>

<h3>MCP 연동형</h3>
<table>
  <tbody>
    <tr><th>도구</th><th>자동화 포인트</th><th>기대 효과</th><th>실행 방법</th></tr>
    <tr>
      <td>{도구 이모지} {도구명}</td>
      <td>{자동화 포인트}</td>
      <td>{기대 효과}</td>
      <td>MCP 연동 → <a href="https://ax-kakaopay.atlassian.net/wiki/spaces/~712020e972b87f01b94a62935b2fcc758be805/pages/1015810/MCP">MCP 연동 가이드</a></td>
    </tr>
  </tbody>
</table>

<h3>파일 처리형</h3>
<table>
  <tbody>
    <tr><th>도구</th><th>자동화 포인트</th><th>기대 효과</th><th>실행 방법</th></tr>
    <tr>
      <td>{도구 이모지} {도구명}</td>
      <td>{자동화 포인트}</td>
      <td>{기대 효과}</td>
      <td>파일을 Claude에 첨부 후 요청</td>
    </tr>
  </tbody>
</table>
```

---

## 주의사항

- 특수문자(&, <, > 등)는 XML 이스케이프 처리한다. (&amp; &lt; &gt;)
- 도구가 MCP 연동형과 파일 처리형 모두 해당하면 각 섹션에 모두 포함한다.
- 해당 분류에 도구가 없으면 해당 섹션(h3 포함)을 생략한다.
- AX 전환 요약은 인터뷰 내용 기반으로 Claude가 직접 작성한다. 1~2줄로 핵심만 요약한다.
