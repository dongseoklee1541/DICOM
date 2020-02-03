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

### Transfer Syntax
DICOM File이 전송되는 형태, byte ordering(Little Endian, Big Endian) 및 압축의 유무 및 압축 방식 등을 나타냄
* 예시 : Implicit Little Endian , Explicit Little Endian, Explicit Big Endian, DICOM JPEG , DICOM JPEG2000
