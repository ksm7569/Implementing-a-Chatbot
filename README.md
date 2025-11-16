#Sequence-to-Sequence 기반 한글 챗봇
LSTM Encoder–Decoder + SentencePiece + Beam Search
이 프로젝트는 Sequence-to-Sequence(Seq2Seq) 구조를 기반으로
한국어 Q/A 데이터셋을 학습하여 자연스러운 “대화형 문장”을 생성하는 한글 챗봇입니다.
사용자의 질문을 이해하고 그에 맞는 답변을 생성하는 전체 파이프라인을 포함합니다.

##1. 프로젝트 개요
한국어 Q/A 데이터(ChatbotData.csv)를 활용한 챗봇 학습

SentencePiece(Unigram) 기반 서브워드 토크나이징

LSTM Encoder–Decoder 구조로 Seq2Seq 대화 모델 구현

Beam Search 기반 문장 생성 알고리즘 적용

콘솔 기반 실시간 대화 기능 제공

이 노트북은
전처리 → 토크나이저 학습 → 모델 학습 → 디코딩 → 실제 대화 실행
까지 모두 포함한 End-to-End 챗봇 프로젝트입니다.

##2. 데이터 처리
✔ 텍스트 정제(clean_text)
한글/영문/숫자/공백만 유지

특수문자 제거

중복 공백 제거

양쪽 공백 제거

✔ SentencePiece(Unigram)
vocab_size: 8000

character_coverage: 0.9995

특수 토큰: <pad>, <sos>, <eos>, <unk>

토큰 충돌 방지: ID offset = 4 적용

SentencePiece의 원리는 PDF의
“단어 → 숫자 → 임베딩 → 의미 반영”
단계와 동일합니다.

##3. 모델 구조 (LSTM Seq2Seq)
✔ Encoder
Embedding 레이어

Multi-layer LSTM

PackedSequence로 variable-length 문장 처리

마지막 hidden, cell → 디코더 초기 상태

✔ Decoder
Multi-layer LSTMCell

Teacher forcing 적용

Step-by-step 단어 생성

Linear → vocabulary 확률 분포

✔ Weight Tying
decoder embedding 가중치를 generator 가중치와 공유

파라미터 감소 + 성능 향상

PDF에서 설명된
<시작> → 디코더 → Softmax → 단어 선택 → 반복 → <끝>
과정을 그대로 구현하고 있습니다.

##4. 학습 방식
Loss: Label Smoothing CrossEntropy

Optimizer: Adam

Scheduler: NoamOpt (Transformer warmup 스타일)

Regularization:

Dropout

UNK token dropout

Gradient clipping

✔ 데이터 분할
Train: 85%

Validation: 15%

##5. 문장 생성 (Beam Search)
Greedy 방식이 아닌 Beam Search로 문장을 생성합니다.

✔ 적용된 기법
beam_size = 4

length normalization (α = 0.6)

no-repeat n-gram = 3

repetition penalty 적용

<eos> 기준 문장 종료

이 과정은 PDF에서 설명된
Softmax 기반 단어 선택 + 반복 생성
로직을 더 안정적이고 자연스럽게 확장한 방식입니다.


##6. 한계점 및 개선 방향
Attention 미적용 → 문맥 이해 부족

단일 턴 대화만 가능

데이터셋이 작아 답변 다양성 낮음

Transformer 기반 확장 가능

Pretrained 모델 기반 fine-tuning으로 성능 향상 가능
