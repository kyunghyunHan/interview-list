# 면접정리

## 블록체인
1. 토큰과 코인의 차이
- 코인은 독립된 블록 네트워크 즉 메인넷을 소유한 경우 코인, 독립된 블록체인 네트워크를 소유하지 않은 경우(Dapp) 토큰으로 불림
2. 토큰 디자인에 대하여
- [자료](https://medium.com/@isp1195/%ED%86%A0%ED%81%B0-%EB%94%94%EC%9E%90%EC%9D%B8-%ED%8C%A8%ED%84%B4-%EC%8B%9C%EB%A6%AC%EC%A6%88-1-%ED%86%A0%ED%81%B0-%EC%9D%B4%EC%BD%94%EB%85%B8%EB%AF%B8%EC%9D%98-%EC%A4%91%EC%9A%94%EC%84%B1%EA%B3%BC-%ED%86%A0%ED%81%B0-%EB%94%94%EC%9E%90%EC%9D%B8-%ED%8C%A8%ED%84%B4-token-design-pattern-725a637ee74a)
3. 이더리움 ERC20 & ERC721의 차이
- ERC721은 ERC20와는 반대로 '대체 불가능(Non Fungible)'한 특징을 지니고 있다. 쉽게 말해 ERC721로 발행되는 토큰은 모두 제 각각의 가치(Value)를 갖고 있다는 것이다.
- ERC20로 발행되는 토큰은 모두 이와 같은 '대체 가능'의 특징을 지닌다. 가령 A거래소에서 파는 오미세고(OmiseGo)와 B거래소에서 파는 오미세고(OmiseGo)는 모두 동일한 화폐이다. 
4. 블록체인 트릴레마(Blockchain Trilemma)란
- 블록체인에서 트릴레마란 확장성(Scalability), 탈중앙화(Decentralization), 보안성(Security)의 세 가지 문제는 한번에 해결할 수 없음을 뜻한다. 
- 확장성(scalability) : 사용자 수의 증대에 유연하게 대응할 수 있는 정도이다. 블록체인에서는 사용자수의 증가에 따라 거래건수가 늘어나더라도 무리 없이 전송 처리용량을 증대시킬 수 있는 능력을 의미한다.
- 탈중앙화(decentralization) : 중앙집중화를 벗어나 분산된 소규모 단위로 자율적으로 운영되는 것을 말한다. 블록체인은 기존의 서버-클라이언트 관계가 아니라, 개별 노드들의 자발적이고 자율적인 연결에 의해 피투피(P2P) 방식으로 작동한다. 블록체인 기술이 도입되어 사회적으로 널리 확산됨에 따라 기존의 중앙집중식 조직, 기업, 단체, 기구 등은 탈중앙 분산 구조로 변경되고 있다.
- 보안(security) : 보안이란 블록체인 내의 데이터나 프로그램을 권한이 없는 이용자가 사용할 수 없도록 하는 것을 의미한다.
5. 프로토콜이란
- [자료](https://mindnet.tistory.com/entry/%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC-%EC%89%BD%EA%B2%8C-%EC%9D%B4%ED%95%B4%ED%95%98%EA%B8%B0-9%ED%8E%B8-%ED%94%84%EB%A1%9C%ED%86%A0%EC%BD%9C-%EC%9D%B4%EB%9E%80-Protocol-%EC%9D%B4%EB%9E%80-%EB%AC%B4%EC%97%87%EC%9D%B8%EA%B0%80)


## 하이퍼 패브릭
1. 하이퍼레저 패브릭 트랜잭션 흐름
- [자료](https://steemit.com/blockchain/@keepit/keep-t-column-hyperledger-fabric)
2. 하이퍼레저 패브릭 MVCC 충돌
- [자료](https://repository.hanyang.ac.kr/handle/20.500.11754/163994)
- [자료2](https://github.com/OnMyTeam/hyperledger_project)
3. 하이퍼레저 패브릭 MSP
- [자료](https://hcnam.tistory.com/23)
4. 하이퍼레저 패브릭 Fabric-CA하는 역활
- [자료](https://hamait.tistory.com/983)
5. 하이퍼레저 패브릭 RWSet이란
- [자료](https://bysssss.tistory.com/86)
- [자료2]()-준비중
6. 하이퍼레저 패브릭 블룸 필터
- 거짓 양성(false positive) 은 실제로는 거짓인데 검사 결과는 참 이라고 나오는 것이며,
거짓 음성(false nagative) 실제로는 참인데 검사 결과는 거짓이라고 나오는 것이다. 
불룸필터는 거짓 양성기반의 확률적인 데이터 분류 체계이다. 효율적인 검색, 효율적인 분류에 사용된다예를 하나 더 들면 
거짓 양성(false positive) 은 부품검사장비에서 실제로는 망가진 부품인데 검사 결과는 참 이라고 나오는 것이며, 거짓 음성(false nagative) 은 부품검사장비에서 실제로는 정상인 부품인데 검사 결과는 고장이라고 나오는 것이다.
이 경우 100%정확한 장비를 만들지 못한다면 거짓음성쪽으로 발생할 확률이 높게 코딩하는게 맞다고 볼 수 있다.
7. 하이퍼레저 패브릭 ACL은 어떤 정보를 어떻게 가져와서 적용되나요
- [자료](https://yssa.tistory.com/entry/Blockchain-%ED%95%98%EC%9D%B4%ED%8D%BC%EB%A0%88%EC%A0%80-%EC%BB%B4%ED%8F%AC%EC%A0%80Hyperledger-Composer)
8. 하이퍼레저 패브릭에서 저장 용량을 줄이기 위한 prunning은 어떻게 할수 있을 가요
- [자료]()-준비중
9. 하이퍼레저 패브릭 Kafka는 왜 사용 되나요? RAFT는 왜 등장 한 걸까요
- [자료](https://velog.io/@dojun527/%EC%98%A4%EB%8D%94%EB%9F%AC%EC%97%90-%EC%82%AC%EC%9A%A9%EB%90%98%EB%8A%94-Kafka%EB%9E%80)
10. 하이퍼레저 패브릭에서 토큰은 만들 수 있나요? FabToken 왜 만들어 졌을가요
- [자료](https://hamait.tistory.com/1058)
11. 하이퍼레저 패브릭에서 리더피어와 앵거피어란 무엇?
- 리더피어는 조직 내에서 피어들간의 오더러로 부터 받은 블록을 공유하기 위한 대표 피어이다. 이 피어가 맛이가면 조직내의 피어들끼리 리더선출을 통해서 새로운 리더를 선출하고 오더러에 알려서 정상적으로 작동하게 된다.
- 앵커피어는 조직 간의 피어들에 대한 정보 교환의 대리인으로 사용된다. 이로써 서로에 대한 위치를 알게 되어 아무 조직의 Peer 하나에 Proposal을 보내도 모두에 적용될 수 있게 되며, MSP에 대한 공유도 가능해진다. 적어도 하나의 앵키피어가 채널 설정시 정의되야하며, 채널에 참여하는 모든 피어들은 제네시스 블록안에 기록된 앵커피어에 대한 정보를 공유하게 된다.  (앵커피어가 1개일 경우 서로 다른 B,C의 조직은 A조직의 그 앵커피어를 통해서 서로에 대해 알게되고 MSP를 직접 교환하게 된다)

12. 하이퍼레저 패브릭에서 Gossip Prorocol는 왜 사용하나요?
- [자료](https://hamait.tistory.com/988)
- [자료2](https://velog.io/@jjimgo/%EB%8D%B0%EC%9D%B4%ED%84%B0%EC%99%80-%EC%9D%B8%ED%94%84%EB%9D%BC-%EC%95%84%ED%82%A4%ED%85%8D%EC%B3%90-%EC%A0%95%EB%8B%B5#-%EC%9D%B4%EB%8D%94%EB%A6%AC%EC%9B%80%EC%97%90%EC%84%9C-%EC%A0%80%EC%9E%A5%EC%9A%A9%EB%9F%89%EC%9D%84-%EC%A4%84%EC%9D%B4%EA%B8%B0-%EC%9C%84%ED%95%9C-prunning%EC%9D%80-%EC%96%B4%EB%96%BB%EA%B2%8C-%ED%95%A0-%EC%88%98-%EC%9E%88%EC%9D%84%EA%B9%8C%EC%9A%94)

## Go 
1. Go의 덕타이핑 장 단점은 무엇인가요?
```
덕 타이핑은 더 적은 코드와 복잡한 상속 구조가 덜 필요하다. 따로 인터페이스를 지정할 필요가 없고, 사용할 메서드를 작성하면 되기 때문이다.!
1번의 장점은 복잡한 기능으로 여러 클래스에서 빌드할 때 가장 눈에 띈다. generic hell과 같은 상황이 일어나기 어렵기 때문이다.
다형성, SOLID등.. 다른 개념들보다 이해하기 매우 쉽다.
단점
간결함이 의도치 않은 동작을 만들 때가 있다.
다른 사람과 협업에서 조금 더 주의가 필요하다. 해당 메소드에 대해서 잘 알지 못한다면 코드가 망가질 수 있다.
컴파일 타임 등에서 객체가 명세가 충족되었는지 알 수 없기 때문에, 많은 유닛 테스트로 해당 객체가 정상적으로 동작할 수 있는지를 계속 체크해야 한다.
```
2. Go에서 고루틴&채널은 무엇인가요?
- [고루틴](https://github.com/kyunghyunHan/go_study/blob/56bbd6e122d560cfab793f7bb5813e4f46378876/goroutine.md)
- [채널](https://github.com/kyunghyunHan/go_study/blob/56bbd6e122d560cfab793f7bb5813e4f46378876/channel.md)
- [자료](https://judo0179.tistory.com/88)
3. Go에서 selet문은 어떻게 사용되나요 아래코드를 설명해 주세욥
- [selet](https://hamait.tistory.com/1017)
