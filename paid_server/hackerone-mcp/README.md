# HackerOne-MCP
[![Python](https://img.shields.io/badge/Python-3.x-blue.svg)](https://www.python.org/)

HackerOne API를 사용하여 버그 바운티 프로그램, 공개된 보고서(Hacktivity), 범위(Scope) 등 다양한 데이터를 조회하고 분석하는 파이썬 프로젝트입니다.

이 프로젝트는 `FastMCP`를 사용하여 HackerOne의 데이터 조회 기능들을 외부에서 호출 가능한 도구(Tool)로 제공합니다.

## Overview
**1) 나의 report 열람**
![hackerone-001](https://github.com/user-attachments/assets/7842855b-f355-4583-8bba-96ec270ba64e)

**2) 버그바운티 대상 정보 조회**
![hackerone-002](https://github.com/user-attachments/assets/0611886c-7763-4f66-8def-927a45f6f1e0)

**3) 공개 취약점 조회**
![hackerone-003](https://github.com/user-attachments/assets/338c0957-dfb4-451e-b57b-e3e0a9311b90)

**4) 취약점 상세 조회**
![hackerone-004](https://github.com/user-attachments/assets/21fc9cca-ff70-44d4-abb5-e7bbcd400f6e)


## 📋 주요 기능

* **🔎 프로그램 정보 조회**: HackerOne에 등록된 전체 프로그램 목록 또는 특정 프로그램의 상세 정보를 조회합니다.
* **🎯 범위(Scope) 분석**: 각 프로그램의 자산(Asset) 정보를 유형별로 필터링하고, 현상금 지급 대상 여부 등을 확인할 수 있습니다.
* **📈 Hacktivity 조회**: 공개적으로 보고된 최신 보안 취약점 리포트 목록을 가져옵니다.
* **🛡️ 보안 태세 분석**: 특정 프로그램의 정책과 범위를 종합적으로 분석하여 구조화된 데이터로 제공합니다.
* **📊 내 리포트 통계**: 인증된 사용자의 리포트 목록을 가져와 상태별, 프로그램별, 취약점 유형별 통계를 생성합니다.
* **⚙️ MCP 서버**: `FastMCP` 서버로 실행하여 다른 애플리케이션이나 서비스에서 데이터 조회 기능을 원격으로 호출할 수 있습니다.

***

## 🚀 시작하기

### 사전 요구사항

* Python 3.x 이상
* HackerOne API Credentials (Username & Token)

### 설치

1.  **GitHub 리포지토리 복제:**
    ```bash
    git clone <your-repository-url>
    cd <repository-name>
    ```

2.  **필요한 패키지 설치:**
    ```bash
    pip install requests fastmcp
    ```

3.  **HackerOne API 키 발급:**
    스크립트를 실행하려면 HackerOne 계정의 **Username**과 **API Token**이 필요합니다.
    * HackerOne에 로그인 후 [API 설정 페이지](https://docs.hackerone.com/en/articles/8544782-api-tokens)로 이동합니다.
    * `Create API Token` 버튼을 눌러 새 토큰을 생성하고, 생성된 **Token**과 본인의 **Username**을 기록해 둡니다.

***

## 🛠️ 사용 방법

### MCP 서버로 실행하기

이 프로젝트는 MCP 서버로 실행하여 등록된 기능들을 외부 도구처럼 사용하는 것을 권장합니다.

**클라이언트 설정 (`claude_desktop_config.json` 등)**

MCP 클라이언트(예: Claude Desktop App)에서 아래와 같이 서버 정보를 등록하여 연동할 수 있습니다. `env` 항목에 발급받은 HackerOne 자격 증명을 설정해야 합니다.

```json
{
  "mcpServers": {
     "hackerone": {
      "command": "/path/to/your/python3",
      "args": [
        "/path/to/your/hackerone-mcp/server.py"
      ],
      "env": {
        "HACKERONE_USERNAME": "YOUR_HACKERONE_USERNAME",
        "HACKERONE_TOKEN": "YOUR_HACKERONE_API_TOKEN"
      }
    }
  }
}
````

  * `hackerone`: 서비스 이름 (자유롭게 지정 가능)
  * `command`: 프로젝트의 파이썬 실행 파일 경로
  * `args`: 실행할 `server.py` 파일의 전체 경로
  * `env`: HackerOne API 인증을 위한 환경 변수. 여기에 자신의 Username과 Token을 입력합니다.

-----

## 📖 Tools (제공 기능)

### `get_programs_scope_details(program_handle)`

특정 또는 전체 HackerOne 프로그램의 범위(scope) 상세 정보를 조회합니다.

  * **매개변수:**
      * `program_handle` (str, 선택): 특정 프로그램의 핸들. 지정하지 않으면 모든 프로그램의 정보를 조회합니다.
  * **반환값:** `list` - 프로그램 범위 정보가 담긴 딕셔너리 리스트

### `get_asset_identifiers_by_type(asset_type, eligible_only)`

특정 자산 유형의 식별자(URL, DOMAIN 등) 목록을 추출합니다.

  * **매개변수:**
      * `asset_type` (str): 자산 유형 (기본값: `URL`). `DOMAIN`, `WILDCARD`, `OTHER` 등 사용 가능.
      * `eligible_only` (bool): 제출 가능한 자산만 포함할지 여부 (기본값: `True`).
  * **반환값:** `str` - 자산 식별자 목록이 담긴 JSON 형식의 문자열

### `get_hacktivity_reports(limit)`

HackerOne Hacktivity의 최신 공개 보고서를 조회합니다.

  * **매개변수:**
      * `limit` (int): 조회할 보고서 수 (기본값: `20`).
  * **반환값:** `list` - Hacktivity 보고서 목록

### `analyze_program_security_posture(program_handle)`

특정 프로그램의 정책, 범위, 통계 등 보안 태세를 종합적으로 분석합니다.

  * **매개변수:**
      * `program_handle` (str): 분석할 프로그램의 핸들.
  * **반환값:** `dict` - 처리 및 분석된 프로그램 정보

### `get_report_details(report_id)`

특정 ID의 HackerOne 보고서 상세 정보를 조회합니다.

  * **매개변수:**
      * `report_id` (int): 조회할 보고서의 ID.
  * **반환값:** `dict` - 보고서 상세 정보

### `process_my_reports()`

API 키에 연결된 사용자의 모든 보고서를 조회하고 상태별, 프로그램별 등으로 통계를 생성합니다.

  * **매개변수:** 없음
  * **반환값:** `dict` - 사용자의 보고서 목록 및 분석 통계

-----

## ⚠️ 주의사항

  * 이 프로젝트는 HackerOne API를 기반으로 동작합니다. API의 사양이나 정책이 변경될 경우 스크래퍼가 정상적으로 동작하지 않을 수 있습니다.
  * HackerOne의 API 요청 제한(Rate Limit) 정책을 준수해야 하며, 과도한 요청은 계정 또는 IP가 일시적으로 차단되는 원인이 될 수 있습니다.
