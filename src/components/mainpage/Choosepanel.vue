<template>
    <div>
        <div class="filt font-white">
            <div class="filter-choose">
                <div class="filter-choose-side">
                    <div class="date-choose"
                         @click="isChangeDate()"
                    >
                        <img src="../../../public/images/Group1.png">
                        <div class="extraVariantsChosen">{{ chosenDay }}</div>
                        <img src="../../../public/images/Vector.png">
                        <div class="extraVariants font-white"
                             v-show="isAutoDayPicked">
                            <div class="extraVariantsEl"
                                 v-for="(item, id) in variants"
                                 :key="id"
                                 @click="changePeriod(id)"
                            >{{ item }}</div>
                        </div>
                    </div>
                    <div
                            @click="this.$parent.isActiveC"
                            class="calendar-pick">
                        <img src="../../../public/images/Group3.png">
                        <div>Фильтр</div>
                        <img src="../../../public/images/Vector.png">
                    </div>
                </div>
                <div class="button-side">
                    <button class="export">Выгрузить</button>
                    <button class="add">Добавить контакты</button>
                </div>
            </div>
            <div class="filter-result"
                 v-if="isFiltered"
            >
                <div class="filterBar"
                    v-for="(el, id) in this.$parent.filteredPeriods"
                     :key="id"
                >{{ showDateFilterBar(el) }}
                    <button class="filterbtn" @click="deleteFilterPeriod(id)">
                        <img src="../../../public/images/GroupCrest.png">
                    </button>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
    export default {
        name: "Choosepanel",
        data() {
            return{
                isAutoDayPicked: false,
                variants: ["За 1 день", "За 7 дней", "За 30 дней", "За весь период"],
                chosenDay: "Выберите период",
            };
        },
        methods: {
            showDateFilterBar: function(el) {
                let first = el[0],
                    firstNum = Number(first.split('-')[0]),
                    firstMonth = Number(first.split('-')[1]),
                    year = Number(first.split('-')[2]),
                    month1 = this.$parent.monthsName[firstMonth - 1].toLowerCase().slice(0, 3);

                if(el.length === 2) {
                    let second = el[1],
                        secondNum = Number(second.split('-')[0]),
                        secondMonth = Number(second.split('-')[1]),
                        month2 = this.$parent.monthsName[secondMonth - 1].toLowerCase().slice(0, 3);

                    if(firstNum < secondNum && firstMonth === secondMonth) {
                        return (`${firstNum} ${month1} - ${secondNum} ${month2} ${year} г.`);
                    }
                    if(firstNum > secondNum && firstMonth === secondMonth) {
                        return (`${secondNum} ${month2} - ${firstNum} ${month1} ${year} г.`);
                    }
                    if(firstMonth > secondMonth) {
                        return (`${secondNum} ${month2} - ${firstNum} ${month1} ${year} г.`);
                    }
                    if(firstMonth < secondMonth) {
                        return (`${firstNum} ${month1} - ${secondNum} ${month2} ${year} г.`);
                    }
                } else {
                    return (`${firstNum} ${month1} ${year} г.`);
                }
            },
            changePeriod: function(id) {
                this.chosenDay = this.variants[id];
                // let variants = document.querySelectorAll('.extraVariantsEl');
                this.$parent.filteredPeriods = [];
                switch(id) {
                    case 0:
                        this.$parent.filterDates.push(`${this.$parent.curDate[2]}-${this.$parent.months.indexOf(this.$parent.curDate[1]) + 1}-${this.$parent.curDate[3]}`);
                        break;
                    case 1:
                        this.$parent.filterDates.push(`${Number(this.$parent.curDate[2])}-${this.$parent.months.indexOf(this.$parent.curDate[1]) + 1}-${this.$parent.curDate[3]}`);
                        this.$parent.filterDates.push(`${Number(this.$parent.curDate[2]) - 6}-${this.$parent.months.indexOf(this.$parent.curDate[1]) + 1}-${this.$parent.curDate[3]}`);
                        break;
                    case 2:
                        this.$parent.filterDates.push(`${Number(this.$parent.curDate[2])}-${this.$parent.monthsName.indexOf(this.$parent.month) + 1}-${this.$parent.year}`);
                        this.$parent.filterDates.push(`${this.$parent.curDate[2]}-${this.$parent.monthsName.indexOf(this.$parent.month)}-${this.$parent.year}`);
                        break;
                    case 3:
                        this.$parent.filterDates = ["1-1-2020", "31-12-2020"];
                        break;
                }
                this.$parent.AddToFilter();
                this.$parent.isActiveC();
                this.isChangeDate();
            },
            isChangeDate: function() {
                this.isAutoDayPicked = !this.isAutoDayPicked;
                let el = document.querySelectorAll('.date-choose img'),
                    div = document.querySelectorAll('.date-choose .extraVariantsChosen')[0];
                if(this.isAutoDayPicked) {
                    el[0].src = './images/GroupOrange.png';
                    div.classList.add('orange');
                    el[1].src = './images/Vector33.png';
                } else {
                    el[0].src = './images/Group1.png';
                    div.classList.remove('orange');
                    el[1].src = './images/Vector.png';
                }
            },
            isFilteredPeriodsLength: function() {
                if(this.$parent.filteredPeriods.length === 0) {
                    this.chosenDay = 'Выберите период';
                }
            },
            deleteFilterPeriod: function(id) {
                this.$parent.filteredPeriods.splice(id, 1);
            }
        },
        computed: {
            isFiltered: function() {
                return this.$parent.filteredPeriods.length > 0 ? true : false;
            }
        },
        updated() {
            this.isFilteredPeriodsLength();
        }
    }
</script>

<style scoped>
    .font-white {
        background-color: white;
    }

    .filt {
        width: 1269px;
        max-height: 154px;
        min-height: 85px;
        border-radius: 10px;
        margin-bottom: 10px;
    }

    .filter-choose {
        display: flex;
        flex-direction: row;
        justify-content: space-between;
        align-items: center;
        align-content: center;
        width: 100%;
        height: 85px;
    }

    .filter-choose-side {
        display: flex;
        flex-direction: row;
        height: 100%;
    }

    .date-choose {
        display: flex;
        flex-direction: row;
        justify-content: center;
        align-items: center;
        align-content: center;
        width: 196px;
        height: 100%;
        margin-right: 15px;
        margin-left: 7px;
    }

    .date-choose img:first-child {
        margin-left: 0px;
        margin-right: 19px;
    }

    .extraVariantsChosen {
        margin-right: 8px;
    }

    .orange {
        color: #FF7439;
    }

    .extraVariants {
        display: flex;
        flex-direction: column;
        justify-content: flex-start;
        align-content: center;
        position: absolute;
        width: 169px;
        height: 200px;
        margin-top: 255px;
        border-radius: 10px;
    }

    .extraVariantsEl {
        display: flex;
        justify-content: center;
        align-items: center;
        width: 169px;
        height: 100%;
        border-radius: 10px;
    }

    .extraVariantsEl:hover {
        background-color: #F0F3F8;
    }

    .calendar-pick {
        display: flex;
        flex-direction: row;
        justify-content: flex-start;
        align-items: center;
        align-content: center;
        width: 169px;
        height: 100%;
    }

    .calendar-pick img:first-child {
        margin-right: 19px;
    }

    .calendar-pick div {
        margin-right: 11px;
    }

    .button-side {
        display: flex;
        margin-right: 42px;
    }

    .export {
        width: 120px;
        height: 45px;
        margin-right: 11px;
        border-radius: 7px;
        background-color: white;
        font-size: 14px;
        font-family: 'Montserrat', sans-serif;
    }

    .add {
        width: 200px;
        height: 45px;
        border-radius: 7px;
        background-color: #FF7439;
        color: white;
        font-size: 14px;
        font-family: 'Montserrat', sans-serif;
        border: none;
    }

    .filter-result {
        display: flex;
        flex-direction: row;
        justify-content: flex-start;
        flex-wrap: wrap;
        align-items: center;
        align-content: center;
        width: 100%;
        min-height: 69px;
        overflow-y: scroll;
        padding: 0 32px 13px 32px;
    }

    .filterBar {
        display: flex;
        justify-content: space-between;
        align-content: center;
        align-items: center;
        width: 200px;
        height: 41px;
        background-color: #586375;
        color: white;
        border-radius: 100px;
        padding-left: 23px;
        margin-right: 25px;
        margin-bottom: 5px;
    }

    .filterbtn {
        width: 27px;
        height: 27px;
        border-radius: 50%;
        background-color: #677489;
        outline: none;
        box-shadow: 0 0 0 rgba(0, 0, 0, 0);
        border: none;
        margin-right: 6px;
    }
</style>