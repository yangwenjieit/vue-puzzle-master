<template>
    <div class="box">
        <ul class="puzzle-wrap">
            <li
                    :class="{'puzzle': true, 'puzzle-empty': !puzzle}"
                    v-for="puzzle in puzzles"
                    v-bind:style="{backgroundImage: 'url(' + puzzle.pic + ')','background-repeat':'no-repeat','background-size':'cover' }"
                    @click="moveFn($index)"
            ></li>
        </ul>
        <button class="btn btn-warning btn-block btn-reset" @click="render">重置游戏</button>
    </div>
</template>

<script>
    export default {
        data () {
            return {
                puzzles: [],
            }
        },
        methods: {

            // 重置渲染
            render () {
                let puzzleArr = [],
                    puzzleContent = [],
                    i = 1,j=0

                // 生成包含1 ~ 9数字的数组
                for (i; i < 9; i++) {
                    puzzleArr.push(i)
                }

                // 随机打乱数组
                puzzleArr = puzzleArr.sort(() => {
                    return Math.random() - 0.5
                });

                for (j;j<puzzleArr.length;j++){
                    let content = {text:puzzleArr[j],pic:'../../src/assets/img/'+puzzleArr[j]+'.png'}
                    puzzleContent.push(content)
                }

                // 页面显示
                this.puzzles = puzzleContent
                this.puzzles.push('')
            },

            // 点击方块
            moveFn (index) {

                // 获取点击位置及其上下左右的值
                let curNum = this.puzzles[index],
                    leftNum = this.puzzles[index - 1],
                    rightNum = this.puzzles[index + 1],
                    topNum = this.puzzles[index - 3],
                    bottomNum = this.puzzles[index + 3]

                // 和为空的位置交换数值
                if (leftNum === '' && index % 3) {
                    this.puzzles.$set(index - 1, curNum)
                    this.puzzles.$set(index, '')
                } else if (rightNum === '' && 2 !== index % 3) {
                    this.puzzles.$set(index + 1, curNum)
                    this.puzzles.$set(index, '')
                } else if (topNum === '') {
                    this.puzzles.$set(index - 3, curNum)
                    this.puzzles.$set(index, '')
                } else if (bottomNum === '') {
                    this.puzzles.$set(index + 3, curNum)
                    this.puzzles.$set(index, '')
                }

                this.passFn()
            },

            // 校验是否过关
            passFn () {
                if (this.puzzles[8] === '') {
                    const newPuzzles = this.puzzles.slice(0, 8)

                    const isPass = newPuzzles.every((e, i) => e.text === i + 1)

                    if (isPass) {
                        let message = ['给你一个超大的么么哒！','厉害了我的丹妹儿！','今天有想我吗？','想我就可以再玩一次！','爱你哟'];
                        var num = Math.random()*5 + 0;
                        num = parseInt(num, 10);
                        alert(message[num]);
                    }
                }
            },
            showAlert() {
                this.$refs.alert.show();
            }
        },
        ready () {
            this.render()
        }
    }
</script>

<style>
    @import url('./assets/css/bootstrap.min.css');

    body {
        font-family: Arial, "Microsoft YaHei";
    }

    .box {
        width: 400px;
        margin: 50px auto 0;
    }

    .puzzle-wrap {
        width: 300px;
        height: 300px;
        margin-bottom: 40px;
        margin-left:50px;
        padding: 0;
        background: #ccc;
        list-style: none;
    }

    .puzzle {
        float: left;
        width: 100px;
        height: 100px;
        font-size: 20px;
        text-align: center;
        line-height: 100px;
        border: 1px solid #ccc;
        box-shadow: 1px 1px 4px;
        text-shadow: 1px 1px 1px #B9B4B4;
        cursor: pointer;
    }

    .puzzle-empty {
        background: #ccc;
        box-shadow: inset 2px 2px 18px;
    }

    .btn-reset {
        box-shadow: inset 2px 2px 18px;
    }
    .btn-block {
        display: block;
        width: 75%;
        margin-left:50px;
    }
</style>