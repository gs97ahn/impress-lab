# NVIDIA Jetson Nano에서 구현 가능한 음성인식

## NeMo Framework
##### reference: https://colab.research.google.com/github/NVIDIA/NeMo/blob/stable/tutorials/asr/ASR_with_NeMo.ipynb#scrollTo=YLln3U-IlRzg

### ASR은 무엇인가?
- ASR(Automatic Speech Recognition)은 말하는 언어를 자동으로 텍스트로 바꿔주는 문제(speech-to-text)를 해결하는 프로그램이다.
- 우리의 목적은 음성을 인식하는 과정(WAV file -> text)에서 단어 에러 비율(Word Error Rate, WER)을 최소화 하는것이다.
- 전통적인 방법으로 음성인식을 구현하는 방법
  - Pr(audio|transcript) * Pr(transcript)
- 새로운 방법
  - Neural model이 더 좋은 성과를 구현한다. 하지만 neural model은 따로 학습을 시켜야되기 때문에 하나의 model의 error는 전체적인 예측 실패로 이어진다.
- End-to-end architectures은 모든 모델들을 따로 학습시킬 필요없이 하나의 목적을 위해 모든 모델이 작동하기 때문에 매우 매력적이다.

### End-To-End ASR
- End-to-end model을 사용한다면 Pr(transcript|audio)를 바로 학습시킬 수 있다.
- 전에는 음성데이터에 timestamp를 사용하여 예측된 알파벳과 매칭한뒤 반복된 알파벳을 합체시켰다. (e.g. LLLLAAAATOOOP -> LAPTOP)
  - 하지만 이 방법은 문제점을 가지고 있다. (e.g. BOOK -> ?)

![Alignment example](https://raw.githubusercontent.com/NVIDIA/NeMo/stable/tutorials/asr/images/alignment_example.png)

- 최신의 end-to-end의 방법은 수동적인 정렬을 요구하지 않는다.
  - input-output은 날것의 음성과 텍스트이다.(추가적은 데이터 또는 레이블링이 필요없다.)
- 최근에 자주 사용되는 모델은 Connectionist Temporal Classification (CTC)와 Seuqence-To-Sequence with Attention이다.

### Connectionist Temporal Classification (CTC)
- 보통의 음성인식은 알파벳 A부터 Z, 숫자 0부터 9 등을 기대한다.
- CTC는 blank toekn("-")을 이용하여 정렬문제를 해결한다.
- CTC는 one token per time segment of speech를 활용하여 예측을 하지만 blank token을 활용하여 예측단계에서 붕괴를 할지말지 결정한다.
  - e.g. 하나의 오디오 파일을 세그먼트 T=11로 나눠서 BOO--OOO--KK이 예측되는 구간을 붕괴하여 BO-O-K으로 변형한뒤 blank token을 지워서 BOOK으로 바꿔준다.
- 매 time step마다 가장 좋은 token을 선택하는 메소드를 greedy decoding 또는 max decoding이라 한다.
- 역전달의 상실을 계산하기 위해 log 확률 모델을 사용하여 글을 생성해야된다.
  - log(Pr(transcript|audio))

### Sequence-To-Sequence with Attention
- CTC의 문제점은 time steps를 이용해서 예측할때 조건부로 독립되어있다.
  - 이러한 문제점 때문에 CTC 위에 언어 model을 얹으므로서 문제를 해결할 수 있다.
- 자주 사용되는 방법 중 하나는 sequence-to-sequence model with attention을 사용한는것이다.
- ASR을 위해 전형적으로 사용되는 seq2seq 모델은 bidirectional RNN encoder이다.
  - bidirectional RNN encoder는 음성의 순서를 timestep-by-timestep로 소모한뒤 attention-based decoder로 패스한다.

### 사용될 데이터(AN4)
- AN4 데이터셋은 글자와 숫자로된 데이터셋이고 Carnegie Mellon University에서 수집하고 공개하였다.
- AN4는 사람들이 주소, 이름, 전화번호 등의 철자를 말하고 그것을 음성으로 말한것을 녹음한것이다.
- 948번의 학습과 130의 음성 테스트를 한것으로 학습이 매우 빠르다.
