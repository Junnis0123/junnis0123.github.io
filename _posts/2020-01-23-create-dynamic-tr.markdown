---
layout: post
title:  "Vue.js 동적으로 3중 배열 rowspan 만들기"
date:   2020-01-23 01:47:31 +0900
subtitle: "Vue.js"
tag: "Vue.js"
---



안녕하세요, 김선진입니다!

vue 에는 동적 엘리먼트 생성 방식으로 아주 쉽고 유용한 v-for 구문이 있습니다.

이 v-for를 이용해서 rowspan이 있는 테이블을 만드는 방법에 대해 작성해보려고 합니다.



# 기존 테이블 구조 생각하기

html으로 table에 rowspan을 생성하는 방법을 생각하면 쉽게 만들 수 있습니다.

rowspan이 딱 하나 있을 경우는 아주 쉽죠.

```html
<tr>
	<td rowspan="3">3칸 행 병합</td>
	<td>예시</td>
	<td>예시</td>
	<td>예시</td>
</tr>
<tr>
	<td>예시2</td>
	<td>예시2</td>
	<td>예시2</td>
</tr>
```

첫 행에서 병합할 셀에 대해서만 rowspan 속성값을 병합하고 싶은 행 개수 만큼 주면 됩니다.



그러나 데이터가 3중이라면 어떻게 될까요?
```html
<tr>
	<td rowspan="3">3칸 행 병합</td>
	<td>예시</td>
	<td>예시</td>
	<td>예시</td>
</tr>
<tr>
	<td rowspan="2">2칸 더 병합</td>
	<td>예시2</td>
	<td>예시2</td>
</tr>
<tr>
	<td>예시3</td>
	<td>예시3</td>
</tr>
```


html으로도 구조가 조금 복잡해 보입니다.

이 3중 병합 테이블을 v-for를 이용해서 rowspan을 만드는 방법에 대해 작성해보려 합니다.



# Vue File 생성하기

```javascript
  			/**
             * Calculate RowSpan
             * @returns {*}
             */
            getRowspan(order){
                return order.reduce(
                    (previousItem, currentItem) => previousItem + currentItem.optionList.length, 0
                );
            }
```
첫 Rowspan의 경우 두번째 배열의 길이를 전부 더한(row 개수를 수동으로 계산) 값을 넘겨주어야 하기 때문에,

Row 개수를 계산하는 간단한 함수를 작성해주세요.

Rowspan을 계산해 주었다면 3중 포문을 돌아 3중 배열을 만드는 코드 템플릿을 작성해주세요.



주의하실 점은,

해당 코드는 무조건 Root 제외한 위치에 있어야 합니다.

저는 tbody 내부에 집어넣었습니다.


```html
   <template v-for="(itemList, i) in orderList">
   		<template  v-for="(item, j) in itemList">
        	<tr v-for="(option, k) in item.optionList" :key="k">
            	<!-- 최상위 로우스팬 -->
            	<td v-if="gIdx === 0 && idx === 0"
                :rowspan="getRowspan(itemList)">
                	{{item.name}}
                </td>
            	<!-- 2단 로우스팬 -->
                <td v-if="k === 0"
                :rowspan="item.optionList.length">
                	{{option.name}}
                </td>
                <!-- 정상 tr -->
                <td>
                	1Row
                </td>
```
템플릿을 사용해서 2중 v-for 를 먼저 만들어주시고, 이후에 로우스팬을 만들어주면 됩니다.

참 쉽죠?



이렇게 해주면 3중 배열을 테이블로 예쁘게 그려줄 수 있습니다.

다들 즐거운 뷰 코딩 하세요.
