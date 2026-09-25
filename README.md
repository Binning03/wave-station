# WaveStation

WaveHome 스마트홈 시스템에서 **실제 환경의 데이터를 수집하고 상위 서버와 연결하는 ESP32 기반 멀티센서 장치**입니다.

마이크, 스피커, IR 송수신기, 온·습도 및 조도 센서를 통합하고, 각 하드웨어를 제어하는 펌웨어와 서버 통신을 구현했습니다.

I2S 오디오 처리, TCP의 특징, ESP32의 제한된 메모리와 Task Stack 등을 고려해 펌웨어를 구성했습니다.

특히 오디오 데이터를 TCP로 수신하는 과정에서 사용하는 대형 버퍼를 정적 영역에 할당하여, ESP32의 제한된 메모리 환경에서도 안정적으로 동작하도록 구현했습니다.

## Features

- **Audio**
  - I2S 마이크 입력 / 스피커 출력
  - 16 kHz Mono PCM 스트리밍
  - 20 ms 단위 오디오 프레임 처리
  - Opus 압축 및 디코딩

- **Sensor & IR**
  - DHT22 온·습도 측정
  - BH1750 조도 측정
  - IR Raw Signal 송수신
  - 주기적 전송 및 변화 감지 기반 센서 전송

- **Network**
  - ESP32 TCP Server
  - TCP Stream Buffering 및 Packet Framing
  - Subscribe / Unsubscribe / Heartbeat / Ack / Error 처리
  - Sequence Number를 이용한 데이터 스트림 관리

## Architecture

```text
Sensors / Audio / IR
        │
        ▼
 Hardware Drivers
        │
        ▼
 ESP32 Firmware
        │
        ▼
    TCP/IP
        │
        ▼
 WaveHome Backend
```

## Structure

```text
esp32/
├── app.cpp              # Main application / network processing
├── i2s_mic.cpp          # I2S microphone
├── speaker_driver.cpp   # I2S speaker
├── opus_codec.cpp       # Opus codec
├── ir_driver.cpp        # IR Tx/Rx
├── sensor_driver.cpp    # Temperature / Humidity / Light
├── wsp_packet.cpp       # WaveStation Protocol (WSP) packet handling
└── external/wsp/wave_station.h       # WaveStation Protocol (WSP) protocol definition

test_backend/
└── wave_station_test_backend.py

wavestation_case/
└── 3D printed case files
```

## License
본 프로젝트는 외부 소프트웨어를 포함하거나 참조하고 있습니다.
각 외부 소프트웨어의 전체 라이선스 본문은 external/의 하위 경로에서 확인할 수 있습니다.