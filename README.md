**# Sequence-to-Sequence 기반 한글 챗봇**

LSTM Encoder–Decoder + SentencePiece + Beam Search

이 프로젝트는 Sequence-to-Sequence(Seq2Seq) 구조를 기반으로
한국어 Q/A 데이터셋을 학습하여 자연스러운 대화형 문장을 생성하는 한글 챗봇입니다.
사용자의 질문을 이해하고 그에 맞는 답변을 생성하는 전체 파이프라인을 포함합니다.

**1. 프로젝트 개요**
한국어 Q/A 데이터(ChatbotData.csv)를 활용한 챗봇 학습

SentencePiece(Unigram) 기반 서브워드 토크나이징

LSTM Encoder–Decoder 구조로 Seq2Seq 대화 모델 구현

Beam Search 기반 문장 생성 알고리즘 적용

콘솔 기반 실시간 대화 기능 제공

이 노트북은
전처리 → 토크나이저 학습 → 모델 학습 → 디코딩 → 실제 대화 실행
까지 모두 포함한 End-to-End 챗봇 프로젝트입니다.

**2. 모델 구조 (LSTM Seq2Seq)**
- Encoder
Embedding 레이어, Multi-layer LSTM, PackedSequence로 variable-length 문장 처리, 마지막 hidden, cell → 디코더 초기 상태

- Decoder
Multi-layer LSTMCell, Teacher forcing 적용, Step-by-step 단어 생성, Linear → vocabulary 확률 분포

- Weight Tying
decoder embedding 가중치를 generator 가중치와 공유

파라미터 감소 + 성능 향상
시작 → 디코더 → Softmax → 단어 선택 → 반복 → 끝

**3. 학습 방식**
Loss: Label Smoothing CrossEntropy

Optimizer: Adam
Scheduler: NoamOpt
Regularization:
Dropout
UNK token dropout
Gradient clipping

**4. 한계점 및 개선 방향**
Attention, Transport 미적용 → 문맥 이해 부족
-> 이후 구현하도록 노력
단일 턴 대화만 가능
-> 학습방식에 대해 좀 더 알아보고 구현되도록 노력
   
데이터셋이 작아 답변 다양성 낮음
-> 외부의 자료를 학습시켜 다양한 답변을 할 수 있도록 개선 구상


