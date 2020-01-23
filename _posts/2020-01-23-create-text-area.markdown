---
layout: post
title:  "[Vue.js] 이모지 입력이 되지 않는 TextArea Component 만들기"
date:   2020-01-08 15:37:31 +0900
---

안녕하세요, 김선진입니다!
회사에서 개발 프로젝트를 진행하던 중, 이모지 입력을 막아야 하는 일이 있었는데요,
필터나 다른 귀찮은 방법 대신 mixin을 적용해서 이모지 입력만 막는 커스텀 텍스트 에리어 컴포넌트를 만들었습니다.
차근차근 커스텀 컴포넌트 만들기에 대해 작성해보겠습니다!

# Custom TextArea Component 만들기
## vue file 만들기
우선, 커스텀 컴포넌트를 만들 뷰파일을 하나 생성해주세요. 이름은 CustomTextArea 로 하겠습니다.
<template>
    <textarea
        :value="text"
        v-on="Listeners" />
</template>
template에 아주 간단하게 textarea를 하나 만들어주세요.
필요에 따라 class, maxlenght, placeholder 등을 따로 지정해 주심 됩니다.

## mixin 만들기
export default function emojiRemover(text = '') {
    const regex = /[\u{1f300}-\u{1f5ff}\u{1f900}-\u{1f9ff}\u{1f600}-\u{1f64f}\u{1f680}-\u{1f6ff}\u{2600}-\u{26ff}\u{2700}-\u{27bf}\u{1f1e6}-\u{1f1ff}\u{1f191}-\u{1f251}\u{1f004}\u{1f0cf}\u{1f170}-\u{1f171}\u{1f17e}-\u{1f17f}\u{1f18e}\u{3030}\u{2b50}\u{2b55}\u{2934}-\u{2935}\u{2b05}-\u{2b07}\u{2b1b}-\u{2b1c}\u{3297}\u{3299}\u{303d}\u{00a9}\u{00ae}\u{2122}\u{23f3}\u{24c2}\u{23e9}-\u{23ef}\u{25b6}\u{23f8}-\u{23fa}]/gu;
    return text.replace(regex, '');
}
emojiRemover mixin을 js file로 만들어주세요. regex는 한글 자모음을 제외하고 이모지만 해당되게 뽑았습니다.
아주 힘들었어요.

## script 짜기
<script>
import emojiRemover from '../../filters/emojiRemover';

export default {
    name: 'TextArea',
    props: {
        // v-model
        value: {},
    },
    computed: {
        Listeners() {
            const vm = this;
            return {
                ...this.$listeners,
                input(event) {
                    const text = vm.filteringText(event.target.value);
                    vm.$forceUpdate();
                    vm.$emit('input', text);
                },
            };
        },
        text() {
            return this.filteringText(this.value);
        },
    },
    methods: {
        filteringText(value) {
            return emojiRemover(value);
        },
    },
};
</script>
v-model 기능을 제공하기 위해 value를 props로 받아주시고, computed에 Listeners 속성을 만들어주세요.
리스너는 원본 textarea에 v-on 으로 붙여주세요.

input event에서 target.value를 필터링 처리 해서 emit 하여 v-model로 넘겨줍시다.
filteringText 함수에서는 mixin 함수를 호출합니다.

적당히 CSS 작성해주시면 이모지 입력이 되지 않는 텍스트에리어 완성!
응용해서 이모지 말고 다른것들또한 입력되지 않는 상태를 만들어 줄 수 있습니다.
예를 들어 숫자만 입력받고 싶을 때는 필터 텍스트에 해당 구문을 추가해주세요.

   const resultString = String(resultString)
                    .replace(/[^0-9]/g, '');

이상으로 regex를 이용해서 커스텀 (인풋 제한) 텍스트 인풋을 만드는 방법이었습니다~
제한시켜둔 글자는 입력조차 되지 않습니다.

그럼 이만!

