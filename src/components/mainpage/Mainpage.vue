<template>
    <div>
        <div class="background-calendar" v-if="isActive" @click="isActiveC()"></div>
        <div class="calendar-box"
             v-show="isActive">
            <div class="period-picker font-white">
                <div class="item"
                     v-for="(el, id) in leftOptions"
                     :key="id"
                     @click="choosePeriod(id)"
                >{{ el }}</div>
            </div>
            <div class="calendar font-white">
                <div class="upperC">
                    <div class="leftV"
                         @click="onReduceCounter()"
                    ><img src="../../../public/images/Vector3.png">
                    </div>
                    <div class="MY">{{ month }}, {{ year }}</div>
                    <div class="rightV"
                         @click="onAddCounter()"><img
                            src="../../../public/images/Vector3.png"
                    ></div>
                </div>
                <div class="mainC">
                    <div class="dayW"
                         v-for="name in dayOfWeek"
                         :key="name">{{ name }}</div>
                    <div class="dayOuter"
                         v-for="(day, id) in calendar"
                         :key="id"
                         @click="chooseEl(id)"><div class="day"> {{ day }} </div></div>
                </div>
                <div class="downC">
                    <button class="cancel"
                            @click="clearCalendar()"
                    >Отмена</button>
                    <button class="renew"
                            :class="{ activeBtn: isActiveBtn }"
                            @click="AddToFilter()"
                    >Обновить</button>
                </div>
            </div>
        </div>
        <Header />
        <main class="main">
            <Aside />
            <section class="main-section">
                <Choosepanel />
                <div class="user-list font-white">
                    <Filtermenu />
                </div>
            </section>
        </main>
    </div>
</template>

<script>
    import Header from './Header.vue';
    import Aside from './Aside.vue';
    import Choosepanel from './Choosepanel.vue';
    import Filtermenu from './Filtermenu.vue';
    import axios from 'axios';

    export default {
        name: "Mainpage",
        components: {
            Header, Aside, Choosepanel, Filtermenu,
        },
        data() {
            return{
                filteredPeriods: [],
                isFilterActive: true,
                list: [],
                calendar: [],
                isActive: false,
                month: null,
                year: null,
                leftOptions: ["Весь срок", "Сегодня", "Вчера", "Последние 7 дней", "Последние 30 дней", "В этом месяце", "Прошлый месяц"],
                dayOfWeek: ["Пн", "Вт", "Ср", "Чт", "Пт", "Сб", "Вс"],
                counterMonth: 0,
                filterDates: [],
                isActiveBtn: false,

                curDate: new Date().toString().split(' ').splice(0, 4),
                monthsName:  ["Январь", "Февраль", "Март", "Апрель", "Май", "Июнь", "Июль", "Август", "Сентябрь", "Октябрь", "Ноябрь", "Декабрь"],
                months: ["Jan", "Feb", "Mar", "Apr", "May", "Jun", "Jul", "Aug", "Sep", "Oct", "Nov", "Dec"],
                monthsCode: [1, 4, 4, 0, 2, 5, 0, 3, 6, 1, 4, 6],
                daysOfWeek: ["Mon", "Tue", "Wed", "Thu", "Fri", "Sat", "Sun"],
                daysOfWeekCode: [2, 3, 4, 5, 6, 0, 1],
                daysInMonths: [],
                codeOfYear: null,
            };
        },
        methods: {
            onDaysInMonthsCount: function() {
                let day;
                Number(this.curDate[3]) % 4 == 0 ? day = 29 : day = 28;
                this.daysInMonths = [31, day, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31];
            },
            oncodeOfYearCount: function() {
                this.codeOfYear = ( 6 + Number(this.curDate[3].slice(2,5)) + Number(this.curDate[3].slice(2,5)) / 4 ) % 7;
            },
            firstDay: function() {
                let a = this.daysOfWeek[this.daysOfWeekCode.indexOf(( 1 + this.monthsCode[this.months.indexOf(this.curDate[1]) + this.counterMonth] + this.codeOfYear ) % 7)];
                return a;
            },
            renderCalendar: function() {

                this.calendar = [];
                for(var i = 1; i <= this.daysInMonths[this.months.indexOf(this.curDate[1]) + this.counterMonth]; i ++) {
                    this.calendar.push(i);
                }
                this.addBeforeDates();
                this.addAfterDates();
                this.setMonth();
                this.setYear();
                this.renderPickedDate();
            },
            addBeforeDates: function() {
                if (this.curDate[1] === "Dec") {
                    let a = 31;
                    for(let j = 0; j < this.daysOfWeek.indexOf(this.firstDay()); j++) {
                        this.calendar.unshift(a);
                        a--;
                    }
                } else {
                    let a = this.daysInMonths[this.months.indexOf(this.curDate[1]) - 1];
                    for(let q = 0; q < this.daysOfWeek.indexOf(this.firstDay()); q++) {
                        this.calendar.unshift(a);
                        a--;
                    }
                }
            },
            addAfterDates: function() {
                if (this.calendar.length / 7 <= 5) {
                    for (let p = 1; this.calendar.length < 35; p++) {
                        this.calendar.push(p)
                    }
                } else {
                    for (let p = 1; this.calendar.length < 42; p++) {
                        this.calendar.push(p);
                    }
                }
            },
            isActiveC: function() {
                this.isActive = !this.isActive;
                this.$children[2].isAutoDayPicked = true;
                this.$children[2].isChangeDate();
            },
            onAddCounter: function () {
                if(this.month != 'Декабрь') {
                    this.counterMonth++;
                    this.renderCalendar();
                }
            },
            onReduceCounter: function () {
                if(this.month != 'Январь') {
                    this.counterMonth--;
                    this.renderCalendar();
                }
            },
            setMonth: function() {
                this.month = this.monthsName[this.months.indexOf(this.curDate[1]) + this.counterMonth];
            },
            setYear: function() {
                this.year = this.curDate[3];
            },
            setTodaysDateColor: function() {
                let day = document.querySelectorAll('.day');
                if(this.months[this.monthsName.indexOf(this.month)] === this.curDate[1] && this.year === this.curDate[3]) {
                    day[this.searchDate(this.curDate[2])].classList.add('dayChoose');
                } else {
                    day.forEach(el => el.classList.remove('dayChoose'));
                }
            },
            chooseEl: function (id) {
                if(id >= this.calendar.indexOf(1) && id <= this.calendar.lastIndexOf(this.daysInMonths[this.monthsName.indexOf(this.month)])) {
                    let day = document.querySelectorAll('.day'),
                        str = `${day[id].innerText}-${this.monthsName.indexOf(this.month) + 1}-${this.year}`;
                    this.isActiveBtn = false;
                    if (this.filterDates.length === 0) {
                        this.filterDates.push(str);
                        this.renderPickedDate();
                        this.isActiveBtn = true;
                    } else if(this.filterDates.length === 1) {
                        this.filterDates.push(str);
                        this.renderPickedDate();
                        this.periodFilter();
                        this.isActiveBtn = true;
                    } else if (Number(this.filterDates[0].split('-')[0]) == day[id].innerText) {
                        this.clearCalendar();
                    }else {
                        this.filterDates.pop();
                        this.filterDates.push(str);
                        this.renderPickedDate();
                        this.periodFilter();
                        this.isActiveBtn = true;
                    }
                }

            },
            renderPickedDate: function() {
                let day = document.querySelectorAll('.day');

                day.forEach(el => el.classList.remove('pickedDay'));

                if(this.filterDates.length === 1) {
                    if(this.monthsName[Number(this.filterDates[0].split('-')[1]) - 1] === this.month) {
                        let num = Number(this.filterDates[0].split('-')[0]);
                        num < 10 ? day[this.calendar.indexOf(num)].classList.add('pickedDay') : day[this.calendar.lastIndexOf(num)].classList.add('pickedDay');
                    }
                } else if(this.filterDates.length === 2) {
                    if(this.monthsName[Number(this.filterDates[0].split('-')[1]) - 1] === this.month) {
                        let num = Number(this.filterDates[0].split('-')[0]);
                        num < 10 ? day[this.calendar.indexOf(num)].classList.add('pickedDay') : day[this.calendar.lastIndexOf(num)].classList.add('pickedDay');
                    }
                    if(this.monthsName[Number(this.filterDates[1].split('-')[1]) - 1] === this.month) {
                        let num1 = Number(this.filterDates[1].split('-')[0]);
                        num1 < 10 ? day[this.calendar.indexOf(num1)].classList.add('pickedDay') : day[this.calendar.lastIndexOf(num1)].classList.add('pickedDay');
                    }
                }
            },
            searchDate: function(id) {
                let num = Number(id);
                if(num < 10) {
                    return this.calendar.indexOf(num);
                } else {
                    return this.calendar.lastIndexOf(num);
                }
            },
            addActiveDates: function () {
                let first = this.calendar.indexOf(1),
                    day = [...document.querySelectorAll('.day')];

                day.forEach(el =>  el.classList.remove('activeDay'));
                day.splice(first, this.daysInMonths[this.monthsName.indexOf(this.month)]).forEach( el =>
                    el.classList.add('activeDay'));
            },
            periodFilter: function () {
                let allDays = document.querySelectorAll('.day'),
                    dayOuter = document.querySelectorAll('.dayOuter');

                dayOuter.forEach(el => el.classList.remove('toRight'));
                dayOuter.forEach(el => el.classList.remove('toLeft'));
                allDays.forEach(el => el.classList.remove('periodOfDates'));
                if(this.filterDates.length === 2) {
                    let first = this.filterDates[0],
                        firstNum = Number(first.split('-')[0]),
                        second = this.filterDates[1],
                        secondNum = Number(second.split('-')[0]),
                        dif = Math.abs(firstNum - secondNum) - 1,
                        arr = [...this.calendar].splice(this.calendar.indexOf(this.compareNum(firstNum, secondNum)), dif);

                    if(Number(first.split('-')[1]) === Number(second.split('-')[1]) && this.monthsName[first.split('-')[1] -1] === this.month) {
                        let day = document.querySelectorAll('.activeDay');
                        for(let i = 0; i < arr.length; i++) {
                            day[arr[i]].classList.add('periodOfDates');
                        }
                        firstNum > secondNum ? this.addColorPicked(second, first) : this.addColorPicked(first, second);
                    } else if(Number(first.split('-')[1]) < Number(second.split('-')[1])) {
                        this.DifMonthsPerion(first, second);
                        this.addColorPicked(first, second);
                    } else if(Number(first.split('-')[1]) > Number(second.split('-')[1])) {
                        this.DifMonthsPerion(second, first);
                        this.addColorPicked(second, first);
                    }
                }
            },
            DifMonthsPerion: function(first, second) {
                let firstNum = Number(first.split('-')[0]),
                    secondNum = Number(second.split('-')[0]);

                if(this.monthsName[first.split('-')[1] -1] === this.month) {
                    let day = document.getElementsByClassName('activeDay');
                    day.forEach(el => {
                        el.innerText > firstNum ? el.classList.add('periodOfDates') : el.classList.remove('periodOfDates');
                    });
                }
                if(this.monthsName[second.split('-')[1] -1] === this.month) {
                    let day = document.getElementsByClassName('activeDay');
                    day.forEach(el => {
                        el.innerText < secondNum ? el.classList.add('periodOfDates') : el.classList.remove('periodOfDates');
                    });
                }
                if (this.monthsName[first.split('-')[1] -1] !== this.month && this.monthsName[second.split('-')[1] -1] !== this.month) {
                    let day = document.getElementsByClassName('activeDay');

                    if(this.monthsName.indexOf(this.month) > first.split('-')[1] - 1 && this.monthsName.indexOf(this.month) < second.split('-')[1] -1) {
                        day.forEach( el => el.classList.add('periodOfDates'));
                    }
                }
            },
            addColorPicked: function(first, second) {
                let day = document.querySelectorAll('.dayOuter'),
                    newDay = [],
                    firstNum = Number(first.split('-')[0]),
                    secondNum = Number(second.split('-')[0]);
                day.forEach(el => {if(el.childNodes[0].classList.contains('activeDay')) {newDay.push(el)}} );

                if(this.monthsName[first.split('-')[1] -1] === this.month && this.monthsName[second.split('-')[1] -1] === this.month) {
                    firstNum < 10 ? newDay[firstNum - 1].classList.add('toLeft') : newDay[firstNum - 1].classList.add('toLeft');
                    secondNum < 10 ? newDay[secondNum - 1].classList.add('toRight') : newDay[secondNum - 1].classList.add('toRight');
                }
                if(this.monthsName[first.split('-')[1] -1] === this.month && this.monthsName[second.split('-')[1] -1] !== this.month) {
                    firstNum < 10 ? newDay[firstNum - 1].classList.add('toLeft') : newDay[firstNum - 1].classList.add('toLeft');
                }
                if(this.monthsName[first.split('-')[1] -1] !== this.month && this.monthsName[second.split('-')[1] -1] === this.month) {
                    secondNum < 10 ? newDay[secondNum - 1].classList.add('toRight') : newDay[secondNum - 1].classList.add('toRight');
                }

            },
            compareNum: function (first, second) {
                return first < second ? first : second;
            },
            clearCalendar: function() {
                let day = document.querySelectorAll('.day'),
                    dayOuter = document.querySelectorAll('.dayOuter');

                this.isActiveBtn = false;
                this.filterDates = [];
                day.forEach(el => el.classList.remove('pickedDay'));
                day.forEach(el => el.classList.remove('periodOfDates'));
                dayOuter.forEach(el => el.classList.remove('toRight'));
                dayOuter.forEach(el => el.classList.remove('toLeft'));
            },
            choosePeriod: function(key) {
                this.filterDates = [];
                let el = document.querySelectorAll('.period-picker .item');
                el.forEach(el => el.classList.remove('activeItem'));
                el[key].classList.add('activeItem');
                this.isActiveBtn = true;
                switch(key) {
                    case 0:
                        this.filterDates = ["1-1-2020", "31-12-2020"];
                        break;
                    case 1:
                        this.filterDates.push(`${this.curDate[2]}-${this.months.indexOf(this.curDate[1]) + 1}-${this.curDate[3]}`);
                        break;
                    case 2:
                        this.filterDates.push(`${Number(this.curDate[2]) - 1}-${this.months.indexOf(this.curDate[1]) + 1}-${this.curDate[3]}`);
                        break;
                    case 3:
                        this.filterDates.push(`${Number(this.curDate[2])}-${this.months.indexOf(this.curDate[1]) + 1}-${this.curDate[3]}`);
                        this.filterDates.push(`${Number(this.curDate[2]) - 6}-${this.months.indexOf(this.curDate[1]) + 1}-${this.curDate[3]}`);
                        break;
                    case 4:
                        this.filterDates.push(`${Number(this.curDate[2])}-${this.monthsName.indexOf(this.month) + 1}-${this.year}`);
                        this.filterDates.push(`${this.curDate[2]}-${this.monthsName.indexOf(this.month)}-${this.year}`);
                        break;
                    case 5:
                        this.filterDates.push(`1-${this.months.indexOf(this.curDate[1]) + 1}-${this.curDate[3]}`);
                        this.filterDates.push(`${this.curDate[2]}-${this.months.indexOf(this.curDate[1]) + 1}-${this.curDate[3]}`);
                        break;
                    case 6:
                        this.filterDates.push(`1-${this.months.indexOf(this.curDate[1])}-${this.curDate[3]}`);
                        this.filterDates.push(`${this.daysInMonths[this.months.indexOf(this.curDate[1]) - 2]}-${this.months.indexOf(this.curDate[1])}-${this.curDate[3]}`);
                        break;
                    default:
                        this.filterDates = [];
                        break;
                }
                this.renderCalendar();
            },
            AddToFilter: function() {
                this.isActiveC();
                if (this.filteredPeriods.length === 5) {
                    this.filteredPeriods.splice(4, 1);
                    this.filteredPeriods = [...this.filteredPeriods, [...this.filterDates]];
                } else {
                    this.filteredPeriods = [...this.filteredPeriods, [...this.filterDates]];
                }
                this.filterDates = [];
                let str = JSON.stringify([...this.filteredPeriods]);

                axios.post('http://localhost:8081/', str)
                    .then((response) => {
                        console.log(response);
                    })
                    .catch((error) => {
                        console.log(error);
                    });
            },
            ListRender: function() {
                let user = {
                    isActive: false,
                    name: 'User',
                    mail: 'user@user.com',
                    regDate: '1-9-2020',
                    lastActivity: '1-9-2020',
                    lastActions: 'view_landing_course',
                    product: 'Как наладить отношения'
                };
                for(let i = 1; i < 14; i++) {
                    user.name = 'User' + String(i);
                    this.list = [...this.list, {...user}]
                }
    },
        },
        mounted: function() {
            this.oncodeOfYearCount();
            this.firstDay();
            this.onDaysInMonthsCount();
            this.renderCalendar();
        },
        updated() {
            this.setTodaysDateColor();
            this.addActiveDates();
            this.periodFilter();
            this.renderPickedDate();
            this.ListRender();
        },
    }
</script>

<style scoped>
    .main {
        display: flex;
        height: max-content;
    }

    .font-white {
        background-color: white;
    }

    .main-section {
        display: flex;
        flex-direction: column;
        width: 100%;
        height: 100%;
    }

    .user-list {
        display: flex;
        flex-direction: column;
        width: 1269px;
        height: 2000px;
        border-radius: 10px;
        font-size: 15px;
    }

    .background-calendar {
        width: 100%;
        height: 100%;
        background-color: rgba(240, 246, 252, 0.8);
        position: fixed;
        left: 0;
        right: 0;
        margin-top: 0;
        transition: display .3s ease-in-out;
    }

    .calendar-box {
        display: flex;
        width: 472px;
        height: 332px;
        position: absolute;
        top: 171px;
        left: 343px;
        transition: all .3s ease-in-out;
    }

    .period-picker {
        display: flex;
        flex-direction: column;
        justify-content: space-between;
        align-items: center;
        width: 176px;
        height: 332px;
        margin-right: 8px;
        border-radius: 10px;
    }

    .item {
        display: flex;
        justify-content: center;
        align-items: center;
        width: 156px;
        height: 39px;
    }

    .item:hover {
        border: 1px solid black;
        border-radius: 5px;
    }

    .item:first-child {
        margin-top: 2px;
    }

    .item:last-child {
        margin-bottom: 2px;
    }

    .calendar {
        display: flex;
        flex-direction: column;
        width: 288px;
        height: 332px;
        border-radius: 8px;
    }

    .upperC {
        display: flex;
        flex-direction: row;
        width: 100%;
        height: 57px;
    }

    .leftV, .rightV {
        display: flex;
        width: 54px;
        height: 100%;
        justify-content: center;
        align-content: center;
        align-items: center;
    }

    .rightV img {
        transform: rotateZ(180deg);
    }

    .MY {
        display: flex;
        justify-content: center;
        align-content: center;
        align-items: center;
        width: 180px;
        height: 100%;
    }

    .mainC {
        display: grid;
        grid-template-columns: repeat(7, 1fr);
        grid-auto-rows: auto;
        width: 100%;
        height: 210px;
        padding: 4px 0 4px 0;
        background-color: #F3F8FD;
        font-weight: 700;
    }

    .dayW {
        display: flex;
        justify-content: center;
        align-items: center;
        cursor: default;
    }

    .dayOuter {
        display: flex;
        flex-direction: row;
        justify-content: center;
        align-items: center;
        font-weight: 400;
        color: rgba(0, 0, 0, 0.2);
        cursor: default;
    }

    .day {
        display: flex;
        justify-content: center;
        align-items: center;
        font-weight: 400;
        color: rgba(0, 0, 0, 0.2);
        cursor: default;
        width: 100%;
        height: 100%;
    }

    .activeDay:hover {
        background-color: #FF7439;
        border-radius: 80%;
        color: white;
    }

    .dayChoose {
        background-color: #DFE9F3;
        border-radius: 80%;
    }

    .activeDay {
        display: flex;
        justify-content: center;
        align-items: center;
        font-weight: 400;
        color: rgba(0, 0, 0, 1);
    }

    .pickedDay {
        background-color: #FF7439;
        border-radius: 80%;
        color: white;
    }

    .toRight {
        background: linear-gradient(to right, rgba(255, 116, 57, 0.2) 0% 49%, rgba(255, 116, 57, 0) 50% 100%);

    }

    .toLeft {
        background: linear-gradient(to left, rgba(255, 116, 57, 0.2) 0% 49%, rgba(255, 116, 57, 0) 50% 100%);

    }

    .periodOfDates {
        background-color: rgba(255, 116, 57, 0.2);
        border-radius: 0;
        border: none;
    }

    .downC {
        display: flex;
        justify-content: flex-start;
        align-items: center;
        width: 100%;
        height: 65px;
    }

    .cancel {
        width: 130px;
        height: 45px;
        border: 2px solid black;
        border-radius: 7px;
        background-color: white;
        margin-left: 11px;
        margin-right: 7px;
        transition: all .1s ease-in;
    }

    .cancel:hover {
        background-color: black;
        color: white;
    }

    .activeItem {
        background-color: #F0F3F8;
    }

    .renew {
        width: 130px;
        height: 45px;
        border: 2px solid rgba(255, 116, 57, 0);
        background-color: rgba(255, 116, 57, 0.5);
        border-radius: 7px;
        color: white;
    }

    .activeBtn {
        background-color: #FF7439;
        transition: all .1s ease-in;
    }

    .activeBtn:hover {
        background-color: #F16E36;
    }


</style>
