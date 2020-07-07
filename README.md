# hover시 두개로 분리되며 나타나는 애니메이션

## 1. preview

<img src="https://j.gifs.com/oVoNEB.gif" />


## 2. 코드 분석

### 1) html

#### (1) item요소에서 front, back이 자식요소이다.
- front 요소 : 이미지(`img태그`)와 제목을 담는 요소(`h2태그`)로, 메인으로 보이는 요소이다.
- back 요소 : 설명(`p태그`)을 담는 요소로, `hover`전에는 뒤에 숨겨져 있다.


```html
<body>
    <div class="items">
        <div class="item">
            <div class="front">
                <img src=""/>
                <h2>Hi android</h2>
            </div>
            <div class="back">
                <p>
                    안드로이드는 휴대 전화를 비롯한 휴대용 장치를 위한 운영 체제와 미들웨어, 사용자 인터페이스 그리고 표준 응용 프로그램
                </p>
                <a href="#none">Read More</a>
            </div>
        </div>
    </div>
</body>

```

<br/><br/>

### 2) css

#### (1)  items를 중앙 정렬시키기
- `position: absolute`를 사용하여 내부에 있는 아이템들을 살짝 아래로 위치시킨다.
- `top : 50%`, `left: 50%`을 사용하여 내부에 있는 아이템들을 살짝 아래로 위치시킨다.
- `transform: translate(-50%, -50%)`를 사용하여, 페이지 크기가 변해도 좌우상하 중앙에 위치시킨다.
            
```css
    body{                                 /*body디자인*/
            background-color: #222;
            color : #fff;
            font-weight: 300;
            font-size: 15px;
        }
        a{
            color : #222;                /*a태그 디자인*/
            text-decoration: none;
        }

        .items{
            text-align : center;
            position: absolute;         /*내부에 있는 아이템들을 살짝 아래로 위치시키기(1)*/
            top : 50%;                  /*내부에 있는 아이템들을 살짝 아래로 위치시키기(2)*/
            left: 50%;
            transform: translate(-50%, -50%); /*좌우상하 중앙에 위치시킴*/
        }
```

<br/>

#### (2)  items의 자식인 item 위치 설정하기
- `height: 200px`을 하는데, `item`의 자식요소 `front`로, `front`도 `item`처럼 같은 높이로 하기위해 `inherit`를 쓴다.
- `item`의 자식은 `front`, `back`이 있는데, item의 속성을 `position: relative`를 해서 `front`, `back`이 겹쳐있게 하기위한 기준으로 사용한다.
            
```css
        .item{
            width: 300px;
            height: 200px;            /*inherit(1)*/
            display: inline-block;    /*가로 정렬*/
            position: relative;       /*중앙의 기준(1)*/
        }
```


<br/>

#### (2)  img태그를 자식으로 하는 front태그
- 만약, 자식요소를 부모와 같은 크기로 하려면?
  
  : `position: absolute`가 되면 모든 요소가 inline이 됨. 그러므로 `front`요소의 크기를 부모와 같은 크기로 만들기 위해 100%로 width를 줌
         
 <br/>
        
- 다른 태그보다 상위에 위치하기 하기
 
  : `z-index: 1`을 해서 back보다 앞에 있게 한다.
 
```css
        .front{
            background-color: #333;
            text-align: center;
            width: 100%;            /*부모와 같은 크기로 만들기*/
            z-index: 1;             /*back보다 앞에 있게 함*/
        }
        
        .front img{
            width: 200px; 
            height: inherit;       /*item의 자식요소 front-> front도 item처럼 같은 높이=> inherit(2)*/
           
        }
```

<br/>

- front태그의 img, h2에 디자인, 애니메이션 넣기
 
  : `.item:hover .front img`을 사용해서 `item이 hover`됐을 때 `front의 img`에 효과를 준다.
 

```css
        .item:hover .front img{                 /*아이템에 호버됐을 때 이미지가 커지게*/
            animation: ani 1s linear infinite;  /*ani는 제일 밑에 keyframe으로 만든 애니메이션*/
        }
        
        .front h2{
        margin-top: 0;                          /*h2는 마진자체가 default로 들어감 -> 마진으로 줄인다.*/
            
        }

```

<br/>

#### (3)  애니메이션 넣기

- `front`, `back`에 동일한 이벤트 적용하기

  : `transition: 0.5s`을 하면 요소들이 이벤트가 발생하면 모든지 0.5초뒤에 일어난다는 사전설정이다.
  
     여기서 주의할 점은 0.5초뒤에 이벤트가 발생하게 하려면, 만약 `top`값이 0.5초뒤에 변하게 하려면 
     
     원래 `front`, `back`요소에 `top`이라는 속성이 있어야, 0.5초뒤에 `top`값이 변하게 만들 수 있다.

<br/>

- 상하로 이동하는 `front`, `back` 애니메이션

  : `item`이라는 부모요소에 마우스가 올라가면 `front`는 50% 위인 위치로, `back`는 50% 아래인 위치로 이동하게 된다.
   
  : 현재 `front`, `back` 둘다 `position이 absolute`이므로 좌표로 이동이 가능한 상태이다.
 
 
```css
        /*공통되는 기능을 아래와 같이 작성*/
        .front,
        .back{
            position: absolute;                /*상위태그(item)이 relative이므로, absolute로 고정위치 가능*/
            transition: 0.5s;                  /*top이 있어야 이벤트 적용가능*/
            top : 0;
        }
        .item:hover .front{
            top : -50%;                       /*item에 hover시 아래 위로 이동하는 애니메이션(1)*/

        }
        .item:hover .back{                    /*item에 hover시 아래 위로 이동하는 애니메이션(1)*/
            top : 50%; 
            opacity: 1;                       /*서서히 나타나는 효과*/
            
        }
```

<br/>

- back에 애니메이션 적용하기 
 
```css
        .back{
            background-color: #fff;
            color : #000;
            height: inherit;              /*item의 자식요소 front-> back도 item처럼 같은 높이=> inherit(3)*/
            text-align: center;
            padding: 20px;
            box-sizing: border-box;        /*padding을 줘도 박스 자체의 사이즈는 변하지 않게 한다.*/
            /*position: absolute;  기준(3)*/
            opacity: 0;
        }
      
        .back a{
            background-color: yellowgreen;
            color : #fff;
            padding: 5px 15px;
            border-radius: 20px;            /*동그랗게*/
            transition: 0.5s;
            
        }
        .back a:hover{
          background-color: #000;

        }

```


<br/>

#### (4)  keyframe 애니메이션 만들기

- 이미지가 커졌다 작아졌다 하게 하는 애니메이션이다.
- scale을 사용하면 크기를 키우거나 줄일 수 있다.

```css
       @keyframes ani{
            0%, 100% {
                transform: scale(1);  /*기본값이므로 이 줄의 코드를 쓰지 않아도 됨*/
            }
            50% {
                transform: scale(1.1);  /*커지게*/
            }
            
        }

```
