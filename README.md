# Fiction-DBDriven-Story-Chatbot-sample

1) 사전 구현 기능 - 픽션 DB화
- 웹소설, 소설 DB 등 텍스트 기반 소설과 스토리를 읽고 인물, 사건, 오브젝트 등을 LLM 요약과 함께 DB 형태로 생성합니다.
  <img width="1333" height="834" alt="1" src="https://github.com/user-attachments/assets/d9b1cb69-1b32-4a46-85b1-585bf84c5dfc" />

정규화 및 아래의 과정을 포함한 버튼형 DB화 구현 (일부 소설을 일부 직접 타이핑한 연구 샘플로 테스트됨)  
- chunk_text.py (텍스트 청크)
- build_storydb_pipeline.py (txt 스캔,  파일명파싱, story_id 생성, raw 병합, manifest 작성)
- build_relation_cards.py (entity_cards + story chunks(enriched)  기반 relation cards 생성. optional 로컬 LLM 보강 가능 (Qwen으로 테스트됨))
- build_event_cards.py (entity_cards + relation_cards + story chunks 기반 event cards 생성)
- refine_event_cards.py (event cards 정제/병합/이중분류 및 로컬 LLM 보강))
- query_context_builder.py (질문에맞는entity/relation/event/evidence 를 모아 query context 생성)

2) 포터블 파일의 주요기능 (몇몇 소설을 일부 직접 타이핑한 연구 샘플로 테스트됨) 

<img width="1333" height="834" alt="1" src="https://github.com/user-attachments/assets/c764f53b-7f73-4aa2-a44d-86c1c818ba67" />

- DB 내 주요 질문 (A와 B는 서로 연인 관계인가? 등 소설 내 사실관계), DB 내 인물/사건/객체 기반 창작 요청, 창작된 내용에 대한 근거 추론 및 작성 요청 가능
- 로컬 LLM, API 기반 LLM (openAI와 Gemini로 테스트됨), 하드로직 기반 응답 설정 가능
- 최종 응답 외 에이전트 협업에 따른 응답 변화 관측(초안 vs 최종본), 응답 서술을 위해 참조한 DB 근거 (DB근거), 원본카드 확인(원본 JSON) 가능
- API Key 미입력, 오프라인 상황에 따른 API 미연결 시 현재 세팅 대비 제공자(에이전트)의 오류 상태 추적 가능
- 세션 변경 및 삭제, 내보내기 가능


  <img width="1333" height="752" alt="2" src="https://github.com/user-attachments/assets/00238388-7009-4559-aa4d-7dc07c5db064" />

- 세션 내 창작 및 요청된 내용을 메모리 에이전트가 전문적으로 요약 관리하여 긴 창작 맥락에서도 핵심 내용 유지 가능
- 응답에 반영할 메모리와 메모리에 종속된 카드 조절 가능 (세션이 길어지면 매 응답마다 메모리 검색이 늘어나므로 토큰 절약용 기능)

  <img width="1333" height="752" alt="3" src="https://github.com/user-attachments/assets/21f94a49-bf4d-45af-8428-f41c8b966a1c" />

- 에이전트 모델 변경 가능, 로컬 LLM (Qwen 3.5, gemma3, gemma4로 테스트됨)
- 지휘자(요청을 정리해 DB관리자에게 넘기고 창작자와 메모리 관리자가 넘겨준 현재 메모리나 질문자 취향에 맞춰 윤문하여 응답, 창작에이전트 존중형인 초안존중형부터 초안을 극적으로 바꾸는 파격형 등 프리셋 지원, 프롬프트 수정 가능)
- DB관리자(요청에 맞는 검색 결과를 전체 DB 중 골라 창작자와 지휘자에게 제공, 로직 기반 제공 가능)
- 창작자(DB 근거와 지휘자 요청에 따라 응답을 서술하거나 소설을 창작)
- 메모리 관리자(창작된 결과물을 요약하여 세션 내 요청과 맥락을 관리)

<img width="1264" height="789" alt="image" src="https://github.com/user-attachments/assets/ade1b687-3450-40cf-9eb2-4aa1f2cf77a2" />
<img width="869" height="299" alt="image" src="https://github.com/user-attachments/assets/5d3dae4f-92d5-4bee-824c-2a8857a31619" />


- 응답에 할당된 카드를 슬롯-JSON 형태로 확인 가능
- 시리즈 권호가 있는 경우 아카이브폴더가 분리되어 있으면 특정 권만 지정하거나 전권 지정 가능
- 상위 관련 검토 카드 지정 가능 (많아질수록 느려지거나 토큰 수 증가, 대다수의 경우 15개로 지정해도 무난한 성능)


포터블 파일 요청 및 기타 문의는 별도 연락


