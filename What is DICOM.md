# DICOM
**Digital Imaging and Communications in Medicine(DICOM)** 은 의료 영상 정보 및 관련 데이터의 통신 및 관리를 위한 표준이다. DICOM은 여러 제조 업체의 
스캐너, 서버, 워크스테이션, 프린터, 네트워크 하드웨어 그리고 PACS(Picture Archiving and Communication System)와 같은 의료 영상을 통합하고
저장하는데 사용된다.


DICOM 파일은 DICOM 형식으로 영상 및 환자 데이터를 수신할 수 있는 두개의 개체 간에 교환 될 수 있다. 서로 다른 장치에는 지원하는 DICOM 클래스를
명확하게 표시한 DICOM conformance Statements가 제공되며 표준으로 파일 형식 정의와 TCP/IP를 사용하여 시스템 간에 통신하는 네트워크 통신 프로토콜이
포함된다.


## Applications
DICOM은 의료 영상을 저장, 교환 및 전송하는데 전 세계적으로 사용된다. DICOM은 방사선 촬영, 초음파, 컴퓨터 단층 촬영(CT), 자기 공명 영상(MRI)및 방사선 
치료와 같은 영상 촬영장비에 대한 표준을 포함하고 있다. 또한 영상 교환 프로토콜과 영상 압축, 3D시각화, 영상 표시 및 결과보고를 포함하고 있다.

DICOM 
* 포함 : 영상 교환 프로토콜과 영상 압축, 3D시각화, 영상 표시 및 결과보고

## Data format
DICOM은 정보(영상, 환자ID, 환자의 증상 등)을 데이터 세트로 그룹화한다. 그 이유는 실수로 영상이 정보와 분리되지 않도록 하기 위함이다.
이런 방식은 JPEG와 같은 이미지 형식에 이미지를 식별하고 설명하기 위한 태그가 내장 되어 있는 방식과 유사하다. 

#### 예시
* 흉부 X선 영상 파일 : 영상 파일 내에(픽셀 정보 외에도) 환자 ID가 포함되는 경우도 있음

DICOM data object는 이름, ID등의 항목을 포함한 여러 속성과 영상 픽셀 데이터를 포함한 하나의 speical attribute로 구성된다. 즉 주 객체에 
픽셀 데이터만 존재하고 "header"는 존재하지 않는다.

대게 단일 이미지이지만, 속성중에는 여러 개의 프레임이 포함 되는 경우(cine loops 또는 다중 프레임 데이터)는 다중 이미지를 저장할 수 있다.

* i.e) NM data : NM영상은 다차원 멀티 프레임 영상이다.이 경우 3차원 또는 4차원 데이터를 단일 DICOM개체에 캡슐화 할 수 있다.

* 픽셀 데이터는 다양한 표준으로 압축할 수 있다. (JPEG, lossless JPEG, JPEG 2000, RLE ...)

또한 DICOM은 세가지 서로 다른 데이터 요소 인코딩 체계를 사용한다. 

* 세가지 인코딩 체계 : GROUP(2 bytes) ELEMENT (2 bytes) VR (2 bytes)

## Image display

동일한 그레이 스케일 이미지를 여러 모니터에서 보여주는 것과, 다양한 프린터에서 똑같은 하드 카피 이미지를 제공하기 위해 DICOM committee는  lookup
table(조회 테이블)을 개발했다.

**DICOM grayscale standard display function(GSDF)** 를 사용하기 위해선, lookup curve를 가진 장치 또는 GSDF curve로 calibrated 된 장치에서 보거나
인쇄해야 한다. 

* lookup table : 디지털로 지정된 픽셀 값을 표시하는 테이블
 
 
## Value representaitions
---------

##참고자료

[DICOM 설명(by wiki)](https://en.wikipedia.org/wiki/DICOM)

[DICOM 설명(slideshare)](https://slidesplayer.org/slide/12934045/)

[CT 3D 촬영 원리](http://www.ucdenver.edu/academics/colleges/medicalschool/departments/Radiology/About%20Us/Faculty/education-research-portal/Pages/filtbackproj.aspx)
