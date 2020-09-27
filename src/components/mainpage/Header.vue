<template>
    <div>
        <header class="header">
            <div class="logo">
                <img src="../../../public/images/OJOWO.png"/>
                <span>Beta</span>
            </div>
            <div class="header-work">
                <div class="font-white call"><img src="../../../public/images/Group.png"></div>
                <div class="font-white profile">Артем <img src="../../../public/images/Vector.png"></div>
                <div class="font-grey search"><img src="../../../public/images/Group2.png"></div>
                <div class="font-grey lang">Ru<img src="../../../public/images/Vector.png"></div>
            </div>
        </header>
    </div>
</template>

<script>
    export default {
        name: "Header"
    }
</script>

<style scoped>
    .header {
        display: flex;
        justify-content: space-between;
        font-size: 14px;
        margin-top: 10px;
        margin-bottom: 48px;
    }

    .logo {
        display: flex;
        justify-content: center;
        align-items: center;
        align-content: center;
    }

    .logo img {
        margin-right: 9px;
        margin-left: 38px;
    }

    .logo span {
        margin-top: 5px;

    }

    .header-work {
        display: flex;
    }

    .header-work div {
        margin-right: 17px;
    }

    .header-work div:last-child {
        margin-right: 38px;
    }

    .font-white {
        background-color: white;
    }

    .font-grey {
        background-color: rgba(203, 215, 228, 0.4);
    }

    .call {
        display: flex;
        justify-content: center;
        align-items: center;
        border-radius: 50%;
        width: 43px;
        height: 43px;
    }

    .profile {
        display: flex;
        justify-content: center;
        align-items: center;
        border-radius: 100px;
        width: 120px;
        height: 43px;
    }

    .search {
        display: flex;
        justify-content: center;
        align-items: center;
        border-radius: 50%;
        width: 43px;
        height: 43px;
    }

    .lang {
        display: flex;
        justify-content: center;
        align-items: center;
        border-radius: 100px;
        width: 64px;
        height: 43px;
    }
</style>