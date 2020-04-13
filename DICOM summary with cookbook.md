# 다이콤 용어 정리

공부하면서 햇갈리는 용어들 정리

### Information Object Definitions(IOD)

IOD 라고 하는 Information Object Definition 은 정보교환을 위한 정보의 내용과 형식을 표준화하는 데에 사용, 서로 다른 회사의 CT 장치 간에
데이터를 교환하기 위한 표준화 방식을 설정하는것이다. 

구조적으로 IOD는 IE 라는 Information Entity 로 구성되어 있고 IE는 다시 모듈로 나뉘고 모듈은 개별적인 속성들로 이루어져 있다. 
* **MR영상을 위한 IOD** : MR IOD 는 환자정보 IE, 검사정보 IE, 시리즈정보 IE, 영상정보 IE 등으로 구성되고 영상정보 IE는 영상의 일반적인 내용을 다루는 모듈
(가로,세로,비트수 등)과 MR 영상의 특수한 특성에 대해 다루는 모듈로 구성이 되어 있다.

### DICOM Service Element(DIMSE)

DIMSE는 IOD 를 가지고 할 수 있는 모든 행위를 나타낸다. ("저장해라" 와 같은 행위) DIMSE는 C-Service, N-Service로 나누어진다.

* C-Service : Image와 같이 복합된 정보를 가지고 있는 것의 행위(저장하라, 찾아라, 전송하라)
* N-Service : 단일한 정보만을 가지고 있는 것에 대한 행위(만들어라, 가져와라)

### SOP Class
IOD("무엇") + DIMSE("어떻게") , 여러가지로 정의되어 있는 IOD와 DIMSE의 의미있는 조합만 찍을 지어 놓은 것이다. 예를 들어 **"CT 영상을 저장하라"**
는 말이 되지만(CT Image Storage SOP Class) CT 영상이 살아있는지 검사하라는 의미가 없기에 CT Image Echo SOP Class는 존재하지 않는다.

#### Cf. Meta SOP Class
여러가지 동작이 함께 동작해야하는 SOP 클래스를 의미한다. 예를들어 DICOM Print의 경우 통신 협상을 한 후, 전체 인쇄에 걸쳐서 공통적인 사항에 대한 정보
교환 마지막으로 필름에 대해셔 몇 장의 영상을 찍을것인지 형식을 정하고 등등의 많은 과정을 거친 후 마지막으로 인쇄를 한다.

### Unique IDentification (UID)
고유한 번호를 만들려고 하는 것, 모든 검사, 시리즈, 영상은 물론 각각의 Abstract Syntax, Transfer Syntax마다 전세계적으로 고유한 번호를 부여하는 방식

### Value Representation(VR)
식별자의 타입, 각각의 Attribute 들은 VR의 지정된 형태로 기록된다. 그러나 Attribute에 일반적으로 사용되는 VR이 사용되었을 경우, Implicit VR 을 사용 하여 VR을 표시하지 않을 수 있다.

### Transfer Syntax
DICOM File이 전송되는 형태, byte ordering(Little Endian, Big Endian) 및 압축의 유무 및 압축 방식 등을 나타냄
* 예시 : Implicit Little Endian , Explicit Little Endian, Explicit Big Endian, DICOM JPEG , DICOM JPEG2000

----
# DICOM NETWORK

### Application Entity(AE)
"DICOM 통신을 할 수 있는 것" 자체를 말한다. 다시 말해 통신을 시작할 때 가장 기본적인 값으로, 각기 서로에 대한 인식을 하기위한 것이다.
Application Entity Title(AE Title)은 "DICOM 통신을 할 수 있는 것의 이름"이라고 할 수 있다. 만약 통신할 수 있는 상대방의 AE Title 을 모르거나
자기 자신의 AE Title이 없다면, DICOM 통신을 할 수 없다.

### Presentation Context(PC)
하나의 Abstract Syntax(ex MR Image를 저장하라) + 여러 개의 Transfer Syntax(ex JPEG 압축, Encoding 방식이 결합된 것), 어떤 SOP  클래스를 어떤 방식으로 처리할 것인지 이야기하는 것이다.

ex) MR Image를 저장하라 + JPEG 압축 = JPEG으로 압축된 MR Image를 저장하라.

## DICOM Service Class

DICOM은 영상을 주고받을 뿐만 아니라 검색, 인쇄, OCS 연동 등 다양한 기능을 할 수 있다.

기본적으로 AE 간에 Associtation Negotiation 을 한 후 진행된다.

### Storage Service Class

### Query/Retrieve Service Class
DICOM 방식으로 영상을 검색하고 조회할 때 쓰는 기능이다. 서로 다른 업체의 장비들 사이에서 표준적인 검색 방식을 정의해 놓은 것이다.
* Query : 검색할 때 사용
* Retrieve : 검색 후에 필요한 영상을 가져올 때 사용

### Modality Worklist Management
Modality Worklist(DCIOM Worklist)는 영상장비와 통신하기 위한 것이 아니라 OCS 연동을 하기 위한 것이다. 장비에서 촬영은 환자 및 검사정보가 많이
필요하다. Modality Worklist가 만들어지기 전에는 검사정보를 촬영자가 수작업으로 입력했고 인력과 시간 낭비 오탈자가 많았다. 이러한 오류를 방지하기
위해서 만든 Modality Worklist Management이다.

Modality Worklist Management는 크게 두가지 서비스로 나뉜다.
* BASIC WORKLIST MANAGMENT SERVICE : 장비에서 촬영을 위해 필요한 환자 및 검사정보를 제공하기 위한 서비스
* MODALITY PERFORMED PROCEDURE STEP : 장비에서 촬영 진행 상황을 나타내기 위한 서비스

> OCS(Order Communication System) : 처방 전달 시스템. 의사의 처방을 인력이나 기계적인 방법에 의존하지 않고 컴퓨터를 이용해 신속,정확하게
진료 지원부서에게 전달하는 시스템이다.

#### Basic Worklist Management 
Modality Worklist란 말 그대로 검사 장비에서 수행되어야 할 작업 목록이다. 
* Matching Key : 원하를 정보를 얻기 위한 조건을 담아 보냄
* Return Key : 그에 해당하는 검사 대상 환자의 정보를 담아서 보내주는 키

#### Modality Performed Procedure Step(MPPS)
장비에 기본적인 환자 및 검사정보가 제공되면 촬영 시작, 진행 및 종료의 과정을 거친다. MPPS는 이런 과정을 DICOM message의 형태로 보내
쉽게 진행 상태를 알 수 있도록 한다.

#### Storage Commitment
영상장비에서 특정 영상이 확실하게 서버에 저장이 되있는지를 확인할 때에 사용하는 것이다. 영상 장비에서 영상을 삭제하기 전에 서버에 Storage Commitment 방식으로 확실하게 저장이 되있는지 확인할 때 사용된다.

#### Media Exchange
의료영상을 네트워크가 아니라 CD 나 MOD,DVD와 같은 저장매체를 통해 영상을 전달할 때에 어떻게 해야 하는지 규정하는 부분이다. 업체마다 호환성이
떨어지는 부분을 제거하기 위해 기록하는 방식을 표준화 한 것이다.

DICOM 방식으로 구운 CD를 보통 DICOM CDR이라고 한다. 이 CD에는 반드시 DICOMDIR 이라고 하는 디렉토리 파일이 있어야 한다. 그래야 어떤 회사의 장비든지
DICOMDIR 파일을 보고 CD에 무슨 내용이 들어있는지 알 수 있게 된다. 

* DICOMDIR : DICOMDIR 은 CD 안에 들어있는 내용의 목차가 저장되어 있는 파일이라고 할 수 있다. 의료영상이 저장된 CD에 이 파일이 없다면, 그것은
곧 비표준이라는 의미로 다른 장비와 호환이 되지 않는것을 의미한다.

### Print Management Service System
필름 등에 hard copy를 할 때 프린트하고자 하는 영상과 영상에 관련된 기타 데이터들을 관리하기 위해 사용하는 class 이다.

#### Print Management Model
프린트를 하기 위해선 세과정으로 나뉜다. ( Print Session Management - Queue Management - Print Process)

* Print Session Management : hard copy를 하기 위해 필요한 모든 영상정보와 영상에 관련된 기타 데이터들을 'Film Session'에 정렬하는 과정이다.
'Film Session'이란 하나 이상의 Film 으로 구성되어 있으며, Film 은 하나 이상의 image로 구성되어 있다. 즉 Film Session 에는 프린트 하고자
하는 영상 뿐만이 아니라 각종 annotation, overlay 등의 정보와 프린트 될 각각의 film 의 layout 등 각종 정보를 포함하고 있다. 이렇기 때문에
정렬하는 과정이 필요하다.

* Queue Management : Film Session이 정렬 된 후 실제로 hard copy 되기 전에 다양한 print 관련 작업을 하기 위한 대기 모드

* Print Process : 실제 hard copy 되는 과정

----

## DICOM Image SOP Instance
### Image Information Model
병원에 환자가 방문해 각 Modality 에서 촬영을 한다면, Patient, Study, Series, Image 는 1 : n의 포함 관계를 가지게 된다.

#### Classification of Image Data
Image Information 은 각자의 역할에 따라 Patient Information, Study Information, Series Information, Application Information 등으로
나누어진다.

Information Model 은 계층적으로 어떻게 다른 SOP Instance 안에 있는 information이 각기 다른 level로 Grouping 되어질 수 있는가를 정의하고
있다.

#### Image Types
DICOM은 여러 가지의 Image SOP class type 들을 정의한다. 이러한 Image Type은 Image를 생성해내는 Modality 를 기준으로 정의하게 된다.
모든 Image SOP들은 모든 Image에서 Display와 기타 모든 목적을 위해 기본적으로 필요한 공통적인 부분들이 있다.

#### 가장 기본적으로 꼭 필요한 Attributes
* Identification Attribute : SOP Class UID, Study Instance UID, Series Instance UID, Image Instance UID(=SOP Instance UID) 등
* Modality Type : Modality Type
* Pixel Value Interpretation : Photometric Interpretation 등
* Pixel Encoding : Bit Allocated, Bit Stored, High Bit, Pixel Representation, Planar Configuration 등
* Pixel Matrix : Pixel Data

위와 같은 Attributes들은 Type1 으로 지정하고 있으며, NULL 값이 들어갈 수 없다. 

#### Image Processing Pipeline

Modality에서 영상을 획득한 후 Display 이전까지의 processing 과정을 말하는 것이다. 
