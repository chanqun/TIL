flex css

### justify-content

```css
flex-start: 요소들을 컨테이너의 왼쪽으로 정렬
flex-end: 요소들을 컨테이너의 오른쪽으로 정렬
center: 요소들을 컨테이너의 가운데로 정렬
space-between: 요소들 사이에 동일한 간격을 둠
space-around: 요소들 주위에 동일한 간격을 둠
```


### align-items

```css
flex-start: 요소들을 컨테이너의 꼭대기로 정렬
flex-end: 요소들을 컨테너의 바닥으로 정렬
center: 요소들을 컨테이너의 세러선 상의 가운제로 정렬
baseline: 요소들을 컨테이너의 시작 위치에 정렬
stretch: 요소들을 컨테이너에 맞도록 늘림
```

### flex-direction

```css
row: 요소들을 텍스트의 방향과 동일하게 정렬
row-reverse: 요소들을 텍스트의 반대 방향으로 정렬
column: 요소들을 위에서 아래로 정렬
column-reverse: 요소들을 아래에서 위로 정렬
```

### order

order 속성 바꾸면 순서 변경 가능 음수도 가능
default는 0이다.

### align-self
각각 요소 변경 가능

### flex-wrap
```css
nowrap: 모든 요소들을 한 줄에 정렬
wrap: 요소들을 여러 줄에 걸쳐 정렬
wrap-reverse: 요소들을 여러 줄에 걸쳐 반대로 정렬
```

### flex-flow
합쳐서 사용하기도 함
```css
flex-flow: column wrap;
```

### align-content

```css
flex-start: 여러 줄들을 컨테이너의 꼭대기에 정렬
flex-end: 여러 줄들을 컨테이너의 바닥에 정렬
center: 여러 줄들을 세로선 상의 가운데에 정렬
space-between: 여러 줄들 사이에 동일한 간격
space-around: 여러 줄들 주위에 동일한 간격
stretch: 여러 줄들을 컨테이너에 맞도록 늘림
```