# 1차 프로젝트 "Magazine K"
> Magazine B 웹 사이트를 모티브하여 만든 커머스 사이트 </br>
> [Magazine K 영상](https://www.youtube.com/watch?v=f-sBWaB70Ck)

## 1. 제작 기간 & 참여 인원
- 7월 18일 ~ 7월 29일 => 약 2주
- Front-End : 주원영, 노정은, 길현민 </br>
Back-End : 김동규, 황유정

## 2. 사용 기술
- React
- Javascript
- SASS

## 3. 핵심 기술
 
#### `우리가 모티브 한 Magazine B`
- 세계 최초 브랜드 다큐맨터리 매거진이고 전 세계의 균형 잡힌 브랜드를 한 호에 하나씩 소개하는 광고 없는 월간지입니다. <br /> 
 브랜드의 숨은 얘기는 물론 감성과 문화까지 담고있어 브랜드에 관심 있는 사람이라면 누구나 쉽게 웹 사이트에 들어와 <br />
 월간지를 구매할 수 있는 사이트입니다.<br />

#### `구현 기능`
- 구현한 기능을 간략하게 소개하자면<br />
 회원가입 후 로그인 -> 로그인 한 사용자 장바구니에 담은 후 결제 -> 베송받은 후 리뷰기능 작성<br />
 커머스 사이트의 기본적인 FLOW를 갖추고 있습니다.

#### `개발 내용`
- 회원가입 페이지
- 로그인 페이지
- 메인 페이지 ➡️ 노정은
- 제품 리스트 페이지 
- 제품 상세 페이지 ➡️ 노정은
- 네비게이션 바
- 장바구니 모달 ➡️ 노정은
- 장바구니 메인 & 결제 페이지
- 푸터 ➡️ 노정은 


## 4. 문제 해결 경험
#### 4-1. 라이브러리 없이 Carousel 구현하기 *[코드로 이동](https://github.com/wjddms4107/magazine-K-frontend/blob/4a51bd8b43237d08e9d0a6161f893ac76b2df30d/src/pages/Main/Main.js#L7)
<img width="700" alt="메인" src="https://user-images.githubusercontent.com/78889402/190542391-14962d89-424a-483a-b94c-53d84a4ed1b2.png">

- 메인 페이지 최상단에 3초마다 이미지와 데이터가 자동 변환되고 이미지의 오른쪽, 왼쪽을 누르면 이미지와 데이터가가 변환하는 Carousel를 구현했습니다. 

- setinterval 함수 사용하여 3초마다 슬라이드 이미지가 자동 변환되도록 구현했고 useState로 숫자 3이 넘어가면 1부터 다시 시작하도록 설정했습니다. 

- 그리고 슬라이드 이미지의 오른쪽을 누르면 앞의 이미지로, 왼쪽을 누르면 뒤의 이미지로 변환하는데 이는 button태그로 오른쪽 반, 왼쪽 반 각각 넓이를 50%씩 주고 opacity: 0으로 안 보이게 처리하였습니다. 또한 `const isFirstSlide = slide === 1; const isLastSlide = slide === 3;`로 변수를 만들어 1과 3에서는 버튼을 눌러도 안 넘어가도록 disabled에 할당해 주었습니다.
  
- 하지만 이 기능을 구현하면서 가장 애를 먹은 부분은 이미지에 맞는 데이터를 출력하는 부분이었습니다. 어떻게 그에 맞는 데이터를 입히느냐 고민 끝에 fatch 받아온 데이터에 filter메서드를 걸어 data.id와 slide의 State 숫자가 같을 때만 리턴하도록 해서 구현할 수 있었습니다.

<details>
<summary><b>구현한 코드</b></summary>
<div markdown="1">

 ~~~javascript
 const [slide, setSlide] = useState(1);
 const [mainSlideData, setMainSlideData] = useState([]);

 useEffect(() => {
   fetch('/data/mainSlideData.json')
     .then(res => res.json())
     .then(data => {
       const sameNumData = data.filter(data => {
         return data.id === slide;
      });
   setMainSlideData(() => sameNumData);
 });
 ~~~
 
</div>
</details>

#### 4-2. 클라이언트가 서버로 요청 보내는 방법 *[코드로 이동](https://github.com/wjddms4107/magazine-K-frontend/blob/4a51bd8b43237d08e9d0a6161f893ac76b2df30d/src/pages/ProductDetail/Detail/Detail.js#L74)
<img width="700" alt="제품상세" src="https://user-images.githubusercontent.com/78889402/190542484-52794c4f-f4d8-4dc3-94e4-43312a852936.png">

- 상품 디테일 페이지에서 제품의 수량을 선택하고 'add to cart'버튼을 누르면 서버로 총수량을 보내서 최종적으로 장바구니 모달에 담기는 기능을 할 때, backend에게 get요청만 해오다가 처음으로 post요청으로 데이터를 body에 담고 서버로 보내는 로직을 구현해 보았습니다.

- 1차 프로젝트에서 backend와 협업을 처음 해보았고 또한 데이터 요청에 감을 잡지 못하던 저에게 '항상 처음이 어렵지 하고 보면 할만하다 그러니 쫄지말자!'라는 자신감을 불어 넣어준 경험이기에 인상 깊습니다.

<details>
<summary><b>구현한 코드</b></summary>
<div markdown="1">

 ~~~javascript
 <button
  onClick={() => {
    const token = localStorage.getItem('login-token') || '';

     if (token) {
        fetch(`http://10.58.3.49:8000/orders/cart/${product_id}`, {
           method: 'POST',
           headers: {
             AUTHORIZATION: token,
           },
           body: JSON.stringify({ quantity: orderQuantity }),
        })
           .then(res => res.json())
           .then(res => {
             if (res.result === 'SUCCESS') {
               setModalState(true);
             }
            });
        } else {
           alert('로그인이 필요한 기능입니다.');
           navigate('/Login');
        }
   }}
 >
   ₩{priceThousand * orderQuantity},000 ADD TO CART
 </button>
 
 ~~~
 
</div>
</details>

#### 4-3. 리뷰 별점 기능 구현하기 *[코드로 이동: 리뷰 추가](https://github.com/wjddms4107/magazine-K-frontend/blob/4a51bd8b43237d08e9d0a6161f893ac76b2df30d/src/pages/ProductDetail/Review.js#L34), [코드로 이동: 별점](https://github.com/wjddms4107/magazine-K-frontend/blob/4a51bd8b43237d08e9d0a6161f893ac76b2df30d/src/pages/ProductDetail/Review.js#L83)
<img width="700" alt="제품상세리뷰" src="https://user-images.githubusercontent.com/78889402/190542523-a16021f7-f9a7-4f06-a935-9b3ccb02b911.png">

- 제품 상세 페이지에 제가 제일 애정을 가지고 있는 리뷰기능이 있습니다. 이는 원래의 Magazine B에는 없는 기능인데 구현하면 재미있을 것 같아 팀원들에게 제안하여 추가되었고 함께 합을 맞춰볼 backend 동기와 깊이 소통하면서 기획, 데이터 구조, 요청 방식 등 모든 부분에 정성을 들여 탄생한 기능이기 때문입니다.

- 그리고 '리뷰 등록' 버튼을 누르면 선택한 별점, 입력한 댓글, 아이디명이 등록되는데 기존에 배열로 된 한 줄 댓글만 구현해 보아서 처음에 엄청 헤맸습니다. 어떻게 데이터를 만져야 할지 감이 잡히지 않았기 때문입니다. 결국은 당연하게도 key와 value를 담은 객체 데이터로 구현할 수 있었는데 이 고민을 통해 수많은 생각을 하면서 코딩에 감을 불어 넣어 준 코드이기에 매우 인상 깊습니다.

- 또한 별점 기능에서 별을 클릭하거나 호버했을 때 별이 채워지며 클릭을 해야지만 state가 변경하는데 이는 일단 [...Array(5)].map으로 빈 별을 채워줬고 클릭하면 rating state가 바뀌어 index와 비교해서 작거나 같을 시 className으로 on을 줘서 별이 채워지도록 하여 구현할 수 있었습니다.

- 위의 작업을 마친 후 backend에게 데이터를 전송하면 리뷰 기능이 성공적으로 구현될 줄 알았는데 별점이 제대로 표시되지 않는 문제가 발생했습니다. 분명히 데이터를 잘 보내주고 있는데 말입니다. 해결을 위해 차근차근 코드를 분석해 보았고 그 결과 back에서 주는 rating은 '1.0', '2.0'과 같은 문자, 클라이언트가 별점을 누를 때는 rating이 1, 2와 같은 숫자여서 댓글에 별점이 표시되지 않던 것입니다. 이는 Number(rating)로 문자를 숫자 데이터로 바꿔주므로 해결할 수 있었지만 생각지도 못한 곳에서 문제가 생겨 '역시 끝까지 긴장을 늦출 수 없는 것이 코딩이구나'하는 깨달음을 준 경험이라 매우 인상 깊습니다. 

<details>
<summary><b>구현한 코드</b></summary>
<div markdown="1">
 
 ~~~javascript
 // '리뷰등록' 버튼을 눌렀을 때 리뷰 추가하기
 const addReview = e => {
    if (commentText.length < 6) {
      alert('5글자 이상을 입력해주세요');
      setCommentText('');
    } else if (rating === 0) {
      alert('별점을 선택해주세요.');
    } else {
      const textareaObj = {
        content: commentText,
        rating: rating,
      };
      fetch(`http://10.58.3.49:8000/products/${product_id}/reviews`, {
        //사용할 http 메소드
        method: 'POST',
        //토큰
        headers: {
          Authorization: token,
        },
        //서버에 보낼 데이터 (별점, 리뷰)
        body: JSON.stringify({
          rating: rating,
          content: commentText,
        }),
      })
        .then(res => res.json())
        .then(data => {
          if (data.MESSAGE === 'SUCCESS') {
            const copyComment = [...comment, textareaObj];
            setComment(copyComment);
            setCommentText('');
          } else {
            alert('구매하셔야 리뷰등록 가능합니다!');
            setCommentText('');
          }
      });
    }
  };
 
 // 별점 채우기
 {[...Array(5)].map((star, index) => {
  index += 1;
  return (
    <button
      type="button"
      id={index}
      key={index}
      className={index <= (hover || rating) ? 'on' : 'off'}
      onClick={() => setRating(index)}
      onMouseEnter={() => setHover(index)}
      onMouseLeave={() => setHover(rating)}
    >
      <span className="star">&#9733;</span>
    </button>
  );
})}
 ~~~
 
</div>
</details>


## 5. 각 페이지별 View
> [유튜브 데모 영상](https://www.youtube.com/watch?v=f-sBWaB70Ck)

<table>
  <thead>
    <tr>
      <th>
        메인페이지
      </th>
      <th>
        메인페이지
      </th>
      <th>
        메인페이지
      </th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td align="center">
        <img width="789" alt="image" src="https://user-images.githubusercontent.com/78889402/190124386-da1019e1-a03f-48f3-9221-1824a03713b3.png">
      </td>
      <td align="center">
       <img width="789" alt="image" src="https://user-images.githubusercontent.com/78889402/190124410-edc57de2-051f-4126-bbe4-7e52b07acb9d.png">
      </td>
      <td align="center">
       <img width="789" alt="image" src="https://user-images.githubusercontent.com/78889402/190124387-2d1dcdf6-081e-422f-b21c-81204720220c.png">
      </td>    
    </tr>
  </tbody>
</table>
<table>
  <thead>
    <tr>
      <th>
        회원가입 페이지
      </th>
      <th>
        로그인 페이지
      </th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td align="center">
        <img width="789" alt="image" src="https://user-images.githubusercontent.com/78889402/190124804-43200f9a-1e97-48f9-9942-660af5011b4c.png">
      </td>
      <td align="center">
          <img width="789" alt="image" src="https://user-images.githubusercontent.com/78889402/190124830-845e2134-a8d1-48ca-81ae-43661dfb4cc9.png">
      </td>
    </tr>
  </tbody>
</table>
<table>
  <thead>
    <tr>
      <th>
        제품 리스트 페이지
      </th>
      <th>
        제품 상세 페이지
      </th>
      <th>
        제품 상세 리뷰 기능
      </th
    </tr>
  </thead>
  <tbody>
    <tr>
      <td align="center">
        <img width="789" alt="image" src="https://user-images.githubusercontent.com/78889402/190124862-286c1def-fd34-41cd-9a77-d9431832c1a8.png">
      </td>
      <td align="center">
       <img width="789" alt="제품상세" src="https://user-images.githubusercontent.com/78889402/190124886-b11ce346-4223-450f-91a1-21f820773755.png">
      </td>
     <td align="center">
      <img width="789" alt="제품상세리뷰" src="https://user-images.githubusercontent.com/78889402/190124908-9c884cc4-9305-4168-bba2-8fde6b039f4a.png">
      </td>
    </tr>
  </tbody>
</table>
<table>
  <thead>
    <tr>
      <th>
        장바구니 모달
      </th>
      <th>
        장바구니 메인, 결제 페이지
      </th>
    </tr>
  </thead>
  <tbody>
    <tr>
     <td align="center">
        <img width="789" alt="image" src="https://user-images.githubusercontent.com/78889402/190126089-03205ddb-9f3f-4f78-963b-bebbd4d5d360.png">
      </td>
      <td align="center">
        <img width="789" alt="image" src="https://user-images.githubusercontent.com/78889402/190125001-3fff2734-9f00-408a-b134-f4be9ea28e87.png">
      </td>
    </tr>
  </tbody>
</table>
<table>
  <thead>
    <tr>
      <th>
        네비게이션 바 검색기능
      </th>
      <th>
        푸터
      </th>
    </tr>
  </thead>
  <tbody>
    <tr>
     <td align="center">
        <img width="789" alt="image" src="https://user-images.githubusercontent.com/78889402/190125776-0e62894d-0cde-46f3-8f03-b2036283b656.png">
      </td>
      <td align="center">
       <img width="789" alt="푸터" src="https://user-images.githubusercontent.com/78889402/190125639-2bd36ada-9e43-42c9-aa68-aa2c8ca02387.png">
      </td>
    </tr>
  </tbody>
</table>



## 6. 프로젝트 협업 도구
### 1. Trello
- 기능 단위로 카드를 생성하여 프로젝트가 sprint 미팅대로 잘 이루어졌는지 파악하고 stand up 미팅 활용한 도구로 활용
### 2. Slack
- 팀원간의 실시간 소통 창구
### 3. Notion
 - 회의정리 기록, 오늘의 공유/질문 사항, 현재 진행 사항, blocker 공유, 기능 단위 페이지 셍성 후 공유 및 기록

<table>
  <tbody>
    <tr>
      <td align="center">
        <img width="789" alt="image" src="https://user-images.githubusercontent.com/97422072/181877342-8c063ea9-3554-477b-b2b8-31b9fd5ba3bc.png">      
      </td>
      <td align="center">
        <img width="789" alt=image" src="https://user-images.githubusercontent.com/83544570/184545339-9336d126-243e-4daa-85b1-fb4044844dbd.jpg">      
       </td>
    </tr>
      <tr>
      <td align="center">
        <img width="789" alt="image" src="https://user-images.githubusercontent.com/78889402/182009668-6eae4b48-6a83-4108-8bd5-4804c922e40b.png">      
      </td>
      <td align="center">
        <img width="789" alt="image" src="https://user-images.githubusercontent.com/78889402/182009676-f1ad22d5-6cc0-4696-9f16-eeb9fb5e3d80.png">      
      </td>
      </tr>
                                                                                                                                                 <tr>
  <td align="center">
        <img width="789" alt="image" src="https://user-images.githubusercontent.com/78889402/182009685-15ed7167-ab01-40f6-9c9e-50b1d0dab04f.png">      
      </td>
                                                                                                                                                <td align="center">
        <img width="789" alt="image" src="https://user-images.githubusercontent.com/78889402/182009689-a6b7a855-8c35-4576-a473-566983dad21a.png">      
      </td>
                                                                                                                                                 </tr>
    
  </tbody>
</table>


## 7. 프로젝트 회고
### ✈️ 회고록
- [🐥 노정은님 회고록(1) - 기능 구현에 대한 회고](https://jeongeuni.tistory.com/47?category=1103401)  <br />
- [🐥 노정은님 회고록(2) - 팀 프로젝트에 대한 회고](https://jeongeuni.tistory.com/48?category=1103401)  <br />

#

### Reference

- 이 프로젝트는 Magazine B 참조하여 학습목적으로 만들었습니다.
- 학습용으로 만들었기 때문에 이 코드를 활용하여 이득을 취하거나 무단 배포할 경우 법적으로 문제될 수 있습니다.
- 이 프로젝트에서 사용하고 있는 사진 대부분은 위코드에서 구매한 것이므로 해당 프로젝트 외부인이 사용할 수 없습니다.
