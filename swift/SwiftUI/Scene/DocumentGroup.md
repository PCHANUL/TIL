# DocumentGroup

DocumentGroup은 문서를 열고 편집하기 위한 사용자 인터페이스를 생성하는 Scene 타입이다. 문서 데이터 모델과 문서 내용을 표시하는 뷰 계층 구조를 초기화한다. [FileDocument](https://developer.apple.com/documentation/swiftui/filedocument?changes=latest_minor) 프로토콜을 준수하는 값 모델을 사용하거나 [ReferenceFileDocument](https://developer.apple.com/documentation/swiftui/referencefiledocument?changes=latest_minor) 프로토콜을 준수하는 참조 모델을 사용할 수 있다. 문서 기반 앱에서 필요한 다중 창 지원, 패널 열기 및 저장, 드래그 앤 드롭과 같은 표준 동작을 지원한다.  

## Value type documents

- [protocol FileDocument](https://developer.apple.com/documentation/swiftui/filedocument?changes=latest_minor) : 문서를 직렬화하는데 사용되는 문서 모델  
- [struct FileDocumentConfiguration](https://developer.apple.com/documentation/swiftui/filedocumentconfiguration?changes=latest_minor) : 열린 파일 문서의 속성  

## Reference type documents

- [protocol ReferenceFileDocument](https://developer.apple.com/documentation/swiftui/referencefiledocument?changes=latest_minor) : 참조 유형 문서를 직렬화하는데 사용되는 문서 모델  
- [struct ReferenceFileDocumentConfiguration](https://developer.apple.com/documentation/swiftui/referencefiledocumentconfiguration?changes=latest_minor) : 열린 참조 파일 문서의 속성  

## Read and write configurations

- [struct FileDocumentReadConfiguration](https://developer.apple.com/documentation/swiftui/filedocumentreadconfiguration?changes=latest_minor) : 파일 읽기
- [struct FileDocumentWriteConfiguration](https://developer.apple.com/documentation/swiftui/filedocumentwriteconfiguration?changes=latest_minor) : 파일 쓰기
