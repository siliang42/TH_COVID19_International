<template>
    <div class="header minHeader">
        <!-- herder bg -->
        <img v-if="!pageConfig.home_logos.length" class="heaader-img" :src="currentBanner" />
        <div v-else class="heaader-img">
            <img :src="$t('url.header_bg_empty')" />
            <div class="cr-cctv">
                <img
                    class="cr-img2"
                    src="https://static.wecity.qq.com/wapdisease/logo_jian-c14dd63ab27365bf076d1b3ec57282e4.png"
                    alt=""
                />
                <div class="cr-ss"><div class="cr-splic"></div></div>
                <template v-if="pageConfig.home_logos.length">
                    <div class="cr-ss"><div class="cr-splic"></div></div>
                    <img class="cr-img" style="width: auto;" :src="pageConfig.home_logos[0]" alt="" />
                </template>
            </div>
        </div>
        <!-- 数据来源 -->
        <div v-if="chinaSummary" class="dashboard-tips" @click="popWin('dataFromDesc')">
            <div class="dashboard-tips-btn">
                <svg
                    width="22px"
                    height="22px"
                    viewBox="0 0 22 22"
                    version="1.1"
                    xmlns="http://www.w3.org/2000/svg"
                    xmlns:xlink="http://www.w3.org/1999/xlink"
                >
                    <g stroke="none" stroke-width="1" fill="none" fill-rule="evenodd" opacity="0.495954241">
                        <g transform="translate(-35.000000, -304.000000)" class="svg-path-tips" fill-rule="nonzero">
                            <g transform="translate(35.000000, 301.000000)">
                                <path
                                    d="M11.1382112,9.00466166 C10.2928327,8.95794371 9.46594627,9.26399172 8.85464663,9.84985679 C8.27104159,10.4532457 7.96299119,11.2711912 8.00355433,12.1097047 L9.40006493,12.1097047 C9.36814645,11.6298671 9.49218835,11.1525667 9.7536878,10.7490005 C10.0684165,10.357161 10.5604152,10.151762 11.0602943,10.20352 C11.4683901,10.1785771 11.8689542,10.3214973 12.1691117,10.5991432 C12.4324942,10.895175 12.5701013,11.2822392 12.5527026,11.6781157 C12.5459923,12.02848 12.4214138,12.3663585 12.1990798,12.6372024 L11.9773163,12.8889626 C11.3945551,13.3536799 10.8871016,13.9056744 10.4729207,14.5254043 C10.2931374,14.9178759 10.2069625,15.3467475 10.2211892,15.7782112 L10.2211892,16 L11.599719,16 L11.599719,15.7782112 C11.5933227,15.4714011 11.665539,15.1680581 11.8094952,14.8970503 C11.9452248,14.6411544 12.1282703,14.4133385 12.3489199,14.2256897 C12.7526038,13.9072691 13.136881,13.5649832 13.4996926,13.2006658 C13.8417485,12.7319617 14.0168957,12.1621407 13.9971621,11.5822071 C14.0319065,10.8681052 13.746205,10.1757494 13.2179931,9.69400521 C12.6365834,9.20991816 11.8936207,8.96366323 11.1382112,9.00466166 Z M10.9998056,17.007477 C10.7381768,16.994838 10.4837146,17.0967576 10.3004843,17.2875759 C10.0998131,17.4666842 9.98959579,17.7288805 10.0007752,18.0005551 C10.0174612,18.5527396 10.4579799,18.9934407 10.9998056,19 C11.2618875,19.0003602 11.5150534,18.9030028 11.7116148,18.7262659 C11.9013742,18.5350648 12.0052997,18.2724803 11.998836,18.0005551 C12.0123802,17.7292272 11.9071792,17.4657516 11.7116148,17.2812101 C11.5162199,17.08943 11.2519998,16.9881246 10.9810738,17.0011111 L10.9998056,17.007477 Z M11,4 C16.5228475,4 21,8.4771525 21,14 C21,19.5228475 16.5228475,24 11,24 C5.4771525,24 1,19.5228475 1,14 C1,8.4771525 5.4771525,4 11,4 M11,3 C4.92486775,3 0,7.92486775 0,14 C0,20.0751322 4.92486775,25 11,25 C17.0751322,25 22,20.0751322 22,14 C22,11.0826186 20.8410748,8.28472557 18.7781746,6.22182541 C16.7152744,4.15892524 13.9173814,3 11,3 Z"
                                ></path>
                            </g>
                        </g>
                    </g>
                </svg>
            </div>
            <p>Updated {{ timeStr }}</p>
        </div>
        <!-- 数据概览 & 最新动态 -->
        <div class="dashboard-wrap">
            <div
                class="dashboard"
                :class="{
                    'dashboard--global': curOverviewData === 'global',
                    'dashboard--local': curOverviewData !== 'global'
                }"
            >
                <div class="dashboard-inner">
                    <div class="dashboard-tab">
                        <!-- TAB BAR -->
                        <div class="dashboard-tab-btn tab-num-two">
                            <a
                                class="dashboard-tab-menu"
                                :class="curOverviewData === 'global' ? 'cur' : ''"
                                @click.stop.prevent="changeCurOverview('global')"
                            >
                                <span>{{ $t('common.Domestic') }}</span>
                            </a>
                            <a
                                class="dashboard-tab-menu"
                                @click.stop.prevent="changeCurOverview('abroad')"
                                :class="{
                                    cur: curOverviewData === 'abroad',
                                    'shadow-left': curOverviewData === 'otherArea',
                                    'shadow-right': curOverviewData === 'global'
                                }"
                            >
                                <span>{{ $t('common.Overseas') }}</span>
                            </a>
                        </div>

                        <!-- TAB CONTENT -->
                        <div class="dashboard-tab-cont dashboard-tab-cont-column">
                            <!-- 新增数据缺省文案 -->
                            <div
                                class="wait-update"
                                v-if="curOverviewData === 'global' && chinaSummary && chinaSummary.showAdd === 0"
                            >
                                {{ chinaSummary && chinaSummary.addNoShowText }}
                            </div>
                            <div
                                class="wait-update hubei"
                                v-if="curOverviewData === 'abroad' && foreignSummary && foreignSummary.showAdd === 0"
                            >
                                {{ foreignSummary && foreignSummary.addNoShowText }}
                            </div>
                            <!-- Domestic TAB  -->
                            <div
                                v-show="curOverviewData === 'global'"
                                class="dashboard-grid dashboard-column-list"
                                :class="{
                                    cur: curOverviewData === 'global',
                                    'increase-hide': chinaSummary && chinaSummary.showAdd === 0
                                }"
                            >
                                <div
                                    class="dashboard-column"
                                    :class="DiseaseMapClass[item]"
                                    v-for="(item, index) of DiseaseMapLetterOrder"
                                    :key="index"
                                >
                                    <div class="column-item">
                                        <p class="increase">
                                            {{
                                                chinaSummary.chinaDayModify[`${item}Desc`] ||
                                                chinaSummary.chinaDayModify[item]
                                            }}
                                            <svg
                                                v-if="item === 'suspect'"
                                                @click="popWin('suspectDesc')"
                                                width="9px"
                                                height="9px"
                                                viewBox="0 0 22 22"
                                                version="1.1"
                                                xmlns="http://www.w3.org/2000/svg"
                                                xmlns:xlink="http://www.w3.org/1999/xlink"
                                            >
                                                <g
                                                    stroke="none"
                                                    stroke-width="1"
                                                    fill="#DE5E5B"
                                                    fill-rule="evenodd"
                                                    opacity="0.495954241"
                                                >
                                                    <g
                                                        transform="translate(-35.000000, -304.000000)"
                                                        class="svg-path-tips2"
                                                        fill-rule="nonzero"
                                                    >
                                                        <g transform="translate(35.000000, 301.000000)">
                                                            <path
                                                                d="M11.1382112,9.00466166 C10.2928327,8.95794371 9.46594627,9.26399172 8.85464663,9.84985679 C8.27104159,10.4532457 7.96299119,11.2711912 8.00355433,12.1097047 L9.40006493,12.1097047 C9.36814645,11.6298671 9.49218835,11.1525667 9.7536878,10.7490005 C10.0684165,10.357161 10.5604152,10.151762 11.0602943,10.20352 C11.4683901,10.1785771 11.8689542,10.3214973 12.1691117,10.5991432 C12.4324942,10.895175 12.5701013,11.2822392 12.5527026,11.6781157 C12.5459923,12.02848 12.4214138,12.3663585 12.1990798,12.6372024 L11.9773163,12.8889626 C11.3945551,13.3536799 10.8871016,13.9056744 10.4729207,14.5254043 C10.2931374,14.9178759 10.2069625,15.3467475 10.2211892,15.7782112 L10.2211892,16 L11.599719,16 L11.599719,15.7782112 C11.5933227,15.4714011 11.665539,15.1680581 11.8094952,14.8970503 C11.9452248,14.6411544 12.1282703,14.4133385 12.3489199,14.2256897 C12.7526038,13.9072691 13.136881,13.5649832 13.4996926,13.2006658 C13.8417485,12.7319617 14.0168957,12.1621407 13.9971621,11.5822071 C14.0319065,10.8681052 13.746205,10.1757494 13.2179931,9.69400521 C12.6365834,9.20991816 11.8936207,8.96366323 11.1382112,9.00466166 Z M10.9998056,17.007477 C10.7381768,16.994838 10.4837146,17.0967576 10.3004843,17.2875759 C10.0998131,17.4666842 9.98959579,17.7288805 10.0007752,18.0005551 C10.0174612,18.5527396 10.4579799,18.9934407 10.9998056,19 C11.2618875,19.0003602 11.5150534,18.9030028 11.7116148,18.7262659 C11.9013742,18.5350648 12.0052997,18.2724803 11.998836,18.0005551 C12.0123802,17.7292272 11.9071792,17.4657516 11.7116148,17.2812101 C11.5162199,17.08943 11.2519998,16.9881246 10.9810738,17.0011111 L10.9998056,17.007477 Z M11,4 C16.5228475,4 21,8.4771525 21,14 C21,19.5228475 16.5228475,24 11,24 C5.4771525,24 1,19.5228475 1,14 C1,8.4771525 5.4771525,4 11,4 M11,3 C4.92486775,3 0,7.92486775 0,14 C0,20.0751322 4.92486775,25 11,25 C17.0751322,25 22,20.0751322 22,14 C22,11.0826186 20.8410748,8.28472557 18.7781746,6.22182541 C16.7152744,4.15892524 13.9173814,3 11,3 Z"
                                                            ></path>
                                                        </g>
                                                    </g>
                                                </g>
                                            </svg>
                                        </p>
                                        <p
                                            class="total"
                                            :class="
                                                isNaN(
                                                    chinaSummary.chinaTotal[`${item}Desc`] ||
                                                        chinaSummary.chinaTotal[item]
                                                )
                                                    ? 'txt'
                                                    : ''
                                            "
                                        >
                                            {{
                                                chinaSummary.chinaTotal[`${item}Desc`] || chinaSummary.chinaTotal[item]
                                            }}
                                        </p>
                                    </div>
                                    <p class="column-name">
                                        {{ DiseaseMapLetter[item] }}
                                        <svg
                                            v-if="item === 'nowConfirm'"
                                            @click="popWin('nowConfirmDesc')"
                                            width="9px"
                                            height="9px"
                                            viewBox="0 0 22 22"
                                            version="1.1"
                                            xmlns="http://www.w3.org/2000/svg"
                                            xmlns:xlink="http://www.w3.org/1999/xlink"
                                        >
                                            <g
                                                stroke="none"
                                                stroke-width="1"
                                                fill="#DE5E5B"
                                                fill-rule="evenodd"
                                                opacity="0.495954241"
                                            >
                                                <g
                                                    transform="translate(-35.000000, -304.000000)"
                                                    class="svg-path-tips2"
                                                    fill-rule="nonzero"
                                                >
                                                    <g transform="translate(35.000000, 301.000000)">
                                                        <path
                                                            d="M11.1382112,9.00466166 C10.2928327,8.95794371 9.46594627,9.26399172 8.85464663,9.84985679 C8.27104159,10.4532457 7.96299119,11.2711912 8.00355433,12.1097047 L9.40006493,12.1097047 C9.36814645,11.6298671 9.49218835,11.1525667 9.7536878,10.7490005 C10.0684165,10.357161 10.5604152,10.151762 11.0602943,10.20352 C11.4683901,10.1785771 11.8689542,10.3214973 12.1691117,10.5991432 C12.4324942,10.895175 12.5701013,11.2822392 12.5527026,11.6781157 C12.5459923,12.02848 12.4214138,12.3663585 12.1990798,12.6372024 L11.9773163,12.8889626 C11.3945551,13.3536799 10.8871016,13.9056744 10.4729207,14.5254043 C10.2931374,14.9178759 10.2069625,15.3467475 10.2211892,15.7782112 L10.2211892,16 L11.599719,16 L11.599719,15.7782112 C11.5933227,15.4714011 11.665539,15.1680581 11.8094952,14.8970503 C11.9452248,14.6411544 12.1282703,14.4133385 12.3489199,14.2256897 C12.7526038,13.9072691 13.136881,13.5649832 13.4996926,13.2006658 C13.8417485,12.7319617 14.0168957,12.1621407 13.9971621,11.5822071 C14.0319065,10.8681052 13.746205,10.1757494 13.2179931,9.69400521 C12.6365834,9.20991816 11.8936207,8.96366323 11.1382112,9.00466166 Z M10.9998056,17.007477 C10.7381768,16.994838 10.4837146,17.0967576 10.3004843,17.2875759 C10.0998131,17.4666842 9.98959579,17.7288805 10.0007752,18.0005551 C10.0174612,18.5527396 10.4579799,18.9934407 10.9998056,19 C11.2618875,19.0003602 11.5150534,18.9030028 11.7116148,18.7262659 C11.9013742,18.5350648 12.0052997,18.2724803 11.998836,18.0005551 C12.0123802,17.7292272 11.9071792,17.4657516 11.7116148,17.2812101 C11.5162199,17.08943 11.2519998,16.9881246 10.9810738,17.0011111 L10.9998056,17.007477 Z M11,4 C16.5228475,4 21,8.4771525 21,14 C21,19.5228475 16.5228475,24 11,24 C5.4771525,24 1,19.5228475 1,14 C1,8.4771525 5.4771525,4 11,4 M11,3 C4.92486775,3 0,7.92486775 0,14 C0,20.0751322 4.92486775,25 11,25 C17.0751322,25 22,20.0751322 22,14 C22,11.0826186 20.8410748,8.28472557 18.7781746,6.22182541 C16.7152744,4.15892524 13.9173814,3 11,3 Z"
                                                        ></path>
                                                    </g>
                                                </g>
                                            </g>
                                        </svg>
                                    </p>
                                </div>
                            </div>
                            <!-- Overseas TAB -->
                            <div
                                v-show="curOverviewData === 'abroad'"
                                class="dashboard-grid-center"
                                :class="{
                                    cur: curOverviewData === 'abroad',
                                    'increase-hide': foreignSummary && foreignSummary.showAdd === 0
                                }"
                            >
                                <div class="dashboard-item">
                                    <p class="increase pink">
                                        <span class="num">{{ foreignSummary.foreignDayModify.nowConfirmDesc }}</span>
                                    </p>
                                    <strong
                                        class="dashboard-item-val pink"
                                        :class="{ txt: isNaN(foreignSummary.foreignTotal.nowConfirmDesc) }"
                                        >{{ foreignSummary.foreignTotal.nowConfirmDesc }}</strong
                                    >
                                    <span class="dashboard-item-tit">{{ $t('common.Active_Cases') }}</span>
                                </div>
                                <div class="dashboard-item">
                                    <p class="increase red">
                                        <span class="num">{{ foreignSummary.foreignDayModify.confirmDesc }}</span>
                                    </p>
                                    <strong
                                        class="dashboard-item-val red"
                                        :class="{ txt: isNaN(foreignSummary.foreignTotal.confirmDesc) }"
                                        >{{ foreignSummary.foreignTotal.confirmDesc }}</strong
                                    >
                                    <span class="dashboard-item-tit">{{ $t('common.Total_Cases') }}</span>
                                </div>
                                <div class="dashboard-item">
                                    <p class="increase green">
                                        <span class="num">{{ foreignSummary.foreignDayModify.healDesc }}</span>
                                    </p>
                                    <strong
                                        class="dashboard-item-val green"
                                        :class="{ txt: isNaN(foreignSummary.foreignTotal.healDesc) }"
                                        >{{ foreignSummary.foreignTotal.healDesc }}</strong
                                    >
                                    <span class="dashboard-item-tit">{{ $t('common.Total_Recovered') }}</span>
                                </div>
                                <div class="dashboard-item">
                                    <p class="increase gray">
                                        <span class="num">{{ foreignSummary.foreignDayModify.deadDesc }}</span>
                                    </p>
                                    <strong
                                        class="dashboard-item-val gray"
                                        :class="{ txt: isNaN(foreignSummary.foreignTotal.deadDesc) }"
                                        >{{ foreignSummary.foreignTotal.deadDesc }}</strong
                                    >
                                    <span class="dashboard-item-tit">{{ $t('common.Total_Deaths') }}</span>
                                </div>
                            </div>
                        </div>
                    </div>
                    <!-- 最新动态 -->
                    <div class="dashboard-news">
                        <div class="index-news-wrap">
                            <div id="areaWrap" @click="infoScrollClick('chengshiyiqing')">
                                <info-scroll v-if="formattedNews.length" :list="formattedNews"></info-scroll>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import Vue from 'vue'
import httpUtils from '@/api'
import InfoScroll from '@/components/home/InfoScroll'
import { EventBus } from '@/utils/event-bus'

export default {
    name: 'HomeHeader',
    data () {
        return {
            showNewIcon: false, // 是否展示NEW图标
            newIconId: 0, // NEW图标的ID
            bannerImg: {
                shishitongbao: Vue.prototype.$i18nt('url.header_bg_logo'),
                chengshiyiqing: Vue.prototype.$i18nt('url.header_bg_logo'),
                haiwaiyiqing: Vue.prototype.$i18nt('url.header_bg_logo')
            },
            DiseaseMapLetterOrder: ['confirm', 'heal', 'dead', 'nowConfirm', 'suspect', 'nowHeavy'],
            DiseaseMapClass: {
                confirm: 'red',
                heal: 'green',
                dead: 'gray',
                nowConfirm: 'pink',
                suspect: 'yellow',
                nowHeavy: 'jacinth'
            },
            DiseaseMapLetter: {
                confirm: Vue.prototype.$i18nt('common.Total_Cases'),
                heal: Vue.prototype.$i18nt('common.Total_Recovered'),
                dead: Vue.prototype.$i18nt('common.Total_Deaths'),
                nowConfirm: Vue.prototype.$i18nt('common.Active_Cases'),
                suspect: Vue.prototype.$i18nt('common.Suspected_Cases'),
                nowHeavy: Vue.prototype.$i18nt('common.Severe_Cases'),
                import: Vue.prototype.$i18nt('common.Imported_Cases')
            },
            otherAreaLetterOrder: ['confirm', 'suspect', 'heal', 'dead'],
            areaInfo: {},
            cityInfo: {},
            curOverviewData: 'global',
            // 轮播最新动态
            localNews: [],
            noLocalNews: false
        }
    },
    props: {
        tab: {
            type: String,
            required: true
        },
        clickedTab: {
            type: String,
            required: true
        },
        chinaSummary: {
            type: Object,
            required: true
        },
        foreignSummary: {
            type: Object
        },
        // 全国最新动态
        fullNews: {
            type: Array,
            default () {
                return []
            }
        }
    },
    components: {
        InfoScroll
    },
    computed: {
        currentBanner () {
            return this.bannerImg[this.tab]
        },
        timeStr () {
            const { chinaSummary } = this
            if (chinaSummary && chinaSummary.chinaTotalUpdateTime) {
                return chinaSummary.chinaTotalUpdateTime.substring(0, chinaSummary.chinaTotalUpdateTime.length - 3)
            } else {
                return ''
            }
        },
        formattedNews () {
            const formatDate = (dateStr, showDate, showTime) => {
                if (!dateStr) {
                    return ''
                }
                let mon = dateStr.substr(5, 2)
                let date = dateStr.substr(8, 2)
                let hour = dateStr.substr(11, 2)
                let minute = dateStr.substr(14, 2)
                if (isNaN(mon) || isNaN(date) || isNaN(hour) || isNaN(minute)) {
                    return ''
                }
                if (mon < 10) {
                    mon = mon.substr(1)
                }
                if (date < 10) {
                    date = date.substr(1)
                }

                return (showDate ? `${mon}-${date} ` : '') + (showTime ? `${hour}:${minute}` : '')
            }
            const news = this.noLocalNews ? this.fullNews : this.localNews
            const { numbers, showDate, showTime } = this.globalConfig.feature.home.infoScrollBar
            const selectedNews = news.slice(0, numbers)
            console.log('selectedNews', selectedNews)
            return (
                selectedNews.map((item) => {
                    const { publicTime, title } = item
                    const timeStr = formatDate(publicTime, showDate, showTime)
                    return `${timeStr ? timeStr + ' ' : ''}${item.title}`
                }) || []
            )
        }
    },
    methods: {
        changeCurOverview (cur) {
            this.curOverviewData = cur
            this.$emit('curOverviewDataChange', cur)
            this.reportStatLog({
                actionType: 'event',
                actionName: 'changeCurOverview.click',
                extra: { tab: cur }
            })
        },
        toTongBao (url) {
            try {
                this.reportStatLog({
                    actionType: 'event',
                    actionName: 'toTongBao',
                    extra: { url: url, tab: this.tab }
                })
            } catch (error) {
                console.log(error)
            }
            if (url) {
                location.href = `${url}&webview=wxjk`
            }
        },
        infoScrollClick (id) {
            this.reportStatLog({
                actionType: 'event',
                actionName: 'index.homeHeader.infoScroll.click'
            })

            this.$emit('update:clickedTab', id)
        },
        toCityHome (areaInfo, cityInfo) {
            this.reportStatLog({
                actionType: 'event',
                actionName: 'yiqingbobao.overview.localArea.toCityHome',
                extra: { name: cityInfo.city }
            })
            this.closeRedDot(2)
            const { channel } = this.$route.query
            this.$router.push({
                path: '/city-detail',
                query: {
                    channel,
                    province: areaInfo.area,
                    cityCode: cityInfo.cityCode,
                    cityName: cityInfo.city
                }
            })
        },
        handleItemClickGlb (d, tag, key) {
            if (key !== 1) this.handleItemClick(d, tag)
        },
        handleItemClick (d, tag) {
            if (d.cityinfo === 0) return
            this.toMapDetail(d.area, tag)
        },
        toMapDetail (name, tag = false) {
            if (tag === 'LocalArea') {
                this.reportStatLog({
                    actionType: 'event',
                    actionName: 'yiqingbobao.overview.localArea.toMapDetail',
                    extra: { name }
                })
            }
            const { cityCode, channel } = this.$route.query
            this.$router.push({
                path: '/map-detail',
                query: {
                    province: name,
                    channel,
                    cityCode
                }
            })
        },
        // 获取本地城市的NEW标记信息_接口宝祥提供
        async getRedDot () {
            try {
                let resp = await httpUtils('getRedDot', { app: 'homepage' }, 'RedDotServer')
                // 调试开始
                // resp = {
                //   code: 0,
                //   dots: [
                //     {
                //       id: 87,
                //       modules: [
                //         'pneumonia-key-city-new'
                //       ]
                //     }
                //   ]
                // }
                // 调试结束
                if (resp.code === 0 && resp.dots && resp.dots.length > 0) {
                    resp.dots.forEach((item) => {
                        if (item.modules && item.modules.includes('pneumonia-key-city-new')) {
                            this.newIconId = item.id
                            this.showNewIcon = true
                            // NEW标记展示2次自动消除
                            const keyCityHis = localStorage.getItem('pneumonia-key-city-new')
                            const oldKeyCityList = keyCityHis ? JSON.parse(keyCityHis) : []
                            let newKeyCityList = []
                            const keyItemData = { id: item.id, count: 1 }
                            let existKey = false
                            oldKeyCityList.forEach((itm, idx) => {
                                if (itm.id === item.id) {
                                    existKey = true
                                    if (itm.count < 2) {
                                        itm.count++
                                        newKeyCityList.push(itm)
                                    } else {
                                        this.closeRedDot(1)
                                        this.showNewIcon = false
                                        this.newIconId = 0
                                    }
                                } else {
                                    newKeyCityList.push(itm)
                                }
                            })
                            if (!existKey) {
                                newKeyCityList.push(keyItemData)
                            }
                            localStorage.setItem('pneumonia-key-city-new', JSON.stringify(newKeyCityList))
                        }
                    })
                }
                console.log('this.newIconId = ', this.newIconId)
            } catch (e) {
                console.log('getRedDot error = ', e)
            }
        },
        // 去除本地城市的NEW标记
        async closeRedDot (type) {
            // type: 1-自然消除 2-用户点击消除
            try {
                if (this.newIconId) {
                    let resp = await httpUtils('closeRedDot', { reddotId: this.newIconId, src: type }, 'RedDotServer')
                    console.log('closeRedDot resp = ', resp)
                }
            } catch (e) {
                console.log('getRedDot error = ', e)
            }
        },
        async fetchLocalDetail (cityCode) {
            try {
                console.log('cityCode', cityCode)
                const data = await httpUtils('getCityInfoByCode', { cityCode }, 'THPneumoniaOuterDataService')
                if (data.code === 0) {
                    let { areaInfo, cityInfo } = data
                    this.areaInfo = areaInfo
                    this.cityInfo = cityInfo
                } else {
                    this.authSuccess = false
                    let toastErr = this.$loadingToast({ message: '位置获取失败' })
                    setTimeout(() => {
                        toastErr.close()
                    }, 2000)
                }
            } catch (e) {
                console.log('位置获取失败')
                this.authSuccess = false
                let toastErr = this.$loadingToast({ message: '位置获取失败' })
                setTimeout(() => {
                    toastErr.close()
                }, 2000)
            }
            // console.log('~~~~~~~~~~~~~~~getCityInfoByCode', data);
        },
        popWin (descKey) {
            EventBus.$emit('showPopWin', descKey)
        },
        async initNews (cityCode) {
            console.log('init news', cityCode)
            const useFullNews = () => {
                this.noLocalNews = true
                console.log('noLocalNews')
            }
            // 暂时不判断 cityCode
            // 产品说，省份更新不及时，暂且不用
            useFullNews()

            // 无cityCode参数，直接展示全国最新信息
            // if (!cityCode) {
            //   useFullNews()
            //   return
            // }

            // try {
            //   const context = this.$route.query && this.$route.query.channel
            //     ? { channel: this.$route.query.channel }
            //     : {}
            //   const data = await httpUtils(
            //     'getAreaContents',
            //     {
            //       tab: 'shishitongbao',
            //       reqType: 1,
            //       locationCode: cityCode
            //     },
            //     'THPneumoniaService',
            //     context
            //   )
            //   const {
            //     contents = []
            //   } = data
            //   if (contents && contents.length > 0) {
            //     this.localNews = contents
            //   } else {
            //     useFullNews()
            //   }
            // } catch (e) {
            //   useFullNews()
            //   console.error(e)
            // }
        }
    },
    async mounted () {
        let cityCode = this.$route.query.cityCode
        let { tab, currheadertab } = this.$route.query
        if (tab === 'haiwaiyiqing' || currheadertab === 'abroad') {
            this.curOverviewData = 'abroad'
            this.$emit('curOverviewDataChange', this.curOverviewData)
        }
        if (currheadertab === 'otherArea') {
            this.curOverviewData = 'otherArea'
            this.$emit('curOverviewDataChange', this.curOverviewData)
        }
        // console.log('this.$route.query.cityCode~~~~~~', this.$route.query)
        // console.log('~~~~~~~~~~~~~~~~~this.chinaSummary', this.chinaSummary)
        this.initNews(cityCode)
        if (cityCode) {
            await this.fetchLocalDetail(cityCode)
            await this.getRedDot()
        }
    }
}
</script>
<style lang="scss" scoped>
.dashboard-gcell-link {
    position: relative;
    z-index: 9;
    .dashboard-gcell-row {
        position: relative;
        &::after {
            content: '';
            display: block;
            width: 350px;
            height: 26px;
            position: absolute;
            left: 0;
            top: 0;
            // background: #fc0;
        }
    }
}
</style>
