# DICOM
**Digital Imaging and Communications in Medicine(DICOM)** 은 의료 영상 정보 및 관련 데이터의 통신 및 관리를 위한 표준이다. DICOM은 여러 제조 업체의 
스캐너, 서버, 워크스테이션, 프린터, 네트워크 하드웨어 그리고 PACS(Picture Archiving and Communication System)와 같은 의료 영상을 통합하고
저장하는데 사용된다.


DICOM 파일은 DICOM 형식으로 영상 및 환자 데이터를 수신할 수 있는 두개의 개체(entities) 간에 교환 될 수 있다. 서로 다른 장치에는 지원하는 DICOM 클래스를
명확하게 표시한 DICOM conformance Statements가 제공되며 표준으로 파일 형식 정의와 TCP/IP를 사용하여 시스템 간에 통신하는 네트워크 통신 프로토콜이
포함된다.


## Applications
DICOM은 의료 영상을 저장, 교환 및 전송하는데 전 세계적으로 사용된다. DICOM은 방사선 촬영, 초음파, 컴퓨터 단층 촬영(CT), 자기 공명 영상(MRI)및 방사선 
치료와 같은 영상 촬영장비에 대한 표준을 포함하고 있다. 또한 영상 교환 프로토콜과 영상 압축, 3D시각화, 영상 표시 및 결과보고를 포함하고 있다.

DICOM 
* 포함 : 영상 교환 프로토콜과 영상 압축, 3D시각화, 영상 표시 및 결과보고

## Data format

<img src="https://github.com/dongseoklee1541/DICOM/blob/master/image/iod.png?raw=true" height="400" width="400">

DICOM은 정보(영상, 환자ID, 환자의 증상 등)을 데이터 세트로 그룹화한다. 그 이유는 실수로 영상이 정보와 분리되지 않도록 하기 위함이다.
이런 방식은 JPEG와 같은 이미지 형식에 이미지를 식별하고 설명하기 위한 태그가 내장 되어 있는 방식과 유사하다. 

* 흉부 X선 영상 파일 : 영상 파일 내에(픽셀 정보 외에도) 환자 ID가 포함되는 경우도 있음

DICOM data object는 이름, ID등의 항목을 포함한 여러 속성과 영상 픽셀 데이터를 포함한 하나의 speical attribute로 구성된다. 즉 주 객체에 
픽셀 데이터만 존재하고 "header"는 존재하지 않는다.

<img src="https://github.com/dongseoklee1541/DICOM/blob/master/image/153156.png?raw=true" height="400" width="800">


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
 
 
## Value representaitions(VR)

VR에는 각 속성에 포함 된 데이터 요소 수를 나타내는 value mulitplicity 가 있다. string 값 표현의 경우 둘 이상의 데이터가 인코딩 되는 경우
연속으로 들어오는 데이터는 "\" 문자로 구분된다.

## Services

DICOM은 대부분 네트워크를 통한 데이터 전송을 하는 서비스로 구성된다.

### Store

DICOM Store service는 영상(images) 또는 영구 객체(structured reports, etc.) 등을 PACS 또는 워크스테이션으로 보내는것에 사용된다.

### Storage commitment

DICOM Storage commitment service는 영상이 중복 디스크나 시디에 백업을 하여 영구적으로 저장되었는지 확인하는 데 사용된다. Service Class User
(SCU : client 와 비슷)가 Service Class Provider(SCP: server와 비슷)이 확인을 근거로 로컬에서 영상을 삭제해도 되는지 아닌지 판단한다.

### Query/retrieve

워크 스테이션은 communication system(통신 시스템)에서 이미지 또는 다른 객체를 찾고 가져올 수 있다.

### Modality worklist

DICOM modality worklist service(modality system)은 영상 촬영시 정해진 작업 목록을 제공한다. 세부 목록으로 
* subject of the procedure: patient ID, name, sex, and age
* type of procedure : equipment type, procedure description, procedure code
* procedure order : referring physician, accession number, reason for exam

### Modality performed procedure step(MPPS)

modality workist의 보조 서비스다. 이 기능은 획득한 영상, 시작 시간, 종료시간, 스터디의 지속시간 등에 대한 데이터를 포함한 검사에 대한 보고서를 전송할 수 있다. 방사선 부서가 더 resoruce를 정확하게 다룰 수 있게 한다. 

MPPS 를 사용하면, 전송하기 전이나 전송하는 동안 전송할 객체 목록을 저장 서버에 제공하여 촬영장비가 image storage server와 더 협업하기 좋아진다.

### Print

DICOM print service는 DICOM printer(주로 "X-RAY")에 영상을 전송하는데 주로 사용된다. 여러 디스플레이 장치와 하드 카피 인쇄물 장치 간의 일관성을 보장하기 위한 표준 보정(Standard Calibration)이 있다.

### Off-line media (files)

오프라인 미디어 파일의 형식은 DICOM Standard part 10에 지정되어 있고, 이러한 파일을 "Part 10 files"라고 한다.

DICOM은 DICOM media의 **파일이름을 8 characters** 로 제한한다. 이 이름에서 정보를 추출하면 안된다.

media의 모든 DICOM파일에 대한 인덱스 및 요약 정보를 제공하는 media directory인 DICOMDIR파일이 있어야 한다. DICOMDIR이 더 많은 정보를 제공하므로
파일 이름에 의미를 만들 필요가 없다.
There is also an ongoing media exchange test and "connectathon" process for CD media and network operation that is organized by the IHE organization.

* DICOM media(확장자 없이 필요한)의 일부가 아닌 경우 DICOM 파일의 확장명은 '.dcm'이다.

* MIME type : "application/dicom"

* Uniform Type Identifier(UTI)의 DICOM 파일은 org.nema.dicom

* connectathon : IHE 조직이 구성한 CD media 와 네트워크 운영에 대한 지속적인 media exchange test가 있다.

## Application areas

DICOM 표준의 핵심 적용 영역은 **의료영상을 캡쳐하고 저장하며 배포하는 것** 이다.

* 그외의 적용 영역 : 영상화 절차 작업 목록 관리, DVD와 같은 디지털 미디어에 영상 복사, 영상 획득 완료에 같은 절차 상태 보고, 영상의 저장확인,data sets의 암호화, 환자를 식별할 수 있는 개인정보 data sets 제거, 리뷰를 위한 영상 레이아웃 구성, 영상 조작 및 주석 저장, 영상 디스플레이 보정, ECG인코딩,  CAD 결과 인코딩, 구조화된 측정 데이터  인코딩, 마지막으로 수집 프로토콜 저장

### Types of equipment

DICOM information object definitions은 다양한 영상 장치에 의해 생성된 데이터를 인코딩 한다.

* 다양한 영상 장치 : CT, MRI, 초음파, X-ray, 형광 투시, 혈관 조영술, 유방 조영술 등 

DICOM은 영상 또는 imaging work flow와 관련된 장치에서도 구현된다.

* 장치 목록 : PACS(picture archiving and communication systems, 사진 보관 및 통신 시스템), 영상 뷰어 및 디스플레이 스테이션,
CAD(computer-aided detection/diagnosis systems, 컴퓨터 기반 감지,), 3D visualization system, clinical analysis applications(임상 실험 결과 app), image printer,  Film scanners, media burners&importer(DCIOM 파일을 저장하거나 분배하는 CDs,DVDs, etc), RIS(radiology information systems, 방사선 정보 시스템), VNA(vendor-neutral archives), EMR(electronic medical record, 전자의료 기록) systems, radiology reporting systems(방사선 보고 시스템) 

### Fields of medicine

많은 의학분야에서 DICOM으 전담하는 실무 그룹이 있다.

* 해당 의학분야 : 방사선학, 심장학, 종양학, 핵 의학, 방사선 요법, 신경학, 정형 외과, 산부인과, 부인 과학, 안과, 치과, 악안면 수술, 피부과, 병리, 임상 시험, 수의학 및 의료 / 임상 사진 

## DICOM Port number

DICOM의 Port 번호는 104 이고, 11112 도 사용할 수 있다.

## 단점

DICOM 표준은 데이터 입력이 가능한 변수들이 너무 많아 문제가 있다. 이 단점은 대게 모든 필드를 데이터로 채우는 불일치에서 나타난다. 일부 필드는 비어 있고 일부 필드는 잘못된 데이터가 입력 되어 있기에 일부 영상들은 불안정한 경우가 많다.

또한 파일 포멧은 악성코드가 포함되어 있을 가능성이 있고 코드 실행도 허용한다.



---------

## 참고자료

[DICOM 기초](https://dcm4che.atlassian.net/wiki/spaces/d2/pages/1835038/A+Very+Basic+DICOM+Introduction)

[DICOM 설명(by wiki)](https://en.wikipedia.org/wiki/DICOM)

[DICOM 설명(slideshare)](https://slidesplayer.org/slide/12934045/)

[CT 3D 촬영 원리](http://www.ucdenver.edu/academics/colleges/medicalschool/departments/Radiology/About%20Us/Faculty/education-research-portal/Pages/filtbackproj.aspx)
