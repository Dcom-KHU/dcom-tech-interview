## Batch Size VS Epoch
배치 사이즈와 에폭은 딥러닝 학습에서 자주 등장하는 개념입니다. 하지만 막상 설명하려면 헷갈리기 십상입니다. 한번 정리해보도록 합시다. 


### Batch Size
**연산 한번에 들어가는 데이터의 크기**  
한번에 너무 큰 크기의 데이터 셋을 학습시키려면 메모리도 부족하고, 계산 시간도 오래 걸리는 등 여러 문제가 발생합니다.
따라서 전체 데이터셋을 Batch로 나눠 학습을 진행합니다. 이때 연산 한번에 들어가는 데이터의 크기를 Batch Size라고 합니다.
참고로, 1 batch size를 다른 말로 mini batch라고도 표현합니다.

### Epoch
**전체 데이터 셋이 모델의 순전파(forward)와 역전파(backward)를 통과한 횟수**  
예를 들어, 10 Epoch는 전체 데이터 셋이 모델의 순전파와 역전파를 10번 통과했다는 것을 의미합니다.  

Epoch이 너무 크면 Overfitting, 작으면 Underfitting이 발생하니 적당한 Epoch을 찾는 것이 중요합니다.

### Iteration
**전체 데이터 셋을 모델에 학습시키기 위해 필요한 배치의 수**  
예를 들어, 전체 데이터 셋의 크기가 1000이고, mini batch의 크기가 100이라면 필요한 Iteration은 10입니다.  
이때, 10 Iteraion은 1 Epoch에 파라미터 업데이트가 10번 진행된다고도 이해할 수 있습니다.



![image](https://user-images.githubusercontent.com/16442978/164952236-8da23376-c84c-4bba-abd6-35cb49adae59.png)

## Reference
- [https://m.blog.naver.com/qbxlvnf11/221449297033](https://m.blog.naver.com/qbxlvnf11/221449297033)
- [https://losskatsu.github.io/machine-learning/epoch-batch/#1-%EC%82%AC%EC%A0%84%EC%A0%81-%EC%9D%98%EB%AF%B8](https://losskatsu.github.io/machine-learning/epoch-batch/#1-%EC%82%AC%EC%A0%84%EC%A0%81-%EC%9D%98%EB%AF%B8)
- [https://jerryan.medium.com/batch-size-a15958708a6](https://jerryan.medium.com/batch-size-a15958708a6)
