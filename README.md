#LSTM 기반 한글 챗봇 (Seq2Seq + SentencePiece + Beam Search)
이 프로젝트는 SentencePiece 기반 토크나이징과 LSTM Encoder–Decoder 모델을 이용해 구현한 한국어 Q/A 챗봇입니다.
사용자가 입력한 문장을 서브워드 단위로 분해하고, Seq2Seq 구조로 학습하여 자연스러운 답변을 생성합니다.
1. 프로젝트 개요
이 챗봇은 다음 과정을 통해 작동합니다:

한국어 Q/A 데이터(ChatbotData.csv) 학습

SentencePiece(Unigram) 기반 토크나이징

LSTM Encoder–Decoder 구성으로 시퀀스 생성

Beam Search 문장 생성 전략 적용

콘솔에서 실시간 대화 가능

노트북 하나에서 데이터 전처리 → SP 학습 → 모델 학습 → 생성 → 테스트까지 전체 파이프라인이 동작합니다.
