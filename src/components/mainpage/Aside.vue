<template>
    <div>
        <aside class="asideBlock">
            <div class="professionality font-white">
                <div class="expert">Эксперт</div>
                <div class="indicator"><img src="../../../public/images/Vector.png"></div>
                <div class="bread"><img src="../../../public/images/Vector-2.png"></div>
            </div>
            <nav class="nav font-white">
                <a class="el" v-for="item in list" :key="item.id">
                    <img :src="item.url">
                    <span>{{ item.title }}</span>
                </a>
            </nav>
        </aside>
    </div>
</template>

<script>
    export default {
        name: "Aside",
        data() {
            return{
                list: [
                    {
                        id: "1",
                        title: "Аналитика",
                        url: "./images/nav/Group-1.png"
                    },
                    {
                        id: "2",
                        title: "Продукт",
                        url: "./images/nav/Group.png"
                    },
                    {
                        id: "3",
                        title: "Пользователи",
                        url: "./images/nav/Group-2.png"
                    },
                    {
                        id: "4",
                        title: "Конструктор сайта",
                        url: "./images/nav/Group-3.png"
                    },
                    {
                        id: "5",
                        title: "Трансляции",
                        url: "./images/nav/Group-6.png"
                    },
                    {
                        id: "6",
                        title: "Рассылка",
                        url: "./images/nav/Group-8.png"
                    },
                    {
                        id: "7",
                        title: "Маркетинг",
                        url: "./images/nav/Group-7.png"
                    },
                    {
                        id: "8",
                        title: "Платежи",
                        url: "./images/nav/Group-4.png"
                    },
                    {
                        id: "9",
                        title: "Настройки",
                        url: "./images/nav/Group-5.png"
                    },
                ]
            };
        },
    }
</script>

<style scoped>
    .font-white {
        background-color: white;
    }

    .asideBlock {
        width: max-content;
        height: max-content;
        margin-left: 38px;
        border-radius: 10px;
        margin-right: 90px;
    }

    .professionality {
        display: flex;
        flex-direction: row;
        width: 248px;
        height: 54px;
        margin-bottom: 11px;
    }

    .expert {
        display: flex;
        justify-content: center;
        align-items: center;
        width: 124px;
        height: 100%;
        margin: 0;
    }

    .indicator {
        display: flex;
        justify-content: center;
        align-content: center;
        align-items: center;
        width: 53px;
        height: 100%;
        margin: 0;
    }

    .indicator img {
        width: 11px;
        height: 8px;
    }

    .bread {
        display: flex;
        justify-content: center;
        align-items: center;
        width: 71px;
        height: 100%;
        margin: 0;
        left: 10px;
    }

    .bread img {
        width: 24px;
        height: 24px;
    }

    .nav {
        display: flex;
        flex-direction: column;
        align-items: center;
        justify-content: space-between;
        width: 177px;
        height: 847px;
        border-radius: 10px;
    }

    .el {
        display: flex;
        flex-direction: column;
        justify-content: center;
        align-content: center;
        justify-items: center;
        align-items: center;
        width: 158px;
        height: 85px;
        border: 1px solid #E7ECF1;
        border-radius: 6px;
        transition: all .3s ease-in-out;
        cursor: pointer;
    }

    .el:hover {
        border: 1px solid #FF7439;
    }

    .el img {
        width: 24px;
        height: 24px;
        margin-bottom: 12px;
    }

    .el:first-child {
        margin-top: 9px;
    }

    .el:last-child {
        margin-bottom: 9px;
    }

    .el:nth-child(3) {
        border: 1px solid #FF7439;
        color: #FF7439;
    }
</style>