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

