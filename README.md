# 법률 비서 시스템 (RAG 기반 AI 법률 검색)
본 프로젝트는 RAG (검색 증강 생성) 기술을 기반으로 법률 전문가와 일반 사용자 모두가 법률 문서 및 정보를 효과적으로 검색하고 관련 답변을 받을 수 있도록 돕는 시스템입니다.       
대규모 언어 모델(LLM)과 검색 증강 생성시스템(RAG)을 결합하여 높은 정확성과 효율성을 제공합니다.       


접속주소 : wsrag.duckdns.oug:50000
도메인주소는 본인이 설정한 주소를 쓰셔야합니다. 해당 도메인은 24/12/13 까지 캡스톤 기간동안에만 하루에 3시간 오픈됩니다.
<br>
<br>
## 주요 기능       
### 법률 문서 검색 및 요약       
사용자가 입력한 질문에 적합한 법률 정보를 검색하고 요약된 답변을 생성합니다.       
       
### PDF 문서 처리       
PDF 문서를 로드하여 텍스트를 추출하고 이를 벡터화하여 빠른 검색이 가능하도록 구성하였습니다.       
       
### RAG 시스템 적용       
검색된 문서를 바탕으로 LLM이 자연스러운 답변을 생성하여 사용자에게 제공됩니다.       
       
### 웹 기반 사용자 인터페이스       
Flask를 활용한 웹 애플리케이션으로, 사용자 친화적인 질문 입력 및 답변 출력 환경을 제공합니다.       
       
<br>
<br>
<br>
<br>
<br>       
       
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
<br>
<br>
<br>
<br>
<br>

## 사용법
          
                 
리눅스 시스템 구동후 해당 프로젝트 파일 구조를 확인하여 app.py코드상에서 변경합니다.          
         
코드구조에 문제가 없다면 googlecolab 시스템에 접속하여 gpu모델을 t4로 설정해주고 본인의 ngrok 토큰을 입력하여 url을 제공받습니다.

### 코랩코드
```plaintext
!curl https://ollama.ai/install.sh | sh

!echo 'debconf debconf/frontend select Noninteractive' | sudo debconf-set-selections
!sudo apt-get update && sudo apt-get install -y cuda-drivers

!pip install pyngrok
#총 설치시간은 5분에서 7분정도 걸리는듯.






from pyngrok import ngrok

token='본인의 엔그록 토큰'
#사용자토큰.

ngrok.set_auth_token(token)








import os
import asyncio

# Set LD_LIBRARY_PATH so the system NVIDIA library
os.environ.update({'LD_LIBRARY_PATH': '/usr/lib64-nvidia'})
#엔비디아 그래픽카드 라이브러리 업데이트.

async def run_process(cmd):
  print('>>> starting', *cmd)
  p = await asyncio.subprocess.create_subprocess_exec(
      *cmd,
      stdout=asyncio.subprocess.PIPE,
      stderr=asyncio.subprocess.PIPE,
  )

  async def pipe(lines):
    async for line in lines:
      print(line.strip().decode('utf-8'))

  await asyncio.gather(
      pipe(p.stdout),
      pipe(p.stderr),
  )
from IPython.display import clear_output
clear_output()


await asyncio.gather(
run_process(['ollama', 'serve']),
# run_process(['ollama', 'pull', 'benedict/linkbricks-llama3.1-korean:8b']),
run_process(['ngrok', 'http', '--log', 'stderr', '11434', '--host-header="localhost:11434"']),
run_process(['ollama', 'pull', 'benedict/linkbricks-llama3.1-korean:8b'])
)
#ngrok은 11434포트를 사용함으로 해당포트 열기, 그리고 ollama에 필요한 모델 설치.
```





         
url을 제공받으면 코드에 해당 터널링 포인트의 url을 입력해줍니다.(주석으로 url 수정할곳이표시되어있습니다)
         
이후 저장하고 리눅스 터미널에서 "uwsgi --ini /home/user/Desktop/ML/app.ini" 를 실행합니다. 디렉토리 구조나 위치가 다르다면 경로수정 해주셔야합니다.
         
리눅스상에서는 본인이 app.ini파일에 설정한 포트로 내부주소를 통해 접속할 수 있고
         
외부로 접속하게 하기위해서는 무료 dns서비스를 사용하여 본인이 가지고있는 공유기의 포트포워딩 설정을 하여 접근하게 해주어야합니다.
         
관련 정보는 아무것도모르는 초보자라도 인터넷에서 1시간정도 포트포워딩에 대해 검색해서 돌아다니면 무조건 할수있습니다.
         
이후 본인이 설정한 dns 를 통해 접속할 수 있습니다.
         
본 프로젝트에서는 duckdns를 사용했고 주소는 wsrag.duckdns.org:50000을 사용했습니다.
         
포트를 지정한 이유는 개발환경이 고정ip가 사용 불가능한 환경이기에 동적ip를 사용했고 그로인해 공유기ip를 5분마다 duckdns서버에 갱신하면서 해당 ip에 들어오는 포트를 
         
라즈베리파이5의 내부할당 ip에 포트포워딩했습니다. 그로인해 포트를 따로 지정해주었습니다.
         
즉 1.1.1.1 이라는 공유기 ip에 접근하여 5000번 포트에 연결 요청을하면
         
공유기에서 포트포워딩 설정으로 1.1.1.1 ip에 5000 포트 요청이 오면 내부아이피
         
168.1.1.99 ip의 5000번 포트로 할당. (내부아이피는 매번 다름)
         
을 한것입니다.
         
그로인해 라즈베리파이 로컬주소5000포트에 열린 웹서버를 외부에서 접근 할 수 있습니다.
         
만약 자기는 돈이 많다~ 하시면 웹서버를 결제하시어 유로로 구동하시는게 제일 마음 편합니다.
         
전 ai공부는 커리큘럼에 하나도 없고 완전 맨땅에 해딩이라 결제하는데 좀 그랬고.. 컴퓨터 성능도 딸려서 결제하면 gpu시스템도 결제해야하는데 그것도 좀 그랬습니다.
         
돈이 없어서요. ai를 주제로 무언가 만들어보라고 한다면 api를 써서 완성했습니다! 할수는 없으니 컴퓨팅환경이라도 학교에서 지원해주거나 기업과 제휴를 맺어 싸게 해줘야하지 않나
         
<br>
<br>
         
완전 파이썬도 모르고 만들수 있는 확신도 없고 어거지로 하려다보니 헛돈 날리기보다는 무료로 구축해보자 해서 하게되었습니다. 라즈베리파이는 리눅스에서 서버 만드는게 좋다고해서
         
가상머신을 돌리기엔 너무 느려 중간에 사업단에 주문했습니다. 메카솔루션 엄청 비쌉니다. 시중가에 50%나 비싸게 주고 샀네요.
                                       
<br>
<br>
<br>
<br>
<br>
<br>
<br><br>
<br>
                        
               
               
               
                              
페이지 구동후 접속 화면입니다.(디자인 바뀌어서 아래 사진 더있습니다) 이미지파일은 직접 제작하여 넣으셔야합니다.
배경 디자인은 중요한 요소입니다.

<br>
<br>
<br>
<br>

페이지 디자인이 변경되었습니다.
![image](https://github.com/user-attachments/assets/c263034f-727b-473e-bdc2-341f9b36380b)
<br>
<br>

응답화면입니다.
![image](https://github.com/user-attachments/assets/ede5f8de-dbcc-4191-93d7-9d3b055b8919)



<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
웹 코드 및 디자인 전 사진입니다.

![image](https://github.com/user-attachments/assets/cf043d36-430b-47e4-83f6-9991f23579af)
          
<br>
<br>
<br>
<br>
          
질문 입력 방법:         
         
웹 페이지에서 법률 관련 질문을 입력합니다.              
예: "근로기준법에 따른 초과근무수당 기준은 무엇인가요?"             
          
<br>
<br>
<br>
<br>
<br>          
                    
                     
결과 확인 :      
          
기다리면 AI가 관련 법령 및 문서를 검색하고 요약된 답변을 제공합니다.       
만약 응답시간이 2분이상 초과한다면 새로고침 해주세요.          

<br>
<br>
<br>
<br>
<br>

## 기여 방법
본 프로젝트는 지속적인 개선을 목표로 합니다.           
버그 리포트, 기능 추가 제안, 코드 리뷰를 통해 기여할 수 있습니다.          

<br>
<br>
<br>
<br>
<br>

## 라이센스
MIT License
코드사용에 대한 모든 활동에 대해 작성자는 책임지지 않습니다.

<br>
<br>
<br>
## 24년 2학기 캡스톤2 팀원 , 문의
               
201710430 김민석               
202010735 배준영               

