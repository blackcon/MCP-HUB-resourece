네, 알겠습니다. 보내주신 샘플 포맷을 참고하여 `reddit-mcp` 프로젝트의 `README.md` 파일을 다시 작성했습니다.

-----

# Reddit-MCP

[](https://www.python.org/)

Reddit의 데이터를 수집하는 파이썬 프로젝트입니다. 특정 서브레딧(subreddit)의 'hot' 게시물 목록을 조회하고, 각 게시물의 제목, URL, 점수, flair 등 주요 정보를 구조화된 JSON 형식으로 가져올 수 있습니다.

이 프로젝트는 `FastMCP`를 사용하여 데이터 수집 기능을 외부에서 호출 가능한 도구(Tool)로 제공합니다.

## 📋 주요 기능

  * **📈 인기 게시물 수집**: 지정한 서브레딧의 'hot' 게시물을 원하는 개수만큼 수집합니다.
  * **📄 구조화된 데이터**: 수집된 각 게시물 정보를 사용하기 쉬운 JSON 객체로 변환하여 제공합니다.
  * **⚙️ MCP 서버**: `FastMCP` 서버로 실행하여 다른 애플리케이션이나 서비스에서 Reddit 데이터 수집 기능을 원격으로 호출할 수 있습니다.

-----

## 🚀 시작하기

### 사전 요구사항

  * Python 3.x 이상
  * Reddit API Key (`CLIENT_ID`, `CLIENT_SECRET`)

### 설치

1.  **GitHub 리포지토리 복제:**

    ```bash
    git clone <your-repository-url>
    cd <repository-name>
    ```

2.  **필요한 패키지 설치:**

    ```bash
    pip install praw fastmcp
    ```

3.  **Reddit API 키 설정:**
    스크립트를 실행하려면 Reddit API 키가 필요합니다. [Reddit 앱 생성 페이지](https://www.reddit.com/prefs/apps)에서 `script` 타입의 앱을 생성한 뒤, 발급받은 `CLIENT_ID`와 `CLIENT_SECRET`을 `server.py` 파일에 입력하세요.

    ```python
    # server.py
    CLIENT_ID = "YOUR_CLIENT_ID"
    CLIENT_SECRET = "YOUR_CLIENT_SECRET"
    USER_AGENT = "script:my_reddit_crawler:v1.0 (by /u/YOUR_REDDIT_USERNAME)"
    ```

-----

## 🛠️ 사용 방법

### MCP 서버로 실행하기

`mcp.run()` 코드를 통해 스크립트를 `FastMCP` 서버로 실행하면, 등록된 `crawl_subreddit` 함수를 외부 도구처럼 사용할 수 있습니다.

**클라이언트 설정 (`claude_desktop_config.json`)**

MCP 클라이언트(예: Claude Desktop App)에서 아래와 같이 서버 정보를 등록하여 연동할 수 있습니다.

```json
{
  "mcpServers": {
    "reddit-mcp": {
      "command": "/path/to/your/python3",
      "args": [
        "/path/to/your/server.py"
      ]
    }
  }
}
```

  * `reddit-mcp`: 서비스 이름 (자유롭게 지정 가능)
  * `command`: 프로젝트의 파이썬 실행 파일 경로
  * `args`: 실행할 `server.py` 파일의 전체 경로

-----

## 📖 Tools (제공 기능)

### `crawl_subreddit(subreddit_name, post_limit)`

지정한 서브레딧에서 인기(hot) 게시물을 수집합니다.

  * **매개변수:**

      * `subreddit_name` (str): 수집할 서브레딧의 이름 (예: "python", "kpop", "news")
      * `post_limit` (int): 수집할 게시물의 최대 개수

  * **반환값:** `str`

      * 각 게시물 정보가 담긴 JSON 객체들이 개행 문자(`\n`)로 구분된 단일 문자열을 반환합니다.
      * **반환 예시 (각 줄은 아래와 같은 JSON 형식):**

    <!-- end list -->

    ```json
    {
        "title": "A Great Day for Science: New Discoveries Announced",
        "url": "https://www.reddit.com/r/science/comments/xxxxxx/a_great_day_for_science_new_discoveries/",
        "score": 12345,
        "date": 1720878736.0,
        "flair": "Physics"
    }
    ```

    ```json
    {
        "title": "How to learn Python effectively in 2025?",
        "url": "https://www.reddit.com/r/Python/comments/yyyyyy/how_to_learn_python_effectively_in_2025/",
        "score": 5678,
        "date": 1720875136.0,
        "flair": "Discussion"
    }
    ```

-----

## ⚠️ 주의사항

  * 이 프로젝트는 `praw` 라이브러리를 통해 Reddit API를 사용합니다. Reddit의 API 정책이 변경될 경우, 기능이 정상적으로 동작하지 않을 수 있습니다.
  * Reddit의 API 사용 정책을 준수해야 하며, 과도한 요청은 계정 제한으로 이어질 수 있으니 주의하시기 바랍니다.
