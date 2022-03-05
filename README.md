# IMPRESS 연구실 (Intelligent Multimedia ProcESSing Lab)
## 음성인식 인공지능 관련 연구


### 강화학습 세미나
강화학습에 전반적인 내용을 세미나 형식으로 발표한 과제입니다.

### [NVIDIA NeMo Framework로 가능한 음성인식](https://github.com/gs97ahn/impress_lab/blob/main/NVIDIA_NeMo_Framework%EB%A1%9C_%EA%B0%80%EB%8A%A5%ED%95%9C_%EC%9D%8C%EC%84%B1%EC%9D%B8%EC%8B%9D.md)
NVIDIA NeMo Framework로 가능한 음성인식 정리본입니다.

### [Speech Recording with Wake Word](https://github.com/gs97ahn/impress_lab/blob/main/speech_recording_with_wake_word.ipynb)
NVIDIA Jetson Nano로 구현 가능한 speech recording with wake word입니다.
- wake word는 pvporcupine를 사용
- speech recording은 pyaudio를 사용

### [Detect and Record Audio](https://github.com/gs97ahn/impress_lab/blob/main/detect_and_record_audio.ipynb)
NVIDIA Jetson Nano로 구현 가능한 detect and record audio입니다.
- pyaudio.paInt16 포맷의 음성
- THRESHOLD가 3000 이상일 경우에만 녹음

### [Urban Sound Classification MFCC 2 Class](https://github.com/gs97ahn/impress_lab/blob/main/urban_sound_classification_mfcc_2class.ipynb)
MFCC Feature Extraction을 사용하여 Sound Classification이 가능한 모델 구현 코드입니다.
- MFCC
- optimizer: Adam
- Loss: Categorical crossentropy

### [Urban Sound Classification Mel-Spectrogram 2 Class](https://github.com/gs97ahn/impress_lab/blob/main/urban_sound_classification_mel-spectrogram_2class.ipynb)
Mel-Spectrogram Feature Extraction을 사용하여 Sound Classification이 가능한 모델 구현 코드입니다.
- Mel-Spectrogram
- optimizer: Adam
- Loss: Categorical crossentropy
