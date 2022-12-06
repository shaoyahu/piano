<template>
    <div class="Piano-wrapper">
        <div class="Piano-header">
            <h2>弹奏钢琴</h2>
            <div class="column volume-slider">
                <span>音量</span>
                <input type="range" min="0" max="1" value="0.5" step="any" @input="handleVolume">
            </div>
            <div class="column keys-checkbox">
                <span>显示按键</span>
                <input type="checkbox" checked @input="showKeys">
            </div>
        </div>
        <ul class="piano-keys">
            <li v-for="item, index in keyList" :key="index" :ref="refObj[refList[index]]" :class="['key', item.color]"
                :data-key="item.dataKey" @click="playTune(item.dataKey)"><span>{{ item.dataKey }}</span></li>
        </ul>
    </div>
</template>

<script setup lang='ts'>
import { ref } from 'vue'

// 所有键盘与按键相关的 颜色和对应按键和ref对象名称
const keyList = [
    {
        color: 'white',
        dataKey: 'a'
    },
    {
        color: 'black',
        dataKey: 'w'
    },
    {
        color: 'white',
        dataKey: 's'
    },
    {
        color: 'black',
        dataKey: 'e'
    },
    {
        color: 'white',
        dataKey: 'd'
    },
    {
        color: 'white',
        dataKey: 'f'
    },
    {
        color: 'black',
        dataKey: 't'
    },
    {
        color: 'white',
        dataKey: 'g'
    },
    {
        color: 'black',
        dataKey: 'y'
    },
    {
        color: 'white',
        dataKey: 'h'
    },
    {
        color: 'black',
        dataKey: 'u'
    },
    {
        color: 'white',
        dataKey: 'j'
    },
    {
        color: 'white',
        dataKey: 'k'
    },
    {
        color: 'black',
        dataKey: 'o'
    },
    {
        color: 'white',
        dataKey: 'l'
    },
    {
        color: 'black',
        dataKey: 'p'
    },
    {
        color: 'white',
        dataKey: ';'
    }
]
const refList = ['aRef', 'wRef', 'sRef', 'eRef', 'dRef', 'fRef', 'tRef', 'gRef', 'yRef', 'hRef', 'uRef', 'jRef', 'kRef', 'oRef', 'lRef', 'pRef', ';Ref']

// 将所有钢琴块变成 ref对象
const refObj: any = {}
for (let i = 0; i < refList.length; i++) {
    refObj[`${refList[i]}`] = ref(null)
}

// 使用键盘弹响钢琴
const pressedKey = (e: KeyboardEvent) => {
    if (refList.includes(`${e.key}Ref`)) playTune(e.key)
}
document.addEventListener('keydown', pressedKey)


// 播放音频(弹响钢琴)
let volume = 0.5
const playTune = (key: string) => {
    let audio = new Audio(`./audio/tunes/${key}.wav`)
    audio.volume = volume
    audio.play()
    setTimeout(() => {
        audio.remove()
    }, 1000);
    // 按键盘播放音频时产生点击相同显示效果
    refObj[`${key}Ref`].value[0].classList.add('active')
    setTimeout(() => {
        refObj[`${key}Ref`].value[0].classList.remove('active')
    }, 150);
}

// 修改音量
const handleVolume = (e: any) => {
    volume = e.target.value
}
// 显示按键
const showKeys = () => {
    for (const key in refObj) {
        refObj[key].value[0].classList.toggle('hidden')
    }
}
</script>

<style scoped>
* {
    box-sizing: border-box;
}

.Piano-wrapper {
    width: 800px;
    border-radius: 20px;
    background-color: black;
    padding: 35px 40px;
}

.Piano-wrapper .Piano-header {
    color: #b2b2b2;
    display: flex;
    align-items: center;
    justify-content: space-between;
    flex-wrap: wrap;
}

.Piano-header h2 {
    font-size: 1.6rem;
}

.Piano-header .column {
    display: flex;
    align-items: center;
}

.Piano-header .column span {
    font-weight: 500;
    font-size: 1.19rem;
    margin-right: 15px;
}

.volume-slider input {
    accent-color: #fff;
    outline: none;
}

.keys-checkbox input {
    appearance: none;
    width: 60px;
    height: 30px;
    outline: none;
    border-radius: 30px;
    position: relative;
    background-color: #4b4b4b;
}

.keys-checkbox input:before {
    content: '';
    height: 20px;
    width: 20px;
    top: 50%;
    left: 5px;
    border-radius: inherit;
    position: absolute;
    background-color: #8c8c8c;
    transform: translateY(-50%);
    transition: all .3s linear;
}

.keys-checkbox input:checked:before {
    left: 35px;
    background-color: #fff;
}

.piano-keys .key {
    color: #a2a2a2;
    list-style: none;
    text-transform: uppercase;
    cursor: pointer;
    position: relative;
}

.piano-keys {
    display: flex;
    margin-top: 40px;
    justify-content: center;
}

.piano-keys .white {
    width: 70px;
    height: 230px;
    border-radius: 8px;
    border: 1px solid #000;
    background: linear-gradient(#fff 96%, #eee 10%);
}

.piano-keys .white.active {
    box-shadow: inset -5px 5px 20px rgba(0, 0, 0, 0.2);
    background: linear-gradient(to bottom, #fff 0%, #eee 100%);
}

.piano-keys .black.active {
    box-shadow: inset -5px 5px 10px rgba(255, 255, 255, 0.1);
    background: linear-gradient(to bottom, #000 0%, #434343);
}

.piano-keys .black {
    width: 44px;
    height: 140px;
    border-radius: 0 0 5px 5px;
    margin: 0 -22px 0 -22px;
    z-index: 2;
    background: linear-gradient(#333, #000);
}

.piano-keys span {
    position: absolute;
    bottom: 20px;
    width: 100%;
    text-align: center;
}

.hidden span {
    display: none;
}

@media screen and (max-width:815px) {

    .piano-keys .key:nth-child(13),
    .piano-keys .key:nth-child(14),
    .piano-keys .key:nth-child(15),
    .piano-keys .key:nth-child(16),
    .piano-keys .key:nth-child(17) {
        display: none;
    }

    .piano-keys .white {
        width: 50px;
    }

    .Piano-header {
        flex-direction: column;
    }
}

@media screen and (max-width:450px) {

    .piano-keys .key:nth-child(9),
    .piano-keys .key:nth-child(10),
    .piano-keys .key:nth-child(11),
    .piano-keys .key:nth-child(12),
    .piano-keys .key:nth-child(13),
    .piano-keys .key:nth-child(14),
    .piano-keys .key:nth-child(15),
    .piano-keys .key:nth-child(16),
    .piano-keys .key:nth-child(17) {
        display: none;
    }

    .piano-keys .white {
        width: 50px;
    }

    .Piano-header {
        flex-direction: column;
    }
}

@media screen and (max-width:340px) {

    .piano-keys .key:nth-child(6),
    .piano-keys .key:nth-child(7),
    .piano-keys .key:nth-child(8),
    .piano-keys .key:nth-child(9),
    .piano-keys .key:nth-child(10),
    .piano-keys .key:nth-child(11),
    .piano-keys .key:nth-child(12),
    .piano-keys .key:nth-child(13),
    .piano-keys .key:nth-child(14),
    .piano-keys .key:nth-child(15),
    .piano-keys .key:nth-child(16),
    .piano-keys .key:nth-child(17) {
        display: none;
    }

    .piano-keys .white {
        width: 50px;
    }

    .Piano-header {
        flex-direction: column;
    }
}
</style>
