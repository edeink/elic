<template>
    <div class="input-container">
        <div v-if="label" class="label">{{label}}</div>
        <input class="input"
               :class="[{'has-label': label}, {'float': isFloat && (isFocus || value)}]"
               :type="type"
               :placeholder="!isFloat && tips"
               v-model="value"
               @focus="focus"
                @blur="blur"/>
        <p v-if="isFloat"
           class="float-tips"
           :class="{'float': value || isFocus}">{{tips}}</p>
        <span class="clear-btn" v-if="value" @click="clearValue"></span>
    </div>
</template>
<script>
    export default {
        name: 'Input',
        data() {
            return {
                value: '',
                isFocus: ''
            }
        },
        props: {
            label: {
                type: String,
            },
            tips: {
                type: String
            },
            type: {
                type: String
            },
            isFloat: {
                type: Boolean
            }
        },
        methods: {
            clearValue() {
                this.value = '';
            },
            focus() {
                this.isFocus = true;
            },
            blur() {
                this.isFocus = false;
            }
        }
    }
</script>
<style lang="less" scoped>
    @lineHeight: 40px;
    .input-container {
        position: relative;
        width: 100%;
        height: @lineHeight;
        line-height: @lineHeight;
        border-bottom: 1PX solid #eaeaea;
        font-size: 12px;
        .label {
            display: inline-block;
            width: 100px;
            padding-left: 15px;
            color: #555;
        }
        .input {
            position: relative;
            display: inline-block;
            width: 100%;
            height: 32px;
            line-height: 32px;
            margin: 0;
            padding: 0 30px 0 15px;
            border: none;
            color: #555;
            background: transparent;
            outline-style: none;
            text-align: left;
            font-size: 12px;
            transition: top .2s ease-in-out, height .2s ease-in-out;
            &.has-label {
                width: calc(100% - 100px);
            }
            &.float {
                top: 8px;
                height: 24px;
                line-height: 24px;
            }
        }
        .float-tips {
            position: absolute;
            top: 0;
            left: 0;
            padding: 0;
            margin: 0;
            color: #999;
            transition: transform .2s ease-in-out;
            z-index: -1;
            &.float {
                transform: translate(-4px, -8px) scale(0.8);
            }
        }
        .clear-btn {
            position: absolute;
            top: 4px;
            right: 0px;
            width: 30px;
            height: 30px;
            background-image: url(data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAACQAAAAkCAYAAADhAJiYAAAAAXNSR0IArs4c6QAAA2FJREFUWAnNWLtu01AYru0oEQuCtVIZkXgFF+dC6ICggi59C2BA6kJHuiAxRH0LlkpcxJBGubV+hQq2BqkduSwoURLzfZV/c2L72E6ahPzS0Tnnv37+z93G2pTU7XZvD4fDx4ZhbMH0Dsq6X+jpwi89z/PquVzus+M4PyjISkZWxVartQPd5ygOguWy2AH0EHpdlMNyuXyUySZNqdPpbI7H47cAYafpJskBzjVNc69YLJ4m6umEAGAhK+8gf6nTmZFfQ7ZeAeAozj52yJrN5i0YvAcozpO5E3zX4Xu3Uqn8DDuPAPLBuDC4F1aeZx+gzhDDDoMy1SBQsPzMLBQMY/KD/ViWimECEOcMFBcyTGpQaTOWP0+FtRYMGVfTaDQ6CSRLbFiWdV9WX5AhLu0lYpgIpca+AsRND+lL22fO8SXPMO4PUNwJjzEdX6eCvecpxOcxKgGLsYmBDNlxuQMnEhy/QFo/UMl13Yf9fv8jmlWNUSOfz2/btv2HcqxcfnjaTk0MRybPJjQclERCWn+LAgMVCoVt9BvCU+oGZQKGfGTrlyLXNR1iMTGRnyBlkimdMp0eIDM3REEDKgLGt3kjdrqaGIiFqcy0zDnOHKYEULFgaENbHZAQf4uANkLMpG5VByo8TASeMs/i4mwYmN1f8QV346QJvEg2VN0ZwXBafGOGeMGaliKZEgezgvHt1wnIE2crUHsEdDkDEO2QaVZf1hCXBMR78DQUAcNhYhEn1wB1QUA9cZShjgXD1aRbffAZt3nqQvUI6FgnVfk8m1KWdmSiS6Zoq/rStbHa6yYOzE8w4OsgjfYZQJQ0qykWFAK9FjtdTQx8Npn+u4lPlUTC4XpTFDRgRBwBpdqKUkzdJRY5ww6hUIlRClg4Z2rtdtvjITsYDA4gSDoOqtA5xqa7TzC0DRzpG8Tw78YI49Mpzhy92xkknGN4Gm3SlJP6ivAle9Jedq3GDgD5d9osqZ033prcp+k4uOSzgyGzME++oM50JaHNdQhDVS+VSo9Qj8RPkCEyKACYXdRnorComjH8WAEYxpoARAZfklC0YVBnfxFE34wRfrUy1sSQqcFhsDo/G1RgK/M7RgXFNvapHVR8qvzfH1YEoxKfKnwdgMeVyPs4b5xy6+RVhuU7Sp3n5LS/9P4Cgt7XvQhDTLcAAAAASUVORK5CYII=);
            background-size: 12px 12px;
            background-repeat: no-repeat;
            background-position: 9px 9px;
            z-index: 3;
        }
    }

</style>