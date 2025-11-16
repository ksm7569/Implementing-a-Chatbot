📌 Sequence-to-Sequence 기반 한글 챗봇
LSTM Encoder–Decoder + SentencePiece + Beam Search

이 프로젝트는 Sequence-to-Sequence(Seq2Seq) 구조를 기반으로
한국어 Q/A 데이터셋을 학습하여 자연스러운 “대화형 문장”을 생성하는 한글 챗봇입니다.
사용자의 질문을 이해하고 그에 맞는 답변을 생성하는 전체 파이프라인을 포함합니다.

⭐ 1. 프로젝트 개요
한국어 Q/A 데이터(ChatbotData.csv)를 활용한 챗봇 학습

SentencePiece(Unigram) 기반 서브워드 토크나이징

LSTM Encoder–Decoder 구조로 Seq2Seq 대화 모델 구현

Beam Search 기반 문장 생성 알고리즘 적용

콘솔 기반 실시간 대화 기능 제공

이 노트북은
전처리 → 토크나이저 학습 → 모델 학습 → 디코딩 → 실제 대화 실행
까지 모두 포함한 End-to-End 챗봇 프로젝트입니다.

⭐ 2. Seq2Seq란 무엇인가?
Seq2Seq는

“하나의 문장을 입력받아 그 의미를 이해하고, 새로운 문장으로 변환하는 기술”입니다.

PDF에서 설명된 구성요소는 아래 두 가지입니다:

✔ Encoder
입력 문장을 읽고 의미를 압축하여 context vector 생성

사람으로 치면 “귀 + 뇌 역할”

입력 토큰 → 임베딩 → LSTM → 문장 의미 요약

✔ Decoder
Encoder가 만든 context vector를 기반으로
<sos>(시작)부터 <eos>(끝)까지 단어를 하나씩 생성

사람으로 치면 “입 역할”

PDF에서 다룬 개념:

토큰화

정수 인코딩

임베딩

컨텍스트 벡터

Softmax 기반 단어 선택

반복하여 문장 완성

⭐ 4. 데이터 처리
✔ 텍스트 정제(clean_text)
한글/영문/숫자/공백만 유지

특수문자 제거

중복 공백 제거

양쪽 공백 제거

✔ SentencePiece(Unigram)
vocab_size: 8000

character_coverage: 0.9995

특수 토큰: <pad>, <sos>, <eos>, <unk>

토큰 충돌을 피하기 위해 ID offset = 4 적용

SentencePiece의 원리는 PDF의
“단어 → 숫자 → 임베딩 → 의미 반영”
단계와 동일합니다.

⭐ 5. 모델 구조 (LSTM Seq2Seq)
✔ Encoder
Embedding 레이어

Multi-layer LSTM 구조

PackedSequence로 variable-length 문장 처리

마지막 hidden, cell → 디코더 초기 상태

✔ Decoder
Multi-layer LSTMCell

Teacher forcing 적용

step-by-step 단어 생성

Linear → vocabulary 확률 분포

✔ Weight Tying
decoder embedding weight = generator weight

파라미터 감소 + 성능 향상

PDF에서 설명된
<시작> → 디코더 → Softmax → 단어 선택 → 반복 → <끝>
과정이 동일하게 구현되어 있습니다.

⭐ 6. 학습 방식
Loss: Label Smoothing CrossEntropy

Optimizer: Adam

Scheduler: NoamOpt (Transformer warmup 스타일)

Regularization:

Dropout

UNK token dropout

Gradient clipping

데이터 분할
Train: 85%

Validation: 15%

⭐ 7. 문장 생성 (Beam Search)
챗봇은 Greedy 방식이 아니라 Beam Search로 문장을 생성합니다.

적용된 기법:

beam_size = 4

length normalization (α = 0.6)

no-repeat n-gram = 3

repetition penalty 적용

<eos> 토큰 기준 문장 종료

이 과정을 통해
PDF에서 보여준 Softmax 기반 단어 선택 + 반복 생성 로직을
더 안정적이고 자연스럽게 확장한 방식으로 문장을 생성합니다.

⭐ 8. 실행 예시
shell
코드 복사
> 안녕하세요
반갑습니다. 무엇을 도와드릴까요?

> 내일 날씨 어때?
확실하진 않지만 내일은 괜찮을 것 같아요.

> 고민이 있어
듣고 있어요. 어떤 고민인가요?

> /quit
>
> ⭐ 10. 한계점 및 개선 방향
Attention 미적용 → 문맥 이해 부족

단일 턴 대화만 가능

데이터셋이 작아 답변 다양성 낮음

추후 Transformer 기반 확장 가능

Pretrained 모델 기반 fine-tuning으로 성능 향상 가능
