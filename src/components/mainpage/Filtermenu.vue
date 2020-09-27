<template>
    <div>
        <form class="item bold">
            <div class="radio-box"><input class="radio" type="radio"></div>
            <div class="User">Пользователь</div>
            <div class="reg-date">Дата регистрации</div>
            <div class="last-activity">Последняя активность</div>
            <div class="last-action">Последнее действие</div>
            <div class="product">Продукт</div>
            <div class="found"><span>Найдено</span><span>63</span></div>
        </form>
        <form class="item"
              v-for="(user, id) in this.$parent.list"
              :key="id"
        >
            <div class="radio-box"
                 @click="user.isActive = !user.isActive"
            ><input class="radio"
                    type="radio"
                    :checked="user.isActive"
            ></div>
            <div class="User">
                <div>{{ user.name }}</div>
                <div>{{ user.mail }}</div>
            </div>
            <div class="reg-date"> {{ dateCreation(user.regDate) }}</div>
            <div class="last-activity"> {{ dateCreation(user.lastActivity) }} </div>
            <div class="last-action gradient-text">{{ user.lastActions }}</div>
            <div class="product gradient-text"> {{ user.product }} </div>
            <div class="found"><img src="../../../public/images/GroupMark.png"><img src="../../../public/images/GroupDel.png"></div>
        </form>
    </div>
</template>

<script>
    export default {
        name: "Filtermenu",
        methods: {
            dateCreation: function(data) {
                let date = Number(data.split('-')[0]),
                    month = Number(data.split('-')[1]),
                    year = Number(data.split('-')[2]);

                return `${this.$parent.monthsName[month - 1]} ${date}, ${year}`
            }
        },
        updated() {
            this.dateCreation('1-9-2020');
        }
    }
</script>

<style scoped>
    .item {
        display: grid;
        grid-template-columns: repeat(7, 4.88% 15.85% 15.85% 15.85% 15.85% 15.85% 15.85%);

    }

    .bold {
        font-weight: bold;
        margin-bottom: 20px;
    }

    .radio-box {
        display: flex;
        justify-content: center;
        align-items: center;
        width: 62px;
        height: 55px;
        margin-left: 11px;
    }

    .radio {
        width: 26px;
        height: 26px;
    }

    .User {
        display: flex;
        justify-content: center;
        align-items: flex-start;
        flex-direction: column;
        margin-left: 37px;
    }

    .User div:last-child {
        color: grey;
    }

    .reg-date {
        display: flex;
        justify-content: center;
        align-items: center;
    }

    .last-activity {
        display: flex;
        justify-content: flex-start;
        align-items: center;
        margin-left: 14px;
    }

    .last-action {
        display: flex;
        justify-content: flex-start;
        align-items: center;
        margin-left: 13px;
    }

    .product {
        display: flex;
        justify-content: flex-start;
        align-items: center;
    }

    .gradient-text {
        background: linear-gradient(to right, black 0% 50%, #dbdee1 100%);
        -webkit-background-clip: text;
        -webkit-text-fill-color: transparent;
        color: black;
    }

    .found {
        display: flex;
        justify-content: center;
        align-items: center;
        margin-left: 8px;
    }

    .found img:first-child {
        margin-right: 33px;
        margin-left: 26px;
    }
    .found span:first-child {
        padding-right: 12px;
    }
</style>