<template>
    <div class="datepicker">
        <!-- 年份 月份 -->
        <div class="datepicker-header">
            <div class="picker-btn" @click="isVisiblePicker = true">
                {{currentYear}}年{{selectedMonth}}月
            </div>
            <span class="today" @click="today">返回今日</span>
            <van-popup v-model="isVisiblePicker" position="bottom" :overlay="true">
                <van-datetime-picker
                    v-model="pickerTime"
                    type="year-month"
                    @confirm="confirmPicker"
                    @cancel="isVisiblePicker=false"
                />
            </van-popup>
        </div>
        <!-- 星期 -->
        <div class="date-list">
            <ul class="weekdays">
                <li>日</li>
                <li>一</li>
                <li>二</li>
                <li>三</li>
                <li>四</li>
                <li>五</li>
                <li>六</li>
                </ul>
            <!-- 日期 -->
            <ul class="days">
                <template v-for="(day, index) in days">
                <li
                    v-if="day.getMonth()+1 == currentMonth"
                    :key="index"
                    :class="{'is-active':day.getDate()==selectedDay}"
                    @click="pick(day)"
                >{{day.getDate()}}</li>
                <li class="other-month"
                    v-else
                    :key="index"
                    :class="{'is-active':day.getDate()==selectedDay}"
                    @click="pick(day)"
                >{{day.getDate()}}</li>
                </template>
            </ul>
            <span class="pre-btn" @click="weekPre"> <van-icon name="arrow-left" size="1.7rem" /> </span>
            <span class="nex-btn" @click="weekNext"> <van-icon name="arrow" size="1.7rem" /> </span>
        </div>
    </div>
</template>

<script>
    import 'vant/lib/index.css';
    import { Icon, Popup, DatetimePicker } from 'vant';
    import Vue from 'vue';
    Vue.use(Icon)
    Vue.use(Popup)
    Vue.use(DatetimePicker)

    export default {
        name: "weekDatePicker",
        props: {
            time:{
                default: ''
            },
            format: {
                type: String    // 可选项：YYYY-MM-DD（默认）, YYYY/MM/DD, YYYY年MM月DD日, YYYYMMDD
            }
        },
        data() {
            return {
                currentYear: 1970,    // 年份
                currentMonth: 1,      // 月份
                currentDay: 1,        // 日期
                currentWeek: 1,       // 星期
                days: [],
                selectedDay: this.currentDay,
                // activeDay: 1,
                pickerTime: new Date(),
                isVisiblePicker: false
            };
        },
        mounted() {
            this.initData(this.initFormat(this.time));
        },
        computed: {
            selectedMonth(){
                return this.currentMonth<10 ? '0'+this.currentMonth : this.currentMonth
            }
        },
        watch: {
            time() {
                this.initData(this.initFormat(this.time))
            }
        },
        methods: {
            // 选择年/月确认
            confirmPicker(){
                this.pick(this.pickerTime)
                this.isVisiblePicker = false
            },

            // 返回今日
            today() {
                this.pick(new Date());
            },

            // 选择日期
            pick(date) {
                let d = new Date(date);
                // this.selectedDay = this.activeDay = d.getDate();
                this.selectedDay = d.getDate();
                this.$emit(
                    "change",
                    this.formatDate(d.getFullYear(), d.getMonth() + 1, d.getDate())
                );
            },

            // 上个星期
            weekPre() {
                console.log(this.selectedDay)
                const d = this.days[0];
                d.setDate(d.getDate() - 6);
                this.initData(d);
                // this.setActive();
            },

            // 下个星期
            weekNext() {
                console.log(this.selectedDay)
                const d = this.days[6];
                d.setDate(d.getDate() + 6);
                this.initData(d);
                // this.setActive();
            },

            // 设置选中
            setActive() {
                let isCurrent = this.currentMonth === new Date().getMonth() + 1;
                console.log(isCurrent)
                isCurrent ? this.selectedDay = "" : this.selectedDay = 1
            },

            // 初始化日期格式
            initFormat(time){
                const r1 = /^(\d{4})\/(\d{2})\/(\d{2})$/gi;         // YYYY/MM/DD
                const r2 = /^(\d{4})(\d{2})(\d{2})$/gi;             // YYYYMMDD
                if (!time) return new Date()
                if (typeof time === 'object' || r1.test(time)) {
                    return time
                } 
                else if (r2.test(time)){
                    return `${time.substr(0,4)}/${time.substr(4,2)}/${time.substr(6)}`  
                }
                else {            // 非YYYY/MM/DD格式的日期，就转换成YYYY/MM/DD
                    return `${time.substr(0,4)}/${time.substr(5,2)}/${time.substr(8)}`  
                }
            },

            // 初始化
            initData(cur) {
                let date = "";
                cur ? date = new Date(cur) : date = new Date();
                // this.selectedDay = this.activeDay = this.currentDay = date.getDate();    
                this.selectedDay = this.currentDay = date.getDate();   
                this.currentYear = date.getFullYear();          // 当前年份
                this.currentMonth = date.getMonth() + 1;        // 当前月份
                this.currentWeek = date.getDay();               // 1...6,0  // 星期几

                const str = this.formatDate(this.currentYear, this.currentMonth, this.currentDay, 'init');  
                this.days.length = 0;

                // 获取本周日期，周日排第一个
                for (let i = this.currentWeek; i >= 0; i -= 1) {
                    const d = new Date(str);        // 当日之前的日期
                    d.setDate(d.getDate() - i);
                    this.days.push(d);
                }
                for (let i = 1; i <= 6 - this.currentWeek; i += 1) {
                    const d = new Date(str);        // 当日之后的日期
                    d.setDate(d.getDate() + i);
                    this.days.push(d);
                }
            },

            // 格式化
            formatDate(year, month, day, init) {
                let y = year;
                let m = month;
                let d = day;
                if (m < 10) m = `0${m}`;
                if (d < 10) d = `0${d}`;
                if (init) {
                    return `${y}/${m}/${d}`;
                } else {
                    return this.setFormat(y, m, d)
                }
            },

            // 设置日期格式
            setFormat(y, m, d){
                const r1 = /^(Y{4})-(M{2})-(D{2})$/gi;              // YYYY-MM-DD（默认）
                const r2 = /^(Y{4})\/(M{2})\/(D{2})$/gi;            // YYYY/MM/DD
                const r3 = /^(Y{4})年(M{2})月(D{2})日$/gi;          // YYYY年MM月DD日
                const r4 = /^(Y{4})(M{2})(D{2})$/gi;                // YYYYMMDD
                if (!this.format || r1.test(this.format)) {
                    return `${y}-${m}-${d}`;          
                } 
                else if (r2.test(this.format)) {
                    return `${y}/${m}/${d}`;            
                }
                else if (r3.test(this.format)) {
                    return  `${y}年${m}月${d}日`;            
                }
                else if (r4.test(this.format)) {
                    return `${y}${m}${d}`;     
                }
                else {
                    alert('日期格式不支持！')
                    return
                }
            }
        }
    };
</script>

<style scoped>
    .datepicker {
        width: 100%;
        height: auto;
        color: #606266;
        text-align: center;
        font-size: inherit;
        display: flex;
        justify-content: center;
        align-items: center;
        flex-direction: column;
    }
    .datepicker-header {
        padding: 0 1.5rem;
        border-radius: 0.5rem;
        width: 100%;
        height: 100%;
        min-height: 3rem;
        color: #1989fa;
        display: flex;
        justify-content: center;
        align-items: center;
        position: relative;
    }
    .picker-btn{
        position: relative;
    }
    .picker-btn::after{
        content: '';
        position: absolute;
        right: -.8rem;
        top: 50%;
        transform: translateY(-10%);
        border-width: .35rem;
        border-style: solid;
        border-color: #1989fa transparent transparent transparent;
    }
    .today {
        position: absolute;
        right: 10%;
        top: 50%;
        -webkit-transform: translateY(-50%);
        -moz-transform: translateY(-50%);
        -ms-transform: translateY(-50%);
        -o-transform: translateY(-50%);
        transform: translateY(-50%);
        color: #fff;
        font-size: .7rem;
        background: #1989fa;
        line-height: .8rem;
        padding: 0.2rem 0.4rem;
        -webkit-border-radius: 0.2rem;
        -moz-border-radius: 0.2rem;
        border-radius: 0.2rem;

    }

    .date-list {
        width: 80%;
        position: relative;
    }
    .month {
        text-align: center;
    }
    .weekdays {
        display: flex;
        justify-content: space-around;
    }
    .weekdays li {
        text-align: center;
        margin: 0.3rem;
        color: #909399;
    }
    .days {
        display: flex;
        justify-content: space-around;
        margin-top: .3rem
    }
    .days li {
        text-align: center;
        padding: 0.3rem;
        min-width: 1.2rem;
    }
    .days li.is-active {
        background: #1989fa;
        border-radius: 50%;
        color: #fff;
    }
    .other-month {
        color: #e4393c;
    }

    .pre-btn,
    .nex-btn {
        position: absolute;
        top: 50%;
        -webkit-transform: translateY(-50%);
        -moz-transform: translateY(-50%);
        -ms-transform: translateY(-50%);
        -o-transform: translateY(-50%);
        transform: translateY(-50%);
        display: flex;
        padding: 0.3rem;
        border-radius: 30%;
        color: #1989fa;
    }
    .pre-btn {
        left: -11%;
    }
    .nex-btn {
        right: -11%;
    }
</style>
