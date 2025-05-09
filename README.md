# 📂 파일별 역할 설명 
## main.py
* 프로젝트의 중심이 되는 실행 파일입니다.
	* 사용자의 음성을 녹음하고 (audio_recorder.py 호출)
	* Whisper STT 서버(app.py)를 통해 텍스트로 변환한 뒤
	* 감정 분석을 수행하여 (language_model.py 사용)
	*	LED 조명을 상황에 맞게 제어합니다 (led_controller.py).
	*	또한, 대답을 생성해 TTS로 응답을 들려주는 전체 흐름을 제어합니다.
## audio_recorder.py
	*	마이크를 통해 사용자의 음성을 녹음합니다.
	*	녹음된 파일은 .wav 형식으로 저장되며, 이후 STT 처리에 사용됩니다.
## weather_module.py
	*	사용자의 IP 주소를 통해 위치를 확인하고, 해당 지역의 날씨 정보를 OpenWeather API에서 받아옵니다.
	*	날씨 상태에 따라 시스템 반응을 달리할 수 있도록 지원합니다.
## language_model.py
	*	Whisper STT 결과를 바탕으로, 사용자의 질문/요청에 대해 LLM (예: LLaMA, GPT 기반)을 호출하여 적절한 응답을 생성합니다.
	*	TTS 전용 응답도 이 모듈에서 가공됩니다.
## led_controller.py
	*	RGB LED(네오픽셀 등)의 색상 제어를 담당합니다.
	*	감정 분석 결과나 날씨 상태 등을 반영해 LED의 색상을 변화시킵니다.
	*	전원 관리와 초기화, 정리(cleanup) 기능도 포함되어 있습니다.
## app.py
* Flask 기반의 로컬 서버 애플리케이션입니다.
  *	/speech-to-text 엔드포인트: Whisper로 음성 인식을 수행합니다.
	*	/generate-tts 엔드포인트: 텍스트를 음성으로 변환해 오디오를 생성합니다.
	*	UI 테스트용 index 라우팅도 포함되어 있으며, 외부에서 이 API를 호출해 STT 및 TTS 처리를 수행할 수 있습니다.
# 📌 흐름 설명
### 1.음성 녹음 단계
사용자가 말을 하면 audio_recorder.py에서 이를 녹음해 .wav로 저장합니다.
### 2.STT 변환 단계
main.py는 이 오디오 파일을 Flask 서버(app.py)의 /speech-to-text로 전송하여 텍스트로 변환합니다.
### 3.LLM 응답 생성 단계
텍스트가 생성되면 language_model.py가 이를 LLM 서버에 전달하여 대화 응답을 생성합니다.
### 4.감정 분석 및 LED 제어
응답 내용을 분석하여 감정을 추정하고, 그에 맞는 색상으로 LED를 제어합니다. (led_controller.py)
### 5.TTS 출력 단계
사용자의 음성으로 다시 출력되도록 app.py의 /generate-tts API를 호출해 음성을 생성하고, 스피커로 재생합니다.
### 6.날씨 활용 (선택 사항)
필요에 따라 weather_module.py를 통해 날씨를 감지하고, LED나 응답에 반영할 수 있습니다.
