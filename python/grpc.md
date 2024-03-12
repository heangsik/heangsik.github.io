# grpc

## 설치

### 필수 패키지

> pip install grpcio grpcio-tools protobuf

## 개발

### proto 컴파일

- 파일 위치 /protos/hello.proto
- python -m grpc_tools.protoc -I=protos/ --python_out=protos/ --grpc_python_out=protos/ hello.proto
