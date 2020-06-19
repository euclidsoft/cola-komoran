# Grpc Tokenizer

## proto 파일 생성

proto/kr/re/keit/Komoran.proto 파일

## protoc를 통해 소스코드 생성

예시일 뿐, `proto/build.sh` 명령을 통해 수행해주세요.

```sh
docker run --rm \
    -v `pwd`:/defs namely/protoc-all \
    -f ./kr/re/keit/Komoran.proto \
    -l java
```

생성한 소스파일 `proto/gen/` 디렉토리에 위치해있으며, 각 언어 별로 복사해서 쓰시면 됩니다.

소스 파일은 수정하지 않고 쓰시되, import 경로명은 수정이 필요할 수도 있습니다.

## Grpc Server 샘플

`Main.kt` 참고

## Grpc Client 샘플

```python
from grpc import insecure_channel
from .kr.re.keit.Komoran_pb2_grpc import KomoranStub
from .kr.re.keit.Komoran_pb2 import TokenizeRequest

class GrpcTokenizer:
    def __init__(self, target):
        channel = insecure_channel(target)
        self.stub = KomoranStub(channel)

    def __call__(self, sentence):
        request = TokenizeRequest(sentence=sentence)
        response = self.stub.tokenize(request)
        keyword_list = response.keyword
        return keyword_list
```

```python
sentence = """
    워크에 대해 권선 동작을 수행하기 위한 방법 및 그 장치가 개시되는데,
    클램프에 의해 그 단부가 파지 되는 와이어는 훅의 의해 걸리고(hooked),
    상기 걸려진 와이어는 워크에 제공된 구멍으로 삽입되며, 그 다음 클램프는
    와이어가 훅을 끌어 당길 때 훅과는 반대 위치로 이동되고, 와이어의 단부를
    파지하는 클램프는 워크에 대해 선회되어 워크에 와이어를 감는다. 따라서,
    상기 구멍의 에지와 와이어 사이의 접 촉 압력은 제거된다.
"""
tokenize = GrpcTokenizer("localhost:50051")
print(tokenize(sentence))
```
