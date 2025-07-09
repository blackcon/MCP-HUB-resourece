# ApplyHome-MCP
[](https://www.python.org/)

`applyhome.co.kr` (청약홈) 웹사이트의 APT 분양 정보를 스크래핑하는 파이썬 프로젝트입니다. 주택명 또는 지역별로 분양 목록을 검색하고, 특정 공고의 상세 정보를 구조화된 JSON 형식으로 가져올 수 있습니다.

이 프로젝트는 `FastMCP`를 사용하여 스크래핑 기능들을 외부에서 호출 가능한 도구(Tool)로 제공합니다.

## Overview
<img width="770" alt="스크린샷 2025-07-08 오전 1 27 03" src="https://github.com/user-attachments/assets/5607cb0e-72a6-4295-ba9b-e5b05509d9b5" />
<img width="780" alt="스크린샷 2025-07-08 오전 1 27 25" src="https://github.com/user-attachments/assets/e7c347a8-ad71-47c4-9965-a773f2fb36bd" />
<img width="759" alt="스크린샷 2025-07-08 오전 1 27 36" src="https://github.com/user-attachments/assets/675b02e1-f2ae-4e63-b182-2d438d593198" />


## 📋 주요 기능

  * **🏡 분양 목록 조회**: 주택명과 지역 코드를 이용해 최근 6개월간의 분양 공고 목록을 조회합니다.
  * **📄 상세 정보 조회**: 각 공고의 고유 번호(주택관리번호, 공고번호)를 사용해 청약일정, 공급대상, 공급금액 등 상세 정보를 파싱합니다.
  * **🤖 자동화된 데이터**: 복잡한 HTML 구조를 파싱하여 사용하기 쉬운 JSON 객체로 변환합니다.
  * **⚙️ MCP 서버**: `FastMCP` 서버로 실행하여 다른 애플리케이션이나 서비스에서 스크래핑 기능을 원격으로 호출할 수 있습니다.


-----

## 🚀 시작하기

### 사전 요구사항

  * Python 3.x 이상
  * 필요한 라이브러리는 `requirements.txt`에 명시되어 있습니다.

### 설치

1.  **GitHub 리포지토리 복제:**

    ```bash
    git clone [your-repository-url]
    cd [repository-name]
    ```

2.  **필요한 패키지 설치:**

    ```bash
    pip install requests beautifulsoup4 FastMCP
    ```

-----

## 🛠️ 사용 방법

이 프로젝트는 두 가지 방식으로 실행할 수 있습니다.

### 1\. MCP 서버로 실행하기 (권장)

`mcp.run()` 코드를 통해 스크립트를 `FastMCP` 서버로 실행하여, 등록된 `search_house`와 `get_house_details` 함수를 외부 도구처럼 사용할 수 있습니다.

**클라이언트 설정 (`claude_desktop_config.json`)**

MCP 클라이언트(예: Claude Desktop App)에서 아래와 같이 서버 정보를 등록하여 연동할 수 있습니다.

```json
{
  "mcpServers": {
    "applyhome": {
      "command": "/path/to/your/python3",
      "args": [
        "/path/to/your/server.py"
      ]
    }
  }
}
```

  * `applyhome`: 서비스 이름 (자유롭게 지정 가능)
  * `command`: 프로젝트의 파이썬 실행 파일 경로
  * `args`: 실행할 `server.py` 파일의 전체 경로

### 2\. 스크립트로 직접 실행하기

`if __name__ == "__main__":` 블록의 주석을 해제하여 특정 기능을 테스트할 수 있습니다.

```python
if __name__ == "__main__":
    # 예제 1: '자이' 브랜드 아파트 목록 검색
    search_results = search_house("자이")
    if search_results:
        print("--- 🏠 분양 목록 검색 결과 ---")
        print(json.dumps(search_results[0], indent=2, ensure_ascii=False))

        # 예제 2: 검색된 첫 번째 아파트의 상세 정보 조회
        first_house = search_results[0]
        hmno = first_house.get("hmno")
        pbno = first_house.get("pbno")
        
        if hmno and pbno:
            details = get_house_details(hmno, pbno)
            if details:
                print("\n--- 📋 상세 정보 조회 결과 ---")
                print(json.dumps(details, indent=2, ensure_ascii=False))
```
-----

## 📖 API (제공 함수)

### `search_house(house_name, suplyAreaCode)`

최근 6개월 내의 APT 분양 공고 목록을 검색합니다.

  * **매개변수:**

      * `house_name` (str): 검색할 주택명 (예: "자이", "푸르지오")
      * `suplyAreaCode` (str, 선택): 지역 코드. 기본값은 전국(`''`).
          * **지역 코드**: `강원`, `경기`, `경남`, `경북`, `광주`, `대구`, `대전`, `부산`, `서울`, `세종`, `울산`, `인천`, `전남`, `전북`, `제주`, `충남`, `충북`

  * **반환값:** `list`

      - 각 아파트의 요약 정보가 담긴 딕셔너리 리스트를 반환합니다.
      - **반환 예시:**

    <!-- end list -->

    ```json
    [
      {
        "시도": "경기",
        "주택구분": "민영",
        "분양_임대": "분양주택",
        "주택명": "해링턴플레이스 풍무(3BL)",
        "건설업체": "효성중공업 주식회사",
        "문의처": "☎ 1877-9877",
        "모집공고일": "2025-07-04",
        "청약기간": "2025-07-14 ~ 2025-07-16",
        "당첨자발표일": "2025-07-22",
        "pbno": "2025000274",
        "hmno": "2025000274"
      }
    ]
    ```

### `get_house_details(house_manage_no, pblanc_no)`

`search_house`를 통해 얻은 고유 번호로 특정 공고의 상세 정보를 조회합니다.

  * **매개변수:**

      * `house_manage_no` (int): 주택관리번호 (`hmno`)
      * `pblanc_no` (int): 공고번호 (`pbno`)

  * **반환값:** `dict`

      - 기본정보, 청약일정, 공급대상, 공급금액 등 상세 정보가 담긴 딕셔너리를 반환합니다.
      - **반환 예시:**

    <!-- end list -->

    ```json
    {
      "주택명": "시흥 은계 에피트",
      "기본정보": {
        "공급위치": "경기도 시흥시 은행동 289-31번지 일원",
        "공급규모": "30세대",
        "문의처": "☎ 031-318-4998",
        "모집공고문": "https://static.applyhome.co.kr/..."
      },
      "청약일정": {
        "모집공고일": "2025-06-27",
        "당첨자발표일": "2025-07-16",
        "계약일": "2025-07-28 ~ 2025-07-30",
        "입주예정월": "2026.10"
      },
      "공급대상": [
        {
          "주택구분": "민영",
          "주택형": "039.1570",
          "주택공급면적": "54.8990",
          "세대수_계": "30",
          "주택관리번호": "2025000254(01)"
        }
      ],
      "공급금액": [
        {
          "주택형": "039.1570",
          "공급금액_최고가_만원": "30,977"
        }
      ],
      "기타사항": {
        "시행사": "신극동아파트 가로주택정비사업조합",
        "시공사": "에이치앨디앤아이한라 주식회사"
      }
    }
    ```
-----

## ⚠️ 주의사항

  * 이 프로젝트는 `applyhome.co.kr` 웹사이트의 HTML 구조를 기반으로 동작합니다. 웹사이트의 구조가 변경될 경우 스크래퍼가 정상적으로 동작하지 않을 수 있으며, 코드 수정이 필요할 수 있습니다.
  * 과도한 요청은 대상 웹사이트에 부하를 줄 수 있으니 적절한 간격을 두고 사용하시기 바랍니다.
