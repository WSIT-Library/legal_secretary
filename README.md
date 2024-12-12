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
       
       
       
## 프로젝트 구조
```plaintext
project/          
├── app.py                       # Flask 애플리케이션 메인 파일        
├── app.ini                      # uwsgi 웹서버 환경파일. uwsgi 설치후 터미널에 "uwsgi --ini /home/user/Desktop/ML/app.ini" 큰따옴표 안의 명령어로 해당 파일 실행하여 웹서버,시스템 구동.       
├── templates/                   # HTML 파일 (웹페이지 뷰)       
│   ├── index.html               # 메인 페이지       
├── static/                      # CSS 및 javascript 정적 파일       
│   ├── css/                     # css 디렉토리       
│   │   ├── style.css            # 기본css       
│   │   └── style_result.css     # 결과페이지css        
│   ├── js/                      # js 디렉토리       
│   │   ├── script.js            # 기본css         
│   │   └── script_re.js         # 결과페이지css      
├── vector_store/                # 벡터데이터베이스      
│   ├── chroma.sqlite3           # chroma벡터스토리지 파일
│   └── ff@@@@@@@-@@-@@-@/       # 임베딩된 벡터 저장 디렉토리  
├── rag_temp                     # rag 템플릿 텍스트 저장위치       
│   └── template.txt             # rag 템플릿 텍스트파일       
├── pdf_data/                    # PDF 문서 디렉토리       
│   └── testpdf.pdf              # PDF 파일       
└── requirements.txt             # 필수 패키지 목록 (구현 안되어있음 필요하면 작성해 사용할것.)       
```
