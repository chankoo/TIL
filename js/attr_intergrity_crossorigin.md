#### 19.07.25

보안을 위한 속성이며

`<script intergrity="해쉬값" crossorigin="anonymous || use-credentials"></script>` 

형태로 주로 쓰인다


## intergrity
__SubResource Integrity__(SRI) 는 브라우저가 가져오는 리소스(ex. from a __CDN__)의 무결성을 보장하는 특성이다. intergrity 속성을 통해 SRI를 보장한다.

예를 들어 CDN에 있는 파일은 중간자 공격에 이용될 수 있다. 서버에서 브라우저로 전송되는 데이터를 가로채서   HTML 이나 js를 수정하는 악의적인 행위이다. 이를 방지하기 위해  script 나 link 엘리먼트 내부에 해쉬값을 삽입하여 원래의 파일과 이용하는 파일이 동일함을 보장받는 것이다. 

![sasd](https://i.stack.imgur.com/44RQB.png)

위와 같이 intergrity 속성이 매치되지 않으면 SRI에 의해 block 된다

## crossorigin
src의 도메인과 crossorigin 이슈가 있을때 허용 여부를 결정한다.

crossorigin 속성을 지정하면 요청의 mode가 __cors__ 로 설정된다.

__anonymous__ 일 경우 요청의 __credentials mode__ 가 "same-origin" 으로 설정되어 crossorigin 요청이 수행되는 반면

 __use-credentials__ 일 경우 __credentials mode__ 가 "include" 로 설정되고 credentials을 필요로 하게 된다.

 ![](https://i.stack.imgur.com/pI33t.png)


