<template>
    <div class="discoloration">
        <div class="header" :style="{ width: `${contentWidth}px` }">
            <div class="item">关卡：{{ level }}</div>
            <div class="item">当前步数：{{ currentSteps }}</div>
            <div class="item">总步数：{{ steps }}</div>
        </div>
        <div class="content">
            <div
                class="item item-color1"
                :class="item == 0 ? 'item-color1' : 'item-color2'"
                :style="{
                    height: `${(contentWidth - level) / level}px`,
                    width: `${(contentWidth - level) / level}px`,
                }"
                v-for="(item, key) in list"
                :key="key"
                @click="handleCurrentIndex(key)"
            ></div>
        </div>
    </div>
</template>

<script>
export default {
    name: "Discoloration",
    data() {
        return {
            contentWidth: 500,
            list: [],
            successList: [],
            level: 1,
            steps: 0,
            currentSteps: 0,
            isLoading: true,
        };
    },
    methods: {
        /**
         * 赋值
         */
        handleList() {
            this.list = [];
            for (let i = 0; i < this.level * this.level; i++) {
                this.list.push(0);
            }
            this.isFlag = true;
        },
        handleCurrentIndex(currentIndex) {
            this.steps++;
            //当前
            this.setList(currentIndex);
            // 上一级
            if (currentIndex > this.level - 1) {
                const upLevelIndex = currentIndex - this.level;
                this.setList(upLevelIndex);
            }
            // 下一级
            if (currentIndex < (this.level - 1) * this.level) {
                const downLevelIndex = currentIndex + this.level;
                this.setList(downLevelIndex);
            }
            //左
            if (currentIndex % this.level !== 0) {
                const leftLevelIndex = currentIndex - 1;
                this.setList(leftLevelIndex);
            }
            //右
            if ((currentIndex + 1) % this.level !== 0) {
                const rightLevelIndex = currentIndex + 1;
                this.setList(rightLevelIndex);
            }
            this.checkList();
        },
        checkList() {
            let isSuccess = true;
            for (let i = 0; i < this.list.length; i++) {
                if (this.list[i] === 0) {
                    isSuccess = false;
                }
            }
            if (isSuccess) {
                this.level++;
                this.handleList();
            }
        },
        /**
         * 修改
         */
        setList(index) {
            this.$set(this.list, index, this.handleVal(this.list[index]));
        },
        /**
         * 判断1还是0
         */
        handleVal(value) {
            let val = value === 1 ? 0 : 1;
            return val;
        },
    },
    created() {
        this.handleList();
    },
};
</script>

<style lang="scss">
.discoloration {
    position: fixed;
    height: 100%;
    width: 100%;
    display: flex;
    justify-content: center;
    flex-flow: column;
    align-items: center;
    background: #edf2f6;
    .header {
        display: flex;
        justify-content: space-between;
        padding-bottom: 10px;
    }
    .content {
        height: 500px;
        width: 500px;
        display: flex;
        flex-wrap: wrap;
        .item {
            border-right: 1px solid #eee;
            border-bottom: 1px solid #eee;
            cursor: pointer;
        }
        .item-color1 {
            background: #07be91;
        }
        .item-color2 {
            background: #e0c007;
        }
    }
    .footer {
        display: flex;
        justify-content: space-between;
        padding-top: 10px;
        button {
            margin-bottom: 7px;
        }
    }
}
</style>