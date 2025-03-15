<template>
    <div class="nodeCard">
        <el-container>
            <el-header class="header">DetailPanel</el-header>
            <div class="demain" v-show="cardFlag">
                <table>
                    <tbody>
                        <tr v-for="(v, k) in nodeMsg">
                            <td>{{ k }}</td>
                            <td>{{ v }}</td>
                        </tr>
                    </tbody>
                </table>
            </div>
            <div class="btnController">
                <div class="mb-4">
                    <el-button v-show="networkFlag" @click="netStart" type="primary">NetWork</el-button>
                    <el-button v-show="topicFlag" type="success">Topic Info</el-button>
                </div>
            </div>
        </el-container>
    </div>

</template>
<script>
import { ref, watch } from 'vue'
export default {
    props: {
        msg: Object
    },
    emits: ["sendNet"],
    setup(props, { emit }) {
        const nodeMsg = ref(props.msg)
        const networkFlag = ref(false)
        const topicFlag = ref(false)
        const cardFlag = ref(false)

        watch(
            () => props.msg,
            (newValue) => {
                if (newValue !== null) {
                    cardFlag.value = true
                    nodeMsg.value = newValue;
                    console.log("show:", nodeMsg.value);
                    if (nodeMsg.value.id !== "") {
                        if (nodeMsg.value.group === 1) {
                            topicFlag.value = true
                            networkFlag.value = false
                        }
                        else if (nodeMsg.value.group === 2 || nodeMsg.value.group === 3) {
                            topicFlag.value = false
                            networkFlag.value = true
                        }
                    }
                    console.log(topicFlag.value);
                }
            },
            { immediate: true }
        );

        function netStart() {
            console.log(nodeMsg.value.cluster);
            emit("sendNet", nodeMsg.value.group, nodeMsg.value.cluster)
        }

        return {
            nodeMsg,
            networkFlag,
            topicFlag,
            netStart,
            cardFlag
        }
    }

}
</script>

<style scoped>
.nodeCard {
    height: 50vh;
    width: 100%;
    border: 2px solid #000;
    text-align: center;
    font-size: 18px;
}

.header {
    display: flex;
    align-items: center;
    justify-content: center;
    height: 10vh;
    background-color: rgba(128, 128, 128, 0.6);
}

.elmain {
    /* line-height: 30px; */
    height: 40vh;

}

.demain {
    width: 100%;
    height: 30vh;
    overflow-y: scroll;
    border-bottom: 3px solid #aaa;
}

td {
    font-size: 12px;
    /* max-width: 80%; */
    text-align: left;
    border-bottom: 1px solid #aaa;
    width: 150px;
    /* 设定固定宽度 */
    word-break: break-word;
    /* 防止超出时不换行 */
}

.btnController {
    height: 10vh;
    width: 100%;
    display: flex;
    align-items: center;
    justify-content: center;
}
</style>