# 법률 비서 시스템 (RAG 기반 AI 법률 검색)
본 프로젝트는 RAG (검색 증강 생성) 기술을 기반으로 법률 전문가와 일반 사용자 모두가 법률 문서 및 정보를 효과적으로 검색하고 관련 답변을 받을 수 있도록 돕는 시스템입니다.
대규모 언어 모델(LLM)과 검색 증강 생성시스템(RAG)을 결합하여 높은 정확성과 효율성을 제공합니다.

## 주요 기능
### 법률 문서 검색 및 요약
사용자가 입력한 질문에 적합한 법률 정보를 검색하고 요약된 답변을 생성합니다.

### PDF 문서 처리
PDF 문서를 로드하여 텍스트를 추출하고 이를 벡터화하여 빠른 검색이 가능하도록 구성하였습니다.

### RAG 시스템 적용
검색된 문서를 바탕으로 LLM이 자연스러운 답변을 생성하여 사용자에게 제공됩니다.

### 웹 기반 사용자 인터페이스
Flask를 활용한 웹 애플리케이션으로, 사용자 친화적인 질문 입력 및 답변 출력 환경을 제공합니다.

