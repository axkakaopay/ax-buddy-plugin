# ax-buddy-plugin

카카오페이 AX TF에서 만든 Claude Cowork 플러그인입니다.
업무 인터뷰를 통해 프로세스를 문서화하고, AI 활용 포인트를 도출하여 Confluence에 자동으로 게시합니다.

---

## 사용 방법

### 1단계. 플러그인 Import

Claude Cowork에서 아래 GitHub 저장소를 Import합니다.

```
https://github.com/ax-kakaopay/ax-buddy-plugin
```

> **GitHub 계정이 없어도 됩니다.**
> 아래 공용 계정으로 로그인하세요.
>
> - 아이디(이메일): ax.kakaopay@gmail.com
> - 비밀번호: 카카오페이!@34

### 2단계. 사전 준비

플러그인 Import 전에 아래 두 가지를 PC에 설치합니다.

1. **Node.js** 설치 → https://nodejs.org/en/download (LTS 버전)
2. **mcp-remote** 설치 → 터미널(Mac) 또는 명령 프롬프트(Windows)에서 실행

```bash
npm install -g mcp-remote
```

> 자세한 설치 방법은 [MCP 연동 가이드](https://ax-kakaopay.atlassian.net/wiki/spaces/~712020e972b87f01b94a62935b2fcc758be805/pages/1015810/MCP)를 참고하세요.

### 3단계. 업무 인터뷰 시작

Claude Cowork 채팅창에 아래 커맨드를 입력합니다.

```
/ax-start
```

Claude가 업무 인터뷰를 진행합니다. 편한 방식으로 업무를 설명하면 됩니다.

### 4단계. 위키 자동 게시

인터뷰가 끝나면 아래 커맨드를 입력합니다.

```
/ax-publish
```

산출물 3종(업무 프로세스 워크플로우, 업무 정의서, AI 활용 포인트)을 확인하고 게시하면
Confluence **업무 프로세스 모음** 페이지에 자동으로 등록됩니다.

---

## 연결 도구

플러그인 Import 시 아래 도구들이 자동으로 연결됩니다.

| 도구 | 용도 |
|---|---|
| Confluence (위키) | 업무 프로세스 페이지 자동 생성 |
| Jira | 이슈 조회 |
| Slack | 메시지 조회 |
| Agit | 공지 조회 |
| GWS | 캘린더 · 메일 |

---

## 문의

AX TF 슬랙 채널로 문의해주세요.
