# KRDS UI/UX 가이드라인 MCP 서버

이 MCP 서버는 [KRDS (Korean Regional Data System) 가이드라인](https://v04.krds.go.kr/guide/index.html)에서 제공하는 모든 UI/UX 컴포넌트와 디자인 패턴에 대한 정보를 제공합니다. 개발자와 디자이너가 정부 서비스를 구축할 때 일관된 사용자 경험을 제공할 수 있도록 도와줍니다.

## Overview

#### 1. yes24 리디자인 예시
- Cluade Code 입력 예시
  <img width="880" height="468" alt="image" src="https://github.com/user-attachments/assets/e2384216-8136-44c6-b806-976a2b348915" />
- 결과물
  <img width="1251" height="945" alt="image" src="https://github.com/user-attachments/assets/4d62b451-89d7-44c4-b978-ba59c96e677f" />

#### 2. NAVER 리디자인 예시
<img width="1323" height="928" alt="image" src="https://github.com/user-attachments/assets/9f0a13e9-ce60-4908-8921-d4db77453a26" />

#### 3. Youtube 리디자인 예시
<img width="701" height="930" alt="image" src="https://github.com/user-attachments/assets/502614a7-f169-4090-afb0-c436124893f6" />


### 🌟 핵심 가치
- **정부 디지털 서비스 통일성**: KRDS 가이드라인을 통한 일관된 UI/UX 제공
- **웹 접근성 우선**: 모든 국민이 접근할 수 있는 서비스 구축
- **개발 효율성**: 즉시 사용 가능한 컴포넌트와 템플릿 제공
- **품질 보장**: 정부 표준을 준수하는 고품질 코드 생성

### 🔧 기술 스택
- **Backend**: Python 3.8+
- **MCP Framework**: FastMCP
- **Accessibility**: WCAG 2.1 AA 준수
- **Code Generation**: HTML, CSS, JavaScript, React, Vue

### 📊 지원 범위
- **컴포넌트**: 40+ 개의 정부 서비스용 UI 컴포넌트
- **패턴**: 15+ 개의 서비스 패턴 및 기본 패턴
- **프레임워크**: HTML, React, Vue 지원
- **접근성**: 자동 검사 및 개선 제안

## 주요 기능

### 🧩 컴포넌트 지원
- **Identity 컴포넌트**: 공식 배너, 운영기관 식별자, 푸터, 헤더
- **Navigation 컴포넌트**: 건너뛰기 링크, 메인 메뉴, 브레드크럼, 사이드 메뉴
- **Layout 컴포넌트**: 구조화 목록, 긴급 공지, 달력, 모달, 아코디언, 탭, 표
- **Action 컴포넌트**: 링크, 버튼
- **Selection 컴포넌트**: 라디오 버튼, 체크박스, 셀렉트, 태그
- **Feedback 컴포넌트**: 단계 표시기, 스피너
- **Help 컴포넌트**: 도움 패널, 따라하기 패널, 맥락적 도움말, 코치마크
- **Input 컴포넌트**: 날짜 입력, 텍스트 영역, 텍스트 입력, 파일 업로드

### 🎨 디자인 패턴
- **기본 패턴**: 개인정보 입력, 입력폼, 목록 탐색, 피드백 등
- **서비스 패턴**: 방문, 검색, 로그인, 신청, 정보 확인

### 🛠️ 개발 도구
- **코드 생성**: HTML, CSS, JavaScript, React 컴포넌트 템플릿 제공
- **접근성 검사**: WCAG 2.1 기준 자동 검사 및 개선 제안
- **가이드라인 문서**: PDF 가이드라인 및 디자인 리소스 다운로드 링크

## 설치 및 실행

### 요구사항
- Python 3.8+
- pip

### 설치
```bash
# 저장소 클론
git clone <repository-url>
cd krds_ui_gidue_mcp

# 종속성 설치
pip install -r requirements.txt
```

### 실행
```bash
python server.py
```

### 클라이언트 설정

클라이언트 애플리케이션(Claude Desktop, Cursor 등)의 설정 파일에 다음 JSON 설정을 추가해야 합니다:

#### Claude Desktop의 경우:
`~/Library/Application Support/Claude/claude_desktop_config.json` 파일에 추가:

```json
{
  "mcpServers": {
    "krds-ui-guide": {
      "command": "python",
      "args": ["server.py"],
      "cwd": "/Users/user/tools/krds_ui_gidue_mcp"
    }
  }
}
```

#### Cursor의 경우:
Cursor 설정 파일에 동일한 JSON 구성을 추가합니다.

**주의사항:**
- `cwd` 경로는 실제 KRDS MCP 서버가 설치된 경로로 변경해야 합니다
- 설정 파일 수정 후 클라이언트 애플리케이션을 재시작해주세요

## MCP 도구 목록

### 1. `get_keyword_list`
컴포넌트 및 패턴 키워드 목록을 반환합니다.

**파라미터:**
- `category` (선택): 필터링할 카테고리

**예시:**
```python
# 모든 컴포넌트와 패턴 조회
get_keyword_list()

# 특정 카테고리 조회
get_keyword_list(category="아이덴티티")
```

### 2. `search_components`
키워드로 컴포넌트나 패턴을 검색합니다.

**파라미터:**
- `keyword`: 검색할 키워드

**예시:**
```python
search_components(keyword="버튼")
search_components(keyword="입력")
```

### 3. `get_component_details`
특정 컴포넌트의 상세 정보를 반환합니다.

**파라미터:**
- `component_name`: 컴포넌트 이름

**예시:**
```python
get_component_details(component_name="버튼")
get_component_details(component_name="헤더")
```

### 4. `get_component_code_examples`
특정 컴포넌트의 구현 코드 예시를 생성합니다.

**파라미터:**
- `component_name`: 컴포넌트 이름
- `code_type`: 코드 타입 ("html", "css", "javascript", "react")

**예시:**
```python
get_component_code_examples(component_name="버튼", code_type="html")
get_component_code_examples(component_name="모달", code_type="javascript")
```

### 5. `generate_component_template`
지정된 컴포넌트 타입에 대한 템플릿 코드를 생성합니다.

**파라미터:**
- `component_type`: 생성할 컴포넌트 타입
- `framework`: 사용할 프레임워크 ("html", "react", "vue")

**예시:**
```python
generate_component_template(component_type="button", framework="html")
generate_component_template(component_type="modal", framework="react")
```

### 6. `get_design_patterns`
디자인 패턴 정보를 반환합니다.

**파라미터:**
- `pattern_type`: 패턴 유형 ("basic", "service", "all")

**예시:**
```python
get_design_patterns(pattern_type="all")
get_design_patterns(pattern_type="service")
```

### 7. `validate_accessibility`
HTML 코드의 웹 접근성을 검사합니다.

**파라미터:**
- `html_code`: 검사할 HTML 코드

**예시:**
```python
html = "<button>클릭</button>"
validate_accessibility(html_code=html)
```

### 8. `get_accessibility_guidelines`
웹 접근성 관련 가이드라인을 반환합니다.

**예시:**
```python
get_accessibility_guidelines()
```

### 9. `download_guidelines`
KRDS 가이드라인 문서 다운로드 링크를 제공합니다.

**예시:**
```python
download_guidelines()
```

## 사용 예시

### 버튼 컴포넌트 생성
```python
# 버튼 컴포넌트 정보 조회
component_info = get_component_details("버튼")

# HTML 코드 예시 생성
html_code = get_component_code_examples("버튼", "html")

# React 템플릿 생성
react_template = generate_component_template("button", "react")

# 접근성 검사
accessibility_result = validate_accessibility(html_code["code"])
```

### 폼 패턴 구현
```python
# 개인정보 입력 패턴 조회
patterns = get_design_patterns("basic")

# 폼 컴포넌트 코드 생성
form_code = get_component_code_examples("폼", "html")

# 접근성 검사 및 개선
validation = validate_accessibility(form_code["code"])
```

## 특징

### ✅ 완전한 KRDS 가이드라인 커버리지
- 8개 컴포넌트 카테고리의 모든 요소 지원
- 기본 패턴과 서비스 패턴 완전 구현
- 실제 정부 서비스에서 사용 가능한 코드 예시

### ♿ 접근성 우선 설계
- WCAG 2.1 AA 기준 준수
- 자동 접근성 검사 도구 내장
- 스크린 리더 및 키보드 탐색 지원

### 🚀 개발자 친화적
- HTML, CSS, JavaScript, React 코드 자동 생성
- 즉시 사용 가능한 템플릿 제공
- 실제 구현에 필요한 모든 정보 포함

### 📱 반응형 디자인
- 모바일 우선 접근법
- 다양한 디바이스 지원
- 정부 서비스 특화 반응형 패턴

### 실제 생성 코드 예시

#### 버튼 컴포넌트 (HTML)
```html
<!-- KRDS 버튼 컴포넌트 -->
<button class="krds-btn krds-btn--primary" type="button">
    정부서비스 신청
</button>
```

#### 헤더 컴포넌트 (완전한 구현)
```html
<!-- KRDS 헤더 컴포넌트 -->
<header class="krds-header" role="banner">
    <div class="krds-masthead">
        <div class="krds-container">
            <span class="krds-masthead__text">대한민국 정부</span>
        </div>
    </div>
    <div class="krds-header__main">
        <!-- 헤더 콘텐츠 -->
    </div>
</header>
```

#### 접근성 개선 제안 예시
```json
{
    "accessibility_issues": [
        {
            "issue": "버튼에 aria-label이 없습니다",
            "suggestion": "aria-label=\"메뉴 열기\" 속성을 추가하세요",
            "severity": "medium"
        }
    ],
    "wcag_compliance": "AA",
    "improvements": 3
}
```

> **이미지 파일 위치**: 모든 스크린샷은 `images/` 폴더에 저장됩니다. 
> 실제 사용 시 해당 경로에 이미지 파일들을 추가해주세요.

## 관련 링크

- [KRDS 가이드라인 공식 사이트](https://v04.krds.go.kr/guide/index.html)
- [Model Context Protocol (MCP)](https://github.com/modelcontextprotocol/python-sdk)
- [FastMCP 프레임워크](https://github.com/jlowin/fastmcp)
- [WCAG 2.1 가이드라인](https://www.w3.org/WAI/WCAG21/quickref/)
